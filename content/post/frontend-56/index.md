---
title: '毎日のフロントエンド　56'
date: 2021-11-11T20:17:32+09:00
description: frontend 每日一练
image: frontend-56-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十六日

## HTML

### **Question:** 渐进式渲染是什么`progressive rendering`

渐进式渲染是用来提高网页性能，以尽快呈现页面的技术。
比如：

- 图片懒加载——页面上的图片不会一次性全部加载。当用户滚动页面到图片部分时，JavaScript 将加载并显示图像。
- 确定显示内容的优先级（分层次渲染）——为了尽快将页面呈现给用户，页面只包含基本的最少量的 CSS、脚本和内容，然后可以使用延迟加载脚本或监听 `DOMContentLoaded/load` 事件加载其他资源和内容。
- 异步加载 `HTML` 片段——当页面通过后台渲染时，把 `HTML` 拆分，通过异步请求，分块发送给浏览器。

## CSS

### **Question:** `margin`和`padding`使用的场景有哪些

`margin`是用来隔开元素与元素的间距

`padding`是用来隔开元素与内容的间隔

`margin`用于布局分开元素使元素与元素互不相干

`padding`用于元素与内容之间的间隔，让内容（文字）与（包裹）元素之间有一段距离

## JavaScript

### **Question:** `JSONP`的原理是什么？解决什么问题

#### `JSONP` 是什么

`JSONP` 是一种动态 `script` 标签跨域请求技术。
指的是请求方动态创建 `script` 标签，`src`指向响应方的服务器（非同源的 url），同时传一个参数`callback`，
`callback` 后面是`functionName`，当请求方发起请求时，响应方根据传过来的参数`callback`,构造并调用：xxx.call(undefined,'数据'),其中 _数据_ 的传入格式是以`JSON`格式传入的，因为传入的 JSON 数据具有左右 `padding`,因而得名 `JSONP` 。
后端代码构造并调用了 xxx，浏览器接收到了响应，就会执行 xxx.call(undefined,'数据'),于是，请求方就知道了要的数据了。

#### `JSONP` Usage

```js
//server.js
const Koa = require('koa');
const bodyParser = require('koa-bodyparser');
const { getUser } = require('./mock');
const app = new Koa();

app.use(bodyParser());

app.use(async (ctx, next) => {
  const { path: curPath } = ctx.request;
  if (curPath === '/jsonp') {
    // 设置响应头
    ctx.set('Content-Type', 'application/javascript;charset=utf-8');
    ctx.set('X-Content-Type-Options', 'nosniff');
    const callback = ctx.query.callback;
    let data = getUser(ctx.query.type);
    data = JSON.stringify(data);
    ctx.body = `${callback}(${data})`;
    console.log(ctx.query);
  }
});

console.log('服务器已启动！');
app.listen(3001);
```

```js
// client.js
const btn: HTMLElement = document.getElementById('btn');
btn.addEventListener(
  'click',
  () => {
    let url = 'http://127.0.0.1:3030/jsonp?type=all&callback=getdata';
    loadScript(url);
  },
  false
);

function loadScript(src) {
  const script: HTMLScriptElement = document.createElement('script');
  script.src = src;
  script.onload = () => {
    // 每次动态创建script标签之后,都将script标签删掉
    document.body.removeChild(script);
  };
  script.onerror = () => {
    console.error('请求失败了');
    delete window['getdata'];
    document.body.removeChild(script);
  };
  document.body.appendChild(script);
}

function getdata(data: any): void {
  // data 为服务端返回的数据
  // to do something
  alert(JSON.stringify(data));
}
```

#### `JSONP`优缺点

优点：

1. JSONP 可以跨源
2. 兼容性更好

缺点：

1. 只支持 `GET` 请求
2. `jsonp` 在调用失败的时候不会返回 HTTP 状态码
3. 只支持跨域 `HTTP` 请求这种情况，不能解决不同域的两个页面之间如何进行 JavaScript 调用的问题。
4. 安全性不佳：假如提供 `jsonp` 的服务存在页面注入漏洞，即它返回的 `javascript` 的内容被人控制的,那么所有调用这个 `jsonp` 的网站都会存在漏洞,这样的话危险就不止在一个域名下。

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[Async Fragments: Rediscovering Progressive HTML Rendering with Marko](https://tech.ebayinc.com/engineering/async-fragments-rediscovering-progressive-html-rendering-with-marko/)
