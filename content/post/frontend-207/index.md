---
title: '毎日のフロントエンド 207'
date: 2022-04-16T15:43:37+09:00
description: frontend 每日一练
categories:
  - CSS
  - Tips
---

## CSS

### `filter` 滤镜

- `filter` 将模糊或颜色偏移等图形效果应用于元素。滤镜通常用于调整图像、背景和边框的渲染。CSS 标准里包含了一些已实现预定义效果的函数。

### Don't Rely on CSS `100vh`

```html
<div className="layout">
  <p>Lorem ipsum dolor sit amet...</p>
  <button>Sign Up</button>
</div>

<style>
  .layout {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 100vh;
  }
</style>
```

- 上述代码，可以在大多是情况下保证`button`, 位于页面底部
- 但是在移动端浏览器上会被浏览器的工具栏遮挡，原因是： [html - CSS3 100vh not constant in mobile browser - Stack Overflow](https://stackoverflow.com/questions/37112218/css3-100vh-not-constant-in-mobile-browser)

- Fix Code

```html
<div className="layout">
  <p>Lorem ipsum dolor sit amet...</p>
  <button>Sign Up</button>
</div>

<style>
  .layout {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 100vh;
  }

  .layout button {
    position: sticky;
    bottom: 0;
  }
</style>
```

#### 其他场景下的：`100vh` bugs

- Create sections equal to the height of the `viewport`

```css
.layout {
  min-height: 100vh; /* fall-back */
  min-height: -moz-available;
  min-height: -webkit-fill-available;
  min-height: fill-available;
}
```

- 上述代码可以在 IOS 系统上工作良好

#### More Issue

1. `fill-available` - 包含`<!DOCTYPE html>`的页面，在 chrome 中不生效
2. Adding vertical padding (bottom and top) to the element that has its min-height (or height) to fill-available causes a problem on the Safari browser and the height won’t be correct
3. fill-available Can’t Be Used with `calc()`

---

#### 终极解决方案 - Fix 100vh Issue on Mobile Devices Using JavaScript

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <style>
      ...;
    </style>
  </head>
  <body>
    <div id="intro">
      <h1>Hello World!</h1>
      <h2>The height of this area is equal to...</h2>
    </div>
    ...
    <script>
      (function () {
        const el = document.getElementById('intro');
        el.style.minHeight = window.innerHeight + 'px';
      })();
    </script>
  </body>
</html>
```

or

```js
// Calculate 1vh value in pixels
// based on window inner height
var vh = window.innerHeight * 0.01;

// Set the CSS variable to the root element
// Which is equal to 1vh
document.documentElement.style.setProperty('--vh', vh + 'px');
```

```css
min-height: calc(var(--vh) * 100);
```

```js
// One final thing would be re-calculating this value when the window
// gets resized or device orientation changes. The complete JavaScript code will be:
function calculateVh() {
  var vh = window.innerHeight * 0.01;
  document.documentElement.style.setProperty('--vh', vh + 'px');
}

// Initial calculation
calculateVh();

// Re-calculate on resize
window.addEventListener('resize', calculateVh);

// Re-calculate on device orientation change
window.addEventListener('orientationchange', calculateVh);
```

## Tips

### TypeScript

- Use `generics` in React to make incredibly dynamic, flexible components.

```ts
interface TableProps {
  items: { id: string }[];
  renderItem: (item: { id: string }) => React.ReactNode;
}

export const Table = (props: TableProps) => {
  return null;
};

const Component = () => {
  return (
    <Table
      items={[
        {
          id: 1,
        },
      ]}
      renderItem={(item) => <div>{item.id}</div>}
    />
  );
};
```

- Solution

```tsx
// In tsx, <T>()=>{} is invalid, so turn the arrow function to the Function definition
interface TableProps<TItem> {
  items: TItem[];
  renderItem: (item: TItem) => React.ReactNode;
}

export function Table<TItem>(props: TableProps<TItem>) {
  return null;
}

const Component = () => {
  return (
    <Table<{ id: number; name: string }> // pass the generic here
      items={[
        {
          id: 1,
          name: 'hello',
        },
      ]}
      renderItem={(item) => <div>{item.id}</div>}
    />
  );
};
```

- [Playground Link](https://www.typescriptlang.org/play?#code/JYWwDg9gTgLgBAJQKYEMDG8BmUIjgcilQ3wFgAoAekrgEkA7OGAZwA8AaOAHgBUA+ABQBKALx8A3gF84wZjPoA3FABtgAE07MITAK5RGMABZI4KKDgDucTDvoZgEA9qMmAYrfuO4apJmD1gGAd6Cn8YJChMdBMeFAAjZSQABRwwZl5acJA+OHEKOBks5gAuOB5MpBAAbQBdAG58uCJ6HygKkFKBQMrS8qyhODFEYhgAOmR0GAA5CB8G8kkKCiRWSFhrDyCvWISkDKzBMFSSsvjElIg0-cq+AbzyAqIYPUZ6HWVlecXyCjRHZngAGFcJB6Eh6PARHBhIMcvdHkhnvpoY0CrwzntxDI1KU3iA4hE6nB6CgQEhSgCoP4AOZwSQ5ahwMAoZhyFxwangiLANBwYxEVEFQqVZgicRVQVC3KSqXY0oARnYMqlJLJpXwxg+EHwSoesoKkl1+pq331zVa7TFXX6sO4amACgk3RAo3Uki4lHtjtNQsofEaQi+dSAA)

## Reference

- [haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

- [filter - CSS（层叠样式表） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter)

- [You Shouldn’t Rely on CSS 100vh and Here’s Why! | Medium](https://ilxanlar.medium.com/you-shouldnt-rely-on-css-100vh-and-here-s-why-1b4721e74487)
