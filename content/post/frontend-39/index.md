---
title: '毎日のフロントエンド　39'
date: 2021-10-25T10:58:32+09:00
description: frontend 每日一练
image: frontend-39-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第三十九日

## HTML

### **Question:** `title` 与 `h1`、`b` 与 `strong`、`i` 与 `em` 的区别分别是什么？

- `title`标签写在`body`里面不会被渲染,只能写在`head`里面,对网站 SEO 比较重要
- `h1`标签写在 body 里面,但是写在 head 里(不推荐),渲染的时候会自动渲染到 body 里面去
- `b`标签与`strong`标签在表现上是一样的,都自带`font-weight: bold`属性
  - `b` 仅表示加粗既装饰用，我们应该使用 `CSS` 而不应该使用 `b`
- `i`标签与`em`标签在表现上是一样的,都自带`font-style: italic`属性
  - `i` 用于斜体展示，我们应该使用 `CSS` 而不应该使用 `i`
- `b`标签与`i`标签是物理标记,告诉浏览器以何种格式显示文字
- `strong`标签与`em`标签是逻辑标记,逻辑元素告诉浏览器这些文字有怎么样的重要性

## CSS

### **Question:** CSS 水平和垂直居中的方法

1. 绝对布局

```css
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
flexbox
```

2. flexbox

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

## JavaScript

### **Question:** 说说对模块化的理解

`Javascipt` 模块化

1. Problem

- 命名问题：所有文件的方法都挂载到`window/global`上，会污染全局环境，并且需要考虑命名冲突问题
- 依赖问题：`script`是顺序加载的，如果各个文件文件有依赖，就得考虑 js 文件的加载顺序
- 网络问题：如果 js 文件过多，所需请求次数就会增多，增加加载时间

2. 4 种规范: `CommonJS`、`AMD`、`UMD`、`ESM`

#### CommonJS

`CommonJS`是一个更偏向于服务器端的规范。`NodeJS`采用了这个规范。`CommonJS`的一个模块就是一个脚本文件。`require` **命令第一次加载该脚本时就会执行整个脚本，然后在内存中生成一个对象。**

`CommonJS`规范代表`node.js`的模块系统，在`node`里面实现了`moduel`、`exports`、`require`、`global`四个变量

- 允许模块以`require`的方式同步加载其他模块,然后通过`exports`和`module.exports`的形式向外暴露接口。
- 使用方式如下，如果只是对外暴露一些属性或者方法用`exports`，如果要暴露一个对象（包含属性和方法）则使用`module.exports`

```js
//导入
require('module');
require('./app.js');

//导出
exports.getSomethInfo = function () {};
module.exports = { someValue, someFunction };
```

- 优点

  - 简单易用
  - 服务端的模块便于复用

- 缺点
  - 同步加载在浏览器端不适用，会阻碍加载，浏览器资源是异步加载的
  - 不能非阻塞并行加载多个模块

#### AMD`Asynchronous Module Definition `（异步加载模块）

采用异步方式加载模块，模块的加载不影响后面语句的运行。所有依赖模块的语句，都定义在一个回调函数中，等到加载完成之后，回调函数才执行。

```js
require([module], callback); // AMD也采用require命令加载模块，但是不同于CommonJS，它要求两个参数
```

- 第一个参数`[module]`，是一个数组，里面的成员是要加载的模块，`callback`是加载完成后的回调函数，回调函数中参数对应数组中的成员（模块）。

```js
 // 定义
define("module", ["dep1", "dep2"], function(d1, d2) {...});
// 加载模块
require(["module", "../app"], function(module, app) {...});
```

- 优点
  - 适合在浏览器环境中异步加载模块
  - 可以并行加载多个模块
- 缺点

  - 提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不顺畅
  - 不符合通用的模块化思维方式，是一种妥协的实现

- AMD 规范代表 `requirejs`[^1]

