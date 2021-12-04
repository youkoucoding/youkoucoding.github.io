---
title: '毎日のフロントエンド　 79'
date: 2021-12-04T17:26:38+09:00
description: frontend 每日一练
image: frontend-79-cover.jpg
categories:
  - CSS
  - Javascript
---

# 第七十九日

## CSS

### **Question:** 说说你对`BEM`规范的理解，同时举例说明常见的`CSS`规范有哪些

#### `BEM`规范

CSS 的命名规范又叫做 BEM 规范，BEM - `Block Element Modifier` 它可以快速创建出可复用的前端组件和前端代码。

- `Block`, 代表了一个独立的块级元素，可以理解为功能组件块，**一个 Block 就是一个独立的功能区块**，比如头部是一个 block，表单功能输入框也可以是一个 block。block 功能可大可小。
- `Element`, 是 Block 的一部分不能独立来使用，**所有的 Element 都是与 Block 紧密关联的**。例如一个带有 icon 的输入框，那么这个 icon 就是输入框 block 的一个 element。
- `Modifier`，是用来修饰 Block 的一个 Element，**表示 block 或者 element 在外观或者行为上的改变**。例如前文提到的输入框 Block，当鼠标悬停时边框高亮，那么这个高亮的效果就应该用 Modifier 来实现。

#### BEM 命名

- 尽量使用类名选择器，**不要使用 tag 或者 id 选择器**
- 使用**小写字母、数字或者`-`**，多个单词描述时采用`-`来连接,例如`el-input`
- 使用两个`_`来连接`Block`和`Element`, `Block__Element`, 例如`el-input__icon`
- 使用两个`-`来连接 `Modifier` 和 `Element` 或 `Block`, `Block__Element--modifier` 或者 `Block--modifier`, 例如 `el-input--color-primary`, `el-input__icon--size-small`

```html
<form action="" class="form form--theme-xmas form--simple">
  <input type="text" class="form__input" />
  <input type="text" class="form__submit  form__submit--disabled" />
</form>
<style>
  .form {
  }
  .form--thema-xmas {
  }
  .form--simple {
  }
  .form__input {
  }
  .form__submit {
  }
  .form__submit--disabled {
  }
</style>
```

#### 在`Sass`中的使用

```sass
.header {
    &__body {
        padding: 20px;
    }
​
    &__button {
        &--primary {
            background: #329FD9;
        }
        &--default {
            background: none;
        }
    }
}
```

#### 在`Less`中的使用

```css
@classname: header;
​ .@{classname} {
  .@{classname}__body {
    padding: 20px;
  }
  ​ .@{classname}__button {
    .@{classname}__button--primary {
      background: #329fd9;
    }
    ​ .@{classname}__button--default {
      background: none;
    }
  }
}
```

## JavaScript

### **Question:** 举例说明什么是`IIFE`？它有什么好处

#### Instantly Invoked Function Expression 即时调用函数表达式

```js
//最好在 IIFE 前追加分号 ; 来避免解析时与前一个表达式合并出现问题
;(function () {
    // ... statements
    return result;
)()
```

- 创建**一个局部作用域隔离变量**；通过立即执行函数来创造一个新的作用域并缓存变量，来解决闭包带来的副作用问题，但在 ES6 拥有了块级作用域后变得没有必要，可以用语句块 { ... } 配合 let/const 替代
- 将运行逻辑转化为可求值的表达式，弥补 JavaScript 基本逻辑语句不是表达式的缺陷

  ```js
  const a = (() => {
  if (...) return 1
  else return 0
  })()

  ```

  基本等价于

  ```js
  let a
  if (...) a = 1
  else a = 0
  ```

- 由于立即执行函数，不污染外部作用域的原则，因此诞生了 umd 模块的写法。一般主流的框架都会提供 umd 格式的 js，通过 script 标签注入页面后会生成唯一一个全局变量，内部变量都不会暴露出来

### **Question:** 写出执行结果,并解释原因

```js
async function async1() {
  console.log('async1 start');
  await async2();
  console.log('async1 end');
}
async function async2() {
  console.log('async2');
}
console.log('script start');
setTimeout(function () {
  console.log('setTimeout');
}, 0);

async1();

new Promise(function (resolve) {
  console.log('promise1');
  resolve();
}).then(function () {
  console.log('promise2');
});
console.log('script end');
```

Result:

```js
script start
async1 start
async2
promise1
script end
async1 end
promise2
setTimeout
```

1. `Promise`优先于`setTimeout(宏任务)`。所以，`setTimeout`回调会在最后执行。
2. `Promise`一旦被定义，就会立即执行
3. `Promise`的`reject`和`resolve`是异步执行的回调。所以，`resolve()`会被放到回调队列中，在主函数执行完和`setTimeout`前调用。
4. `await`执行完后，会让出线程。`async`标记的函数会返回一个`Promise`对象

```js
async function async1() {
  console.log('async1 start');
  await async2();
  console.log('async1 end');
}
async function async2() {
  console.log('async2');
}

// 相当于
function async1() {
  return new Promise((resolve, reject) => {
    console.log('async1 start');
    async2().then((result) => {
      console.log('async1 end');
    });
    resolve();
  });
}
function async2() {
  return new Promise((resolve) => {
    console.log('async2');
    resolve();
  });
}
```

执行流程

1. 首先，事件循环从宏任务(macrotask)队列开始，这个时候，宏任务队列中，只有一个 script(整体代码)任务；从宏任务队列中取一个任务出来执行。

   - 首先执行`console.log('script start')` ，输出 'script start'
   - 遇到`setTimeout` 把 `console.log('setTimeout')` 放到`macroTask`队列中
   - 执行`async1()`， 输出 'async1 start' 和 'async2' ，把 `console.log('async1 end')` 放到`microTask` 队列中
   - 执行到`promise`， 输出 'promise1' ， 把 `console.log('promise2')` 放到`microTask`队列中
   - 执行 `console.log('script end')` 。输出 'script end'

2. `macroTask` 执行完会执行`microTask`，把 `microtask quene` 里面的 `microTask` 全部拿出来一次性执行完，所以会输出 ‘async1 end’ 和 ‘promise2’

3. 开始新一轮事件循环，去除一个`macroTask`执行，所以会输出 “setTimeout”

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
