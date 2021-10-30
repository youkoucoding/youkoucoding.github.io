---
title: '毎日のフロントエンド　44'
date: 2021-10-30T20:55:46+09:00
description: frontend 每日一练
image: frontend-44-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第四十四日

## HTML

### **Question:** ｀ video` 标签中预加载视频用到的属性是什么

`preload`

| 属性     | 值       | 描述                                                                                      |
| -------- | -------- | ----------------------------------------------------------------------------------------- |
| autoplay | autoplay | 如果出现该属性，则视频在就绪后马上播放                                                    |
| controls | controls | 如果出现该属性，则向用户显示控件，比如播放按钮                                            |
| height   | pixels   | 设置视频播放器的高度                                                                      |
| loop     | loop     | 如果出现该属性，则当媒介文件完成播放后再次开始播放                                        |
| preload  | preload  | 如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性 |
| src      | url      | 要播放的视频的 URL                                                                        |
| width    | pixels   | 设置视频播放器的宽度                                                                      |

## CSS

### **Question:** 写一个满屏品字布局的方案

- 标准流

```html
<body>
    <div class="top"><div>
    <div class="bottom">
        <div class="left"><div>
        <div class="right"><div>
    <div>
</body>
```

```css
.top {
    width:50%;
    marign:auto;
}
.bottom {
    font-size:0;
}
.bottom div {
    display:inline-block
    width:50%;
}

```

- 浮动布局

```html
<body>
    <div class="top"><div>
    <div class="bottom">
        <div class="left"><div>
        <div class="right"><div>
    <div>
</body>
```

```css
.top {
  width: 50%;
  height: 50%;
  marign: auto;
}
.bottom {
  height: 50%;
  overflow: hidden;
}
.bottom div {
  float: left;
  width: 50%;
}
```

- `flex`布局

```html
<body>
    <div class="top"><div>
    <div class="bottom">
        <div class="left"><div>
        <div class="right"><div>
    <div>
</body>
```

```css
.top {
  width: 50%;
  height: 50%;
  marign: auto;
}
.bottom {
  display: flex;
  height: 50%;
}
.bottom div {
  flex: 1;
  height: 100%;
}
```

- `grid`布局

```html
<body>
    <div class="top"><div>
    <div class="left"><div>
    <div class="right"><div>
</body>
```

```css
.body {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(2, 1fr);
}
.top {
  grid-column-start: 1;
  grid-column-end: 3;
}
```

## JavaScript

### **Question:** 深度克隆对象的方法有哪些

深拷贝和浅拷贝只针对像`Object`和`Array`这样的复杂对象的

- `JSON.stringify`
  > `JSON.parse(JSON.stringfy(X))`，其中`X`只能是`Number`, `String`, `Boolean`, `Array`, 扁平对象，即那些能够被 `JSON` 直接表示的数据结构。

JSON.stringify 有一定的局限:

1. `date: [new Date(1536627600000), new Date(1540047600000)]`,
2. `date: new RegExp('\\w+')`,
3. `date: function func() { console.log('fff') }`
4. `object`里有`NaN`、`Infinity`和`-Infinity`，则序列化的结果会变成`null`
5. `JSON.stringify()` 只能序列化对象的可枚举的自有属性，例如 如果 obj 中的对象是有构造函数生成的， 则使用`JSON.parse(JSON.stringify(obj))`深拷贝后，会丢弃对象的 `constructor`
6. 对象中存在循环引用的情况也无法正确实现深拷贝

```js
let obj = {
  name: 'liming',
  age: 12,
  parents: {
    mother: { name: 'zhanglan', age: 34 },
    father: { name: 'lifeng', age: 35 },
  },
  score: [1, 2, 3, 4, 5, 6],
  say: function () {
    console.log('my name is ' + this.name);
  },
  null: null,
  undefined: undefined,
};

console.log(JSON.parse(JSON.stringify(obj)));
```

- 深度递归

```js
function deepClone(value) {
  if (Object.prototype.toString.call(value) === '[object Object]') {
    //对象
    let returnObj = {};
    for (let key in value) {
      returnObj[key] = deepClone(value[key]);
    }
    return returnObj;
  } else if (Object.prototype.toString.call(value) === '[object Array]') {
    //数组, 需要特殊处理
    let returnArr = [];
    for (i = 0, len = value.length; i < len; i++) {
      returnArr.push(deepClone(value[i]));
    }
    return returnArr;
  }
  return value;
}
```

## Reference

[haizlin/fe-interview: HTML/CSS/JavaScript/Vue/React/Nodejs/TypeScript/ECMAScritpt/Webpack/Jquery/小程序/软技能……](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)
