# 154 Constructors and Inheritance

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/jWjoh0PgWkQ?si=xqomdsJGk9QKE-nK"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Constructor Call Order in Inheritance

### 🏗 Order of Construction
- **Base class constructor** is always called **before** the derived class constructor.
- Ensures base part of the object is fully initialized before derived part.

### 📜 Example Output
```
Widget constructed
TextBox constructed
```

---

## 2. Default Constructor in Base Class

### 🧩 Scenario
- If base class has a **default constructor**, derived class constructors **automatically** call it (unless explicitly calling another base constructor).

### 📌 Example
```cpp
class Widget {
public:
    Widget() { cout << "Widget constructed\n"; }
};
class TextBox : public Widget {
public:
    TextBox() { cout << "TextBox constructed\n"; }
};
```

---

## 3. No Default Constructor in Base Class

### 🚨 Problem
- If base class **only** has parameterized constructors, derived class **must** explicitly call one in its initializer list.
- Otherwise → **Compilation error**.

### 📌 Example
```cpp
class Widget {
public:
    Widget(bool enabled) : enabled{enabled} {}
private:
    bool enabled;
};

class TextBox : public Widget {
public:
    TextBox() {} // ❌ Error — must call Widget(bool)
};
```

---

## 4. Fixing the Error

### ✅ Option 1 — Add Default Constructor
```cpp
Widget() = default;
Widget(bool enabled) : enabled{enabled} {}
```

### ✅ Option 2 — Call Base Constructor Explicitly
```cpp
TextBox::TextBox(bool enabled) : Widget(enabled) {
    cout << "TextBox constructed\n";
}
```

### ✅ Option 3 — Pass Parameters from Derived Constructor
```cpp
TextBox::TextBox(bool enabled, const string& value)
    : Widget(enabled), value{value} {}
```

---

## 5. Inheriting Constructors

### 📜 Why?
- If derived constructors **only** forward arguments to base constructors without extra logic, you can **inherit** them instead of rewriting.

### 📌 Syntax
```cpp
class TextBox : public Widget {
public:
    using Widget::Widget; // Inherit all Widget constructors
};
```

- Saves boilerplate.
- Still possible to add **custom constructors** alongside inherited ones.

---

## 6. Final Example

### Widget.h
```cpp
class Widget {
public:
    Widget(bool enabled);
    void enable();
    void disable();
    bool isEnabled() const;
private:
    bool enabled;
};
```

### Widget.cpp
```cpp
Widget::Widget(bool enabled) : enabled{enabled} {
    cout << "Widget constructed\n";
}
```

### TextBox.h
```cpp
class TextBox : public Widget {
public:
    using Widget::Widget; // Inherit constructors
    string getValue() const;
    void setValue(const string& value);
private:
    string value;
};
```

---

## 7. Interview Insights

### 💬 Common Questions
- What is the order of constructor calls in inheritance?
- What happens if base class has no default constructor?
- How do you call a specific base class constructor from derived?
- When should you use `using Base::Base`?

### 🧠 Mental Model
```
Object Construction:
[Base Part] → [Derived Part]
```

---

## 8. Quick Revision Sheet

- Base constructor runs **before** derived constructor.
- If base has **no default constructor**, derived must call one explicitly.
- Use initializer lists to call base constructors.
- `using Base::Base` inherits base constructors — reduces boilerplate.

---

## 9. Best Practices

- Keep base constructors minimal — avoid heavy logic.
- Use `explicit` for single‑parameter constructors to avoid implicit conversions.
- Prefer `using Base::Base` when forwarding constructors without extra logic.
- Always initialize members in initializer lists, not inside constructor body.

---

## 10. Common Pitfalls

- Forgetting to call base constructor when no default exists → compile error

## 11. Code from the video

### Part - 1

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    Widget();

    void enable();
    void disable();
    bool isEnabled() const;

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

Widget::Widget() {
    cout << "Widget constructed" << endl;
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
```


#### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : public Widget {
public:
    TextBox();
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

using namespace std;

TextBox::TextBox() {
    cout << "TextBox constructed" << endl;
}

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
#include "Widget.h"

#include <iostream>

using namespace std;

int main() {
    TextBox box;

    return 0;
}
```

### Part - 2

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    Widget() = default;
    Widget(bool enabled);

    void enable();
    void disable();
    bool isEnabled() const;

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

Widget::Widget(bool enabled) : enabled{enabled} {
    cout << "Widget constructed" << endl;
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
```

#### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : public Widget {
public:
    TextBox();
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

using namespace std;

TextBox::TextBox() {
    cout << "TextBox constructed" << endl;
}

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
#include "Widget.h"

#include <iostream>

using namespace std;

int main() {
    TextBox box;

    return 0;
}
```

### Part - 3

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    Widget(bool enabled);

    void enable();
    void disable();
    bool isEnabled() const;

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

Widget::Widget(bool enabled) : enabled{enabled} {
    cout << "Widget constructed" << endl;
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
```


#### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : public Widget {
public:
    TextBox(bool enabled);
    explicit TextBox(bool enabled, const string& value);

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

using namespace std;

TextBox::TextBox(bool enabled) : Widget(enabled) {
    cout << "TextBox constructed" << endl;
}

TextBox::TextBox(bool enabled, const string& value) : Widget(enabled), value{value} {}

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
#include "Widget.h"

#include <iostream>

using namespace std;

int main() {
    TextBox box;

    return 0;
}
```

### Part - 4

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    Widget(bool enabled);

    void enable();
    void disable();
    bool isEnabled() const;

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

Widget::Widget(bool enabled) : enabled{enabled} {
    cout << "Widget constructed" << endl;
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
```


#### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include "Widget.h"

class TextBox : public Widget {
public:
    using Widget::Widget;
    explicit TextBox(bool enabled, const string& value);

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

using namespace std;

TextBox::TextBox(bool enabled, const string& value) : Widget(enabled), value{value} {}

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
#include "Widget.h"

#include <iostream>

using namespace std;

int main() {
    TextBox box;

    return 0;
}
```