# 122 Creating Objects

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/25p7mETe8ig?si=hL_yRazo3oo781DH"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Separation of Interface and Implementation

### 🔧 Why Split `.h` and `.cpp`?
- **Header file (`Rectangle.h`)**: Declares the class interface (attributes + method prototypes).
- **Source file (`Rectangle.cpp`)**: Implements the methods.
- **Main file (`main.cpp`)**: Uses the class via the header.

### ⚡ Compilation Optimization
- Changes in `.cpp` → only that file is recompiled.
- Changes in `.h` → all dependent files must be recompiled.
- This separation improves **build efficiency** in large projects.

---

## 2. Creating Objects from a Class

### 🧱 Syntax
```cpp
Rectangle rectangle;
```
- Creates an object named `rectangle` of type `Rectangle`.
- Similar to declaring a variable of a built-in type.

### 🧩 Accessing Members
```cpp
rectangle.width = 10; // ❌ Compilation error
```
- Members are `private` by default in `class`.
- Cannot access `width` or `height` directly unless they are made `public`.

---

## 3. Access Modifiers

### 🔐 Default Behavior
- `class` → members are `private` by default.
- `struct` → members are `public` by default.

### ✅ Making Members Public
```cpp
class Rectangle {
public:
    int width;
    int height;
    void draw();
    int getArea();
};
```

### 🔍 Dot Operator
- Used to access public members:
```cpp
rectangle.width = 10;
rectangle.height = 20;
cout << rectangle.getArea();
```

---

## 4. Memory Allocation & Object Identity

### 🧠 Each Object Is Independent
```cpp
Rectangle first;
Rectangle second;
```
- `first` and `second` are **distinct instances**.
- Stored at different memory addresses.

### 📍 Printing Addresses
```cpp
cout << &first << endl;
cout << &second << endl;
```
- Demonstrates that each object occupies its own space in memory.

---

## 5. Full Code Recap

### Rectangle.h
```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    int width;
    int height;
    void draw();
    int getArea();
};

#endif
```

### Rectangle.cpp
```cpp
#include "Rectangle.h"
#include <iostream>
using namespace std;

void Rectangle::draw() {
    cout << "Drawing a rectangle" << endl;
    cout << "Dimensions: " << width << ", " << height << endl;
}

int Rectangle::getArea() {
    return width * height;
}
```

### main.cpp
```cpp
#include "Rectangle.h"
#include <iostream>
using namespace std;

int main() {
    Rectangle first;
    Rectangle second;

    first.width = 10;
    first.height = 20;

    cout << "Area: " << first.getArea() << endl;
    cout << "Address of first: " << &first << endl;
    cout << "Address of second: " << &second << endl;

    return 0;
}
```

---

## 6. Interview Insights

### 💡 Key Questions
- Why separate `.h` and `.cpp` files?
- What’s the default access level in a class vs. struct?
- How do you expose private members safely? (→ Getters/Setters)
- How does memory layout differ between objects?

### 🧠 Mental Model
```
Class
 ├── Interface (.h)
 │    ├── Public members
 │    └── Private members
 └── Implementation (.cpp)
      └── Method definitions

Object
 ├── Unique memory address
 └── Independent state
```

---

## 7. Quick Revision Sheet

- `class` → members are `private` by default.
- Use `public:` to expose members.
- Use `.` to access members of an object.
- Each object has a unique memory address.
- Include only `.h` in `main.cpp`, not `.cpp`.

---

## 8. Best Practices

- Avoid making data members `public` — use **encapsulation**.
- Prefer **getters/setters** for controlled access.
- Use **constructors** for initialization (coming next).
- Don’t use `using namespace std;` in headers — restrict it to `.cpp`.

## 9. Code from the video

### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    int width;
    int height;
    void draw();
    int getArea();
};

#endif
```

### Rectangle.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

void Rectangle::draw() {
    cout << "Drawaing a rectanle" << endl;
    cout << "Dimensions: " << width << ", " << height << endl;
}

int Rectangle::getArea() {
    return width * height;
}
```

### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle rectangle;
    rectangle.width = 10;
    rectangle.height = 20;
    cout << rectangle.getArea();

    return 0;
}
```

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle first;
    Rectangle second;
    cout << &first << endl;
    cout << &second << endl;

    return 0;
}
```