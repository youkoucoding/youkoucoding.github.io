---
title: '毎日のフロントエンド 193'
date: 2022-03-31T11:01:04+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - Tips
---

## HTML

### `dl`、`ul`、`ol`三个标签的区别

- `dl` 标签就是定义列表，英文的单词是 definition list
  - definition title `dt` 列表的标题 这个标签是必须要的
  - definition description 列表的列表项，如果不需要它，可以不加 `dd`
  - **dt、dd 只能在 dl 里面；dl 里面只能有 dt、dd。**

```html
<dl>
  <dt>Firefox</dt>
  <dd>
    A free, open source, cross-platform, graphical web browser developed by the Mozilla Corporation
    and hundreds of volunteers.
  </dd>
  <!-- other terms and definitions -->
</dl>
```

- `ul`标签 无序列表 unordered list

  - 无序列表中的每一项是 `li` 标签 `li：list item`，“列表项”的意思

- `ol`标签 有序列表 ol 英文单词：Ordered List

## CSS

### 为什么 css 的 reset 不建议直接这么写：`*{ margin:0; padding:0;}`

- `*`是通配符，需要把所有的标签都遍历一遍，当网站较大时，样式比较多，这样写就大大的加强了网站运行的负载，会使网站加载的时候需要很长一段时间，因此一般大型的网站都有分层次的一套初始化样式

## Tips

### 脚手架工具 Scaffold

准备工作：

1. 首先需要有 package.json，它是 npm 依赖管理体系下的基础配置文件。
2. 然后选择使用 npm 或 Yarn 作为包管理器，这会在项目里添加上对应的 lock 文件，来确保在不同环境下部署项目时的依赖稳定性。
3. 确定项目技术栈，使用哪一种数据流模块？是否使用 TypeScript？使用哪种 CSS 预处理器？等等。在明确选择后安装相关依赖包并在 src 目录中建立入口源码文件。
4. 选择构建工具，目前来说，构建工具的主流选择还是 webpack （除非项目已先锋性地考虑尝试 nobundle 方案），对应项目里就需要增加相关的 webpack 配置文件，可以考虑针对开发/生产环境使用不同配置文件。
5. 打通构建流程，通过安装与配置各种 Loader 、插件和其他配置项，来确保开发和生产环境能正常构建代码和预览效果。
6. 优化构建流程，针对开发/生产环境的不同特点进行各自优化。例如，开发环境更关注构建效率和调试体验，而生产环境更关注访问性能等。
7. 选择和调试辅助工具，例如代码检查工具和单元测试工具，安装相应依赖并调试配置文件。
8. 最后是收尾工作，检查各主要环节的脚本是否工作正常，编写说明文档 README.md，将不需要纳入版本管理的文件目录记入 .gitignore 等。

```shell
package.json         1. npm 项目文件
package-lock.json    2. npm 依赖 lock 文件
public/              3. 预设的静态目录
src/                 3. 源代码目录
  main.ts            3. 源代码中的初始入口文件
  router.ts          3. 源代码中的路由文件
  store/             3. 源代码中的数据流模块目录
webpack/             4. webpack 配置目录
  common.config.js   4. webpack 通用配置文件
  dev.config.js      4. webpack 开发环境配置文件
  prod.config.js     4. webpack 生产环境配置文件
.browserlistrc       5. 浏览器兼容描述 browserlist 配置文件
babel.config.js      5. ES 转换工具 babel 配置文件
tsconfig.json        5. TypeScript 配置文件
postcss.config.js    5. CSS 后处理工具 postcss 配置文件
.eslintrc            7. 代码检查工具 eslint 配置文件
jest.config.js       7. 单元测试工具 jest 配置文件
.gitignore           8. Git 忽略配置文件
README.md            8. 默认文档文件
```

---

- `Create React App` 是 Facebook 官方提供的 React 开发工具集。它包含了 create-react-app 和 react-scripts 两个基础包。其中 create-react-app 用于选择脚手架创建项目，而 react-scripts 则作为所创建项目中的运行时依赖包，提供了封装后的项目启动、编译、测试等基础工具。
  - 将一个项目开发运行时的各种配置细节完全封装在了一个 react-scripts 依赖包中，这大大降低了开发者，尤其是对 webpack 等构建工具不太熟悉的开发者上手开发项目的学习成本，也降低了开发者自行管理各配置依赖包的版本所需的额外测试成本。

---

- `Vue CLI` 工具在设计上吸取了 CRA 工具的教训，在保留了创建项目开箱即用的优点的同时，提供了用于覆盖修改原有配置的自定义构建配置文件和其他工具配置文件。
- 在创建项目的流程中，Vue CLI 也提供了通过用户交互自行选择的一些定制化选项，例如是否集成路由、TypeScript 等，使开发者更有可能依据这些选项来生成更适合自己的初始化项目，降低了开发者寻找模板或单独配置的成本。

---

- [Vite](https://vitejs.dev/)

#### 如何了解一个脚手架

- `webpack loaders`
- `webpack plugins`
- `webpack.optimize`
- `webpack resolve`

#### 如何定制一个脚手架模板

1. 为项目引入新的通用特性。
2. 针对构建环节的 webpack 配置优化，来提升开发环境的效率和生产环境的性能等。
3. 定制符合团队内部规范的代码检测规则配置。
4. 定制单元测试等辅助工具模块的配置项。
5. 定制符合团队内部规范的目录结构与通用业务模块，例如业务组件库、辅助工具类、页面模板等。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<dl>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/dl)

[Utilizing the Underused (But Semantically Awesome) Definition List | CSS-Tricks - CSS-Tricks](https://css-tricks.com/utilizing-the-underused-but-semantically-awesome-definition-list/)

[Adding <em>React Fast Refresh</em> to Your Create React App Project | Things I've Learnt](https://dutzi.party/react-fast-refresh/)
