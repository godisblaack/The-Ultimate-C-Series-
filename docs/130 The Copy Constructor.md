# 130 The Copy Constructor

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/rBg3WQhmHmo?si=SjMK9bmrFZr6R28d"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. What is a Copy Constructor?

### 🧠 Definition
- A **copy constructor** creates a **new object** as a **copy** of an existing object.
- Signature:
  ```cpp
  ClassName(const ClassName& source);
  ```
- **Takes**: a `const` reference to the same class type.
- **Purpose**: Define how an object is copied.

---

## 2. Compiler‑Generated Copy Constructor

- If you **don’t** define one, the compiler **automatically** generates a copy constructor.
- Performs a **member‑wise copy**:
  - Each data member is copied from the source object to the new object.
- Works fine in most cases (especially for simple types and `std::string`).

---

## 3. When to Define Your Own

- When your class:
  - Manages **dynamic resources** (e.g., raw pointers, file handles).
  - Needs **custom copy logic** (e.g., deep copy).
  - Must **log or track** copy operations.
- Example:
  ```cpp
  Rectangle::Rectangle(const Rectangle& source) {
      cout << "Rectangle copied" << endl;
      width = source.width;
      height = source.height;
      color = source.color;
  }
  ```

---

## 4. Drawbacks of Manual Copy Constructor

- **Maintenance risk**: If you add a new member, you must remember to update the copy constructor.
- Forgetting to do so can cause **bugs**.
- **Best practice**: Use compiler‑generated copy constructor unless you have a specific reason to override it.

---

## 5. Copying in Action

### Copy on Assignment
```cpp
Rectangle first{10, 20};
Rectangle second = first; // Copy constructor called
```

### Copy When Passing by Value
```cpp
void showRectangle(Rectangle r) {} // Copy constructor called
showRectangle(first);
```

### Avoid Copy with Reference
```cpp
void showRectangle(const Rectangle& r) {} // No copy
```

---

## 6. Preventing Copies

- Use `= delete` to disable copying:
  ```cpp
  Rectangle(const Rectangle& source) = delete;
  ```
- Prevents:
  - Passing by value.
  - Copy initialization.
- Useful for classes managing **unique resources**.

---

## 7. Updated Rectangle Class

### Rectangle.h
```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

#include <string>
using namespace std;

class Rectangle {
public:
    Rectangle() = default;
    Rectangle(int width, int height);
    Rectangle(int width, int height, const string& color);
    Rectangle(const Rectangle& source); // Copy constructor

    void draw();
    int getArea();
    int getWidth();
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

private:
    int width = 0;
    int height = 0;
    string color;
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

Rectangle::Rectangle(int width, int height, const string& color)
    : Rectangle(width, height) {
    cout << "Constructing a Rectangle with color" << endl;
    this->color = color;
}

Rectangle::Rectangle(const Rectangle& source) {
    cout << "Rectangle copied" << endl;
    width = source.width;
    height = source.height;
    color = source.color;
}
```

---

## 8. Mental Model

```
Copy Constructor
 ├── Creates a new object
 ├── Initializes it with another object's data
 ├── Called when:
 │     ├── Passing by value
 │     ├── Returning by value
 │     └── Initializing from another object
 └── Can be:
       ├── Compiler-generated (default)
       ├── User-defined (custom logic)
       └── Deleted (prevent copying)
```

---

## 9. Quick Revision Sheet

- **Signature**: `ClassName(const ClassName& source)`
- **Default behavior**: Member‑wise copy.
- **When to override**: Resource management, logging, deep copy.
- **Avoid duplication**: Prefer compiler‑generated unless necessary.
- **Prevent copying**: `= delete`.

---

## 10. Interview Insights

💬 **Possible Questions**
- When is the copy constructor called?
- Difference between copy constructor and assignment operator?
- Why pass by `const` reference?
- How to prevent copying?
- What’s the risk of manually writing a copy constructor?

---

## 11. Code from the video

### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle first{10, 20};
    Rectangle second = first; // Copy constructor is used here.

    return 0;
}
```

### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

#include <string>

using namespace std;

class Rectangle {
public:
    Rectangle() = default;
    Rectangle (int width, int height);
    Rectangle (int width, int height, const string& color);
    Rectangle(const Rectangle& source); // Copy constructor

    void draw();
    int getArea();

    int getWidth();
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

private:
    int width = 0;
    int height = 0;
    string color;
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

Rectangle::Rectangle(int width, int height, const string& color) : Rectangle(width, height) {
    cout << "Constructing a Rectangle with color" << endl;

    this->color = color;
}

Rectangle::Rectangle(const Rectangle& source) {
    cout << "Rectangle copied" << endl;

    this->width = source.width;
    this->height = source.height;
    this->color = source.color;
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

### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle first{10, 20};
    Rectangle second = first; // User's copy constructor is used here.

    return 0;
}
```

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

// Objects are copied when passed to functions.
void showRectangle(Rectangle rectangle) {}

int main() {
    Rectangle first{10, 20};
    Rectangle second = first;

    return 0;
}
```