---
title: '毎日のフロントエンド　 68'
date: 2021-11-23T12:07:51+09:00
description: frontend 每日一练
image: frontend-68-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第六十八日

## HTML

### **Question:** 说明`HTML5`在移动端如何打开 APP

```js
<a href='zhihu://'>打开应用</a>
```

`Android` 是利用 `deeplink`， `iOS` 是利用 `URL Schemes`

- `URL Scheme`

```
[scheme:][//authority][path][?query][#fragment]
```

## CSS

### **Question:** 对 jpg、png、gif 的理解，分别在什么场景下使用？有使用过 webp 吗

`jpg`: 色彩复杂图片; `png`: 色彩简单图片; `gif`: 动图, 或者色彩极简的 icon 等

`webp`: 判断能使用`webp`的浏览器就是用`webp`

`WebP`是一种支持有损压缩和无损压缩的图片文件格式，根据 Google 的测试，无损压缩后的 WebP 比 PNG 文件少了 26％的体积，有损压缩后的 WebP 图片相比于等效质量指标的 JPEG 图片减少了 25％~34%的体积。

在浏览器中可以采用 `JavaScript` 检测是否支持 `WebP`，对支持 `WebP` 的用户输出 `WebP` 图片，否则输出其他格式的图片。

## JavaScript

### **Question:** `FormData`主要是用来做什么的？它的操作方法有哪些

`FormData` interface provides a way to easily construct a set of **key/value** pairs representing form fields and their values, which can then be easily sent using the `XMLHttpRequest.send()` method. It uses the same format a form would use if the encoding type were set to `"multipart/form-data"`.

### **Question:** `js`动画和`css`动画有什么区别

1. js 动画

   - 会进入函数调用栈，走完事件循环才会渲染
   - 相比 `css` 动画（不考虑 css 变量），js 动画可配置目标值或速率等
   - `js` 动画做暂停、反向和复杂的动画更易用

2. css 动画
   - 简易的 `hover` `active` `checked` 等动效用 css
   - 对循环播放的动画，多数情况下也是 css 动画更佳
   - css 动画库的复用方便

### **Question:** 写出执行结果，并解释原因

```js
var foo = function bar() {
  return 12;
};
console.log(typeof bar());
```

- 输出是抛出异常，bar is not defined。

上述命名函数表达式函数只能在函数体内有效

```js
var foo = function bar() {
  // foo is visible here
  // bar is visible here
  console.log(typeof bar()); // Work here :)
};
// foo is visible here
// bar is undefined here

typeof(bar). // "undefined"
typeof(foo()). // "number"
typeof(foo).   // "function"
typeof(bar()). // VM5167:1 Uncaught ReferenceError: bar is not defined

```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[H5 唤起 APP 指南(附开源唤端库) - 掘金](https://juejin.cn/post/6844903664155525127)

[JELLY | 探究 WebP 一些事儿](https://jelly.jd.com/article/6006b1035b6c6a01506c87a9)

[FormData - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/FormData)
