# 159 Polymorphic Collections

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/LXxvYMsd1OA?si=tj2kRaWMZCVmzSzS"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Polymorphic Collections

### 🧰 What’s New?

* Instead of working with a single `Widget`, we now store **multiple widgets** in a collection.
* Each element may represent a different type (`TextBox`, `CheckBox`).
* Goal: call `draw()` on all of them and see the correct output.

---

# 2. First Attempt: Vector of Objects

```cpp
vector<Widget> widgets;

widgets.push_back(TextBox{});
widgets.push_back(CheckBox{});

for (const auto& widget : widgets) {
    widget.draw();
}
```

### ❌ Problem: Object Slicing

* Even though we add a `TextBox` and `CheckBox`, they are **copied** into the vector as plain `Widget` objects.
* Derived part of the object is sliced off.
* So `draw()` from `Widget` gets called every time:

```
Drawing a Widget
Drawing a Widget
```

➡️ This is called **static binding or early binding** (decided at compile time).

---

# 3. Solution with Raw Pointers

```cpp
vector<Widget*> widgets;

widgets.push_back(new TextBox{});
widgets.push_back(new CheckBox{});

for (const auto& widget : widgets) {
    widget->draw(); // ✅ Correct method called
}

// Manual cleanup
for (auto widget : widgets) {
    delete widget;
}
widgets.clear();
```

### ✅ Fix

* Store **pointers to Widget**, not objects.
* No slicing → actual type (`TextBox` or `CheckBox`) is preserved.
* Runtime decides which `draw()` to call (**dynamic binding**).

```
Drawing a TextBox
Drawing a CheckBox
```

➡️ This is called **dynamic binding or late binding** (decided at compile time).

### ⚠️ Issue

* Using `new` requires **manual `delete`**.
* Easy to forget → memory leaks.

---

# 4. Modern Fix: Smart Pointers

```cpp
vector<unique_ptr<Widget>> widgets; // Polymorphic collection

widgets.push_back(make_unique<TextBox>());
widgets.push_back(make_unique<CheckBox>());

for (const auto& widget : widgets) {
    widget->draw();
}
```

### 🚀 Benefits

* `unique_ptr` manages memory automatically.
* No need for manual `delete`.
* Clean, safe, and modern C++ style.

### 🖥️ Output

```
Drawing a TextBox
Drawing a CheckBox
```

---

# 5. Binding Recap

* **Static Binding (Early)** → happens when we copy objects into `vector<Widget>`.

  * Function calls resolved **at compile time**.
  * Causes **object slicing**.
* **Dynamic Binding (Late)** → happens with `vector<Widget*>` or `vector<unique_ptr<Widget>>`.

  * Function calls resolved **at runtime**.
  * Correct derived `draw()` is executed.

---

## 6. Visual Mental Model

```
[Bad] vector<Widget> (values)          --> each element is a Widget base subobject
  push_back(TextBox{})  ==> TextBox sliced → | Widget | Widget | ...
  call draw()           ==> Widget::draw()  (no derived behavior)

[Works] vector<Widget*> (raw pointers)  --> heap objects, pointers in vector
  push_back(new TextBox{})  ==> [TextBox heap]  [CheckBox heap]
  vector holds pointers ---> widget->draw() -> dispatches to derived draw

[Best] vector<unique_ptr<Widget>> (ownership)
  push_back(make_unique<TextBox>()) -> vector owns heap object
  automatic cleanup on vector destruction
  widget->draw() -> derived draw()
```

---

## 7. Best Practices & Insights

### ✅ Design / code-level

* **Use smart pointers** (usually `std::unique_ptr`) for containers of polymorphic objects:

  ```cpp
  vector<unique_ptr<Widget>> widgets;
  widgets.push_back(make_unique<TextBox>());
  ```
* **Prefer `std::make_unique` / `std::make_shared`** over `new`.
* **Iterate by `const auto&`** to avoid unnecessary copies:

  ```cpp
  for (const auto& widget : widgets)
      widget->draw();
  ```
