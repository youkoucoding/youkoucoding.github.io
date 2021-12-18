---
title: '毎日のフロントエンド  93'
date: 2021-12-18T20:27:51+09:00
description: frontend 每日一练
image: frontend-93-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第九十三日

## HTML

### **Question:** 用`svg`画一个圆

```html
<svg width="500" height="500">
  <circle cx="100" cy="100" r="80" fill="skyblue"></circle>
</svg>
```

## CSS

### **Question:** 写出 div 在不固定高度的情况下水平垂直居中的方法

- `flex`布局:

```css
#section {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

- `transform`属性

```css
.father {
  position: relative;
}
.children {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```

## JavaScript

### **Question:** js 的函数有哪几种调用形式

定义：`function fn(){}`

- 常见调用：`fn()`
- 作为对象方法调用：
  - `let obj = {fn:function(){}};`
  - `obj.fn()`
- 使用构造函数调用：`new fn()`
- 使用 `call` `apply` `bind` 调用：`fn.call(); fn.apply()`

### **Question:** 写出代码执行结果

```js
[1, 2, 3, 4, 5].map(parseInt);
```

Result: `[1, NaN, NaN, NaN, NaN]`

`Array.prototype.map(callback(currentValue[, index[, array]])[, thisArg])`

- `callback`
  - 生成新数组元素的函数，使用三个参数：
    - `currentValue`: 数组中正在处理的当前元素
    - `index`(可选):数组中正在处理的当前元素的索引
    - `array`(可选): `map` 方法调用的数组
  - `thisArg`(可选): 执行 callback 函数时值被用作`this`。

`parseInt(string, radix)` 解析一个字符串并返回指定基数的十进制整数， `radix` 是 2-36 之间的整数，表示被解析字符串的基数。

`NaN`，当

- radix 小于 2 或大于 36 ，或
- 第一个非空格字符不能转换为数字。

`map`方法执行的过程中，`parseInt`将会进行如下调用：

```js
parseInt('1', 0); // 0 索引
parseInt('2', 1); // 1 索引 radix 小于2 NaN
parseInt('3', 2); // 2 索引 3转二进制位 NaN
parseInt('4', 3); // 3 索引 NaN
parseInt('5', 4); // 4 索引 NaN
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[SVG | MDN](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial)

[Array.prototype.map() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
