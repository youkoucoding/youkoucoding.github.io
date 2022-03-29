---
title: '毎日のフロントエンド 190'
date: 2022-03-28T10:54:51+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
  - Tips
---

## CSS

### 颜色中`#F00` 的每一位分别表示什么？为什么会有三位和六位的表示

- 颜色可以使用红-绿-蓝（red-green-blue (RGB)）模式的两种方式被定义：
  - 十六进制符号 `#RRGGBB` 和 `#RGB`
  - 三位数的 RGB 符号（#RGB）和 六位数的形式（#RRGGBB）是相等的。
    - `#f03` 和 `#ff0033` 代表同样的颜色

## JavaScript

### 原生 js 实现类似`getElementsByClassName`的方法，不使用`querySelectorAll`

```js
function getElementsByClassName(className) {
  const tags = document.getElementsByTagName('*');
  const tempTags = [];
  for (let i = 0, len = tags.length; i < len; i++) {
    let tag = tags[i];
    tag.classList.contains(className) && tempTags.push(tag);
  }
  return tempTags;
}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Document.getElementsByTagName() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByTagName)
