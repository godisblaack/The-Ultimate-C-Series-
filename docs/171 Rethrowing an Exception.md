# 171 Rethrowing an Exception

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/hGBX2V75PUg?si=2xh7N2Enh_oPHFc2"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Context

* Sometimes functions use **resources** (e.g., files, memory, sockets).
* If an exception is thrown, and the function exits immediately:

  * The resource (file, memory, etc.) may remain **unreleased**.
* We need a way to:

  1. **Handle cleanup locally**.
  2. **Still propagate the error** to be handled higher up.

---

## 2. Solution → Catch + Rethrow

* Wrap resource-using code in a `try/catch`.
* In the `catch`:

  1. Perform **local cleanup** (e.g., close the file).
  2. Use `throw;` (without arguments) to rethrow the *same* exception.
* Control goes to the next matching `catch` **higher in the call stack**.

---

## 3. Code Walkthrough

```cpp
void createRectangle() {
    try {
        Rectangle rect;
        rect.setWidth(-1);  // throws invalid_argument
    }
    catch (const invalid_argument& ex) {
        cout << "Close the file" << endl;  // local cleanup

        throw;  // rethrow to be handled in main
    }
}

void doWork() {
    createRectangle();
}

int main() {
    try {
        doWork();
    }
    catch (const exception& ex) {
        cout << ex.what() << endl;  // global handler
    }
}
```

### Execution Flow

1. `setWidth(-1)` → throws `invalid_argument`.
2. `createRectangle` catches it → performs cleanup (`Close the file`).
3. `throw;` rethrows the exception.
4. Exception propagates to `main`.
5. `main` prints:

   ```
   Close the file
   The width cannot be negative!
   ```

---

## 4. Visual Mental Model

```
createRectangle()
 └── try { setWidth(-1) }
     └── catch { cleanup → rethrow }
doWork()
main()
 └── catches rethrown exception
```

---

## 5. Best Practices & Insights

* ✅ Always release resources before rethrowing.
* ✅ Use `throw;` (no arguments). Writing `throw ex;` would **slice/copy** the exception.
* ✅ Rethrow when:

  * You cannot fully handle the exception.
  * Cleanup must happen locally, but decision-making belongs higher up.
* ⚠️ Don’t rethrow blindly everywhere — only where it makes sense.

---

## 6. Interview Insights

* Q: *What’s the difference between `throw;` and `throw ex;`?*

  * A: `throw;` rethrows the original exception (preserves type & stack info).
    `throw ex;` creates a copy → may cause **object slicing** and lose details.

* Q: *Why catch locally if you’re just going to rethrow?*

  * A: To perform **cleanup** (e.g., closing files, releasing memory) before propagating the error.

---

## 7. Quick Revision Sheet

* Local `catch` → cleanup resources.
* Use `throw;` → rethrow the original exception.
* Top-level `catch` (e.g., in `main`) still receives the error.
* Prefer `throw;` over `throw ex;` to avoid slicing.

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
    try {
        Rectangle rect;
        rect.setWidth(-1);
    }
    catch (const invalid_argument& ex) {
        cout << "Close the file" << endl;

        throw // Rethrows the error to main function
    }
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