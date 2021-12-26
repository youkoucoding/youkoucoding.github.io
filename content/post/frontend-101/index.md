---
title: '毎日のフロントエンド  101'
date: 2021-12-26T17:44:12+09:00
description: frontend 每日一练
image: frontend-101-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百零一日

## HTML

### **Question:** `accesskey`，举例说明它有什么运用场景

- 毎日のフロントエンド 80

## CSS

### **Question:** 前端二倍图的理解？移动端使用二倍图比一倍图有什么好处

- 一倍图：当这个比率为 1:1 时，使用 1 个设备像素显示 1 个 CSS 像素。
- 二倍图：当这个比率为 2:1 时，使用 4 个设备像素显示 1 个 CSS 像素，
- 三倍图：当这个比率为 3:1 时，使用 9（3\*3）个设备像素显示 1 个 CSS 像素。

## JavaScript

### **Question:** 防抖和节流 again

- 防抖：动作绑定事件，动作发生后一定时间后触发事件，在这段时间内，如果该动作又发生，则重新等待一定时间再触发事件。

```js
function debounce(func, time) {
  let timer = null;
  return () => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      func.apply(this, arguments);
    }, time);
  };
}
```

- 节流： 动作绑定事件，动作发生后一段时间后触发事件，在这段时间内，如果动作又发生，则无视该动作，直到事件执行完后，才能重新触发。

```js
function throtte(func, time) {
  let activeTime = 0;
  return () => {
    const current = Date.now();
    if (current - activeTime > time) {
      func.apply(this, arguments);
      activeTime = Date.now();
    }
  };
}
```

### **Question:** 模块化 again

#### `CommonJS`

- 特点: `require`、`module.exports`、`exports`
- CommonJS 一般用在服务端或者 Node 用来同步加载模块，它对于模块的依赖发生在代码运行阶段，**不适合在浏览器端做异步加载**。 exports 实际上是一个对 module.exports 的引用:

```js
exports.add = function add() {
  /* 方法 */
};
// 等同于
module.exports.add = function add() {
  /* 方法 */
};
```

#### `ESModule`

- `import`、`export` **ES6 模块化不是对象**
- `import`会在 JavaScript 引擎**静态分析**，在编译时就引入模块代码，而并非在代码运行时加载，因此也不适合异步加载。 在 HTML 中如果要引入模块需要使用

```html
<script type="module" src="./module.js"></script>
```

- ESModule 的优势：

1. 可以用静态分析工具检测出哪些模块没有被调用过： 通过静态分析可以在打包时去掉这些未曾使用过的模块，以减小打包资源体积。
2. 模块变量类型检查。 ES6 Module 的静态模块结构有助于确保模块之间传递的值或接口类型是正确的。
3. 编译器优化。在 CommonJS 等动态模块系统中，无论采用哪种方式，本质上导入的都是一个对象，而 ES6 Module 支持直接导入变量，减少了引用层级，程序效率更高。

#### `CommonJS` `ESM`差异

1. CommonJS 模块引用后是一个值的拷贝，而 ESModule 引用后是一个值的动态映射，并且这个映射是只读的。
   - CommonJS 模块输出的是值的拷贝，一旦输出之后，无论模块内部怎么变化，都无法影响之前的引用。
   - ESModule 是引擎会在遇到 import 后生成一个引用链接，在脚本真正执行时才会根据这个引用链接去模块里面取值，模块内部的原始值变了 import 加载的模块也会变。
2. CommonJS 运行时加载，ESModule 编译阶段引用。
   - CommonJS 在引入时是加载整个模块，生成一个对象，然后再从这个生成的对象上读取方法和属性。
   - ESModule 不是对象，而是通过 export 暴露出要输出的代码块，在 import 时使用静态命令的方法引用指定的输出代码块，并在 import 语句处执行这个要输出的代码，而不是直接加载整个模块。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[模块化 | ProcessOn 免费在线作图,在线流程图,在线思维导图](https://www.processon.com/view/link/5c8409bbe4b02b2ce492286a#map)
