---
title: '毎日のフロントエンド　22'
date: 2021-10-07T11:06:55+09:00
description: frontend 每日一练
image: frontend-22-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第二十二日

## HTML

### **#Question:** `js`放在`html`的`<body>`和`<head>`有什么区别

> 在浏览器渲染页面之前，它需要通过解析`HTML`标记然后构建`DOM`树。在这个过程中，如果解析器遇到了一个脚本(script)，它就会停下来，并且执行这个脚本，然后才会继续解析 HTML。如果遇到了一个引用外部资源的脚本(script)，它就必须停下来等待这个脚本资源的下载，而这个行为会导致一个或者多个的网络往返，并且会延迟页面的首次渲染时间

> 外部引入的脚本(script)会阻塞浏览器的并行下载

> 浏览器解析`HTML`顺序[^1]

[^1]: [只谈 js 文件和 html dom 标签](https://github.com/haizlin/fe-interview/issues/74#issuecomment-643983992)

1. `js` 放在 `<head>` 中，如果不添加 `async` 或者 `defer` 时，当浏览器遇到 script 时，会阻塞 DOM 树的构建，进而影响页面的加载。当 js 文件较多时，页面白屏的时间也会变长。

2. 把 js 放到 `<body>` 里（一般在 `</body>` 的上面）时，由于 DOM 是顺序解析的，因此 js 不会阻塞 DOM 的解析。对于必须要在 DOM 解析前就要加载的 js，我们需要放在 `<head>` 中。

3. 一般情况下是在网站中，同步在 `<head>` 加载的脚本通常是业务必须的，比如说我要注册一个 window 对象，或者用 document.write 写入一些内容，或者是业务需求，我们可以用 head 来做加载:**头部给 script 标签加入 async 的属性，表示它是异步加载的脚本，不会对 html 进行阻塞，这也是大部分网站的做法**

#### Conclusion

- 对于必须要在 `DOM` 加载之前运行的 `JavaScript` 脚本，我们需要把这些脚本放置在页面的 `head` 中，而不是通过外部引用的方式，因为外部的引用增加了网络的请求次数；并且我们要确保内敛的这些 `JavaScript` 脚本是很小的，最好是压缩过的，并且执行的速度很快，不会造成浏览器渲染的阻塞

- 对于支持使用 `script` 标签的 `async` 和 `defer` 属性的浏览器，我们可以使用这两个属性；其中需要注意的点就是，`async` 表示的意思是异步加载 `JavaScript` 文件，它的下载过程可以在 HTML 的解析过程中进行，加载完成之后立即执行这个文件的代码，执行文件代码的过程中会阻塞 HTML 的解析，它不保证文件加载的顺序。`defer` 表示的意思是在 `HTML` 文档解析之后在执行加载完成的 JavaScript 文件，JavaScript 文件的下载过程可以在 HTML 的解析过程中进行，它是按照 script 标签的先后顺序来加载文件的。更多详细的解释可以参考 async vs defer attributes[^2]

[^2]: [async vs defer attributes - Growing with the Web](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)

## CSS

### **#Question:** 说说浏览器解析 CSS 选择器的过程

**从上到下，从右到左**

因为从左到右，首先浏览器会遍历你最左边的选择器，可能是`div`，可能是`span`，我需要在整个页面去把匹配成功的 dom 找出来，可以说是海底捞针，但是从右到左不一样了，它通过具体的遍历条件去寻找一个最匹配的值，查找之后在向上查询，是否符合自己的选择器规则，才最后匹配成功；

前者会浪费大量的遍历时间，造成大量错误的匹配结果

```css
.class ul li span {
  // css 属性
}
/* 从右向左开始解析。因为一般来说，最右侧的节点范围反而会比较大，越向左限定的条件就越多。也因此 CSS 的选择器设计上不宜嵌套过多，会带来性能上的问题。 */
```

## JavaScript

### **#Question:** 你对 new 操作符的理解是什么？手动实现一个 new 方法

#### new operator

The `new` operator lets developers create an instance of a user-defined object type or of one of the built-in object types that has a constructor function.

> new 运算符创建一个用户定义的对象类型的实例 或 具有构造函数的内置对象类型

`new constructor[([arguments])]`

#### Description

The new keyword does the following things:

1. Creates a blank, plain JavaScript object (`{}`)
2. Adds a property to the new object `__proto__` that links to the constructor function's **prototype** object
3. Blinds the newly created object instance as the `this` context( all references to `this` in the constructor function now refer to the object created in the first step).
4. Returns `this` if the doesn't return an object

> 1. 创建一个新对象，即 `{}`
> 2. 把新对象的原型指向构造函数的 prototype
> 3. 把构造函数里的 this 指向新对象
> 4. 返回这个新对象

```js
function Foo(bar1, bar2) {
  this.bar1 = bar1;
  this.bar2 = bar2;
}

var myFoo = new Foo('Bar 1', 2021);
```

When the code `new Foo(...)` is executed, the following things happen:

1. A new object is created, inheriting from `Foo.prototype`
2. the constructor function `Foo` is called with specified arguments, and with `this` bound to newly created object. `new Foo` is **equivalent** to `new Foo()`, if no argument list is specified, `Foo` is called without argument.
3. The object **(not null, false 123 or other primitive types)** returned by the constructor function becomes the result of the whole `new ` expression. If the constructor function doesn't explicitly return an object, the object created in step one is used instead **(normally constructors don't return a value, but they can choose to do so if they want to override the normal object creation process.)**

#### Simulation `new` operator

```js
function _new() {
  // 1. 创建一个新对象
  var newObj = {};
  // 得到构造函数, 并调用 shift 得到数组的第一个参数，并且会改变元数组
  var Con = [].shift.call(arguments);
  // 2. 把新对象的原型指向构造函数的prototype
  newObj.__proto__ = Con.prototype;
  // 3. 把构造函数里的this指向新对象
  var res = Con.apply(newObj, arguments);
  // 4. 返回新对象
  return typeof res === 'object' ? res : newObj;
}
var obj = _new(constructorFunction, 'willian', 18);
console.log(obj.name, obj.age); //'willian', 18
console.log(obj.say()); //Hello willian
```

- `arguments` 对象来获取传入的所有参数
- `arguments` 对象是所有（**非箭头**）函数中都可用的局部变量
- 可以使用 `arguments` 对象在函数中引用函数的参数
- The `shift()` method removes the first element from an array and returns that removed element. This method changes the length of the array.

---

```js
// method Two

function _new(fn, ...arg) {
  //将创建的对象的原型指向构造函数的原型 ==> 新对象.__proto__(原型) == fn.prototype
  const obj = Object.create(fn.prototype);
  // 将this指向新对象
  const ret = fn.apply(obj, arg);
  //判断返回值 （如果构造函数本身有返回值且是对象类型，就返回本身的返回值，如果没有才返回新对象）
  return ret instanceof Object ? ret : obj;
}
```

## Reference

[移除会阻止内容呈现的 JavaScript  |  PageSpeed Insights  |  Google Developers](https://developers.google.com/speed/docs/insights/BlockingJS)

[All about <script>. In this article, you’ll learn about… | by Oussema Miled | Level Up Coding](https://levelup.gitconnected.com/all-about-script-87fea475b976)
