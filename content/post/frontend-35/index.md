---
title: '毎日のフロントエンド　35'
date: 2021-10-21T10:36:22+09:00
description: frontend 每日一练
image: frontend-35-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第三十五日

## HTML

### **Question** 用一个 `div` 模拟 `textarea` 的实现

`<div class="edit" contenteditable="true" style="resize: both"></div>`

## CSS

### **Question** 使用 flex 实现三栏布局，两边固定，中间自适应

```css
.container {
  display: flex;
  height: 100px;
  .left,
  .right {
    width: 100px;
    background: #8c939d;
  }
  .content {
    flex: 1;
    background: #306eff;
  }
}
```

`flex: 0 1 auto` 分别表示什么意思

三个参数分别对应的是 `flex-grow`, `flex-shrink` 和 `flex-basis`，默认值分别是`0 1 auto`

1. `flex-grow`属性定义项目的放大比例，默认为 0，即如果存在剩余空间，也不放大
2. `flex-shrink`属性定义了项目的缩小比例，默认为 1，即如果空间不足，该项目将缩小
3. `flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）

## JavaScript

### **Question** 解释一个为什么 `10.toFixed(10)` 会报错

因为在这里的 `.` 发生了歧义，它既可以理解为小数点，也可以理解为对方法的调用
因为这个点紧跟于一个数字之后，按照规范，解释器就把它判断为一个小数点

- `(10).toFixed(10)`
- `10..toFixed(10)`
- `10 .toFixed(10)`
- `10.0.toFixed(10)`

出现这个报错是因为前面这个数是整数，如果本来就是小数就不会出现这个报错

### **Question** 写一个 `mySetInterVal(fn, a, b)` ,每次间隔 `a`,`a+b`,`a+2b`, `...`,`a+nb` 的时间，然后写一个 `myClear`，停止上面的 `mySetInterVal`

```js
function mySetInterVal(fn, a, b) {
  this.a = a;
  this.b = b;
  this.time = 0;
  this.handle = null;

  this.start = () => {
    this.handle = setTimeout(() => {
      fn();
      this.time++;
      this.start();
      console.log(this.a + this.b * this.time);
    }, this.a + this.b * this.time);
  };

  this.stop = () => {
    clearTimeout(this.handle);
    this.time = 0;
  };
}

const demo = new mySetInterVal(
  () => {
    console.log('demo');
  },
  100,
  200
);
demo.start();
demo.stop();
```

## Soft Skill

### **Question** 谈一谈知道的前端性能优化方案有哪些

客户端优化

- 减少`http`请求次数：`CSS Sprites`, `JS`、`CSS`源码压缩、图片大小控制合适；网页`Gzip`，`CDN`托管，`data`缓存 ，图片服务器
- 使用`CSS Sprites`: 将多个图片合并到一张单独的图片，这样就大大减少了页面中图片的`HTTP`请求
- 减少`DOM`操作次数，优化`javascript`性能
- 少用全局变量、减少 DOM 操作、缓存 DOM 节点查找的结果。减少 IO 读取操作
- 延迟加载 | 延迟渲染
- 图片预加载，将样式表放在顶部，将脚本放在底部 加上时间戳
- 避免在页面的主体布局中使用 `table`，`table` 要等其中的内容完全下载之后才会显示出来，**显示比 `div+css` 布局慢**
- `http`缓存 设置`cache-control expires Last-modified`
- 前端缓存 对于一些页面今天配置直接存储到`localStorage`中
- 对于长期不发生改变的代码可以直接通过`server-work`存储到本地

> 优化加载

1. `webpack` 开启 `tree-shaking` 减少代码体积
2. 通过 `preload` `prefetch` 优化加载资源的时间
3. `import('').then()`异步加载资源
4. 图片小于 `30k` 的图片直接做成 `base64`；
5. 对于首屏的样式可以直接内嵌到 `html` 中；

服务端优化

- 尽量减少响应的体积，比如用 `gzip` 压缩(`nignx`)，优化图片字节数，压缩 `css` 和 `js`；或加快文件读取速度，优化服务端的缓存策略。
- 客户端优化 `dom`、`css` 和 `js` 的代码和加载顺序；或进行**服务器端渲染**，减轻客户端渲染的压力。
- 优化网络路由，比如增加 `CDN 缓存`；或增加并发处理能力，比如服务端设置多个域名，客户端使用多个域名同时请求资源，增加并发量。

Summary

**尽量向前端优化、减少数据库操作、减少磁盘`IO`**:
**向前端优化**指的是，在不影响功能和体验的情况下，能在浏览器执行的不要在服务端执行，能在缓存服务器上直接返回的不要到应用服务器，程序能直接取得的结果不要到外部取得，本机内能取得的数据不要到远程取，内存能取到的不要到磁盘取，缓存中有的不要去数据库查询。
**减少数据库操作**指减少更新次数、缓存结果减少查询次数、将数据库执行的操作尽可能的让你的程序完成（例如 join 查询），减少磁盘 IO 指尽量不使用文件系统作为缓存、减少读写文件次数等。程序优化永远要优化慢的部分，换语言是无法“优化”的。

## Reference

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)

[前端面试 - 大厂前端面试真题](https://lgwebdream.github.io/FE-Interview/)

[『前端面试 100 问』之弹性盒子中 flex: 0 1 auto 表示什么意思 - 掘金](https://juejin.cn/post/6844904182156115982)
