# 124 Getters and Setters

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Bte1oGc0DKg?si=3vS9yrYj-3L6Dr7I"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Encapsulation Matters

### 🧠 Object State
- An object’s **state** is defined by its internal data.
- If we allow invalid data (e.g., `width = -1`), the object enters a **bad state**.
- This leads to **bugs**, **unexpected behavior**, and **logic errors**.

### 🔒 Data Hiding Recap
- We made `width` and `height` private to prevent direct access.
- Now we expose **controlled access** via **getters** and **setters**.

---

## 2. Getters and Setters

### 📥 Setter (Mutator)
- Sets the value of a private member.
- Validates input before assignment.
- Example:
  ```cpp
  void Rectangle::setWidth(int width) {
      if (width < 0) throw invalid_argument("width");
      this->width = width;
  }
  ```

### 📤 Getter (Accessor)
- Returns the value of a private member.
- Example:
  ```cpp
  int Rectangle::getWidth() {
      return width;
  }
  ```

---

## 3. Using `this` Pointer

### 🔍 Why Use `this`?
- Resolves ambiguity when parameter name matches member name.
- `this->width` refers to the member variable. It is as same as `(*this).width`.
- Alternatives:
  - Rename parameter (`int newWidth`)
  - Prefix member (`m_width`)
  - Use scope resolution (`Rectangle::width`) ← less preferred inside methods

---

## 4. Exception Handling

### 🚨 Input Validation
- Use `throw invalid_argument("parameter")` to reject bad input.
- Prevents object from entering invalid state.
- The exception `invalid_argument()` is defined in `stdexcept` header file. Mosh forgot to include it in the `Rectangle.cpp`. Include it as `#include <stdexcept>`.
- Example:
  ```cpp
  if (height < 0) throw invalid_argument("height");
  ```

---

## 5. Updated Rectangle Class

### Rectangle.h
```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    void draw();
    int getArea();

    int getWidth();
    int getHeight() const;

    void setWidth(int width);
    void setHeight(int height);

private:
    int width;
    int height;
};

#endif
```

### Rectangle.cpp
```cpp
#include "Rectangle.h"
#include <iostream>
#include <stdexcept>

using namespace std;

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

### main.cpp
```cpp
#include "Rectangle.h"
#include <iostream>

using namespace std;

int main() {
    Rectangle rectangle;
    rectangle.setWidth(-1); // Throws exception

    return 0;
}
```

---

## 6. Exercise
### Create a textBox Class, with one string variable. Define a getter and a setter for it.

### TextBox.h
```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include <string>
using namespace std;

class TextBox {
public:
    string getValue() const;
    void setValue(const string& value);

private:
    string value;
};

#endif
```

### TextBox.cpp
```cpp
#include "TextBox.h"

string TextBox::getValue() const {
    return value;
}

void TextBox::setValue(const string& value) {
    this->value = value;
}
```

### main.cpp
```cpp
#include "TextBox.h"
#include <iostream>

using namespace std;

int main() {
    TextBox box;
    box.setValue("Hello World");
    cout << box.getValue();

    return 0;
}
```

---

## 7. Interview Insights

### 💬 Common Questions
- Why use getters/setters instead of public members?
- How do you validate input in setters?
- What’s the role of `this` pointer?
- How do exceptions help enforce invariants?

### 🧠 Mental Model
```
Encapsulation
 ├── Private Data
 │    ├── width
 │    └── height
 └── Public Interface
      ├── getWidth()
      ├── setWidth()
      ├── getHeight()
      └── setHeight()
```

---

## 8. Quick Revision Sheet

- Use **getters/setters** to control access.
- Validate input in setters to protect object state.
- Use `this->member` to resolve naming conflicts.
- Throw `invalid_argument` for bad input.
- Prefer `const string&` for string parameters (performance + safety).

---

## 9. Best Practices

- Keep data members private.
- Validate all external input.
- Use `const` on getters when appropriate.
- Avoid `using namespace std;` in headers.
- Be consistent with naming and access patterns.

## 10. Code from the video

### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle {
public:
    void draw();
    int getArea();

    // Getter (accessor)
    int getWidth();
    int getHeight() const;

    // Setter (mutator)
    void setWidth(int width);
    void setHeight(int height);

private:
    int width;
    int height;
};

#endif
```

### Rectangle.cpp

```cpp
#include "Rectangle.h"

#include <iostream>
#include <stdexcept>

using namespace std;

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

    this->width = width; // (*this).width = width;
}

int Rectangle::getHeight() const {
    return height;
}

void Rectangle::setHeight(int height) {
    if (height < 0) {
        throw invalid_argument("height");
    }

    this->height = height; // Rectangle::height = height;
}
```

### main.cpp

```cpp
#include "Rectangle.h"

#include <iostream>

using namespace std;

int main() {
    Rectangle rectangle;
    rectangle.setWidth(-1);

    return 0;
}
```

## 11. Exercise

### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include <string>

using namespace std;

class TextBox {
public:
    string getValue() const;
    void setValue(const string& value);
private:
    string value;
};

#endif
```

### TextBox.cpp

```cpp
#include "TextBox.h"

#include <iostream>

string TextBox::getValue() const {
    return value;
}

void TextBox::setValue(const string& value) {
    this->value = value;
}
```

### main.cpp

```cpp
#include "TextBox.h"

#include <iostream>

using namespace std;

int main() {
    TextBox box;
    
    box.setValue("Hello World");

    cout << box.getValue();

    return 0;
}
```