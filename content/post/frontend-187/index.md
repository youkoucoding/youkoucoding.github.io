---
title: '毎日のフロントエンド 187'
date: 2022-03-25T14:19:54+09:00
description: frontend 每日一练
categories:
  - JavaScript
  - Tips
---

## JavaScript

### `FileReader`

- FileReader 对象允许 Web 应用程序**异步读取**存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据

## Tips

### 前端性能指标

#### 首屏绘制（First Paint，FP）

- 首屏绘制时间是指*从开始加载到浏览器首次绘制像素到屏幕上的时间*，也就是页面在屏幕上首次发生视觉变化的时间。
  - 首屏绘制不包括默认的背景绘制，但包括非默认的背景绘制。由于首次绘制之前网页呈现默认背景白色，所以也俗称“白屏时间”。
- 在 HTML5 下可以通过 performance API 来获取首屏绘制时间

```js
performance.getEntriesByType('paint')[0];
/* 
{ 
  duration: 0, 
  entryType: "paint", 
  name: "first-paint", 
  startTime: 197.58499998715706, 
} 
*/
```

- 通过 `performance.getEntriesByType()` 函数返回了一个 `PerformanceEntry` 实例组成的数组
  - `duration` 为该事件的耗时
  - `entryType` 为性能指标实例的类型
  - `name` 为指标名称
  - `startTime` 为指标采集时间。

#### 首屏内容绘制（First Contentful Paint，FCP）

- 浏览器首次绘制来自 DOM 的内容时间，这个内容可以是文字、图片（也包括背景图片）、非空白的 canvas 和 svg。

```js
performance.getEntriesByType('paint')[1];
/*
{ 
  duration: 0, 
  entryType: "paint", 
  name: "first-contentful-paint", 
  startTime: 797.8649999859044 
} 
*/
```

- 和获取 FP 值的唯一区别就在于通过 `performance.getEntriesByType()` 函数获取到 `PerformanceEntry` 实例数组的下标值不一样，FP 为第 1 个元素，FCP 为第 2 个元素。

---

- FCP 有时候会和 FP 时间相同，也可能晚于 FP。
  - FP 只需要满足“开始绘制”这一个条件就可以了，而 FCP 还要满足第二个条件，那就是“绘制的像素有内容”

#### 可交互时间（Time to Interactive，TTI）

