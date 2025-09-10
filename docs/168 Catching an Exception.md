# 168 Catching an Exception

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/EqWlu7XlDWw?si=CGjRL6XIBf3NpSJH"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Introduction

* When a function **throws** an exception, execution stops and control looks for a **catch** block.
* A `try` block wraps the code that may throw.
* A `catch` block handles the exception:

```cpp
try {
    Rectangle rect;
    rect.setWidth(-5);
}
catch (const invalid_argument& ex) {
    cout << ex.what() << endl;
}
```

* After the `catch`, execution continues normally → program doesn’t crash.

---

## 2. Three Methods of Handling

Inside `setWidth` and `main`, the video walks through **three approaches**:

### Method 1 – Hardcoded Message in Catch Block

```cpp
catch (const invalid_argument& ex) {
    cout << "Invalid width";
}
```

* Works only in **console apps**.
* Not reusable in GUI apps (no console to print to).
* Couples the error message to the catch block.

---

### Method 2 – Attach a Short Tag, Expand in Catch Block

```cpp
// In setWidth
throw invalid_argument("width");

// In main
catch (const invalid_argument& ex) {
    cout << "Invalid " << ex.what();
}
```

* Provides a **generic prefix** (`"Invalid"`) + error detail (`"width"`).
* Slightly better than Method 1, but still spreads responsibility between throw site and catch site.

---

### Method 3 – Descriptive Error Message in Exception Object ✅

```cpp
// In setWidth
throw invalid_argument("The width cannot be negative!");

// In main
catch (const invalid_argument& ex) {
    cout << ex.what() << endl;
}
```

* The **exception object itself carries the full message**.
* Catch block simply reads `ex.what()`.
* Most flexible: works in console, GUI, or even logging systems.
* Preferred best practice.

---

## 3. Unsafe Catch Block

Example from your snippet:

```cpp
catch (const invalid_argument& ex) {
    cout << ex.what();

    Rectangle rect;
    rect.setWidth(-1); // This throws again!
}
```

* **Problem:** Catch block itself throws another exception.
* No further catch available → program crashes.
* **Rule:** Code inside a `catch` must be **absolutely safe** (never throw).

---

## 4. End of Program Flow

```cpp
cout << "End of program";
```

* This line is placed **after the catch block**.
* Proves that once an exception is caught and handled, the program continues execution.
* Helps demonstrate graceful recovery instead of termination.

---

Got it ✅ — I’ll add a **new section** in your notes that clearly explains what the `what()` function does, how it works, and why it’s important in your example.  
I’ll keep the style consistent with your existing numbered sections.

---

## 5. Understanding the `what()` Function

* `what()` is a **member function** of the standard exception hierarchy in C++ (`std::exception` and its derived classes like `std::invalid_argument`).
* It returns a **C‑style string** (`const char*`) containing a human‑readable description of the error.
* Signature:
  ```cpp
  virtual const char* what() const noexcept;
  ```
  - **`const`** → does not modify the exception object.
  - **`noexcept`** → guaranteed not to throw another exception.
  - **`virtual`** → allows derived exception classes to override it with their own message.

---

### How it Works in Your Code

```cpp
catch (const invalid_argument& ex) {
    cout << ex.what() << endl;
}
```

* When you `throw invalid_argument("The width cannot be negative!")`,  
  the string `"The width cannot be negative!"` is stored inside the exception object.
* Inside the `catch` block, `ex.what()` retrieves that stored message.
* This allows the **throw site** to decide the message, and the **catch site** to simply display or log it — without hardcoding the text.

---

### Why It’s Useful

* **Separation of concerns** → The code that detects the error decides the message; the code that handles the error just uses it.
* **Reusability** → Works in console apps, GUI apps, logging systems, or even network error reporting.
* **Consistency** → All standard exceptions support `what()`, so you can handle different exception types in a uniform way.

---

### Example

```cpp
try {
    throw invalid_argument("Height must be positive");
}
catch (const invalid_argument& ex) {
    cout << "Error: " << ex.what() << endl;
}
```

**Output:**
```
Error: Height must be positive
```
---

## 6. Visual Mental Model

```
try {
   rect.setWidth(-5);   --> throws invalid_argument
   cout << "Done";      --> skipped
}
catch (const invalid_argument& ex) {
   cout << ex.what();   --> prints error message
}
cout << "End of program"; --> always executes after catch
```

---

## 7. Best Practices & Insights

* ✅ Use **Method 3** (store descriptive message in exception).
* ✅ Always catch exceptions by **const reference**.
* ✅ After handling, continue program gracefully.
* ⚠️ Never let a catch block throw unless rethrowing intentionally.

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
        //  Part of Method 3
        throw invalid_argument{"The width cannot be negative!"};

        // Part of Method 2
        // throw invalid_argument("width");
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
    catch (const invalid_argument& ex) {
        // Method 3
        cout << ex.what() << endl;

        // Method 2
        // cout << "Invalid" << ex.what();
        // This will print "Invalid <Message passed by function>" which threw the error.

        // Method 1
        // Print error message on consol
        // cout << "Invalid width";
    }

    // Unsafe catch block
    // catch (const invlaid_argument& ex) {
    //     cout << ex.what();

    //     Rectangle rect;
    //     rect.setWidth(-1);
    // }

    // This will be executed after the catch block.
    cout << End of program; 

    return 0;
}
```