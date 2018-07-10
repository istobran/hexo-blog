---
title: Vue 数据驱动原理教程链接集合
date: 2018-07-10 12:34:05
urlname: vue-data-driving-tutorial-links
tags: [javascript, 前端, Vue]
categories: 技术研究
---

文档：
- [Vue 技术揭秘](https://github.com/ustbhuangyi/vue-analysis)
- [剖析Vue实现原理 - 如何实现双向绑定mvvm](https://github.com/DMQ/mvvm)

<!-- more -->

监听数据变化：
- [VUE2.0如何追踪数据变化？](https://www.jianshu.com/p/a86a9a377c85)
- [Vue源码学习之一：监听数据对象变化](https://www.jianshu.com/p/311bb4541336)

实现从 VDOM 到 DOM：
- [Vue 2.0 的 virtual-dom 实现简析](https://github.com/DDFE/DDFE-blog/issues/18)
- [Vue2.1.7源码学习](http://hcysun.me/2017/03/03/Vue%E6%BA%90%E7%A0%81%E5%AD%A6%E4%B9%A0/?nsukey=uNLdASBFj%2FX10MlVD2RJX77Wnt13nx%2FH6ZVE1t65sDrd02q9RSH6MjRyIoAYE9jBDbkUWtjTlTEDKaox7OTe%2FrjTXOZNHNQ%2Fhbuptv0Gh%2BTnPWuM6Gz23NfBcXVHH98iRRWmVzvD4S64LQ37%2BspyYUVNC9%2Bzt7UOB3145ltgz5k%3D#%E6%B3%A8%E9%87%8D%E5%A4%A7%E4%BD%93%E6%A1%86%E6%9E%B6%EF%BC%8C%E4%BB%8E%E5%AE%8F%E8%A7%82%E5%88%B0%E5%BE%AE%E8%A7%82)

    一、createElement(): 用 JavaScript对象(虚拟树) 描述 真实DOM对象(真实树)<br>
    二、diff(oldNode, newNode) : 对比新旧两个虚拟树的区别，收集差异<br>
    三、patch() : 将差异应用到真实DOM树

监听 DOM 表单元素变化：
- [Vue.js双向绑定的实现原理](https://www.cnblogs.com/kidney/p/6052935.html)
- [从vue.js的源码分析，input和textarea上的v-model指令到底做了什么](https://www.cnblogs.com/Eden-cola/p/vue-v-model-with-input.html)

其他：
- [vue2 实现 div contenteditable="true" 类似于 v-model 的效果](https://segmentfault.com/a/1190000008261449)
- [
如何理解v-model就是语法糖？](https://segmentfault.com/a/1190000007662815)
- [Vue 进阶教程之：详解 v-model](https://www.jianshu.com/p/4147d3ed2e60)
- [如何监听页面 DOM 变动并高效响应（事件委托）](https://hijiangtao.github.io/2017/08/03/How-to-Manipulate-DOM-Effectively/)