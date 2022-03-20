---
title: '毎日のフロントエンド  182'
date: 2022-03-20T14:04:20+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
  - Tips
---

## HTML

### `<meter>` 标签

> `<meter>`标签是 HTML 5 中的新标签

- 作用：定义已知范围或分数值内的标量测量。

```html
<p>显示度量值：</p>
<meter value="3" min="0" max="10">3/10</meter><br />
<meter value="0.6">60%</meter>
<p><b>注释：</b>Internet Explorer 不支持 meter 标签。</p>
```

## CSS

### 写一个 reset 的文件

- CSS reset，又叫做 CSS 重写或者 CSS 重置，用于改写 HTML 标签的默认样式, 尽量抹平个浏览器的样式差异
  - 进行样式重写时，不建议使用 `*` 选择器进行重写，这样会降低效率，影响性能

## JavaScript

### 为什么说 js 是弱类型语言

- **静态语言**：我们把在使用之前就需要确认其变量数据类型的称为静态语言。
- **动态语言**：我们把在运行过程中需要检查数据类型的语言称为动态语言。

---

- 通常把偷偷进行类型转换的操作成为隐式类型转换：

  - 支持因此类型转换的语言称为**弱类型语言**
  - 不支持隐式类型转换的语言称为**强类型语言**

---

- **编译型语言**：通常都会对源代码进行编译，生成可以执行的二进制代码，执行的是编译后的结果。（C/C++、Object-C、swift, rust）
- **解释型语言**:通常不用对源代码进行编译，一般是通过解释器载入脚本后运行。由于每个语句都是执行的时候才进行解释翻译，这样解释性语言每次执行就要翻译一次，效率相对要低。（JavaScript、Python、Erlang、PHP、Perl、Ruby）

---

- `Javascript`属于弱类型、动态、解释型语言

## Tips

### 让浏览器更快地加载网络资源

- 通过减少响应内容大小

  - 使用 gzip 算法压缩响应体内容
  - HTTP/2 的压缩头部功能
  - 另一种更通用也更为重要的技术就是使用**缓存**

- Web 缓存按存储位置来区分，包括**数据库缓存**、**服务端缓存**、**CDN 缓存**和**浏览器缓存**

#### HTTP 缓存

> 尽可能地让浏览器从缓存中获取资源，但同时又要保证被使用的缓存与服务端最新的资源保持一致。为了达到这个目的，需要制定合适的缓存过期策略（简称“缓存策略”），HTTP 支持的缓存策略有两种：强制缓存和协商缓存。

##### 强制缓存

- 强制缓存是在浏览器加载资源的时候，先直接从缓存中查找请求结果，如果不存在该缓存结果，则直接向服务端发起请求。

1. Expires
2. Cache-Control
   - `no-cache`，表示使用协商缓存，即每次使用缓存前必须向服务端确认缓存资源是否更新；
   - `no-store`，禁止浏览器以及所有中间缓存存储响应内容；
   - `public`，公有缓存，表示可以被代理服务器缓存，可以被多个用户共享；
   - `private`，私有缓存，不能被代理服务器缓存，不可以被多个用户共享；
   - `max-age`，以秒为单位的数值，表示缓存的有效时间；
   - `must-revalidate`，当缓存过期时，需要去服务端校验缓存的有效性。

- `cache-control: public, max-age=31536000` - 公有缓存，有效期 1 年

- cache-control 的 max-age 优先级高于 Expires，也就是说如果它们同时出现，浏览器会使用 max-age 的值。

- HTML5 规范中，并不支持这种方式(`<meta http-equiv="expires" content="Wed, 20 Jun 2021 22:33:00 GMT">`)，尽量不使用 meta 标签来设置缓存。

##### 协商缓存

> 协商缓存的更新策略是不再指定缓存的有效时间了，而是浏览器直接发送请求到服务端进行确认缓存是否更新，如果请求响应返回的 HTTP 状态为 `304`，则表示缓存仍然有效。控制缓存的难题就是从浏览器端转移到了服务端。

1. `Last-Modified` 和 `If-Modified-Since`

   - 通过响应头部字段 Last-Modified 和请求头部字段 If-Modified-Since **比对双方资源的修改时间**。
   - 具体工作流程如下：
     - 浏览器第一次请求资源，服务端在返回资源的**响应头中加入 `Last-Modified` 字段**，该字段表示这个资源在服务端上的最近修改时间；
     - 当浏览器再次向服务端请求该资源时，**请求头**部带上之前服务端返回的修改时间，这个请求头叫 If-Modified-Since；
     - 服务端再次收到请求，根据请求头 If-Modified-Since 的值，判断相关资源是否有变化，如果没有，则返回 304 Not Modified，并且不返回资源内容，浏览器使用资源缓存值；否则正常返回资源内容，且更新 Last-Modified 响应头内容。
   - 这种方式虽然能判断缓存是否失效，但也存在两个问题：
     - _精度问题_，Last-Modified 的时间精度为秒，如果在 1 秒内发生修改，那么缓存判断可能会失效；
     - _准度问题_，考虑这样一种情况，如果一个文件被修改，然后又被还原，内容并没有发生变化，在这种情况下，浏览器的缓存还可以继续使用，但因为修改时间发生变化，也会重新返回重复的内容。

