---
title: '毎日のフロントエンド  143'
date: 2022-02-06T21:32:13+09:00
description: frontend 每日一练
image: frontend-143-cover.jpg
categories:
  - Javascript
---

# 第一百四十三日

## HTML

### **Question:** `<pre>` 和 `<code>` 标签的区别

- `pre`标签是块级元素，`code`标签是行内元素
- `pre`的内容保留换行符和连续空格，`code`的内容不保留
- 单行代码用`code`，多行代码用`pre`

## CSS

### **Question:** 自定义 radio 按钮的样式

`input[type="radio"]`

#### 控件与 label 并列

- 为`<label>`元素添加生成性内容（伪元素），用其来设置单选按钮的样式
- 把真正的单选按钮隐藏起来

```html
<div class="choose-box">
  <input type="radio" id="yes" name="choose" />
  <label for="yes">是</label>
</div>
<div class="choose-box">
  <input type="radio" id="no" name="choose" />
  <label for="no">否</label>
</div>
```

1. 给`label`标签添加伪类, 选中时使用`background-clip`裁剪内容时，`padding`会让内容撑大，故`width`、`height`要相应缩小

```css
.choose-box input[type='radio'] + label::before {
  content: '';
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 1px solid #0069ab;
  margin-right: 15px;
}
.choose-box input[type='radio']:checked + label::before {
  background-clip: content-box;
  background-color: #0069ab;
  width: 6px;
  height: 6px;
  padding: 2px;
}
```

2. 隐藏原始按钮

```css
.choose-box input[type='radio'] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
}
```

3. 让按钮和文字水平居中对齐

```css
.choose-box label {
  display: inline-flex;
  flex-direction: row;
  align-items: center;
}
```

#### 控件置于 label 内

> input 嵌套在 label 标签内，不是并行的关系了，给 label 添加伪类用不了 input 标签的 checked 属性。故考虑直接给 input 标签添加伪类来实现

```html
<label for="yes" class="choose-box"><input type="radio" id="yes" name="choose" />是</label>
<label for="no" class="choose-box"><input type="radio" id="no" name="choose" />否</label>
```

- 给 input 标签添加伪类，不能隐藏原始的 input 的按钮，也不能使用 display:none; 以及 opacity:0; 等方式隐藏，会直接隐藏到 input 的伪类的展示，可以通过**设置 `input` 标签的`width:0; height:0` 来实现**

```css
.choose-box {
  padding-left: 10px;
  position: relative;
  display: inline-block;
  height: 40px;
  line-height: 40px;
}
.choose-box input[type='radio']::before {
  content: '';
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 1px solid #0069ab;
  margin-right: 15px;
  position: absolute;
  top: 15px;
  left: 0;
}
.choose-box input[type='radio']:checked::before {
  background-clip: content-box;
  background-color: #0069ab;
  width: 6px;
  height: 6px;
  padding: 2px;
}
.choose-box input[type='radio'] {
  width: 0;
  height: 0;
}
```

## JavaScript

### **Question:** 实现 convert 方法，把原始 list 转换成树形结构，要求尽可能降低时间复杂度

```js
// 原始 list 如下
let list =[
    {id:1,name:'部门A',parentId:0},
    {id:2,name:'部门B',parentId:0},
    {id:3,name:'部门C',parentId:1},
    {id:4,name:'部门D',parentId:1},
    {id:5,name:'部门E',parentId:2},
    {id:6,name:'部门F',parentId:3},
    {id:7,name:'部门G',parentId:2},
    {id:8,name:'部门H',parentId:4}
];
const result = convert(list, ...);

// 转换后的结果如下
let result = [
    {
      id: 1,
      name: '部门A',
      parentId: 0,
      children: [
        {
          id: 3,
          name: '部门C',
          parentId: 1,
          children: [
            {
              id: 6,
              name: '部门F',
              parentId: 3
            }, {
              id: 16,
              name: '部门L',
              parentId: 3
            }
          ]
        },
        {
          id: 4,
          name: '部门D',
          parentId: 1,
          children: [
            {
              id: 8,
              name: '部门H',
              parentId: 4
            }
          ]
        }
      ]
    },
  ···
];
```

---

```js
// O(n)
function convert(list) {
  const res = [];
  const map = list.reduce((res, v) => ((res[v.id] = v), res), {});
  for (const item of list) {
    if (item.parentId === 0) {
      res.push(item);
      continue;
    }
    if (item.parentId in map) {
      const parent = map[item.parentId];
      parent.children = parent.children || [];
      parent.children.push(item);
    }
  }
  return res;
}
```

```js
function convert(list, topid = 0) {
  const result = [];
  let map = {};

  list.forEach((item) => {
    const { id, parentId } = item;

    if (parentId === topid) {
      result.push(item);
    } else {
      map[parentId] = map[parentId] || [];

      map[parentId].push(item);
    }

    item.children = item.children || map[id] || (map[id] = []);
  });
  map = null;

  return result;
}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[前端进阶](https://muyiy.cn/question/)

[自定义原生单选按钮 input[type="radio"]样式的两种方式](https://www.jianshu.com/p/fd41c203599e)
