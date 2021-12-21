---
title: '毎日のフロントエンド  96'
date: 2021-12-21T11:06:04+09:00
description: frontend 每日一练
image: frontend-96-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第九十六日

## HTML

### **Question:** HTML 标签：字体、居中、文字加粗、下标

- `<font>` 已废弃
- `<center>`已废弃: 该特性已经从 Web 标准中删除
- `<b>` HTML 提醒注意（"Bring Attention To"）元素,替代方案是使用 CSS `font-weight` 属性来创建粗体文字。
  - `<strong>`元素表示某些重要性的文本，`<em>`强调文本，而`<mark>`元素表示某些相关性的文本。`<b>`元素不传达这样的特殊语义信息；仅在没有其他合适的元素时使用它。
- 下标`<sub>`:这个元素不能用于样式上的目的，比如产品名称 `LaTeX` 的样式，这时应该使用 CSS 样式： `vertical-align` 属性的 `sub` 值能实现相同效果。

## CSS

### **Question:** 行内 css 和 important 哪个优先级高

`!important` 将覆盖行内 css
行内 css>id 选择器(`#`)>伪类(`:`)>属性选择器(`[]`)>类选择器(`.`) > 类型选择器(`div` `p` `a`等) > 通用选择器(`*`)

## JavaScript

### **Question:** 要实现一个 js 的持续动画，有什么比较好的方法

`window.requestAnimationFrame()`[^1]
[^1]: [`window.requestAnimationFrame` - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)

### **Question:** 下列代码输出结果

```js
var a = 10;
(function () {
  console.log(a);
  a = 5;
  console.log(window.a);
  var a = 20;
  console.log(a);
})();
```

Result: undefined 10 20

- 函数作用域可以访问全局作用域
- 全局作用域中无法访问局部作用域的变量
  - ```js
    (function () {
      var a = 456;
    })();
    console.log(a); // Error: a is not defined window已经是全局作用域了，在这里并没有发现变量`a`所以不会继续向上寻找，直接输出 a is not defined
    ```
- 当局部作用域中进行隐式声明时，默认会在全局作用域中声明该变量
  - ```js
    (function () {
      a = 456;
    })();
    console.log(a); // 456
    ```

```js
var window = this; // 再次声明此处可忽略，只是为了让你看到window是全局this的别名
var a; // undefined
a = 10;
(function () {
  var a; // undefined
  console.log(a); //  此处因为是显式声明所以你在函数作用域中访问到的，一定是他内部声明变量`a`
  a = 5; // 显式声明的变量`a` = 5
  console.log(window.a); // 直接找的是`window`的`a`则不在函数作用域中寻找 有趣的是，`function`中不通过`call`或`apply`修改`this`指针，此处输出 `this.a` 的效果是一致的
  a = 20; // 显式声明的变量`a` 由 5 变成 20
  console.log(a);
})();
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[`<b>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/b)

[`<sub>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/sub)

[vertical-align - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)
