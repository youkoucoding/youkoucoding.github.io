---
title: '毎日のフロントエンド  129'
date: 2022-01-23T13:32:22+09:00
description: frontend 每日一练
image: frontend-129-cover.jpg
categories:
  - CSS
  - Javascript
---

# 第一百二十九日

## CSS

### **Question:** `absolute`的`containing block`（包含块）计算方式和正常流有什么区别

大多数情况下，包含块（containing block）就是这个元素最近的祖先块元素的内容区

如何确定包含块(`containing block`):

- 包含块的过程完全依赖于这个元素的 position 属性：

  1. 如果 position 属性为 static 、 relative 或 sticky，包含块可能由它的最近的祖先块元素（比如说 inline-block, block 或 list-item 元素）的内容区的边缘组成，也可能会建立格式化上下文(比如说 a table container, flex container, grid container, 或者是 the block container 自身)。
  2. 如果 position 属性为 `absolute` ，包含块就是由它的最近的 position 的值不是 static （也就是值为 fixed, absolute, relative 或 sticky）的祖先元素的内边距区的边缘组成。
  3. 如果 position 属性是 fixed，在连续媒体的情况下(continuous media)包含块是 viewport ,在分页媒体(paged media)下的情况下包含块是分页区域(page area)。
  4. 如果 position 属性是 absolute 或 fixed，包含块也可能是由满足以下条件的最近父级元素的内边距区的边缘组成的：
     1. transform 或 perspective 的值不是 none
     2. will-change 的值是 transform 或 perspective
     3. filter 的值不是 none 或 will-change 的值是 filter(只在 Firefox 下生效).
     4. contain 的值是 paint (例如: contain: paint;)

## JavaScript

### **Question:** js 关闭当前窗口

`window.close()`

window.close 只能关闭由 window.open 方法打开的页面。网上说在 FireFox 下直接使用 window.close 可能会有问题。最好先执行一下 `window.open('', '_self')` 后再执行 window.close

## Clean Code JavaScript

### Comments

#### 1. Only comment things that have business logic complexity

Comments are an apology, not a requirement. Good code mostly documents itself.

**Bad:**

```js
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**Good:**

```js
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

#### 2. Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Bad:**

```js
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good:**

```js
doStuff();
```

#### 3. Don't have journal comment

Use Version Control! There's no need for dead code, commented code, and especially journal comments. **Use `git log` to get history**

**Bad:**

```js
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Good:**

```js
function combine(a, b) {
  return a + b;
}
```

#### 4. Avoid positional markers

They usually just aff noise. Let the functions and variable names alongs with proper indentation and formatting give the visual structure to your code.

**Bad:**

```js
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: 'foo',
  nav: 'bar',
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function () {
  // ...
};
```

**Good:**

```js
$scope.model = {
  menu: 'foo',
  nav: 'bar',
};

const actions = function () {
  // ...
};
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)

[布局和包含块 - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Containing_block)
