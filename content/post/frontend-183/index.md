---
title: '毎日のフロントエンド  183'
date: 2022-03-21T15:15:07+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### `<output>` 标签

- `<output>` 标签表示计算或用户操作的结果。
- 属性
  - `for`其它影响计算结果的标签的 ID，可以多个。
  - `form`与当前标签有关联的 form（从属的表单）。该属性的值必须是当前文档内的表单元素的 ID。如果未指明该属性，output 标签必须是一个 form 的后代标签。该属性的用处在于可以让 output 标签脱离 form 标签，存在于一个网页文档的任意位置。
  - `name` 属性

```html
<form oninput="result.value=parseInt(a.value)+parseInt(b.value)">
  <input type="range" name="b" value="50" /> + <input type="number" name="a" value="10" /> =
  <output name="result"></output>
</form>
```

## CSS

### 去除图片自带的边距

- 图片以及所有其他 `inline-block` 元素之间都会有 4px 的间距，直接在父元素 `font-size: 0` 去除。

## Tips

### 函数返回多个值方法

- 函数 return expression, 这个表达式应该是一个具体的值,这个具体的值可以是一个承载多个值的复杂值, 诸如 Array,Object, Map,Set 等方式来实现

### `AMD`、`CMD`和`CommonJS`

- AMD,CMD 和 CommonJs 是 es6 之前推出的模块化方案

  1. CommonJs 用在服务端
  2. AMD,CMD 用在浏览器环境

- CommonJs 是由 node 推广使用的

  - 导出 `module.exports`，导入 `require`

- AMD 是 RequireJS 在推广过程中对模块定义的规范化
  - 异步加载模块，模块的加载不影响后面语句的执行
  - 所有依赖这些模块的语句，都定义到一个回调函数中，等依赖模块全部加载完成后，执行回调函数

```js
define(['回调函数的依赖模块'], function() {
    // 回调函数，依赖模块的语句
    ...
})

define(['./a', './b'], function(a, b) { // 依赖刚开始就要去加载，加载依赖是异步加载
  // a b 依赖加载完成后，才去执行回调中语句
  a.doSomething()
  b.doSomething()
  ...
})
```

- CMD 是`SeaJS`在推广过程中对模块定义的规范化
  - 采用就近依赖，同步加载。即在用到依赖的地方，才去加载依赖。

```js
define(function (require, exports, module) {
  var a = require('./a'); // 依赖等用的时候才去加载，加载依赖是同步加载
  a.doSomething();

  var b = require('./b');
  b.doSomething();
  // ...
});
```

### 浏览器的同源策略（Same Origin Policy）

- 源（Origin）是由 URL 中**协议**、**主机名（域名 domain）** 以及 **端口**共同组成的部分。

> 同源策略在保障安全的同时也带来了不少问题，比如 iframe 中的子页面与父页面无法通信，浏览器与其他服务端无法交互数据。所以我们需要一些跨域方案来解决这些问题。

#### 请求跨域解决方案

##### 1. 跨域资源共享

- 跨域资源共享（`CORS`，Cross-Origin Resource Sharing）是浏览器为 AJAX 请求设置的一种跨域机制，让其可以在服务端允许的情况下进行跨域访问。
- 主要通过 HTTP 响应头来告诉浏览器服务端是否允许当前域的脚本进行跨域访问。

---

- 跨域资源共享将 AJAX 请求分成了两类：**简单请求**和**非简单请求**

  - 其中简单请求符合下面 2 个特征：
    - 请求方法为 `GET`、`POST`、`HEAD`
    - **请求头只能使用**下面的字段：`Accept`（浏览器能够接受的响应内容类型）、`Accept-Language`（浏览器能够接受的自然语言列表）、`Content-Type` （请求对应的类型，只限于 text/plain、multipart/form-data、application/x-www-form-urlencoded）、`Content-Language`（浏览器希望采用的自然语言）、`Save-Data`（浏览器是否希望减少数据传输量）
  - **任意一条要求不符合的即为非简单请求**

- 简单请求，处理流程:

  - 浏览器发出简单请求的时候，会在请求头部增加一个 Origin 字段，对应的值为当前请求的源信息；
  - 当服务端收到请求后，会根据请求头字段 Origin 做出判断后返回相应的内容
  - 浏览器收到响应报文后会根据响应头部字段 `Access-Control-Allow-Origin` 进行判断，这个字段值为服务端允许跨域请求的源，其中通配符“\*”表示允许所有跨域请求。如果头部信息没有包含 `Access-Control-Allow-Origin` 字段或者响应的头部字段 `Access-Control-Allow-Origin` 不允许当前源的请求，则会抛出错误。

