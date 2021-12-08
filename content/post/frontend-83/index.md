---
title: '毎日のフロントエンド　 83'
date: 2021-12-08T17:52:29+09:00
description: frontend 每日一练
image: frontend-83-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八十三日

## HTML

### **Question:** 在新窗口打开链接的方法是什么？那怎么设置全站链接都在新窗口打开

1. `a`标签的`target`属性

```html
<a href="#" target="_blank"></a>
```

2.在 `head` 标签下,为所有的链接设置默认的打开方式为新窗口打开；

```html
<head>
  <base target="_blank" />
</head>
```

## JavaScript

### **Question:** 举例说明 js 如何实现继承

`JS` 基于原型链实现的继承，简单来说就是通过对象的`__proto__`实现的向上查找。

#### 原型

JS 中的对象包含了一个`prototype`的内部属性，这个属性所对应的就是该对象的原型, in chrome `[[prototype]]` or `__proto__`。

- 所有引用类型（函数，数组，对象）都拥有`__proto__`属性（隐式原型)
- 所有函数除了有*proto*属性之外还拥有`prototype`属性（显式原型）
- 原型对象：**每创建一个函数，该函数会自动带有一个`prototype`属性**，该属性是一个指针，指向了一个对象，我们称之为原型对象。

> 函数除了有`__proto__`属性之外还拥有`prototype`属性

1. 实例对象只有`__proto__`（隐式原型），构造函数既有 `__proto__`（隐式原型）也有`prototype`（显式原型）

2. `__proto__` 和 `prototype` 都是一个对象，既然是对象，就表示他们也有一个 `__proto__`

```js
a.__proto__.__proto__;
A.prototype.__proto__;
```

3. 实例对象 a 的隐式原型指向它构造函数的显式原型

```js
a.__proto__ === A.prototype;
```

4. 当调用某种方法或查找某种属性时，首先会在自身调用和查找，如果自身并没有该属性或方法，则会去它的`__proto__`属性中调用查找，也就是它构造函数的`prototype`中调用查找。

#### 原型链

1. 查找属性时，如果本身没有，则会去`__proto__`中查找，也就是构造函数的显式原型中查找，如果构造函数的显式原型中也没有该属性，因为构造函数的显式原型也是对象，也有`__proto__`，那么会去它的显式原型中查找，一直到 `null`, `Object.prototype.__proto__ === null`，如果没有则返回 `undefined`

2. `p.__proto__.constructor == function Person(){}`

3. `p.__proto__.__proto__== Object.prototype`

4. `p.__proto__.__proto__.__proto__== Object.prototype.__proto__ === null`

5. **通过`__proto__`形成原型链而非`protrotype`**, 通过原型链的方式, 实现了继承。

### **Question:** 写出执行结果,并解释原因

```js
function user(obj) {
  obj.name = '一';
  obj = new Object();
  obj.name = '二';
}
let person = new Object();
user(person);
console.log(person.name);
```

答案：一

```js
// obj传入的是引用
function user(obj) {
  obj.name = '一'; // 修改传入引用的值

  obj = new Object(); // obj 指向了一个新地址，（新实例， 与传入的person引用，无关）

  obj.name = '二'; //  修改的是新对象，没法改变传入的对象
}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[`<base>`：文档根 `URL` 元素 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/base)

[搞懂原型和原型链](https://www.cnblogs.com/powertoolsteam/p/14009110.html)
