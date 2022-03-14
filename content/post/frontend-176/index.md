---
title: '毎日のフロントエンド  176'
date: 2022-03-14T17:25:38+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## JavaScript

### 模块化

#### `ES6` 模块

- ES6 模块强制自动采用严格模式，所以说不管有没有“user strict”声明都是一样的，换言之，编写代码的时候不必再刻意声明了；
- 虽然大部分主流浏览器支持 ES6 模块，但是和引入普通 JS 的方式略有不同，需要在对应 script 标签中将属性 `type` 值设置为`module`才能被正确地解析为 ES6 模块；
- 在 Node.js 下使用 ES6 模块则需要将文件名后缀改为“.mjs”，用来和 Node.js 默认使用的 CommonJS 规范模块作区分

---

- ES6 模块有两个重要特性

  - **值引用** 指 `export` 语句输出的接口，与其对应的值是动态绑定关系。即通过该接口，可以取到模块内部实时的值，可以简单地理解为变量浅拷贝。
  - **静态分析** 指不需要执行代码，只从字面量上对代码进行分析。

```js
// 必须首部声明
let a = 1
import { app } from './app';
// 不允许使用变量或表达式
import { 'a' + 'p' + 'p' } from './app';
// 不允许被嵌入语句逻辑
if (moduleName === 'app') {
  import { init } from './app';
} else {
  import { init } from './bpp';
}
```

---

- `import` 的动态模块提案
  - 通过 `import()` 函数来支持动态引入模块。

```js
// 调用 import() 函数传入模块路径，得到一个 Promise 对象
import(`./section-modules/${link.dataset.entryModule}.js`)
  .then((module) => {
    module.loadPageInto(main);
  })
  .catch((err) => {
    main.textContent = err.message;
  });
```

- import() 函数违反了上面静态声明的所有要求，并且提供了其他更强大的功能特性：
  - 违反首部声明要求，意味着可以在代码运行时按需加载模块，这个特性就可以用于首屏优化，根据路由和组件只加载依赖的模块。
  - 违反变量或表达式要求，则意味着可以根据参数动态加载模块。
  - 违反嵌入语句逻辑规则，可想象空间更大，比如可以通过 Promise.race 方式同时加载多个模块，选择加载速度最优模块来使用，从而提升性能。

---

#### `CommonJS`

- CommonJS 规定每个文件就是一个模块，有独立的作用域。每个模块内部，都有一个 module 对象，代表当前模块。通过它来导出 API，它有以下属性：
  - `id` 模块的识别符，通常是带有绝对路径的模块文件名；
  - `filename` 模块的文件名，带有绝对路径；
  - `loaded` 返回一个布尔值，表示模块是否已经完成加载；
  - `parent` 返回一个对象，表示调用该模块的模块；
  - `children` 返回一个数组，表示该模块要用到的其他模块；
  - `exports` 表示模块对外输出的值。
- 引用模块则需要通过 `require` 函数，它的基本功能是，读入并执行一个 JS 文件，然后返回该模块的 `exports` 对象。
- CommonJS 特性和 ES6 恰恰相反，它采用的是 **值拷贝** 和 **动态声明**。
  - 值拷贝和值引用相反，一旦输出一个值，**模块内部的变化就影响不到这个值了，可以简单地理解为变量浅拷贝**
  - 动态声明，就是消除了静态声明的限制，可以“自由”地在表达式语句中引用模块

---

#### `AMD`

> 在 ES6 模块出现之前，AMD（Asynchronous Module Definition，异步模块定义）是一种很热门的浏览器模块化方案。

- AMD 规范只定义了一个全局函数 `define`，通过它就可以定义和引用模块，它有 3 个参数：

`define(id?, dependencies?, factory);`

1. `id` 为模块的名称，该参数是可选的。如果没有提供该参数，模块的名字应该默认为模块加载器请求的指定脚本的名字；如果提供了该参数，模块名必须是“顶级”的和绝对的（不允许相对名字）
2. `dependencies` 是个数组，它定义了所依赖的模块。依赖模块必须根据模块的工厂函数优先级执行，并且执行的结果应该按照依赖数组中的位置顺序以参数的形式传入（定义中模块的）工厂函数中。
3. `factory` 为模块初始化要执行的函数或对象。如果是函数，那么该函数是单例模式，只会被执行一次；如果是对象，此对象应该为模块的输出值。

```js
define('alpha', ['require', 'exports', 'beta'], function (require, exports, beta) {
  exports.verb = function () {
    return beta.verb();
  };
});
```

- 重要特性就是**异步加载**。指同时并发加载所依赖的模块，当所有依赖模块都加载完成之后，再执行当前模块的回调函数。这种加载方式和浏览器环境的性能需求刚好吻合。
- AMD 并不是浏览器原生支持的模块规范，所以需要借助第三方库来实现，其中最有名的就是[RequireJS](https://requirejs.org/)。 它的核心是两个全局函数 define 和 require，define 函数可以将依赖注入队列中，并将回调函数定义成模块；require 函数主要作用是创建 script 标签请求对应的模块，然后加载和执行模块。[源代码](https://requirejs.org/docs/release/2.3.6/comments/require.js)

---

#### `CMD`

> CMD（Common Module Definition，通用模块定义）是基于浏览器环境制定的模块规范。

- CMD 定义模块也是通过一个全局函数 define 来实现的，但只有一个参数，该参数既可以是函数也可以是对象：

`define(factory);`

- 如果这个参数是对象，那么模块导出的就是对象；如果这个参数为函数，那么这个函数会被传入 3 个参数 require 、 exports 和 module。

```js
define(function (require, exports, module) {
  //...
});
```

1. `require` 是一个函数，通过调用它可以引用其他模块，也可以调用 require.async 函数来异步调用模块。
2. `exports` 是一个对象，当定义模块的时候，需要通过向参数 exports 添加属性来导出模块 API。
3. `module` 是一个对象，它包含 3 个属性：
   - `uri`，模块完整的 URI 路径；
   - `dependencies`，模块的依赖；
   - `exports`，模块需要被导出的 API，作用同第二个参数 exports。

- 下面是一个简单的例子，定义了一个名为 increment 的模块，引用了 math 模块的 add 函数，经过封装后导出成 increment 函数。

```js
define(function (require, exports, module) {
  var add = require('math').add;
  exports.increment = function (val) {
    return add(val, 1);
  };
  module.id = 'increment';
});
```

- CMD 最大的特点就是懒加载，和上面示例代码一样，不需要在定义模块的时候声明依赖，可以在模块执行时动态加载依赖。当然还有一点不同，那就是 CMD 同时支持同步加载模块和异步加载模块。
  > [seajs/seajs: A Module Loader for the Web](https://github.com/seajs/seajs)

---

#### `UMD`

- UMD（Universal Module Definition，统一模块定义）并不是模块管理规范，而是带有前后端同构思想的模块封装工具。通过 UMD 可以在合适的环境选择对应的模块规范。比如在 Node.js 环境中采用 CommonJS 模块管理，在浏览器端且支持 AMD 的情况下采用 AMD 模块，否则导出为全局函数。

- 实现原理：
  - 先判断是否支持 Node.js 模块格式（exports 是否存在），存在则使用 Node.js 模块格式；
  - 再判断是否支持 AMD（define 是否存在），存在则使用 AMD 方式加载模块；
  - 若前两个都不存在，则将模块公开到全局（Window 或 Global）。

```js
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    define([], factory);
  } else if (typeof exports === 'object') {
    module.exports, (module.exports = factory());
  } else {
    root.returnExports = factory();
  }
})(this, function () {
  //。。。
  return {};
});
```

---

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
