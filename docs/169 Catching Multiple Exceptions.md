# 169 Catching Multiple Exceptions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/0FjafTJ5r08?si=RkUnTrDVUDKsmKEl"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Introduction

* Functions may throw **different exception types** depending on the error.
* Example:

  * `invalid_argument` → width is negative.
  * `out_of_range` → width is too large (>100).
* Each exception type can be caught with its own `catch` block.

```cpp
try {
    rect.setWidth(width);
}
catch (const invalid_argument& ex) {
    cout << ex.what() << endl;
}
catch (const out_of_range& ex) {
    cout << ex.what() << endl;
}
```

* Each `catch` block is **independent**, like separate functions.

---

## 2. Simplifying with a Common Base Class

* Both `invalid_argument` and `out_of_range` derive from **`logic_error`**.
* Thanks to inheritance, we can replace two catches with **one**:

```cpp
catch (const logic_error& ex) {
    cout << ex.what() << endl;
}
```

* Any `logic_error` subtype (e.g., invalid\_argument, out\_of\_range) will be caught here.
* This avoids repeating identical handling code.

---

## 3. Ordering Matters: Specific → Generic

* If you want **different handling** for `out_of_range` but generic handling for other `logic_error`s:

❌ Wrong order (generic first):

```cpp
catch (const logic_error& ex) {  // catches everything, including out_of_range
    cout << ex.what();
}
catch (const out_of_range& ex) { // never reached
    cout << "Special handling: " << ex.what();
}
```

✅ Correct order (specific first):

```cpp
catch (const out_of_range& ex) {
    cout << "Special handling: " << ex.what();
}
catch (const logic_error& ex) {
    cout << ex.what();
}
```

* **Rule:** Always order catch blocks from **most specific → most generic**.

---

## 4. Visual Mental Model

```
Exception hierarchy:
exception
   └── logic_error
         ├── invalid_argument
         └── out_of_range
```

* If `width = -5` → `invalid_argument` thrown → caught by `logic_error` block.
* If `width = 200` → `out_of_range` thrown →

  * If specific block exists → handled there.
  * Otherwise → handled by generic `logic_error`.

---

## 5. Best Practices & Insights

* ✅ Use **common base classes** (`logic_error`, `runtime_error`) to simplify code.
* ✅ Always order catch blocks **specific before generic**.
* ✅ Keep exception messages descriptive at the throw site.
* ⚠️ Avoid duplicating handling code across multiple catches if behavior is identical.

---

## 6. Interview Insights

* Q: *What happens if you catch a generic base type (`logic_error`) before a derived type (`out_of_range`)?*

  * A: The generic block will always handle it, and the specific one will be ignored.

* Q: *When should you consolidate vs separate catch blocks?*

  * A: Consolidate if handling logic is identical, separate if behavior differs.

---

## 7. Quick Revision Sheet

* **Multiple catches**: one per exception type.
* **Consolidation**: catch by common base (`logic_error`).
* **Order rule**: specific → generic.
* **Inheritance principle**: base catch can handle derived exceptions.

---

## 8. Code from the video

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
        throw invalid_argument{"The width cannot be negative!"};
    }

    if (width > 100) {
        throw out_of_range{"The width cannot be greater than 100!"};
    }

    this->width = width;
}

void Rectangle::setHeight(int height) {
    if (height < 0) {
        throw invalid_argument{"height"};
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
    try {
        cout << "Width: ";
        int width;
        cin >> width;
        
        Rectangle rect;
        rect.setWidth(width);
    }

    // catch (const invalid_argument& ex) {
    //     cout << ex.what() << endl;
    // }

    // catch (const out_of_range& ex) {
    //     cout << ex.what() << endl;
    // }

    // Replacement
    catch (const logic_error& ex) {
        cout << ex.what();
    }

    return 0;
}
```

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    try {
        cout << "Width: ";
        int width;
        cin >> width;
        
        Rectangle rect;
        rect.setWidth(width);
    }

    // Order specific to generic
    catch (const out_of_range& ex) {
        cout << ex.what() << endl;
    }

    catch (const logic_error& ex) {
        cout << ex.what();
    }

    return 0;
}
```