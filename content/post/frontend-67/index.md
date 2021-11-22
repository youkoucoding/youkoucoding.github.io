---
title: '毎日のフロントエンド　 67'
date: 2021-11-22T14:50:01+09:00
description: frontend 每日一练
image: frontend-67-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第六十七日

## HTML

### **Question:** 请写出唤醒拔打电话、发送邮件、发送短信的例子

```html
<a href="tel:1xxxxxxxx">一键拨打号码</a>
<a href="mailto:xxxxxx@xxxx.com">一键发送邮件</a>
<a href="sms:1xxxxxxxxx">一键发送短信</a>
```

## CSS

### **Question:** 如何消除 `transition` 闪屏

```css
.css {
  -webkit-transform-style: preserve-3d;
  -webkit-backface-visibility: hidden;
  -webkit-perspective: 1000;
}
```

## JavaScript

### **Question:** 举例子说说对 js 隐式类型转换的理解

1. 字符串连接符(+)，转换为 String
2. 关系运算符(`>,<,>=,<=,==`)、算术运算符号(`+,-,*,/,%,++,--`)，转换为`Number`
3. 逻辑非运算符(`!`)，转换为`Boolean`

#### note:

1. 数组、对象等复杂数据类型在隐式转换时会先使用`valueOf()`获取其原始值，如果原始值不是 `Number` 则调用 `toString()` 转成 `Sting`，再转成 `Number`

2. `Boolean`转换为`false`： `0，-0，NaN，undefined，""，null，[]，false`

3. `undefined`与`null`的特殊情况

```js
undefined == undefined; // true
undefined == null; // true
null == null; // true
```

4. `NaN`与任何数据比较都是 false，包括自己。

### **Question:** 手写数组转树

```js
// 递归实现
let input = [
  {
    id: 1,
    val: '学校',
    parentId: null,
  },
  {
    id: 2,
    val: '班级1',
    parentId: 1,
  },
  {
    id: 3,
    val: '班级2',
    parentId: 1,
  },
  {
    id: 4,
    val: '学生1',
    parentId: 2,
  },
  {
    id: 5,
    val: '学生2',
    parentId: 3,
  },
  {
    id: 6,
    val: '学生3',
    parentId: 3,
  },
];

function buildTree(arr, parentId, childrenArray) {
  arr.forEach((item) => {
    if (item.parentId === parentId) {
      item.children = [];
      buildTree(arr, item.id, item.children);
      childrenArray.push(item);
    }
  });
}
function arrayToTree(input, parentId) {
  const array = [];
  buildTree(input, parentId, array);
  return array.length > 0 ? (array.length > 1 ? array : array[0]) : {};
}
const obj = arrayToTree(input, null);
console.log(obj);
```

---

```js
// reduce 函数 生成： map 数据结构
let list = [
  { id: 1, name: '部门A', parentId: 0 },
  { id: 3, name: '部门C', parentId: 1 },
  { id: 4, name: '部门D', parentId: 1 },
  { id: 5, name: '部门E', parentId: 2 },
  { id: 6, name: '部门F', parentId: 3 },
  { id: 7, name: '部门G', parentId: 2 },
  { id: 8, name: '部门H', parentId: 4 },
];

//
function convert(list) {
  // reduce callback first parameter `acc`=> 累加值，初始化为 一个空对象 {}
  // item => list 的 第一个元素
  // 返回 acc => 一个以 数组元素的id 为 key， 以 此元素为 value的键值对，的对象 （map）。
  // 相当于map = new Map() 类 map结构
  const map = list.reduce((acc, item) => {
    acc[item.id] = item;
    return acc;
  }, {});

  const result = [];
  // reduce函数返回的对象 便于进行 for in 循环。 由操作一个数组，转化操作一个对象。

  for (const key in map) {
    // 拿到每一个原始元素id 对应的元素
    const item = map[key];
    if (item.parentId === 0) {
      result.push(item);
    } else {
      // 使用map结构的方便之处： 直接通过匹配的parentId 找到对应的parent_item
      // 用本轮循环中的item的父id，直接从map中取得对应的父元素，
      // 增加children属性，向children属性添加数组形式的子元素
      const parent = map[item.parentId];
      if (parent) {
        parent.children = parent.children || [];
        parent.children.push(item);
      } else {
        result.push(item); // 要加上else，保证匹配不到 parent_id 的元素被保留
      }
    }
  }
  return result;
}

const result = convert(list);
console.log(result);
```

---

一个 “更” map 的写法

```js
type Node<T> = {
  id: number;
  value: T;
  parentId: number;
};

type TreeNode<T> = {
  id: number;
  value: T;
  parentId: number;
  children: TreeNode<T>[];
};

const list: Node<string>[] = [
  { id: 1, value: 'V1', parentId: 0 },
  { id: 3, value: 'V2', parentId: 1 },
  { id: 4, value: 'V3', parentId: 1 },
  { id: 5, value: 'V4', parentId: 2 },
  { id: 6, value: 'V5', parentId: 3 },
  { id: 7, value: 'V6', parentId: 2 },
  { id: 8, value: 'V7', parentId: 4 },
];

const listToTree = <T>(list: Node<T>[]): TreeNode<T> | undefined => {
  const map = list.reduce<Map<number, TreeNode<T>>>((prev, curr) => {
    prev.set(curr.id, { ...curr, children: [] });
    return prev;
  }, new Map());


  let headId = 1;

  map.forEach((treeNode) => {
    const parent = map.get(treeNode.parentId);

    if (parent) {
      parent.children.push(treeNode);
    }

    if(treeNode.parentId === 0) {
      headId = treeNode.id;
    }
  });

  return map.get(headId);
};

```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[lgwebdream/FE-Interview ](https://github.com/lgwebdream/FE-Interview)

[手写数组转树 · lgwebdream/FE-Interview](https://github.com/lgwebdream/FE-Interview/issues/35)
