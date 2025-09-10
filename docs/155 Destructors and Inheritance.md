# 155 Destructors and Inheritance

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/IOf37sOXF2Q?si=0X2834lHhMoJ2wmn"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Destructor Order in Inheritance

### 🧩 Key Behavior

* In C++, **destructors are called in reverse order of construction**.
* First, the **derived class destructor** executes.
* Then, the **base class destructor** executes.

### 🖥️ Example

* If `TextBox` inherits from `Widget`:

  * `~TextBox()` runs first.
  * Then `~Widget()` runs.

---

## 2. Adding Destructors

### 📌 Implementation

* We add destructors to both `Widget` and `TextBox` to observe order.
* Each prints a message when called.

### 📝 Why?

* Even though in this case destructors aren’t needed (no dynamic memory or resources used),
  it’s a **good exercise** to see destructor execution order.

---

## 3. Problem with Constructors

### ⚠️ Issue

* In earlier code, `Widget` required a `bool enabled` parameter in its constructor.
* This forced us to pass a value when creating derived objects (e.g., `TextBox`), as shown below:
    ```cpp
    #include "TextBox.h"
    #include "Widget.h"
    #include <iostream>
    using namespace std;

    int main() {
        TextBox box{true};
        return 0;
    }
    ```
* This is **bad design** — widgets should be **enabled by default**.

### ✅ Fix

* Remove unnecessary constructors from:

  * `Widget`
  * `TextBox`
* Simplifies object creation → `TextBox box;`

---

## 4. Code Walkthrough

### Part 1: With Destructors

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    ~Widget();

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

Widget::~Widget() {
    cout << "Widget destructed" << endl;
}

void Widget::enable() {
    enabled = true;
}

void Widget::disable() {
    enabled = false;
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
    cout << "TextBox destructed" << endl;
}

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

---

### Part 2: After Cleanup (Better Design)

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
    enabled = true;
}

void Widget::disable() {
    enabled = false;
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

---

## 5. Output (Part 1)

```
TextBox destructed
Widget destructed
```

---

## 6. Mental Model

```
TextBox (Derived)
   ↓ Destructor runs first
Widget (Base)
   ↓ Destructor runs second
```

---

## 7. Interview Insights

### 💬 Common Questions

* What’s the order of destructor calls in inheritance?
* When should you declare destructors as `virtual`?
* Why avoid unnecessary constructors in base classes?
* What happens if you don’t provide a destructor?

---

## 8. Quick Revision Sheet

* **Destructor order** → Derived first, then Base.
* Don’t require unnecessary parameters in base constructors.
* Only define destructors when managing resources.
* If base class is polymorphic, declare destructor as `virtual`.

---

## 9. Best Practices

* Prefer **default constructors** unless you need special initialization.
* Use destructors only for **resource cleanup** (memory, files, sockets).
* Mark destructors `virtual` in base classes when using polymorphism.
* Keep inheritance hierarchies **simple and meaningful**.

---

## 10. Code from the video

### Part 1

#### Widget.h

```cpp
#ifndef ADVANCED_WIDGET_H
#define ADVANCED_WIDGET_H

class Widget {
public:
    ~Widget();

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

Widget::~Widegt() {
    cout << "Widget destructed" << endl;
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
    ~TextBox();

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

TextBox::~TextBox() {
    cout << "TextBox destructed" << endl;
}

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

### Part 2

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