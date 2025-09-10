# 158 Polymorphism

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/2gBy3irWKwg?si=afaN5eyRESMnDCjn"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

# 1. What is Polymorphism?

### 📖 Definition

* **Poly** → many
* **Morph** → form
* **Polymorphism** = *"many forms"*
* In OOP, polymorphism allows an object to take different forms depending on its actual type.

### ✨ Why It Matters

* Helps build **extensible applications**.
* One interface (`Widget`) can be used with many different implementations (`TextBox`, `CheckBox`).
* Makes code **flexible** and **maintainable**.

---

# 2. Base Class: `Widget`

### 🔧 Key Features

* Declares a **virtual method** `draw()`.
* Serves as a base type for all UI elements.

```cpp
class Widget {
public:
    void enable();
    void disable();
    bool isEnabled() const;

    virtual void draw() const; // ✅ virtual = can be overridden
};
```

### 💡 Note

* `draw()` is **virtual**, meaning derived classes can provide their **own version**.
* Without `virtual`, polymorphism **won’t work**.

---

# 3. Derived Class: `TextBox`

### 📝 Adds Behavior

* Inherits from `Widget`.
* Adds its own property `value`.
* Overrides `draw()`.

```cpp
class TextBox : public Widget {
public:
    string getValue() const;
    void setValue(const string& value);

    void draw() const override; // 🔄 overrides Widget::draw
};
```

```cpp
void TextBox::draw() const {
    cout << "Drawing a TextBox" << endl;
}
```

---

# 4. Derived Class: `CheckBox`

### ✅ Another Form of Widget

* Also inherits from `Widget`.
* Provides its **own implementation** of `draw()`.

```cpp
class CheckBox : public Widget {
public:
    void draw() const override;
};
```

```cpp
void CheckBox::draw() const {
    cout << "Drawing a CheckBox" << endl;
}
```

---

# 5. Polymorphism in Action

### 🔄 Common Interface

* The function `showWidget()` takes a `Widget&`.
* At runtime, it calls the **correct `draw()` method** depending on the object passed.

```cpp
void showWidget(Widget& widget) {
    widget.draw(); // 👈 Polymorphism happens here
}
```

### 🖥️ Example Usage

```cpp
TextBox box;
showWidget(box);     // → "Drawing a TextBox"

CheckBox checkBox;
showWidget(checkBox); // → "Drawing a CheckBox"
```

➡️ Even though `showWidget` expects a `Widget`, the correct `draw()` is called based on the actual type.

---

# 6. Visual Mental Model

```
Widget (Base)
 └── virtual draw()
      ↓
      ├── TextBox → "Drawing a TextBox"
      └── CheckBox → "Drawing a CheckBox"

showWidget(Widget&) → resolves to actual type at runtime
```

---

# 7. Best Practices & Insights

* Always mark **overridden methods** with `override` (C++11+).
* Use **base class references or pointers** (`Widget&`, `Widget*`) to enable polymorphism.
* Keep base class methods `virtual` when expecting derived behavior.
* Polymorphism helps achieve **Open/Closed Principle**:

  * *Open for extension* (add new widgets).
  * *Closed for modification* (don’t need to change existing code).

---

# 8. Interview Insights

### 💬 Common Questions

* What is polymorphism, and why is it useful?
* What’s the difference between **compile-time** (function overloading) and **runtime** polymorphism (virtual functions)?
* Why do we use `virtual` in the base class?
* What happens if you forget `virtual` in the base class?
* What’s the difference between `virtual` and `override`?
* How does the compiler implement polymorphism internally (vtable / vptr)?
* Can constructors and static functions be virtual in C++? Why not?

---

# 9. Quick Revision Sheet

* `virtual` → enables runtime polymorphism.
* `override` → ensures correct overriding.
* `Widget` = common interface.
* `TextBox`, `CheckBox` = different forms of `Widget`.
* `showWidget(widget)` → calls the right `draw()` at runtime.

---

## 10. Code from the video

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
    void draw() const override;
};

#endif
```

### CheckBox.cpp

```cpp
#include "CheckBox.h"

#include <iostream>

using namespace std;

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

using namespace std;

void showWidget(Widget& widget) {
    widget.draw();
}

int main() {
    TextBox box;

    showWidget(box);
 
    CheckBox checkBox;

    showWidget(checkBox);
 
    return 0;
}
```