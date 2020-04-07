---
title: 仿知乎日报web首页
date: 2017-07-28 22:41:00
tags: 练习
categories: 练习
---
仿知乎日报web首页(17/08/17新版本)
===
<!-- more -->
---

# 缘由：

起因是想复习下ajax和json的一些数据处理，于是网上找到了知乎日报的api（解决跨域了，非常方便），然后做了相应的仿造页面（旧版本），之后是学习了些新技术，观看了Sass和Bootstrap4的官方文档，想直接实践下，就自己给自己要求，套用了HTML5、Sass、Bootstrap4三项技术，做了新版本的页面，同时也解决了一些旧版本的问题，让新版本更加还原知乎日报web页面。

# Demo：

> 新版本：  
> 使用技术：HTML5、Sass、Bootstrap4  
> https://rason00.github.io/rason/zhihuribaoweb/index.html  
> 旧版本：  
> https://rason00.github.io/rason/zhihuribaoweb/index(old).html

# 代码：

> https://github.com/rason00/rason/tree/gh-pages/zhihuribaoweb


# 未实现：
    
    1，头部head和banner部分的按钮未作相应功能和链接处理。（新版本已实现）
    
    2，详细内容页面未做，只是把链接连到了知乎日报web相应的详细内容页面上。
    
# 不同处：
    
    内容显示部分排版是简单的三个一排，与知乎日报web的排版不同。（新版本已处理，和原版排版一致）

# 2017/08/17 修改增加

    1、使用HTML5、Sass、Bootstrap4完成新版本。

    2、新版本内容：

      a、nav和header部分对比旧版本完善，实现了全部按钮做了处理效果。

      b、内容部分排版不再是旧版本的三个一排实现了与原版一致。
 
# 感谢
  
- 内容部分使用了 https://segmentfault.com/a/1190000009242864 整理的api，在此表示感谢。 

- 另图片加载时遇到了图片防盗链问题，最后在 https://github.com/biaodigit/vue-news 里找到了相应的处理方法，在此表示感谢。