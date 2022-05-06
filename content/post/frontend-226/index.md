---
title: '毎日のフロントエンド 226(227)(228)(229)(230)'
date: 2022-05-06T22:16:05+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## HTML

### html 规范 - Tips

1. 标签名和属性推荐用小写
2. 标签都需闭合，不管是单标签还是双标签
3. 双标签不宜使用单标签闭合方式
4. 属性值需双引号
5. img 标签需加上 alt
6. img 标签推荐加上固定宽高
7. html 和 body 标签最好不好
8. 部分字符推荐转义，比如 <
9. link 写在 head 内，script 写在 body 内 末尾
10. 不推荐使用已废弃的标签和属性名，比如 marquee center 等
11. 推荐使用语义化标签
12. `meta`里设置有意义的`title`，`keyword`，`description`

## CSS

### css 规范 - Tips

1. 命名规范（连字符-分隔的字符串）
2. 文件宽度限制（每行 80 个字符）
3. 加注释
4. 编写选择器应有助于重用
5. 尽量不要加 `!important`
6. 避免使用 CSS 表达式
7. 选择`<link>` 舍弃 `@import`
8. 避免使用滤镜（IE 专有的 AlphaImageLoader 滤镜）
9. 把样式表放在顶部 `/` 把 CSS 放在外部文件
10. 压缩 CSS

### 不建议使用`@import`

- `@import` 属于 CSS，所以导入语句应写在 CSS 中，要注意的是导入语句应写在样式表的开头，否则无法正确导入外部文件；
- `@import` 是 CSS2.1 才出现的概念，所以如果浏览器版本较低，无法正确导入外部样式文件；
- 当 HTML 文件被加载时，`<link>` 引用的文件会同时被加载，而 `@import` 引用的文件则会等页面全部下载完毕再被加载；

## JavaScript

### JS 规范 - Tips

1. 尽量使用 const 去定义常量，且采用大写加`_`，如 `MAX_COUNT = 100`
2. 尽量写代码注释；
3. try catch 不确定的代码块；
4. Promise 的 reject 处理；
5. 及时清理不用的变量、定时器；
6. switch 语句应使用 break 中断，而不是 return；
7. 命名语义化；
8. 尽量减少对闭包的使用
9. 尽可能使用 if 判断做容错处理；
10. 尽量避免使用内置方法或属性名字去定义，如 `var self = this;`

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
