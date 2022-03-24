---
title: '毎日のフロントエンド 186'
date: 2022-03-24T14:22:03+09:00
description: frontend 每日一练
categories:
  - Tips
---

## HTML

### 如何实现浏览器桌面通知

- `Websocket` Desktop Notification

```JavaScript
Notification.requestPermission(function (perm) {
        if (perm == "granted") {
            let notification = new Notification("通知", {
                dir: "auto",
                lang: "hi",
                tag: "testTag",
                icon: "https://static.cnblogs.com/images/adminlogo.gif",
                body: "content"
            });
        }
})
```

## CSS

### 相邻兄选择器

- 相邻兄弟选择器 (`+`) 介于两个选择器之间，当第二个元素紧跟在第一个元素之后，并且两个元素都是属于同一个父元素的子元素，则第二个元素将被选中

```CSS
/* 图片后面紧跟着的段落将被选中 */
img + p {
  font-style: bold;
}
```

## Tips

### 组件通信 - 状态管理

#### 全局状态

- 父子组件通信
  - 父组件通过 prop(s) 属性向子组件传参，子组件通过自定义事件来向父组件发送消息

1. 状态管理库

- 状态管理库具有 3 个特点：**可预测**、**中心化**、**可调式**。

#### 其他组件通信方式

1. 全局上下文
2. 事件监听

### 代码编译之`webpack`

- webpack 有两个执行入口，分别是:
  - 通过命令行调用的 `bin/webpack.js`
  - 以及直接在代码中引用的 `lib/webpack.js`

```JavaScript
// lib/webpack.js 部分源码
const webpack = (( options, callback ) => {
  validateSchema(webpackOptionsSchema, options);
  let compiler;
  compiler = createCompiler(options);
  if (callback) {
    compiler.run((err, stats) => {
      compiler.close(err2 => {
        callback(err || err2, stats);
      });
    });
  }
  return compiler;
});
```

- `webpack()` 函数内部有 3 个重要的操作：**校验配置项**、**创建编译器**、**执行编译**。

#### 校验配置项

- 校验配置项是通过调用 `validateSchema()` 函数来实现的，这个函数的内部是调用的 schema-utils 模块的 `validate ()` 函数 ，`validate()` 函数支持通过 JSONSchema 规则来校验 json 对象。

- JSONSchema 规则保存在 schemas/WebpackOptions.json 文件中，对应代码中的 webpackOptionsSchema 变量。

- JSONSchema，它是通过 JSON 文件来描述 JSON 文件 ，可以用来校验 JSON 对象

> `validateSchema()` 函数通过 `JSONSchema` 对 `options` 进行校验，如果不符合配置规则，则退出并在控制台输出格式化的错误信息。这样就能避免因为选项参数不正确而导致程序运行出错。

#### 创建编译器

- 创建编译器操作是在 `compiler.compile()` 函数中调用 `createCompiler()` 函数来实现的，该函数会返回一个 Compiler 实例。createCompiler() 函数源码如下：

```js
// lib/webpack.js
const createCompiler = (rawOptions) => {
  const options = getNormalizedWebpackOptions(rawOptions);
  applyWebpackOptionsBaseDefaults(options);
  const compiler = new Compiler(options.context);
  compiler.options = options;
  new NodeEnvironmentPlugin({
    infrastructureLogging: options.infrastructureLogging,
  }).apply(compiler);
  if (Array.isArray(options.plugins)) {
    for (const plugin of options.plugins) {
      if (typeof plugin === 'function') {
        plugin.call(compiler, compiler);
      } else {
        plugin.apply(compiler);
      }
    }
  }
  applyWebpackOptionsDefaults(options);
  compiler.hooks.environment.call();
  compiler.hooks.afterEnvironment.call();
  new WebpackOptionsApply().process(options, compiler);
  compiler.hooks.initialize.call();
  return compiler;
};
```

