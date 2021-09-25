---
title: '毎日のフロントエンド　10'
date: 2021-09-23T23:39:38+09:00
description: frontend 每日一练
image: frontend-10-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第十日

## HTML

**#Question:** viewport 常见设置都有哪些？

`viewport` 就是视区窗口，也就是浏览器中显示网页的部分。PC 端上基本等于设备显示区域，但在移动端上`viewport` 会超出设备的显示区域 (即会有横向滚动条出现)。设备默认的 `viewport` 在 `980 - 1024` 之间。

| 设置          | 解释                                                               |
| ------------- | ------------------------------------------------------------------ |
| width         | 设置 layout viewport 的宽度，为一个正整数，或字符串"width-device"  |
| initial-scale | 设置页面的初始缩放值，为一个数字，可以带小数                       |
| minimum-scale | 允许用户的最小缩放值，为一个数字，可以带小数                       |
| maximum-scale | 允许用户的最大缩放值，为一个数字，可以带小数                       |
| height        | 设置 layout viewport 的高度，这个属性对我们并不重要，很少使用      |
| user-scalable | 是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes 代表允许 |

```html
// width=device-width, initial-scale=1.0 是为了兼容不同浏览器
<meta
  name="viewport"
  content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0"
/>
```

> dpr 与 CSS 像素。CSS 像素的 1px 在 PC 端上与设备的物理像素基本一致，而到手机端就会有两个物理像素对应一个 CSS 像素的情况出现（如 iPhone 的视网膜屏）。 所以 iPhone 上的 dpr = 2 即 2 个物理像素 / 一个 CSS 像素（独立像素）

## CSS

**#Question:** 对比下 px、em、rem 有什么不同？

`px`、`em`、`rem` 都是计量单位，都能表示*尺寸*，但是有有所不同，而且其各有各的优缺点。

- `px`表示“绝对尺寸”（并非真正的绝对），实际上就是 css 中定义的像素（此像素与设备的物理像素有一定的区别，后续详细说明见文末说明 1），利用 px 设置字体大小及元素宽高等比较稳定和精确。Px 的缺点是其不能适应浏览器缩放时产生的变化，因此一般不用于响应式网站。

- `em`表示**相对尺寸**, 其相对于当前对象内文本的 `font-size`（如果当前对象内文本的 `font-size` 计量单位也是 `em`，则当前对象内文本的 `font-size` 的参考对象为**父元素文本** `font-size`）。使用 `em` 可以较好的相应设备屏幕尺寸的变化，但是在进行元素设置时都需要知道父元素文本的 `font-size` 及当前对象内文本的 `font-size`，如有遗漏可能会导致错误。

  - `em`子元素字体大小`font-size` 的 `em` 是 相对于 父元素的 `font-size`
  - 子元素的 `height width padding margin` 的 `em` 值 是相对于 *本元素*的`font-size` 值

  ```html
  <div>
    我是父元素div
    <p>
      我是子元素p
      <span>我是孙元素span</span>
    </p>
  </div>
  ```

  ```css
  div {
    font-size: 40px;
    width: 10em; /* 400px */
    height: 10em;
    border: solid 1px black;
  }
  p {
    font-size: 0.5em; /* 20px */
    width: 10em; /* 200px */
    height: 10em;
    border: solid 1px red;
  }
  span {
    font-size: 0.5em;
    width: 10em;
    height: 10em;
    border: solid 1px blue;
    display: block;
  }
  ```

- `rem` 其参考对象为**根元素 root**的 `font-size` 即 `<html>`元素。通常做法是给 `html` 元素设置一个字体大小，然后其他元素的长度单位就为 `rem`。

  ```css
  html {
    font-size: 10px;
  }
  div {
    font-size: 4rem; /* 40px */
    width: 30rem; /* 300px */
    height: 30rem;
    border: solid 1px black;
  }
  p {
    font-size: 2rem; /* 20px */
    width: 15rem;
    height: 15rem;
    border: solid 1px red;
  }
  span {
    font-size: 1.5rem;
    width: 10rem;
    height: 10rem;
    border: solid 1px blue;
    display: block;
  }
  ```

  - 当用 `rem` 做响应式页面，直接在媒体中改变 `html` 的 `font-size` 那么用 `rem` 作为单位的元素的大小都会相应改变，很方便。

**_`px`用于元素的边框或定位。 推荐使用 `rem`(只有一个参照，方便管理)， em 容易出错_**

## JavaScript

**Question:** 简要描述下什么是回调函数并写一个例子出来

回调是把一个函数作为参数传递给另一个函数，当该函数满足某个条件时触发该参数函数。

主要用于异步操作 例如网络请求 防止页面同步代码阻塞导致渲染线程停止

```js
function longTask(callback, timeout) {
  setTimeout(callback, timeout);
}

longTask(() => {
  console.log('回调任务被执行了');
}, 2000);

console.log('我是同步代码 不会阻塞我');
```

## Reference

[移动前端开发之 viewport 的深入理解](https://www.cnblogs.com/2050/p/3877280.html)

[前端面试每日 3+1-以前端面试题来驱动学习，提倡每日学习与思考，每天进步一点！](http://www.h-camel.com/index.html)
