---
title: '毎日のフロントエンド　8'
date: 2021-09-22T16:40:32+09:00
description: frontend 每日一练
image: frontend-8-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八日

## CSS

**#Question:** 清除浮动的方式有哪些及优缺点？

- 现阶段 较多使用`Flex` 布局
- 浮动带来的问题是盒子塌陷问题

#### 什么是盒子塌陷？

外部盒子本应该包裹住内部的浮动盒子，结果却没有

#### 原因

父元素只包含浮动元素，那么它的高度就会塌缩为零, 前提是没有设置高度`height`属性，或者设置为 `auto`，就会出现盒子塌陷，如果父元素不包含任何的可见背景，这个问题会很难被注意到。
因为子元素设置了 `float` 属性，而 float 属性会把元素从标准文档流中抽离，直接结果就是外部盒子丢了两个孩子，因为内部没有其它盒子了，所以外部盒子只包裹文本节点内容，却把两个内部盒子扔在外面了。

#### 解决方案

1.  把外部盒子也从标准文档流中抽离

    - 缺点是： 父元素加上 `float` 有可能影响整个页面

2.  在外部盒子内最下方添上带 clear 属性的空盒子： 把 `<div style="clear:both;"></div>`放在盒内底部

3.  给外部盒子添加： `overflow:hidden` 清除浮动

4.  用`after`伪元素清除浮动
    - ```css
      .clearfix {
        \*zoom: 1;
      }
      .clearfix:before,
      .clearfix:after {
        display: table;
        line-height: 0;
        content: '';
      }
      .clearfix:after {
        clear: both;
      }
      ```
5.  当然，还有前文提到的 `BFC`

## JavaScript

**#Question:** 写一个加密字符串的方法

```js
function encodeStr(str, key) {
  return str
    .split('')
    .map((item) => {
      return item.charCodeAt() * key;
    })
    .join('#');
}

function decodeStr(str, key) {
  return str
    .split('#')
    .map((item) => {
      return String.fromCharCode(+item / key);
    })
    .join('');
}

console.log(decodeStr(encodeStr('hello world', 665), 665));
```

## Reference

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
