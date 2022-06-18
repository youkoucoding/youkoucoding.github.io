---
title: '毎日のフロントエンド 335~336'
date: 2022-06-18T20:12:22+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

## CSS

### `pointer-events`

- pointer-events CSS 属性指定在什么情况下 (如果有) 某个特定的图形元素可以成为鼠标事件的 target。
- point-events 为 none 时，比如 a 连接不再生效

### css 的`:hover`和 js 的`mouseover`

- `:hover`为 CSS 伪类，`mousehover`为 JS DOM 事件

- CSS 只能改变元素样式，JS 既可以改变元素样式又可以改变元素中的内容。
- :hover 当鼠标移出后恢复之前的样式，mouseover 需要结合 mouseout 才能恢复之前的样式
- 同等效果下，从性能上讲，:hover 优于 mousehover

## JS

### 前端做单元测试时基本原则

- 测试代码时，只考虑测试，不考虑实现
- 测试数据尽量模拟现实
- 测试时要考虑边界条件
- 对复杂重点核心代码重点测试

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [pointer-events - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/pointer-events)
