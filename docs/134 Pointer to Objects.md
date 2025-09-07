# 134 Pointer to Objects

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/jspik9CAaVg?si=scalsHJt_Z30kyju"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Stack vs Heap Allocation

| **Stack** | **Heap** |
|-----------|----------|
| Memory automatically managed (freed when variable goes out of scope) | Memory manually managed (must `delete` when done) |
| Fast allocation/deallocation | Slower allocation/deallocation |
| Lifetime tied to scope | Lifetime controlled by programmer |
| Example: `Rectangle rect(10, 20);` | Example: `auto* rect = new Rectangle(10, 20);` |

---

## 2. Raw Pointers – Old Way

```cpp
auto* rectangle = new Rectangle(10, 20); // Allocate on heap
rectangle->draw(); // Access via arrow operator
delete rectangle; // Free memory
rectangle = nullptr; // Avoid dangling pointer
```

⚠ **Risks**:
- **Dangling pointer** → pointer to freed memory.
- **Memory leak** → forgetting to `delete`.

---

## 3. Smart Pointers – Modern C++

Header:  
```cpp
#include <memory>
```

### **Unique Pointer**
- **Exclusive ownership** — only one `unique_ptr` can point to an object.
- Automatically deletes the object when it goes out of scope.

```cpp
auto rectangle = make_unique<Rectangle>(10, 20);
rectangle->draw();
// No delete needed — destructor frees memory
```

### **Shared Pointer**
- **Shared ownership** — multiple `shared_ptr`s can point to the same object.
- Object is deleted when the last `shared_ptr` is destroyed.

---

## 4. Why Smart Pointers?
✅ No manual `delete`  
✅ No dangling pointers (auto‑null after destruction)  
✅ No leaks (RAII — Resource Acquisition Is Initialization)  

---

## 5. Building a Minimal Smart Pointer (Exercise)

### SmartPointer.h
```cpp
#ifndef ADVANCED_SMARTPOINTER_H
#define ADVANCED_SMARTPOINTER_H

class SmartPointer {
public:
    explicit SmartPointer(int* ptr);
    ~SmartPointer();

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
```

### main.cpp
```cpp
#include "SmartPointer.h"

int main() {
    SmartPointer ptr{new int}; // Allocates int on heap
    // Memory automatically freed when ptr goes out of scope
}
```

---

## 6. Mental Model

```
SmartPointer<int>
 ├── Holds raw pointer
 ├── Constructor stores pointer
 ├── Destructor deletes pointer
 └── No manual delete needed
```

---

## 7. Interview Insights

💬 **Possible Questions**
- Difference between stack and heap allocation.
- Why use smart pointers over raw pointers?
- How does `unique_ptr` differ from `shared_ptr`?
- What is RAII and how do smart pointers implement it?
- What is a dangling pointer and how to avoid it?

---

## 8. Next Step
In the next section, **operator overloading** will be used to add:
- `*` (dereference) operator
- `->` (member access) operator  
…so our custom `SmartPointer` behaves more like a real `unique_ptr`.

---

## 9. Code from the video

### Part 1 - Pointer to objects

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

#### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    auto* rectangle = new Rectangle(10, 20);
    // It is same as: Rectangle* rectangle = new Rectangle(10, 20);

    rectangle->draw();

    delete rectangle;
    
    rectangle = nullptr;

    return 0;
}
```

### Part 2 - Using smart pointers

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
#include <memory>

using namespace std;

int main() {
    auto rectangle = make_unique<Rectangle>(10, 20);

    rectangle->draw();

    // Destructor will be called and memory will be released

    return 0;
}
```

## Exercise

### SmartPointer.h

```cpp
#ifndef ADVANCED_SMARTPOINTER.H
#define ADVANCED_SMARTPOINTER.H

class SmartPointer {
public:
    explicit SmartPointer(int* ptr);
    ~SmartPointer();

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
```

### main.cpp

```cpp
#include "SmartPointer.h"

#include <iostream>

using namespace std;

int main() {
    // Previously used: int* ptr = new int; to declare integer pointer in heap.
     
    SmartPointer ptr{new int};

    return 0;
}
```