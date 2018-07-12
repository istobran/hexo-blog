---
title: 事件 DOMContentLoaded 和 load 的区别
urlname: difference-between-domcontentloaded-and-load
date: 2018-07-10 17:17:54
categories: 技术研究
tags: [javascript, 前端, ECMAScript]
---

他们的区别是，触发的时机不一样，先触发DOMContentLoaded事件，后触发load事件。

DOM文档加载的步骤为

1. 解析HTML结构。
2. 加载外部脚本和样式表文件。
3. 解析并执行脚本代码。
4. DOM树构建完成。//DOMContentLoaded   //jQuery.ready
5. 加载图片等外部文件。
6. 页面加载完毕。//load

在第4步，会触发DOMContentLoaded事件。在第6步，触发load事件。

<!-- more -->

用原生js可以这么写

```javascript
// 不兼容老的浏览器，兼容写法见[jQuery中ready与load事件](http://www.imooc.com/code/3253)，或用jQuery
document.addEventListener("DOMContentLoaded", function() {
   // ...代码...
}, false);
```
```javascript
window.addEventListener("load", function() {
    // ...代码...
}, false);
```

用jQuery这么写
```javascript
// DOMContentLoaded
$(document).ready(function() {
    // ...代码...
});
```
```javascript
//load  
$(document).load(function() {
    // ...代码...
});
//貌似1.8版本之后,load就抛弃了.只能用window.onload()或者addEventListener这两个事件监听页面加载完成
```

参考资料：

- [事件DOMContentLoaded和load的区别](http://www.lrxin.com/archives-1146.html)
- [JS、CSS以及img对DOMContentLoaded事件的影响](http://www.alloyteam.com/2014/03/effect-js-css-and-img-event-of-domcontentloaded/)
- [DOMContentLoaded - 事件类型一览表 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/Events/DOMContentLoaded)
- [DOMContentLoaded与load的区别](https://www.cnblogs.com/caizhenbo/p/6679478.html)

