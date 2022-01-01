---
title: '毎日のフロントエンド  107'
date: 2022-01-01T12:05:13+09:00
description: frontend 每日一练
image: frontend-107-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百零七日

## HTML

### **Question:** 网站的 TDK 该怎么设置？有什么作用

TDK 是`title`, `description`, `keywords`的简称，这三个常用于网站的 seo

- `title`和`description`用于搜索呈现时的网站描述信息，而`keywords`则是用于搜索引擎的关键字搜索

## CSS

### **Question:** 过渡和动画的区别是什么

#### 过渡 `transition`

- 需要事件触发，比如`hover`，`focus`，`checked`
- 只能触发一次
- 只能定义开始和结束状态，不能定义中间状态

#### 动画 animation

- 不需要事件触发
- 显示地随着时间的流逝，周期性的改变元素的 css 属性值。区别于一次性
- 通过百分比来定义过程中的不同形态，可以很细

## JavaScript

### **Question:** 写一个方法判断给定的字符串是否同态(isomorphic)

> 字符串同态是使得每个字母被替代为一个单一字符串的字符串代换。字符串同态就是字符串拥有同样的字符结构

```js
function isomorphic(a, b) {
  if (a.length != b.length) return false;

  let res = true;
  let map = {};

  for (let i = 0; i < a.length; i++) {
    let code1 = a.charCodeAt(i);
    let code2 = b.charCodeAt(i);
    // 判断了每个字母 的 unicode 编码的差值
    let diff = code1 - code2;
    if (map.hasOwnProperty(a[i])) {
      return diff === map[a[i]];
    } else {
      map[a[i]] = diff;
    }
  }
  return res;
}

console.log(isomorphic('add', 'egg')); // true
console.log(isomorphic('paper', 'title')); // true
console.log(isomorphic('xyxx', 'xztt')); // false
console.log(isomorphic('aaaaa', 'abcda')); // false
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[Object.prototype.hasOwnProperty() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)
