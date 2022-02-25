---
title: '毎日のフロントエンド  162'
date: 2022-02-25T10:13:29+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
  - JavaScript
---

## HTML

### `output`

- 定义表单输出
- 有以下属性
  - `for`: `<element_id>` 定义输出域相关的一个或多个元素
  - `form`: `<form_id>` 定义输入字段所属的一个或多个表单
  - `name`: 定义对象的唯一名称。（表单提交时使用）

```html
<form id="form" oninput="x.value=parseInt(a.value)+parseInt(b.value)">
  0<input type="range" id="a" value="50" />100 +<input type="number" id="b" value="50" />
</form>
<output form="form" name="x" for="a b"></output>
```

## CSS

### 移动端的布局媒体查询

## JavaScript

### 图片加水印

```js
/**
 *
 * @param {*} image 图片 img对象
 * @param {*} words 水印内容
 */
export default function (image, words) {
  const { canvas, context } = getCanvas();
  let width = image.width;
  let height = image.height;
  canvas.width = width;
  canvas.height = height;
  canvas.style.width = image.style.width;
  canvas.style.height = image.style.height;

  context.drawImage(image, 0, 0);

  // 重复绘制内容贴图
  const mark = getMark(words);
  for (let i = 0; i < width; i += 250) {
    for (let j = 0; j < height; j += 250) {
      context.drawImage(mark, i, j);
    }
  }
  return toImage(canvas);
}

function getCanvas() {
  const canvas = document.createElement('canvas');
  return {
    canvas,
    context: canvas.getContext('2d'),
  };
}

// 构造内容
function getMark(content = '') {
  const { canvas, context } = getCanvas();
  canvas.width = 200;
  canvas.height = 200;
  context.translate(100, 100);
  context.rotate((45 * Math.PI) / 180);
  context.font = '30px 微软雅黑';
  context.textAlign = 'center';
  // 隐形水印, 肉眼不可见,图片被人下载后可用ps查看
  // content 一般是登录的信息，用以内部截图外泄后查清截图人身份用
  context.fillStyle = 'rgba(220,20,60, 0.005)';

  const words = content.split('\n'); // 以\n为换行
  const lines = words.length;
  const fontHeight = context.measureText('田').width * 1.1;
  const wordsHeight = fontHeight * lines;
  const start = -wordsHeight / 2;
  for (let i = 0; i < lines; i++) {
    context.fillText(words[i], 0, start + i * fontHeight);
  }
  return canvas;
}

function toImage(canvas) {
  var image = new Image();
  image.src = canvas.toDataURL('image/png');
  return image;
}
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[`<output>` - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/output)
