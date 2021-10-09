---
title: 'Currying in JavaScript (カリー化 & 柯里化)'
date: 2021-10-09T15:24:26+09:00
image: currying-cover.png
categories:
  - Javascript
---

# Currying in JavaSctipt

Functional programming is a style of programming that attempts to pass functions as arguments(callbacks) and return functions without side-effects(changes to the program's state).

So many languages adopted this programming style. `Javascript`, `Haskell`, `Erlang`, `Clojure`, and `Scala` are the most popular among them.

And with its ability to pass the return functions, it brought so many conscepts: **Pure Functions**, **Currying**, **Higher-Order functions**.

## What is Currying?

Currying is a **process** in functionnal programming in which we can transform a function with multiple arguments into a sequence of nesting functions. It returns a new functon that expects the next argument inline.

Currying doesn't call a function. It just transforms it.

只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

It keeps returning a new function (that expects the current argument) until all the arguments are exhausted. The arguments are kept `alive`(via `closure`) and all are used in execution when the final function tin the currying chain is returnd and executed.

```
Currying is the process of turning a function with mutiple arity(the number of arguments of operator taken by a function) into a function with less arity.
```

```js
function fn(a, b) {} // 2-arity function
```

**SO, currying transforms a function with multiple arguments into a sequence/series of functions each taking _single argument_.**

Currying is a transformation of functions that translates a function from callbale as `f(a, b, c)` into callable as `f(a)(b)(c)`.

Example:

We'll create a helper function `curry(fn)` that performs currying for a two-argument `fn`. In other words, `curry(fn)` for two-argument `fn(a, b)` translates it into a function that runs as `fn(a)(b)`:

```js
// curry(fn) does the currying transform
function curry(fn) {
  return function (a) {
    return function (b) {
      return fn(a, b);
    };
  };
}

//usage
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

console.log(curriedSum(1)(2)); // 3
```

As you see the implementation is straightforward: **Just two wrapper**

- The result of `curry(fn)` is a wrapper `function(a)`

- When it is called like `curriedSum(1)`, the argument is saved in the Lexical Environment[^1], and a new wrapper is returned `function(b)`

[^1]: [Lexical Environment — The hidden part to understand Closures | by Amandeep Singh | Medium](https://amnsingh.medium.com/lexical-environment-the-hidden-part-to-understand-closures-71d60efac0e0)

- Then this wrapper is called with `2` as an argument, and it passes the call to the original `sum`.

#### `_lodash`

More advanced implementations of currying, such as `_.curry` from **lodash** library, return a wrapper that allows a function to be called both normally and partially:

```js
function sum(a, b) {
  return a + b;
}

// using _.curry from lodash library
let curriedSum = _.curry(sum);

console.log(curriedSum(1, 2)); // 3, still callable normally
console.log(curriedSum(1)(2)); // 3, called partially
```

### What for?

For instance, we have the logging function `log(date, importance, message)` that formats and outputs the information. In real projects such functions have many useful features like sending logs over the network, here we'll just use `alert`:

```js
function log(date, importance, message) {
  alert(`[${date.getHours()}:${date.getMinutes()}][${importance}]${message}`);
}
```

---

```js
// curry it
log = _.curry(log);
```

---

```js
// how it works

log(new Date(), 'DEBUG', 'some bugs'); // log(a, b, c)

log(new Date())('DEBUG')('some bugs'); //log(a)(b)(c)
```

---

```js
// Usage
// logNow will be the partial of log with fixed first argument
let logNow = log(new Date());

// use it
logNow('INFO', 'message'); // [HH:mm] INFO message
```

Now `logNow` is `log` with first argument, in other words "partially applied function" or "partial" for short.

We can go further and make a convenience function for current debug logs:

```js
let debugNow = logNow('DEBUG');

debugNow('message'); // [HH:mm] DEBUG message
```

1. We didn't lose anyhing after currying: `log` is still callable normally
2. We can easily generate partial functions such as for today log.

### Advanced curry implementation

```js
function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length) {
      return func.apply(this, args);
    } else {
      return function (...args2) {
        return curried.apply(this, args.concat(args2));
      };
    }
  };
}

// usage

function sum(a, b, c) {
  return a + b + c;
}

let curriedSum = curry(sum);

alert(curriedSum(1, 2, 3)); // 6, still callable normally
alert(curriedSum(1)(2, 3)); // 6, currying of 1st arg
alert(curriedSum(1)(2)(3)); // 6, full currying
```

> `Function.length`: The `length` property indicates the number of parameters expected by the function.

1. If passed `args` count is the same or more than the orignal function has in its definition (`func.length`), then just pass the call to it using `func.apply`
2. Otherwise, get a partial: we don't call `func` just yet. Instead, another wrapper is returned, that will re-apply `curried` providing previous arguments together with the new ones.

Thenm if we call it, again we'll get either a new partial (if not enough arguments) or, finally, the result.

## When

Currying comes in handy when you want to:

1. Write little code modules that can be reused and configured with ease, much like what we do with npm.

2. Avoid frequently calling a function with the same argument.

## Summary

_Currying_ is a transform that makes `fn(a,b,c)` callbale as `fn(a)(b)(c)`. JavaScript implementations usually both keep the function callable normally and return the partial if the arguments count is not enough.

_Currying_ allows us to easily get partials. As we've seen in the logging example, after currying the three argement unniversal function `log(date, importance, message)` gives us partials when called with one argument or two arguments(`log(date, importance)`).

## Reference

[Currying](https://javascript.info/currying-partials)

[カリー化](https://kenju.gitbooks.io/js_step-up-to-intermediate/content/content/part03/currying.html)

[柯里化（curry） · 函数式编程指北](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch4.html#%E4%B8%8D%E4%BB%85%E4%BB%85%E6%98%AF%E5%8F%8C%E5%85%B3%E8%AF%AD%E5%92%96%E5%96%B1)
