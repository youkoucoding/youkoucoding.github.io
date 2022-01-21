---
title: '毎日のフロントエンド  127'
date: 2022-01-21T10:54:55+09:00
description: frontend 每日一练
image: frontend-127-cover.jpg
categories:
  - HTML
  - CSS
  - Javascript
---

# 第一百二十七日

## HTML

### **Question:** `<button>`中的`reset`

`button.type="reset"` ，重置按钮，即清除表单数据。与 form 表单配合使用。demo 如下：

```html
<form action="form_action.asp" method="get">
  First name: <input type="text" name="fname" /> Last name:
  <input type="text" name="lname" />
  <button type="submit" value="Submit">Submit</button>
  <button type="reset" value="Reset">Reset</button>
</form>
```

## CSS

### **Question:** `min-width`和`max-width`的理解

多用于自适应宽度布局中，希望某些元素的宽度不能小于或大于某个宽度

- `min-width`：设定元素的最小宽度，可以阻止元素的宽度小于该值；
- `max-width`：设定元素的最大宽度，可以阻止元素的宽度大于该值；

## Clean Code JavaScript

### Error Handling

#### 1. Don't ignore caught errors

Doing nothing with a caught error doesn't give you the ability to ever fix or react to said error. Logging the error to the console isn't much better as often times it can get lost in a sea of things printed to the console. If you wrap any bit of code in a try/catch it means you think an error may occur there and therefore you should have a plan, or create a code path, for when it occurs.

**Bad:**

```js
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Good:**

```js
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

#### 2. Don't ignore rejected promises

For the same reason you shouldn't ignore caught errors from `try/catch`.

**Bad:**

```js
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    console.log(error);
  });
```

**Good:**

```js
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview/blob/master/category/history.md)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)
