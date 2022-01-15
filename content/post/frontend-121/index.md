---
title: '毎日のフロントエンド  121'
date: 2022-01-15T17:22:59+09:00
description: frontend 每日一练
image: frontend-121-cover.jpg
categories:
  - Javascript
---

# 第一百二十一日

## Clean Code JavaScript

### Functions

#### 1. Function arguments (2 or fewer ideally)

Limiting the amount of function parameters is incredibly important because it makes testing your function explosion where you have to teset tons of different cases with each separeate argument.

减少函数的参数数量，有利于测试函数。

**One or two arguments is the ideal case**, and three should be avoided if possible. Anything more than that should be consolidated(被合并). Usually, if you have more than two arguments then your function is trying to do too much. In cases where it's not, most of the time a higher-level object will suffice as an argument.

如果参数太多且无法缩减，应考虑用对象代替。

Since JavaScript allows you to make objects on the fly, without a lot of class boilerplate, you can use an object if you are finding youself needing a lot of arguments.

To make it obvious what properties the function expects, we can use the destructuring syntax(from EX2015/ES6). This has a few advantages:

1. When someone looks at the function signature, it's immediately clear what properties are being used.
2. It can be used to simulate named parameters.
3. Destructuring also **clones the specified primitive values of the argument object** passed into the function. **This can help prevent side effects.**
   > Note: objects and arrays that are destructured from the argument object are NOT cloned.
4. Linters can warn you about unused properties, which would be impossible without destructuring.

**Bad:**

```js
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Good:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true,
});
```

#### 2. Functions should do one thing

**This is by far the most important rule in software engineering**. When functions do more than one thing, they are harder to compose, test, and reason about. When you can isolate a function to just one action, it can be refactored easily and your code will read much cleaner.

**Bad:**

```javascript
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

#### 3. Function names should say what they do

**Bad:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Good:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

#### 4. Functions should only be one level of abstraction

When you have more than one level of abstraction your function is usually doing too much. Splitting up functions leads to reusability and easier testing.

**Bad:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Good:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach((token) => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

#### 5. Remove duplicate code

Do your absolute best to avoid duplicate code. Duplicate code is bad because it means that there's more than one place to alter something if you need to change some logic.

Imagine if you run a restaurant and you keep track of your inventory: all your tomatoes, onions, garlic, spices, etc. If you have multiple lists that you keep this on, then all have to be updated when you serve a dish with tomatoes in them. **If you only have one list, there's only one place to update!**

Oftentimes you have duplicate code because you have two or more slightly different things, that share a lot in common, but their differences force you to have two or more separate functions that do much of the same things. **Removing duplicate code means creating an abstraction that can handle this set of different things with just one function/module/class.**

Getting the abstraction right is critical, that's why you should follow the SOLID principles laid out in the _Classes_ section. Bad abstractions can be worse than duplicate code, so be careful! Having said this, if you can make a good abstraction, do it! **Don't repeat yourself**, otherwise you'll find yourself updating multiple places anytime you want to change one thing.

**Bad:**

```javascript
function showDeveloperList(developers) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink,
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio,
    };

    render(data);
  });
}
```

**Good:**

```javascript
function showEmployeeList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience,
    };

    switch (employee.type) {
      case 'manager':
        data.portfolio = employee.getMBAProjects();
        break;
      case 'developer':
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

#### 6. Set default objects with `Object.assign`

**Bad:**

```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true,
};

function createMenu(config) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Good:**

```javascript
const menuConfig = {
  title: 'Order',
  // User did not include 'body' key
  buttonText: 'Send',
  cancellable: true,
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: 'Foo',
      body: 'Bar',
      buttonText: 'Baz',
      cancellable: true,
    },
    config
  );
  return finalConfig;
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

#### 7. Don't use flags as function parameters

Flags tell your user that this function does more than one thing. Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.

**Bad:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

#### 8. Avoid Side Effects (part 1)

A function produces a side effect if it does anything other than take a value in and return another value or values. A side effect could be writing to a file, modifying some global variable, or accidentally wiring all your money to a stranger.

Now, you do need to have side effects in a program on occasion. Like the previous example, you might need to write to a file. What you want to do is to centralize where you are doing this. Don't have several functions and classes that write to a particular file. Have one service that does it. One and only one.

The main point is to avoid common pitfalls like sharing state between objects without any structure, using mutable data types that can be written to by anything, and not centralizing where your side effects occur. If you can do this, you will be happier than the vast majority of other programmers.

**Bad:**

```js
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Good:**

```js
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

#### 9. Avoid Side Effects (part 2)

In JavaScript, some values are unchangeable (immutable) and some are changeable (mutable). Objects and arrays are two kinds of mutable values so it's important to handle them carefully when they're passed as parameters to a function. A JavaScript function can change an object's properties or alter the contents of an array which could easily cause bugs elsewhere.

Suppose there's a function that accepts an array parameter representing a shopping cart. If the function makes a change in that shopping cart array - by adding an item to purchase, for example - then any other function that uses that same cart array will be affected by this addition. That may be great, however it could also be bad. Let's imagine a bad situation:

