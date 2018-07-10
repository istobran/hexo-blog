---
title: display: none 和 visibility: hidden的区别
urlname: difference-between-display-none-and-visibility-hidden
date: 2018-07-10 14:37:56
categories: 技术研究
tags: [javascript, 前端, CSS3]
---

display:none 和 visibility: hidden 都是 CSS 中用于隐藏元素的属性，但他们在具体的场景上还是有一些区别：

## 1. 继承属性的差异
  - display: none 是非继承属性，一旦设置后，就算修改其子元素 display 属性的值也不会显示。
  - visibility: hidden 是继承属性，其子节点会继承 visibility 属性的值，若修改其子节点的 visibility 为 visibile，则子节点会重新显示

<!-- more -->

## 2. 是否占据渲染空间
  - display: none 仅会存在于 DOM 树中，并不存在于渲染树中，不会占据渲染空间
  - visibility: hidden 既存在于 DOM 树中，也存在于渲染树中，会占据渲染空间

## 3. 修改属性所导致的重绘
  - 修改 display 属性的值会导致文档重排 (reflow)，重新计算样式并生成渲染树
  - 修改 visibility 属性的值仅会产生局部重绘 (repaint)，不需要重新生成渲染树


参考资料：
- [What is DOM reflow?](https://stackoverflow.com/questions/27637184/what-is-dom-reflow/27637245#27637245)
- [display: none;与visibility: hidden;的区别](https://blog.csdn.net/crystal6918/article/details/77017487)
- [回流、重绘及其优化](https://juejin.im/post/5ad815df6fb9a045cb6d1fb4)
- [浏览器中的回流，重绘](https://juejin.im/post/5ae977da518825673446d92d)