---
title: CSS学习：居中
date: 2017-10-10 12:48:10
tags: CSS学习
categories: 学习笔记
---

常用居中方法：水平居中，垂直居中

<!-- more -->

居中在布局中很常见，我们假设DOM文档结构如下，子元素要在父元素中居中：

```
<div class = "parent">
  <div class = "child"></div>
</div>
```

> 以下 div 的显示效果皆加入了 padding: 1em; 和边框 border: 3px; 和文章所写样式有些许不同，只是为了做视觉显示样式所加。

# 水平居中

子元素为行内元素还是块状元素，宽度一定还是宽度未定，采取的布局方案不同。下面进行分析：

1. 行内元素：对父元素设置 text-align: center;
2. 定宽块状元素: 设置左右 margin 值为 auto;
3. 不定宽块状元素: 设置子元素为 display: inline,然后在父元素上设置 text-align: center;
4. 通用方案: flex 布局，对父元素设置 display: flex; justify-content: center;

## 1. 行内元素

```
.parent1 {
  text-align: center;
}
```

<div class = "parent1 relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class = "parent1"</span>
  <div class = "child relative" style = "border: solid #D64078 3px; padding: 1em;"><span class="divTitle " style = "background: #D64078;">class = "child"</span>父元素设置了 text-align：center 子元素<strong>行内元素</strong>内容居中</div>
</div>

## 2. 定宽块状元素

```
.child2 {
  width: 200px;
  margin: 0 auto;
}
```

<div class = "parent relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class = "parent"</span>
  <div class = "child2 relative" style = "border: solid #D64078 3px; padding: 1em;"><span class="divTitle " style = "background: #D64078;">class = "child2"</span><strong>定宽块状元素</strong>设置左右 margin 为 auto 实现居中</div>
</div>

## 3. 不定宽块状元素

```
.parent3 {
  text-align: center;
}
.child3 {
  display: inline;
}
```

<div class = "parent3 relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class = "parent3"</span>
  <div class = "child3 relative" style = "border: solid #D64078 3px; padding: 1.5em;"><span class="divTitle " style = "background: #D64078;">class = "child3"</span><strong>不定宽块状元素</strong></div>

</div>

## 4. 通用方案: flex 布局

```
.parent4 {
  display: flex;
  justify-content: center;
}
```

<div class = "parent4 relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class = "parent4"</span>
  <div class = "child relative" style = "border: solid #D64078 3px; padding: 1em;"><span class="divTitle " style = "background: #D64078;">class = "child"</span><strong>通用方案: flex 布局</strong></div>
</div>

# 垂直居中

垂直居中对于子元素是单行内联文本、多行内联文本以及块状元素采用的方案是不同的。

1. 父元素一定，子元素为单行内联文本：设置父元素的 height 等于行高 line-height
2. 父元素一定，子元素为多行内联文本：设置父元素的 display: table-cell，再设置 vertical-align: middle;
3. 块状元素:设置子元素 position: fixed 或 absolute、top、right、bottom、left 的属性然后设置 margin: auto 0;
4. 通用方案: flex 布局，给父元素设置 {display: flex; align-items: center;}

## 1. 父元素一定，子元素为单行内联文本

```
.parent5 {
  height: 234px;
  line-height: 200px;
}

```

> 为了显示出效果样式，parent 元素设置了 padding: 1em; 即 14px，外加上边框 3px 上下即为 6px，所以高度 height 外加上了 34px 以达到显示效果。

<div class = "parent5 relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class = "parent5"</span><div class = "child relative" style = "border: solid #D64078 3px; padding: 1em;"><span class="divTitle " style = "background: #D64078;">class = "child"</span><strong>单行内联文本</strong></div></div>

## 2. 父元素一定，子元素为多行内联文本

```
.parent6 {
  height: 500px;
  display: table-cell;
  vertical-align: middle;
}

```

<div class = "parent6 relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class = "parent6"</span><div class = "child relative" style = "border: solid #D64078 3px; padding: 1em;"><span class="divTitle " style = "background: #D64078;">class = "child"</span><strong>多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本多行内联文本</strong></div></div>

## 3. 块状元素

```
.parent7 {
  height: 500px;
}
.child7 {
  width: 200px;
  height: 200px;
  position: absolute;
  top:0;
  right:0;
  bottom:0;
  left:0;
  margin: auto 0;
}

```

<div class = "parent7 relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class = "parent7"</span><div class = "child7 relative" style = "border: solid #D64078 3px; padding: 1em;"><span class="divTitle " style = "background: #D64078;">class = "child7"</span><strong>块状元素</strong></div></div>

## 4. 通用方案: flex 布局

```
.parent8 {
  height: 500px;
  display: flex;
  align-items: center;
}
.child8 {
  width: 200px;
  height: 200px;
}
```

<div class = "parent8 relative" style = "border: solid #6AC5AC 3px; padding: 1em;"><span class="divTitle " style = "background: #6AC5AC;">class = "parent8"</span><div class = "child8 relative" style = "border: solid #D64078 3px; padding: 1em;"><span class="divTitle " style = "background: #D64078;">class = "child8"</span><strong>通用方案: flex 布局</strong></div></div>

<style>
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
.parent1 {
  text-align: center;
}
.child2 {
  width: 200px;
  margin: 0 auto;
}
.parent3 {
  text-align: center;
}
.child3 {
  display: inline;
}
.parent4 {
  display: flex;
  justify-content: center;
}
.parent5 {
  height: 234px;
  line-height: 200px;
}
.parent6 {
  height: 500px;
  display: table-cell;
  vertical-align: middle;
}
.parent7 {
  height: 500px;
}
.child7 {
  width: 200px;
  height: 200px;
  position: absolute;
  top:0;
  right:0;
  bottom:0;
  left:0;
  margin: auto 0;
}
.parent8 {
  height: 500px;
  display: flex;
  align-items: center;
}
.child8 {
  width: 200px;
  height: 200px;
}
</style>