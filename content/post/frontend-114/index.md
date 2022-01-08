---
title: '毎日のフロントエンド  114'
date: 2022-01-08T11:42:58+09:00
description: frontend 每日一练
image: frontend-114-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百一十四日

## HTML

### **Question:** HTML5 的 Canvas 元素有什么用途

- 最常见的就是做图表,数据可视化产品
- 做动画特效,在线画图,3d 的 webgl 有 threeJs, 2d 的有 zrender
- web 图像处理,在 canvas 上画图片,进行像素级的修改，如制作灰度图，对用户上传的图进行裁剪,模糊,多图合成等操作

## CSS

### **Question:** 为什么要使用 css sprites(old)

- CSS Sprites 指的是将该页面请求的图片资源都拼接到一个图片文件上，通过 css 从拼接好的图片上获取需要的部分。
- CSS Sprites are a means of combining multiple images into a single image file for use on a website, to help with performance.

- 优点:
  1. 一个是减少了 HTTP 请求的发送次数
  2. 一个是合并后的图片一般小于合并前的图片大小总和（大图大小<=多张小图大小）
- 缺点：
  - 如果发生改动需要重新做拼接

## JavaScript

### **Question:** 写一个把数字转成中文的方法，例如：101 转成一百零一

`~~`(按位非(32 位)) 按位操作符只操作整数部分，非整数部分会被舍弃，可用于取整操作

```js
function transformNumber(number) {
  const mapping = {
    100000000: '亿',
    10000000: '千万',
    1000000: '百万',
    100000: '十万',
    10000: '万',
    1000: '千',
    100: '百',
    10: '十',
    0: '零',
    1: '一',
    2: '二',
    3: '三',
    4: '四',
    5: '五',
    6: '六',
    7: '七',
    8: '八',
    9: '九',
  };
  const keys = Object.keys(mapping).sort((a, b) => b - a);
  let retStr = '',
    temp = number,
    tempStr;
  for (let k of keys) {
    // The “double tilde” (~~) operator is a double NOT Bitwise operator. Use it as a substitute for Math.floor(), since it’s faster.
    tempStr = ~~(temp / k);
    temp = temp % k;
    if (tempStr) {
      retStr += `${mapping[tempStr]}${mapping[k]}`;
    } else if (mapping[k] === '十' && tempStr === 1) {
      retStr += '一十'; // 特殊处理 10 读作 一十
    } else if (!tempStr && retStr && retStr[retStr.length - 1] !== '零') {
      retStr += '零';
    } else if (temp < 10) {
      if (temp !== 0) {
        // 特殊处理末尾 0
        retStr += mapping[temp];
      }
      break; // 已到达个位数，退出循环
    }
    if (!temp) break;
  }
  console.log(retStr);
}
transformNumber(10101101); // 一千万零一十万零一千一百零一
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[Canvas - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)

[CSS Sprites: What They Are, Why They're Cool, and How To Use Them | CSS-Tricks - CSS-Tricks](https://css-tricks.com/css-sprites/)

[表达式与运算符 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Expressions_and_Operators#%E4%BD%8D%E8%BF%90%E7%AE%97%E7%AC%A6)
