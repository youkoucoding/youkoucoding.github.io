---
title: '毎日のフロントエンド 208'
date: 2022-04-17T14:48:54+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### FONT

#### 字体的分类

- 字体族，是一系列字体的集合:
  - serif(衬线)
  - sans-serif(无衬线)
  - monospace(等宽)
  - fantasy(梦幻)
  - cuisive(草体)

#### `serif`(衬线字体) `sans-serif`(无衬线字体)

- `Serif`的意思是，在字的笔划开始及结束的地方有额外的装饰，而且笔画的粗细会有所不同。衬线字体的风格都比较突出，**常见的衬线字体有 Times New Roman、宋体**。
- `Sans Serif`字体没有这些额外的装饰，笔划粗细大致差不多，字形端庄，横平竖直，**常见的无衬线字体有 Tahoma、Verdana、Arial、Helvetica、苹方、微软雅黑等等**
- `monospace`(等宽字体)等宽字体是指字符宽度相同的字体，通常用于编辑器以及技术文章的代码块中，等宽主要针对西文字体，而对于中文每个字都是等宽的，**courier 是最常见的等宽字体**

#### `font-family`属性

- 设置元素的字体，可以同时指定多个，如果浏览器不支持第一个字体，则会尝试下一个，可以设置字体或字体系列。`font-family: Arial, sans-serif;`
- 不设置 font-family 则使用浏览器默认字体，如果设置的 font-family 无效，也会 fallback 到**浏览器的默认字体**

#### 常见字体

- **Helvetica**
  - 苹果系统支持的一种**西文无衬线字体**，是苹果生态中最常用的一套西文字体。Helvetica Neue 是 Helvetica 字体改善版，增加了更多不同粗细与宽度的字形
- **Arial**
  - 是为了与 Helvetica 竞争而设计的无衬线西文字体，表现形式和 Helvetica 类似，在不同系统的浏览器都支持，_兼容性非常好_
- **Tahoma**
  - 一种无衬线字体，间距较小，在不同系统的浏览器都支持，兼容性良好，可以解决 Helvetica 和 Arial 所为人诟病的缺点，比如大写的 I 和小写的 L 难以分辨。
- **San Francisco**
  - 苹果于 2017 年推出一种无衬线字体，也是目前苹果系统的默认西文字体，相比于 Helvetica 字体，San Francisco 的字体风格更加简洁，减少了一些修饰的细节，支持符号的整体居中，比如时间显示，之前的 Helvetica 的冒号是不居中的
- **PingFang SC(苹方-简)**
  - 中文无衬线字体，在 2017 年和 San Francisco 一起推出，SC 代表简体，同时还有台湾繁体和香港繁体，整体造型简洁美观，是苹果系统默认的中文字体。
- **Hiragino Sans GB(冬青黑体)、Heiti SC(黑体)**
  - 苹果系统中较早的中文无衬线字体，为了兼容旧版 macOS 系统，我们一般用它们作为苹方字体的 fallback。
- **Segoe UI**
  - *windows 系统*下的一种无衬线西文字体，也是 windows 系统的**默认西文字体**。
- **Microsoft YaHei(微软雅黑)**
  - Windows 系统默认的中文字体，也是一套无衬线字体。macOS 上的浏览器大都预装微软雅黑，但不包括 safari 浏览器(_ios 和 android 系统不支持微软雅黑，所以设置移动端字体时可以忽略微软雅黑_)
- **Roboto**
  - Android 系统的默认西文字体，也是一种无衬线字体
- **Noto Sans (思源黑体)**
  - Android 系统的默认中文无衬线字体，由 google 推出的一款开源字体
- **Apple Color Emoji**
  - 苹果产品的文字表情，在 Mac 和 iOS 系统都支持
- **Segoe UI Emoji**
  - Windows10 系统中的 Emoji 表情，黑描边风格，没有苹果的圆润和质感。
- **Noto Color Emoji**
  - Google 推出的表情，和苹果的较为类似，更加扁平

#### 浏览器默认字体

> 默认字体分为系统默认字体(system-ui)和浏览器默认字体，这两者是不同的

- 当元素没有指定 font-family 或者设置的 font-family 无效时，会 fallback 到浏览器默认字体。

- 浏览器默认字体，可在浏览器 setting 里查看

- 通常在 font-family 最后设置`sans-serif`指定无衬线字体作为兜底

#### system-ui

- system-ui 是一种通用字体系列，它选择**当前操作系统下的默认系统字体**，它的优势在于和当前系统使用的字体相匹配，可以让 Web 页面和 App 风格看起来更统一
- `-apple-system`
  - `-apple-system` 是 system-ui 的兼容写法，只在 ios 和 macOS 上的 safari、Firefox、webview 等环境下支持。
- BlinkMacSystemFont

  - 也是 system-ui 的兼容写法，只在 macOS chrome 下支持，主要针对 chrome 53-55 版本

- _在 ios 和 macOS 上，system-ui 指向的中文字体为苹方，西文字体为 San Francisco。android 系统下中文通常为 Noto Sans (思源黑体)，西文字体为 Roboto。windows 系统上一般为微软雅黑，但是在部分 windows 系统上的字体会出现问题，所以 windows 上不建议使用 system-ui_

---

- 在现行主流终端设备中，无衬线体比有衬线体更易读，**无衬线体 `sans-serif`**更适合作为网页的默认全局字体设置
- 每个系统都会带有无衬线字体，所以 sans-serif 一般放在最后作为兜底，sans-serif 之后的字体都不会生效，除了 Emoji 字体
- PC 端浏览器可以在设置中指定 sans-serif 字体。
- 移动端浏览器不能在设置中指定 sans-serif 字体，它们会根据 lang 指定的语言环境选择合适的字体，和 system-ui 指定的字体不一定相同。

