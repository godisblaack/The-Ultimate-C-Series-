# 135 Array of Objects

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/c4FOKytUW6U?si=4T9g1hRuVnZxsD93"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Arrays of Objects – The Basics

- You can create an array of objects **just like** arrays of built‑in types.
- Syntax:
  ```cpp
  Rectangle rectangles[3];
  ```
- **Requirement**: The class must have a **default constructor** so the compiler knows how to create each element without arguments.

---

## 2. Why a Default Constructor is Needed

- When you write:
  ```cpp
  Rectangle rectangles[3];
  ```
  the compiler:
  1. Allocates space for 3 `Rectangle` objects.
  2. Calls the **default constructor** for each.
- If no default constructor exists, compilation fails:
  ```
  error: no matching constructor for initialization of 'Rectangle'
  ```

---

## 3. Initializing Arrays of Objects

### **Method 1 – Explicit Constructor Calls**
```cpp
Rectangle rectangles[] = {
    Rectangle(),
    Rectangle(10, 20),
    Rectangle(10, 20, "blue")
};
```

### **Method 2 – Brace Initializer (Concise)**
```cpp
Rectangle rectangles[] = { // = is not necessary
    {},             // Default constructor
    {10, 20},       // 2‑param constructor
    {10, 20, "blue"}// 3‑param constructor
};
```
- The compiler knows each element is a `Rectangle` and calls the matching constructor.

---

## 4. Accessing Elements

- **By index**:
  ```cpp
  rectangles[0].draw();
  ```
- **Range‑based for loop**:
  ```cpp
  for (Rectangle rect : rectangles) {
      rect.draw(); // Copies each element
  }
  ```
- **Optimized with reference**:
  ```cpp
  for (Rectangle& rect : rectangles) {
      rect.draw(); // No copy, works directly on array element
  }
  ```

---

## 5. Memory & Lifetime

- Arrays created like this are on the **stack**.
- All elements are destroyed automatically when they go out of scope.
- Destructors are called for each element in reverse order of construction.

---

## 6. Mental Model

```
Rectangle rectangles[3];
 ├── rectangles[0] → default ctor
 ├── rectangles[1] → default ctor
 └── rectangles[2] → default ctor
```

With initializer list:
```
Rectangle rectangles[] = { {}, {10,20}, {10,20,"blue"} };
 ├── rectangles[0] → default ctor
 ├── rectangles[1] → ctor(int,int)
 └── rectangles[2] → ctor(int,int,string)
```

---

## 7. Quick Revision Sheet

- **Default constructor** required for fixed‑size arrays without explicit initialization.
- Use **brace initializers** for concise syntax.
- Prefer **reference** in range‑based loops to avoid copies.
- Stack arrays → automatic destruction.
- Each element is constructed individually.

---

## 8. Interview Insights

💬 **Possible Questions**
- Why is a default constructor required for arrays of objects?
- How does the compiler decide which constructor to call for each element?
- What’s the difference between `for (T x : arr)` and `for (T& x : arr)`?
- How are destructors called for array elements?

---

## 9. Code from the video

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

    static int getObjectsCount();

    int width = 0;
    int height = 0;
    string color;

    static int objectsCount;
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

### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle rectangles[3]; // Only work if there is a default constructor

    return 0;
}
```

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle rectangles[] = { // = is not necessary
        Rectangle(),
        Rectangle(10, 20),
        Rectangle(10, 20, "blue")
    };

    recatanles[0].draw();

    for (Rectangle rect : rectangles) {
        rect.draw();
    }

    return 0;
}
```

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle rectangles[] = {
        {},
        {10, 20},
        {10, 20, "blue"}
    };

    for (Rectangle& rect : rectangles) {
        rect.draw();
    }

    return 0;
}
```