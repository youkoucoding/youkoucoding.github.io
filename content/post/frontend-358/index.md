---
title: '毎日のフロントエンド 358'
date: 2022-07-07T19:57:15+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JS
  - Tips
---

## HTML

### 给一个元素加下划线

- 使用 `<u></u>` 标签
- 给元素添加 `boder-bottom`
- 文字样式 `text-decoration: underline;`
- 使用伪类或者子元素做绝对定位

```css
.target {
  position: relative;
}
.underline {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 1px;
  background-color: black;
}
```

## JS

###

```js
var person = {
	age:1
}
Object.defineProperty(person,'age',{
	get(){
		return this.age
	},
	set(val){
		this.age = val
	}
})

person.age   Error：Maximum call stack size exceeded，循环引用导致栈溢出
```

- `person.age --> get.call(person) -->this.age --> person.age --> get.call(person) --> this.age -->...`

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)
- [Object.defineProperty() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
