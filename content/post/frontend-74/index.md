---
title: '毎日のフロントエンド　 74'
date: 2021-11-29T16:22:25+09:00
description: frontend 每日一练
image: frontend-74-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第七十四日

## HTML

### **Question:** 在`a`标签上的四个伪类执行顺序是什么

1. `a:link`: 定义正常链接的样式
2. `a:visited`: 定义已访问过链接的样式
3. `a:hover`: 定义鼠标悬浮在链接上时的样式
4. `a:active`: 定义鼠标点击链接时的样式

`link--visited--hover-active`，也就是我们常说到的 LoVe HAte 原则（大写字母就是它们的首字母）

在`W3C`规范中，也规定了链接的声明顺序：
在 `CSS` 定义中，`a:hover` 必须被置于 `a:link` 和 `a:visited` 之后，才是有效的。
在 `CSS` 定义中，`a:active` 必须被置于 `a:hover` 之后，才是有效的。

## CSS

### **Question:** `!important`，一般在哪些场景使用

`!important` 可以让样式的 特指度最高，覆盖任何样式，而且本身不可被覆盖。
一般场景就是用来强制覆盖其他样式，用的比较少，不建议使用。

## JavaScript

### **Question:** 写一个方法随机生成指定位数的字符串

```js
function getRandomString(length) {
  //let str = Math.random().toString(36).substr(2); // substr: This feature is no longer recommended.
  // substring(2) remove the 0 and .
  let str = Math.random().toString(36).substring(2);

  if (str.length >= length) {
    return str.substring(0, length);
  }
  str += getRandomString(length - str.length);
  return str;
}
```

- `toString(36)`: `radix`是`36`，就会把数字表示为由`0-9`, `a-z`组成的的"36 进制字符串".
- `substring()` 方法返回一个字符串在开始索引到结束索引之间的一个子集, 或从开始索引直到字符串的末尾的一个子集。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[String.prototype.substring() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

[Object.prototype.toString() - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
