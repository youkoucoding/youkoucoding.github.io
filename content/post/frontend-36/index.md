---
title: '毎日のフロントエンド　36'
date: 2021-10-22T11:27:44+09:00
description: frontend 每日一练
image: frontend-36-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第三十六日

## HTML

### **Question:** HTML 与 XHTML 二者有不同

HTML vs HTML5 vs XHTML

All three are _markup_ language, performing, essentially, the same task.

#### HTML and HTML5

A markup language most common today is HTML. It was designed by the inventor of the World Wide Web.

#### XHTML

It is worth noting that before HTML5 was released, the World Wide Web Consortium (W3C) initiated work to develop an extension of the basic HTML, merging it with XML format.

This was meant to resolve some compatibility issues that browsers were experiencing at the time.

#### Differences between HTML vs XHTML vs HTML5

Listed below are some of the main differences between `HTML` and `HTML5`:

- `HTML5` provides better support for different types of media – for example, audio and video. _It is done by providing additional tags for media files._
- `HTML5` enables `Javascript` to run in the browser – a feature, previously unavailable in HTML.
- `HTML5` introduced brand-new input attributes, including `email`, `URLs`, `date` and `time`, and search, to name a few.
- `HTML5` has better browser compatibility than HTML. It is also device-independent.
- `HTML5` adopts superior text processing rules, also known as `parsing`. This allows for much greater parsing flexibility than HTML.
- `HTML5` now makes it a lot easier to find locations without the need for third-party plugins.
- `HTML5` provides native support for vector graphics. _This reduces the need for third-party software, such as Adobe Flash._

Listed below are some of the main differences between `HTML` and `XHTML`:

- `HTML` accepts that some elements may not contain the closing tag. `XHTML` expects that all elements with no exception include a closing tag(`XHTML` 标签必须关闭)
- XHTML, unlike HTML, does not allow elements to overlap.
- For a document to be valid XHTML, attributes cannot be minimized.
  -XHTML requires that all attribute values (e.g., font size) are quoted – even the numeric ones. There is no such requirement in HTML.

Listed below are some of the main differences between `HTML5` and `XHTML`:

- `XHTML` is **case sensitive** (same as HTML), whereas HTML5 is not.
- Both XHTML and HTML have a more complex doctype than HTML5.
- HTML5 is compatible with all browsers. XHTML is not.
- HTML5, being a successor of HTML, is much more flexible than XHTML.
- XHTML is better suited for desktop computers, while HTML5 is better suited for mobile devices — smartphones and tablets.

## CSS

### **Question:** 写出主流浏览器内核私有属性的 css 前缀

| 浏览器  | 内核              | CSS 前缀   |
| ------- | ----------------- | ---------- |
| Chrome  | Blink 内核（新）  | `-webkit-` |
| Firefox | Gecko 内核        | `-moz-`    |
| Safari  | Webkit 内核       | `-webkit-` |
| Opera   | Webkit 内核（新） | `-o-`      |
| IE/Edge | Trident 内核      | `-ms-`     |

## JavaScript

### **Question:** 合并二维有序数组成一维有序数组，归并排序的思路

```js
/**
 * 解题思路：
 * 双指针 从头到尾比较 两个数组的第一个值，根据值的大小依次插入到新的数组中
 * 空间复杂度：O(m + n)
 * 时间复杂度：O(m + n)
 * @param {Array} arr1
 * @param {Array} arr2
 */
function merge(arr1, arr2) {
  let result = [];
  while (arr1.length > 0 && arr2.length > 0) {
    if (arr1[0] < arr2[0]) {
      /*shift()方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。*/
      result.push(arr1.shift());
    } else {
      result.push(arr2.shift());
    }
  }
  return result.concat(arr1).concat(arr2);
}

function mergeSort(arr) {
  let lengthArr = arr.length;
  if (lengthArr === 0) {
    return [];
  }
  while (arr.length > 1) {
    let arrayItem1 = arr.shift();
    let arrayItem2 = arr.shift();
    let mergeArr = merge(arrayItem1, arrayItem2);
    arr.push(mergeArr);
  }
  return arr[0];
}
let arr1 = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
  [1, 2, 3],
  [4, 5, 6],
];
let arr2 = [
  [1, 4, 6],
  [7, 8, 10],
  [2, 6, 9],
  [3, 7, 13],
  [1, 5, 12],
];
mergeSort(arr1);
mergeSort(arr2);
```

## Soft Skill

### **Question** 前端安全，说说对 `XSS` 和 `CSRF` 的理解

`XSS`攻击全称跨站脚本攻击,一般有`sql`注入，`js`脚本注入。
评论类型模块的提交过程中不要相信客户的输入内容，需要进行转义。

`csrf`跨站请求伪造
请求需要鉴权，如携带 `token`，或者利用 `seesion`，`cookie`。敏感信息提交可以使用验证码

## Reference

[HTML vs HTML5 vs XHTML: the Difference](https://scandiweb.com/blog/html-vs-html5-vs-xhtml-the-difference-in-a-nutshell/)

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
