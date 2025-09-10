# 156 Conversion between Base and Derived Classes

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Y7wzNP7D2mQ?si=3NSTl_zoL6ZDTRRt"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Base ↔ Derived Conversion

### 🔼 Upcasting

* Converting a **derived class object** (`TextBox`) to a **base class type** (`Widget`).
* Always **legal**.
* Example:

  ```cpp
  TextBox box;
  Widget widget = box;  // Upcasting
  ```

### 🔽 Downcasting

* Converting a **base class object** to a **derived class type**.
* **Illegal**, because the base object doesn’t contain derived members.
* Example:

  ```cpp
  Widget widget;
  TextBox box = widget; // ❌ Illegal
  ```

---

## 2. Object Slicing

### ⚠️ Problem

* When a **derived object** is passed **by value** to a base parameter:

  * Extra members in the derived class are **discarded**.
  * This is called **object slicing**.

### Example

```cpp
void showWidget(Widget widget) {}

int main() {
    TextBox box;
    showWidget(box); // Object slicing happens
}
```

* `TextBox` has a `string value` (24 bytes by default).
* That extra data is **discarded** when copied into a `Widget`.

---

## 3. Preventing Slicing

### ✅ Pass by Reference

* No copy is made → no slicing.
* Example:

  ```cpp
  void showWidget(Widget& widget) {}
  showWidget(box); // OK
  ```

### ✅ Pass by Pointer

* Same as reference: no slicing.
* Example:

  ```cpp
  void showWidget(Widget* widget) {}
  showWidget(&box); // OK
  ```

### ❗ Limitation

* Even with reference/pointer, you can only access **base members** inside the function.

---

## 4. Code Walkthrough

### Pass by Value (Slicing Happens)

```cpp
void showWidget(Widget widget) {}

int main() {
    TextBox box;

    Widget widget = box;    // Upcasting
    showWidget(box);        // Object slicing

    // Downcasting (Illegal)
    // Widget widget;
    // TextBox box = widget;
}
```

### Pass by Reference (No Slicing)

```cpp
void showWidget(Widget& widget) {
    // Only access Widget’s members
}

int main() {
    TextBox box;
    showWidget(box); // No slicing
}
```

### Pass by Pointer (No Slicing)

```cpp
void showWidget(Widget* widget) {
    // Only access Widget’s members
}

int main() {
    TextBox box;
    showWidget(&box); // No slicing
}
```

---

## 5. Mental Model

```
TextBox (Derived)
 ├── Widget members
 └── + string value (extra data)

Upcasting:
   TextBox → Widget (safe, discards derived features)

Downcasting:
   Widget → TextBox (illegal, missing members)

Object Slicing:
   Passing by value → derived-specific data lost
```

---

## 6. Interview Insights

### 💬 Common Questions

* What is **upcasting** vs **downcasting**?
* What is **object slicing**?
* How do references/pointers prevent slicing?
* Why does downcasting usually signal **bad design**?
* What happens if you pass derived objects **by value** to base class functions?

---

## 7. Quick Revision Sheet

* ✅ Upcasting (Derived → Base) is safe.
* ❌ Downcasting (Base → Derived) is illegal by default.
* ⚠️ Object slicing occurs when passing **by value**.
* ✅ Use **references** or **pointers** to avoid slicing.
* ⚡ Even with references/pointers, you only see **base class members**.

---

## 8. Best Practices

* Avoid **passing base classes by value** — prefer references or pointers.
* If polymorphic behavior is needed, always use **virtual methods**.
* Keep inheritance hierarchies shallow → composition often better.
* Don’t rely on downcasting — it usually means your design needs rethinking.

---

## 9. Code from the video

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

using namespace std;

string TextBox::getValue() const {
    return value;
}

void TextBox::setValue(const string& value) {
    this->value = value;
}
```

### main.cpp

#### Pass by value - slicing happens

```cpp
#include "TextBox.h"
#include "Widget.h"

#include <iostream>

using namespace std;

void showWidget(Widget widget) {}

int main() {
    TextBox box;

    // Upcasting senario 1
    Widget widget = box;

    // Upcasting senario 2
    showWidget(box);

    // Downcasting (Illegal)
    // Widget widget;
    // TextBox box = widget;

    return 0;
}
```

#### Pass by reference - no slicing
```cpp
#include "TextBox.h"
#include "Widget.h"

#include <iostream>

using namespace std;

void showWidget(Widget& widget) {
    // We don't have access to any member of TextBox, we can only access the members of Widget
}

int main() {
    TextBox box;

    // Upcasting
    showWidget(box);

    return 0;
}
```

#### Pass by reference using pointers - no slicing
```cpp
#include "TextBox.h"
#include "Widget.h"

#include <iostream>

using namespace std;

void showWidget(Widget* widget) {
    // We don't have access to any member of TextBox, we can only access the members of Widget
}

int main() {
    TextBox box;

    // Upcasting
    showWidget(&box);

    return 0;
}
```