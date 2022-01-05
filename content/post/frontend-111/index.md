---
title: '毎日のフロントエンド  111'
date: 2022-01-05T09:11:05+09:00
description: frontend 每日一练
image: frontend-111-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百一十一日

## HTML

### **Question:** HTML 如何创建分区响应图

- 分区响应图:一张图片,分成多个模块,点击模块可以链接到不同的 URL 地址
- 实现: `map`, `area`

```html
<p>
  <img src="./1.png" usemap="#myMap" />
</p>
<map name="myMap">
  <area href="http://baidu.com" shape="rect" coords="50,106,220,273" />
  <area href="http://google.com" shape="rect" coords="260,106,430,275" />
  <area href="http://juejin.im" shape="default" />
</map>
```

## CSS

### **Question:** table 布局

- 在 div + css 布局成为主流之前，基本都是以 table 布局为主
- table 布局对于排版比较友好，水平居中、垂直居中都可以利用 table 的属性来完成。
- 缺点也是十分明显
  - `table` 布局往往是 table 嵌套 table，会有非常多的 DOM 节点，性能糟糕
  - 语义化不明，本身就是标签错误的用法。因此对 SEO 不友好
  - DOM 操作困难，tr、td 中要寻找到目标 DOM 元素非常困难，代码没有维护性

## JavaScript

### **Question:** 用 js 编写一个红绿灯程序

```js
function sleep(t) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve();
    }, t);
  });
}

/**
 * 循环显示红绿灯
 * @param {number} green 绿灯显示毫秒数
 * @param {number} yellow 黄灯显示毫秒数
 * @param {number} red 红灯显示毫秒数
 */
async function light(green = 15000, yellow = 3000, red = 10000) {
  let status = 'green';
  while (true) {
    await sleep(green).then(() => {
      status = 'yellow';
      console.log(status);
    });
    await sleep(yellow).then(() => {
      status = 'red';
      console.log(status);
    });
    await sleep(red).then(() => {
      status = 'green';
      console.log(status);
    });
  }
}

light(3000, 1000, 1000);
```

### **Question:** 浏览器缓存读取规则

浏览器缓存可以在网络请求、浏览器响应中优化性能。

#### 缓存位置

从缓存位置上来说分为四种，并且各自有优先级，当依次查找缓存且都没有命中的时候，才会去请求网络。

- `Service Worker`
- `Memory Cache`
- `Disk Cache`
- `Push Cache`

##### `Service Worker`

Service Worker 是运行在浏览器背后的独立线程，一般可以用来实现缓存功能。使用 Service Worker 的话，传输协议必须为 `HTTPS`。因为 Service Worker 中涉及到请求拦截，所以必须使用 HTTPS 协议来保障安全。

- Service Worker 的缓存与浏览器其他内建的缓存机制不同，它可以让我们自由控制缓存哪些文件、如何匹配缓存、如何读取缓存，并且缓存是持续性的。

Service Worker 实现缓存功能一般分为三个步骤：首先需要先注册 Service Worker，然后监听到 install 事件以后就可以缓存需要的文件，那么在下次用户访问的时候就可以通过拦截请求的方式查询是否存在缓存，存在缓存的话就可以直接读取缓存文件，否则就去请求数据。

当 Service Worker 没有命中缓存的时候，我们需要去调用 fetch 函数获取数据。也就是说，如果我们没有在 Service Worker 命中缓存的话，会根据缓存查找优先级去查找数据。但是不管我们是从 Memory Cache 中还是从网络请求中获取的数据，浏览器都会显示我们是从 Service Worker 中获取的内容。

##### `Memory Cache`

Memory Cache 也就是内存中的缓存，主要包含的是当前中页面中已经抓取到的资源,例如页面上已经下载的样式、脚本、图片等。读取内存中的数据肯定比磁盘快,内存缓存虽然读取高效，可是缓存持续性很短，会随着进程的释放而释放。 **关闭 Tab 页面，内存中的缓存就被释放了。**

内存缓存中有一块重要的缓存资源是 preloader 相关指令（例如`<link rel="prefetch">`）下载的资源。可以一边解析 js/css 文件，一边网络请求下一个资源。

需要注意的事情是，内存缓存在缓存资源时并不关心返回资源的 HTTP 缓存头 Cache-Control 是什么值，同时资源的匹配也并非仅仅是对 URL 做匹配，还可能会对 Content-Type，CORS 等其他特征做校验。

##### `Disk Cache`

Disk Cache 也就是存储在硬盘中的缓存，读取速度慢点，但是什么都能存储到磁盘中，比之 Memory Cache 胜在容量和存储时效性上。

在所有浏览器缓存中，Disk Cache 覆盖面基本是最大的。它会根据 HTTP Herder 中的字段判断哪些资源需要缓存，哪些资源可以不请求直接使用，哪些资源已经过期需要重新请求。并且即使在跨站点的情况下，相同地址的资源一旦被硬盘缓存下来，就不会再次去请求数据。绝大部分的缓存都来自 Disk Cache

浏览器会把哪些文件进入内存中？哪些进入硬盘中：

- 对于大文件来说，大概率是不存储在内存中的，反之优先
- 当前系统内存使用率高的话，文件优先存储进硬盘

##### `Push Cache`

Push Cache（推送缓存）是 HTTP/2 中的内容，当以上三种缓存都没有命中时，它才会被使用。它只在会话（Session）中存在，一旦会话结束就被释放，并且缓存时间也很短暂，在 Chrome 浏览器中只有 5 分钟左右，同时它也并非严格执行 HTTP 头中的缓存指令。

#### 缓存过程分析

浏览器与服务器通信的方式为应答模式，即是：浏览器发起 HTTP 请求 – 服务器响应该请求，那么浏览器怎么确定一个资源该不该缓存，如何去缓存呢？浏览器第一次向服务器发起该请求后拿到请求结果后，将请求结果和缓存标识存入浏览器缓存，浏览器对于缓存的处理是根据第一次请求资源时返回的响应头来确定的。

![](http-cache.png)

- 浏览器每次发起请求，都会先在浏览器缓存中查找该请求的结果以及缓存标识

- 浏览器每次拿到返回的请求结果都会将该结果和缓存标识存入浏览器缓存中

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[<map>: The Image Map element - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/map)

[一文读懂前端缓存](https://juejin.cn/post/6844903747357769742?utm_source=gold_browser_extension)

[深入理解浏览器的缓存机制](https://www.jianshu.com/p/54cc04190252)
