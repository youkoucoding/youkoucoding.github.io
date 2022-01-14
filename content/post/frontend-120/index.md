---
title: '毎日のフロントエンド  120'
date: 2022-01-14T10:59:02+09:00
description: frontend 每日一练
image: frontend-120-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百二十日

## HTML

### **Question:** 富文本编辑器的实现原理

WYSIWYG stands for “What you see is what you get”. [^1]

[^1]: [Program your own WYSIWYG editor - with HTML, CSS & JavaScript](https://webdeasy.de/en/program-wysiwyg-editor/)

[javascript - How to build an own WYSIWYG editor (RTE) using Vanilla JS - Stack Overflow](https://stackoverflow.com/questions/67021368/how-to-build-an-own-wysiwyg-editor-rte-using-vanilla-js)

1. `contenteditable`(HTML)属性：一个全局枚举属性，用于表示元素是否可编辑；该属性可被继承；
2. `caret-color`(CSS)属性：可设置编辑区域内的可插入光标的颜色；
3. `window.getSelection()`方法：返回一个 `Selection` 对象，表示用户选择的文本范围或光标的当前位置；利用该方法可以对选中的文字进行样式设置，或在光标处插入内容等操作

## CSS

### **Question:** 设置字体时为什么建议设置替换字体

在浏览器不兼容首选字体情况下的一种替代方案，设置替换字体（尽可能样式接近的替换字体）可以尽可能保证所有用户的浏览体验。

`font-family:name/inherit;`

- name 为首选字体的名称，字体名称有多个单词，即中间有空格，则需要将字体名称用一对单引号或者双引号包围起来
- 为 font-family 属性指定多种字体，且多种字体之间用逗号隔开，这样可以为页面指定一个字体列表。
- 如果用户机器没有第一种字体，则浏览器会查找字体列表中的下一种字体作为替代字体显示。如果找遍了字体列表还是没有可以使用的字体，浏览器才会使用默认字体显示页面。

## JavaScript

### **Question:** 如何终止 `Web Worker`

Web Worker 的作用，就是为 JavaScript 创造多线程环境，允许主线程创建 Worker 线程，将一些任务分配给后者运行。Worker 线程一旦新建成功，就会始终运行，不会被主线程上的活动（比如用户点击按钮、提交表单）打断。在主线程运行的同时，Worker 线程在后台运行，两者互不干扰。等到 Worker 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算密集型或高延迟的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢。

> Web Worker 比较耗费资源，不应该过度使用，而且一旦使用完毕，就应该关闭

`worker.terminate();`

## Clean Code JavaScript

### Variables 变量

#### 1. Use meaningful and pronounceable variable names

**Bad:**

```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Good:**

```javascript
const currentDate = moment().format('YYYY/MM/DD');
```

#### 2. Use the same vocabulary for the same type of variable

**Bad:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good:**

```javascript
getUser();
```

#### 3. Use searchable names

It's important that the code we do write is readable and searchable.

- [buddy.js](https://github.com/danielstjules/buddy.js)
- [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)

**Bad:**

```javascript
// What's 86400000 for?
setTimeout(blastOff, 86400000);
```

**Good:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

#### 4. Use explanatory variables

**Bad:**

```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Good:**

```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

#### 5. Avoid Mental Mapping

**Explicit is better than implicit.**

**Bad:**

```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**Good:**

```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

#### 6. Don't add unneeded context

If your class/object name tells you something, don't repeat that in your
variable name.

**Bad:**

```javascript
const Car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue',
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**Good:**

```javascript
const Car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue',
};

function paintCar(car, color) {
  car.color = color;
}
```

#### 7. Use default arguments instead of short circuiting or conditionals

Default arguments are often cleaner than short circuiting(短路). Be aware that if you
use them, your function will only provide default values for `undefined`
arguments. Other "falsy" values such as `''`, `""`, `false`, `null`, `0`, and
`NaN`, will not be replaced by a default value.（This will make checkfalsy mandatory.）

**Bad:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}
```

**Good:**

```javascript
function createMicrobrewery(name = 'Hipster Brew Co.') {
  // ...
}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)

[contenteditable - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/contenteditable)

[Program your own WYSIWYG editor - with HTML, CSS & JavaScript](https://webdeasy.de/en/program-wysiwyg-editor/)

[Web Worker 使用教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2018/07/web-worker.html)
