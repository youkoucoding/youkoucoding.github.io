---
title: '毎日のフロントエンド 261~263'
date: 2022-05-16T21:40:43+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### 移动端滚动穿透

#### 什么是滚动穿透

- 移动端开发中在页面上进行弹窗、加浮层等，在上面滑动时，下面的内容也在跟随滚动，即滚动“穿透”到了下方，即滚动穿透（scroll-chaining）

#### 原因

> [CSSOM View Module](https://www.w3.org/TR/2016/WD-cssom-view-1-20160317/#scrolling-events)

- 首先滚动的 target 可以是 document 和里面的 element
- 其次，在 element 上的 scroll 事件是不冒泡的，document 上的 scroll 事件冒泡。
- 想通过在 scroll 的节点上去阻止它的滚动事件冒泡来解决问题是不可行的, 因为它根本就不冒泡，无法触及 dom tree 的父节点， 因此也无法触发滚动

#### 解决

- `overflow: hidden`

> 如果遮罩层是透明的，弹出后用户仍然会看到丢失距离后的下方页面

```css
.modal--open {
  height: 100%;
  overflow: hidden;
}
```

- prevent touch event
  - 直接阻止掉遮罩层和弹窗的 touch event 这样就不会在移动端触发 scroll 事件了。但是在 PC 上没有 touch 事件， scroll 事件仍然可以被触发

```html
<div id="app">
  <div class="mask">mask</div>
  <div class="dialog">dialog</div>
</div>
```

```js
const $mask = document.querySelector('.mask');
const $dialog = document.querySelector('.dialog');
const preventTouchMove = ($el) => {
  $el.addEventListener(
    'touchmove',
    (e) => {
      e.preventDefault();
    },
    { passive: false }
  );
};
preventTouchMove($mask);
preventTouchMove($dialog);
```

- 在 Chrome 56 开始将会默认开启 passive event listener 所以不能直接在 touch 事件中使用 preventDefault，需要先将 passive 选项设置为 false

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [解析移动端滚动穿透 - SegmentFault 思否](https://segmentfault.com/a/1190000020321154)

- [CSSOM View Module](https://www.w3.org/TR/2016/WD-cssom-view-1-20160317/#scrolling-events)

- [使用被动监听器优化滚动体验](https://web.dev/uses-passive-event-listeners/)
