---
title: '毎日のフロントエンド  112'
date: 2022-01-06T12:12:18+09:00
description: frontend 每日一练
image: frontend-112-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百一十二日

## HTML

### **Question:** `checkbox`有几种状态吗？它们分别用来表示什么

`checkbox`状态的属性有`checked`，`disabled`和`indeterminate`:

- `checked`：选中/未选中状态；
- `disabled`：禁用/可用状态；
- `indeterminate`：用于表示复选框组介于全部选中或全部未选中之间的状态；

## CSS

### **Question:** 时间、频率、角度、弧度、百分度的单位分别是哪些

- 时间: `s`, `ms`
- 频率: `Hz`
- 角度: `deg`
- 弧度: `rad`
- 百分度: `grad`

## JavaScript

### **Question:** 函数声明与函数表达式有什么区别

> 两者最大的区别是： **函数声明**会提升至作用域的顶端，函数表达式则会在赋值之后能调用。

- 函数声明
  > 在函数声明被定义之前，它就可以被调用

```js
// 函数名必须有
function sayHi() {
  alert('Hello');
}
```

- 函数表达式
  > 函数表达式是在代码执行到达时被创建，并且仅从那一刻起可用(函数表达式在代码执行到它时才会被创建)

```js
// 在函数表达式中可以省略函数名
let sayHi = function () {
  alert('Hello');
};
```

---

在大多数情况下，当我们需要声明一个函数时，最好使用函数声明，因为函数在被声明之前也是可见的。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)
