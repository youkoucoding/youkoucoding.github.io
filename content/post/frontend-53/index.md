---
title: '毎日のフロントエンド　53'
date: 2021-11-08T19:38:24+09:00
description: frontend 每日一练
image: frontend-53-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十三日

## HTML

### **Question:** `web workers` 能帮我们解决哪些问题

`Web Worker` 的作用，就是为 `JavaScript` 创造多线程环境，允许主线程创建 `Worker` 线程，将一些任务分配给后者运行。在主线程运行的同时，`Worker` 线程在后台运行，两者互不干扰。等到 `Worker` 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算密集型或高延迟的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢。

Web Worker 有以下几个使用注意点:

1. 同源限制: 必须与主线程的脚本文件同源。

2. `DOM` 限制

   - Worker 线程所在的全局对象，与主线程不一样，无法读取主线程所在网页的 DOM 对象，也无法使用`document`、`window`、`parent`这些对象。但是，`Worker` 线程可以 `navigator` 对象和 `location` 对象。

3. 通信联系

   - Worker 线程和主线程不在同一个上下文环境，它们不能直接通信，必须通过消息完成。

4. 脚本限制

   - `Worker` 线程不能执行`alert()`方法和`confirm()`方法，但可以使用 `XMLHttpRequest` 对象发出 `AJAX` 请求。

5. 文件限制

   - `Worker` 线程无法读取本地文件，即不能打开本机的文件系统`file://`，它所加载的脚本，必须来自网络。

## CSS

### **Question:** 怎么使用自定义字体？有什么注意事项

```css
@font-face {
  font-family: '自定义字体名称';
  src: url('字体文件名.eot'); /* Modern Browsers url('字体文件名.ttf') format('truetype'), / Safari, Android, iOS / url('字体文件名.svg#字体文件名') format('svg'); / Legacy iOS */
  font-style: normal;
  font-weight: normal;
}
```

## JavaScript

### **Question:** `document`的 `load` 和 `ready` 有什么区别

`Dom`文档执行顺序：

1. 解析 HTML 结构
2. 加载外部脚本和样式表文件
3. 解析并执行脚本代码
4. 构建 `html` `dom` 模型 // `ready` 页面资源加载完成
5. 加载图片等外部文件
6. 页面加载完毕 // `load` `dom`加载完成

## Reference

[Web Worker 使用教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2018/07/web-worker.html)
