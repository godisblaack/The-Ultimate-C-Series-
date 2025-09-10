# 152 Inheritance

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/WbNcLNFJTeY?si=ITyqbpePTx-H-qj5"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Inheritance Matters

### 🔄 Code Reuse
- **Inheritance** allows a class to acquire properties and methods of another class.
- Avoids **repetition** — common features are implemented once in a **base (parent)** class.
- Derived (child) classes **reuse** and **extend** base class functionality.

### 🖥 GUI Framework Example
- Common features for UI elements: **size**, **position**, **enabled/disabled state**.
- Implement once in `Widget` → reuse in `TextBox`, `CheckBox`, etc.

---

## 2. Base and Derived Classes

### 📦 Base Class (`Widget`)
- Holds **shared attributes** and **methods**.
- Example: `enabled` flag with `enable()`, `disable()`, `isEnabled()`.

### 🧩 Derived Class (`TextBox`)
- Inherits from `Widget`.
- Adds **specific attributes** (`value`) and **methods** (`getValue()`, `setValue()`).

---

## 3. Public vs Private Inheritance

### 🌐 Public Inheritance
- **Public members** of base → remain **public** in derived.
- **Protected members** of base → remain **protected** in derived.
- **Private members** are **not** directly accessible in derived.
- Used **99% of the time** — preserves base class interface.

### 🔒 Private Inheritance
- **Public** and **protected** members of base → become **private** in derived.
- Accessible **inside** derived class, but **not** outside.
- Rarely used — typically for **implementation detail reuse**.

---

## 4. Implementing the Example

### Widget.h
```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    void enable();
    void disable();
    bool isEnabled() const;

private:
    bool enabled{};
};

#endif
```

### Widget.cpp
```cpp
#include "Widget.h"

void Widget::enable() { enabled = true; }
void Widget::disable() { enabled = false; }
bool Widget::isEnabled() const { return enabled; }
```

---

### Public Inheritance — TextBox.h
```cpp
class TextBox : public Widget {
public:
    TextBox() = default;
    explicit TextBox(const std::string& value);

    std::string getValue() const;
    void setValue(const std::string& value);

private:
    std::string value;
};
```

### Public Inheritance — main.cpp
```cpp
TextBox box;
box.disable();              // Inherited from Widget
std::cout << box.isEnabled();
```

---

### Private Inheritance — TextBox.h
```cpp
class TextBox : private Widget {
public:
    TextBox() = default;
    explicit TextBox(const std::string& value);

    std::string getValue() const;
    void setValue(const std::string& value);

private:
    std::string value;
};
```

### Private Inheritance — main.cpp
```cpp
TextBox box;
box.disable(); // ❌ Compilation error — now private
```

---

## 5. Exercise — Shapes

### Shape.h
```cpp
#ifndef ADVANCED_SHAPE_H
#define ADVANCED_SHAPE_H

#include <string>

class Shape {
public:
    std::string getBackground() const;
    void setBackground(const std::string& background);

private:
    std::string background;
};

#endif
```

### Shape.cpp
```cpp
#include "Shape.h"

std::string Shape::getBackground() const { return background; }
void Shape::setBackground(const std::string& background) {
    this->background = background;
}
```

### Rectangle.h
```cpp
class Rectangle : public Shape {};
```

### Circle.h
```cpp
class Circle : public Shape {};
```

---

## 6. Interview Insights

### 💬 Common Questions
- Difference between **public** and **private** inheritance?
- Why are base class **private members** not accessible in derived classes?
- When would you use **private inheritance**?
- How does inheritance help in **code reuse**?

### 🧠 Mental Model
```
Widget (Base)
 ├── enable()
 ├── disable()
 └── isEnabled()
      ↑
      │
TextBox (Derived)
 ├── getValue()
 └── setValue()
```

---

## 7. Quick Revision Sheet

- **Inheritance** = reuse + extend.
- **Public inheritance** preserves interface — default for “is-a” relationships.
- **Private inheritance** hides base interface — “implemented-in-terms-of”.
- Base class **private members** are inaccessible directly in derived.
- Use **const** for getters that don’t modify state.

---

## 8. Best Practices

- Use **public inheritance** for “is-a” relationships.
- Keep base class **data members private** — expose via public/protected methods.
- Initialize members in **constructor initializer lists**.
- Avoid `using namespace std;` in headers.
- Mark **non-mutating methods** as `const`.

---

## 9. Common Pitfalls

- Forgetting to initialize base class members.
- Using private inheritance when public is intended — breaks interface.
- Accessing base class private members directly — not allowed.
- Forgetting `const` correctness in getters.

---

## 10. Code from the video

### Inheriting publicly

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    void enable();
    void disable();
    bool isEnabled() const;

private:
    bool enabled;
};

#endif
```

#### Widget.cpp

```cpp
#include "Widget.h"

void Widget::enable() {
    enable = true;
}

void Widget::disable() {
    enable = false;
}

bool Widget::isEnabled() const {
    return enabled;
}
```


#### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : public Widget {
public:
    TextBox() = default;
    explicit TextBox(const string& value);

    string getValue() const;
    void setValue(const string& value);

private:
    string value;
};

#endif
```

#### TextBox.cpp

```cpp
#include "TextBox.h"

#include <iostream>

TextBox::TextBox(const string& value) : value{value} {}

string TextBox::getValue() const {
    return value;
}

void TextBox::setValue(const string& value) {
    this->value = value;
}
```

#### main.cpp

```cpp
#include "TextBox.h"

#include <iostream>

using namespace std;

int main() {
    TextBox box;
    
    box.disable();

    cout << box.isEnabled();

    return 0;
}
```

### Inheriting privately

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    void enable();
    void disable();
    bool isEnabled() const;

private:
    bool enabled;
};

#endif
```

#### Widget.cpp

```cpp
#include "Widget.h"

void Widget::enable() {
    enable = true;
}

void Widget::disable() {
    enable = false;
}

bool Widget::isEnabled() const {
    return enabled;
}
```

#### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : private Widget {
public:
    TextBox() = default;
    explicit TextBox(const string& value);

    string getValue() const;
    void setValue(const string& value);

private:
    string value;
};

#endif
```

#### TextBox.cpp

```cpp
#include "TextBox.h"

#include <iostream>

TextBox::TextBox(const string& value) : value{value} {}

string TextBox::getValue() const {
    this->enable();

    return value;
}

void TextBox::setValue(const string& value) {
    this->value = value;
}
```

#### main.cpp

```cpp
#include "TextBox.h"

#include <iostream>

using namespace std;

int main() {
    TextBox box;

    // Compilation error
    box.disable(); 

    cout << box.isEnabled();

    return 0;
}
```

## 11. Exercise

### Shape.h

```cpp
#ifndef ADVANCED_SHAPE_H
#define ADVANCED_SHAPE_H

class Shape {
public:
    string getBackground();
    void setBackground();

private:
    string background;
};

#endif
```

### Shape.cpp

```cpp
#include "Shape.h"

string Shape::getBackground() {
    return background;
}

void Shape::setBackground(const string& background) {
    this->background = background;
}
```

### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle : public Shape {};

#endif
```

### Circle.h

```cpp
#ifndef ADVANCED_CIRCLE_H
#define ADVANCED_CIRCLE_H

class Circle : public Shape {};

#endif
```