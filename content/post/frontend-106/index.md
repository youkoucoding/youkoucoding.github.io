---
title: '毎日のフロントエンド  106'
date: 2021-12-31T15:04:11+09:00
description: frontend 每日一练
image: frontend-106-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百零六日

## HTML

### **Question:** 除了音频和视频，HTML5 还支持哪些媒体标签

| 标签    | 作用                                                                         | 应用                                      |
| ------- | ---------------------------------------------------------------------------- | ----------------------------------------- |
| embed   | 标签定义嵌入的内容                                                           | CodePen 等网站的代码编辑器可直接嵌入 html |
| track   | 为 video、audio 添加.vtt 格式的字幕文件                                      |                                           |
| source  | 为媒体元素如 video、audio 定义不同格式的媒体资源，让浏览器选择其所支持的一个 |
| canvas  | 定义画布                                                                     | web 游戏开发                              |
| picture | 响应式处理图片                                                               | 适配 Retina 屏幕                          |

## CSS

### **Question:** CSS 中的`calc()`有什么作用

- `calc`使得开发者能够使用四则运算表达式来计算长宽的属性。
- `px`、`%`、`em` `rem`等不同单位的数值均可参与计算，浏览器会进行自动转换。

#### Caveat

运算符的两边必须要有空白字符。

## JavaScript

### **Question:** 作用域链的理解

- 作用域链指的是代码执行时,查找变量的规则

- 作用域链与函数执行栈相对应。js 运行环境分为**全局**、**函数**以及**eval**三类，每当代码执行进入了一个新的运行环境就会将环境的执行上下文入栈，退出环境时将其出栈，从栈顶到栈底形成从内层到外层的嵌套关系。

- 在全局环境中无法访问局部变量

- 由执行上下文创建的词法环境持有外层执行上下文的词法环境引用，当 JS 引擎在当前词法环境中找不到相应的变量时，会逐层向外查找，如此形成的链表即为作用域链。

#### 闭包 again

> 能够访问另一个函数作用域变量的函数，**声明在另一个函数内部的函数**

- 闭包可以访问当前函数以外的变量
- 即使外部函数已经返回，闭包仍能访问外部函数定义的变量与参数
- 闭包可以更新外部变量的值

---

- 避免全局变量的污染
- 能够读取函数内部的变量
- 可以在内存中维护一个变量

---

Caveat

1. 闭包内部可以访问上级作用域，改变上级作用域的私有变量，**不应随便改变上级作用域私有变量的值**
2. 由于闭包会使得函数中的变量都保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在 IE 中可能导致内存泄漏

- 解决方法是，在退出函数之前，将不使用的局部变量全部删除（引用设置为 null ，这样就解除了对这个变量的引用，其引用计数也会减少，从而确保其内存可以在适当的时机回收）

3. 持续运行的服务进程，必须及时释放不再用到的内存，否则占用越来越高，轻则影响系统性能，重则导致进程崩溃。不再用到的内存，没有及时释放，就叫做内存泄漏

4. **闭包的 this 指向的是 window**

---

应用场景

> 闭包通常用来创建内部变量，使得这些变量不能被外部随意修改，同时又可以通过指定的函数接口来操作。例如 `setTimeout` 传参、回调、IIFE、函数防抖、节流、柯里化、模块化等等

- 函数防抖、节流
  - 防抖 （`debounce`） ：多次触发，只在最后一次触发时，执行目标函数
  - 节流（`throttle`）：限制目标函数调用的频率，比如：1s 内不能调用 2 次

---

`Debounce`

```js
// 这个是用来获取当前时间戳的
function now() {
  return +new Date();
}
/**
 * 防抖函数，返回函数连续调用时，空闲时间必须大于或等于 wait，func 才会执行
 *
 * @param  {function} func        回调函数
 * @param  {number}   wait        表示时间窗口的间隔
 * @param  {boolean}  immediate   设置为ture时，是否立即调用函数
 * @return {function}             返回客户调用函数
 */
function debounce(func, wait = 50, immediate = true) {
  let timer, context, args;

  // 延迟执行函数
  const later = () =>
    setTimeout(() => {
      // 延迟函数执行完毕，清空缓存的定时器序号
      timer = null;
      // 延迟执行的情况下，函数会在延迟函数中执行
      // 使用到之前缓存的参数和上下文
      if (!immediate) {
        func.apply(context, args);
        context = args = null;
      }
    }, wait);

  // 这里返回的函数是每次实际调用的函数
  return function (...params) {
    // 如果没有创建延迟执行函数（later），就创建一个
    if (!timer) {
      timer = later();
      // 如果是立即执行，调用函数
      // 否则缓存参数和调用上下文
      if (immediate) {
        func.apply(this, params);
      } else {
        context = this;
        args = params;
      }
      // 如果已有延迟执行函数（later），调用的时候清除原来的并重新设定一个
      // 这样做延迟函数会重新计时
    } else {
      clearTimeout(timer);
      timer = later();
    }
  };
}
```