- 可交互时间由[Web Incubator Community Group (WICG)](https://wicg.io/)提出，是指网页在视觉上都已渲染出了，浏览器可以响应用户的操作了。两个条件：

  - 第一个条件是主线程的长任务（长任务是指耗时超过 50 ms）执行完成后
  - 第二个条件是随后网络静默时间达到 5 秒，这里的静默时间是指请求数不超过 2 个， 排除失败的资源请求和未使用 GET 方法进行的网络请求

- TTI 测量可以使用 Google 提供的模块 [tti-polyfill - npm](https://www.npmjs.com/package/tti-polyfill)，示例代码如下：

```js
import ttiPolyfill from 'tti-polyfill';
ttiPolyfill.getFirstConsistentlyInteractive(opts).then((tti) => {
  ...
});
```

- 通过调用模块提供的 `getFirstConsistentlyInteractive()` 函数即可返回一个 Promise 对象，如果当前浏览器支持相关测量方法，则返回 TTI 值，否则返回 null。

#### 总阻塞时间（Total Blocking Time，TBT）

- 总阻塞时间由 W3C 标准 [Long Tasks API 1](https://www.w3.org/TR/2017/WD-longtasks-1-20170907/) 提出，是指阻塞用户响应（比如键盘输入、鼠标点击）的所有时间。指标值是将 FCP 之后一直到 TTI 这段时间内的阻塞部分时间总和，阻塞部分是指长任务执行时间减去 50 毫秒。

- 获取长任务耗时的方式如下：

```js
var observer = new PerformanceObserver(function (list) {
  var perfEntries = list.getEntries();
  for (var i = 0; i < perfEntries.length; i++) {
    console.log(perfEntries[i].toJSON());
    /* 
  { 
    attribution: [TaskAttributionTiming]， 
    duration: 6047.770000004675， 
    entryType: "longtask"， 
    name: "self"， 
    startTime: 22.444999995059334 
  } 
  */
  }
});
observer.observe({
  entryTypes: ['longtask'],
});
```

- 首先通过 `PerformanceObserver` 函数构造一个性能监测实例，通过回调函数参数的 getEntries() 函数来获取 PerformanceEntry 实例数组，每个实例对应一个长任务。同时要指定监测实例的实体类型为“longtask”。

#### 最大内容绘制（Largest Contentful Paint，LCP)

- 最大内容绘画指的是视口内可见的最大图像或文本块的绘制时间。测量这个指标的值和 TBT 相似，不同的是将实体类型改为“largest-contentful-paint”。

- 监测代码：

```js
var observer = new PerformanceObserver(function (list) {
  var perfEntries = list.getEntries();
  for (var i = 0; i < perfEntries.length; i++) {
    console.log(perfEntries[i].toJSON());
    /* 
    { 
      duration: 0, 
      element: img, 
      entryType: "largest-contentful-paint", 
      id: "", 
      loadTime: 274.864, 
      name: "", 
      renderTime: 0, 
      size: 2502, 
      startTime: 274.864, 
           url: "https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png" 
      } 
    */
  }
});
observer.observe({ entryTypes: ['largest-contentful-paint'] });
```

#### 统计方式

- 平均值统计
  - 平均值统计应该是大家最容易想到的统计方法，将所有用户产生的性能指标值收集起来，然后对这些数据取平均值，最终得到平均耗时数据。这种统计方式最大的问题就是容易受极值影响
- 百分位数统计
  - 百分位数统计可以解决极值问题。百分位数是对应于百分位的实际数值，比如第 70 百分位数：将数据从小到大排列，处于第 70% 的数据称为 70 分位数，表示 70% 的性能数据均小于等于该值，那剩下的 30% 的数据均大于等于该值了
  - 百分位数的好处在于，对于性能需求不同的页面或应用，可以设置不同的百分位数。对性能要求越高，使用越大的百分位数。比如在追求极致性能的情况下，要求 99% 的用户都要小于 3 秒，我们看页面加载时长时候就应该看 99 分位数。而某些重要程度比较低的页面，可以只要求 50% 的用户页面加载时长小于 3 秒，那么对应的就是 50 分位数，也称**中位数**。

#### 优化思路

- 前端性能优化一般可以从两个方向入手：**加载性能优化** 和 **渲染性能优化**。

- **做减法**是直接减少耗时操作或资源体积
- **做除法**是在耗时操作和资源体积无法减少的情况下，对其进行拆分处理或者对不可拆分的内容进行顺序调换。

---

- 加载性能的优化手段中，

  - 做减法的有：
    - 采用 gzip 压缩，典型的减少资源的传输体积；
    - 使用缓存，强制缓存可以减少浏览器请求次数，而协商缓存可以减少传输体积；
    - 使用雪碧图，减少浏览器请求次数。
  - 做除法的有：
    - HTTP2 多路复用，把多个请求拆分成二进制帧，并发传输；
    - 懒加载，将 Web 应用拆分成不同的模块或文件，按需加载；
    - 把 script 标签放到 body 底部，通过调整顺序来控制渲染时间。

- 在渲染性能优化的手段中
  - 做减法的有：
    - 避免重排与重绘，减少渲染引擎的绘制；
    - 防抖操作，减少函数调用或请求次数；
    - 减少 DOM 操作，减少渲染引擎和脚本引擎的切换，同时也减少渲染引擎绘制。
  - 做除法的有：
    - 骨架屏，将页面内容进行拆分，调整不同部分的显示顺序；
    - 使用 Web Worker，将一些长任务拆分出来，放到 Web Worker 中执行；
    - React Fiber，将同步视图的任务进行拆分，可调换顺序，可暂停。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[FileReader - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)

[Paint Timing 1](https://www.w3.org/TR/paint-timing/#sec-paint-timing)
