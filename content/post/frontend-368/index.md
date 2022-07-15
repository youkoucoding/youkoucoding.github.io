---
title: '毎日のフロントエンド 368~371'
date: 2022-07-15T23:22:16+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JS
  - Tips
---

## HTML

### 层叠上下文、层叠等级、层叠顺序

- 层叠上下文(stacking context)，是 HTML 中一个三维的概念。在 CSS2.1 规范中，每个盒模型的位置是三维的，分别是平面画布上的 X 轴，Y 轴以及表示层叠的 Z 轴。如果一个元素含有层叠上下文，(也就是说它是层叠上下文元素)，最终表现就是它离屏幕观察者更近。

- 层叠等级(stacking level，“层叠级别”/“层叠水平”)。在同一个层叠上下文中，它描述定义的是该层叠上下文中的层叠上下文元素在 Z 轴上的上下顺序。在其他普通元素中，它描述定义的是这些普通元素在 Z 轴上的上下顺序。

- 层叠顺序(stacking order)表示元素发生层叠时按照特定的顺序规则在 Z 轴上垂直显示。

![stacking order](./stacking-order.png)

## Tips

### Source Order View

> Once you have enabled the Source Order Viewer, the current inspected page presents a visualisation (within the inspected page) which indicates the document source order of page elements.

1. Inspect an element on a web page.
2. Open the Accessibility Pane (in the Elements panel).
3. Enable the Show source order option.

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
- [purifycss/purifycss: Remove unused CSS. Also works with single-page apps.](https://github.com/purifycss/purifycss)
- [Source Order Viewer - Chrome DevTools - Dev Tips](https://umaar.com/dev-tips/245-source-order-viewer/)