---

`Throttle`

```js
/**
 * underscore 节流函数，返回函数连续调用时，func 执行频率限定为 次 / wait
 *
 * @param  {function}   func      回调函数
 * @param  {number}     wait      表示时间窗口的间隔
 * @param  {object}     options   如果想忽略开始函数的的调用，传入{leading: false}。
 *                                如果想忽略结尾函数的调用，传入{trailing: false}
 *                                两者不能共存，否则函数不能执行
 * @return {function}             返回客户调用函数
 */
_.throttle = function (func, wait, options) {
  var context, args, result;
  var timeout = null;
  // 之前的时间戳
  var previous = 0;
  // 如果 options 没传则设为空对象
  if (!options) options = {};
  // 定时器回调函数
  var later = function () {
    // 如果设置了 leading，就将 previous 设为 0
    // 用于下面函数的第一个 if 判断
    previous = options.leading === false ? 0 : _.now();
    // 置空一是为了防止内存泄漏，二是为了下面的定时器判断
    timeout = null;
    result = func.apply(context, args);
    if (!timeout) context = args = null;
  };
  return function () {
    // 获得当前时间戳
    var now = _.now();
    // 首次进入前者肯定为 true
    // 如果需要第一次不执行函数
    // 就将上次时间戳设为当前的
    // 这样在接下来计算 remaining 的值时会大于0
    if (!previous && options.leading === false) previous = now;
    // 计算剩余时间
    var remaining = wait - (now - previous);
    context = this;
    args = arguments;
    // 如果当前调用已经大于上次调用时间 + wait
    // 或者用户手动调了时间
    // 如果设置了 trailing，只会进入这个条件
    // 如果没有设置 leading，那么第一次会进入这个条件
    // 还有一点，你可能会觉得开启了定时器那么应该不会进入这个 if 条件了
    // 其实还是会进入的，因为定时器的延时
    // 并不是准确的时间，很可能你设置了2秒
    // 但是他需要2.2秒才触发，这时候就会进入这个条件
    if (remaining <= 0 || remaining > wait) {
      // 如果存在定时器就清理掉否则会调用二次回调
      if (timeout) {
        clearTimeout(timeout);
        timeout = null;
      }
      previous = now;
      result = func.apply(context, args);
      if (!timeout) context = args = null;
    } else if (!timeout && options.trailing !== false) {
      // 判断是否设置了定时器和 trailing
      // 没有的话就开启一个定时器
      // 并且不能不能同时设置 leading 和 trailing
      timeout = setTimeout(later, remaining);
    }
    return result;
  };
};
```

---

柯里化

> 柯里化（Currying）是把接受多个参数的函数变换成接受一个单一参数(最初函数的第一个参数)的函数，并且返回接受余下的参数且返回结果的新函数的技术。

```js
var add = function (x) {
  return function (y) {
    return x + y;
  };
};
var increment = add(1);
var addTen = add(10);
increment(2);
// 3
addTen(2);
// 12
add(1)(2);
// 3
```

---

模块化

> 模块化的目的在于将一个程序按照其功能做拆分，分成相互独立的模块，以便于每个模块只包含与其功能相关的内容，模块之间通过接口调用。

- 外部必须是一个函数,且函数必须至少被调用一次(**每次调用产生的闭包作为新的模块实例**)
- 外部函数内部至少有一个内部函数, 内部函数用于修改和访问各种内部私有成员

```js
function myModule() {
  const moduleName = '自定义模块';
  var name = 'sisterAn';

  // 在模块内定义方法(API)
  function getName() {
    console.log(name);
  }
  function modifyName(newName) {
    name = newName;
  }

  // 模块暴露:  向外暴露API
  return {
    getName,
    modifyName,
  };
}

// 测试
const md = myModule();
md.getName(); // 'sisterAn'
md.modifyName('PZ');
md.getName(); // 'PZ'

// 模块实例之间互不影响
const md2 = myModule();
md2.sayHello = function () {
  console.log('hello');
};
console.log(md); // {getName: ƒ, modifyName: ƒ}
```

### **Question:** Web 安全色所能够显示的颜色种类有多少种

- 所谓 web 安全色，在不同的平台展示的效果和预期一致。例如，在 mac 和 window 展示的效果一致
- WEB 安全色的 RGB 值均为 51 的倍数。举个例子，rgb(0,0,51),rgb(0,0,102), rgb(0,0,153) 都是 web 安全色
- 216 种，某些平台只支持 216 种颜色

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[calc() - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc%28%29)

[Web 安全色](https://www.bootcss.com/p/websafecolors/)

[闭包的使用场景，使用闭包需要注意什么](https://github.com/sisterAn/blog/issues/81)

[Array.prototype.forEach() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
