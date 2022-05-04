---
title: '毎日のフロントエンド 223'
date: 2022-05-03T14:30:23+09:00
description: frontend 每日一练
categories:
  - Tips
---

## Tips

### `tsconfig.json` 配置

> The presence of a `tsconfig.json` file in a directory indicates that the directory is the root of a TypeScript project. The tsconfig.json file specifies the root files and the compiler options required to compile the project.

#### `compilerOptions`

##### **编译选项**是 TypeScript 配置的核心部分，`compilerOptions` 内的配置根据功能可以分为:

1. `target`

   - target 选项用来指定 TypeScript 编译代码的目标，不同的目标将影响代码中使用的特性是否会被降级
   - target 的可选值包括 ES3、ES5、ES6、ES7、ES2017、ES2018、ES2019、ES2020、ESNext
   - target 的默认值为 **ES3**，如果不配置选项的话，代码中使用的 ES6 特性，_比如箭头函数会被转换成等价的函数表达式_

2. `module`

   - module 选项可以用来设置 TypeScript 代码所使用的**模块系统**
   - 如果 target 的值设置为 ES3、ES5 ，那么 module 的默认值则为 CommonJS；
   - 如果 target 的值为 ES6 或者更高，那么 module 的默认值则为 ES6
   - module 还支持 ES2020、UMD、AMD、System、ESNext、None 的选项

3. `jsx`

   - jsx 选项用来控制 jsx 文件转译成 JavaScript 的输出方式。该选项只影响.tsx 文件的 JS 文件输出，并且没有默认值选项
     - react: 将 jsx 改为等价的对 React.createElement 的调用，并生成 .js 文件
     - react-jsx: 改为 `__jsx` 调用，并生成 .js 文件
     - react-jsxdev: 改为 `__jsx` 调用，并生成 .js 文件
     - preserve: 不对 jsx 进行改变，并生成 .jsx 文件
     - react-native: 不对 jsx 进行改变，并生成 .js 文件

4. `incremental`

   - incremental 选项用来表示是否启动增量编译。incremental 为 true 时，则会将上次编译的工程图信息保存到磁盘上的文件中

5. `declaration`

   - declaration 选项用来表示是否为项目中的 TypeScript 或 JavaScript 文件生成 .d.ts 文件，这些 .d.ts 文件描述了模块导出的 API 类型

6. `sourceMap`

   - sourceMap 选项用来表示是否生成 sourcemap 文件，这些文件允许调试器和其他工具在使用实际生成的 JavaScript 文件时，显示原始的 TypeScript 代码
   - Source map 文件以 `.js.map` （或 `.jsx.map`）文件的形式被生成到与 .js 文件相对应的同一个目录下

7. `lib`

   - lib 配置项允许更细粒度地控制代码运行时的库定义文件，比如说 Node.js 程序，由于并不依赖浏览器环境，因此不需要包含 DOM 类型定义；而如果需要使用一些最新的、高级 ES 特性，则需要包含 ESNext 类型

#### 严格模式 `strict`

- 开启 strict 选项时，一般会同时开启一系列的类型检查选项，以便更好地保证程序的正确性
- strict 为 true 时，一般会开启以下编译配置:
  - `alwaysStrict`：保证编译出的文件是 ECMAScript 的严格模式，并且每个文件的头部会添加 'use strict'
  - `strictNullChecks`：更严格地检查 `null` 和 `undefined` 类型，比如数组的 `find` 方法的返回类型将是更严格的 `T | undefined`
  - `strictBindCallApply`：更严格地检查 `call`、`bind`、`apply` 函数的调用，比如会检查参数的类型与函数类型是否一致
  - `strictFunctionTypes`：更严格地检查函数参数类型和类型兼容性
  - `strictPropertyInitialization`：更严格地检查类属性初始化，如果类的属性没有初始化，则会提示错误
  - `noImplicitAny`：禁止隐式 any 类型，需要显式指定类型。TypeScript 在不能根据上下文推断出类型时，会回退到 any 类型
  - `noImplicitThis`：禁止隐式 this 类型，需要显示指定 this 的类型

#### 额外检查

- `noImplicitReturns`：禁止隐式返回。如果代码的逻辑分支中有返回，则所有的逻辑分支都应该有返回
- `noUnusedLocals`：禁止未使用的本地变量。如果一个本地变量声明未被使用，则会抛出错误
- `noUnusedParameters`：禁止未使用的函数参数。如果函数的参数未被使用，则会抛出错误
- `noFallthroughCasesInSwitch`：禁止 switch 语句中的穿透的情况。开启 noFallthroughCasesInSwitch 后，如果 switch 语句的流程分支中没有 break 或 return ，则会抛出错误，从而避免了意外的 swtich 判断穿透导致的问题

