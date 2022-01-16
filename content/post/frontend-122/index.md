---
title: '毎日のフロントエンド  122'
date: 2022-01-16T10:21:54+09:00
description: frontend 每日一练
image: frontend-122-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百二十二日

## HTML

### **Question:** `form-data`、`x-www-form-urlencoded`、`raw`、`binary`的区别

1. `form-data`
   - http 请求中 `multipart/form-data` 它会将表单的数据处理为一条消息，以标签为单元，用分隔符分开
   - 既可以上传键值对，也可以上传文件
   - 当上传的字段是文件时，`Content-Type`来说明文件类型；
   - `content-disposition`，用来说明字段的一些信息
2. `x-www-form-urlencoded`
   - `application/x-www-from-urlencoded`, 会将表单内的数据转换为**键值对** `name=leyangjun&age =28&work＝meituan`
3. `raw`
   - 可上传任意格式的文本，可以上传 text、json、xml、html 等各种文本类型
4. `binary`
   - 等同于`Content-Type:application/octet-stream`，只可上传二进制数据，通常用来上传文件，由于没有键值，所以一次只能上传一个文件

note:

- `form-data`与`x-www-form-urlencoded`不同之处在于：
  - `multipart/form-data`：既可以上传文件等二进制数据，也可以上传表单键值对，只是最后会转化为一条信息
  - `x-www-form-urlencoded`：只能上传键值对，且键值对都是间隔分开

## JavaScript

### **Question:** 能否正确获取本地上传的文件路径

- 无法获取,客户端 js 脚本没有文件访问权限,只能由浏览器代为操作
- 可以通过 DOM-api 获取由浏览器转义的文件路径

```js
file.addEventListener('change', () => {
  var reader = new FileReader();
  reader.readAsDataURL(file.files[0]);
  reader.onload = function (e) {
    console.log(e.target.result); //base64数据 or 虚拟路径  取决于浏览器的实现
  };
});
```

## Clean Code JavaScript

### Object & Data structures

#### 1. Use getters and setters

Using getters and setters to access data on objects could be better than simply looking for a property on an object. Here's an unorganized list of reasons why:

- When you want to do more beyond getting an object property, you don't have to look up and change every accessor in your codebase.
- Makes adding validation simple when doing a `set`
- Easy to add logging and error handling when getting and setting.
- You can lazy load your object's properties, let's say getting it form a server.

**Bad:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0,
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**Good:**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance,
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

#### 2. Make objects have private members

This can be accomplished through `closures` (for ES5 and below).

**Bad:**

```javascript
const Employee = function (name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**Good:**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    },
  };
}

const employee = makeEmployee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)

[FileReader - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)
