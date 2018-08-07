---
urlname: mobile-web-capabiltiy-faq
title: 移动端常见兼容性问题及解决方案
date: 2018-08-07 16:26:09
categories: 技术研究
tags: [前端, HTML5, CSS3]
---

文章转自：[常见移动端兼容性问题](https://zhuanlan.zhihu.com/p/28206065)

## 1. IOS移动端click事件300ms的延迟相应
移动设备上的web网页是有300ms延迟的，往往会造成按钮点击延迟甚至是点击失效。

这是由于区分单机事件和双击屏幕缩放的历史原因造成的。
<!--more-->

解决方式：

- fastclick可以解决在手机上点击事件的300ms延迟
- zepto的touch模块，tap事件也是为了解决在click的延迟问题
- 触摸屏的相应顺序为touchstart-->touchmove-->touchend-->click，也可以通过绑定ontouchstart事件，加快事件的响应，解决300ms的延迟问题

## 2. 一些情况下对非可点击元素（label，span）监听click事件，iOS 下不会触发

css增加cursor：pointer就搞定了。  


## 3. h5底部输入框被键盘遮挡问题

h5页面有个很蛋疼的问题就是，当输入框在最底部，点击软键盘后输入框会被遮挡。可采用如下方式解决

```javascript
var oHeight = $(document).height(); //浏览器当前的高度
$(window).resize(function(){
  if ($(document).height() < oHeight) {
    $("#footer").css("position","static");
  } else {
    $("#footer").css("position","absolute");
  }
});
```

## 4. 不让 Android 手机识别邮箱

```HTML
<meta content="email=no" name="format-detection" />   
```

## 5. 禁止 iOS 识别长串数字为电话

```HTML
<meta content="telephone=no" name="format-detection" />
```

## 6. 禁止 iOS 弹出各种操作窗口

```CSS
-webkit-touch-callout:none
```

## 7. 消除 transition 闪屏

```CSS
-webkit-transform-style: preserve-3d;     /*设置内嵌的元素在 3D 空间如何呈现：保留 3D*/
-webkit-backface-visibility: hidden;      /*(设置进行转换的元素的背面在面对用户时是否可见：隐藏)*/
```

## 8. iOS 系统中文输入法输入英文时，字母之间可能会出现一个六分之一空格

可以通过正则去掉

```javascript
this.value = this.value.replace(/\u2006/g, '');
```

## 9. 禁止ios和android用户选中文字

```CSS
-webkit-user-select:none
```

## 10. CSS动画页面闪白,动画卡顿

解决方法:
1. 尽可能地使用合成属性transform和opacity来设计CSS3动画，不使用 position 的 left 和 top 来定位
2. 开启硬件加速

```CSS
  -webkit-transform: translate3d(0, 0, 0);
     -moz-transform: translate3d(0, 0, 0);
      -ms-transform: translate3d(0, 0, 0);
          transform: translate3d(0, 0, 0);
```

## 11. fixed定位缺陷
- ios下fixed元素容易定位出错，软键盘弹出时，影响fixed元素定位
- android下fixed表现要比iOS更好，软键盘弹出时，不会影响fixed元素定位
- ios4下不支持position:fixed
- 解决方案：可用iScroll插件解决这个问题