- 当处理非简单的请求时，浏览器会先发出一个预检请求（`Preflight`）。
  - 这个预检请求为 OPTIONS 方法，并会添加了 1 个请求头部字段 Access-Control-Request-Method，值为跨域请求所使用的请求方法。
  - 在服务端收到预检请求后，除了在响应头部添加 `Access-Control-Allow-Origin` 字段之外，至少还会添加 `Access-Control-Allow-Methods` 字段来告诉浏览器服务端允许的请求方法，并返回 204(`No Content`) 状态码。
  - 浏览器得到预检请求响应的头部字段之后，会判断当前请求服务端是否在服务端许可范围之内，如果在则继续发送跨域请求，反之则直接报错。

##### 2. `JSONP`

> `JSONP`（JSON with Padding）的大概意思就是用 JSON 数据来填充， 把 JSON 数据填充到一个回调函数中，依赖的是 **script 标签跨域引用 js 文件不会受到浏览器同源策略的限制**。

具体实现方式

1. 全局声明一个用来处理返回值的函数 fn，该函数参数为请求的返回结果。

```js
function fn(result) {
  console.log(result);
}
```

2. 将函数名与其他参数一并写入 URL 中

```js
var url = 'http://www.b.com?callback=fn&params=...';
```

3. 创建一个 script 标签，把 URL 赋值给 script 的 src

```js
var script = document.createElement('script');
script.setAttribute('type', 'text/javascript');
script.src = url;
document.body.appendChild(script);
```

4. 当服务器接收到请求后，解析 URL 参数并进行对应的逻辑处理，得到结果后将其写成回调函数的形式并返回给浏览器

```js
fn({
  list: [],
  ...
})
```

5. 在浏览器收到请求返回的 js 脚本之后会立即执行文件内容，即在控制台打印传入的数据内容

- JSONP 问题：
  - 只能发送 GET 请求，限制了参数大小和类型；
  - 请求过程无法终止，导致弱网络下处理超时请求比较麻烦；
  - 无法捕获服务端返回的异常信息。

##### 3. `Websocket`

> Websocket 是 HTML5 规范提出的一个应用层的全双工协议，适用于浏览器与服务器进行实时通信场景。

```js
// 在 a 网站直接创建一个 WebSocket 连接，连接到 b 网站即可
// 然后调用 WebScoket 实例 ws 的 send() 函数向服务端发送消息
// 监听实例 ws 的 onmessage 事件得到响应内容。
var ws = new WebSocket('ws://b.com');
ws.onopen = function () {
  // ws.send(...);
};
ws.onmessage = function (e) {
  // console.log(e.data);
};
```

##### 4. 代理转发

- 跨域是为了突破浏览器的同源策略限制，既然同源策略只存在于浏览器，在服务端进行跨域，比如设置代理转发。这种在服务端设置的代理称为“**反向代理**”，用户是无感知的。

- 另一种在客户端使用的代理称为“**正向代理**”，主要用来代理客户端发送请求，用户使用时必须配置代理服务器的网址，比如常用的 VPN 工具就属于正向代理。

  - 当浏览器发起前缀为 /api 的请求时都会被转发到 http://localhost:3000 这个网址，然后将响应结果返回给浏览器。对于浏览器而言还是请求当前网站，但实际上已经被服务端转发。

```js
// webpack.config.js
// webpack-dev-server 配置代理的示例代码
module.exports = {
  //...
  devServer: {
    proxy: {
      '/api': 'http://localhost:3000',
    },
  },
};
```

or

```js
// 在 Nginx 服务器上配置同样的转发规则
// 通过 location 指令匹配路径，然后通过 proxy_pass 指令指向代理地址即可
location /api {
    proxy_pass   http://localhost:3000;
}
```

#### 页面跨域解决方案

> 除了浏览器请求跨域之外，页面之间也会有跨域需求，例如使用 iframe 时父子页面之间进行通信

##### 1. `postMessage`

- HTML5 新函数 `postMessage()` 用来实现父子页面之间通信，而且不论这两个页面是否同源

- 父页面向子页面发消息

```js
// https://example.com
var child = window.open('https://child.example.com');
child.postMessage('hi', 'https://child.example.com');
```

- `window.open()` 函数打开了子页面，然后调用 `child.postMessage()` 函数发送了字符串数据“hi”给子页面

- 在子页面中，只需要监听“message”事件即可得到父页面的数据

```js
// https://child.example.com
window.addEventListener(
  'message',
  function (e) {
    console.log(e.data);
  },
  false
);
```

- 父页面也可以监听“message”事件来接收子页面发送的数据。子页面发送数据时则要通过 window.opener 对象来调用 postMessage() 函数

```js
// https://child.example.com
window.opener.postMessage('hello', 'https://example.com');
```

##### 2. 改域

- 对于主域名相同，子域名不同的情况，可以通过修改 `document.domain` 的值来进行跨域。如果将其设置为其当前域的父域，则这个较短的父域将用于后续源检查。
  - 只能把 document.domain 设置成更高级的父域才有效果

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<output>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/output)

[RxJS - Introduction](https://rxjs.dev/guide/overview)

[RequireJS](https://requirejs.org/)

[seajs/seajs: A Module Loader for the Web](https://github.com/seajs/seajs)

[window.postMessage - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)

[Document.domain - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/domain)
