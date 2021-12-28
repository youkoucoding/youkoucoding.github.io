---
title: '毎日のフロントエンド  103'
date: 2021-12-28T21:04:50+09:00
description: frontend 每日一练
image: frontend-103-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百零三日

## HTML

### **Question:** 描述`cookies`、`sessionStorage`和`localStorage`的区别

- `Cookies`
  - 网站为了对用户身份做标识而存储在本地的数据。在同源的 http 请求中会被一同发送，因此会占用网络带宽。最大大小 4KB。以前每个域名支持 20 个，新版本的增加到 50 个。生命周期为设置的过期时间，若不设置则到关闭浏览器为止。
- `localStorage`
  - HTML5 新增的，不会将数据自动发送到服务器，仅存储在本地，并且会永久存储，除非用户主动删除，不然不会过期或者自动清除。
- `sessionStorage`
  - 有效期限为一次 session 会话（即一个 tab 页从打开到关闭之间的时间段）。webStorage 在不同浏览器的最大大小不同，因此一般以 5M 为上限较为安全。

## CSS

### **Question:** 低版本 IE 的盒子模型

- CSS 盒子由四部分组成，由内到外依次是：`content`、`padding`、`border`、`margin`
- 所谓盒子模型定义的是盒子宽高的计算方法:

  - `IE`盒子模型的宽高为 content、padding、border 之和
  - W3C 盒子的宽高仅为 content

- 从页面布局来说**IE 盒子模型**实际上更实用一点，这也是后来 W3C 提供 box-sizing 属性用于切换盒子模型的原因。`box-sizing:border-box;`

## JavaScript

### **Question:** 如何更好地处理`Async/Await`的异常的

对 promise 对象进行一层包装，通过返回值判断是否有异常

```js
// 对Promise对象进行一层包装，将异常值和成功结果一起返回
function wrapper(promise) {
  return promise.then((res) => [null, res]).catch((err) => [err, null]);
}

function sleep(t) {
  return new Promise((resolve, reject) => {
    if (t < 1000) {
      reject('123');
    } else {
      setTimeout(() => {
        resolve();
      }, t);
    }
  });
}

async function delay() {
  let [err, res] = await wrapper(sleep(100));
  if (err) {
    console.log(`error: ${err}`);
  }
}

delay();
```

### **Question:** 列举出多种减少页面加载时间的方法

- 缓存利用： 缓存 Ajax，使用 CDN、外部 JavaScript 和 css 文件缓存，添加 Expires 头，在服务器端配置 Etag，减少 DNS 查找等
- 请求数量．合并样式和脚本，使用 css 图片精灵，初始首屏之外的图片资源按需加载，静态资源延迟加载
- 请求带宽：压缩文件，开启 GZIP
- css 代码：避免使用 css 表达式、高级选择器、通配选择器
- js 代码：用散列表来优化查找，少用全局变量，用 innerHTML 代替 DOM 操作，减少 DOM 操作次数，优化 JavaScript 性能，用 setTimeout 避免页面失去响应，缓 存 DOM 节点查找的结果，避免使用 with (with 会创建自己的作用域， 增加作用域链的 长度），多个变量声明合并
- HTML 代码：**避免图片和 iFrame 等 src 属性为空**, src 属性为空，会重新加载当前页面
- 尽量避免 js 操作 dom, 避免 js 造成的 reflow 和闪烁

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[async/await 优雅的错误处理方法 - 掘金](https://juejin.cn/post/6844903767129718791)

[How to write async await without try-catch blocks in Javascript](https://blog.grossman.io/how-to-write-async-await-without-try-catch-blocks-in-javascript/)
