---
title: '毎日のフロントエンド  99'
date: 2021-12-24T14:22:47+09:00
description: frontend 每日一练
image: frontend-99-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第九十九日

## HTML

### **Question:** HTML5 的页面可见性（`Page Visibility`）有哪些应用场景

- `Document.hidden` 只读
  - 如果页面处于被认为是对用户隐藏状态时返回 true，否则返回 false。
- `Document.visibilityState` 只读
  - 是一个用来展示文档当前的可见性的 DOMString 。该属性的值为以下值之一：
    - `visible` : 页面内容至少是部分可见。 在实际中，这意味着页面是非最小化窗口的前景选项卡。
    - `hidden` : 页面内容对用户不可见。 在实际中，这意味着文档可以是一个后台标签，或是最小化窗口的一部分，或是在操作系统锁屏激活的状态下。
    - `prerender` : 页面内容正在被预渲染且对用户是不可见的(被`document.hidden`当做隐藏). 文档可能初始状态为 prerender，但绝不会从其它值转为该值。
- `Document.onvisibilitychange`

## CSS

### **Question:** 比较 `opacity: 0;` `visibility: hidden;` `display: none;` 优劣和适用场景

可见性：

- `display:none;`: 让元素完全从渲染树中消失，渲染的时候不占据任何空间, 不能点击
- `visibility: hidden;`: 不会让元素从渲染树消失，渲染元素继续占据空间，只是内容不可见，不能点击
- `opacity: 0;`: 不会让元素从渲染树消失，渲染元素继续占据空间，只是内容不可见，可以点击

继承性：

- `display: none` 和 `opacity: 0` ：是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示。
- `visibility: hidden`：是继承属性，子孙节点消失**由于继承了 hidden**，通过设置 visibility: visible;可以让子孙节点显式。

性能：

- `display: none;` : 修改元素会造成文档回流,读屏器不会读取 display: none 元素内容，性能消耗较大
- `visibility: hidden;`: 修改元素只会造成本元素的重绘,性能消耗较少读屏器可读取`visibility: hidden`元素内容
- `opacity: 0` ： 修改元素会造成重绘，性能消耗较少

---

- `display: none;`

  1. DOM 结构：浏览器不会渲染 display 属性为 none 的元素，不占据空间；
  2. 事件监听：无法进行 DOM 事件监听；
  3. 性能：动态改变此属性时会引起重排，性能较差；
  4. 继承：不会被子元素继承，毕竟子类也不会被渲染；
  5. `transition`：transition 不支持 display。

- `visibility: hidden;`

  1. DOM 结构：元素被隐藏，但是会被渲染不会消失，占据空间；
  2. 事件监听：无法进行 DOM 事件监听；
  3. 性 能：动态改变此属性时会引起重绘，性能较高；
  4. 继 承：会被子元素继承，子元素可以通过设置 visibility: visible; 来取消隐藏；
  5. `transition`：transition 支持 visibility。visibility 会立即显示，隐藏时会延时

- `opacity: 0;`

  1. DOM 结构：透明度为 100%，元素隐藏，占据空间；
  2. 事件监听：可以进行 DOM 事件监听；
  3. 性 能：提升为合成层，不会触发重绘，性能较高；
  4. 继 承：会被子元素继承,且，子元素并不能通过 opacity: 1 来取消隐藏；
  5. `transition`：transition 支持 opacity。opacity 可以延时显示和隐藏

## JavaScript

### **Question:** 不用第三方库，原生 js 怎么实现读取和导出 excel

- 接收后端传过来的 excel 文件流

```js
axios({
  url: window.fdConfig.url[indexParams.index].export,
  method: window.fdConfig.methodPost,
  data: downloadPrams,
  // 设置类型为 blob
  responseType: 'blob',
}).then(
  (response) => {
    const blob = new Blob([response.data], {
      type: 'application/vnd.ms-excel',
    });
    const link = document.createElement('a');
    link.href = window.URL.createObjectURL(blob);
    // 这里也可以从headers中获取文件名.
    link.download = '导出excel.xlsx';
    // element.click() 模拟鼠标点击
    link.click();
  },
  (error) => {
    console.log('导出excel出错');
  }
);
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[页面可见性 API - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Page_Visibility_API)

[Document.visibilityState - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/visibilityState)

[Page Visibility API 教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2018/10/page_visibility_api.html)
