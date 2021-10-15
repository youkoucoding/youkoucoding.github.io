---
title: '毎日のフロントエンド　29'
date: 2021-10-15T22:05:17+09:00
description: frontend 每日一练
image: frontend-29-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二十九日

## HTML

### **#Quesion:** 了解什么是无障碍 web（WAI）吗？在开发过程中要怎么做呢

无障碍 `Web` == 有良好访问性的 `Web`

1. 页面的内容结构

应该让标题、段落、列表等各司其职，让整个页面内容结构清晰，比如：

```js
<article>
  <h2>静夜思</h2>
  <p>[唐] 李白</p>
  <div>
    床前明月光，疑是地上霜。
    <br />
    举头望明月，低头思故乡。
  </div>
  <ul>
    <li>
      <a href='#'>译文</a>
    </li>
    <li>
      <a href='#'>注释</a>
    </li>
    <li>
      <a href='#'>作者介绍</a>
    </li>
  </ul>
</article>
```

好的语义，屏幕阅读器会：

- 在你浏览内容时，读取每个标题，通知标题是什么，段落是什么等
- 它会在每个元素之后停止，让你有个短暂的停歇
- 你可以跳转到上一个/下一个标题
- ...

2. 简写和缩写
3. `form` 表单

```js
<form>
    <div>
        <label for="name">姓名：</label>
        <input type="text" id="name" name="name">
    </div>

    <div>
        <label for="age">年龄：</label>
        <input type="text" id="age" name="age">
    </div>

    <div>
        <label for="gender">性别：</label>
        <select id="gender" name="gender">
            <option>男</option>
            <option>女</option>
        </select>
    </div>

</form>
```

`<label>` 标签可以让提示文本和输入框完美的对应起来，还可以扩大激活输入框的范围，方便用户选择和输入

4. 键盘可访问性

键盘可访问包括按 tab 键能让页面中的元素获得焦点、按 Return/Enter 键能激活该元素、表单元素 `<select>` 在获得焦点时按方向键可以上下切换选项。自带键盘可访问性的标签有`<a>`、`<button>`、`<label>`以及表单元素

5. `alt` 属性

## CSS

### **#Quesion:** 请描述 css 的权重计算规则

权重值计算

| 选择器                         | 例              | 权重值     |
| ------------------------------ | --------------- | ---------- |
| `!important`                   | `!important`    | `Infinity` |
| 内联样式                       | `style=".."`    | 1000       |
| ID                             | `#id`           | 100        |
| class                          | `.class`        | 10         |
| 属性                           | `[type='text']` | 10         |
| 伪类                           | `:hover`        | 10         |
| 标签                           | `p`             | 1          |
| 伪元素                         | `::first-line`  | 1          |
| 相邻选择器、子代选择器、通配符 | `\* > +`        | 0          |

- 1000>100。也就是说从左往右逐个等级比较，前一等级相等才往后比
- 在权重相同的情况下，后面的样式会覆盖掉前面的样式
- 继承属性没有权重
- 通配符、子选择器、相邻选择器等的。虽然权值为 0，但是也比继承的样式优先
- ie6 以上才支持 important，并且尽量少用

## JavaScript

### **#Quesion:** 写一个获取数组的最大值、最小值的方法

```js
Math.max.apply(Array, [25, 62, 91, 78, 34, 62]); //  91
Math.min.apply(Array, [27, 64, 90, 78, 34, 62]); // 27

// or
maxValue = Math.max.apply(null, arr);
minValue = Math.min.apply(null, arr);
```

---

```js
//es6
let arr = [1, 2, 3, 4];
Math.max(...arr);
Math.min(...arr);
```

## Reference

[CSS 是怎样确定图像大小的？ · Issue #44 · anjia/blog](https://github.com/anjia/blog/issues/44)
