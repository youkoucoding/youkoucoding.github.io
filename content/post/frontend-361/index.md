---
title: '毎日のフロントエンド 361~364'
date: 2022-07-12T22:29:38+09:00
description: frontend 每日一练
categories:
  - CSS
  - JS
  - Tips
---

## CSS

### `height:100%` 和 `height:inherit`区别

- 父容器 height: auto，无论 height:100%或者 height:inherit 表现都是 auto.
- 父容器定高 height: 100px，无论 height:100%或者 height:inherit 表现都是 100px 高.

---

- 在 position: absolute 的情况下
  - `height：100%`对应的标准是上一个 position:relative 的元素
  - `height： inherit` 依然是它的父级元素

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
