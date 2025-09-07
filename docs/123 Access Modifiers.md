# 123 Access Modifiers

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/lTPu-xi6VUk?si=BvN4flEORWO4-qVJ"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. The Problem: Invalid Object State

### ❌ What's Wrong?
- In the current design, we can do:
```cpp
    #include "Rectangle.h"
    #include <iostream>
    
    using namespace std;

    int main() {
        Rectangle rectangle;

        rectangle.width = -1;

        return 0;
    }
```
- This leads to an **invalid state** — a rectangle with negative width makes no sense.
- Objects with bad internal data can behave incorrectly or unpredictably.

---

## 2. The Solution: Data Hiding

### 🔒 Principle of Data Hiding
- A class should **hide its internal data** from outside code.
- External code should interact with the object only through **controlled interfaces** (methods).
- Analogy: A **TV remote** hides its internal electronics and exposes only buttons.

### 💡 Benefits
- Prevents accidental misuse
- Allows internal changes without affecting external code
- Enforces **invariants** (e.g., width must be positive)

---

## 3. Access Modifiers in C++

| Modifier   | Meaning                                                                 |
|------------|-------------------------------------------------------------------------|
| `public`   | Accessible from anywhere                                                |
| `private`  | Accessible only within the class                                        |
| `protected`| Accessible within the class and derived classes (covered in inheritance)|

### ✅ Best Practice
- Declare **public methods first** for readability.
- Keep **data members private** to enforce encapsulation.

---

## 4. Updated Class Design

### Rectangle.h
```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    void draw();
    int getArea();

private:
    int width;
    int height;
};

#endif
```

### 🔍 What Changed?
- `width` and `height` moved to the `private` section.
- Now they cannot be accessed directly from outside the class.

---

## 5. Interview Insights

### 💬 Common Questions
- Why is data hiding important?
- What are access modifiers and how do they affect encapsulation?
- How would you enforce valid state in a class?

### 🧠 Mental Model
```
Class Rectangle
 ├── Public Interface
 │    ├── draw()
 │    └── getArea()
 └── Private Data
      ├── width
      └── height
```

---

## 6. Quick Revision Sheet

- **Invalid state** = object holds bad data (e.g., negative width)
- **Data hiding** = restrict direct access to internal data
- Use **access modifiers** to control visibility:
  - `public` → exposed
  - `private` → hidden
- Keep **data private**, expose **behavior via methods**

---

## 7. What’s Next?

To fix the compilation error and allow controlled access to `width` and `height`, we’ll need to:
- Add **setter methods** to validate input
- Possibly add **getter methods** to read values safely

This leads us into the next core topic: **Encapsulation via Getters and Setters**.

## 8. Code from the video

### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    void draw();
    int getArea();
private:
    int width;
    int height;
};

#endif
```

### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle rectangle;

    rectangle.width = -1;
    
    return 0;
}
```