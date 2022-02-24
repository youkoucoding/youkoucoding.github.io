---
title: '毎日のフロントエンド  161'
date: 2022-02-24T16:52:49+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## HTML

### HTML 调用摄像头

- `window.navigator.getUserMedia()`

  - 接收三个参数，第一个是视频或者音频以及分辨率{video:true}第二个是成功回调，第三个是失败回调

- `window.navigator.mediaDevices.getUserMedia()`
  - 也是三个参数，参数格式和上文一样，区别在于这个 api 是基于 promise 实现的

## JavaScript Nodejs

### `Process.nextTick`

- `process.nextTick(callback[, ...args])`

  - 第一个参数是 callback 回调函数，第二个参数是 args 调用 callback 时额外传的参数，是可选参数

- `Process.nextTick` 是微任务 也是异步 API 的一部分

- `Process.nextTick` 的运行逻辑：

  - `Process.nextTick` 会将 `callback` 添加到“next tick queue”；
  - “next tick queue”会在当前 JavaScript stack 执行完成后，下一次 event loop 开始执行前按照 FIFO 出队；
  - 如果递归调用 Process.nextTick 可能会导致一个无限循环，需要去适时终止递归。

- 根据代码执行顺序，`Process.nextTick` 是在每一次的事件循环最后执行的。

```js
let bar;

function someAsyncApiCall(callback) {
  process.nextTick(callback);
}

someAsyncApiCall(() => {
  console.log('bar', bar); // 1
});
```

bar = 1;

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[MediaDevices.getUserMedia() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaDevices/getUserMedia)

[你未必知道的 49 个 CSS 知识点](https://juejin.cn/post/6844903902123393032)

[一些 css 技巧](https://github.com/haizlin/fe-interview/issues/1252)
