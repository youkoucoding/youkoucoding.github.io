---
date: 2022-01-12T17:11:59+09:00
title: '毎日のフロントエンド  118'
description: frontend 每日一练
image: frontend-118-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百一十八日

## HTML

### **Question:** HTML5 的跟踪元素`<track>` (轨道)

跟踪元素 - 向视频和音频提供字幕和说明文字的方法

```html
<video width="320" height="240" controls>
  <source src="forrest_gump.mp4" type="video/mp4" />
  <source src="forrest_gump.ogg" type="video/ogg" />
  <track src="subtitles_en.vtt" kind="subtitles" srclang="en" label="English" />
  <track
    src="subtitles_no.vtt"
    kind="subtitles"
    srclang="no"
    label="Norwegian"
  />
</video>
```

## CSS

### **Question:** 哪些元素能触发 GPU 硬件加速

- GPU: 用于渲染页面
- 在 css 中使用 `transform: translateZ(0)`, `opacity`, `filter`等,可以开启 GPU 硬件加速

## Javascript

### **Question:** 使用正则去掉 html 中标签与标签之间的空格

```js
htmlStr.replace(/>\s+</g, '><');
```

---

### **Question:** 骨架屏 - Skeleton Screen

#### 为什么需要

- 在最开始关于 MIT 2014 年的研究中已有提到，用户大概会在 200ms 内获取到界面的具体关注点，在数据获取或页面加载完成之前，给用户首先展现骨架屏，骨架屏的样式、布局和真实数据渲染的页面保持一致，这样用户在骨架屏中获取到关注点，并能够预知页面什么地方将要展示文字什么地方展示图片，这样也就能够将关注焦点移到感兴趣的位置。当真实数据获取后，用真实数据渲染的页面替换骨架屏，如果整个过程在 1s 以内，用户几乎感知不到数据的加载过程和最终渲染的页面替换骨架屏，而在用户的感知上，出现骨架屏那一刻数据已经获取到了，而后只是数据渐进式的渲染出来。这样用户感知页面加载更快了。

- 三大框架有一个共同的特点，都是 JS 驱动，在 JS 代码解析完成之前，页面不会展示任何内容，也就是所谓的白屏。用户是极其不喜欢看到白屏的，什么都没有展示，用户很有可能怀疑网络或者应用出了什么问题。 例如： Vue，在应用启动时，Vue 会对组件中的 data 和 computed 中状态值通过 `Object.defineProperty` 方法转化成 set、get 访问属性，以便对数据变化进行监听。而这一过程都是在启动应用时完成的，这也势必导致页面启动阶段比非 JS 驱动（比如 jQuery 应用）的页面要慢一些

#### 如何构建

> 手写 skeleton Screen 的代价：每次需求的变更我们不仅需要修改业务代码， 同时也要去修改骨架屏的样式和布局，这往往是比较机械重复的工作，手写骨架屏增加了维护成本。

- 那么为什么不直接使用服务端渲染`nuxtjs` `nextjs`等，来加快内容展现？

  - 服务端渲染主要有两个目的，一是 SEO，二是加快内容展现。在带来这两个好处的同时，也需要评估服务端渲染的成本，首先需要服务端的支持，因此涉及到服务构建、部署等，同时如果 web 项目是一个流量较大的网站，也需要考虑服务器的负载，以及相应的缓存策略。尤其是，不同用户看到的页面也是不一样的，也就是所谓的千人千面，也为缓存造成了一定困难。

- 为什么不使用 预渲染（prerender）
  - 所谓预渲染，就是在项目的构建过程中，通过一些渲染机制，比如 puppeteer[^1] 或则 jsdom[^2] 将页面在构建的过程中就渲染好，然后插入到 html 中，这样在页面启动之前首先看到的就是预渲染的页面了。
  - 预渲染渲染的页面数据是在构建过程中就已经打包到了 html 中， 当真实访问页面的时候，真实数据可能已经和预渲染的数据有了很大的出入，而且预渲染的页面也是一个不可交互的页面，在页面没有启动之前，用户无法和预渲染的页面进行任何交互，预渲染页面中的数据反而会影响到用户获取真实的信息，当涉及到一些重要数据，甚至会导致用户做出一些错误的决定。

##### 生成骨架屏基本方案

通过 puppeteer[^1] 在服务端使用 `headless Chrome` 打开开发中的需要生成骨架屏的页面，在等待页面加载渲染完成之后，在保留页面布局样式的前提下，通过对页面中元素进行删减或增添，对已有元素通过层叠样式进行覆盖，这样达到在不改变页面布局下，隐藏图片和文字，通过样式覆盖，使得其展示为灰色块。然后将修改后的 HTML 和 CSS 样式提取出来，这样就可以生成想要的骨架屏了。

##### 具体生成过程

