---
title: '毎日のフロントエンド 331'
date: 2022-06-07T21:10:29+09:00
description: frontend 每日一练
categories:
  - CSS
  - JavaScript
---

## HTML

### 去除标签`<i>`默认斜体

- `font-style: normal;`

## CSS

### `writing-mode`

- 用于规定文字的书写方式
  - `horizontal-tb` 从左到右从上到下(水平书写)
  - `vertical-rl` 从上到下从右到左 (垂直书写)
  - `vertiacl-lr` 从上到下从左到右
  - `sideways-rl`：内容垂直方向从上到下排列
  - `sideways-lr`：内容垂直方向从下到上排列

## JS

### JS 循环时应该注意什么

- 循环中不要截取数组，改变数组的长度(不修改对象的结构)
- var 声明的索引 不能给循环创建的闭包函数使用(会导致所有闭包函数使用同一个引用)
- 使用 let 不会有上述情况
- 循环要有终止条件，不能有死循环

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