The user clicks the "Purchase" button which calls a purchase function that spawns a network request and sends the cart array to the server. Because of a bad network connection, the purchase function has to keep retrying the request. Now, what if in the meantime the user accidentally clicks an "Add to Cart" button on an item they don't actually want before the network request begins? If that happens and the network request begins, then that purchase function will send the accidentally added item because the cart array was modified.

A great solution would be for the addItemToCart function to always clone the cart, edit it, and return the clone. This would ensure that functions that are still using the old shopping cart wouldn't be affected by the changes.

Two caveats to mention to this approach:

1. There might be cases where you actually want to modify the input object, but when you adopt this programming practice you will find that those cases are pretty rare. Most things can be refactored to have no side effects!

2. Cloning big objects can be very expensive in terms of performance. Luckily, this isn't a big issue in practice because there are great libraries that allow this kind of programming approach to be fast and not as memory intensive as it would be for you to manually clone objects and arrays.

**Bad:**

```js
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Good:**

```js
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

#### 10. Don't write to global functions

Polluting globals is a bad practice in JavaScript because you could clash with another library and the user of your API would be none-the-wiser until they get an exception in production. Let's think about an example: what if you wanted to extend JavaScript's native Array method to have a diff method that could show the difference between two arrays? You could write your new function to the Array.prototype, but it could clash with another library that tried to do the same thing. What if that other library was just using diff to find the difference between the first and last elements of an array? This is why it would be much better to just use ES2015/ES6 classes and simply extend the Array global.

**Bad:**

```js
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter((elem) => !hash.has(elem));
};
```

**Good:**

```js
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter((elem) => !hash.has(elem));
  }
}
```

#### 11. Favor functional programming over imperative programming

JavaScript isn't a functional language in the way that Haskell is, but it has a functional flavor to it. Functional languages can be cleaner and easier to test. Favor this style of programming when you can.

**Bad:**

```js
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500,
  },
  {
    name: 'Suzie Q',
    linesOfCode: 1500,
  },
  {
    name: 'Jimmy Gosling',
    linesOfCode: 150,
  },
  {
    name: 'Gracie Hopper',
    linesOfCode: 1000,
  },
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Good:**

```js
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500,
  },
  {
    name: 'Suzie Q',
    linesOfCode: 1500,
  },
  {
    name: 'Jimmy Gosling',
    linesOfCode: 150,
  },
  {
    name: 'Gracie Hopper',
    linesOfCode: 1000,
  },
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

#### 12. Encapsulate conditionals

**Bad:**

```js
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Good:**

```js
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

#### 13. Avoid negative conditionals

**Bad:**

```js
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Good:**

```js
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

#### 14. Avoid conditionals

This seems like an impossible task. Upon first hearing this, most people say, "how am I supposed to do anything without an if statement?" The answer is that you can use polymorphism to achieve the same task in many cases. The second question is usually, "well that's great but why would I want to do that?" The answer is a previous clean code concept we learned: a function should only do one thing. When you have classes and functions that have if statements, you are telling your user that your function does more than one thing. Remember, just do one thing.

**Bad:**

```js
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Good:**

```js
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

#### 15. Avoid type-checking (part 1)

JavaScript is untyped, which means your functions can take any type of argument. Sometimes you are bitten by this freedom and it becomes tempting to do type-checking in your functions. There are many ways to avoid having to do this. The first thing to consider is consistent APIs.

**Bad:**

```js
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**Good:**

```js
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```

#### 16. Avoid type-checking (part 2)

If you are working with basic primitive values like strings and integers, and you can't use polymorphism but you still feel the need to type-check, you should consider using TypeScript. It is an excellent alternative to normal JavaScript, as it provides you with static typing on top of standard JavaScript syntax. The problem with manually type-checking normal JavaScript is that doing it well requires so much extra verbiage that the faux "type-safety" you get doesn't make up for the lost readability. Keep your JavaScript clean, write good tests, and have good code reviews. Otherwise, do all of that but with TypeScript (which, like I said, is a great alternative!).

**Bad:**

```js
function combine(val1, val2) {
  if (
    (typeof val1 === 'number' && typeof val2 === 'number') ||
    (typeof val1 === 'string' && typeof val2 === 'string')
  ) {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**Good:**

```js
function combine(val1, val2) {
  return val1 + val2;
}
```

#### 17. Don't over-optimize

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of times, if you are optimizing then you are just wasting your time. There are good resources for seeing where optimization is lacking. Target those in the meantime, until they are fixed if they can be.

**Bad:**

```js
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good:**

```js
for (let i = 0; i < list.length; i++) {
  // ...
}
```

#### 18. Remove dead code

Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase. If it's not being called, get rid of it! It will still be safe in your version history if you still need it.

**Bad:**

```js
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**Good:**

```js
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

## Reference

[haizlin/fe-interview](https://github.com/haizlin/fe-interview)

[前端进阶](https://muyiy.cn/)

[ryanmcdermott/clean-code-javascript: Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript#introduction)

[Optimization killers · petkaantonov/bluebird Wiki](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
