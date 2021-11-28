---
title: '毎日のフロントエンド　 73'
date: 2021-11-28T18:03:33+09:00
description: frontend 每日一练
image: frontend-73-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第七十三日

## HTML

### **Question:** favicon.ico 有什么作用？怎么在页面中引用？常用尺寸有哪些？可以修改后缀名吗

- `favicon`不仅是网站的头像，也是其可以让浏览器的收藏夹中除了显示相应的标题外，还以图标的方式区分不同的网站。

- `<link rel="shortcut icon" type="image/x-icon" href="/favicon.ico">`

- 推荐使用后缀名`.ico`，对浏览器的支持范围广，一个.ico 文件可以同时满足多个尺寸的需求。

- 常用尺寸：`16*16` `32*32` `48*48`

## CSS

### **Question:** 在实际编写 css 中遇到过哪些浏览器兼容性的问题？怎么解决的

TODO

1. [postcss - npm](https://www.npmjs.com/package/postcss)
2. [postcss/autoprefixer: Parse CSS and add vendor prefixes to rules by Can I Use](https://github.com/postcss/autoprefixer)
3. [browserslist/browserslist: 🦔 Share target browsers between different front-end tools, like Autoprefixer, Stylelint and babel-preset-env](https://github.com/browserslist/browserslist)

## JavaScript

### **Question:** 移动端点击事件为什么会有延迟？延迟多长时间？有哪些方法可以解决

原因：等待 300ms 看用户是点击还是双击缩放

解决办法：

1. 禁止缩放
2. 设置默认视口宽度为设备宽度
3. 设置 css `touch-action:none`
4. `fastclick.js`: [ftlabs/fastclick: Polyfill to remove click delays on browsers with touch UIs](https://github.com/ftlabs/fastclick)

### **Question:** 写出执行结果,并解释原因

```js
[typeof null, null instanceof Object];
```

Result: `[object, false]`

1. `typeof` 返回一个**表示类型的字符串**

   - typeof 的结果列表
     - `Undefined "undefined"`
     - `Null "object"`
     - `Boolean "boolean"`
     - `Number "number"`
     - `String "string"`
     - `Symbol "symbol"`
     - `Function "function"`
     - `Object "object"`

2. `instanceof` 运算符用来检测 `constructor.prototype` 是否存在于参数 `object` 的原型链上

### **Question:** 写出执行结果,并解释原因

```js
function f() {}
const a = f.prototype,
  b = Object.getPrototypeOf(f);

console.log(a === b);
```

Result: `false`

- `f.prototype` 是使用 `new` 创建的 `f` 实例的原型.
- `Object.getPrototypeOf` 是 `f` **函数的原型**
- `Function.prototype === Object.getPrototypeOf(f) // true`
- `a === Object.getPrototypeOf(new f()) // true`
- `b === Function.prototype // true`
- `a === b // false`

> `Object.getPrototypeOf()` 方法返回指定对象的原型（内部`[[Prototype]]`属性的值）。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
