---
title: 在网件 WNDR4300v1 上编译 OpenWRT 并实现多线多拨
tags: [操作系统, OpenWRT, Linux基础]
date: 2018-10-20 12:18:28
urlname: openwrt-compile-and-mwan3-config-in-netgear-wndr4300
categories:
  - [技术研究]
  - [日常运维]
---

家里一直屯着两根网线，一根是低价优惠时期买的长城，另一根是旧的贵价联通。由于一些历史原因，这联通的线路经常不稳定，会莫名其妙的挂掉，一直以来当联通线路挂掉的时候，就要手动拔掉联通的网线，换成长城的，然后又要进路由器改一番 PPPoE 的拨号配置，实在是烦，看到 OpenWRT 这么强大，突发奇想的要不干脆把路由器的一个 LAN 口拿出来，当成 WAN 来用，两个口都进行拨号，经过查了一番资料之后，感觉这个东西可行，就开始折腾了，不过中途一直没有找到自己想要的固件，于是打算自己编译一份，于是就这样掉进了 OpenWRT 的坑，折腾了两个星期之后终于成功的弄出来了。
<!--more-->

# 编译

写这篇文章的时候 OpenWRT 18.06 才刚出不久，感觉不太成熟，笔者还是选择了用 Lean 大的 LEDE 8.x 来进行编译。  

