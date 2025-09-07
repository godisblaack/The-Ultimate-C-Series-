# 119 Introduction

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/7s0zKU0Fxws?si=uhzthsiUm-aQJeI-" 
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

```cpp
#include <iostream>

using namespace std;

int main() {
    cout << "Hello World";

    return 0;
}
```

## 1. Why Classes Matter
- **Classes** are the cornerstone of **Object-Oriented Programming (OOP)** in C++.
- They allow you to define **new data types** that bundle **data (attributes)** and **functions (methods)** together.
- OOP is the dominant paradigm in modern C++ applications because it promotes:
  - **Encapsulation** – hiding internal details
  - **Abstraction** – exposing only essential features
  - **Inheritance** – reusing and extending code
  - **Polymorphism** – enabling flexible, interchangeable components

---

## 2. Core Building Blocks of a Class
1. **Access Modifiers**
   - `public` – accessible from anywhere
   - `private` – accessible only within the class
   - `protected` – accessible within the class and its derived classes

2. **Constructors**
   - Special functions called when an object is created
   - Used to initialize member variables
   - Can be **default**, **parameterized**, or **copy constructors**

3. **Destructors**
   - Special functions called when an object is destroyed
   - Used for cleanup (e.g., releasing memory, closing files)

4. **Static Members**
   - Belong to the class, not individual objects
   - Shared across all instances
   - Can be **static data members** or **static member functions**

---

## 3. Modern C++ Considerations
- Use **member initializer lists** for efficient initialization
- Prefer **`= default`** and **`= delete`** for controlling special member functions
- Leverage **`explicit`** constructors to avoid unintended conversions
- Understand **RAII** (Resource Acquisition Is Initialization) for safe resource management

---

## 4. Interview & Revision Tips
- Be ready to explain **why** OOP is used in C++
- Know the **order of constructor/destructor calls** in inheritance
- Understand **when to use static members** vs. instance members
- Practice writing **small, self-contained class examples** that demonstrate encapsulation

---

## 5. Quick Mental Map
```
Class
 ├── Data Members (state)
 ├── Member Functions (behavior)
 ├── Access Modifiers
 ├── Constructors
 ├── Destructor
 └── Static Members
```