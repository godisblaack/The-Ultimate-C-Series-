# 162 Final Classes and Methods

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/--YtVBv88g0?si=YN5SgZTMOSIhS__4"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Setup

So far, we’ve used **virtual** and **override** to enable polymorphism.
But sometimes we want to **stop further overriding** of a function or **stop inheritance** of a class.

Examples:

* `TextBox::draw()` should always use the same drawing algorithm → no other class should override it.
* You want `TextBox` itself to be the last class in the hierarchy → no one should inherit from it.

👉 This is where the **`final` keyword** comes in.

---

## 2. `final` on Methods

We can mark a method as `final` to prevent further overrides:

```cpp
class TextBox : public Widget {
public:
    void draw() const override final;
    // Or: void draw() const final override;
};
```

* Both orders (`override final` or `final override`) are correct.
* Once marked, **no derived class can override it**.

---

## 3. Example – Preventing Override

```cpp
class MaskedTextBox : public TextBox {
public:
    void draw() const override; // ❌ Compilation error
};
```

Error:

```
declaration of ‘draw’ overrides a ‘final’ function
```

👉 This ensures all `TextBox`-based widgets use the **same drawing algorithm**.

---

## 4. `final` on Classes

You can also mark a class itself as `final`:

```cpp
class TextBox final : public Widget {
public:
    void draw() const override final;
};
```

Now:

```cpp
class MaskedTextBox : public TextBox {}; // ❌ Error: base ‘TextBox’ is marked final
```

👉 No class can inherit from `TextBox`.

---

## 5. Visual Mental Model

```
Widget (abstract)
   |
   v
 TextBox
   |
   +-- draw() marked final  --> no override possible
   |
   +-- class marked final   --> no inheritance possible
```

Two levels of "stopping extension":

* `final` method → lock down **behavior**.
* `final` class → lock down **hierarchy**.

---

## 6. Best Practices & Insights

✅ Use `final` when:

* You want **stability in design** (e.g., core UI widgets shouldn’t change drawing logic).
* Preventing accidental overrides (safer maintenance in large projects).

✅ Placement flexibility:

```cpp
void f() override final;
void f() final override;  // both work
```

⚠️ Be careful:

* Too much use of `final` reduces extensibility.
* Overuse might force unnecessary code duplication later.

---

## 7. Interview Insights

**Q:** Difference between `override` and `final`?
**A:**

* `override` → ensures you are overriding a base class method.
* `final` → prevents further overriding or inheritance.

**Q:** Can a class be abstract and `final` at the same time?
**A:** No — `final` prevents inheritance, while abstract classes require inheritance.

**Q:** Why use `final`?
**A:** To enforce design decisions and avoid fragile hierarchies.

**Q:** Where is `final` commonly used?
**A:** Frameworks, GUI libraries, or APIs where certain methods/classes must not be changed.

---

## 8. Quick Revision Sheet

* `final` on method → no further overrides.
* `final` on class → no further inheritance.
* Syntax:

  ```cpp
  void draw() const override final;
  class TextBox final : public Widget;
  ```
* `override final` order doesn’t matter.
* Good for **locking down APIs** & ensuring consistency.
* Avoid combining `abstract` and `final` → design contradiction.

---

## 9. Code from the video

### Part 1

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

    virtual void draw() const = 0;

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

    void draw() const override final;
    // void draw() const final override; Both ways of writing is correct.

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

#### MaskedTextBox.h

```cpp
#ifndef ADVANCED_MASKEDTEXTBOX_H
#define ADVANCED_MASKEDTEXTBOX_H

#include "TextBox.h"

class MaskedTextBox : public TextBox {
public:
    void draw() const override; // Compilation error
};

#endif
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

    virtual void draw() const = 0;

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
```

#### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox final : public Widget {
public:
    ~TextBox();

    string getValue() const;
    void setValue(const string& value);

    void draw() const override final;
    // void draw() const final override; Both ways of writing is correct.

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

#### MaskedTextBox.h

```cpp
#ifndef ADVANCED_MASKEDTEXTBOX_H
#define ADVANCED_MASKEDTEXTBOX_H

#include "TextBox.h"

// Compilation error
class MaskedTextBox : public TextBox {}; 

#endif
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