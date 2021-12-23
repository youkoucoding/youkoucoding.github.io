---
title: '毎日のフロントエンド  98'
date: 2021-12-23T16:33:08+09:00
description: frontend 每日一练
image: frontend-98-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第九十八日

## HTML

### **Question:** 列举出常用的 meta 标签的写法和作用

`<meta>`用来描述网页的元信息；诸如字符编码，浏览器引擎编译，文档信息等等；

- `charset`：声明当前文档的字符编码，用于浏览器解析文档；如：
  `<meta charset="UTF-8">`
- `name`：声明当前文档的元信息；常用的有 viewport，keywords，description 等；
  - `viewport`：文档视口设置，如初始视口大小（`initial-scale`），是否允许用户缩放（`user-scalable`）等；
  - `keywords`：网页的关键字，常用于搜索引擎对于该网页的搜索关键字；
  - `description`：网页的描述信息；
- `http-equiv`：可以用来设定一些属性改变服务器或浏览器引擎对文档的编译行为；

- http-equiv 和 name 的属性，**属性值**通过`<meta>`标签的`content`属性来设置；例如：`<meta name="format-detection" content="telephone=no" />`

## JavaScript

### **Question:** 下列代码运行结果

```js
function changeObjProperty(o) {
  o.siteUrl = 'http://www.123.com';
  o = new Object();
  o.siteUrl = 'http://www.456.com';
}
let webSite = new Object();
changeObjProperty(webSite);
console.log(webSite.siteUrl);
```

Result: `http://www.123.com`

```js
// 这里把o改成a
// webSite引用地址的值copy给a了
function changeObjProperty(a) {
  // 改变对应地址内的对象属性值
  a.siteUrl = 'http://www.123.com';
  // 变量a指向新的地址 以后的变动和旧地址无关
  a = new Object();
  a.siteUrl = 'http://www.456.com';
  a.name = 456;
}
var webSite = new Object();
webSite.name = '123';
changeObjProperty(webSite);
console.log(webSite); // {name: 123, siteUrl: 'http://www.123.com'}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)
