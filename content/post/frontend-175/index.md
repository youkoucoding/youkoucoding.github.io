---
title: '毎日のフロントエンド  175'
date: 2022-03-13T17:19:01+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### css 文件过大时， "异步加载 CSS"

1. 利用媒体查询

`<link rel="stylesheet" href="./index.css" media="none" onload="this.media='all'">`

OR

```html
<link rel="stylesheet" href="./index1.css" media="screen and (max-width: 800px)" />
<link rel="stylesheet" href="./index2.css" media="screen and (min-width: 800px)" />
```

> rel means relationship

2. 提前加载资源

> **优先级最高**，异步加载，不会阻塞 DOM 的渲染，浏览器支持度比较低。

`<link rel="preload" href="./index.css" as="style">`

- 该属性还可以应用于其他资源
- 当用到这些资源的时候，浏览器会从缓存中取得，不再次发送请求了

- Here however, we will use a rel value of preload, which turns `<link>` into a preloader for any resource we want. You will also need to specify:
  - The path to the resource in the [`href`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#attr-href) attribute.
  - The type of resource in the [`as`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#attr-as) attribute.

```html
<link rel="preload" href="./index.js" as="script" />
<link rel="preload" href="./index.png" as="image" />
<link rel="preload" href="./index.mp4" as="video" type="video/mp4" />
```

## Tips

### `Promise` 错误处理

- Promise 有一个隐藏的“坑”，那就是内部的异常不能在外部通过 `try/catch` 所捕获，当内部发生异常时，会自动进入失败状态（`rejected`）。所以下面的代码是等价的：

```js
new Promise((resolve, reject) => {
  throw new Error(0); // 等价于  reject(new Error(0))
});
```

- 尽量使用 catch 子句而不是在 then 子句中捕获 Promise 异常，因为这样可以捕获 then 子句中的异常信息

```js
Promise.resolve(1).then(
  (data) => {
    const arr = data.split('');
    //...
  },
  (error) => {
    // 这里捕获不到
    // ...
  }
);
Promise.resolve(1)
  .then((data) => {
    const arr = data.split('');
    // ...
  })
  .catch((error) => {
    // 这里可以捕获
    // ...
  });
```

### `Promise` 局限性

1. 立即执行

   - 当一个 Promise 实例被创建时，内部的代码就会立即被执行，而且无法从外部停止。比如无法取消超时或消耗性能的异步调用，容易导致资源的浪费。

2. 单次执行
   - Promise 处理的问题都是“一次性”的，因为一个 Promise 实例只能 resolve 或 reject 一次，所以面对某些需要持续响应的场景时就会变得力不从心。比如上传文件获取进度时，默认采用的就是通过事件监听的方式来实现。
3. 解决方案
   - [RxJS](https://rxjs.dev/)

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[链接类型：preload - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Link_types/preload)

[HTML attribute: rel - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel)
