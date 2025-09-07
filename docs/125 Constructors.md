# 125 Constructors

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/KfEcIGfa3fE?si=6QoYkSaLE_w5dZpz"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Constructors Matter

### 🧠 The Problem Without Initialization
- If we **don’t** call a setter or initialize members, they may hold **garbage values**.
- Example:  
  ```cpp
  Rectangle r;
  cout << r.getWidth(); // Undefined value
  ```
- This can lead to **unpredictable behavior** and **hard-to-find bugs**.

---

## 2. Two Ways to Initialize Members

### **Method 1 – Default Member Initializers**
- Assign default values **inside the class definition**.
- Example:
  ```cpp
  private:
      int width = 0;
      int height = 0;
  ```
- ✅ Simple and ensures members always start with a known value.
- ❌ Doesn’t allow **custom initialization** at object creation.

---

### **Method 2 – Constructor**
- A **special function** that runs **automatically** when an object is created.
- **Same name** as the class.
- **No return type** (not even `void`).
- Can take parameters to set initial values.

---

## 3. Constructor Basics

### 📜 Declaration
```cpp
Rectangle(int width, int height);
```

### 📦 Definition
```cpp
Rectangle::Rectangle(int width, int height) {
    cout << "Constructing a Rectangle" << endl;
    setWidth(width);   // Reuse validation logic
    setHeight(height);
}
```

**Why reuse setters?**
- Avoids duplicating validation logic.
- Keeps all input checks in one place.

---

## 4. Object Creation Syntax

### **Parentheses**
```cpp
Rectangle r(10, 20);
```

### **Curly Braces (Modern C++)**
```cpp
Rectangle r{10, 20};
```
- Preferred in modern C++ for **uniform initialization**.
- Both forms are mostly equivalent, but curly braces avoid **narrowing conversions**.

---

## 5. Exception Safety

- If invalid values are passed:
  ```cpp
  Rectangle r{-5, 10}; // Throws invalid_argument
  ```
- The object is **never constructed** in an invalid state.
- This enforces **class invariants**.

---

## 6. Updated Rectangle Class

### Rectangle.h
```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    Rectangle(int width, int height); // Constructor

    void draw();
    int getArea();

    int getWidth();
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

private:
    // Method 1 alternative:
    // int width = 0;
    // int height = 0;

    int width;
    int height;
};

#endif
```

### Rectangle.cpp
```cpp
#include "Rectangle.h"
#include <iostream>
#include <stdexcept>
using namespace std;

Rectangle::Rectangle(int width, int height) {
    cout << "Constructing a Rectangle" << endl;
    setWidth(width);
    setHeight(height);
}

void Rectangle::draw() {
    cout << "Drawing a rectangle" << endl;
    cout << "Dimensions: " << width << ", " << height << endl;
}

int Rectangle::getArea() {
    return width * height;
}

int Rectangle::getWidth() {
    return width;
}

void Rectangle::setWidth(int width) {
    if (width < 0) throw invalid_argument("width");
    this->width = width;
}

int Rectangle::getHeight() const {
    return height;
}

void Rectangle::setHeight(int height) {
    if (height < 0) throw invalid_argument("height");
    this->height = height;
}
```

### main.cpp
```cpp
#include "Rectangle.h"
#include <iostream>
using namespace std;

int main() {
    Rectangle rectangle{10, 20};
    cout << rectangle.getWidth();
    return 0;
}
```

---

## 7. Interview Insights

### 💬 Common Questions
- What’s the difference between **default member initializers** and **constructors**?
- Why should you **reuse setters** inside constructors?
- What happens if a constructor **throws an exception**?
- Why are curly braces preferred over parentheses in modern C++?

---

## 8. Mental Model

```
Object Creation
 ├── Allocate memory
 ├── Call constructor
 │    ├── Validate inputs
 │    ├── Initialize members
 │    └── Enforce invariants
 └── Object ready for use
```

---

## 9. Quick Revision Sheet

- **Default member initializers** → simple defaults.
- **Constructors** → flexible, parameterized initialization.
- Always **validate inputs** (reuse setters).
- **Curly braces** → modern, safe initialization.
- Throw exceptions to prevent invalid objects.

---

## 10. Best Practices

- Keep **all public members grouped** for clarity.
- Avoid **duplicate validation logic**.
- Use **const** on getters when they don’t modify state.
- Prefer **uniform initialization** with `{}`.
- Ensure constructors leave objects in a **valid state**.

---

## 11. Code from the video

### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    // Method 2
    Rectangle (int width, int height);

    void draw();
    int getArea();

    int getWidth();
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

private:
    // Method 1
    int width = 0;
    int height = 0;
};

#endif
```

### Rectangle.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

Rectangle::Rectangle(int width, int height) {
    cout << "Constructing a Rectangle" << endl;

    setWidth(width);
    setHeight(height);
}

void Rectangle::draw() {
    cout << "Drawaing a rectanle" << endl;
    cout << "Dimensions: " << width << ", " << height << endl;
}

int Rectangle::getArea() {
    return width * height;
}

int Rectangel::getWidth() {
    return width;
}

void Rectangle::setWidth(int width) {
    if (width < 0) {
        throw invalid_argument("width");
    }

    this->width = width; // (*this).width = width;
}

int Rectangle::getHeight() const {
    return height;
}

void Rectangle::setHeight(int height) {
    if (height < 0) {
        throw invalid_argument("height");
    }

    this->height = height; // Rectangle::height = height;
}
```

### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle rectangle{10, 20}; // Rectangle rectangle(10, 20);
    cout << rectangle.getWidth();

    return 0;
}
```