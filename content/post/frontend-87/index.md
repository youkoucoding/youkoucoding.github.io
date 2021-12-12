---
title: '毎日のフロントエンド  87'
date: 2021-12-12T15:34:58+09:00
description: frontend 每日一练
image: frontend-87-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第八十七日

## HTML

### **Question:** 举例说明如何原样输出 HTML 代码，不被浏览器解析

- `<pre>`[^1]
- `<code>`[^2]
- `<textarea>`[^3]

```html
<textarea rows="" cols="" style="border: none;resize: none;width: 100%;">
<div class="div2">2</div>
</textarea>
```

[^1]: [`<pre>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/pre)
[^2]: [`<code>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code)
[^3]: [`<textarea>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/textarea)

## CSS

### **Question:** 写出几个初始化 CSS 的样式，并解释说明为什么要这样写

[https://necolas.github.io/normalize.css/latest/normalize.css](https://necolas.github.io/normalize.css/latest/normalize.css)

- 因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对 CSS 初始化往往会出现浏览器之间的页面显示差异
- 初始化 CSS 样式可以提高编码质量，保持代码的统一性，如果不初始化，重复的 CSS 样式很多。

---

Examples:

```css
/*清除元素默认的内外边距  */
* {
  margin: 0;
  padding: 0;
}
/*让所有斜体 不倾斜*/
em,
i {
  font-style: normal;
}
/*去掉列表前面的小点*/
li {
  list-style: none;
}
/*图片没有边框   去掉图片底侧的空白缝隙*/
img {
  border: 0; /*ie6*/
  vertical-align: middle;
}

/*取消链接的下划线*/
a {
  text-decoration: none;
}

/*清除浮动*/
.clearfix:after {
  visibility: hidden;
  clear: both;
  display: block;
  content: '.';
  height: 0;
}

.clearfix {
  *zoom: 1;
}
```

## JavaScript

### **Question:** 写一个 sleep（暂停）函数

```js
function sleep(time) {
    return new Promise((resolve) =>
        setTimeout(resolve, time))
}
async function main() {
    // … do something …
    await sleep(5000)
    // … do something else …
}()
```

### **Question:** 写出输出结果，并解释

```js
function foo() {
  console.log(length);
}
function bar() {
  var length = 'Nice to meet you!';
  foo();
}
bar();
```

Result: 0

- 因为 foo 函数是由 window 对象调用，打印的 length 是 window 对象下的 length 属性 0。`window.length ===0`[^4]

[^4]: [`Window.length` - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/length)

- `var` 在代码中的任意位置声明变量总是等效于在代码开头声明,“hoisting”就像是把所有的变量声明移动到函数或者全局代码的开头位置。

- `foo`函数不是`bar`的内部函数（闭包），而是全局函数。**一个函数内部定义(不是调用)另一个函数 内部函数可调用外部函数的变量**

- 函数作用域链的确定：
  - 函数域是指函数声明时的所在的地方，或者函数在顶层被声明时指整个程序。
  - 函数提升仅适用于函数声明，而不适用于函数表达式。
  - 在函数内定义的变量不能在函数之外的任何地方访问，因为变量仅仅在该函数的域的内部有定义。相对应的，一个函数可以访问定义在其范围内的任何变量和函数。换言之，定义在全局域中的函数可以访问所有定义在全局域中的变量。在另一个函数中定义的函数也可以访问在其父函数中定义的所有变量和父函数有权访问的任何其他变量。

> 在函数创建的时候创建一个包含全局变量对象的作用域链，储存在内部`[[Scope]]`属性中。函数执行的时候会创建一个执行环境，通过复制`[[Scope]]`属性中的对象，构建执行环境的作用域链，并把自己的活动对象推入该作用域链的前端以此形成完整的作用域链。`[[Scope]]`保存的是对全局变量的引用，而不是值的复制。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[函数 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Functions)
