---
title: '毎日のフロントエンド　51'
date: 2021-11-06T14:08:07+09:00
description: frontend 每日一练
image: frontend-51-cover.jpg
categories:
  - Javascript
---

# 第五十一日

## JavaScript

### **Question:** 字符串相连有哪些方式？哪种最好？为什么？

```js
var a = 'aaaa';
var b = 'bbbbb';

// 方法一： “+”
var c = a + b;
console.log('c:', c);

// 方法二： “join("")”
var d = [];
d.push(a, b);
console.log('d:', d.join(''));

// 方法三：模版字符串 `${}`
var e = `${a}${b}`;
console.log('e:', e);
```

## Test

### **Question:** 说一下单元测试、E2E 测试？它们有什么区别？

1. Unit Test

单元测试是用来对一个模块， 一个函数或者一个类进行正确性校验的测试工作。 是从程序员的角度进行测试

2. E2E Test

站在用户的角度进行测试。 不关心内部实现。

3. 区别。

- Unit 测试是程序员写好逻辑后，很容易测试自己的实现， 是否符合预期。
- E2E 是测试所有需求是不是都可以正确的实现。**且代码重构之后，需求不变的情况下， 测试代码是无需改变的。**
