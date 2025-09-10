# 160 Virtual Destructors

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/1bUYFJ6P2Rc?si=eTnztm_9MEPir08Q"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Setup

We added destructors to all classes (`Widget`, `TextBox`, `CheckBox`) that print messages when objects are destroyed.

```cpp
~Widget()  { cout << "Destructing Widget\n"; }
~TextBox() { cout << "Destructing TextBox\n"; }
~CheckBox(){ cout << "Destructing CheckBox\n"; }
```

Then we stored `TextBox` and `CheckBox` objects in a **polymorphic collection**:

```cpp
vector<unique_ptr<Widget>> widgets;
widgets.push_back(make_unique<TextBox>());
widgets.push_back(make_unique<CheckBox>());
```

---

## 2. First Run: What Goes Wrong?

Output:

```
Destructing Widget
Destructing Widget
```

⚠️ Problem:

* Only the **base class destructor (`Widget`)** is called.
* The destructors of `TextBox` and `CheckBox` are skipped.
* This breaks proper cleanup → derived resources would leak.

Reason:
Destructor binding is **static (early)** by default → compiler only knows about `Widget`’s destructor.

---

## 3. Correct Solution: Virtual Destructor

In `Widget.h`:

```cpp
class Widget {
public:
    virtual ~Widget() = default; // ✅ ensures proper destruction
    virtual void draw() const;
};
```

### Second Run (after fix):

```
Destructing TextBox
Destructing Widget
Destructing CheckBox
Destructing Widget
```

✅ Now destructors are called in correct reverse order:
Derived first → then base.

---

## 4. Key Takeaways

* Inheritance destruction order should always be:

  * **Derived destructor → Base destructor**
* Without `virtual ~Widget()`, polymorphic deletion calls **only base destructor**.
* With `virtual ~Widget()`, deletion dispatches **dynamically (late binding)**.

---

## 5. Visual Mental Model

```
Case 1: ~Widget() NOT virtual
   [unique_ptr<Widget>] --> deletes as Widget only
   Output: Destructing Widget
   (Derived destructor skipped ❌)

Case 2: ~Widget() virtual
   [unique_ptr<Widget>] --> dynamic dispatch
   Actual type = TextBox/CheckBox
   Calls Derived::~ → then Base::~
   Output: Destructing TextBox → Destructing Widget
```

---

## 6. Best Practices & Insights

### ✅ Always:

* If a class has **any virtual function**, make the destructor **virtual** too.
* Mark it as `= default` if no custom logic:

```cpp
virtual ~Widget() = default;
```

* Use **smart pointers** (`unique_ptr`) so destruction is automatic.

### ⚠️ Common Gotchas:

* Forgetting virtual destructor → leaks/undefined behavior.
* Declaring destructor but not marking it `virtual`.
* Printing/logging destructors in real code → only useful for learning/debugging.

### 🧭 Architecture:

* If a class is designed as a **base class for polymorphism**, add `virtual ~Base()`.
* If class is **final (no inheritance)**, no need for virtual destructor.

---

## 7. Interview Insights

**Q:** Why do we need a virtual destructor in base classes?
**A:** To ensure when deleting derived objects via a base pointer, the derived destructor runs before the base destructor.

**Q:** What happens if the base destructor is not virtual?
**A:** Only the base destructor executes. Derived resources won’t be released → undefined behavior.

**Q:** When should destructors be virtual?
**A:** Whenever the class has at least one other virtual function (i.e., intended for polymorphic use).

**Q:** Cost of virtual destructors?
**A:** Slight increase in vtable size; negligible compared to correctness.

---

## 8. Quick Revision Sheet

* Destructors are **non-virtual by default** → static binding.
* Polymorphic base classes must declare:

  ```cpp
  virtual ~Base() = default;
  ```
* Destruction order: **Derived → Base**.
* Forgetting this leads to undefined behavior and leaks.
* Rule of thumb: **If class has any virtual functions, destructor must be virtual.**

---

## 10. Code from the video

### Part 1

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    virtual ~Widget():

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

#### Widget.cpp

```cpp
#include "Widget.h"

#include <iostream>

using namespace std;

Widget::~Widget() {
    cout << "Destructing a Widget" << endl;
}

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

#### TextBox.h

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

#### TextBox.cpp

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

#### CheckBox.h

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

#### CheckBox.cpp

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

#### main.cpp

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
    vector<unique_ptr<Widget>> widgets;
    widgets.push_back(make_unique<TextBox>());
    widgets.push_back(make_unique<CheckBox>());

    for (const auto& widget : widgets) {
        widget->draw();
    }

    return 0;
}
```

### Part 2

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    virtual ~Widget() = default:

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

#### Widget.cpp

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

#### TextBox.h

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

#### TextBox.cpp

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

#### CheckBox.h

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

#### CheckBox.cpp

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

#### main.cpp

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
    vector<unique_ptr<Widget>> widgets;
    widgets.push_back(make_unique<TextBox>());
    widgets.push_back(make_unique<CheckBox>());

    for (const auto& widget : widgets) {
        widget->draw();
    }

    return 0;
}
```