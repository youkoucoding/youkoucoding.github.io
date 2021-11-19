---
title: '毎日のフロントエンド　64'
date: 2021-11-19T14:21:57+09:00
description: frontend 每日一练
image: frontend-64-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第六十四日

## HTML

### **Question:** 写出 `html` 提供的几种空格实体

- `&nbsp;`: No-Break Space 不换行空格
- `&ensp;`: En Space 半角空格, en 是字体排印学的计量单位，为 em 宽度的一半(如`16px`字体中就是`8px`)
- `&emsp;`: Em Space 全角空格, em 是字体排印学的计量单位，相当于当前指定的点数。例如，`1em`在`16px`的字体中就是`16px`。
- `&thinsp;`: Thin Space 窄空格
- `&zwnj;`: Zero Width Non Joiner 是一个不打印字符

此外，浏览器还会把以下字符当作空白进行解析：空格（`&#x0020;`）、制表位（`&#x0009;`）、换行（`&#x000A;`）和回车（`&#x000D;`）还有（`&#12288;`）

## CSS

### **Question:** 举例说明`css`中颜色的表示方法有几种

1. 颜色单词: `blue` / `lightblue` / `skyblue` / `transparent`(透明)
2. `rgb(0-255, 0-255, 0-255)` / `rgba(0-255, 0-255, 0-255, 0-1)`
3. `hsl`色相: `hsl(色调，饱和度，明度)` `hsla( 色调，饱和度，亮度，不透明度 )` (兼容性)
4. 十六进制: `#0- #FFFFFF ( #0 - #fff ) ( 0-9 a-f | [A-F] )`

## JavaScript

### **Question:** (谜题)如何让(`a==1 && a==2 && a==3`)的值为 true，把 `==` 换成 `===` 后还能为`true`吗

```js
const a = { value: 0 };
a.valueOf = function () {
  return (this.value += 1);
};

console.log(a == 1 && a == 2 && a == 3); //true
```

#### 宽松相等`==`

先将左右两两边的值转化成**相同的原始类型**，然后再去比较他们是否相等。

在进行两个值的比较时，执行了类型的强制转换:

`ToPrimitive(input, PreferredType?)`

转化过程如下:

1. 如果输入`input`是基本类型, 就返回这个值
2. 如果输入变量是`Object`类型, 那么调用`input.valueOf()`. 如果返回结果是基本类型，就返回这个值
3. 如果都不是的话就调用`input.toString()`. 如果结果是基本类型, 就返回它
4. 如果以上都不可以，就会抛出一个类型错误`TypeError`， 表示转化`input`变量到基本类型失败。

#### `(a === 1 && a === 2 && a === 3)`(严格匹配) 问题

```js
var value = 0; //window.value
Object.defineProperty(window, 'a', {
  get: function () {
    return (this.value += 1);
  },
});

console.log(a === 1 && a === 2 && a === 3); // true
```

JS 中的原始类型将不再满足于上面的条件(**严格相等没有转化的过程**)，所以需要通过一些方式去调用一个函数，并在这个函数中做想做的事情。

这里不是宽松相等==，valueOf 将不会被 JS 引擎调用

> 属性描述符(`property descriptors`)

使用`Object.defineProperty`为对象定义了一个属性, 此处, 需要调用一个无需`()`(执行)的函数, 通过`get`属性, 我们可以调用一个函数并且不用在函数名后添加`()`

上面提到的解决方案中, 在 `window` 对象上定义了一个具有 `getter` 的 `a` 属性, 所以 `a` 可以在代码中直接被访问到(全局变量)， 因此也可以直接获得 a 的值。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[[译] 在 JS 中，如何让(a===1 && a===2 && a === 3)(严格相等)的值为 true？ - 掘金](https://juejin.cn/post/6844903725442531341)

[Object.defineProperty() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
