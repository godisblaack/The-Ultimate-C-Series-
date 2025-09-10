# 167 Throwing an Exception

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/ttpI1v4nfI8?si=auuPR0E200i3JFUI"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Introduction

* **Throwing an exception** = creating an exception object and sending it outside the current function.
* Example in `Rectangle::setWidth`:

```cpp
void Rectangle::setWidth(int width) {
    if (width < 0) {
        throw invalid_argument("width"); // creates and throws an exception object
    }

    this->width = width;
}
```
* We can also use curly braces:
```cpp
throw invalid_argument{"width"};
```

* Once `throw` is executed:

  * The rest of the function is **skipped**.
  * Control is **transferred out** to the nearest matching `catch` block.

* You can use **parentheses `()`** or **braces `{}`** when constructing the exception object. Both are valid — it’s style.

---

## 2. Why Not Just Print Errors?

* Printing (e.g., `cout << "Width cannot be negative";`) only works in **console apps**.

```cpp
void Rectangle::setWidth(int width) {
    if (width < 0) {
        // This method only works for console application
        cout << "The width cannot be negative" << endl;

        // throw invalid_argument("width");
    }

    this->width = width;
}
```
* For **GUI apps**, there may be no console → error messages will never be seen.
* Throwing exceptions **decouples error detection from error handling**:

  * Function detects the problem → *throws exception*.
  * Caller decides *how to handle* (console message, dialog box, logging, etc.).

---

## 3. Visual Mental Model

```
setWidth(-5)  
   ↓
[ function detects invalid value ]  
   ↓ throw invalid_argument("width")  
[ function ends abruptly ]  
   ↓  
Somewhere else: catch block decides action  
   - Console → print error  
   - GUI → show dialog  
   - Server → write log
```

---

## 4. Best Practices & Insights

* **Don’t handle errors where they occur unless it makes sense.**
  → Responsibility: detect problem and throw.
* Use **specific exception types** (`invalid_argument`, `out_of_range`) instead of `exception` or generic types.
* Avoid mixing concerns:

  * Method = validation + throw.
  * Caller = decides presentation (UI/logging).
* Throw exceptions for **exceptional conditions only** (not for normal control flow).

---

## 5. Interview Insights

**Q:** What happens after a `throw` in C++?
**A:** Control immediately leaves the function, skipping remaining code, and looks for a matching `catch` block up the call stack.

**Q:** Why is throwing better than printing/logging errors inside the function?
**A:** Printing ties the function to a console app. Throwing allows **flexible handling** (console, GUI, logging, retry) at the caller level.

**Q:** Can you throw using braces `{}` instead of parentheses `()`?
**A:** Yes — both work because it’s just object construction syntax.

---

## 6. Quick Revision

* Throwing = creating + sending an exception object outside the function.
* Function execution stops immediately.
* Caller decides what to do with the exception.
* More flexible than printing/logging inside functions.

---

👉 Next up: we’ll learn how to **catch exceptions** and handle them.

---

## 7. Code from the video

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
        // This method only works for console application
        // cout << "The width cannot be negative" << endl;

        throw invalid_argument{"width"};
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