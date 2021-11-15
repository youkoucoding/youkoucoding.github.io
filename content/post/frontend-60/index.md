---
title: '毎日のフロントエンド　60'
date: 2021-11-15T15:49:03+09:00
description: frontend 每日一练
image: frontend-60-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第六十日

## HTML

### **Question:** `src`、`href`、`link`的区别是什么

> `src`用于替代这个元素，而`href`用于建立这个标签与外部资源之间的关系

1. `href` - `HyperText Reference`

超文本引用， 常用的有： `<a>`,`<link>`, `href`是引用和页面关联，是在当前元素和引用资源之间 **建立联系**

`<link href="reset.css" rel=”stylesheet“/>` 浏览器会识别该文档为 css 文档，**并行下载**该文档，并且不会停止对当前文档的处理。

2. `src` - `source`

src 的内容是页面必不可少的一部分，是 **引入**。`src`指向的内容会嵌入到文档中当前标签所在的位置。常用的有：`<img>`、`<script>`、`<iframe>`

`<script src="script.js"></script>` 当浏览器解析到该元素时，会暂停浏览器的渲染，直到该资源加载完毕。

#### 补充 `link`和`@import`的区别

1. `link`引用`CSS`时，在页面载入时同时加载；`@import`需要页面网页完全载入以后加载。
2. `link`支持使用`JS`控制`DOM`去改变样式；而`@import`不支持
3. `link` 兼容性更好

## JavaScript

### **Question:** 请实现一个`flattenDeep`函数，把多维数组扁平化

solution:

```js
function flatten(arr) {
  return arr.reduce((pre, current) => {
    return pre.concat(Array.isArray(current) ? flatten(current) : current);
  }, []);
}
```

> `arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])`

reduce 接收参数说明：

1. `callback` 函数，执行数组中每个值 (如果没有提供 `initialValue`, 则第一个值除外)的函数， 此函数包含四个参数：

   1. `accumulator`: 累计器， 用于累计回调函数的返回值； acc 的值是上一次调用回调时返回的累计值。或者是 函数外提供的值 => `initialValue`
   2. `currentValue`: 数组中正在被处理的那个元素
   3. `index`(可选)： currentValue 的索引。 **如果提供了`initialValue`， 则 index 从 0 开始，否则从 1 开始**
   4. `array`(可选): 调用`reduce()`的数组

2. `initialValue`(可选):
   作为第一次调用 `callback` 函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的**第一个元素**。 在没有初始值的空数组上调用 `reduce` 将报错。
3. 说明：
   回调函数第一次执行时，`accumulator` 和 `currentValue` 的取值有两种情况：如果调用 `reduce()`时提供了 `initialValue`，`accumulator` 取值为 `initialValue`，`currentValue` 取数组中的第一个值；如果没有提供 `initialValue`，那么 `accumulator` 取数组中的第一个值，`currentValue` 取数组中的第二个值。

### **Question:** 写出执行结果

```js
var length = 10;
function fn() {
  console.log(this.length);
}
var obj = {
  length: 2,
  show: function (fn) {
    this.length = 3;
    fn();
    arguments[0]();
  },
};
obj.show(fn);
```

Answer: 10, 1

Explain:

1. 10 => 来自 `show` 的匿名函数内的 `fn()`,嵌套函数里的 `this` 在未指定的情况下，默认绑定，应该指向的是 `window` 对象(此处**并不指向**调用它外部函数的对象`obj`), `let` 声明变量时会形成块级作用于，且不存在变量提升，而 `var` 存在声明提升,因此，`window.a` 为 10
2. 1 => 可以将`arguments[0]()`看成`arguments.0()`; 因此这里的 `this` 是函数的参数，也就是 `arguments` 的个数

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[Array.prototype.reduce() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
