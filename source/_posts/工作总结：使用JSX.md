---
title: 工作总结：使用JSX
date: 2021-5-18 22:12:43
tags: vue
categories: 工作总结
---

# `vue` 中使用 `jsx` 语法

工作中主要使用技术是`vue`，然后由于自学写过一些`react`的 demo，对`react`的`jsx`语法比较感兴趣，因为其逻辑上束缚小，可以比较自由，特别是在`vue`中开发时遇到较为复杂的功能组件时，感觉有点吃力，而`vue`文档中也有对`jsx`的介绍，所以就想在`vue`中使用`jsx`，然后研究学习了下，然后使用在了工作中。

<!-- more -->

## 格式对比

1. `vue`

```vue
<script>
/**
 * 这是一个测试文件
 */
import styles from "./test.module.less";
export default {
  name:"test",
  data() {
    return {
      val: "testMsg"
    }
  },
  mounted() { console.log("生命周期") },
  methods: {
    click() { alert("click") }
  },
  computed: {
    styles() {
      return styles;
    }
  },
}
</script>

<template>
  <section :class="styles.test">
    <input v-model="val"></input>
    <span @click="click">{{ val }}</span>
  </section>
</template>
```

> `template`写在最下面仅是个人习惯（一是习惯最开头注释写明文件属于及一些说明，而`js`的注释比`html`的注释看起来舒服，二是受到`react`影响里，个人习惯了这样的阅读顺序）。

2. `jsx`

```jsx
/**
 * 这是一个测试文件
 */
import styles from "./test.module.less";
export default {
  // 以下相对于 vue 中 script 内容
  name: "test",
  data() {
    return {
      val: "testMsg"
    }
  },
  mounted() { console.log("生命周期") },
  methods: {
    click() { alert("click") }
  }
  // 以下相当于 vue 中 template 内容
  render() {
    return (
      <div class={ styles.test }>
        <input vModel={ this.val } />
        <span onClick={ ()=>{ this.click() } }>{ this.val }</span>
      </div>
    )
  },
}
```

> `jsx`其实就是把`vue`中的`template`内容放到了`render()`函数里面了，并且有一些细节写法有些不一样。如：vue：`""`，jsx：`{}`
>
> 注意：`jsx`中的`onClick`要用箭头函数来调用方法。

## 注意对比

| vue         | jsx                      |
| :---------- | ------------------------ |
| v-model     | vModel                   |
| @click      | onClick                  |
| class       | class                    |
| <solt />    | {this.$slots.default}    |
| v-if        | &&（一元判断）             |
| v-if v-else | xx ? xx : xx（三元判断符） |
| v-show      | (无)                     |

## 总结
总的来说`vue`中使用`jsx`语法，阅读上并没有什么负担问题，更多的是如果没有学过`react`的纯`vue`开发者一看到可能就有点奇怪，但是只要不抗拒，稍微了解下也就没什么开发难度了，主要就是转换一个思维，把模版语法转换成`javascript`的逻辑去开发就可以了。就是因为这样，所以才会比模版语法更自由，对复杂逻辑的处理束缚更小，开发更符合程序思维。