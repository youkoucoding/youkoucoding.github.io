---
title: '毎日のフロントエンド　54'
date: 2021-11-09T11:39:45+09:00
description: frontend 每日一练
image: frontend-54-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十四日

## HTML

### **Question:** 了解 HTML5 的地理定位吗？怎么使用

`Geolocation.getCurrentPosition()` 方法用来获取设备当前位置

```js
navigator.geolocation.getCurrentPosition(success, error, options);
```

## CSS

### **Question:** css3 的`:nth-child` 和`:nth-of-type` 的区别是什么？

`:nth-child(n)` 选择器匹配属于其父元素的第 `N` 个子元素，不论元素的类型。
`:nth-of-type(n)` 选择器匹配属于父元素的特定类型的第 N 个子元素。`n` 可以是数字、关键词或公式

## JavaScript

### **Question:** 写一个函数找出给定数组中的最大差值

```js
function difference(arr) {
  return Math.max(...arr) - Math.min(...arr);
}
```

## One more Question

写出执行结果，并解释原因

```js
function side(arr) {
  arr[0] = arr[2];
}
function a(a, b, c = 3) {
  c = 10;
  side(arguments);
  return a + b + c;
}
a(1, 1, 1);
```

---

12

`arguments` 中 c 的值还是 1 不会变成 10。
c=1 赋值给了 a， 1+1+10， 10 为函数 a 中块级作用域内定义的`c=10`。

因为 `function a()` 函数加了默认值，就按 ES 的方式解析，`ES6` 是有块级作用域的，`c` 的值不会改变

## Reference

[Geolocation.getCurrentPosition() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/getCurrentPosition)

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
