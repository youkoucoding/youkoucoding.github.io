---
title: '毎日のフロントエンド  163'
date: 2022-02-26T10:35:29+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `xpath`和`dom`区别

- `xpath`是一门在 XML 文档中查找信息的语言
- `DOM`是文档对象类型。
  - W3C 文档对象模型 （DOM） 是中立于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。
    - 核心 DOM - 针对任何结构化文档的标准模型
    - XML DOM - 针对 XML 文档的标准模型
    - HTML DOM - 针对 HTML 文档的标准模型

## JavaScript

### 创建一个 `worker` 线程

- webworker 基本流程，新建一个 worker，然后 postMessage 来传递数据，onmessage 接收数据并执行函数

```js
const worker = new Worker('a.js');
worker.postMessage('Hello World');
worker.onmessage = function (e) {
  console.log(e.data);
};
```

## Tips

- 在做响应式网站的话，优先做移动端的网页，再去适配桌面端，这样移动端用的代码量少，有助于提高性能

- `async` 属性。立即请求文件，但不阻塞渲染引擎，而是文件加载完毕后阻塞渲染引擎并立即执行文件内容。
- `defer` 属性。立即请求文件，但不阻塞渲染引擎，等到解析完 HTML 之后再执行文件内容。
- `HTML5` 标准 type 属性，对应值为“module”。让浏览器按照 ECMA Script 6 标准将文件当作模块进行解析，默认阻塞效果同 defer，也可以配合 async 在请求完成后立即执行。

- `link` 标签：通过预处理提升渲染速度
  - `dns-prefetch`。当 link 标签的 rel 属性值为“dns-prefetch”时，浏览器会对某个域名预先进行 DNS 解析并缓存。这样，当浏览器在请求同域名资源的时候，能省去从域名查询 IP 的过程，从而减少时间损耗
  - `preconnect`。让浏览器在一个 HTTP 请求正式发给服务器前预先执行一些操作，这包括 DNS 解析、TLS 协商、TCP 握手，通过消除往返延迟来为用户节省时间
  - `prefetch/preload`。两个值都是让浏览器预先下载并缓存某个资源，但不同的是，prefetch 可能会在浏览器忙时被忽略，而 preload 则是一定会被预先下载
  - `prerender`。浏览器不仅会加载资源，还会解析执行页面，进行预渲染

![`<link>`标签的性能优化](link-preformance.png)

- 搜索优化
  - `meta` 标签：提取关键信息
    - [The Open Graph protocol](https://ogp.me/)
    - `content`
  - `link` 标签：减少重复:`rel="canonical"`

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Worker() - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Worker/Worker)

[Web Worker 使用教程 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2018/07/web-worker.html)

[dns-prefetch - Web 性能 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/Performance/dns-prefetch)
