---
title: '毎日のフロントエンド  168'
date: 2022-03-03T11:19:24+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## CSS

### 去掉点击 a 链接或者图片出现的边框

```css
a:active,
img:active {
  border: none;
  outline: none;
  text-decoration: none;
}
```

## Tips

### 解决在手机上长时间点击会选中图片

- 事件穿透

```css
img {
  pointer-events: none;
}
```

### 如何 css 组织样式文件

- [Sass Guidelines](https://sass-guidelin.es/#architecture)7-1 模式，建议将目录结构划分为 7 个目录和 1 个文件，这 1 个文件是样式的入口文件，它会将项目所用到的所有样式都引入进来，一般命名为 `main.scss`
  - `base/`，模板代码，比如默认标签样式重置；
  - `components/`，组件相关样式；
  - `layout/`，布局相关，包括头部、尾部、导航栏、侧边栏等；
  - `pages/`，页面相关样式；
  - `themes/`，主题样式，即使有的项目没有多个主题，也可以进行预留；
  - `abstracts/`，其他样式文件生成的依赖函数及 mixin，不能直接生成 css 样式；
  - `vendors/`，第三方样式文件。

---

- 上述文件结构优化版
  - main.scss 文件存在意义不大，页面样式、组件样式、布局样式都可以在页面和组件中引用，全局样式也可以在根组件中引用。而且每次添加、修改样式文件都需要在 main.scss 文件中同步，这种过度中心化的配置方式也不方便。
  - layout 目录也可以去除，因为像 footer、header 这些布局相关的样式，放入对应的组件中来引用会更好，至于不能被组件化的“`_grid`”样式存在性也不大。因为对于页面布局，既可以通过下面介绍的方法来拆分成全局样式，也可以依赖第三方 UI 库来实现。所以说这个目录可以去除。
  - themes/ 目录也可以去除，毕竟大部分前端项目是不需要设置主题的，即使有主题也可以新建一个样式文件来管理样式变量。
  - vendors/ 目录可以根据需求添加。因为将外部样式复制到项目中的情况比较少，更多的是通过 npm 来安装引入 UI 库或者通过 webpack 插件来写入对应的 cdn 地址。

```bash
# 优化后
src/
|
|– abstracts/
|   |– _variables.scss
|   |– _functions.scss
|   |– _mixins.scss
|   |– _placeholders.scss
|
|– base/
|   |– _reset.scss
|   |– _typography.scss
|   …
|
|– components/
|   |– _buttons.scss
|   |– _carousel.scss
|   |– _cover.scss
|   |– _dropdown.scss
|   |- header/
|      |- header.tsx
|      |- header.sass
|   |- footer/
|      |- footer.tsx
|      |- footer.sass
|   …
|
|– pages/
|   |– _home.scss
|   |– _contact.scss
|   …
|
```

#### 如何避免样式冲突

- [BEM — Block Element Modifier](http://getbem.com/)

  - BEM 是 Block、Element、Modifier 三个单词的缩写，Block 代表独立的功能组件，Element 代表功能组件的一个组成部分，Modifier 对应状态信息
  - 各个单词通过双横线（也可以用双下划线）连接（双横线虽然能和单词的连字符进行区分，但确实有些冗余，可以考虑直接用下划线代替）
  - BEM 的命名方式具有语义，很容易理解，非常适用于组件样式类

- 工具命名 `CSS Modules`
  - 一个问题:
    - 想在引用组件的同时，覆盖它的样式会变得困难，因为编译后的样式名是随机\

#### 如何高效复用样式

- 哪些样式规则可以设置为全局公共样式
  - 首先是具有枚举值的属性，除了上面提到的，还包括 cursor:pointer、float:left 等。
  - 其次是那些特定数值的样式属性值，比如 margin: 0、left: 0、height: 100%。
  - 最后是设计规范所使用的属性，比如设计稿中规定的几种颜色

#### CSS in JS

> [styled-components](https://styled-components.com/)

> [MicheleBertoli/css-in-js: React: CSS in JS techniques comparison](https://github.com/MicheleBertoli/css-in-js)

- 可以通过随机命名解决作用域问题，但命名规则和 CSS Modules 都可以解决这个问题；
- 样式可以使用 JavaScript 语言特性，比如函数、循环，实现元素不同的样式效果可以通过新建不同样式类，修改元素样式类来实现。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
