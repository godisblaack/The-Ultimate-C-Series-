# 151 Introduction

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/xeocaBidVA4?si=NvcEuQkQkUURoINJ"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Introduction to Inheritance & Polymorphism

### 🧠 Core Principles

* **Inheritance** and **Polymorphism** are **essential** in Object-Oriented Programming.
* They allow code **reuse**, **extension**, and **flexibility**.
* These topics are often **misunderstood**, but mastering them is key to writing clean, extensible C++.

---

## 2. Concepts to Cover

### 🔑 Protected Members

* Members declared as `protected` are:

  * Accessible within the class itself.
  * Accessible within derived (child) classes.
  * Not accessible from outside classes.

### 🎭 Virtual Methods

* Declared with `virtual` keyword.
* Allow **dynamic binding** at runtime.
* Enable polymorphism (deciding which method to call at runtime, not compile-time).

### ✏️ Method Overriding

* Derived class redefines a method from the base class.
* Requires `virtual` in base class method.
* Mark overriding methods with `override` for clarity.

### 🏛️ Abstract Classes

* Contain at least one **pure virtual function** (`= 0`).
* Cannot be instantiated.
* Serve as a **blueprint** for derived classes.

### 🚫 Final Classes & Methods

* `final` keyword prevents further inheritance or overriding.
* Useful for **design constraints** and **safety**.

---

## 3. Mental Model

```
OOP Principles
 ├── Inheritance
 │    ├── Base Class
 │    ├── Derived Class
 │    └── Protected Members
 └── Polymorphism
      ├── Virtual Methods
      ├── Overriding
      ├── Abstract Classes
      └── Final Classes
```

---

## 4. Interview Insights

### 💬 Common Questions

* What’s the difference between **public**, **protected**, and **private** inheritance?
* Why do we need **virtual functions** in C++?
* What’s the difference between **overloading** and **overriding**?
* Why and when would you use an **abstract class**?
* When would you mark a class or method as **final**?

---

## 5. Quick Revision Sheet

* **Protected members** → accessible by base + derived, not external.
* **Virtual methods** → enable runtime polymorphism.
* **Overriding** → redefine base behavior in derived class.
* **Abstract class** → cannot instantiate, only extend.
* **Final** → stops inheritance or overriding.

---

## 6. Best Practices

* Always use `override` keyword to avoid accidental overloading instead of overriding.
* Use abstract classes to define **interfaces**.
* Minimize use of `protected`; prefer `private` with controlled access.
* Avoid deep inheritance chains (composition often better).
* Use `final` only when you have a clear reason to prevent extension.