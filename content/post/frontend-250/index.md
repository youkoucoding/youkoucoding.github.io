---
title: '毎日のフロントエンド 250~252'
date: 2022-05-12T23:42:06+09:00
description: frontend 每日一练
categories:
  - Tips
---

## HTML

## CSS

### `PPI`和`DP`

- PPI（pixel per inch）：像素密度，1 英寸屏幕上显示的像素量。值越高，屏幕越细腻
- DP（Density-independent pixel）：安卓开发用的长度单位

- 1dp 等于屏幕像素密度为 160ppi 时 1px 的长度，因此 dp 在整个系统大小是固定的。
  - 公式：1dp=（屏幕 ppi / 160）px。

### `radio`和`checkbox`自定义样式

#### 方法一：`appearance` + `:after`

- 利用 appearance 属性清除 radio 或 checkbox 的原生样式，然后在此基础添加上一些自定义的样式和动画效果，以达到自定义样式的目的。 用到了:after 伪元素生成控件内部的选中样式。

```css
input[type='radio'],
input[type='checkbox'] {
  appearance: none;
}
```

```html
<div class="dz-switch">
  <input type="checkbox" name="c1" />
  <input type="checkbox" name="c1" checked />
</div>
```

```css
@color-error: #f56c6c;
@color-black: #000;

.dz-switch {
  margin: 30px;
  font-size: 18px;
  input[type='radio'],
  input[type='checkbox'] {
    -webkit-appearance: none;
    -moz-appearance: none;
    position: relative;
    display: inline-block;
    vertical-align: middle;
    margin: -0.3em 0.25em 0 0;
    padding: 0;
    width: 1.6em;
    height: 1.6em;
    border: 1px solid #ccc;
    font-size: 0.9em;
    cursor: pointer;
    outline: none;
    transition: all 0.2s ease;
  }

  input[type='radio'] {
    border-radius: 50%;
  }

  input[type='radio']:after {
    content: '';
    display: inline-block;
    position: absolute;
    left: 50%;
    top: 50%;
    background: @color-black;
    border-radius: 50%;
    width: 0;
    height: 0;
    opacity: 0;
    transform: translate(-50%, -50%);
    transition: all 0.2s ease;
    transform-origin: center center;
    pointer-events: none;
  }

  input[type='radio']:checked:after {
    width: 1em;
    height: 1em;
    opacity: 1;
  }

  input[type='checkbox'] {
    border-radius: 0.2em;
  }

  input[type='checkbox']:after {
    content: '\2714';
    display: inline-block;
    position: absolute;
    left: 50%;
    top: 50%;
    color: @color-black;
    font-size: 2.5em;
    font-family: meiryo, 'sans-serif';
    opacity: 0;
    transition: all 0.1s ease;
    transform: translate(-50%, -50%) scale3d(0.2, 0.2, 1);
    transform-origin: center center;
  }

  input[type='checkbox']:checked:after {
    opacity: 1;
    transform: translate(-50%, -50%) scale3d(1.2, 1.2, 1);
  }
}
```

#### `checkbox` 应用

```less
input[type='checkbox'] {
  display: none;

  &:checked ~ span {
    color: @color-gray;

    &:after {
      width: 100%;
    }
  }

  &:checked ~ label {
    &:after {
      opacity: 1;
    }
  }
}
```

#### 方法二：`opacity: 0`(display:none) + 额外标签

- 这个方法是多数 UI 库使用的方法：首先将 checkbox 和 radio 隐藏，然后让相邻它的标签（或 label 标签）伪装成它，并设置样式， 利用 css3 的“相邻兄弟选择器”（ + 或 ~ ）,通过 checkbox 和 radio 的选中切换该元素不同的样式，

## JavaScript

### 短路求值

#### 短路求值即利用 ||(逻辑或) 和 &&(逻辑与)的短路特性进行赋值

- `const number = test || 0;`

  - 当 test 值为 truthy 时，取 test 的值，否则取 0。这样可以避免 number 被赋为 NaN、null、undefined、false 等值。

- `const number = test && test.value;`
  - 当 test 值为 truthy 时，再去取 test.value 并返回其值，否则返回 false。这样可以避免 test 为空时，test.value 报空指针异常

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [radio 和 checkbox 的自定义样式 | 前端学习笔记](https://xiaotianxia.github.io/blog/vuepress/css/styled_switch.html)

- [checkbox 的应用 | 前端学习笔记](https://xiaotianxia.github.io/blog/vuepress/css/use_of_checkbox.html)
