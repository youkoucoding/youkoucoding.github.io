---
title: '毎日のフロントエンド  91'
date: 2021-12-16T11:56:42+09:00
description: frontend 每日一练
image: frontend-91-cover.jpg
categories:
  - HTML
  - Javascript
---

# 第九十一日

## HTML

### **Question:** 描述下元素的`href`和`src`有什么区别

1. 概念不同
   - `href`用于在当前文档和引用资源之间确立联系
   - `src`用于将资源替换当前元素
2. 解析方式不同
   - `href`解析时，会并行下载资源且不会停止当前文档处理， 异步
   - `src`解析时，会暂停当前文档处理， 同步

## JavaScript

### **Question:** `null`和`undefined`的区别是什么？这两者分别运用在什么场景

1. 概念方面:
   - `undefined`:一般是简单数据类型,表示此处应该有个值,但是当前尚未赋值
   - `null`:一般是复杂数据类型,表示不存在
2. 用途方面:
   - `undefined`: 返回执行之后无返回值/ 获取对象不存在的属性值
   - `null`: 原型链的最顶部的不存在对象

---

- es6 的结构与函数默认值，只有 `undefined` 可设默认值，`null` 不能
- `+null` 为 `0`，`+undefined` 为 `NaN`
- `JSON.stringify(undefined)` 为 `undefined`，`JSON.stringify(null)` 为 `'null'`
- `JSON.stringify({a:undefined})` 为 `'{}'`，`JSON.stringify({a:null})` 为 `'{"a":null}'`
- `typeof null` 为 `'object'`，`typeof undefined` 为 `'undefined'`

---

`null == undefined // true`

### **Question:** 原生 `JavaScript` 实现图片懒加载

#### 实现方案: 当图片在可视区域内时，才加载图片，否则不加载；也可一个给个默认的图片占位

1. 在 img 元素时，自定义一个属性 `data-src`，用于存放图片的地址；
2. 获取屏幕可视区域的尺寸；
3. 获取元素到窗口边缘的距离；
4. 判断元素时候在可视区域内，在则将 data-src 的值赋给 src,否则，不执行其他操作；

#### `APIs`:

- `IntersectionObserver`: 提供了一种异步观察目标元素与其祖先元素或顶级文档视窗(`viewport`)交叉状态的方法。祖先元素与视窗(`viewport`)被称为根(`root`)。

- `window.requestIdleCallback()`: 将在浏览器的空闲时段内调用的函数排队。这使开发者能够在主事件循环上执行后台和低优先级工作，而不会影响延迟关键事件，如动画和输入响应。

#### Caution:

- 提前加载，+100 像素
- 滚动时只处理未加载的图片即可
- 函数节流

#### Code:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>图片懒加载</title>
    <style>
      img {
        display: block;
        height: 450px;
        margin-bottom: 20px;
      }
    </style>
  </head>

  <body>
    <img data-src="./images/1.png" alt="" />
    <img data-src="./images/2.png" alt="" />
    <img data-src="./images/3.png" alt="" />
    <img data-src="./images/4.png" alt="" />
    <img data-src="./images/5.png" alt="" />
    <img data-src="./images/6.png" alt="" />
  </body>
  <script>
    var imgs = document.querySelectorAll('img');

    // 节流函数,定时器版本
    function throttle(func, wait) {
      let timer = null;
      return function (...args) {
        if (!timer) {
          func(...args);
          timer = setTimeout(() => {
            timer = null;
          }, wait);
        }
      };
    }

    //方法1： H + S > offsetTop
    function lazyLoad1(imgs) {
      //offsetTop是元素与offsetParent的距离，循环获取直到页面顶部
      function getTop(e) {
        var T = e.offsetTop;
        while ((e = e.offsetParent)) {
          T += e.offsetTop;
        }
        return T;
      }

      // 屏幕可视区域的高度 + 滚动条滚动距离 > 元素到文档顶部的距离，document.documentElement.clientHeight + document.documentElement.scrollTop > element.offsetTop
      var H = document.documentElement.clientHeight; //获取可视区域高度
      var S = document.documentElement.scrollTop || document.body.scrollTop;
      Array.from(imgs).forEach(function (img) {
        // +100 提前100个像素就开始加载
        // 并且只处理没有src即没有加载过的图片
        if (H + S + 100 > getTop(img) && !img.src) {
          img.src = img.dataset.src;
        }
      });
    }
    const throttleLazyLoad1 = throttle(lazyLoad1, 200);

    // 方法2：el.getBoundingClientRect().top <= window.innerHeight
    function lazyLoad2(imgs) {
      function isIn(el) {
        // 获取元素大小和位置
        var bound = el.getBoundingClientRect();
        var clientHeight = window.innerHeight;
        return bound.top <= clientHeight + 100;
      }
      Array.from(imgs).forEach(function (img) {
        if (isIn(img) && !img.src) {
          img.src = img.dataset.src;
        }
      });
    }
    const throttleLazyLoad2 = throttle(lazyLoad2, 200);

    // 滚轮事件监听
    // window.onload = window.onscroll = function () {
    //   throttleLazyLoad1(imgs);
    //   // throttleLazyLoad2(imgs);
    // };

    // 方法3：IntersectionObserver 自动观察元素是否在视口内
    function lazyLoad3(imgs) {
      const io = new IntersectionObserver((ioes) => {
        ioes.forEach((ioe) => {
          const img = ioe.target;
          const intersectionRatio = ioe.intersectionRatio;
          if (intersectionRatio > 0 && intersectionRatio <= 1) {
            if (!img.src) {
              img.src = img.dataset.src;
            }
          }
          img.onload = img.onerror = () => io.unobserve(img);
        });
      });
      imgs.forEach((img) => io.observe(img));
    }
    lazyLoad3(imgs);
  </script>
</html>
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[url、href、src 详解 - SegmentFault 思否](https://segmentfault.com/a/1190000002877022)

[Intersection Observer - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)

[`data-*` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/data-*)
