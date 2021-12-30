---
title: '毎日のフロントエンド  105'
date: 2021-12-30T11:41:08+09:00
description: frontend 每日一练
image: frontend-105-cover.jpg
categories:
  - CSS
  - Javascript
---

# 第一百零五日

## CSS

### **Question:** 怎么实现移动端的边框 `0.5px`

1. 通过 transform 中的 scale

```css
border: 1px solid red;
transform: scaleY(0.5);
```

2. 通过 meta viewport 中设置`init-scale`为 0.5

```html
<meta name="viewport" content="width=device-width, initial-scale=0.5" />
```

3. 背景渐变实现, 一半透明

```css
.border-half {
  height: 1px;
  background-image: linear-gradient(0deg, red 50%, transparent 50%);
}
```

4. 伪类:after + 缩放

```css
li:after {
  content: '';
  display: block;
  position: absolute;
  left: -50%;
  width: 200%;
  height: 1px;
  background: red;
  transform: scale(0.5);
  -webkit-transform: scale(0.5);
}
```

## JavaScript

### **Question:** 写个方法，找出指定字符串中重复最多的字符及其长度(出现次数)

```js
function countStr(str) {
  let maxLen = 0;
  let char = '';

  for (let i = 0; i < str.length; i++) {
    const div = str[i];
    const len = str.split(div).length - 1;

    if (len > maxLen) {
      maxLen = len;
      char = div;
    }
  }

  return [char, maxLen];
}
```

---

```js
// 同上
function findTargetChar(str) {
  let maxStr;
  let maxLength = 0;
  let obj = {};
  let res;
  if (typeof str === 'string' && str.length > 0) {
    str.split('').forEach((ele, index) => {
      if (obj[ele] === undefined) {
        obj[ele] = 1;
      } else {
        obj[ele] += 1;
      }
      if (obj[ele] > maxLength) {
        maxLength = obj[ele];
        maxStr = ele;
      }
    });
    res = [maxStr, maxLength];
  }
  return res;
}
```

### **Question:** 如下代码的打印结果

```js
var name = 'Tom';
(function () {
  if (typeof name == 'undefined') {
    var name = 'Jack';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();
```

Result: Goodbye Jack

原代码相当于：

```js
var name = 'Tom';
(function () {
  // 变量会提升到最近的 function 作用域的上层
  var name;
  if (typeof name == 'undefined') {
    name = 'Jack';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();
```

- 上述代码执行结果与立即执行函数无关
- 本例中，函数内部使用 var 声明了 name,所以在创建执行环境时，函数声明了一个私有变量 name，函数执行阶段，if 中的 name 找到了私有变量 name，此时没有赋值，所以走的是 true 的分支。
- 如果在函数内部的 if 语句中使用 let 声明 name，则在创建执行环境的时候不会声明内部变量 name（if 花括号中 name 被认为是 if 的局部变量），在执行代码阶段，找到了全局变量 name（全局变量 name 已经赋值了），所以此时 if 判断走 false 分支。

---

- 立即执行函数，一个在定义时就会立即执行。
- 第一部分是包围在 圆括号运算符 () 里的一个匿名函数，这个匿名函数拥有独立的词法作用域。这不仅避免了外界访问此 IIFE 中的变量，而且又不会污染全局作用域。
- 第二部分再一次使用 () 创建了一个立即执行函数表达式，JavaScript 引擎到此将直接执行函数。
- 当函数变成立即执行的函数表达式时，表达式中的变量不能从外部访问。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[for...of （属性值）- JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)

[for...in (属性)- JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)

[IIFE（立即调用函数表达式） - 术语表 | MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/IIFE)
