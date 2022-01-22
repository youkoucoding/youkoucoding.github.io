---
title: '毎日のフロントエンド  128'
date: 2022-01-22T12:21:35+09:00
description: frontend 每日一练
image: frontend-128-cover.jpg
categories:
  - CSS
  - Javascript
---

# 第一百二十八日

## CSS

### **Question:** `visibility`属性 `collapse`属性值有什么作用

- 设置`visibility: collapse;`后对于普通元素来说跟`visibility: hidden`效果一样，**隐藏元素，且占用空间**
- 对于一些 table 元素，比如 row、columu、group，效果则跟`display: none;`一样，**隐藏元素，但不占空间**

## JavaScript

### **Question:** js 包装对象

所谓“包装对象”，指的是与数值、字符串、布尔值分别相对应的 Number、String、Boolean 三个原生对象。这三个原生对象可以把原始类型的值变成（包装成）对象。

```js
let v1 = new Number(123);
let v2 = new String('abc');
let v3 = new Boolean(true);

typeof v1; // "object"
typeof v2; // "object"
typeof v3; // "object"

v1 === 123; // false
v2 === 'abc'; // false
v3 === true; // false
```

- **设计目的**，首先是使得“对象”这种类型可以覆盖 JavaScript 所有的值，整门语言有一个通用的数据模型，其次是使得原始类型的值也有办法调用自己的方法。

- `Number`、`String`和`Boolean`这三个原生对象，如果不作为构造函数调用（即调用时不加 new），而是**作为普通函数调用，常常用于将任意类型的值转为数值、字符串和布尔值。**

- `valueOf()`

  - `valueOf()`方法返回包装对象实例对应的原始类型的值。

  ```js
  new Number(123).valueOf(); // 123
  new String('abc').valueOf(); // "abc"
  new Boolean(true).valueOf(); // true
  ```

- `toString()`
  - `toString()`方法返回对应的字符串形式。
  ```js
  new Number(123).toString(); // "123"
  new String('abc').toString(); // "abc"
  new Boolean(true).toString(); // "true"
  ```

#### 原始类型与实例对象的自动转换

- 字符串可以调用 length 属性，返回字符串的长度。

```js
'abc'.length; // 3
```

caveat:

自动转换生成的包装对象是只读的，无法修改。所以，字符串无法添加新属性。

```js
var s = 'Hello World';
s.x = 123;
s.x; // undefined
```

调用结束后，包装对象实例会自动销毁。这意味着，下一次调用字符串的属性时，实际是调用一个新生成的对象，而不是上一次调用时生成的那个对象，所以取不到赋值在上一个对象的属性。

如果要为字符串添加属性，只有在它的原型对象 `String.prototype` 上定义

- 自定义方法

```js
String.prototype.double = function () {
  return this.valueOf() + this.valueOf();
};

'abc'.double();
// abcabc

Number.prototype.double = function () {
  return this.valueOf() + this.valueOf();
};

(123).double(); // 246 必须要加上圆括号，否则后面的点运算符（.）会被解释成小数点
```

## Clean Code JavaScript

### Formatting

Formatting is subjective. Like many rules herein, there is no hard and fast rule that you must follow. The main point is DO NOT ARGUE over formatting. There are tons of tools to automate this. Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting (indentation, tabs vs. spaces, double vs. single quotes, etc.) look here for some guidance.

#### 1. Use consistent capitalization

JavaScript is untyped, so capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just be consistent.

**Bad:**

```js
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good:**

```js
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

#### 2. Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee. We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Bad:**

```js
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**Good:**

```js
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)

[JavaScript Standard Style](https://standardjs.com/rules.html)
