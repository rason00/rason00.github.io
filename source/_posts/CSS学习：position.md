---
title: CSS学习：position
date: 2017-10-08 16:10:26
tags: CSS学习
categories: 学习笔记
---


# 定义和用法

position 属性规定元素的定位类型。

# 说明

这个属性定义建立元素布局所用的定位机制。任何元素都可以定位，不过绝对或固定元素会生成一个块级框，而不论该元素本身是什么类型。相对定位元素会相对于它在正常流中的默认位置偏移。

<!-- more -->

默认值：	static
继承性：	no
版本：	CSS2
JavaScript 语法：	object.style.position="absolute"

# 可用的值

| 值	| 描述 |
| --  | --- |
| absolute	| 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| fixed	| 生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| relative	| 生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。  |
| static	| 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
| inherit	| 规定应该从父元素继承 position 属性的值。 |

> **以上内容原文出处：http://www.w3school.com.cn/cssref/pr_class_position.asp**

# static

```
.static {
  position: static;
}
```

<div class = "relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class="static</span>static 是默认值。任意 position: static; 的元素不会被特殊的定位。一个 static 元素表示它不会被“positioned”，一个 position 属性被设置为其他值的元素表示它会被“positioned”。
</div>

# relative

```
.relative1 {
  position: relative;
}
.relative2 {
  position: relative;
  top: -20px;
  left: 20px;
  background-color: white;
  width: 500px;
}
```

<div class = "relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class="relative1"</span>relative 表现的和 static 一样，除非你添加了一些额外的属性。
</div>

<div class = "relative" style = "width:500px; top: -20px; left: 20px; background-color: white; border: solid #D64078 3px; padding: 1em;"><span class="divTitle " style = "background: #D64078;">class="relative2"</span>在一个相对定位（position属性的值为relative）的元素上设置 top 、 right 、 bottom 和 left 属性会使其偏离其正常位置。其他的元素的位置则不会受该元素的影响发生位置改变来弥补它偏离后剩下的空隙。
</div>

# fixed

一个固定定位（position属性的值为fixed）元素会相对于视窗来定位，这意味着即便页面滚动，它还是会停留在相同的位置。和 relative 一样， top 、 right 、 bottom 和 left 属性都可用。

我相信你已经注意到页面右下角的固定定位元素。你现在可以仔细看看它，这里有它所使用的CSS：

```
.fixed {
  position: fixed;
  bottom: 20px;
  right: 100px;
  width: 200px;
  background-color: white;
}
```

一个固定定位元素不会保留它原本在页面应有的空隙（脱离文档流）。

<div class="fixed" style = "padding: 1em; border: solid #FDC72F 3px;"><span class="divTitle " style = "background: #FDC72F;">class="fixed"</span>Hello！暂时不用关注我哦。
</div>

> **注意：当父级元素有 transform 属性，子级的 fixed 的特性将转换成 absolute**

# absolute

absolute 是最棘手的 position 值。 absolute 与 fixed 的表现类似，但是它不是相对于视窗而是相对于最近的“positioned”祖先元素。如果绝对定位（position 属性的值为 absolute）的元素没有“positioned”祖先元素，那么它是相对于文档的 body 元素，并且它会随着页面滚动而移动。记住一个“positioned”元素是指 position 值不是 static 的元素。

这里有一个简单的例子：

```
.relative {
  position: relative;
  width: 600px;
  height: 400px;
}
.absolute {
  position: absolute;
  top: 120px;
  right: 0;
  width: 300px;
  height: 200px;
}
```

<div class="relative1" style = "padding: 1em; border: solid #6AC5AC 3px;"><span class="divTitle " style = "background: #6AC5AC;">class="relative"</span>这个元素是相对定位的。如果它是 position: static; ，那么它的绝对定位子元素会跳过它直接相对于body元素定位。<div class="absolute" style = "padding: 1em; border: solid #D64078 3px;"><span class="divTitle " style = "background: #D64078;">class="absolute"</span>这个元素是绝对定位的。它相对于它的父元素定位。</div>
</div>

<br>

> **以上内容只是本人稍加整理，原文出处：http://zh.learnlayout.com/position.html**

<style>
/* 父元素上有 transform 影响 fixed 强制覆盖 */ 
.post-type-normal {
      transform: none!important;
}

.divTitle {
    position: absolute;
    color: #414142;
    line-height: 1em;
    top: 0;
    left: 0;
    padding: 0 3px 3px 0;
}

.relative {
  position: relative;
}
.relative1 {
  position: relative;
  width: 600px;
  height: 400px;
}
.fixed {
    position: fixed;
    bottom: 20px;
    right: 100px;
    width: 200px;
    background-color: white;
    z-index: 9999;
}
.absolute {
  position: absolute;
  top: 120px;
  right: 0;
  width: 300px;
  height: 200px;
}
</style>