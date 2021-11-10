---
title: '毎日のフロントエンド　55'
date: 2021-11-10T17:43:19+09:00
description: frontend 每日一练
image: frontend-55-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十五日

## HTML

### **Question:** HTML5 中新添加的表单属性有哪些

1. 新的`form`属性[^1]

[^1]: [`<form>`: The Form element - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form)

- `autocomplete`
- `novalidate`

2. 新的`input`属性[^2]
   [^2]: [`<input>`: The Input (Form Input) element - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)
   - 新类型：`color`，`date`，`email`，`month`，`number`，`range`，`search`，`tel`，`time`，`week`
   - 新属性：`autocomplate`，`autofocus`，`list`，`placeholder`

## CSS

### **Question:** 什么是视差滚动？如何实现视差滚动的效果

在同一视角下，鼠标或者页面滚动时，不同元素以不同的速率跟随滚动，产生生动的效果。

如何实现视差滚动： 根据页面滚动高度的变化，JS 相应调整不同元素的不同位移，常见的插件有 [Parallax.js | Simple Parallax Scrolling Effect with jQuery](https://pixelcog.github.io/parallax.js/)

## JavaScript

### **Question:** 写出执行结果，并解释原因

```js
var a = 1;
(function a() {
  a = 2;
  console.log(a);
})();
```

---

立即执行的函数表达式(`IIFE`)的函数名称跟内部变量名称重名后，函数名称优先，因为函数名称是不可改变的，内部会静默失败，在严格模式下会报错

```js
var a = 1;
(function a() {
  'use strict';
  a = 2;
  console.log(a);
})();
// VM1059:4 Uncaught TypeError: Assignment to constant variable.
//  at a (<anonymous>:4:7)
// at <anonymous>:6:3
```

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
