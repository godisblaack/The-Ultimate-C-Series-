# 129 Constructor Delegation

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Ln_yWfEX0Mk?si=vjFxvSng_TScLcPt" 
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Constructor Delegation Matters

### 🧠 The Problem
- Multiple constructors often share **common initialization logic**.
- Without delegation, you **duplicate code**:
  - If logic changes, you must update it in multiple places.
  - Increases risk of **inconsistency** and **bugs**.

---

## 2. What is Constructor Delegation?

- **Constructor Delegation**: One constructor calls another constructor **in the same class** to reuse its initialization logic.
- Introduced in **C++11**.
- Syntax:
  ```cpp
  ClassName(params...) : ClassName(otherParams...) {
      // additional initialization
  }
  ```

---

## 3. Example – Rectangle with Color

### Without Delegation (Code Duplication)
```cpp
Rectangle::Rectangle(int width, int height, const string& color) {
    setWidth(width);
    setHeight(height);
    this->color = color;
}
```
- Width/height initialization logic is **copied** from the two‑parameter constructor.

---

### With Delegation (Cleaner)
```cpp
Rectangle::Rectangle(int width, int height, const string& color)
    : Rectangle(width, height) { // delegates to 2‑param constructor
    cout << "Constructing a Rectangle with color" << endl;
    this->color = color;
}
```
✅ Reuses validation logic in `setWidth` and `setHeight`  
✅ Avoids duplication  
✅ Easier to maintain

---

## 4. Execution Flow

For:
```cpp
Rectangle r{10, 20, "blue"};
```
1. **Three‑param constructor** is called.
2. Delegates to **two‑param constructor**:
   - Prints `"Constructing a Rectangle"`.
   - Validates and sets width/height.
3. Returns to **three‑param constructor body**:
   - Prints `"Constructing a Rectangle with color"`.
   - Sets `color`.

---

## 5. Best Practices for Delegation

- Use delegation to **centralize shared initialization**.
- Keep delegated constructors **focused** on a specific initialization task.
- Avoid **deep delegation chains** — too much hopping between constructors hurts readability.
- Still validate inputs in the **primary constructor** that handles the logic.

---

## 6. Updated Rectangle Class

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
    : Rectangle(width, height) { // Delegation
    cout << "Constructing a Rectangle with color" << endl;
    this->color = color;
}

void Rectangle::draw() {
    cout << "Drawing a rectangle" << endl;
    cout << "Dimensions: " << width << ", " << height << endl;
}

int Rectangle::getArea() {
    return width * height;
}

int Rectangle::getWidth() {
    return width;
}

void Rectangle::setWidth(int width) {
    if (width < 0) throw invalid_argument("width");
    this->width = width;
}

int Rectangle::getHeight() const {
    return height;
}

void Rectangle::setHeight(int height) {
    if (height < 0) throw invalid_argument("height");
    this->height = height;
}
```

---

## 7. Mental Model

```
Constructor Delegation
 ├── Delegating Constructor
 │     ├── Calls another constructor in same class
 │     └── Reuses its initialization logic
 └── Adds its own extra initialization
```

---

## 8. Quick Revision Sheet

- **Delegating constructor**: Calls another constructor in the same class.
- **Syntax**: `: ClassName(args...)` after parameter list.
- **Benefits**: Removes duplication, centralizes logic.
- **Caution**: Avoid long delegation chains.
- **Introduced in**: C++11.

---

## 9. Interview Insights

💬 **Possible Questions**
- What is constructor delegation and why use it?
- How does it differ from calling a helper function?
- What happens if both constructors have validation logic?
- Can you delegate to multiple constructors?

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
    Rectangle rectangle{10, 20, "blue"};

    return 0;
}
```