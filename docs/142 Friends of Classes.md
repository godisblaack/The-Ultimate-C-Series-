# 142 Friends of Classes

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/5hrAQTglOTo?si=u5UdR_6eGm01Unom"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Friend Functions – The Basics

- A **friend function** is **not** a member of a class, but it can access the class’s **private** and **protected** members.
- Declared inside the class with the `friend` keyword.
- Defined **outside** the class like a normal function.
- Useful when:
  - You need external functions (e.g., operator overloads) to access private data.
  - You don’t want to expose that data via public getters/setters.

---

## 2. Why Use a Friend Function?

- Sometimes, an operator or helper function **must** access internal state that:
  - Has **no public accessor**.
  - Should **not** be exposed to the outside world (pure implementation detail).
- Example: `Length` class has a private `x` attribute — no getter/setter — but `operator<<` needs it for output formatting.

---

## 3. Declaring a Friend Function

- Inside the class:
  ```cpp
  friend ostream& operator<<(ostream& stream, const Length& length);
  ```
- This tells the compiler:
  - "`operator<<` is trusted — it can access my private members."

---

## 4. Defining a Friend Function

- Outside the class, define it like a normal function:
  ```cpp
  ostream& operator<<(ostream& stream, const Length& length) {
      stream << length.value << " (x=" << length.x << ")";
      return stream;
  }
  ```
- No `friend` keyword in the definition — only in the declaration.

---

## 5. Memory & Access Rules

- Friendship is **not mutual**:
  - If `A` declares `B` as a friend, `B` can access `A`’s private members.
  - But `A` cannot access `B`’s private members unless `B` also declares `A` as a friend.
- Friendship is **not inherited**:
  - Derived classes do not automatically inherit friendship.

---

## 6. Mental Model

```
Length class
 ├── public: getValue(), setValue(), operators...
 ├── private: value, x
 └── friend: operator<<
 
operator<<
 ├── Not a member of Length
 ├── Can directly access value and x
 └── Defined outside the class
```

---

## 7. Quick Revision Sheet

- **friend** keyword → grants access to private/protected members.
- Declare inside class, define outside.
- No `friend` in definition.
- Friendship is **one-way** and **not inherited**.
- Common use: stream operators (`<<`, `>>`).

---

## 8. Interview Insights

💬 **Possible Questions**
- What is a friend function? How does it differ from a member function?
- Why might you prefer a friend function over a public getter?
- Is friendship mutual or inherited in C++?
- Can a friend function be overloaded?
- Can a member function of another class be a friend?

---

## 9. Code from the video

### Length.h

```cpp
#ifndef ADVANCED_LENGTH_H
#define ADVANCED_LENGTH_H

#include <compare>
#include <ostream>
#include <istream>

using namespace std;

class Length {
public:
    explicit Length(int value);

    bool operator==(const Length& other) const;
    bool operator==(int other) const;
    strong_ordering operator<=>(const Length& other) const;

    int getValue() const;
    void setValue(int value);
    
private:
    int value;
    int x;

    friend ostream& operator<<(orstream& stream, const Length& length);
};

ostream& operator<<(orstream& stream, const Length& length);

istream& operator>>(irstream& stream, Length& length);

#endif
```

### Length.cpp

```cpp
#include "Length.h"

Length::Length(int value) : value(value) {}

bool Length::operator==(const Length& other) const {
    return value == other.value; 
}

bool Length::operator==(int other) const {
    return value == other;
}

int Length::getValue() const {
    return value;
}

void Length::setValue(int value) {
    this->value = value;
}

strong_ordering Length::operator<=>(const Length& other) const {
    return value <=> other.value;
}

ostream& operator<<(ostream& stream, const Length& length) {
    stream << length.getValue();
    
    length.x;

    return stream;
}

istream& operator>>(irstream& stream, Length& length) {
    int value;

    stream >> value;
    length.setValue(value);
    
    return stream;
}
```

### main.cpp

```cpp
#include "Length.h"

#include <iostream>

using namespace std;

int main() {
    Length length{10};
    
    cout << "Length: ";

    cin >> length;

    cout << length;

    return 0;
}
```