- 在 createCompiler() 函数内部，首先会通过 `getNormalizedWebpackOptions()` 函数将默认的配置参数与自定义的配置参数 rawOptions 进行合成，赋值给变量 options。
- `applyWebpackOptionsBaseDefaults()` 函数则将程序当前执行路径赋值给 options.context 属性。

- 经过以上处理之后，变量 options 才会作为参数传递给类 Compiler 来生成实例。在构造函数中，实例的很多属性进行了初始化操作，其中比较重要的是 hooks 属性。下面是截取的部分源码：

```js
// lib/Compiler.js
constructor(context) {
    this.hooks = Object.freeze({
      initialize: new SyncHook([]),
      shouldEmit: new SyncBailHook(["compilation"]),
      done: new AsyncSeriesHook(["stats"]),
      afterDone: new SyncHook(["stats"]),
      additionalPass: new AsyncSeriesHook([]),
      beforeRun: new AsyncSeriesHook(["compiler"]),
      run: new AsyncSeriesHook(["compiler"]),
      emit: new AsyncSeriesHook(["compilation"]),
      assetEmitted: new AsyncSeriesHook(["file", "info"]),
      afterEmit: new AsyncSeriesHook(["compilation"]),
      thisCompilation: new SyncHook(["compilation", "params"]),
      compilation: new SyncHook(["compilation", "params"]),
      normalModuleFactory: new SyncHook(["normalModuleFactory"]),
      contextModuleFactory: new SyncHook(["contextModuleFactory"]),
      beforeCompile: new AsyncSeriesHook(["params"]),
      compile: new SyncHook(["params"]),
      make: new AsyncParallelHook(["compilation"]),
      finishMake: new AsyncSeriesHook(["compilation"]),
      afterCompile: new AsyncSeriesHook(["compilation"]),
      watchRun: new AsyncSeriesHook(["compiler"]),
      failed: new SyncHook(["error"]),
      invalid: new SyncHook(["filename", "changeTime"]),
      watchClose: new SyncHook([]),
      infrastructureLog: new SyncBailHook(["origin", "type", "args"]),
      environment: new SyncHook([]),
      afterEnvironment: new SyncHook([]),
      afterPlugins: new SyncHook(["compiler"]),
      afterResolvers: new SyncHook(["compiler"]),
      entryOption: new SyncBailHook(["context", "entry"])
    });
}
```

- 为了防止 hooks 属性被修改，这里使用 `Object.freeze()` 函数来创建对象。

  - `object.freeze()` 函数冻结一个对象。
    - 一个被冻结的对象再也不能被修改了；
    - 冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。
    - 冻结一个对象后该对象的原型也不能被修改

- 一共创建了 4 种类型的`hook`，它们的名称和作用如下：

  - **`SyncHook`**，当 hook 触发时，会依次调用 hook 队列中的回调函数；
  - **`SyncBailHook`**，当 hook 触发时，会依次调用 hook 队列中的回调函数，如果遇到有返回值的函数则停止继续调用；
  - **`AsyncSeriesHook`**，如果 hook 队列中有异步回调函数，则会等其执行完成后再执行剩余的回调函数；
  - **`AsyncParallelHook`** 可以异步执行钩子队列中的所有异步回调函数

- 下面一段代码是 hook 函数的简单用法。通过 new 关键字创建 hook 实例，然后调用 `tap()` 函数来监听 hook，向 hook 的队列中添加一个回调函数 。 当执行 `hook.call()` 函数时，会依次调用队列中的回调函数，并将参数传递给这些回调函数 。 需要注意的是， 参数的数量必须与实例化的数组长度一致。在下面的例子中，只能传递 1 个参数。

