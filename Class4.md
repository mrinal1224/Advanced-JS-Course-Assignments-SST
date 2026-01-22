

---

## 🔥 ASSIGNMENT 1: Classes Are Just Prototypes (Reality Check)

### 📌 Title

**Is This Really Object-Oriented? Prove It.**

---

### 🧩 Problem Statement

A teammate claims that ES6 classes are a “new OOP system” in JavaScript.

Your task is to **prove using code** that classes are just syntax over prototypes.

---

### 📝 Problem Description

```js
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hello, ${this.name}`;
  }
}

const user = new User("Mrinal");
```

Answer the following:

1. Is `greet` an own property of `user`?
2. Where is `greet` actually stored?
3. Does `User.prototype` still exist?

---

### 💡 Hints

* Inspect the object, not the syntax
* Classes do not change how JS works internally

---

### 🛠 Solution Approach

* Methods defined in classes live on the prototype
* Instances only store data, not methods
* Prototype chain still powers everything

---

### ✅ Final Answer

```js
user.hasOwnProperty("greet");          // false
User.prototype.hasOwnProperty("greet"); // true
user.__proto__ === User.prototype;     // true
```

---

### 🧪 Test Cases

```js
console.assert(!user.hasOwnProperty("greet"));
console.assert(User.prototype.hasOwnProperty("greet"));
console.assert(user.__proto__ === User.prototype);
```

---

## 🔥 ASSIGNMENT 2: Inheritance with `extends` & `super` (Real System Design)

### 📌 Title

**Building a Role-Based System**

---

### 🧩 Problem Statement

You are building a system with users and admins.

Admins should:

* inherit user behavior
* add extra capabilities
* reuse parent constructor logic

---

### 📝 Problem Description

```js
class User {
  constructor(name) {
    this.name = name;
  }

  login() {
    return `${this.name} logged in`;
  }
}

class Admin {
  // incomplete
}
```

Requirements:

* `Admin` must inherit from `User`
* `Admin` must call the parent constructor
* `Admin` must add `deleteUser()`

---

### 💡 Hints

* `extends` sets up the prototype chain
* `super()` is mandatory in derived constructors

---

### 🛠 Solution Approach

* Use `extends` for inheritance
* Use `super()` to initialize parent state
* Add admin-only behavior

---

### ✅ Final Solution

```js
class Admin extends User {
  constructor(name) {
    super(name);
  }

  deleteUser() {
    return `${this.name} deleted a user`;
  }
}
```

---

### 🧪 Test Cases

```js
const admin = new Admin("Mrinal");

console.assert(admin.login() === "Mrinal logged in");
console.assert(admin.deleteUser() === "Mrinal deleted a user");
console.assert(admin instanceof Admin);
console.assert(admin instanceof User);
```

---

## 🔥 ASSIGNMENT 3: Method Overriding & Polymorphism (Interview Gold)

### 📌 Title

**When Child Behavior Replaces Parent Behavior**

---

### 🧩 Problem Statement

A child class needs to **change** how a method behaves, while still keeping the same method name.

This is classic polymorphism.

---

### 📝 Problem Description

```js
class Payment {
  process(amount) {
    return `Processing payment of ₹${amount}`;
  }
}

class UpiPayment extends Payment {
  // override process
}
```

Expected:

```txt
Processing UPI payment of ₹500
```

---

### 💡 Hints

* Child class methods override parent methods
* Method lookup starts from the instance

---

### 🛠 Solution Approach

* Define a method with the same name in the child
* JavaScript will pick the closest method in the chain

---

### ✅ Final Solution

```js
class UpiPayment extends Payment {
  process(amount) {
    return `Processing UPI payment of ₹${amount}`;
  }
}
```

---

### 🧪 Test Cases

```js
const payment = new UpiPayment();

console.assert(payment.process(500) === "Processing UPI payment of ₹500");
console.assert(payment instanceof Payment);
```

---

## 🔥 ASSIGNMENT 4: Static Methods (Common Misuse Bug)

### 📌 Title

**Why Can’t I Call This Method on the Object?**

---

### 🧩 Problem Statement

A utility method is defined inside a class but behaves unexpectedly.

Understand **static vs instance methods**.

---

### 📝 Problem Description

```js
class MathUtils {
  static add(a, b) {
    return a + b;
  }
}

const math = new MathUtils();
```

Questions:

1. Can `math.add(2, 3)` work?
2. Why or why not?
3. What is the correct usage?

---

### 💡 Hints

* Static methods belong to the class, not instances
* Think “utility”, not “object behavior”

---

### 🛠 Solution Approach

* Static methods live on the constructor
* Instances do not inherit static methods

---

### ✅ Final Answer

```js
MathUtils.add(2, 3); // 5
math.add;           // undefined
```

---

### 🧪 Test Cases

```js
console.assert(MathUtils.add(2, 3) === 5);
console.assert(math.add === undefined);
```

---

## 🔥 ASSIGNMENT 5: Private Fields & OOPS Reality (Modern JS)

### 📌 Title

**True Encapsulation (Finally?)**

---

### 🧩 Problem Statement

You need to protect sensitive data so it cannot be accessed or modified directly.

Use **private class fields** to enforce encapsulation.

---

### 📝 Problem Description

```js
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const acc = new BankAccount();
```

Questions:

1. Can `acc.#balance` be accessed?
2. How is this different from `_balance`?

---

### 💡 Hints

* `#` fields are enforced by the language
* They are not just conventions

---

### 🛠 Solution Approach

* Private fields are inaccessible outside the class
* This is *real* encapsulation, not naming convention

---

### ✅ Final Answer

```js
acc.deposit(1000);
acc.getBalance();   // 1000
acc.#balance;       // SyntaxError
```

---

### 🧪 Test Cases

```js
acc.deposit(500);
console.assert(acc.getBalance() === 1500);
```

---


