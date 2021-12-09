---
title: '毎日のフロントエンド　 84'
date: 2021-12-09T17:14:12+09:00
description: frontend 每日一练
image: frontend-84-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八十四日

## HTML

### **Question:** `a`标签下的`href="javascript:void(0)"`起到了什么作用

当用户点击一个以 `javascript:` 开头的`URI` 时，它会执行 URI 中的代码，然后用返回的值替换页面内容，除非返回的值是`undefined`。`void`运算符可用于返回`undefined`。

```html
<a href="javascript:void(0);">
  这个链接点击之后不会做任何事情，如果去掉 void()，
  点击之后整个页面会被替换成一个字符 0。
</a>
<p> chrome中即使<a href="javascript:0;">也没变化，firefox中会变成一个字符串0 </p>
<a href="javascript:void(document.body.style.backgroundColor='green');">
  点击这个链接会让页面背景变成绿色。
</a>
```

> `void` 运算符 对给定的表达式进行求值，然后返回 `undefined`。

- `void`关键字在 js 的含义为执行一个表达式，但不会返回任何值（即`undefined`）；因此 void(0)语句相当于执行表达式 0，然后不返回任何值；
- `href="javascript:void(0)"`的作用是点击链接后不发生任何行为，**常用于阻止页面刷新或跳转**

## CSS

### **Question:** `font-style`的属性有`Italic`和`oblique`，区别

`italic`和`oblique`都是向右倾斜的文字, 但区别在于`Italic`是指**斜体字**，而`Oblique`是倾斜的文字，_对于没有斜体的字体应该使用 Oblique 属性值来实现倾斜的文字效果_

## JavaScript

### **Question:** 简述浏览器同源策略的理解

#### 浏览器的同源安全策略

为了安全，浏览器同源策略是指，某个页面上执行的 `AJAX`请求只能访问到同域名下同端口的 `URL`

#### 跨域

1. 跨域请求不是 JS 限制的，是浏览器的同源策略所限制(可以把同源策略当成浏览器的一种约定，安全策略)
2. 目的是从一个服务去请求另外一个域名和端口不一样的服务从而拿到所需要的数据。(一半用于调用别人的接口数据等)

> 解决跨域一般使用`jsonp`或者**服务端**设置 Response header `Access-control-Allow-Origin`, `Access-Control-Allow-Method`

### **Question:** 写出执行结果,并解释原因

```js
const company = { name: 'facebook' };
Object.defineProperty(company, 'address', { value: 'apple' });
console.log(company);
console.log(Object.keys(company));
```

Result:

```js
{name: 'facebook', address: 'apple'}
['name']
```

`Object.defineProperty()`[^1] 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

[^1]: [Object.defineProperty() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

`Object.defineProperty(obj, prop, descriptor)`

- 使用 `defineProperty` 方法给对象添加了一个属性之后，属性默认为 不可枚举(`not enumerable`)
- `Object.keys`方法仅返回对象中 可枚举(enumerable) 的属性，因此只剩下了 `name`

> 用 `defineProperty` 方法添加的属性默认不可变。可以通过 `writable`, `configurable` 和 `enumerable`属性来改变这一行为。于是，相比于自己添加的属性，`defineProperty` 方法添加的属性有了更多的控制权。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[void 运算符 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/void)
