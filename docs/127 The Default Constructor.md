# 127 The Default Constructor

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/iUzguEtb2VI?si=gSZX1B9ylW4ZiIAP"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Recap

- After adding a **parameterized constructor**:
  ```cpp
  Rectangle(int width, int height);
  ```
  you **can no longer** create:
  ```cpp
  Rectangle r; // ❌ compilation error
  ```
- Error: *no matching constructor for initialization of 'Rectangle'*.

---

## 2. Why This Happens

- **Rule**:  
  The compiler **automatically** generates a default constructor **only if**:
  - You **don’t** declare **any** constructor yourself.
- Once you declare **any** constructor (e.g., parameterized), the compiler **stops** generating the default one.

---

## 3. Solutions

### **Method 1 – Manually Define a Default Constructor**
```cpp
class Rectangle {
public:
    Rectangle(); // declaration
    Rectangle(int width, int height);
    ...
};

Rectangle::Rectangle() {
    // Empty body – members already initialized in-class
}
```
✅ Works in all C++ versions  
❌ Slightly more boilerplate

---

### **Method 2 – Use `= default` (Modern C++)**
```cpp
class Rectangle {
public:
    Rectangle() = default; // compiler generates it
    Rectangle(int width, int height);
    ...
};
```
✅ Cleaner, lets compiler generate optimal code  
✅ Expresses intent clearly  
❌ Requires C++11 or later

---

## 4. When to Use Each

| Case | Recommended |
|------|-------------|
| Pre‑C++11 codebase | Manual definition |
| Modern C++ | `= default` |
| Need custom logic in default constructor | Manual definition |
| Just want compiler‑generated behavior | `= default` |

---

## 5. In‑Class Member Initialization

- If you initialize members in the class declaration:
  ```cpp
  class Rectangle {
      int width{0};
      int height{0};
  };
  ```
  then your default constructor (manual or `= default`) **doesn’t need** to set them again.

---

## 6. Mental Model

```
Without any constructor:
    Compiler → generates default constructor

With only parameterized constructor:
    Compiler → does NOT generate default constructor

Want both:
    You → must declare default constructor (manual or = default)
```

---

## 7. Quick Revision Sheet

- **Default Constructor**: No parameters, initializes object to default state.
- **Compiler Rule**: Generated only if no constructors are declared.
- **Overloading**: You can have multiple constructors with different signatures.
- **Modern Shortcut**: `= default` tells compiler to generate it.
- **Best Practice**: Use `= default` unless you need custom initialization logic.

---

## 8. Interview Insights

💬 **Possible Questions**
- When does the compiler generate a default constructor?
- What happens if you define a parameterized constructor but no default one?
- Difference between `= default` and manually defining a constructor?
- Why might you still define a default constructor manually?

---

## 9. Code from the video

### Method 1

#### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    Rectangle();
    Rectangle (int width, int height);

    void draw();
    int getArea();

    int getWidth();
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

private:
    int width = 0;
    int height = 0;
};

#endif
```

#### Rectangle.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;
Rectangle::Rectangle() {}

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

    this->width = width;
}

int Rectangle::getHeight() const {
    return height;
}

void Rectangle::setHeight(int height) {
    if (height < 0) {
        throw invalid_argument("height");
    }

    this->height = height;
}
```

### Method 2

#### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    Rectangle() = default;
    Rectangle (int width, int height);

    void draw();
    int getArea();

    int getWidth();
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

private:
    int width = 0;
    int height = 0;
};

#endif
```

#### Rectangle.cpp

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

    this->width = width;
}

int Rectangle::getHeight() const {
    return height;
}

void Rectangle::setHeight(int height) {
    if (height < 0) {
        throw invalid_argument("height");
    }

    this->height = height;
}
```