# 128 Using the Explicit Keyword

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/kBtytoUnG40?si=BgYXgIwhAq-c0B_6" 
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. The Problem

- A **single‑parameter constructor** without `explicit` can be used by the compiler for **implicit conversions**.
- Example:
  ```cpp
  class Person {
  public:
      Person(int age); // converting constructor
  private:
      int age;
  };

  void showPerson(Person p) {}

  showPerson(20); // ✅ allowed — compiler converts int → Person
  ```
- This can lead to **unexpected or illogical code**:
  - Passing an `int` where a `Person` is expected.
  - Compiler silently creates a temporary `Person` object.

---

## 2. The `explicit` Keyword

- **Purpose**: Prevents the compiler from using the constructor for **implicit conversions**.
- Syntax:
  ```cpp
  explicit Person(int age);
  ```
- Now:
  ```cpp
  showPerson(20); // ❌ error — no implicit conversion
  showPerson(Person{20}); // ✅ explicit construction
  ```

---

## 3. Key Terms

| Term | Meaning |
|------|---------|
| **Converting Constructor** | A constructor that can be called with a single argument and is not marked `explicit`. Enables implicit conversions. |
| **Explicit Constructor** | A constructor marked with `explicit`. Can only be called directly, not via implicit conversion. |

---

## 4. When to Use `explicit`

✅ Use `explicit` when:
- The constructor takes **a single parameter**.
- You want to **avoid accidental conversions**.
- The conversion would be **semantically unclear**.

❌ You may omit `explicit` when:
- The conversion is **natural and intended** (e.g., `std::string` from `const char*`).

---

## 5. Example – Person Class

### Without `explicit` (Converting Constructor)
```cpp
class Person {
public:
    Person(int age); // implicit conversion allowed
private:
    int age;
};

Person::Person(int age) : age{age} {}

void showPerson(Person p) {}

showPerson(20); // ✅ int → Person automatically
```

### With `explicit`
```cpp
class Person {
public:
    explicit Person(int age); // no implicit conversion
private:
    int age;
};

showPerson(20);       // ❌ error
showPerson(Person{20}); // ✅ explicit
```

---

## 6. Exercise – TextBox Class

```cpp
class TextBox {
public:
    TextBox() = default; // default constructor
    explicit TextBox(const std::string& value); // explicit single-param constructor

    std::string getValue() const;
    void setValue(const std::string& value);
private:
    std::string value;
};

TextBox::TextBox(const std::string& value) : value{value} {}
```

---

## 7. Mental Model

```
Function Call
 ├── Argument type matches parameter type → direct call
 ├── Argument type different
 │     ├── Single-arg constructor exists & not explicit → implicit conversion
 │     └── Constructor is explicit → ❌ no implicit conversion
```

---

## 8. Quick Revision Sheet

- **`explicit`**: Stops implicit conversions for single‑argument constructors.
- **Default**: Without `explicit`, single‑arg constructors are *converting constructors*.
- **Best Practice**: Mark all single‑argument constructors `explicit` unless you *want* implicit conversion.
- **Applies to**: Constructors and conversion operators.

---

## 9. Interview Insights

💬 **Possible Questions**
- What is a converting constructor?
- How does `explicit` affect overload resolution?
- Why might implicit conversions be dangerous?
- Give an example where `explicit` prevents a bug.

---

## 10. Code from the video

### Person.h without explicit constructor

```cpp
#ifndef ADVANCED_PERSON_H
#define ADVANCED_PERSON_H

class Person {
public:
    // Converting constructor
    Person(int age);
private:
    int age;
};

#endif
```

###  Person.h with explicit constructor

```cpp
#ifndef ADVANCED_PERSON_H
#define ADVANCED_PERSON_H

class Person {
public:
    explicit Person(int age);
private:
    int age;
};

#endif
```

### Person.cpp

```cpp
#include "Person.h"

Person::Person(int age) : age{age} {}
```

### main.cpp

```cpp
#include "Person.h"

#include <iostream>

using namespace std;

void showPerson(Person person) {

}

int main() {
    Person person{20};
    showPerson(20);

    return 0;
}
```

## Exercise

### TextBox.h

```cpp
#ifndef ADVANCED_TEXTBOX_H
#define ADVANCED_TEXTBOX_H

#include <string>

using namespace std;

class TextBox {
public:
    TextBox() = default;
    explicit TextBox(const string& value);

    string getValue() const;
    void setValue(const string& value);
private:
    string value;
}

#endif
```

### TextBox.cpp

```cpp
#include "TextBox.h"

#include <iostream>

TextBox::TextBox(const string& value) : value{value} {}

string TextBox::getValue() const {
    return value;
}

void TextBox::setValue(const string& value) {
    this->value = value;
}
```

### main.cpp

```cpp
#include "TextBox.h"

#include <iostream>

using namespace std;

int main() {
    TextBox box;
    
    box.setValue("Hello World");

    cout << box.getValue();

    return 0;
}
```