2. `ETag` 和 `If-None-Match`
   - 不依赖于修改时间，而依赖于文件哈希值的精确判断缓存的方式，响应头部字段 ETag 和请求头部字段 If-None-Match。
   - 具体工作流程如下：
     - 浏览器第一次请求资源，服务端在返响应头中加入 Etag 字段，Etag 字段值为该资源的哈希值；
     - 当浏览器再次跟服务端请求这个资源时，在请求头上加上 If-None-Match，值为之前响应头部字段 ETag 的值；
     - 服务端再次收到请求，将请求头 If-None-Match 字段的值和响应资源的哈希值进行比对，如果两个值相同，则说明资源没有变化，返回 304 Not Modified；否则就正常返回资源内容，无论是否发生变化，都会将计算出的哈希值放入响应头部的 ETag 字段中。
     - **计算成本**。生成哈希值相对于读取文件修改时间而言是一个开销比较大的操作，尤其是对于大文件而言。如果要精确计算则需读取完整的文件内容，如果从性能方面考虑，只读取文件部分内容，又容易判断出错。
     - **计算误差**。HTTP 并没有规定哈希值的计算方法，所以不同服务端可能会采用不同的哈希值计算方式。这样带来的问题是，同一个资源，在两台服务端产生的 Etag 可能是不相同的，所以对于使用服务器集群来处理请求的网站来说，使用 Etag 的缓存命中率会有所降低。

> 强制缓存的优先级高于协商缓存，在协商缓存中，Etag 优先级比 Last-Modified 高。

#### ServiceWorker

> ServiceWorker 是浏览器在后台独立于网页运行的脚本，也可以这样理解，它是浏览器和服务端之间的代理服务器。

1. 使用限制
   - 在 ServiceWorker 中无法直接访问 DOM，但可以通过 postMessage 接口发送的消息来与其控制的页面进行通信；
   - ServiceWorker 只能在本地环境下或 HTTPS 网站中使用；
   - ServiceWorker 有作用域的限制，一个 ServiceWorker 脚本只能作用于当前路径及其子路径；
   - 由于 ServiceWorker 属于实验性功能，所以兼容性方面会存在一些问题，具体兼容情况请看下面的截图。
2. 使用方法
   - 在使用 ServiceWorker 脚本之前先要通过“注册”的方式加载它

```js
if ('serviceWorker' in window.navigator) {
  window.navigator.serviceWorker
    .register('./sw.js')
    .then(console.log)
    .catch(console.error)
} else {
  console.warn('不支持 ServiceWorker!')
```

- 首先考虑到浏览器的兼容性，判断 window.navigator 中是否存在 serviceWorker 属性，然后通过调用这个属性的 register 函数来告诉浏览器 ServiceWorker 脚本的路径。
- 浏览器获取到 ServiceWorker 脚本之后会进行解析，解析完成会进行安装。可以通过监听 “install” 事件来监听安装，但这个事件只会在第一次加载脚本的时候触发。要让脚本能够监听浏览器的网络请求，还需要激活脚本。
- 在脚本被激活之后，就可以通过监听 `fetch` 事件来拦截请求并加载缓存的资源了

```js
// 利用 ServiceWorker 内部的 caches 对象来缓存文件
const CACHE_NAME = 'ws'
let preloadUrls = ['/index.css']
self.addEventListener(‘install’, function (event) {
  event.waitUntil(
    caches.open(CACHE_NAME)
    .then(function (cache) {
      return cache.addAll(preloadUrls);
    })
  );
});
self.addEventListener(‘fetch’, function (event) {
  event.respondWith(
    caches.match(event.request)
    .then(function (response) {
      if (response) {
        return response;
      }
      return caches.open(CACHE_NAME).then(function (cache) {
          const path = event.request.url.replace(self.location.origin, ‘’)
          return cache.add(path)
        })
        .catch(e => console.error(e))
    })
  );
})
```

- 首先监听 install 事件，在回调函数中调用了 event.waitUntil() 函数并传入了一个 Promise 对象。event.waitUntil 用来监听多个异步操作，包括缓存打开和添加缓存路径。如果其中一个操作失败，则整个 ServiceWorker 启动失败。
- 然后监听了 fetch 事件，在回调函数内部调用了函数 event.respondWith() 并传入了一个 Promise 对象，当捕获到 fetch 请求时，会直接返回 event.respondWith 函数中 Promise 对象的结果。
- 在这个 Promise 对象中，我们通过 caches.match 来和当前请求对象进行匹配，如果匹配上则直接返回匹配的缓存结果，否则返回该请求结果并缓存。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<meter>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meter)

[CSS Tools: Reset CSS](https://meyerweb.com/eric/tools/css/reset/)