[^1]: [RequireJS](https://requirejs.org/)

#### UMD 规范（异步加载模块）(`Universal Module Definition`)

所谓的通用，就是兼容了 `CmmonJS` 和 `AMD` 规范，这意味着无论是在 `CommonJS` 规范的项目中，还是 `AMD` 规范的项目中，都可以直接引用 `UMD` 规范的模块使用。

原理其实就是在模块中去判断全局是否存在 exports 和 define，如果存在 exports，那么以 CommonJS 的方式暴露模块，如果存在 define 那么以 AMD 的方式暴露模块:

```js
(function (root, factory) {
  if (typeof define === "function" && define.amd) {
    define(["jquery", "underscore"], factory);
  } else if (typeof exports === "object") {
    module.exports = factory(require("jquery"), require("underscore"));
  } else {
    root.Requester = factory(root.$, root._);
  }
}(this, function ($, _) {
  // this is where I defined my module implementation
  const Requester = { // ... };
  return Requester;
}));
```

#### CMD 规范（异步加载模块）

- CMD 规范和 AMD 很相似，简单，并与 CommonJS 和 Node.js 的 Modules 规范保持了很大的兼容性；在 CMD 规范中，一个模块就是一个文件。
- 定义模块使用全局函数 define，其接收 factory 参数，factory 可以是一个函数，也可以是一个对象或字符串；
- `factory` 是一个函数，有三个参数，`function(require, exports, module)`：
  - `require` 是一个方法，接受模块标识作为唯一参数，用来获取其他模块提供的接口：`require(id)`
  - `exports` 是一个对象，用来向外提供模块接口
  - `module` 是一个对象，上面存储了与当前模块相关联的一些属性和方法

```js
define(function (require, exports, module) {
  var a = require('./a');
  a.doSomething();
  // 依赖就近书写，什么时候用到什么时候引入
  var b = require('./b');
  b.doSomething();
});
```

#### `ES6`模块化

ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 `CommonJS` 和 `AMD` 规范，成为浏览器和服务器通用的模块解决方案

```js
 // 导入
import "/app";
import React from “react”;
import { Component } from “react”;
// 导出
export function multiply() {...};
export var year = 2018;
export default ...
...
```

#### `require`与`import`的区别

`require`使用与`CommonJs`规范，`import`使用于 Es6 模块规范；所以两者的区别实质是两种规范的区别；

- **CommonJS**

  - 对于基本数据类型，属于复制。即会被模块缓存；同时，在另一个模块可以对该模块输出的变量重新赋值。
  - 对于复杂数据类型，属于浅拷贝。由于两个模块引用的对象指向同一个内存空间，因此对该模块的值做修改时会影响另一个模块。
  - 当使用 require 命令加载某个模块时，就会运行整个模块的代码。
  - 当使用 require 命令加载同一个模块时，不会再执行该模块，而是取到缓存之中的值。也就是说，- CommonJS 模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。
  - 循环加载时，属于加载时执行。即脚本代码在 require 的时候，就会全部执行。一旦出现某个模块被”循环加载”，就只输出已经执行的部分，还未执行的部分不会输出。

- **ES6 模块**
  - 对于只读来说，即不允许修改引入变量的值，import 的变量是只读的，不论是基本数据类型还是复杂数据类型。当模块遇到 import 命令时，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。
  - 对于动态来说，原始值发生变化，import 加载的值也会发生变化。不论是基本数据类型还是复杂数据类型。
  - 循环加载时，ES6 模块是动态引用。只要两个模块之间存在某个引用，代码就能够执行。

**`require/exports` 是必要通用且必须的；因为事实上，目前编写的 `import/export` 最终都是编译为 `require/exports` 来执行的。**

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[文字を強調するタグ strong・b・em・i の違いと SEO 効果 | なんでものびる WEB](https://nandemo-nobiru.com/2096/)

[Js 前端模块化规范 ](https://www.bozaigao.net/2020/07/21/js_standard/)

[Javascript 模块化详解 - SegmentFault 思否](https://segmentfault.com/a/1190000039375332)
