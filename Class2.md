**Class-2**
* constructor functions
* `new` keyword mechanics
* how object creation works internally
* constructor return rules
* constructor vs factory patterns
* real-world + interview framing


---

# 🔥 ASSIGNMENT 1: Rebuilding `new` (Mental Model Builder)

## 📌 Title

**What Does `new` Really Do Behind the Scenes?**

---

## 🧩 Problem Statement

Many developers use `new` daily but cannot explain what it does.

Your task is to **manually recreate the behavior of `new`** using plain JavaScript — without using the `new` keyword.

---

## 📝 Problem Description

You are given a constructor:

```js
function User(name, role) {
  this.name = name;
  this.role = role;
}
```

Write a function `manualNew(Constructor, args)` that creates an object exactly like:

```js
const user = new User("Mrinal", "admin");
```

---

## 💡 Hints

* `new` creates an empty object
* That object becomes `this` inside the constructor
* The constructor initializes properties
* The object is returned automatically

---

## 🛠 Solution Approach

1. Create a plain object
2. Set its internal reference to the constructor’s prototype
3. Execute the constructor with that object as `this`
4. Return the object unless the constructor explicitly returns another object

---

## ✅ Final Solution

```js
function manualNew(Constructor, args) {
  const obj = {};
  obj.__proto__ = Constructor.prototype;

  Constructor.apply(obj, args); // internal execution step
  return obj;
}
```

> ⚠️ Note for students: this mimics behavior conceptually, not as an API recommendation.

---

## 🧪 Test Cases

```js
const u = manualNew(User, ["Mrinal", "admin"]);

console.assert(u.name === "Mrinal");
console.assert(u.role === "admin");
console.assert(u instanceof User);
```

---

# 🔥 ASSIGNMENT 2: Constructor Return Rules (Classic Interview Question)

## 📌 Title

**Why Did My Constructor Ignore `this`?**

---

## 🧩 Problem Statement

A constructor is modified to return a value.

The behavior of `new` changes in a non-obvious way.

---

## 📝 Problem Description

```js
function Device(name) {
  this.name = name;
  return { name: "Unknown Device" };
}

const d = new Device("Phone");
console.log(d.name);
```

### ❓ Question

What is logged, and why?

---

## 💡 Hints

* Does `new` always return `this`?
* What happens when a constructor returns an object?

---

## 🛠 Solution Approach

* If a constructor returns an **object**, it replaces the newly created one
* If it returns a **primitive**, the return value is ignored
* This rule is tested frequently in interviews

---

## ✅ Final Solution

```js
"Unknown Device"
```

Explanation:

* The explicitly returned object replaces the default instance

---

## 🧪 Test Cases

```js
console.assert(d.name === "Unknown Device");
```

---

# 🔥 ASSIGNMENT 3: Forgetting `new` (Real Production Bug)

## 📌 Title

**The Missing `new` That Broke Everything**

---

## 🧩 Problem Statement

A teammate forgets to use `new` when calling a constructor.

This causes silent bugs and unexpected side effects.

Refactor the code so it **works correctly even without `new`**.

---

## 📝 Problem Description

```js
function Order(id, amount) {
  this.id = id;
  this.amount = amount;
}

const order = Order(101, 5000);
console.log(order);
```

### ❌ Problems

* `order` is `undefined`
* In non-strict mode, data leaks to global scope

---

## 💡 Hints

* What does `this` refer to without `new`?
* Can the function return something explicitly?

---

## 🛠 Solution Approach

* Constructor functions rely on `new`
* Factory functions are safer and explicit
* Convert this into a **factory-style function**

---

## ✅ Final Solution

```js
function Order(id, amount) {
  return {
    id,
    amount
  };
}

const order = Order(101, 5000);
```

---

## 🧪 Test Cases

```js
console.assert(order.id === 101);
console.assert(order.amount === 5000);
```

---

# 🔥 ASSIGNMENT 4: Constructor vs Factory (Design Decision)

## 📌 Title

**Choosing the Right Object Creation Pattern**

---

## 🧩 Problem Statement

You are designing a notification system.

Each notification has:

* `message`
* `type`
* `timestamp`

The system should be:

* safe to use
* easy to read
* hard to misuse

Choose the **best creation pattern** and implement it.

---

## 💡 Hints

* Constructors require `new`
* Factories return objects explicitly
* Simpler systems favor explicit behavior

---

## 🛠 Solution Approach

* No inheritance required
* No shared state needed
* Factory functions are safer and clearer

---

## ✅ Final Solution

```js
function createNotification(message, type) {
  return {
    message,
    type,
    timestamp: Date.now()
  };
}
```

---

## 🧪 Test Cases

```js
const n1 = createNotification("Server down", "error");
const n2 = createNotification("Login successful", "info");

console.assert(n1 !== n2);
console.assert(typeof n1.timestamp === "number");
```

---

# 🔥 ASSIGNMENT 5: Debugging Shared State (Interview Debug Round)

## 📌 Title

**Why Are My Objects Sharing Data?**

---

## 🧩 Problem Statement

A constructor is supposed to create independent objects.

Instead, updating one instance affects others.

Identify the bug and fix it.

---

## 📝 Problem Description

```js
function User(name) {
  this.name = name;
}

User.prototype.skills = [];

const u1 = new User("Mrinal");
const u2 = new User("Rahul");

u1.skills.push("JS");

console.log(u2.skills);
```

### ❌ Current Output

```js
["JS"]
```

### ✅ Expected Output

```js
[]
```

---

## 💡 Hints

* Where is `skills` stored?
* Is it per object or shared?

---

## 🛠 Solution Approach

* Properties on the prototype are shared
* Instance-specific data must live inside the constructor

---

## ✅ Final Solution

```js
function User(name) {
  this.name = name;
  this.skills = [];
}
```

---

## 🧪 Test Cases

```js
u1.skills.push("JS");

console.assert(u1.skills.length === 1);
console.assert(u2.skills.length === 0);
```

---