* **If polymorphism is expected, ensure the base class has a virtual destructor**:

  ```cpp
  struct Widget {
      virtual ~Widget() = default;   // <- crucial
      virtual void draw() const; // or virtual void draw() const;
  };
  ```

  * Without a virtual destructor, deleting derived objects through a base pointer causes undefined behaviour.

### ⚠️ Common gotchas

* **Object slicing** when storing derived objects by value in a base-typed container.
* **Forgetting virtual destructor** in base class (can silently break deletion).
* **Copying `unique_ptr`** (not allowed) — use `move` if transferring ownership.
* **Using raw `new`/`delete`** — error-prone; prefer smart pointers.

### 🧭 Architecture tips

* If **shared ownership** is needed (many owners), use `shared_ptr`. Otherwise prefer `unique_ptr`.
* If you frequently need value semantics and no polymorphism, keep values in the container (no pointers).
* Consider `std::variant`/`std::visit` if you have a fixed set of types and want value semantics with pattern matching.

---

## 8 Interview Insights

### 💬 Common questions & short answers you should prepare

* **Q:** What is object slicing?
  **A:** When a derived object is copied into a base-type value (e.g., `Widget`), the derived portion is discarded — only the base subobject remains.

* **Q:** Why does `vector<Widget>` not call `TextBox::draw()`?
  **A:** Because the `TextBox` got sliced into a `Widget` value. Virtual dispatch needs a base **pointer/reference** to a polymorphic (virtual) base.

* **Q:** How do you store a polymorphic collection properly?
  **A:** Store pointers (raw or smart) to the base: `vector<unique_ptr<Widget>>` is the usual modern solution.

* **Q:** `unique_ptr` vs `shared_ptr`?
  **A:** `unique_ptr` = single ownership, cheaper; `shared_ptr` = shared ownership, uses atomic refcount (costly). Choose based on ownership semantics.

* **Q:** What happens if base destructor is not virtual?
  **A:** Deleting a derived object through a base pointer leads to undefined behavior (possible memory/resource leak and wrong destructor calls).

* **Q:** What is vtable/vptr?
  **A:** The compiler implementation of virtual dispatch: a vtable holds pointers to virtual functions; each polymorphic object has a vptr pointing to its type’s vtable. The call is resolved at runtime via vptr→vtable.

* **Q:** When to prefer polymorphic containers vs other techniques?
  **A:** Use polymorphism when you need runtime interchangeable behavior. For closed sets of types with compile-time known alternatives, `std::variant` can be better.

* **Q:** Why `make_unique`? Why not `new`?
  **A:** `make_unique` provides exception safety and clearer ownership semantics; less boilerplate and no raw `new` usage.

---

## 9. Quick Revision Sheet

* `vector<Widget>` → **slicing** → wrong calls.
* `vector<Widget*>` → works, but **manual memory**.
* `vector<unique_ptr<Widget>>` → **best practice**: no leaks, ownership clear.
* Always use `virtual` for methods you expect to override and **virtual destructor** for polymorphic base classes.
* Use `make_unique<T>()` / `make_shared<T>()`. Iterate with `const auto&`.
* Know vtable/vptr basics — useful for interviews.

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
#include <memory>
#include <vector>

using namespace std;

void showWidget(Widget& widget) {
    widget.draw();
}

int main() {
    // Static or early binding
    vector<Widget> widgets;

    widgets.push_back(TextBox{}); 
    widgets.push_back(checkBox{}); 

    for (const auto& widget : widgets) {
        widget.draw();
    }

    // Dynamic or late binding
    vector<Widget*> widgets;

    widgets.push_back(new TextBox{}); 
    widgets.push_back(new checkBox{}); 

    for (const auto& widget : widgets) {
        widget->draw();
    }

    // Manual deletion
    for (auto widget : widgets) {
        delete widget;
    }

    widgets.clear();


    // Smart pointer
    vector<unique_ptr<Widget>> widgets; // Polymorphic collection
    widgets.push_back(make_unique<TextBox>());
    widgets.push_back(make_unique<CheckBox>());

    for (const auto& widget : widgets) {
        widget->draw();
    }

    return 0;
}
```