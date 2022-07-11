---
title: '毎日のフロントエンド 360'
date: 2022-07-10T23:51:14+09:00
description: frontend 每日一练
categories:
  - CSS
  - JS
  - Tips
---

## CSS

### `height:100%;`

> 如何让 height:100%起作用

- 当前元素的包含块的高度是**明确**的即可有效（height 不为 auto）
- 如果当前元素的包含块高度也是百分比，就再向上找
- 如果当前元素是`absolute`定位的，那么它的`height`百分比，是相对于浏览器窗口高度的, 绝对定位元素的高度百分比计算是往上找直到找到一个 非 static 的元素
- 其他的都是最后找到 html 元素，html 元素的高度百分比也是相对浏览器窗口高度， body 默认没有 height

## Tips

### 屏幕坐标、（可视窗口）坐标、页面坐标

#### 屏幕坐标

- 整个电脑屏幕上任意一点的位置坐标，对应的属性分别为`screenX`，`screenY`

#### （可视窗口）坐标

- 可以将浏览器分为两大块，即浏览器上部的用户操作栏 和下部的可视窗口区域 ，该坐标系 的原点位于可视窗口的右上角，对应的属性为`clientX`,`clientY`

#### 页面坐标

- 页面坐标通过事件对象的 `pageX` 和 `pageY` 属性，能告诉你事件是在页面中的什么位置发生的。换句话说，这两个属性表示**鼠标光标在页面中的位置**，因此坐标是从页面本身而非视口的左边和顶边计算的

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
