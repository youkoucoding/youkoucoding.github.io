---
title: '毎日のフロントエンド 257~260'
date: 2022-05-15T21:07:28+09:00
description: frontend 每日一练
categories:
  - Tips
---

## CSS

### `opacity: 0、visibility: hidden、display: none`

- `opacity: 0` : 单纯视觉效果，除了看不见，其他都正常, 不会改变页面布局，能触发点击事件的
- `visibility: hidden` : 可继承也可覆盖, 不会触发该元素已经绑定的事件
- `display: none` : 元素不会渲染，不影响布局，不会被 css 计数，也不支持 transition, 动态创建和销毁元素，会改变页面布局且忽略掉该元素的权重

## Tips

### js 事件中`currentTarget`和`target`的区别

#### `event.currentTarget`

- Event 接口的**只读属性** currentTarget 表示的，标识是当事件沿着 DOM 触发时事件的当前目标。它总是指向事件绑定的元素，而 Event.target 则是事件触发的元素。

#### `Event.target`

- 触发事件的对象 (某个 DOM 元素) 的引用。当事件处理程序在事件的冒泡或捕获阶段被调用时，它与 event.currentTarget 不同。

### CDN

- [The History of Content Delivery Networks (CDN) | GlobalDots](https://www.globaldots.com/resources/blog/the-history-of-content-delivery-networks-cdn/)

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [Event.target - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/target)

- [event.currentTarget - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/currentTarget)
