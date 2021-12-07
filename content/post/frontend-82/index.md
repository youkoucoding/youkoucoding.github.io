---
title: '毎日のフロントエンド　 82'
date: 2021-12-07T12:20:43+09:00
description: frontend 每日一练
image: frontend-82-cover.jpg
categories:
  - Javascript
---

# 第八十二日

## CSS

### **Question:** 遇到过`IE6/7/8/9`的 BUG 及解决方法

1. IE6 `margin`双边距问题
2. IE67 `li`间隙问题
3. 图片间隙问题——`vertical-align：top`
4. ie6 下高度小于`19px`处理成`19px；` `font-size:0;`或者`overflow：hidden`
5. ie8 以下滤镜问题，`filter:alpha(opacity=50)`
6. IE6 `position:fixed` 不兼容，`fixed`定位 ie6 兼容，js 处理，通过获取滚动高度，赋值给需要固定的元素，设置绝对定位，设置`top`
7. ie6 下父级的`overflow：hidden`是保不住子级的相对定位的（`relative`）-解决，给父级加定位
8. ie6 下，绝对定位的父级，宽高是奇数的话，定位偏移会出现 1px 的偏差
9. ie6 下，内容会撑开设置好的宽度
10. ie6，`7` `3px`问题
11. `<p><h3></h3></p>`会把 p 元素分割成两个，原因，嵌套的规范性，p 需要嵌套 inline 元素
12. 在 ie6 下，`1px dotted #0` 不兼容。精度问题，可以用背景平铺
13. ie6 下`margin`传递需要触发`haslayout`，父级有边框时，子元素`margin`值消失，解决办法，触发父级`haslayout`
14. ie6 下当一行子元素占有的宽度之和与父级的宽度相差超过`3px`或者有不满行状态的时候，最后一行子元素的下`margin`就会失效
15. ie6 下的文字溢出 bug 条件 1，子元素的宽度和父级的宽度相差小于 3px 的时候，2，两个浮动元素中间有注释或内联元素——解决办法：用 div 吧注释或内联元素包裹起来
16. ie6 下，当浮动元素和绝对定位元素是并列关系的时候，绝对定位会消失，解决办法：给定位元素外面包裹`div`
17. ie6，7 下，输入类型的表单控件上下各有`1px`的间隙——解决办法：给`input`加浮动
18. ie6，7 下，输入类型的表单控件加`border：none`无效，还是会出现边框——解决办法：1，给`border：0;2，` 重置`input`的背景
19. ie6，7 下，输入类型的表单控件输入文字的时候，背景图片会跟随移动——解决办法：把背景加给父级
20. 处理 ie6 `png`图片兼容问题 js 插件，DD_belatedPNG.js,也可以用 CSS 滤镜处理
    - a.css 处理
    - b.微软 behavior 扩展，下载 iepngfix.htc
    - c.js 插件
21. css hack \9——IE10 之前的浏览器解析，+，\*——IE7 包括 IE7 之前的浏览器解析， \_ ——IE6 包括 IE6 之前的 IE 浏览器
22. `important` 兼容问题，在 IE6 下，在 `important` 后面加一条同样的样式，会破坏 `important` 优先级作用，按照默认的优先级顺序来走
23. IE6 `margin` 负值不兼容，处理，只要 `position：relative;` 这个解决方案在圣杯布局中有出现。圣杯布局，可以用 `position：absolute;` 来定位

## JavaScript

### **Question:** 实现一个九九乘法口诀表

```js
const MAX_WIDTH = 7;
let table = '';
for (let rhs = 1; rhs <= 9; rhs++) {
  for (let lhs = 1; lhs <= 9; lhs++) {
    if (lhs <= rhs) table += `${lhs}*${rhs}=${lhs * rhs}`.padEnd(MAX_WIDTH);
  }
  table += '\n';
}
console.log(table);
```

> `String.prototype.padEnd()` 用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从当前字符串的末尾（右侧）开始填充。如果这个数值小于当前字符串的长度，则返回当前字符串本身。填充字符串的缺省值为 `""`

### **Question:** 写出执行结果,并解释原因

```js
const obj = {
  2: 3,
  3: 4,
  length: 2,
  splice: Array.prototype.splice,
  push: Array.prototype.push,
};
obj.push(1);
obj.push(2);
console.log(obj);
console.dir(obj); // 正确显示此对象，（不是显示带empty的类数组）打印出该对象的所有属性和属性值
```

Result: `[empty × 2, 1, 2, splice: ƒ, push: ƒ]`

涉及知识点：

1. 类数组（`ArrayLike`）：一组数据，由数组来保存，如果要对这组数据进行扩展，会影响到数组原型，`ArrayLike`的出现则提供了一个中间数据桥梁，ArrayLike 有数组的特性， 但是对 ArrayLike 的扩展并不会影响到原生的数组。
2. `push`方法：具有通用性。该方法和 `call()` 或 `apply()` 一起使用时，可应用在类似数组的对象上。**`push` 方法根据 `length` 属性来决定从哪里开始插入给定的值。** 如果 `length` 不能被转成一个数值，则插入的元素索引为 0，包括 `length` 不存在时。当 `length` 不存在时，将会创建`length`。
3. 唯一的原生类数组（`array-like`）对象是 `String`，尽管如此，它们并不适用该方法，因为字符串是不可改变的。
4. 对象转数组的方式：`Array.from()`、`splice()`、`concat()`等

---

- `obj` 中定义了两个 `key` 值，分别为 `splice` 和 `push` 分别对应数组原型中的 splice 和 push 方法，因此这个 obj 可以调用 push 和 splice 方法。

- 调用对象的 push 方法：`push(1)`,因为此时 obj 中定义 length 为 2，所以从数组中的第二项开始插入，也就是数组的第三项（下标为 2 的那一项， 此处为`key`为 2 的键值对），这时已经定义了下标为 2 和 3 这两项，所以它会替换第三项也就是下标为 2 的值，第一次执行 push 完，此时 key 为 2 的属性值为 1。

- 同理：第二次执行 `push` 方法，`key` 为 3 的属性值为 2。此时的输出结果就是：`Object(4) [empty × 2, 1, 2, splice: ƒ, push: ƒ]`，因为只是定义了 2 和 3 两项，没有定义 0 和 1 这两项，所以前面会是 empty。

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[console.dir - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/console/dir)
