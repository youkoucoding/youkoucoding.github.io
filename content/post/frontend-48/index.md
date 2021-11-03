---
title: '毎日のフロントエンド　48'
date: 2021-11-03T15:29:08+09:00
description: frontend 每日一练
image: frontend-48-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四十八日

## HTML

### **Question:** 对 WEB 标准和 W3C 的理解与认识

web 标准简单来说可以分为结构、样式和行为。`HTML`标签构成页面的结构框架。`css`完成美化`html`标签构成的页面。`js`可以完成页面和用户的交互。

`W3C`对`web`标准提出了规范化的要求，也就是在实际编程中的一些代码规范：包含如下几点

1. 对于结构要求：（标签规范可以提高搜索引擎对页面的抓取效率，对 SEO 很有帮助）

   - 标签字母要小写
   - 标签要闭合
   - 标签不允许随意嵌套

2. 对于 css 和 js 来说

   - 尽量使用外链 css 样式表和 js 脚本。是结构、表现和行为分为三块，符合规范。同时提高页面渲染速度，提高用户的体验。
   - 样式尽量少用行间样式表，使结构与表现分离，标签的 id 和 class 等属性命名要做到见文知义，标签越少，加载越快，用户体验提高，代码维护简单，便于改版
   - 不需要变动页面内容，便可提供打印版本而不需要复制内容，提高网站易用性。

## CSS

### **Question:** 全屏滚动的原理是什么吗？它用到了 CSS 的哪些属性

1. `html`

```html
<div class="page-container">
  <div class="page-item">1</div>
  <div class="page-item">2</div>
  <div class="page-item">3</div>
</div>
```

2. `css`

`html`, `body`设置 `overflow: hidden`, 让视图中只包括一个分页;
设置滑动分页的长宽都是 `100%`;
外部容器设置 `transition` 过渡效果, 并设置为相对定位, 滚动是修改外部容器的 Top 值, 实现滚动效果

```css
html,
body {
  padding: 0;
  margin: 0;
  overflow: hidden;
}
.page-container {
  position: relative;
  top: 0;
  transition: all 1000ms ease;
  touch-action: none;
}
.page-item {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  border: 1px solid #ddd;
}
```

3. JavaScript

- 初始化,容器高度设置为窗口高度

```js
var container = document.querySelector('.page-container');
// 获取根元素高度, 页面可视高度
var viewHeight = document.documentElement.clientHeight;
// 获取滚动的页数
var pageNum = document.querySelectorAll('.page-item').length;
// 初始化当前位置, 距离原始顶部距离
var currentPosition = 0;
// 设置页面高度
container.style.height = viewHeight + 'px';
```

- 初始化滚动事件

向下滚动时, 当 `currentPosition` 比 整体分页高度 大的时候(绝对值相比小的时候), 向下滚动;
向上滚动时, 当 `currentPosition` 大于 0 的时候, 向上滚动.

```js
// 向下滚动页面
function goDown() {
  if (currentPosition > -viewHeight * (pageNum - 1)) {
    currentPosition = currentPosition - viewHeight;
    container.style.top = currentPosition + 'px';
  }
}

// 向上滚动页面
function goUp() {
  if (currentPosition < 0) {
    currentPosition = currentPosition + viewHeight;
    container.style.top = currentPosition + 'px';
  }
}
```

- 节流函数: 即在规定时间内只会触发一次指定方法, 用于滚动时防止多次触发

```js
function throttle(fn, delay) {
  let baseTime = 0;
  return function () {
    const currentTime = Date.now();
    if (baseTime + delay < currentTime) {
      fn.apply(this, arguments);
      baseTime = currentTime;
    }
  };
}
```

- 监听鼠标滚动

滚动事件 firefox 与其他浏览器的事件不同, 所以需要进行判断. `deltaY`大于 0 的时候, 想下滚动; 反之, 向上滚动.

```js
var handlerWheel = throttle(scrollMove, 1000);
// https://developer.mozilla.org/en-US/docs/Web/API/Element/mousewheel_event#The_detail_property
// firefox的页面滚动事件其他浏览器不一样
if (navigator.userAgent.toLowerCase().indexOf('firefox') === -1) {
  document.addEventListener('mousewheel', handlerWheel);
} else {
  document.addEventListener('DOMMouseScroll', handlerWheel);
}
function scrollMove(e) {
  if (e.deltaY > 0) {
    goDown();
  } else {
    goUp();
  }
}
```

- 监听移动端 touch 操作

当 touch 的最终位置大于起始位置时, 则页面向上滚动; 反之, 向下滚动.

```js
var touchStartY = 0;
document.addEventListener('touchstart', (event) => {
  touchStartY = event.touches[0].pageY;
});
var handleTouchEnd = throttle(touchEnd, 500);
document.addEventListener('touchend', handleTouchEnd);
function touchEnd(e) {
  var touchEndY = e.changedTouches[0].pageY;
  if (touchEndY - touchStartY < 0) {
    // 向上滑动, 页面向下滚动
    goDown();
  } else {
    goUp();
  }
}
```

## JavaScript

### **Question:** 对事件循环有了解吗

#### 单线程模型

JS 引擎有多个线程，但引擎同时只执行一个任务，其他任务都必须在后面排队，即引擎只在一个线程上运行。这个线程称为主线程。

#### 事件循环机制

JS 本身并不慢，慢的是读写外部数据，比如等待 Ajax 请求返回结果。如果等着 Ajax 返回结果出来，再往下执行，就会耗费很长的时间。所以 JS 设计了一种机制，CPU 可以不管 IO 操作，而是挂起该任务，先执行后面的任务，等到 IO 操作返回了结果，再继续执行挂起的任务。

同步任务执行完后，引擎一遍又一遍检查那些挂起来的异步任务是否满足进入主线程的条件。这种循环检查的机制，就叫做事件循环机制。

#### 任务队列

JS 引擎运行时，除了一个正在运行的主线程，还提供一个或多个任务队列，里面是各种被挂起的异步任务。首先，主线程会去执行所有的同步任务，等到同步任务全部执行完，就会去看任务队列里面的异步任务，如果满足条件，那么异步任务就重新进入主线程开始执行，这时它就会变成同步任务。等到执行完，下一个异步任务再进入主线程开始执行。一旦任务队列清空，程序就结束执行。

#### 同步任务和异步任务

程序里面所有的任务可以分成两类：

同步任务：没有被引擎挂起，在主线程上排队执行的任务。只有前一个任务执行完毕，才能执行后一个任务。
异步任务：被引擎挂起，不进入主线程，而进入任务队列的任务。只有引擎认为某个异步任务可以执行了，该任务才会进入主线程执行。排在异步任务后面的代码，不用等待异步任务结束会马上运行。

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)
