---
title: CSS 中的继承属性与非继承属性
urlname: css-inherited-properties-and-non-inherited-properties
date: 2018-07-13 18:16:03
categories: 技术研究
tags: [CSS3, HTML5, 前端]
---

当我们参考CSS规范时，就会发现每个属性中都指出了 Inherited的值，即是否可继承。这决定了当你还没有为元素的属性指定值时该如何计算值。今天我们就大概的说说CSS中的继承属性与非继承属性。

# 1. 继承属性(inherited property)
当元素的一个继承属性没有指定值时，则取父元素的同属性的计算值。只有文档根元素取该属性的概述中给定的初始值。下面我们举一个简单的例子:
<!-- more -->

**典型例子: `color` 属性**

```css
p { color: orange; }
```
HTML:
```HTML
<p> I am a <em>smile</em> girl. </p>
```
这里你就会发现 “smile” 文本将呈现橙色，原因是 `em` 元素继承了 `p` 元素的 `color` 属性的值。

**常见的继承属性**  
那么，有哪些我们常见的继承属性呢？这里我给大家例举一下:

- border-collapse
- border-spacing
- caption-side
- color
- cursor
- direction
- font (其中包括 `font-family` , `font-size` , `font-weight` , `font-style`)
- letter-spacing
- line-height
- list-style (其中包括 `list-style-image` , `list-style-position` , `list-style-type`)
- text-align
- text-indent
- text-transform
- visibility
- white-space
- word-spacing

大家有没有发现一些字体呀，文本呀，颜色等的设置都是可继承属性 ~

[参考地址](https://www.w3.org/TR/CSS21/propidx.html)

# 2. 非继承属性(reset property)
当元素的一个非继承属性没有指定值时，则取属性的初始值。

典型例子: border属性
```css
p { border: medium solid }
```

HTML:
```HTML
<p> I am a <em>smile</em> girl. </p>
```
这时你就会发现文本 “smile” 没有边框，原因是 `border` 属性为不可继承属性，其初始值为 `none`。

**常见的非继承属性**  
这里例举几个常见的非继承属性:

- z-index
- width (其中包括 min-width , max-width)
- dispaly
- float
- clear
- vertical-align
- unicode-bidi
- position
- top
- bottom
- left
- right
- text-decoration
- background (其中包括 `background-color` , `background-image` , `background-position` , `background-attachment` , `background-repeat`)
- border (其中包括 `border-color` , `border-style` , `border-width` , `border-spacing` and so on)
- padding (其中包括 `padding-left` , `padding-right` , `padding-top` , `padding-bottom`)
- margin (其中包括 `margin-left` , `margin-right` , `margin-top` , `margin-bottom`)
- outline (其中包括 `outline-color` , `outline-style` , `outline-width`)
- clip
- content

非继承属性大部分都是一些和定位呀，浮动呀，盒子模型呀等有关 ~

[参考地址](https://www.w3.org/TR/CSS21/propidx.html)