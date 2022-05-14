---
title: '毎日のフロントエンド 253~256'
date: 2022-05-14T20:32:27+09:00
description: frontend 每日一练
categories:
  - CSS
---

## CSS

### 移动端动态计算根元素的字体大小

```js
(function (doc, win) {
  var docEl = doc.documentElement, //文档根标签
    resizeEvent = 'orientationchange' in window ? 'orientationchang' : 'resize'; //viewport变化事件源
  var rescale = function () {
    win.clientWidth = docEl.clientWidth;
    if (!win.clientWidth) return;
    // (屏幕宽度/设计图宽度) = 缩放或扩大的比例值;
    docEl.style.fontSize = 36 * (win.clientWidth / 375) + 'px';
  };
  if (!doc.addEventListener) return;
  win.addEventListener(resizeEvent, rescale, false);
  doc.addEventListener('DOMContentLoaded', rescale, false);
})(document, window);
```

### `display:none`和`visibility:hidden`

- `display:none` dom 对象不渲染, 不占位置
- `visibility:hidden` 隐藏但是 dom 对象渲染, 占位置

### `user-select`

- CSS 属性 user-select 控制用户能否选中文本。除了文本框内，它对被载入为 chrome 的内容没有影响

- `none`：
  - 文本不能被选择
- `text`：
  - 可以选择文本
- `all`：
  - 当所有内容作为一个整体时可以被选择。如果双击或者在上下文上点击子元素，那么被选择的部分将是以该子元素向上回溯的最高祖先元素。
- `element`：
  - 可以选择文本，但选择范围受元素边界的约束

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [user-select - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/user-select)
