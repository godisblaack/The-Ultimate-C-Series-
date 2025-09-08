# 133 Constant Objects and Functions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/scLSqvJwseM?si=sP_ipW3E7_hpl79z"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. `const` Objects

### 🧠 What Happens
- Declaring an object as `const` makes **all its data members read‑only**.
- The object becomes **immutable** — you cannot change its state after creation. Even if the attributes are **public**.

```cpp
const Rectangle rect;
rect.width = 5; // ❌ Compilation error
```

---

## 2. Effect on Methods

- You **cannot** call non‑`const` member functions on a `const` object.
- Only **`const` member functions** are callable.

Example:
```cpp
const Rectangle rect;
rect.setWidth(10); // ❌ Not allowed
rect.getHeight();  // ✅ Allowed if getHeight() is const
```

---

## 3. Declaring `const` Member Functions

- Add `const` **after** the parameter list in the declaration and definition.
- Tells the compiler:  
  *"This function will not modify the object’s state."*

```cpp
int getHeight() const; // Declaration
int Rectangle::getHeight() const { return height; } // Definition
```

- Inside a `const` function:
  - You **cannot** modify member variables.
  - You **cannot** call non‑`const` member functions.

---

## 4. Why Some Methods Disappear for `const` Objects

- If a method is **not** marked `const`, the compiler assumes it *might* modify the object.
- Therefore, it’s **not callable** on a `const` object.
- Example:
  - `draw()` without `const` → not callable on `const Rectangle`.
  - Add `const` → callable.

---

## 5. Static Member Functions & `const` Objects

- Static methods are **not tied to an object instance**.
- They can be called on a `const` object, but **should** be called via the class name.

```cpp
Rectangle::getObjectsCount(); // ✅ Preferred
rect.getObjectsCount();       // ⚠ Works, but not recommended
```

---

## 6. Updated Rectangle Class

### Rectangle.h
```cpp
class Rectangle {
public:
    Rectangle() = default;
    Rectangle(int width, int height);
    Rectangle(int width, int height, const string& color);
    Rectangle(const Rectangle& source);
    ~Rectangle();

    void draw() const;
    int getArea() const;
    int getWidth() const;
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

    static int getObjectsCount();

private:
    int width = 0;
    int height = 0;
    string color;

    static int objectsCount;
};
```

### Rectangle.cpp
```cpp
void Rectangle::draw() const {
    cout << "Drawing a rectangle" << endl;
    cout << "Dimensions: " << width << ", " << height << endl;
}

int Rectangle::getArea() const {
    return width * height;
}

int Rectangle::getWidth() const {
    return width;
}

int Rectangle::getHeight() const {
    return height;
}
```

---

## 7. Mental Model

```
const Rectangle obj;
 ├── All data members are read-only
 ├── Only const methods are callable
 ├── Static methods are callable (but via class name)
 └── Compiler enforces immutability
```

---

## 8. Quick Revision Sheet

- **`const` object** → immutable state.
- **`const` method** → promises not to modify object.
- Mark all **read‑only** methods as `const`.
- Static methods are unaffected by `const` objects.
- Call static methods via **class name**, not object.

---

## 9. Interview Insights

💬 **Possible Questions**
- What’s the difference between a `const` object and a `const` pointer?
- Why can’t you call non‑`const` methods on a `const` object?
- How does the compiler enforce `const` correctness?
- Why is `const` correctness important in large codebases?

---

## 10. Code from the video

### Part 1

#### Rectangle.h

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
    Rectangle(const Rectangle& source); 
    ~Rectangle();

    void draw();
    int getArea();

    int getWidth();
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

    static int getObjectsCount();

    int width = 0;
    int height = 0;
    string color;

    static int objectsCount;
};

#endif
```

#### Rectangle.cpp

```cpp
#include "Rectangle.h"

#include <iostream>
#include <stdexcept>

using namespace std;

Rectangle::Rectangle(int width, int height) {
    objectsCount++;

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

Rectangle::~Rectangle() {
    cout << "Destructor called" << endl;;
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
    width = 1; // Compilation error
    return height;
}

void Rectangle::setHeight(int height) {
    if (height < 0) {
        throw invalid_argument("height");
    }

    this->height = height;
}

int Rectangle::objectsCount = 0;

int Rectangle::getObjectsCount() {
    return objectsCount;
}
```

#### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    // Immutable
    const Rectangle rectangle;

    rectangle.width = 1; // Compilation error

    return 0;
}
```

### Part 2

#### Rectangle.h

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
    Rectangle(const Rectangle& source); 
    ~Rectangle();

    void draw() const;
    int getArea() const;
    int getWidth() const;
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

    static int getObjectsCount();

private:
    int width = 0;
    int height = 0;
    string color;

    static int objectsCount;
};

#endif
```

#### Rectangle.cpp

```cpp
#include "Rectangle.h"

#include <iostream>
#include <stdexcept>

using namespace std;

Rectangle::Rectangle(int width, int height) {
    objectsCount++;

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

Rectangle::~Rectangle() {
    cout << "Destructor called" << endl;;
}

void Rectangle::draw() const {
    cout << "Drawaing a rectanle" << endl;
    cout << "Dimensions: " << width << ", " << height << endl;
}

int Rectangle::getArea() const {
    return width * height;
}

int Rectangel::getWidth() const {
    return width;
}
    
int Rectangle::getHeight() const {
    return height;
}

void Rectangle::setWidth(int width) {
    if (width < 0) {
        throw invalid_argument("width");
    }

    this->width = width;
}

void Rectangle::setHeight(int height) {
    if (height < 0) {
        throw invalid_argument("height");
    }

    this->height = height;
}

int Rectangle::objectsCount = 0;

int Rectangle::getObjectsCount() {
    return objectsCount;
}
```

#### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    // Immutable
    const Rectangle rectangle;

    rectangle.getObjectCount; // Not the right way to call it.

    Rectangel::getObjectCount();

    return 0;
}
```