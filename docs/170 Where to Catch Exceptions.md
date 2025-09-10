# 170 Where to Catch Exceptions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Q3R7-OPGy-Y?si=xLXX1T96EuJ5P8SE"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Call Stack Refresher

* **Call stack** = runtime’s record of function calls in reverse order (top = most recent).
* Example (program flow):

  ```
  main → doWork → createRectangle
  ```
* If an exception is thrown in `createRectangle`:

  * Runtime checks for a `catch` **inside `createRectangle`**.
  * If none, function is popped off the call stack.
  * Control goes back to `doWork` → looks for a `catch`.
  * If none, runtime pops again → goes to `main`.
  * If still no `catch` → program terminates.

---

## 2. Where to Catch Exceptions?

* **Anywhere in the call stack** where it makes sense.
* Options:

  1. **Handle locally** (inside `createRectangle`)

     * Example: If user enters invalid width, replace with default `0` or `1`.
     * Advantage: keeps error handling close to where it happened.
     * Disadvantage: hides the error from higher-level logic.
  2. **Propagate upwards** (let caller handle it).

     * Caller decides how to respond.
     * Example: `main` catches all exceptions and shows/logs an error.

---

## 3. Typical Strategy in Real Applications

* Wrap **top-level code** (e.g., `main`) in a `try/catch`.
* Catch the base type `exception` (covers all standard exceptions).
* Ensures program **does not crash silently**.

```cpp
int main() {
    try {
        doWork();
    }
    catch (const exception& ex) {
        cout << "Error: " << ex.what() << endl;
        // In real apps: log error to file or DB
    }
}
```

---

## 4. Code Walkthrough

```cpp
void createRectangle() {
    Rectangle rect;
    rect.setWidth(-1);   // throws invalid_argument
}

void doWork() {
    createRectangle();   // no catch → propagates
}

int main() {
    try {
        doWork();        // propagates here
    }
    catch (const exception& ex) {
        cout << ex.what();  // handled safely
    }
}
```

### Execution Flow

1. `rect.setWidth(-1)` → throws `invalid_argument`.
2. No catch in `createRectangle` → function exits.
3. No catch in `doWork` → function exits.
4. `main` has `catch (const exception& ex)` → error handled.

---

## 5. Debugging with Call Stack

* Debuggers (VSCode, CLion, Visual Studio, etc.) show the **call stack panel**.
* Helps trace *where the exception originated* and *how it propagated*.
* Useful for diagnosing *deeply nested function calls*.

---

## 6. Best Practices

* ✅ **Catch only where you can handle** the exception meaningfully.
* ✅ Use **base exception types** (`exception`, `logic_error`, `runtime_error`) at higher levels.
* ✅ Always log unhandled exceptions at the top level.
* ⚠️ Avoid “swallowing” exceptions (catching without action).

---

## 7. Interview Insights

* Q: *What happens if an exception is not caught anywhere in the call stack?*

  * A: The runtime terminates the program.

* Q: *Why is it common to catch exceptions in `main`?*

  * A: To prevent crashes, show friendly error messages, and log issues.

---

## 8. Quick Revision Sheet

* Exception propagates **up the call stack** until caught.
* **Handle locally** if you can recover; otherwise **propagate upwards**.
* Wrap **top-level code** in `try/catch` for safety.
* Use **debugger call stack** to trace errors.

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

void createRectangle() {
    Rectangle rect;
    rect.setWidth(-1);
}

void doWork() {
    createReactangle();
}

int main() {
    try {
        doWork();
    }
    catch (const exception& ex) {
        cout << ex.what();
    }

    return 0;
}
```