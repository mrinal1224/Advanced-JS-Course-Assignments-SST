
---

# 🔥 ASSIGNMENT 1: Context Loss in a Logging System

## 📌 Title

**The Logger That Works… Until It Doesn’t**

---

## 🧩 Problem Statement

You are building a logging utility for a web service.
It works correctly when called directly but **fails when passed around**.

Fix the issue so logging works in all scenarios.

---

## 📝 Problem Description

```js
const logger = {
  service: "AuthService",
  logs: [],
  logError(message) {
    this.logs.push(`[${this.service}] ERROR: ${message}`);
  }
};

function processError(callback) {
  callback("Invalid token");
}

processError(logger.logError);
```

### ❌ Current Behavior

* `this` is incorrect
* Logs are not stored

### ✅ Expected Output

```js
["[AuthService] ERROR: Invalid token"]
```

---

## 💡 Hints

* What decides the value of `this` at runtime?
* How is `callback()` being invoked?
* Can you **control the invocation site**?

---

## 🛠 Solution Approach

* When a method is passed as a value, it **loses its object**
* `this` depends on *how* the function is called
* Ensure the function is invoked **as an object method**

---

## ✅ Final Solution

```js
processError((message) => {
  logger.logError(message);
});
```

---

## 🧪 Test Cases

```js
console.assert(logger.logs.length === 1);
console.assert(
  logger.logs[0] === "[AuthService] ERROR: Invalid token"
);
```

---

# 🔥 ASSIGNMENT 2: Strict Mode Surprise

## 📌 Title

**Why Did This Become Undefined Overnight?**

---

## 🧩 Problem Statement

After moving to modern JavaScript (ES modules), a utility breaks silently.

Fix it without disabling strict mode.

---

## 📝 Problem Description

```js
"use strict";

function getUserRole() {
  return this.role;
}

const user = {
  role: "admin"
};

console.log(getUserRole());
```

### ❌ Current Output

```js
undefined
```

### ✅ Expected Output

```js
"admin"
```

---

## 💡 Hints

* What is `this` inside a plain function in strict mode?
* Should this function depend on global context?

---

## 🛠 Solution Approach

* In strict mode, `this` inside a function is `undefined`
* Functions that depend on `this` should be **object methods**
* Let invocation decide context naturally

---

## ✅ Final Solution

```js
const user = {
  role: "admin",
  getUserRole() {
    return this.role;
  }
};

console.log(user.getUserRole());
```

---

## 🧪 Test Cases

```js
console.assert(user.getUserRole() === "admin");

const anotherUser = {
  role: "editor",
  getUserRole: user.getUserRole
};
console.assert(anotherUser.getUserRole() === "editor");
```

---

# 🔥 ASSIGNMENT 3: `this` in Event Handlers

## 📌 Title

**The Counter That Counts the Wrong Thing**

---

## 🧩 Problem Statement

A UI counter should increment its internal state on every click.

Instead, it increments something else.

---

## 📝 Problem Description

```js
function Counter(button) {
  this.count = 0;

  button.addEventListener("click", function () {
    this.count++;
    console.log(this.count);
  });
}
```

### ❌ Current Behavior

* `this` refers to the button
* Counter logic breaks

---

## 💡 Hints

* What does `this` refer to inside a DOM event handler?
* How can you preserve the surrounding context?

---

## 🛠 Solution Approach

* DOM event handlers assign `this` to the element
* Arrow functions do **not** create a new `this`
* Use lexical `this` from the constructor

---

## ✅ Final Solution

```js
function Counter(button) {
  this.count = 0;

  button.addEventListener("click", () => {
    this.count++;
    console.log(this.count);
  });
}
```

---

## 🧪 Test Cases

```js
// After 3 clicks → logs: 1, 2, 3
```

---

# 🔥 ASSIGNMENT 4: `this` in Async Code

## 📌 Title

**The Service Name That Disappears After a Delay**

---

## 🧩 Problem Statement

An API client logs its service name after an async operation.

The name prints correctly at first… then disappears.

---

## 📝 Problem Description

```js
const apiClient = {
  service: "PaymentAPI",
  fetchData() {
    setTimeout(function () {
      console.log(this.service);
    }, 1000);
  }
};

apiClient.fetchData();
```

### ❌ Current Output

```js
undefined
```

---

## 💡 Hints

* How is the callback function executed?
* Does it still belong to the object?

---

## 🛠 Solution Approach

* Async callbacks are invoked as plain functions
* Plain functions lose object context
* Arrow functions preserve surrounding `this`

---

## ✅ Final Solution

```js
const apiClient = {
  service: "PaymentAPI",
  fetchData() {
    setTimeout(() => {
      console.log(this.service);
    }, 1000);
  }
};
```

---

## 🧪 Test Cases

```js
apiClient.fetchData(); // logs "PaymentAPI"
```

---

# 🔥 ASSIGNMENT 5: Arrow Functions vs Object Methods

## 📌 Title

**The Refactor That Broke Everything**

---

## 🧩 Problem Statement

A developer refactors object methods using arrow functions.

The code looks cleaner — but breaks behavior.

---

## 📝 Problem Description

```js
const user = {
  name: "Mrinal",
  greet: () => {
    console.log(`Hello, ${this.name}`);
  }
};

user.greet();
```

### ❌ Current Output

```js
Hello, undefined
```

---

## 💡 Hints

* Do arrow functions have their own `this`?
* Where does `this` come from here?

---

## 🛠 Solution Approach

