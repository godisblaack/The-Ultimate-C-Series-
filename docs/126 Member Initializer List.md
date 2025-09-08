# 126 Member Initializer List

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/qLIC5-guAUw?si=49TE5KZbitbcXgNS" 
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Member Initializer Lists Matter

### 🧠 The Usual Way
- In earlier examples, we initialized members **inside the constructor body**:
  ```cpp
  Rectangle::Rectangle(int w, int h) {
      setWidth(w);
      setHeight(h);
  }
  ```
- This works, but the compiler:
  1. **First** default‑constructs the members.
  2. **Then** assigns new values in the constructor body.

---

### ⚡ The More Efficient Way
- **Member Initializer List**: Initialize members **directly** at creation time.
- Syntax:
  ```cpp
  Rectangle::Rectangle(int w, int h) : width{w}, height{h} {
      // constructor body
  }
  ```
- Here, members are **constructed and initialized in one step**.

---

## 2. How It Works

- Placed **after the parameter list** and before the constructor body.
- Each member is initialized in the order they are **declared** in the class, not in the order in the list.
- Curly braces `{}` are preferred in modern C++ for **uniform initialization**.
- No ambiguity:  
  - Left side → member variable  
  - Inside braces → constructor parameter

---

## 3. Efficiency Difference

| Approach | Steps Taken by Compiler | Efficiency |
|----------|------------------------|------------|
| **Body Initialization** | 1. Default-construct member<br>2. Assign new value | Less efficient |
| **Member Initializer List** | 1. Construct member with given value | More efficient |

---

## 4. Limitations

- **No validation** possible in the initializer list.
- Cannot call setters or perform complex logic.
- Best for:
  - Simple assignments
  - Initializing `const` members
  - Initializing reference members

---

## 5. Mixing Approaches

- You can **combine**:
  - Use initializer list for simple members.
  - Use constructor body for members needing validation.
- In our `Rectangle` example, we **don’t** use initializer list because we want to reuse setters for validation.

---

## 6. Updated Rectangle Example

### **Using Member Initializer List**
```cpp
Rectangle::Rectangle(int width, int height)
    : width{width}, height{height} {
    cout << "Constructing a Rectangle" << endl;
}
```
✅ Efficient  
❌ No validation

---

### **Using Setters in Constructor**
```cpp
Rectangle::Rectangle(int width, int height) {
    cout << "Constructing a Rectangle" << endl;
    setWidth(width);   // Validates
    setHeight(height); // Validates
}
```
✅ Validation  
❌ Slightly less efficient

---

## 7. Interview Insights

### 💬 Common Questions
- What is a **member initializer list** and why is it more efficient?
- When must you use an initializer list?  
  *(Hint: `const` members, references, base class constructors)*
- Why might you still prefer body initialization in some cases?
- In what order are members initialized?

---

## 8. Mental Model

```
Constructor Execution
 ├── Member Initializer List (if present)
 │     ├── Directly constructs members with given values
 │     └── Happens before constructor body
 └── Constructor Body
       ├── Runs after all members are initialized
       └── Can perform validation, logic, side effects
```

---

## 9. Quick Revision Sheet

- **Syntax**: `ClassName(params) : member1{val1}, member2{val2} { ... }`
- **Order**: Members initialized in **declaration order**, not list order.
- **Efficiency**: Skips default construction + assignment.
- **Use Cases**: `const` members, references, base classes.
- **Limitation**: No validation or complex logic.

---

## 10. Best Practices

- Prefer initializer lists for **simple, direct initialization**.
- Always use them for `const` and reference members.
- Keep member declaration order consistent with initializer list order to avoid confusion.
- Use constructor body when **validation** or **logic** is required.
- In mixed cases, initialize simple members in the list, validate others in the body.

---

## 11. Code from the video

### Rectangle.cpp

#### Using Member initializer list

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

Rectangle::Rectangle(int width, int height) : width{width}, height{height} {
    cout << "Constructing a Rectangle" << endl;
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
```

#### Using setter and getter for initialization

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