---
title: '毎日のフロントエンド  134'
date: 2022-01-28T17:46:44+09:00
description: frontend 每日一练
image: frontend-134-cover.jpg
categories:
  - CSS
  - Javascript
---

# 第一百三十四日

## HTML

### **Questions:** Web Worker 线程的限制是什么

- 同源限制

  - 分配给 Worker 线程运行的脚本文件，必须与主线程的脚本文件同源。

- DOM 限制

  - Worker 线程所在的全局对象，与主线程不一样，无法读取主线程所在网页的 DOM 对象，也无法使用 document、window、parent 这些对象。但是，Worker 线程可以 navigator 对象和 location 对象。

- 通信联系

  - Worker 线程和主线程不在同一个上下文环境，它们不能直接通信，必须通过消息完成。

- 脚本限制

  - Worker 线程不能执行 alert()方法和 confirm()方法，但可以使用 XMLHttpRequest 对象发出 AJAX 请求。

- 文件限制

  - Worker 线程无法读取本地文件，即不能打开本机的文件系统（file://），它所加载的脚本，必须来自网络。

## CSS

### **Questions:** transition、animation、transform 三者有什么区别

- `transition`：可以用来设置一个过渡动画效果

  - `transition: margin-right 4s ease-in-out 1s;`

- `animation`：css 动画效果设置，可以通过指定不同的关键帧设置复杂的动画效果

  - ```css
    animation: mymove 5s infinite;
    @keyframes mymove {
      from {
        left: 0px;
      }
      to {
        left: 200px;
      }
    }
    ```

- `transform`：css3 新增的一个变形属性，可以对元素做 2d 或 3d 旋转，缩放，倾斜的效果
  - `transform:rotate(9deg) scale(0.5) ;`

## JavaScript

### **Questions:** 代码运行的结果及解释

```js
var type = 'images';
var size = { width: 800, height: 600 };
var format = ['jpg', 'png'];

function change(type, size, format) {
  type = 'video';
  size = { width: 1024, height: 768 };
  format.push('map');
}

change(type, size, format);

console.log(type, size, format);
```

result:

```js
type = 'images';
size = { width: 800, height: 600 };
format = ['jpg', 'png', 'map'];
```

- 函数形参的传递根据实参值的类型分为值复制（对应基本类型）和引用复制（对应引用类型）
- 通过值复制的值无论怎么改变都不会影响原来的值（对应 type 变量的情况）
- 直接改变引用本身并不会影响其他引用（即引用另一个引用类型的值时，原值的其他引用不会改变；对应 size 变量的情况）
- 通过引用复制的值改变后原值也会改变（对应 format 变量的情况）

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
