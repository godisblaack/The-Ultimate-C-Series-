# 131 The Destructor

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/R6ZWmhO-hMc?si=t0zaR6NY7WSStXK_"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. What is a Destructor?

### 🧠 Definition
- A **destructor** is a special member function that is **automatically called** when an object is destroyed.
- Purpose:  
  - Release system resources (memory, file handles, network connections, etc.).
  - Perform cleanup tasks before the object’s lifetime ends.

---

## 2. Destructor Syntax

- Declared in the class with a **tilde (~)** followed by the class name.
- No return type, no parameters.
- Cannot be overloaded — **only one destructor per class**.

```cpp
class Rectangle {
public:
    ~Rectangle(); // Destructor declaration
};
```

---

## 3. Destructor Definition

```cpp
Rectangle::~Rectangle() {
    cout << "Destructor called" << endl;
}
```

---

## 4. When is the Destructor Called?

- **Automatic (stack) objects**: When they go out of scope.
- **Dynamic (heap) objects**: When `delete` is called.
- **Temporary objects**: At the end of the full expression in which they are created.

Example:
```cpp
int main() {
    Rectangle r1{10, 20}; // Constructor runs
} // Destructor runs here automatically
```

---

## 5. Destructor in Our Rectangle Class

- Our `Rectangle` doesn’t allocate resources, so a destructor isn’t strictly necessary.
- Still useful for:
  - Logging object lifecycle.
  - Understanding object destruction timing.

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
    ~Rectangle(); // Destructor

    // ...
};
```

### Rectangle.cpp
```cpp
Rectangle::~Rectangle() {
    cout << "Destructor called" << endl;
}
```

---

## 7. Mental Model

```
Object Lifecycle
 ├── Constructor → sets up object
 ├── Object in use
 └── Destructor → cleans up before memory is reclaimed
```

---

## 8. Quick Revision Sheet

- **Syntax**: `~ClassName();`
- **No parameters**, **no return type**, **cannot be overloaded**.
- Called automatically when object’s lifetime ends.
- Use for releasing resources (memory, files, sockets).
- If no destructor is defined, compiler generates a default one.

---

## 9. Interview Insights

💬 **Possible Questions**
- When is a destructor called?
- Can a destructor be overloaded?
- What happens if a class manages dynamic memory but has no destructor?
- Difference between destructor and `delete` operator?
- What is RAII (Resource Acquisition Is Initialization) and how does it relate to destructors?

---

## 10. Code from the video

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
    Rectangle(const Rectangle& source); 
    ~Rectangle();

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

    return 0;
}
```