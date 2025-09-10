# 161 Abstract Classes

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/e0raXVS0Kj8?si=CejDvQGC3IdRjpIZ"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Setup

Currently, `Widget` has a `draw()` implementation:

```cpp
void Widget::draw() const {
    cout << "Drawing a Widget" << endl;
}
```

But in a real framework, this makes no sense:

* How do you “draw a generic widget”?
* Each widget type (`TextBox`, `CheckBox`) must implement its own drawing algorithm.

👉 The base class method has no meaningful implementation.

---

## 2. Pure Virtual Method

Solution: declare `draw()` as a **pure virtual function**.

```cpp
class Widget {
public:
    virtual ~Widget() = default;

    // Pure virtual
    virtual void draw() const = 0;
};
```

Changes:

* Assigning `= 0` → makes `draw()` **pure virtual**.
* Remove its definition from `Widget.cpp`.

---

## 3. Abstract Classes

* Any class with ≥1 pure virtual method is **abstract**.
* Abstract classes **cannot be instantiated**:

```cpp
Widget w;   // ❌ Compilation error
```

* But they can be used as:

  * **References**
  * **Pointers**

This allows polymorphism:

```cpp
void showWidget(Widget& widget) {
    widget.draw(); // Works with any derived
}
```

---

## 4. Inheriting from Abstract Class

* Derived classes **must override** pure virtual functions.
* If they don’t, the derived class remains **abstract** too.

Example:

```cpp
class TextBox : public Widget {
public:
    void draw() const override; // ✅ required
};
```

If `TextBox` doesn’t override `draw()`, it becomes abstract and you **can’t instantiate it**.

---

## 5. Visual Mental Model

```
Widget (Abstract)
 ├─ virtual ~Widget()
 ├─ enable(), disable(), ...
 └─ pure virtual draw() = 0   <-- no body, must be overridden

Derived classes:
   TextBox   --> must override draw()
   CheckBox  --> must override draw()
   (otherwise they also remain abstract)
```

---

## 6. Best Practices & Insights

### ✅ When to use pure virtual functions

* When base class has no meaningful implementation.
* To enforce a **contract** for derived classes.

### ✅ Abstract classes

* Serve as **interfaces with partial implementation**.
* Cannot be instantiated.
* But can be **used as references/pointers**.

### ✅ Syntax tips

* Declare pure virtual:

  ```cpp
  virtual void f() const = 0;
  ```
* Still provide destructor (virtual!):

  ```cpp
  virtual ~Base() = default;
  ```

### ⚠️ Gotchas

* Don’t forget to override → derived remains abstract.
* Avoid providing accidental definitions for pure virtuals unless you intend to.
* Abstract classes can still have non-virtual methods (shared code).

---

## 7. Interview Insights

**Q:** What is an abstract class in C++?
**A:** A class with ≥1 pure virtual function; cannot be instantiated; designed to be inherited.

**Q:** Difference between virtual and pure virtual?
**A:**

* `virtual` → function may be overridden, but has a default implementation.
* `= 0` (pure virtual) → must be overridden; no default implementation.

**Q:** Can an abstract class have constructors?
**A:** Yes, but you cannot directly instantiate it. Constructors are used by derived classes.

**Q:** Can a pure virtual function have a body?
**A:** Yes (unusual but allowed) — but derived classes must still override it.

**Q:** Why are abstract classes useful?
**A:** They define an **interface/contract** for all derived types while allowing shared base functionality.

---

## 8. Quick Revision Sheet

* `virtual void f() = 0;` → pure virtual → class becomes **abstract**.
* Abstract classes:

  * **Cannot be instantiated**.
  * **Can be used via pointers/references**.
* Derived must override pure virtuals → otherwise still abstract.
* Use abstract classes to enforce **“must implement” behavior**.
* Always give abstract bases a **virtual destructor**.

---

## 9. Code from the video

### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

// Abstract
class Widget {
public:
    virtual ~Widget() = default:

    void enable();
    void disable();
    bool isEnabled() const;

    // Pure virtual method
    virtual void draw() const = 0;

private:
    bool enabled;
    int width;
};

#endif
```

### Widget.cpp

```cpp
#include "Widget.h"

#include <iostream>

using namespace std;

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

### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : public Widget {
public:
    ~TextBox();

    string getValue() const;
    void setValue(const string& value);

    void draw() const override;

private:
    string value;
};

#endif
```

### TextBox.cpp

```cpp
#include "TextBox.h"

#include <iostream>

using namespace std;

TextBox::~TextBox() {
    cout << "Destructing a Widget" << endl;
}

string TextBox::getValue() const {
    return value;
}

void TextBox::setValue(const string& value) {
    this->value = value;
}

void TextBox::draw() const {
    cout << "Drawing a TextBox" << endl;
}
```

### CheckBox.h

```cpp
#ifndef ADVANCED_CHECKBOX_H
#define ADVANCED_CHECKBOX_H

class CheckBox : public Widget {
public:
    ~CheckBox();

    void draw() const override;
};

#endif
```

### CheckBox.cpp

```cpp
#include "CheckBox.h"

#include <iostream>

using namespace std;

CheckBox::~CheckBox() {
    cout << "Destructing a CheckBox" << endl;
}

void CheckBox::draw() const {
    cout << "Drawing a CheckBox" << endl;
}
```

### main.cpp

```cpp
#include "CheckBox.h"
#include "TextBox.h"
#include "Widget.h"

#include <iostream>
#include <memory>
#include <vector>

using namespace std;

void showWidget(Widget& widget) {
    widget.draw();
}

int main() {
    Widget widget; // Compilation error

    vector<unique_ptr<Widget>> widgets;
    widgets.push_back(make_unique<TextBox>());
    // If we don't override the draw() function in TextBox class "void draw() const override;", then we will get a compilation error, because we the TextBox class will also become "Abstract".
    widgets.push_back(make_unique<CheckBox>());

    for (const auto& widget : widgets) {
        widget->draw();
    }

    return 0;
}
```


## 10. Exercise

### Shape.h

```cpp
#ifndef ADVANCED_SHAPE_H
#define ADVANCED_SHAPE_H

// Abstract
class Shape {
public:
    string getBackground();
    void setBackground();

    // Pure virtual method
    virtual void draw() const = 0;

private:
    string background;
};

#endif
```

### Shape.cpp

```cpp
#include "Shape.h"

#include <iostream>

using namespace std;

string Shape::getBackground() {
    return background;
}

void Shape::setBackground(const string& background) {
    this->background = background;
}

void Shape::draw() const {
    cout << "Drawing a Shape" << endl;
}
```

### Rectangle.h

```cpp
#ifndef ADVANCED_RECTANGLE_H
#define ADVANCED_RECTANGLE_H

class Rectangle : public Shape {
public:
    void draw() const override;
};

#endif
```

### Rectangle.cpp

```cpp
#include "Rectangle.cpp"

#include <iostream>

using namespace std;

void Rectangle::draw() const {
    cout << "Drawing a Rectangle" << endl;
}
```

### Circle.h

```cpp
#ifndef ADVANCED_CIRCLE_H
#define ADVANCED_CIRCLE_H

class Circle : public Shape {
public:
    void draw() const override;
};

#endif
```

### Circle.cpp

```cpp
#include "Circle.cpp"

#include <iostream>

using namespace std;

void Circle::draw() const {
    cout << "Drawing a Cicle" << endl;
}
```