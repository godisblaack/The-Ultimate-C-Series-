# 148 Overloading the Indirection Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/ZfYnt7StHgY?si=bmmwzBHO--GYadTu"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem with Current SmartPointer

### ❌ Limitation

* Earlier, we built a simple `SmartPointer` class.
* It managed memory (constructor accepted `int*`, destructor released it).
* But **we couldn’t access or modify the actual integer** inside the pointer.

### 🧩 Needed Feature

* We need to access the underlying value (like raw pointers do with `*`).
* Solution: **Overload the indirection (`*`) operator**.

---

## 2. Overloading Indirection Operator

### 📌 Concept

* By overloading `operator*`, we can:

  * Dereference our `SmartPointer` object directly.
  * Access or modify the integer it points to.
* Return type must be `int&` (reference), so:

  * We don’t create a copy.
  * We can read/write the value in memory.

### ⚠️ Real-World Note

* In practice, you don’t need to write this.
* The Standard Library provides `std::unique_ptr`, `std::shared_ptr`, etc.
* This exercise is just to **understand how STL smart pointers are built**.

---

## 3. Implementation

### 🛠️ Key Steps

1. Define `operator*()` in class header.
2. Implement it by dereferencing `ptr`.
3. Use it like a raw pointer.

---

## 4. Code Walkthrough

### SmartPointer.h

```cpp
#ifndef ADVANCED_SMARTPOINTER_H
#define ADVANCED_SMARTPOINTER_H

class SmartPointer {
public:
    explicit SmartPointer(int* ptr);
    ~SmartPointer();

    int& operator*(); // Overload indirection operator

private:
    int* ptr;
};

#endif
```

### SmartPointer.cpp

```cpp
#include "SmartPointer.h"

SmartPointer::SmartPointer(int* ptr) : ptr{ptr} {}

SmartPointer::~SmartPointer() {
    delete ptr;
    ptr = nullptr;
}

int& SmartPointer::operator*() {
    return *ptr; // Dereference the stored pointer
}
```

### main.cpp

```cpp
#include "SmartPointer.h"
#include <iostream>

using namespace std;

int main() {
    SmartPointer ptr{new int}; // Allocates memory on heap

    *ptr = 100;                // Use overloaded operator*
    cout << *ptr << endl;

    *ptr = 101;                // Modify value
    cout << *ptr << endl;

    return 0;                  // Destructor frees memory
}
```

---

## 5. Output

```
100
101
```

---

## 6. Mental Model

```
SmartPointer
 ├── Holds: int* ptr
 ├── Destructor: delete ptr
 └── operator*()
       ↓
    return *ptr
```

---

## 7. Interview Insights

### 💬 Common Questions

* Why should `operator*` return a reference instead of a value?
* How is `SmartPointer` different from `std::unique_ptr`?
* What happens if you forget to implement destructor?
* Why should we prefer smart pointers over raw pointers?

---

## 8. Quick Revision Sheet

* `operator*()` lets objects behave like pointers.
* Always return **reference** for dereferencing.
* Destructor is crucial to prevent memory leaks.
* `new` allocates memory on heap → destructor must `delete`.
* Smart pointers in STL are **safer and preferred**.

---

## 9. Best Practices

* Never leave raw `new`/`delete` scattered in code → wrap in RAII (Resource Acquisition Is Initialization).
* Prefer `std::unique_ptr` or `std::shared_ptr` in production.
* Mark single-argument constructors as `explicit` to avoid implicit conversions.
* After deleting a pointer, reset it to `nullptr` to avoid dangling pointers.

---

## 10. Code from the video

### SmartPointer.h

```cpp
#ifndef ADVANCED_SMARTPOINTER.H
#define ADVANCED_SMARTPOINTER.H

class SmartPointer {
public:
    explicit SmartPointer(int* ptr);
    ~SmartPointer();

    int& operator*();

private:
    int* ptr;
};

#endif
```

### SmartPointer.cpp

```cpp
#include "SmartPointer.h"

SmartPointer::SmartPointer(int* ptr) : ptr{ptr} {}

SmartPointer::~SmartPointer() {
    delete ptr;

    ptr = nullptr;
}

int& SmartPointer::operator*() {
    return *ptr;
}
```

### main.cpp

```cpp
#include "SmartPointer.h"

#include <iostream>

using namespace std;

int main() {
    SmartPointer ptr{new int};

    *ptr = 100;
    
    cout << *ptr << endl;

    *ptr = 101;

    cout << *ptr << endl;

    return 0;
}
```