#### 书写字体规则

1. 不同平台，预置的字体并不相同。比如**Helvetica 和苹方也只有苹果系统内置**，**微软雅黑只有 windows 系统内置**(由于安装 Office365 的缘故，Mac 电脑中也会出现微软雅黑字体)，android 和 iOS 系统内置的字体各不相同

2. 字体是有版权的，但是如果没有引用外部字体文件（如 Web font 或者内嵌到 App 中），而仅仅是在 CSS 中指定字体名称，不需要购买授权，因为其只是一段声明，表示期望浏览器优先使用某种字体渲染文本。

3. 中文网站涉及两种文字类型：西文字体和中文字体，西文字体包括英文数字等，不包括中文，但是中文字体一般包括英文和数字，**一般先设置西文字体，后设置中文**

4. 如果字体包含空格或者是中文，需要加引号

5. 大部分字体全名中会包含字体粗细、斜体（italic）、长体（condensed）等具体属性，但一般不建议直接使用，字体的风格应该在 CSS 中通过 font-weight、font-style、font-stretch 等属性指定，由浏览器决定实际渲染的字体

6. font-family 属于**可继承属性**，全局的 font-family 一般设置在 body 元素上

---

- **西文在前，中文在后**
- **优先使用 `system-ui`**: 让 web 页面和操作系统的风格统一，体验更好
- **兼顾不同的操作系统**, 比如都使用无衬线字体
  - 保证苹果系统中使用更优雅的中文字体，优先声明苹方字体, 对于不支持苹方的低版本 macOS，使用 Hiragino Sans GB(冬青黑体)字体做兜底
  - 如果需要兼容不同平台的表情符号，一般在 sans-serif 后添加"Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji"等字体
- **以 serif 或 sans-serif 字体族结尾**
  - 一般将 sans-serif 放在字体的后面，_非衬线字体在显示器中的显示效果更好_
- **简洁实用**
  - 字体设置并不是越多越好，在能满足设计需求的情况下尽量简洁。相同系统下中西文字体各有一个 fallback 即可，不需要太多

---

#### `font-family`推荐写法

##### 移动端 兼容版本：ios9+、android4+

- `font-family: system-ui, -apple-system, Arial, sans-serif;`
  - 优先使用 system-ui，使用 Arial 做西文字体的 fallback 是因为它和 Helvetica 字体相似，并且在 ios 和 android 支持性很好。

##### PC 端

1. macOS 系统优先使用系统字体

- `font-family: -apple-system, BlinkMacSystemFont, Tahoma, Arial, "Hiragino Sans GB", "Microsoft YaHei", sans-serif;`

2. 指定字体
   - `font-family: Tahoma, Arial, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;`
   - 理由：
     - windows 系统电脑屏幕分辨率普遍不高，`Tahoma` 字体在小字号下结构清晰端整、阅读辨识容易，在不同操作系统支持性好，所以作为首选字体，如果系统没有预装 Tahoma，则使用 Arial 作为替代。但两者差别不大，Arial 优先级提前也行。
   - **如想突出 macOS 和 windows 的字体特色，可以在 Tahoma 前面设置 Helvetica 或 Segoe UI 为首选字体**
     - `font-family: "Helvetica Neue", "Segoe UI", Arial, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;`

---

- css 的 font-family 权重是高于浏览器默认字体
- 当给某个元素设置了 font-faimily 后，全局默认字体则对这个元素无效，这时也要考虑字体兼容问题，最好指定一个 fallback 字体，并以 sans-serif 结尾

```css
div {
  font-faimiy: 'PingFang SC', sans-serif;
}
```

## Tips

### TypeScript

- Generics can be "Locked in " by function calls, meaning that generics can be "curried" through functions.

```ts
export const makeKeyRemove = () => () => {};

const keyRemover = makeKeyRemover(['a', 'b']);

const newObject = keyRemover({ a: 1, b: 2, c: 3 });
```

- Solution

```ts
// the function makeKeyRemover will return a new function that can remove the key of a object
export const makeKeyRemover =
  <Key extends string>(keys: Key[]) =>
  <Obj>(obj: Obj): Omit<Obj, Key> => {
    return {} as any;
  };

const keyRemover = makeKeyRemover(['a', 'b']);

const newObject = keyRemover({ a: 1, b: 2, c: 3 });
```

- [Playground Link](https://www.typescriptlang.org/play?jsx=0#code/PTAEBcAsFNQMwK4DsDG4CWB7JoC2BDAa2gGloBPAJWl0wDdoAnUAd3QBt3RHpwFGc+UEmgt4yNFhxR84UCnw4etBhBihi5UJjighmAEYAraGgCwAKBTYAznILEyVGvSagAvJdCgAPE9DQAB7g0EgAJjagdozoSADmAHwAFJo2AFygTgDaALoAlB4JXr4A8sbJhkYZZUZ51bjo4D41ADSZFAmFoADexd48fAI9AL56kYrkANzFw9MWltZIdhoU1Cpu7nhEpKsuDIxJWQDk+EdtRwZH+XMLtnIiLDWmcpuaa64H3XoZAIxtBhkAExtFAZADMoGGeUmQA)

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [pointer-events - CSS: Cascading Style Sheets | MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events)

- [Console - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Console)

- [Web 字体 font-family](https://segmentfault.com/a/1190000038284125)

- [CSS Font Stack: Web Safe and Web Font Family with HTML and CSS code.](https://www.cssfontstack.com/)
