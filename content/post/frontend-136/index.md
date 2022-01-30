---
title: '毎日のフロントエンド  136'
date: 2022-01-30T10:39:54+09:00
description: frontend 每日一练
image: frontend-136-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百三十六日

## HTML

### **Question:** HTML5 的服务器(server-sent event)发送事件

一个网页获取新的数据通常需要发送一个请求到服务器，也就是向服务器请求的页面。使用 server-sent 事件，服务器可以在任何时刻向我们的 Web 页面推送数据和信息。这些被推送进来的信息可以在这个页面上作为 Events + data 的形式来处理。

两种服务端推送技术的简单对比:

| server-sent event          | WebSocket                    |
| -------------------------- | ---------------------------- |
| 服务器到浏览器的单向通信   | 两端之间的双向实时通信       |
| 不兼容 IE                  | 兼容性更好                   |
| 协议实现断线重连与消息追踪 | 不在协议范围内, 需要手动处理 |
| 实现简单, 复用 HTTP        | 独立于 Http, 实现较复杂      |

具体的应用场景有:

- 邮箱: 实时获取新邮件
- 后台性能监控: 实时更新监控数据
- 天气预报: 实时更新天气信息

## CSS

### **Question:** css 计数器（序列数字字符自动递增）

- `counter-reset`:设置计数器
  - `counter-reset: count 0 /* 计数器从1开始 */`
- `counter-increment`: 递增数值
  - `counter-increment: count 2 /* 用于count 每次递增2 */`

```html
<ul>
  <li>Item</li>
  <li>Item</li>
  <ul>
    <li>Item</li>
    <li>Item</li>
  </ul>
</ul>
```

```css
ul {
  counter-reset: count;
}
li::before {
  counter-increment: count;
  content: counters(count, '-') '.';
}
```

```
1.Item
2.Item
    2-1.Item
    2-2.Item
```

## JavaScript

### Tips 判断数据类型

方法一： typeof (返回结果首字母小写)

```js
// 注意运行结果有单引号
typeof 1; // 'number'
typeof '1'; // 'string'
typeof undefined; // 'undefined'
typeof true; //'boolean'
typeof Sympol(); // 'sympol'
// =========
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
typeof console; // 'object'
// =========
typeof console.log; // 'function'
```

---

方法二： instanceof

```js
let Fruit = function () {};
let apple = new Fruit();
apple instanceof Fruit; // true

// =========

let banana = new String('バナナ');
banana instanceof String; // true

// =========
let str = 'peach';
str instanceof String; // false
```

```js
const customInstanceof = (left, right) => {
  // 先用 typeof 判断是否为 基本数据类型，如果是 则直接返回fasle；
  // 注意 'object' 单引号
  if (typeof left !== 'object' || left === null) return false;

  // 通过 getPrototypeOf 拿到参数的原型对象
  let proto = Object.getPrototypeOf(left);
  while (true) {
    if (proto === null) return false;
    if (proto === right.protoType) return true;

    // 拿到原型链再上一层原型，继续对比
    proto = Object.getPrototypeOf(proto);
  }
};
```

---

- `typeof` 可以判断基础数据类型（`null`除外）, 引用数据类型中 ，（`function`除外） 都无法判断
- `instanceof` 可以判断引用数据类型，但不能正确判断基础数据类型

---

方法三： `Object.prototype.toString`

返回结果的统一格式：`object Xxxxx`

```js
Object.prototype.toString({}); // '[object Object]'
Object.prototype.toString.call({}); // '[object Object]'
Object.prototype.toString.call(3); // '[object Number]'
Object.prototype.toString.call('3'); // '[object String]'
Object.prototype.toString.call(true); // '[object Boolean]'
Object.prototype.toString.call(function () {}); // '[object Function]'
Object.prototype.toString.call(null); // '[object Null]'
Object.prototype.toString.call(undefined); // '[object Undefined]'
Object.prototype.toString.call(/abc/g); // '[object RegExp]'
Object.prototype.toString.call(new Date()); // '[object Date]'
Object.prototype.toString.call([]); // '[object Array]'
Object.prototype.toString.call(document); // '[object HTMLDocument]'
Object.prototype.toString.call(window); // '[object Window]'
```

一个获取数据类型的通用方法

```js
function getType(obj) {
  let type = typeof obj;
  if (type !== 'object') return type; // 如果是 基本数据类型 可直接返回结果

  // 注意正则表达式中 object 后有一个空格
  return Object.prototype.toString.call(obj).replace(/^\[object (\S+)]$/, '$1');
}

// 验证， typeof 返回的结果首字母小写； toString返回结果 首字母大写
getType('123'); // 'string'
getType([]); // 'Array'
getType(null); // 'Null' typeof null, 返回结果是object，因此继续判断
getType(window); // 'Window'
getType(undefined); // 'undefined' typeof 直接返回
getType(); // ''
getType(function () {}); //  'function' typeof 直接判断后返回
getType(/123/g); // 'RegExp'
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[Server-sent events - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Server-sent_events)

[EventSource - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/EventSource)

[Object.getPrototypeOf() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/GetPrototypeOf)

[Object.prototype.toString() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
