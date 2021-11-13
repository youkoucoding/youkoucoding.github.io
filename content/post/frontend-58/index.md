---
title: '毎日のフロントエンド　58'
date: 2021-11-13T15:00:34+09:00
description: frontend 每日一练
image: frontend-58-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十八日

## HTML

### **Question:** `HTML5` 相对于 `HTML4` 有哪些优势

更简洁-文档声明
更语义-语义标签
功能更强-各种表单属性及自定义属性等

## CSS

### **Question:** `box-sizing` 常用的属性有哪些？分别有什么作用

The `box-sizing` css property sets how the total width and heights of elements are calculated.

There are two behaviors to be used to adjust:

- `content-box` gives you the default `box-sizing` behavior. If you set an element's width to 100 px, then element's content box will be 100 px wide, and the width of any border or padding will be added to the final rendered width, **making the element wider than 100 px.**

- `border-box` tells the browser to account for any border and padding in the values you specify for an element's width and height. If you set an element's width to 100 pixels, that 100 px will **include any border or padding you added,** and the content box will shrink to absorb that extra width.

**`border-box` typically makes it much easier to size elements.** And that's the default styling that browsers use for the `<table>`, `<select>`, `<button>`elements, and for `<input>` elements whose type us `radio`, `checkbox`,`reset`, `button`, `submit`, `color`, or `search`.

> content-box 盒子的宽度不包含 border 和 padding，`border-box` 盒子的宽度包含 `border` 和 `padding`

## JavaScript

### **Question:** 请说下对`proto`和 `prototype` 的理解

- 只有**函数对象**才有`prototype`属性, `prototype`对象上**存放**共用的方法和属性
- **对象**都有 `__proto__`属性，`__proto__` 是指向该对象构造函数的原型属性（即`prototype`）, 函数本身也是对象， 同样有`__proto__`属性
- 同时，`constructor`是**对象**才有的属性,它是从一个对象指向一个函数的,指向的函数就是该对象的构造函数。每个对象都有构造函数

通过`__proto__`属性（从一个对象指向另一个对象）, 可以拿到`Object`原型对象上的属性和方法，原型对象上的`__proto__`又指向该构造函数的`prototype`，从而形成了一条原型链。

`p1.__proto__ === Parent.prototype; //true`

`__proto__`通常称为隐式原型，`prototype`通常称为显式原型，那可以说一个对象的隐式原型指向了该对象的**构造函数的显式原型**。

那么我们在显式原型上定义的属性方法，通过隐式原型传递给了构造函数的实例。这样一来实例就能很容易的访问到构造函数原型上的方法和属性了。

`Parent.prototype.__proto__ === Object.prototype; //true`

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[一张图搞定 JS 原型&原型链](https://segmentfault.com/a/1190000021232132)
