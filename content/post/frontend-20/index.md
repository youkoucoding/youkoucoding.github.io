---
title: '毎日のフロントエンド　20'
date: 2021-10-05T11:27:36+09:00
description: frontend 每日一练
image: frontend-20-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二十日

## HTML

### **#Question:** 请描述 HTML 元素的显示优先级

在 html 中，帧元素（`frameset`）的优先级最高(已不提倡使用)，表单元素比非表单元素的优先级要高。

`frameset > 表单元素 > 非表单元素`

- 表单元素:
  - 文本输入框，密码输入框，单选框，复选框，文本输入域，列表框等等
- 非表单元素:
  - `a`，`div`, `table`, `span` 等等

有窗口元素比无窗口元素的优先级高

- 有窗口元素:
  - select 元素，object 元素，以及 frames 元素等等
- 无窗口元素:
  - 大部分 html 元素都是无窗口元素

## CSS

### **#Question:** 要让 Chrome 支持小于 12px 的文字怎么做？

Chrome 中有最小字号的限制，一般为 `12px`。原因是 Chrome 认为小于这个字号会影响阅读。

当需要小于 12px 字体的时候，有以下几个方法可以使用。

`-webkit-text-size-adjust:none;` 这个属性**在高版本的 Chrome 中已经被废除。**

使用 `transform: scale(0.5, 0.5);`，但使用 transform 需要注意下面几点：

- `transform` 对行内元素无效，因此要么使用 `display: block;` 要么使用 `display: inline-block;`
- `transform` 即使进行了缩放，原来元素还是会占据对应的位置。因此需要做调整，最好是在外面再包一层元素，以免影响其他元素
- 使用图片

> 最方便是切图

## JavaScript

### **#Question:** 写一个验证身份证号的方法

> 身份证号码的组成：地址码 6 位+年份码 4 位+月份码 2 位+日期码 2 位+顺序码 3 位+校验码 1 位

```js
function check(val) {
  let reg =
    /^[1-9]\d{5}(19|20)\d{2}((0[1-9])|(1[0-2]))(([0-2][1-9])|(10|20|30|31))\d{3}[0-9Xx]$/;
  return reg.test(val);  // regexObj.test(str)  return a boolean
}
```

## Reference

[HTML 元素的显示优先级 - 简书](https://www.jianshu.com/p/868a7d16fb68)

[Issue #68 haizlin/fe-interview](https://github.com/haizlin/fe-interview/issues/68)