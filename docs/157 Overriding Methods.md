# 157 Overriding Methods

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Wg8JLOc66pM?si=WOaByi87rBCITwn_"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Redefining vs Overriding

### 🧩 Redefining

* If a derived class provides a method with the same name as the base class method, **but the base method is not virtual**, it’s just **redefining**.
* The compiler uses **early (static) binding** at compile time.
* Example:

  * `Widget::draw()` exists.
  * `TextBox::draw()` is defined.
  * If you pass a `TextBox` to a function taking `Widget&`, **Widget’s `draw()`** runs.

### 🎭 Overriding

* If a base method is declared as `virtual`, derived methods with the same signature **override** it.
* The compiler uses **late (dynamic) binding** at runtime.
* Example:

  * `Widget::draw()` is `virtual`.
  * `TextBox::draw()` overrides it.
  * If you pass a `TextBox` to a function taking `Widget&`, **TextBox’s `draw()`** runs.

---

## 2. Virtual Functions

### 🔑 Key Idea

* Mark base methods as `virtual` if they are meant to be **customized by derived classes**.
* At runtime, the correct method implementation is chosen based on the actual object type.

### ⚡ Why?

* Without `virtual` → early binding (base method called).
* With `virtual` → late binding (derived method called).

---

## 3. Best Practice: `override`

### ✅ Use `override` in Derived Classes

* Makes intent explicit: you’re **overriding**, not redefining.
* If base method signature changes, the compiler warns you.

Example:

```cpp
class Widget {
public:
    virtual void draw() const;
};

class TextBox : public Widget {
public:
    void draw() const override; // safer
};
```

---

## 4. Difference: Overloading vs Overriding

### 🔄 Overloading

* Same method name, **different parameter lists**.
* Decided at compile time.

### 🎭 Overriding

* Same method name and **same signature**.
* Base method must be `virtual`.
* Decided at runtime.

---

## 5. Code Walkthrough

### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    void enable();
    void disable();
    bool isEnabled() const;

    virtual void draw() const; // Marked virtual

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
    enabled = true;
}

void Widget::disable() {
    enabled = false;
}

bool Widget::isEnabled() const {
    return enabled;
}

void Widget::draw() const {
    cout << "Drawing a Widget" << endl;
}
```

### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : public Widget {
public:
    string getValue() const;
    void setValue(const string& value);

    void draw() const override; // Overriding draw

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

### main.cpp

```cpp
#include "TextBox.h"
#include "Widget.h"
#include <iostream>
using namespace std;

void showWidget(Widget& widget) {
    widget.draw(); // Will call correct draw at runtime
}

int main() {
    TextBox box;

    box.draw();         // Drawing a TextBox
    showWidget(box);    // Also Drawing a TextBox (thanks to virtual)

    return 0;
}
```

---

## 6. Output

```
Drawing a TextBox
Drawing a TextBox
```

---

## 7. Exercise

### 🔨 Task

* Create a base class `Shape` with virtual method `draw()`.
* Override it in `Rectangle` and `Circle`.

### Shape.h

```cpp
#ifndef ADVANCED_SHAPE_H
#define ADVANCED_SHAPE_H

#include <string>

class Shape {
public:
    string getBackground() const;
    void setBackground(const string& background);

    virtual void draw() const; // Virtual

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

string Shape::getBackground() const {
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

#include "Shape.h"

class Rectangle : public Shape {
public:
    void draw() const override;
};

#endif
```

### Rectangle.cpp

```cpp
#include "Rectangle.h"
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

#include "Shape.h"

class Circle : public Shape {
public:
    void draw() const override;
};

#endif
```

### Circle.cpp

```cpp
#include "Circle.h"
#include <iostream>
using namespace std;

void Circle::draw() const {
    cout << "Drawing a Circle" << endl;
}
```

---

## 8. Interview Insights

### 💬 Common Questions

* What’s the difference between **redefining** and **overriding**?
* How does C++ implement polymorphism?
* Why should you use the `override` keyword?
* What’s the difference between **overloading** and **overriding**?
* Why is `virtual` important in base classes?

---

## 9. Quick Revision Sheet

* Use `virtual` in base class → enables polymorphism.
* Use `override` in derived class → safer code.
* Redefining without `virtual` → early binding (base method called).
* Overriding with `virtual` → late binding (derived method called).
* Overloading → same name, different parameters.

---

## 10. Best Practices

* Always mark base methods as `virtual` if they’re meant to be overridden.
* Always use `override` keyword in derived classes.
* Don’t mix up overriding with overloading.
* Keep function signatures consistent between base and derived classes.
* Use polymorphism to design **extensible frameworks**.

---

## 11. Code from the video

### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    void enable();
    void disable();
    bool isEnabled() const;

    virtual void draw() const;

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

void Widget::draw() const {
    cout << "Drawing a Widget" << endl;
}
```

### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : public Widget {
public:
    string getValue() const;
    void setValue(const string& value);

    // Overriding
    void draw() const override; // We shouldn't do overriding without the "override" keyword like void draw() const;. Override keyword helps the compiler to generate warning when the method of the base class is modified.

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

### main.cpp

```cpp
#include "TextBox.h"
#include "Widget.h"

#include <iostream>

using namespace std;

void showWidget(Widget& widget) {
    widget.draw();
}

int main() {
    TextBox box;

    box.draw();

    showWidget(box); // Without the "virtual method" this will print "Drawing a Widget" due to early or static binding done by the compiler. Now, the compiler will use late or dynamic binding. At the run time the compiler will decide which draw() method to call based on the type of object passed to the showWidget()

    return 0;
}
```

## 12. Exercise

### Shape.h

```cpp
#ifndef ADVANCED_SHAPE_H
#define ADVANCED_SHAPE_H

class Shape {
public:
    string getBackground();
    void setBackground();

    virtual void draw() const;

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