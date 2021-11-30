---
title: '毎日のフロントエンド　 75'
date: 2021-11-30T09:01:53+09:00
description: frontend 每日一练
image: frontend-75-cover.jpg
categories:
  - CSS
  - Javascript
---

# 第七十五日

## CSS

### **Question:** 解释下什么是浮动和它的工作原理是什么？同时浮动会引起什么问题

- 什么是浮动：通过浮动可以让元素左右浮动，然后通过`margin`调整位置
- 工作原理：使元素脱离文档流，让元素可以左右浮动，直到遇到另一个浮动元素的边缘才停止。
- 带来的问题：浮动元素会造成父级元素无法自动获取高度，导致父级塌陷，布局错乱。

## JavaScript

### **Question:** js 中`=`、`==`、`===`三个的区别是什么？并说明它们各自的工作过程

- `=` 是赋值运算符

  - 遵循右结合律
  - 返回 ` lhs(left-hand-side)` （但在声明语句（`var`, `let`, `const`）中返回 `undefined`）
  - 若 ` rhs(right-hand-side)` 是 `primitive value` （`number`, `string`, `symbol`, `undefined`, `boolean`) 则 `lhs` 被赋值为该值
  - 若 `rhs` 是 `object` 则 `lhs` 被赋值为指向该 `object` 的 `reference`
  - `const` 声明的不变量不能被再次赋值，否则会 `throw ReferenceError`
  - 如果在局部作用域不使用声明语句就给一个既未声明于局部作用域，也未声明于任何上层作用域的变量赋值，那它将会被创建为一个全局变量。不应该如此使用。

- `==` 是带有 `implicit type conversion`(隐式类型转换) 的判等运算符

  - 遵循左结合律
  - 返回 boolean

- `===` 是严格的判等运算符
  - 遵循左结合律
  - 返回 `boolean`
  - 若两侧是 `primitive value` 则判断两侧值是否相等
  - 若两侧是 `object` 则判断两侧 `reference` 是否指向同一块内存

### **Question:** 写出执行结果,并解释原因

```js
function showCase(value) {
  switch (value) {
    case 'A':
      console.log('Case A');
      break;
    case 'B':
      console.log('Case B');
      break;
    case undefined:
      console.log('undefined');
      break;
    default:
      console.log('Do not know!');
  }
}
showCase(new String('A'));
```

Result: Do not know!

`switch` 是严格比较, `String` 实例和 字符串不一样

```js
var str1 = 'str';
var str2 = new String('str');
console.log(typeof str1); // "string"
console.log(typeof str2); //"object"
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[与 `<head>` 相关的标签列表。](https://github.com/Amery2010/HEAD#meta-%E6%A0%87%E7%AD%BE)
