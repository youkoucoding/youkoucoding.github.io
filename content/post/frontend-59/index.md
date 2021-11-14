---
title: '毎日のフロントエンド　59'
date: 2021-11-14T14:41:10+09:00
description: frontend 每日一练
image: frontend-59-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第五十九日

## HTML

### **Question:** 对`WebGL`的理解

`WebGL`（Web 图形库）是一个 `JavaScript API`，可在任何兼容的 Web 浏览器中渲染高性能的交互式 3D 和 2D 图形，而无需使用插件。WebGL 通过引入一个与 OpenGL ES 2.0 非常一致的 API 来做到这一点，该 API 可以在 `HTML5` `<canvas>` 元素中使用。 这种一致性使 API 可以利用用户设备提供的硬件图形加速。

## JavaScript

### **Question:** 一个格式化金额的方法( 添加逗号 )

```js
function formatPrice(val, spacer = ',') {
  const typeVal = typeof val;
  if (typeVal !== 'string' && typeVal !== 'number') return val;
  let _val = '' + val;
  return _val.replace(/\B(?=(\d{3})+\b)/g, spacer);
}

console.log(formatPrice(123567890.23)); // 123,567,890.23
```

---

> [Intl.NumberFormat - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat)

### **Question:** 写出执行结果， 并解释原因

```js
var a = [0];
if (a) {
  console.log(a == true);
} else {
  console.log(a);
}
```

> 在程序和方法的最顶端，let 不像 var 一样，let 不会在全局对象里新建一个属性。比如：位于函数或代码顶部的 var 声明会给全局对象新增属性, 而 let 不会。

Answer: `false`

当 a 出现在 if 的条件中时，被转成布尔值，而 `Boolean([0])`为 `true`,所以就进行下一步判断 `a == true`,在进行比较时，`[0]`被转换成了 0，所以 0==true 为 false

```js
!![]; //true 空数组转换为布尔值是 true,
!![0]; //true 数组转换为布尔值是 true
[0] == true; //false 数组与布尔值比较时却变成了 false
Number([]); //0
Number(false); //0
Number(['1']); //1
```

> 双感叹号：用两个!!就可以将变量转化为对应布尔值

```js
1 == true;  //true   1 === Number(true)
'true' == true; //false Number('true')->NaN  Number(true)->1
'' = 0;//true
'1' == true;//true  Number('1')->1
```

> `==` 比较， 类型转换规则：
>
> 1. 如果比较的是原始类型的值，原始类型的值会转成数值再进行比较
> 2. 对象与原始类型值比较，对象会转换成原始类型的值再进行比较。
> 3. undefined 和 null 与其它类型进行比较时，结果都为 false，他们相互比较时结果为 true

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[WebGL - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API)