Lean 大的 LEDE 仓库地址：[https://github.com/coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)  

起初在自己电脑 macOS 上进行编译，怎么编译都编译不通过，首先是 macOS 的文件系统 hfs 有大小写不敏感的问题，接着又是编译过程中的一些包有 macOS 环境的兼容性问题。  

所以后来笔者为了避免编译环境差异造成这些不必要的麻烦，去找了个 Docker 的编译环境，才成功地编译了出来，在这里感谢 [@timiil](http://www.right.com.cn/forum/forum.php?mod=viewthread&tid=256137) 提供的 Docker 镜像。  

## 预备措施

由于笔者是选择了腾讯云的 CentOS 服务器来进行编译操作，所以还需要再服务器上先安装 Docker，至于 Docker 怎么安装，由于不同环境下方法不同，在此本文不做详述，仅提供一篇[教程](https://docs.docker.com/install/linux/docker-ce/centos/#install-from-a-package)作为参考。

## 启动 Docker 实例

安装好 Docker 之后，需要先拉取用于编译 LEDE 的 Docker 镜像
```bash
docker pull timiil/coolsnowwolf-lede-builder
```
然后创建并启动一个 Docker 实例，并挂载实例内的镜像输出目录 `/lede/bin` 到本地的 `~/lede_output` 目录下
```bash
docker run -it -v ~/lede_output:/lede/bin timiil/coolsnowwolf-lede-builder
```

## 更新 git 库

由于 Lean 大的 github 仓库是经常更新的，而 timiil 的 docker 镜像没有随之更新，所以进入到 docker 实例内后会发现里面的代码不是最新的代码，这会导致编译时的一些错误，所以这里需要手动更新一下代码，确保当前的代码跟 Lean 大仓库内的代码是同步的。
```bash
git pull
```

## 安装编译工具

为确保编译工作能够正常进行，也需要把编译所需的工具安装一下
```bash
sudo apt-get install build-essential subversion libncurses5-dev zlib1g-dev gawk gcc-multilib flex git-core gettext libssl-dev unzip
```

## 初始化 LEDE 的编译目录

1、创建一份 feeds.conf
```bash
cp feeds.conf.default feeds.conf
```
2、更新本地的包索引列表
```bash
./scripts/feeds update -a
```
3、将本地的包更新到最新版本
```bash
./scripts/feeds install -a
```
PS：如果中途 freeswitch 或者其他包报错，可以删掉重装一下
```bash
./scripts/feeds uninstall freeswitch
./scripts/feeds install -a
```

## 配置 LEDE 编译目标

打开配置菜单
```bash
make menuconfig
```
笔者仅需要用于 WNDR4300，故在此已 WNDR4300 为例进行配置  

    Target System => Atheros AR7xxx/AR9xxx
    Subtarget => Generic devices with NAND flash
    Target Profile => NETGEAR WNDR4300v1
    Target Images => Root filesystem images => squashfs
    LuCI => Applications => 把自己需要的东西勾上
    LuCI => Themes => 可以勾个 material
    LuCI => Modules => Translations => 勾上 Chinese

配好之后保存一下就行了  

PS：Target Profile 找不到的话可以在 OpenWRT wiki 上把路由器型号输进去，然后 wiki 会有这个提示。  

附：[WNDR4300 说明](https://wiki.openwrt.org/toh/netgear/wndr4300)  

## 修改 WNDR4300 的 mtd 分布，充分利用路由器的 flash

由于 OpenWRT 官方给的 WNDR4300 的 mtdlayout 只划分了 32M，WNDR4300 的 flash 实际上有 128M，所以这里最好修改一下编译时的 mtdlayout，这样可以充分利用我们的 flash 空间

```bash
cd ./target/linux/ar71xx/image/
cp legacy.mk legacy.mk.bak
vi legacy.mk
```

找到这一段  

> wndr4300_mtdlayout=mtdparts=ar934x-nfc:256k(u-boot)ro,256k(u-boot-env)ro,256k(caldata),512k(pot),2048k(language),512k(config),3072k(traffic_meter),2048k(kernel),**23552k**(ubi),**25600k**@0x6c0000(firmware),256k(caldata_backup),-(reserved)  

改为（将 ubi 和 firmware 增加96M，完全使用 128M flash）  

> wndr4300_mtdlayout=mtdparts=ar934x-nfc:256k(u-boot)ro,256k(u-boot-env)ro,256k(caldata)ro,512k(pot),2048k(language),512k(config),3072k(traffic_meter),2048k(kernel),**121856k**(ubi),**123904k**@0x6c0000(firmware),256k(caldata_backup),-(reserved)  

改完后记得保存退出

## 执行编译

配置好之后可以直接执行编译
```bash
nohup make V=99 &   # 编译的日志会输出到 nohup.out
```
也可以在此之前先 `make download` 一下，把需要的 dl 包先下载下来（有时可能会需要能访问外网的网络环境），接着再 `make V=99`，听说这样会快一些。  

这样就可以慢慢地等它编译完，自己先干别的事情去了，第一次编译要几个小时是正常的。  

PS：docker 可以用 `ctrl+p` 然后 `ctrl+q` 操作，直接从容器里面退出来而不中断容器。然后再用 `docker attach $id` 重新进入实例之后，按下 enter 键，如果 make 编译执行完了会有 `Done` 的提示。  

# 刷入固件

编译完之后，在本机挂载的 `~/lede_output/targets/ar71xx/nand` 目录下，看到编译完成的 `openwrt-ar71xx-nand-wndr4300-ubi-factory.img` 文件，我们只要这个文件就行了，把它拿出来。

## tftp 刷入固件

由于刷入 img 固件需要 tftp 工具，需要先找个有 tftp 的环境  

### Windows 环境

1、Windows 下的话 tftp 客户端默认是没有安装的，需要先在 `控制面板 > 程序 > 程序和功能 > 打开或关闭 Windows 功能` 里面勾选 `TFTP 客户端`，然后按确定。这样在命令行里面才能调用它。  

2、用牙签或者粗针或者笔头按住路由器背面红色的 reset 键，重启路由器，直到路由器进入绿灯闪烁状态之后再松开 reset 键  

3、在 Windows 下手工把网卡的 IP 配成 192.168.1.11，然后在 cmd 下去 `ping 192.168.1.1`，能够 ping 通才能进行下一步操作

4、然后在 Windows 下的 cmd 命令行里面输入  
```cmd
tftp -i 192.168.1.1 PUT c:\path\to\openwrt-ar71xx-nand-wndr4300-ubi-factory.img
```
把上面的路径改成 img 文件所在的路径即可  

5、上传完后路由器会自动安装固件并重启，这时把前面手工配的 IP 地址改回 DHCP 分配。当路由器第一次重启完后，若能 ping 通 192.168.1.1 的话，先别急着进入路由器，要先拔掉路由器电源线，再插回去重启一遍，等 5G 的灯亮了再进行后面的操作，不然 5G wifi 可能会打不开。

### macOS 环境

操作基本跟 Windows 环境相同，只是刷 tftp 的时候，所用的命令会有所不同  
```bash
tftp
tftp> connect 192.168.1.1
tftp> mode binary
tftp> put /path/to/openwrt-ar71xx-nand-wndr4300-ubi-factory.img
tftp> quit
```
同样的，把上面的路径改成 img 的实际路径即可

安装完成后，登录 LuCI 的面板 [http://192.168.1.1](http://192.168.1.1)，初始用户名为 `root`，初始密码为 `password`

# 配置多线多拨

安装好 LEDE 之后，别的 wifi 什么的配置就不多说了，这些都没有什么问题，这里要解决的是多线多拨的问题，一开始我在这里卡了很久，折腾了半天终于给弄出来了。

## 给交换机划分 vlan

在 `网络 => 交换机` 中添加一行新的 vlan  
![](/images/2018/10/switch_config.png)  

把 CPU 列设成 “已标记”，把需要转 WAN 的 LAN 口改成 “未标记”。  
然后 VLAN 1 上对应的端口也要设为 “关”  

PS：这里端口状态“未标记”(untagged)，即该端口作为本VLAN成员，进行二层交换；若选择“已标记”(tagged)，端口之间通信无二层交换，而是冲突广播（hub方式）  

## 添加接口配置

在 `网络 => 接口` 中添加新的接口配置  
![](/images/2018/10/add_interface.png)

然后建好接口后把 PPPoE 的用户名和密码自己填上  
接着给这个接口新增一个防火墙域叫 'wanb' 的  
![](/images/2018/10/add_firewall_area.png)  

接着要给不同的接口配置不同的跃点数，避免路由失效  
![](/images/2018/10/wan_metric.png)  
![](/images/2018/10/wanb_metric.png)  


## 配置防火墙域

在 `网络 => 防火墙` 中把对应的规则也给添加进去  
![](/images/2018/10/firewall_config.png)  

## 配置 MWAN 负载均衡

接着在 `网络 => 负载均衡` 给配置一下 MWAN3 的相关配置
由于我给接口起的名字是 wanb，MWAN3 默认就有这个配置，所以相关的配置工作量也就减少了  
启用接口：
![](/images/2018/10/active_mwan_interface.png)  

最后，根据自己的需要，在规则这边选择规则，我这里我这里选择了 wan 优先的策略  
![](/images/2018/10/mwan_policy.png)  

## 修改 MAC 地址

由于新增的 eth0.3 没有配置 MAC 地址，所以新增接口会沿用 eth0 的 MAC 地址，导致 MAC 地址冲突，所以这里还要加一个步骤，就是给 eth0.3 配置 MAC 地址  

ssh 进入路由器
```bash
ssh root@192.168.1.1
```
打开网络配置文件
```bash
vi /etc/config/network
```
在 wanb 的配置前面添加以下配置
```
config device 'wanb_dev'
        option name 'eth0.3'
        option macaddr 'c0:ff:d4:b3:64:f7'
```
保存退出，然后重启 eth0.3 接口
```bash
ifdown wanb
ifup wanb
```

配完之后最好重启一下路由器，再看看 `状态 => 概览`，如果有两个 IPv4 的上游就说明配置成功了，这样子就解决了 MAC 地址冲突带来的问题  
![](/images/2018/10/mwan_statistic.png)  

整个多线多拨到这里就算实现了  