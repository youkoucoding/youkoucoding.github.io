---
title: '毎日のフロントエンド　61'
date: 2021-11-16T12:44:26+09:00
description: frontend 每日一练
image: frontend-61-cover.jpg
categories:
  - HTML
  - Javascript
---

# 第六十一日

## HTML

### **Question:** HTML5 的`webSQL`(deprecated)和`IndexedDB`

`IndexedDB` 就是浏览器提供的本地数据库，它可以被网页脚本创建和操作。

`IndexedDB` 允许储存大量数据，提供查找接口，还能建立索引。这些都是 `LocalStorage` 所不具备的。

`IndexedDB` 不属于关系型数据库（不支持 SQL 查询语句），更接近 `NoSQL` 数据库。

`IndexedDB` 具有以下特点:

1. 键值对储存:每一个数据记录都有对应的主键，主键是独一无二的，不能有重复，否则会抛出一个错误
2. 异步: 这与 `LocalStorage` 形成对比，后者的操作是同步的
3. 支持事务: 一系列操作步骤之中，只要有一步失败，整个事务就都取消，数据库回滚到事务发生之前的状态，不存在只改写一部分数据的情况
4. 同源限制: 网页只能访问自身域名下的数据库，而不能访问跨域的数据库
5. 储存空间大: `IndexedDB` 的储存空间比 `LocalStorage` 大得多，一般来说不少于 250MB
6. 支持二进制储存: 二进制数据（`ArrayBuffer` 对象和 `Blob`(`Binary Large Object`) 对象）

## JavaScript

### **Question:** 写一个方法获取图片的原始宽高

```js
function loadImageAsync(url) {
  return new Promise(function (resolve, reject) {
    // Image(width, height) 创建一个新的HTMLImageElement实例
    var image = new Image();
    image.src = url;

    image.onload = function () {
      let obj = {
        w: image.naturalWidth, // 如果图片是以其原来的大小渲染，则此值等于图片的宽度。
        h: image.naturalHeight, // 如果图片是以其原来的大小渲染，则此值等于图片的高度
      };
      resolve(obj);
    };

    image.onerror = function () {
      reject(new Error('Could not load image at ' + url));
    };
  });
}
```

### **Question:** One more Question

```js
// 写出执行结果，并解释原因
(function () {
  var a = (b = 5);
})();
console.log(b);
console.log(a);
```

Answer: 5, Uncaught ReferenceError: a is not defined

Explain:

在这个立即执行函数表达式（`IIFE`）中包括两个赋值操作，其中 a 使用 var 关键字进行声明，因此其属于函数内部的局部变量（**仅存在于函数中**），相反，b 被分配到全局命名空间(window)。

> 这里没有在函数内部使用严格模式(`use strict;`)。如果启用了严格模式，代码会在输出 b 时报错`Uncaught ReferenceError: b is not defined`,需要记住的是，严格模式要求**显式的引用全局作用域**。

```js
(function () {
  'use strict';
  var a = (b = 5);
})();

console.log(b); //Uncaught ReferenceError: b is not defined

/*---------------------------*/

(function () {
  'use strict';
  var a = (window.b = 5);
})();

console.log(b); // 5
```

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[浏览器数据库 IndexedDB](https://www.ruanyifeng.com/blog/2018/07/indexeddb.html)
