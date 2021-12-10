---
title: '毎日のフロントエンド  85'
date: 2021-12-10T17:06:03+09:00
description: frontend 每日一练
image: frontend-85-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八十五日

## HTML

### **Question:** `iframe`的使用场景有哪些

1. 典型系统结构，左侧是功能树，右侧就是一些常见的 table 或者表单之类的。每一个功能，单独分离，采用 iframe
2. ajax 上传文件
3. 加载别的网站内容，例如 google 广告，网站流量分析
4. 在上传图片时，不用 flash 实现无刷新
5. 跨域访问的时候可以用到 iframe，使用 iframe 请求不同域名下的资源

## CSS

### **Question:** 怎么让 `body` 高度自适应屏幕

Solution: `body{height:100vh}`

设置`body{height：100%}` **不可行**

- `height：100%` 是相对于父元素来说的，如果只设置 body 的高度属性，由于其父元素是 html 高度未设置，且并非浏览器窗口高度
- 只设置 body 为 100%是不能达到效果的，**必须`html，body`都设置 100%**
- `body{height: 100vh}`直接把高度设置成了视口高度，与`html`大小无关

## JavaScript

### **Question:** js 延迟加载的方式有哪些

- **`defer` 属性**[^1]
- **`async` 属性**[^1]
- 动态创建 DOM 方式
- 使用 `setTimeout` 延迟方法
- 让 JS 最后加载
- 使用 jQuery 的 getScript 方法

[^1]: [`<script>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script#attr-defer)

### **Question:** 写出执行结果,并解释原因

```js
let num = 10;
const increaseNumber = () => num++;
const increasePassedNumber = (number) => number++;
const num1 = increaseNumber();
const num2 = increasePassedNumber(num1);
console.log(num1);
console.log(num2);
```

Result: 10 10

- 一元操作符 `++` **先返回** 操作值, 再累加 操作值。
- num1 的值是 10, 因为 increaseNumber 函数首先返回 num 的值，也就是 10，随后再进行 num 的累加。
- num2 是 10 因为我们将 num1 传入 increasePassedNumber. number 等于 10（ num1 的值。同样道理， `++` 先返回 操作值, 再累加 操作值。） number 是 10，所以 num2 也是 10.

### **Question:** 写出执行结果,并解释原因

```js
function addToList(item, list) {
  return list.push(item);
}
const result = addToList('company', ['yideng']);
console.log(result);
```

Result:2

`push()`方法返回新数组的长度。[^2]

[^2]: [Array.prototype.push() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
