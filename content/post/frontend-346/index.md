---
title: '毎日のフロントエンド 346~349'
date: 2022-06-28T23:07:09+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### `shape-outside`

- `shape-outside`的 CSS 属性定义了一个可以是非矩形的形状，相邻的内联内容应围绕该形状进行包装。默认情况下，内联内容包围其边距框;
- `shape-outside`提供了一种自定义此包装的方法，可以将文本包装在复杂对象周围而不是简单的框中。

## Tips

### API 设计原则

#### 好 API 的特质

- 极简：
- 完备：
- 语义清晰简单
- 符合直觉
- 易于记忆
- 引导 API 使用者写出可读代码

#### 静态多态

- 尽量减少继承，让相似的类具备相似的 API，而不是统一继承一个父类。因为统一继承会带来 API public 数量过多，父级无意义的方法对用户产生误导

#### 基于属性的 API

- 属性指的是对象状态，通过属性为粒度的 API，有利于使用者理解 API 的含义，但需注意关联属性的顺序性

#### API 语义和文档

- 比如传值 -1 的含义是什么？如果 API 文档不像 [http status codes](https://www.restapitutorial.com/httpstatuscodes.html) 一样健全，建议通过枚举的方式增加可读性

#### 命名的艺术

- 不要使用缩写，保持一致性。类命名以功能分组作为后缀，比零散命名更易懂
- 函数命名要体现出是否包含副作用，参数过多时以对象作为传参，布尔参数改为枚举类型，或者分解为两个语义化 API

#### 统一关键字库

- 所有 api 定义之前，先抽离业务和功能语义的关键字，统一关键字库;
- 可以更好的让多人协作看起来如出一辙, 而且关键字库 更能够让调用者感觉到 符合直觉、语义清晰;
- 关键字库也是 PREDO 的内容之一

#### 单一职责

- 接口设计尽量要做到 单一职责,最细粒度化;
- 可以使用组合的方式把多个解耦的单个接口组合在一起作为一个大的功能项接口;
- 接口设计的单一职责，也更方便多人协作时候的扩展和组合;

#### 面向未来的多态

- 对于接口参数的扩展，我们要做到面向扩展开放，面向修改关闭;
- 升级做到要兼容，否则会导致大批量的下游不可用
- 同时也要避免过度设计，当抽象功能只有一处使用时，尽量不要过早抽象

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [shape-outside - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/shape-outside)

- [weekly/23.精读《API 设计原则》](https://github.com/ascoders/weekly/blob/master/%E5%89%8D%E6%B2%BF%E6%8A%80%E6%9C%AF/23.%E7%B2%BE%E8%AF%BB%E3%80%8AAPI%20%E8%AE%BE%E8%AE%A1%E5%8E%9F%E5%88%99%E3%80%8B.md)

- [API 设计原则 – Qt 官网的设计实践总结 ](https://coolshell.cn/articles/18024.html)

- [HTTP Status Codes](https://www.restapitutorial.com/httpstatuscodes.html)