#### 模块解析 `moduleResolution`

> 模块解析部分的编译配置会影响代码中模块导入以及编译相关的配置

- `moduleResolution` 用来指定模块解析策略
- module 配置值为 AMD、UMD、System、ES6 时，moduleResolution 默认为 classic，否则为 node。在目前的新代码中，一般都是使用 node，而不使用 classic

#### `baseUrl`

- `baseUrl` 指的是基准目录，用来设置解析非绝对路径模块名时的基准目录。比如设置 baseUrl 为 `'./'` 时，TypeScript 将会从 tsconfig.json 所在的目录开始查找文件

#### `paths`

- `paths` 指的是路径设置，用来将模块路径重新映射到相对于 baseUrl 定位的其他路径配置。这里可以将 paths 理解为 webpack 的 alias 别名配置

```json
{
  "compilerOptions": {
    "paths": {
      "@src/*": ["src/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```

> 因为 paths 中配置的别名仅在类型检测时生效，所以在使用 tsc 转译或者 webpack 构建 TypeScript 代码时，需要引入额外的插件将源码中的别名替换成正确的相对路径

#### `rootDirs`

- `rootDirs` 可以指定多个目录作为根目录。这将允许编译器在这些“虚拟”目录中解析相对应的模块导入，就像它们被合并到同一目录中一样

#### `typeRoots`

- typeRoots 用来指定类型文件的根目录
- 在默认情况下，所有 `node_modules/@types` 中的任何包都被认为是可见的。如果手动指定了 typeRoots ，则仅会从指定的目录里查找类型文件

#### `types`

- 在默认情况下，所有的 typeRoots 包都将被包含在编译过程中
- 手动指定 types 时，只有列出的包才会被包含在全局范围内，如下示例：

```json
{
  "compilerOptions": {
    "types": ["node", "jest", "express"]
  }
}
```

#### `allowSyntheticDefaultImports`

- `allowSyntheticDefaultImports` 允许合成默认导出
- 当 `allowSyntheticDefaultImports` 设置为 true，即使一个模块没有默认导出（export default），我们也可以在其他模块中像导入包含默认导出模块一样的方式导入这个模块，如下示例：

```js
// allowSyntheticDefaultImports: true 可以使用
import React from 'react';
// allowSyntheticDefaultImports: false
import * as React from 'react';
```

> 在上面的示例中，对于没有默认导出的模块 react，如果设置了 allowSyntheticDefaultImports 为 true，则可以直接通过 import 导入 react；但如果设置 allowSyntheticDefaultImports 为 false，则需要通过 `import * as` 导入 react

#### `esModuleInterop`

- esModuleInterop 指的是 ES 模块的互操作性
- 在默认情况下，TypeScript 像 ES6 模块一样对待 CommonJS / AMD / UMD，但是此时的 TypeScript 代码转移会导致不符合 ES6 模块规范。不过，开启 esModuleInterop 后，这些问题都将得到修复
- 一般情况下，在启用 esModuleInterop 时，将同时启用 allowSyntheticDefaultImports。

#### Source Maps

- 为了支持丰富的调试工具，并为开发人员提供有意义的崩溃报告，TypeScript 支持生成符合 JavaScript Source Map 标准的附加文件（即 .map 文件）

#### `sourceRoot`

- sourceRoot 用来指定调试器需要定位的 TypeScript 文件位置，而不是相对于源文件的路径
- sourceRoot 的取值可以是路径或者 URL

#### `mapRoot`

- mapRoot 用来指定调试器需要定位的 source map 文件的位置，而不是生成的文件位置

#### `inlineSourceMap`

- 开启 inlineSourceMap 选项时，将不会生成 .js.map 文件，而是将 source map 文件内容生成内联字符串写入对应的 .js 文件中。虽然这样会生成较大的 JS 文件，但是在不支持 .map 调试的环境下将会很方便

#### `inlineSources`

- 开启 inlineSources 选项时，将会把源文件的所有内容生成内联字符串并写入 source map 中。这个选项的用途和 inlineSourceMap 是一样的

## Reference

- [Use a source map — Firefox Source Docs documentation](https://firefox-source-docs.mozilla.org/devtools-user/debugger/how_to/use_a_source_map/index.html)

- [TypeScript: TSConfig Reference - Docs on every TSConfig option](https://www.typescriptlang.org/tsconfig)
