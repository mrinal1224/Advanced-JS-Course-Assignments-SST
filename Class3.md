**Class3**
---

# 🔥 ASSIGNMENT 1: Method Lookup & Prototype Chain (Core Mental Model)

## 📌 Title

**Where Is This Method Actually Coming From?**

---

## 🧩 Problem Statement

You are debugging a production issue where a method works, but **no one knows where it’s defined**.

Your task is to **trace method lookup through the prototype chain**.

---

## 📝 Problem Description

```js
function User(name) {
  this.name = name;
}

User.prototype.sayHello = function () {
  return `Hello, ${this.name}`;
};

const user = new User("Mrinal");

console.log(user.sayHello());
```

### ❓ Questions

1. Is `sayHello` an own property of `user`?
2. How does JavaScript find `sayHello`?
3. Where does the lookup stop?

---

## 💡 Hints

* Check the object itself first
* Then move to its prototype
* Continue until `null`

---

## 🛠 Solution Approach

* JS performs **property lookup**
* First: object itself
* Then: `__proto__` chain
* Stops at `null`

---

## ✅ Final Answer

```js
user.hasOwnProperty("sayHello"); // false
"user" in user;                 // true
```

Explanation:

* `sayHello` exists on `User.prototype`
* `user.__proto__ === User.prototype`
* Lookup stops once found

---

## 🧪 Test Cases

```js
console.assert(!user.hasOwnProperty("sayHello"));
console.assert("sayHello" in user);
```

---

# 🔥 ASSIGNMENT 2: `prototype` vs `__proto__` (Interview Favorite)

## 📌 Title

**These Look Similar — But Are Not**

---

## 🧩 Problem Statement

A junior dev uses `prototype` and `__proto__` interchangeably.

Explain the difference **using code**, not theory.

---

## 📝 Problem Description

```js
function Car(model) {
  this.model = model;
}

const car = new Car("Tesla");
```

Answer the following:

1. What is `Car.prototype`?
2. What is `car.__proto__`?
3. Are they the same?

---

## 💡 Hints

* One exists on functions
* One exists on objects
* They point to the same object in constructor usage

---

## 🛠 Solution Approach

* `prototype` is used during object creation
* `__proto__` is the internal link
* They reference the same object

---

## ✅ Final Answer

```js
Car.prototype === car.__proto__; // true
```

Explanation:

* `prototype` → blueprint
* `__proto__` → actual internal link

---

## 🧪 Test Cases

```js
console.assert(Car.prototype === car.__proto__);
console.assert(car.__proto__.__proto__ === Object.prototype);
```

---

# 🔥 ASSIGNMENT 3: Prototype Inheritance (Real-World OOPS)

## 📌 Title

**Sharing Behavior Without Duplicating Code**

---

## 🧩 Problem Statement

You are building a role-based system.

Admins and Users should share common behavior, but Admins should have extra capabilities.

Implement this using **prototype chaining**.

---

## 📝 Problem Description

```js
function User(name) {
  this.name = name;
}

User.prototype.login = function () {
  return `${this.name} logged in`;
};

function Admin(name) {
  this.name = name;
}
```

### ❌ Missing

* Admin should inherit `login`
* Admin should have `deleteUser`

---

## 💡 Hints

* Objects can link to other objects
* Prototype chaining enables inheritance

---

## 🛠 Solution Approach

* Link `Admin.prototype` to `User.prototype`
* Add admin-specific methods

---

## ✅ Final Solution

```js
Admin.prototype = Object.create(User.prototype);

Admin.prototype.deleteUser = function () {
  return `${this.name} deleted a user`;
};
```

---

## 🧪 Test Cases

```js
const admin = new Admin("Mrinal");

console.assert(admin.login() === "Mrinal logged in");
console.assert(admin.deleteUser() === "Mrinal deleted a user");
console.assert(admin instanceof User);
```

---

# 🔥 ASSIGNMENT 4: Property Shadowing (Subtle Production Bug)

## 📌 Title

**Why Did My Property Suddenly Change?**

---

## 🧩 Problem Statement

A property exists on the prototype.
An instance updates the same property — behavior changes unexpectedly.

Understand **property shadowing**.

---

## 📝 Problem Description

```js
function Account() {}

Account.prototype.balance = 100;

const acc1 = new Account();
const acc2 = new Account();

acc1.balance += 50;

console.log(acc1.balance);
console.log(acc2.balance);
```

### ❓ Output?

Why?

---

## 💡 Hints

* Reading vs writing behaves differently
* Writing creates an own property

---

## 🛠 Solution Approach

* Read → prototype
* Write → instance
* Prototype property gets shadowed

---

## ✅ Final Answer

```js
150
100
```

Explanation:

* `acc1.balance` becomes an own property
* `acc2` still reads from prototype

---

## 🧪 Test Cases

```js
console.assert(acc1.hasOwnProperty("balance"));
console.assert(!acc2.hasOwnProperty("balance"));
```

---

# 🔥 ASSIGNMENT 5: Object Inspection & Debugging (Interview Gold)

## 📌 Title

**Inspecting Objects Like a Senior Engineer**

---

## 🧩 Problem Statement

You’re debugging an object in production.

You must determine:

* where properties come from
* what an object inherits from
* whether a property truly belongs to the object

---

## 📝 Problem Description

```js
function Device(type) {
  this.type = type;
}

Device.prototype.powerOn = function () {};

const d = new Device("Laptop");
```

Answer:

1. Is `type` an own property?
2. Is `powerOn` an own property?
3. Is `powerOn` accessible?
4. Is `d` an instance of `Device`?
5. Is `d` an instance of `Object`?

---

## 💡 Hints

* Use inspection operators
* Think like the JS engine

---

## 🛠 Solution Approach

* `hasOwnProperty` → ownership
* `in` → accessibility
* `instanceof` → prototype chain

---

## ✅ Final Answer

```js
d.hasOwnProperty("type");       // true
d.hasOwnProperty("powerOn");   // false
"powerOn" in d;                // true
d instanceof Device;           // true
d instanceof Object;           // true
```

---

## 🧪 Test Cases

```js
console.assert(d.hasOwnProperty("type"));
console.assert(!d.hasOwnProperty("powerOn"));
console.assert("powerOn" in d);
console.assert(d instanceof Device);
console.assert(d instanceof Object);
```

---

