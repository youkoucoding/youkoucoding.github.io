---
title: '毎日のフロントエンド　 70'
date: 2021-11-25T15:54:26+09:00
description: frontend 每日一练
image: frontend-70-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第七十日

## HTML

### **Question:** `html` 的 `<a>` 标签属性 `rel='nofollow'` 有什么作用

`rel="nofollow"`

- `<meta name="robots" content="nofollow" />`: 告诉搜索引擎爬虫该页面上所有链接都无需追踪。

- `<a href="login.aspx" rel="nofollow">登录</a>`: 该页面无需追踪。

## JavaScript

### **Question:** 写出执行结果，并解释原因

```js
var x = 1;
if (function f() {}) {
  x += typeof f;
}
console.log(x);
```

Result: `1undefined`

条件判断为**假**的情况有：`0`，`false`，`''`，`null`，`undefined`，`未定义对象`。

函数声明写在运算符中，其为 true，但放在运算符中的函数声明在**执行阶段是找不到**。
另外，对未声明的变量执行`typeOf`不会报错，会返回`undefined`

```js
function f() {
  return f;
}
console.log(new f() instanceof f);
```

Result: `false`

`a instanceof b` 用于检测 a 是不是 b 的实例, 即检测原型链上有没有 `b.prototype` 即 `a.proto === b.prototype || a.proto.proto === b.prototype`。

- 如果题目`f`中没有`return f`,则答案为`true`
- 在本题中`new f()`其返回的结果为 `f` 的函数对象，其并不是 `f` 的一个实例。(`new` 操作符会返回该函数的一个实例)

> The `new` keyword does the following things:
>
> 1. Creates a blank, plain JavaScript object.
> 2. Adds a property to the new object(`__proto__)`) that links to the constructor function's prototype object.
> 3. Binds the newly created object instance as the `this` context (i.e(in other words) `this` in the constructor function now refer to the object created in the first step).
> 4. Returns `this` **if the function doesn't return an object.**

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
