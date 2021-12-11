---
title: '毎日のフロントエンド  86'
date: 2021-12-11T20:18:58+09:00
description: frontend 每日一练
image: frontend-86-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八十六日

## HTML

### **Question:** `HTML5`的`ruby`标签，有哪些应用场景

HTML `<ruby>` 元素 被用来展示东亚文字注音或字符注释。

## CSS

### **Question:** `display`有哪些值？分别说明他们的作用是什么

常用：

- `display:block/inline-block` 给元素转块/转行内块
- `display:inline` 把元素转成内联元素（我很少用到）
- `display:none` 让元素消失，不显示
- `display:flex` 弹性布局
- `display:grid` 网格布局

## JavaScript

### **Question:** 写出执行结果,并解释原因

```js
var a = 0;
if (true) {
  a = 10;
  console.log(a, window.a);
  function a() {}
  console.log(a, window.a);
  a = 20;
  console.log(a, window.a);
}
console.log(a);
```

Result:

```js
10 0
10 10
20 10
10
```

#### 1. 变量提升

变量的提升是以变量作用域来决定的，即全局作用域中声明的变量会提升至全局最顶层，函数（或者块级）内声明的变量只会提升至该作用域最顶层。

```js
console.log(a);
var a = 10;
// 等价于
var a;
console.log(a);
a = 10;
```

#### 2. 函数提升

- **函数表达式不会声明提升**，这里输出 undefined,是 var a 变量声明的提升

```js
console.log(a); // undefined
var a = function () {};
```

---

- 函数一等公民,与其他值地位相同，所以 **函数声明会覆盖变量声明**
- 存在函数声明和变量声明（注意：仅仅是声明，还没有被赋值），而且变量名跟函数名是相同的，那么，它们都会被提示到外部作用域的开头，但是，函数的优先级更高，所以变量的值会被函数覆盖掉。

```js
/*************未赋值的情况***************/
// 变量名与函数名相同
var company;
function company() {
  console.log('yideng');
}
console.log(typeof company); // function,函数声明将变量声明覆盖了

/*************赋值的情况***************/
// 如果这个变量或者函数其中是赋值了的，那么另外一个将无法覆盖它：
var company = 'yideng'; // 变量声明并赋值
function company() {
  console.log('yideng');
}
console.log(typeof company); // string
// 这个其实再次赋值了
var company;
function company() {}
company = 'yideng'; // 被重新赋值
console.log(typeof company);
```

---

note:

1. 允许在块级作用域内声明函数
2. 函数声明类似于 var,即会提升到全局作用域或函数作用域的头部
3. 函数声明还会提升到所在的块级作用域的头部。

---

4. 块级作用域函数在编译阶段将函数声明提升到全局作用域，并且会在全局声明一个变量，值为 undefined。同时，也会被提升到对应的块级作用域顶层。
5. 块级作用域函数只有定义声明函数的那行代码执行过后，才会被映射到全局作用域。解释如下：

```js
console.log(window.a, a); //undefined undefined
{
  console.log(window.a, a); //undefined function a(){}
  function a() {}
  a = 10;
  console.log(window.a, a); //function a(){} 10
}
console.log(window.a, a); //function a(){} function a(){}

/**
1. 第一个log,块级作用域函数a的声明会被提升到全局作用域，所以不报错，是undefined undefined
2. 第二个log,在块级作用域中，由于声明函数a提升到块级作用域顶端,所以打印a = function a(){}，而window.a由于并没有执行函数定义的那一行代码，所以仍然为undefined。
3. 第三个log,这时已经执行了声明函数定义，所以会把函数a映射到全局作用域中。所以输出function a(){},
4. 第四个log,就是function a(){} function a(){}，因为在块级作用域中window.a的值已经被改变了，变成了function a(){}
*/
```

> **块级作用域函数只有执行函数声明语句的时候，才会重写对应的全局作用域上的同名变量。**

解析：

```js
var a; // 函数a声明提前，块级作用域函数a的声明会被提升到全局作用域
var a = 0; // 已经声明了a,这里会忽略变量声明，直接赋值为0
if (true) {
  // 块级作用域
  //function a() {} // 函数a的定义，提升到块级作用域最前面
  a = 10; // 执行a = 10，此时，在块级作用域中函数声明已经被提升到顶层，那么此时执行a，就是相当于赋值，将函数声明a赋值为数字a，这里就是赋值为10了，
  console.log(a, window.a); // a是块级作用域的function a,所以输出 10,window.a还是0，因为块级作用域函数只有执行函数声明语句的时候，才会重写对应的全局作用域上的同名变量
  function a() {} // 执行到函数声明语句，虽然这一行代码是函数声明语句，但是a，已经为数字10了，所以，执行function a(){}之后，a的值10就会被赋值给全局作用域上的a，所以下面打印的window.a,a都为10
  console.log(a, window.a); // a 还是块级作用域中的function a,前边已经被赋值为10，所以window.a前边已经变为了10
  a = 20; // 仍然是函数定义块级作用域的a,重置为20
  console.log(a, window.a); // 输出为函数提升的块级作用域的a,和window.a,所以这里输出20 10
}
console.log(a); // 因为在块级作用域中window.a被改变成了10，所以这里现在是10
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[`<ruby> - HTML`（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/ruby)

[display - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display#display_grid)