- 将页面划分为不同的块，然后分别对每个块进行处理，这样不会破坏页面整体的样式和布局，当我们最终生成骨架屏后，骨架屏的布局样式将和真实页面的布局样式完全一致，这样就达到了复用样式及页面布局的目的。

  - 文本块：仅包含**文本节点**（NodeType 为 Node.TEXT_NODE）的元素（NodeType 为 Node.ELEMENT_NODE），一个文本块可能是一个 p 元素也可能是 div 等。文本块将会被转化为灰色条纹
  - 图片块：图片块是很好区分的，任何 img 元素都将被视为图片块，图片块的颜色将被处理成配置的颜色，形状也被修改为配置的矩形或者圆型
  - 按钮块：任何 button 元素、 type 为 button 的 input 元素，role 为 button 的 a 元素，都将被视为按钮块。按钮块中的文本块不在处理
  - svg 块：任何最外层是 svg 的元素都被视为 svg 块
  - 伪类元素块：任何伪类元素都将视为伪类元素块，如 ::before 或者 ::after

[^1]: [puppeteer/puppeteer: Headless Chrome Node.js API](https://github.com/puppeteer/puppeteer)
[^2]: [jsdom - npm](https://www.npmjs.com/package/jsdom)

---

1. 前提：将生成骨架屏的脚本，插入到 `puppeteer` 打开的页面中，这样才能够执行脚本，并最终生成骨架屏。

- puppetter 原生方法，`page.addScriptTag(options)`，插入一段 js 脚本的 url 或者是相对/绝对路径，也可以直接是 js 脚本的内容。

```js
  async makeSkeleton(page) {
    const { defer } = this.options
    await page.addScriptTag({ content: this.scriptContent })
    await sleep(defer)
    await page.evaluate((options) => {
      Skeleton.genSkeleton(options)
    }, this.options)
  }
```

- 通过上面插入的脚本，同时在插入的脚本中提供了一个全局对象 `Skeleton`，这样就可以直接通过 `page.evaluate` 方法来执行脚本内容并最终生成骨架页面了。

---

2. 文本块的骨架结构生成

- 任何只包含文本节点的元素都将视为文本块，在确定某个元素是文本块后，下一步就是通过一些 CSS 样式，以及元素的增减将其修改为骨架样式。
- 在生成文本块骨架屏之前，我们首先需要了解一些基本的参数。

  - 单行文本内容的高度，可以通过 `fontSize` 获取到
  - 单行文本内容加空白间隙的高度，可以通过 `lineHeight` 获取到
  - `p` 元素总共有多少行文本，也就是所谓行数，这个可以通过（height - paddingTop - paddingBottom）/ lineHeight 计算出大致结果
  - 文本的 textAlign 属性

    > fontSize、lineHeight、paddingTop、paddingBottom 都可以通过 getComputedStyle 获取

    > 元素的高度 height 可以通过 getBoundingClientRect 获取

[@Lea Verou - css secretz](CSS-secrets.png)

- 文本块的骨架屏就是是通过线性渐变来绘制的。核心简化代码：

```js
const textHeightRatio = parseFloat(fontSize, 10) / parseFloat(lineHeight, 10);
const firstColorPoint = (((1 - textHeightRatio) / 2) * 100).toFixed(decimal);
const secondColorPoint = (
  ((1 - textHeightRatio) / 2 + textHeightRatio) *
  100
).toFixed(decimal);

const rule = `{
  background-image: linear-gradient(
    transparent ${firstColorPoint}%, ${color} 0%,
    ${color} ${secondColorPoint}%, transparent 0%);
  background-size: 100% ${lineHeight};
  position: ${position};
  background-origin: content-box;
  background-clip: content-box;
  background-color: transparent;
  color: transparent;
  background-repeat: repeat-y;
}`;
```

首先计算了 lineHeight 和 fontSize 等一些样式参数，通过这些参数我们计算出了文本占整个行高的比值，也就是 textHeightRadio，有了这一比值，就可以知道灰色条纹的分界点

> **CSS Secrets** “If a color stop has a position that is less than the specied position of any color stop before it in the list, set its position to be equal to the largest speci ed position of any color stop before it.” — CSS Images Level 3 (http://w3.org/TR/css3-images)

在线性渐变中，如果将线性渐变的起始点设置小于前一个颜色点的起始值，或者设置为 0%，那么线性渐变将会消失，取而代之的将是两条颜色分明的条纹，也就是说不再有线性渐变。

绘制文本块的时候，backgroundSize 宽度为 100%， 高度为 lineHeight，也就是灰色条纹加透明条纹的高度是 lineHeight。虽然把灰色条纹绘制出来了，但是，文字依然显示，在最终骨架样式效果出现之前，我们还需要隐藏文字，设置 color：‘transparent’ 这样文字就和背景色一致，最终显示得也就是灰色条纹了。

根据 `lineCount` 可以判断文本块是单行文本还是多行，在处理单行文本的时候，由于文本的宽度并没有整行宽度，因此，针对单行文本，还需要计算出文本的宽度，然后设置灰色条纹的宽度为文本宽度，这样骨架样式的效果才能够更加接近文本样式。

---

3. 图片块的骨架生成

选择了将一张 1 \* 1 像素的 gif 透明图片，转化成 dataUrl ，然后将其赋予给 IMG 元素的 src 特性上，同时设置图片的 width 和 height 特性为之前图片的宽高，将背景色调至为骨架样式所配置的颜色值

```js
// 1 * 1像素的 base64 格式的透明 gif 图片
'data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7';
```

```js
function imgHandler(ele, { color, shape, shapeOpposite }) {
  const { width, height } = ele.getBoundingClientRect();
  const attrs = {
    width,
    height,
    src,
  };

  const finalShape =
    shapeOpposite.indexOf(ele) > -1 ? getOppositeShape(shape) : shape;

  setAttributes(ele, attrs);

  const className = CLASS_NAME_PREFEX + 'image';
  const shapeName = CLASS_NAME_PREFEX + finalShape;
  const rule = `{
    background: ${color} !important;
  }`;
  addStyle(`.${className}`, rule);
  shapeStyle(finalShape);

  addClassName(ele, [className, shapeName]);

  if (ele.hasAttribute('alt')) {
    ele.removeAttribute('alt');
  }
}
```

---

4. svg 块骨架结构

判断 svg 元素 hidden 属性是否为 true，如果为 true，说明该元素不展示的，可以直接删除该元素

```js
if (width === 0 || height === 0 || ele.getAttribute('hidden') === 'true') {
  return removeElement(ele);
}
```

如果不是隐藏的元素，那么我们将会把 svg 元素内部所有元素删除，减少最终生成的骨架页面体积，其次，设置 svg 元素的宽、高和形状等

```js
const shapeClassName = CLASS_NAME_PREFEX + shape;
shapeStyle(shape);

Object.assign(ele.style, {
  width: px2relativeUtil(width, cssUnit, decimal),
  height: px2relativeUtil(height, cssUnit, decimal),
});

addClassName(ele, [shapeClassName]);

if (color === TRANSPARENT) {
  setOpacity(ele);
} else {
  const className = CLASS_NAME_PREFEX + 'svg';
  const rule = `{
    background: ${color} !important;
  }`;
  addStyle(`.${className}`, rule);
  ele.classList.add(className);
}
```

---

5. 优化细节

- 首先，由上面一些代码可以看出，在生成骨架页面的过程中，将所有的共用样式通过 `addStyle` 方法缓存起来，最后在生成骨架屏的时候，统一通过 style 标签插入到骨架屏中。这样保证了样式尽可能多的复用

- 其次，在处理列表的时候，为了生成骨架屏尽可能美观，对列表进行了同化处理，也就是说将 list 中所有的 listItem 都是同一个 listItem 的克隆。这样生成的 list 的骨架屏样式就更加统一了

- 骨架屏仅是一种加载状态，并非真实页面，因此其并不需要完整的页面，其实只需要首屏就好了，对非首屏的元素进行了删除，只保留了首屏内部元素，这样也大大缩减了生成骨架屏的体积

- 删除无用的 CSS 样式，只是我们只提取了对骨架屏有用的 CSS，然后通过 style 标签引入

```js
// 大致代码
const checker = (selector) => {
  if (DEAD_OBVIOUS.has(selector)) {
    return true;
  }
  if (/:-(ms|moz)-/.test(selector)) {
    return true;
  }
  if (/:{1,2}(before|after)/.test(selector)) {
    return true;
  }
  try {
    const keep = !!document.querySelector(selector);
    return keep;
  } catch (err) {
    const exception = err.toString();
    console.log(
      `Unable to querySelector('${selector}') [${exception}]`,
      'error'
    );
    return false;
  }
};
```

- 主要通过 document.querySelector 方法来判断该 CSS 是否被使用到，如果该 CSS 选择器能够选择上元素，说明该 CSS 样式是有用的，保留。如果没有选择上元素，说明该 CSS 样式没有用到，移除

##### 通过 webpack 将骨架屏打包到项目中

Webpack 在整个打包的过程中提供了众多生命周期事件，比如 compilation 、after-emit 等，比如最终将骨架屏插入到 html 中就是在 after-emit 钩子函数中进行

```js
//大致代码

SkeletonPlugin.prototype.apply = function (compiler) {
  // 其他代码
  compiler.plugin('after-emit', async (compilation, done) => {
    try {
      await outputSkeletonScreen(
        this.originalHtml,
        this.options,
        this.server.log.info
      );
    } catch (err) {
      this.server.log.warn(err.toString());
    }
    done();
  });
  // 其他代码
};
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[Media accessibility](https://web.dev/media-accessibility/)

[一种自动化生成骨架屏的方案](https://github.com/Jocs/jocs.github.io/issues/22)
