# 132 Static Members

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/H7Rt5Cijk5Q?si=40kgtuUnb_Beizyk"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Instance Members vs Static Members

| **Instance Members** | **Static Members** |
|----------------------|--------------------|
| Belong to **each object** | Belong to the **class itself** |
| Each object has its **own copy** | **One shared copy** for all objects |
| Accessed via object (`obj.member`) | Accessed via class name (`ClassName::member`) |
| Can access both instance and static members | Can only access static members |

---

## 2. Declaring a Static Member Variable

- Use the `static` keyword **inside the class**.
- Must **define** it **outside** the class in the `.cpp` file.

```cpp
class Rectangle {
public:
    static int objectsCount; // Declaration
    // ...
};

// Definition in .cpp
int Rectangle::objectsCount = 0;
```

---

## 3. Incrementing Static Members in Constructors

- Since `objectsCount` is shared, increment it whenever a new object is created.

```cpp
Rectangle::Rectangle(int width, int height) {
    objectsCount++; // Count new object
    setWidth(width);
    setHeight(height);
}
```

---

## 4. Accessing Static Members

### ❌ Wrong (via instance)
```cpp
cout << first.objectsCount; // Works but gives a warning
```

### ✅ Correct (via class)
```cpp
cout << Rectangle::objectsCount << endl;
```

---

## 5. Applying Data Hiding

- Move `objectsCount` to **private**.
- Provide a **static getter**.

```cpp
class Rectangle {
public:
    static int getObjectsCount(); // Static method
private:
    static int objectsCount;
};
```

---

## 6. Static Member Functions

- Declared with `static` keyword.
- Can **only** access static members (no `this` pointer).
- Belong to the class, not to any object.

```cpp
int Rectangle::getObjectsCount() {
    return objectsCount;
}
```

---

## 7. Example Usage

```cpp
int main() {
    Rectangle first{10, 20};
    Rectangle second{10, 20};

    cout << Rectangle::getObjectsCount() << endl; // Output: 2
}
```

---

## 8. Mental Model

```
Memory Layout:
 ┌────────────────────────────────┐
 │ Static: Rectangle::objectsCount│  <-- One copy shared by all
 ├────────────────────────────────┤
 │ Instance: width, height, color │  <-- Separate copy per object
 ├────────────────────────────────┤
 │ Instance: width, height, color │
 └────────────────────────────────┘
```

---

## 9. Quick Revision Sheet

- **Static variable**: `static int var;` → shared by all objects.
- **Definition**: `int ClassName::var = initValue;`
- **Access**: `ClassName::var` (preferred).
- **Static method**: Can only access static members.
- **Use cases**:
  - Object counters.
  - Shared configuration.
  - Caching.

---

## 10. Interview Insights

💬 **Possible Questions**
- Difference between instance and static members?
- Why must static members be defined outside the class?
- Can static methods access instance variables?
- How do static members behave with inheritance?
- What happens if you modify a static variable via one object?

---

## 11. Code from the video

### Part 1 - Declaring a static member variable

#### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

#include <string>

using namespace std;

class Rectangle {
public:
    static int objectsCount;

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
    return height;
}

void Rectangle::setHeight(int height) {
    if (height < 0) {
        throw invalid_argument("height");
    }

    this->height = height;
}

int Rectangle::objectsCount = 0;
```

#### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle first{10, 20};
    Rectangle second{10, 20};

    // Incorrect method to call
    cout << first.objectsCount;

    // Correct method to call
    cout << Rectangle::objectsCount << endl;

    return 0;
}
```

### Part 2 - Applying data  hiding

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
    Rectangle first{10, 20};
    Rectangle second{10, 20};

    cout << Rectangle::getObjectsCount() << endl;

    return 0;
}
```