- [tapable - npm](https://www.npmjs.com/package/tapable)

```js
const { SyncHook } = require('tapable');
const hook = new SyncHook(['whatever']);
hook.tap('1', function (arg1) {
  console.log(arg1);
});
hook.call('lagou');
```

---

```js
// 调用插件（plugin）的 apply() 函数的写法在 webpack 中很常见
// 主要作用就是监听 compiler hook事件，或者说是向hook队列中插入一个回调函数
// 当对应的hook事件触发时调用
// lib/webpack.js
new NodeEnvironmentPlugin({
  infrastructureLogging: options.infrastructureLogging,
}).apply(compiler);
```

- 初始化完成后会调用 3 个 hook 事件

```js
// lib/webpack.js
compiler.hooks.environment.call();
compiler.hooks.afterEnvironment.call();
new WebpackOptionsApply().process(options, compiler);
compiler.hooks.initialize.call();
```

- process() 函数会根据不同的执行环境引入一些默认的插件并调用它的 apply() 函数，比如 Node 环境下会引入下面的插件：

```js
// lib/WebpackOptionsApply.js
const NodeTemplatePlugin = require('./node/NodeTemplatePlugin');
const ReadFileCompileWasmPlugin = require('./node/ReadFileCompileWasmPlugin');
const ReadFileCompileAsyncWasmPlugin = require('./node/ReadFileCompileAsyncWasmPlugin');
const NodeTargetPlugin = require('./node/NodeTargetPlugin');
new NodeTemplatePlugin({
  asyncChunkLoading: options.target === 'async-node',
}).apply(compiler);
new ReadFileCompileWasmPlugin({
  mangleImports: options.optimization.mangleWasmImports,
}).apply(compiler);
new ReadFileCompileAsyncWasmPlugin().apply(compiler);
new NodeTargetPlugin().apply(compiler);
new LoaderTargetPlugin('node').apply(compiler);
```

---

- 创建编译器步骤的主要逻辑:
  - 首先会将配置参数进行修改，比如加入一些默认配置项；
  - 然后创建一个编译器实例 compiler，这个实例的构造函数会初始化一些 hook；
  - 最后就是调用插件的 apply() 函数来监听 hook，同时也会主动触发一些 hook 事件

#### 执行编译

- 调用 `compiler.compile()` 函数进入编译阶段

```js
// compile() 函数的部分代码
// lib/Compiler.js
compile(callback) {
  const params = this.newCompilationParams();
  this.hooks.beforeCompile.callAsync(params, err => {
    if (err) return callback(err);
    this.hooks.compile.call(params);
    const compilation = this.newCompilation(params);
    this.hooks.make.callAsync(compilation, err => {
    })
  })
}
```

- 首先是触发了 `compiler.hooks.compile` hook，触发后，一些插件将进行初始化操作，为编译做好准备，比如 LoaderTargetPlugin 插件将会加载需要的加载器。
- 调用 `newCompilation()` 函数则会创建了一个 Compilation 实例。
  - 这里的 Compilation 和前面创建的 Compiler 是有区别的：Compiler 是全局唯一的，包含了配置参数、加载器、插件这些信息，它会一直存在 webpack 的生命周期中；而 Compilation 包含了当前模块的信息，只是代表一次编译过程。
- 在创建 compilation 完成后会触发 compiler.hooks.thisCompilation 和 compiler.hooks.compilation，激活 JavaScriptModulesPlugin 插件的监听函数，从而加载 JavaScript 的解析模块 acorn

```js
// lib/Compiler.js
newCompilation(params) {
  const compilation = this.createCompilation();
  compilation.fileTimestamps = this.fileTimestamps;
  compilation.contextTimestamps = this.contextTimestamps;
  compilation.name = this.name;
  compilation.records = this.records;
  compilation.compilationDependencies = params.compilationDependencies;
  this.hooks.thisCompilation.call(compilation, params);
  this.hooks.compilation.call(compilation, params);
  return compilation;
}
```

- 在 `compiler.compile()` 函数中触发 `compiler.hooks.make` 标志着编译操作正式开始
- 模块构建完成后，通过 `acorn` 生成模块代码的抽象语法树，根据抽象语法树分析这个模块是否还有依赖的模块，如果有则继续解析每个依赖的模块，直到所有依赖解析完成，最后合并生成输出文件。这个过程和编译器执行过程类似

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[notification - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/notification)

[相邻兄弟选择器 - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator)
