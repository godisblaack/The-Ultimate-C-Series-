# 153 Protected Members

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/gk46CfalvJY?si=7aTfUnuT9BhsxyBS"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why `protected` Matters

### 🔒 Access Control Recap
- **Private members** → accessible **only** within the same class.
- **Protected members** → accessible **within the same class** **and** in **derived classes**.
- **Public members** → accessible from anywhere.

---

## 2. `protected` in Action

### 🧩 Example: `Widget` Base Class
- `enabled` → **private** (cannot be accessed in derived classes).
- `width` → **protected** (accessible in derived classes, but not outside).

---

## 3. Access Rules Summary

| Access Level  | Same Class | Derived Class | Outside Class |
|---------------|------------|--------------|---------------|
| **public**    | ✅         | ✅           | ✅            |
| **protected** | ✅         | ✅           | ❌            |
| **private**   | ✅         | ❌           | ❌            |

---

## 4. Code Walkthrough

### Widget.h
```cpp
class Widget {
public:
    void enable();
    void disable();
    bool isEnabled() const;

private:
    bool enabled;   // ❌ Not accessible in derived classes

protected:
    int width;      // ✅ Accessible in derived classes
};
```

### Widget.cpp
```cpp
void Widget::enable()  { enabled = true; }
void Widget::disable() { enabled = false; }
bool Widget::isEnabled() const { return enabled; }
```

---

### TextBox.h
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

### TextBox.cpp
```cpp
std::string TextBox::getValue() const {
    this->width; // ✅ Accessible (protected)
    // enabled;  // ❌ Not accessible (private)
    return value;
}
```

---

### main.cpp
```cpp
Widget widget;
widget.width; // ❌ Compilation error — protected

TextBox box;
box.disable(); // ✅ Public method from base
```

---

## 5. When to Use `protected`

### ✅ Use When:
- You **must** allow derived classes to directly access a member.
- Example: base class geometry attributes (`width`, `height`) that derived shapes manipulate.

### 🚫 Avoid When:
- You can provide access via **public/protected getters/setters** instead.
- Overusing `protected` can **break encapsulation** — derived classes become tightly coupled to base internals.

---

## 6. Interview Insights

### 💬 Common Questions
- Difference between **private** and **protected**?
- Why is `protected` less common than `private`?
- How does `protected` affect encapsulation?
- Can `protected` members be accessed outside the class hierarchy?

### 🧠 Mental Model
```
Access Levels
 ├── public     → Everyone
 ├── protected  → Class + Derived
 └── private    → Class only
```

---

## 7. Quick Revision Sheet

- **private** → class only.
- **protected** → class + derived.
- **public** → everywhere.
- Prefer **private** by default — use `protected` only when necessary.
- Access base class members in derived classes via `this->member`.

---

## 8. Best Practices

- Keep data members **private** unless a strong reason for `protected`.
- Use **protected** for helper methods/attributes meant for subclass use.
- Avoid exposing implementation details unnecessarily.
- Maintain **encapsulation** — don’t let derived classes depend on base internals unless essential.

---

## 9. Common Pitfalls

- Making members `protected` just to “make it work” — leads to fragile designs.
- Forgetting that `protected` still **prevents outside access**.
- Confusing `protected` with `public` — they behave differently in inheritance.

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

private:
    bool enabled;

protected:
    int width;
};

#endif
```

### Widget.cpp

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

### TextBox.h

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

### TextBox.cpp

```cpp
#include "TextBox.h"

#include <iostream>

TextBox::TextBox(const string& value) : value{value} {}

string TextBox::getValue() const {
    // Protected members are accessible
    this->width;

    // Private members are not accessible
    // this.enabled();

    return value;
}

void TextBox::setValue(const string& value) {
    this->value = value;
}
```

### main.cpp

```cpp
#include "TextBox.h"
#include "Widget.h"

#include <iostream>

using namespace std;

int main() {
    Widget widegt;
    widget.width; // Compilation error

    TextBox box;
    
    box.disable();

    cout << box.isEnabled();

    return 0;
}
```