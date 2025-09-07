# 121 Defining a Class

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Q3R7-OPGy-Y?si=xLXX1T96EuJ5P8SE"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Recap
- **Member Variables / Attributes:**
  - Variables inside a class that hold data.
  - In UML: called *attributes*.
  - In C++: officially *member variables*.
  - In other languages: *fields* or *properties*.
- **Member Functions / Methods:**
  - Functions inside a class that operate on its data.
  - In C++: officially *member functions*.
  - We'll use the term *methods* for brevity.

---

## 2. Class Design – `Rectangle`
**Members:**
- **Attributes:**
  - `width` (int)
  - `height` (int)
- **Methods:**
  - `draw()` – outputs rectangle info.
  - `getArea()` – returns computed area.

---

## 3. Header & Implementation Separation
- **Header File (`.h` / `.hpp`):**
  - Declares the class interface (attributes + method prototypes).
  - Uses **header guards** to prevent multiple inclusion:
    ```cpp
    #ifndef ADVANCED_RECTANGLE_H
    #define ADVANCED_RECTANGLE_H
    ...
    #endif
    ```
  - Analogy: **Remote control** – buttons visible to the user, internal electronics hidden.

- **Source File (`.cpp`):**
  - Implements the methods declared in the header.
  - Includes the header file with **double quotes**:
    ```cpp
    #include "Rectangle.h"
    ```
  - Includes standard library headers with **angle brackets**:
    ```cpp
    #include <iostream>
    ```

---

## 4. Scope Resolution Operator `::`
- Used to define a class method **outside** the class body:
  ```cpp
  void Rectangle::draw() { ... }
  ```
- Syntax:  
  ```
  ClassName::methodName(parameters)
  ```

---

## 5. Full Example

### Rectangle.h
```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
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

---

## 6. Key Points & Best Practices
- **Encapsulation:** Keep attributes private (default in `class`) and expose behavior via methods.
- **Header Guards:** Prevent multiple inclusion errors during compilation.
- **Namespace Usage:** `using namespace std;` is fine for small examples, but avoid in headers for large projects.
- **Implementation Separation:** Improves compilation speed and code organization.
- **Access to Members:** Methods can directly access other members of the same class.

---

## 7. Interview Tips
- Be able to explain:
  - Difference between **declaration** (in `.h`) and **definition** (in `.cpp`).
  - Purpose of **scope resolution operator**.
  - Why we use **header guards**.
- Common follow-up questions:
  - How to make attributes accessible? (Getters/Setters)
  - How to initialize attributes? (Constructors)
  - Difference between `struct` and `class` in C++.

---

## 8. Quick Revision Sheet
- **Attributes:** Data members inside a class.
- **Methods:** Functions inside a class.
- **Header Guards:** `#ifndef / #define / #endif`.
- **Scope Resolution:** `ClassName::methodName`.
- **Include Syntax:**  
  - Project files → `"filename.h"`  
  - Standard library → `<header>`

---

## 9. Mental Map
```
Rectangle Class
 ├── Attributes
 │    ├── width : int
 │    └── height : int
 └── Methods
      ├── draw() : void
      └── getArea() : int
```

## 10. Code from the video

### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
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