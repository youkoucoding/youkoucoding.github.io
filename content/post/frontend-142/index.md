---
title: '毎日のフロントエンド  142'
date: 2022-02-05T20:35:58+09:00
description: frontend 每日一练
image: frontend-142-cover.jpg
categories:
  - Javascript
---

# 第一百四十二日

## CSS

### **Question:** `word-wrap`、`word-break`和`white-space`有什么区别

1. `overflow-wrap` 是否允许浏览器再单词内进行断句

   - normal 只允许在断字点换行 默认值
   - break-word 在长单词或这 url 地址内部进行换行

2. `word-break` 怎么样进行单词内的断句

   - normal 默认值
   - break-all 允许在单词内换行
   - keep-all 只能在半角空格或字符串处换行

3. `white-space` 属性设置如何处理元素内的空白
   - normal 默认值
   - pre 空包会被浏览器保存
   - nowrap 文本不会换行，在同一行显示，知道遇到 br 标签为止
   - pre-wrap 保留空白序列，会正常的换行
   - pre-line 合并空白序列，会保留换行符
   - inherit 应该从父元素继承 white-space 属性的值

## JavaScript

### **Question:** `Promise.all`

- `Promise.all()`接受一个由**promise 任务组成的数组**，可以同时处理多个 promise 任务，当所有的任务都执行完成时，Promise.all()返回 resolve，但当有一个失败(reject)，则返回失败的信息，即使其他 promise 执行成功，也会返回失败。

- 对于 `Promise.all(arr)` 来说，在参数数组中所有元素都变为决定态后，然后才返回新的 promise

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[前端进阶](https://muyiy.cn/question/)

[overflow-wrap - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow-wrap)

[word-break - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/word-break)

[white-space - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space)

[Promise.all() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
