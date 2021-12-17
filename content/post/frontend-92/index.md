---
title: '毎日のフロントエンド  92'
date: 2021-12-17T17:35:52+09:00
description: frontend 每日一练
image: frontend-92-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第九十二日

## HTML

### **Question:** `ISISO8859-2`字符集

ISO/IEC 8859-1，又称 Latin-1 或“西欧语言”，ISO/IEC 8859-2 Latin-2 或“中欧语言”，是国际标准化组织内 ISO/IEC 8859 的 8 位字符集。它以 ASCII 为基础，在空置的 0xA0-0xFF 的范围内，加入 192 个字母及符号，藉以供使用变音符号的拉丁字母语言使用。 (ASCII 的扩展)

## CSS

### **Question:** 媒体查询

`media query`

## JavaScript

### **Question:** 为什么`{} + [] === 0`为`true`

- `{}` 被认为是代码块,不参与计算， 因而 `+[] === 0 // true`;
- 如果写成 `[] + {}` 则结果为 `[object Object]`

### 手写实现 `Array.flat()`

方法一

```js
function flat(arr) {
  const stack = [...arr];
  const res = [];
  while (stack.length) {
    const next = stack.pop();
    if (Array.isArray(next)) {
      stack.push(...next);
    } else {
      res.push(next);
    }
  }
  return res.reverse();
}
```

方法二

```js
// depth, 可指定嵌套数组的深度
function flat(arr, depth = 1) {
  if (depth > 0) {
    return arr.reduce(
      (acc, val) => acc.concat(Array.isArray(val) ? flat(val, depth - 1) : val),
      []
    );
  } else {
    return arr.slice();
  }
}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[使用媒体查询 - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media_Queries/Using_media_queries)

[@media - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media)

[Symbol.toPrimitive - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive)

[JS 中的加号+运算符详解](https://www.cnblogs.com/MasterYao/p/7783004.html)

[Array.prototype.flat() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)