* Arrow functions capture `this` from outer scope
* Object methods require dynamic `this`
* Use regular function syntax for methods

---

## ✅ Final Solution

```js
const user = {
  name: "Mrinal",
  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

user.greet();
```

---

## 🧪 Test Cases

```js
user.greet(); // Hello, Mrinal

const anotherUser = { name: "Rahul", greet: user.greet };
anotherUser.greet(); // Hello, Rahul
```

---

Perfect.
Here are **5 MORE rock-solid, real-world, interview-grade assignments**, **strictly without mentioning or implying `call / apply / bind`**.

These continue to **deeply wire the mental model of `this`** through:

* constructor functions
* method extraction
* class syntax
* higher-order functions
* timers & callbacks
* arrow vs regular functions

Think of these as **“this under pressure”** problems.

---

# 🔥 ASSIGNMENT 6: Method Extraction Bug (Very Common Interview Trap)

## 📌 Title

**Why Did My Method Stop Knowing Its Object?**

---

## 🧩 Problem Statement

A user service exposes a method that returns user details.
When the method is assigned to another variable, it breaks.

Fix the issue **without changing how the method works internally**.

---

## 📝 Problem Description

```js
const userService = {
  user: "Mrinal",
  getUser() {
    return this.user;
  }
};

const getUserFn = userService.getUser;

console.log(getUserFn());
```

### ❌ Current Output

```js
undefined
```

### ✅ Expected Output

```js
"Mrinal"
```

---

## 💡 Hints

* What decides `this` at runtime?
* Is `getUserFn()` still a method call?

---

## 🛠 Solution Approach

* Assigning a method to a variable removes its object context
* Ensure the function is invoked **through the object**
* Fix the invocation site, not the function body

---

## ✅ Final Solution

```js
const getUserFn = () => userService.getUser();

console.log(getUserFn());
```

---

## 🧪 Test Cases

```js
console.assert(getUserFn() === "Mrinal");
```

---

# 🔥 ASSIGNMENT 7: `this` Inside Array Callbacks

## 📌 Title

**Why Is My Total Price Always NaN?**

---

## 🧩 Problem Statement

You are calculating the total price of items in a cart.
The logic looks correct, but `this` breaks inside a callback.

---

## 📝 Problem Description

```js
const cart = {
  tax: 0.1,
  prices: [100, 200, 300],
  getTotal() {
    let total = 0;

    this.prices.forEach(function (price) {
      total += price + price * this.tax;
    });

    return total;
  }
};

console.log(cart.getTotal());
```

### ❌ Current Output

```js
NaN
```

### ✅ Expected Output

```js
660
```

---

## 💡 Hints

* What is `this` inside the `forEach` callback?
* Does the callback belong to the object?

---

## 🛠 Solution Approach

* Array callbacks run as plain functions
* Plain functions lose object context
* Arrow functions inherit surrounding `this`

---

## ✅ Final Solution

```js
const cart = {
  tax: 0.1,
  prices: [100, 200, 300],
  getTotal() {
    let total = 0;

    this.prices.forEach((price) => {
      total += price + price * this.tax;
    });

    return total;
  }
};
```

---

## 🧪 Test Cases

```js
console.assert(cart.getTotal() === 660);
```

---

# 🔥 ASSIGNMENT 8: Constructor Function & Event Callback

## 📌 Title

**Why Does My Class Stop Working After a Click?**

---

## 🧩 Problem Statement

A component tracks how many times a button is clicked.
The counter works initially, then breaks.

---

## 📝 Problem Description

```js
function ClickTracker(button) {
  this.clicks = 0;

  button.addEventListener("click", function () {
    this.clicks++;
    console.log(this.clicks);
  });
}
```

### ❌ Current Behavior

* `this` refers to the button
* `clicks` is undefined

---

## 💡 Hints

* What does `this` refer to in DOM event handlers?
* How can we keep constructor context?

---

## 🛠 Solution Approach

* Event handlers assign `this` to the DOM element
* Arrow functions do not rebind `this`
* Preserve constructor context using lexical `this`

---

## ✅ Final Solution

```js
function ClickTracker(button) {
  this.clicks = 0;

  button.addEventListener("click", () => {
    this.clicks++;
    console.log(this.clicks);
  });
}
```

---

## 🧪 Test Cases

```js
// After 2 clicks → logs: 1, 2
```

---



# 🔥 ASSIGNMENT 10: Arrow Function Misuse in Classes

## 📌 Title

**The Arrow Function That Looked Right but Wasn’t**

---

## 🧩 Problem Statement

A developer converts a class method to an arrow function, expecting cleaner code.
Unexpected behavior occurs when creating multiple instances.

---

## 📝 Problem Description

```js
class Counter {
  count = 0;

  increment = () => {
    this.count++;
  };
}

const c1 = new Counter();
const c2 = new Counter();

c1.increment();
c1.increment();

console.log(c2.count);
```

### ❓ Question

What is the output?
Is this design correct?

---

## 💡 Hints

* Arrow functions are created per instance
* Think about memory and behavior

---

## 🛠 Solution Approach

* Arrow functions capture `this` lexically
* They are recreated for every instance
* Use regular methods unless lexical `this` is required

---

## ✅ Final Solution (Preferred Design)

```js
class Counter {
  constructor() {
    this.count = 0;
  }

  increment() {
    this.count++;
  }
}
```

---

## 🧪 Test Cases

```js
const c1 = new Counter();
const c2 = new Counter();

c1.increment();
c1.increment();

console.assert(c1.count === 2);
console.assert(c2.count === 0);
```

---

