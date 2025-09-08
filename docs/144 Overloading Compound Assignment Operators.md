# 144 Overloading Compound Assignment Operators

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/R2fihr1K87o?si=wrIomyQxCc2pzhF9"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Compound Assignment Operators Matter

### 🔄 Complement to Arithmetic Operators
- If you implement an arithmetic operator like `+`, it’s common to also implement its **compound assignment** counterpart (`+=`).
- This allows:
  ```cpp
  first = first + second;  // Less efficient
  first += second;         // More efficient
  ```
- **Benefit:** Avoids creating unnecessary temporary objects.

### 🧠 Key Difference
- `+` → Creates and returns a **new object**.
- `+=` → **Modifies the current object** and returns a **reference** to it.

---

## 2. Implementing `operator+=`

### 📜 Function Signature
- **Return type:** `Length&` (reference to the current object).
- **Parameter:** `const Length& other` (pass by const reference).
- **Not `const` method** — modifies the current object.

Example:
```cpp
Length& operator+=(const Length& other);
```

---

## 3. Defining `operator+=`

### 🛠 Implementation
```cpp
Length& Length::operator+=(const Length& other) {
    value += other.value;
    return *this; // Dereference `this` pointer to return the current object
}
```

### 🔍 Key Points
- `this` is a pointer to the current object.
- `*this` dereferences it to get the actual object.
- Returning `*this` allows **chaining**:
  ```cpp
  first += second += third;
  ```

---

## 4. Updated `Length` Class

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
    Length operator+(const Length& other) const;
    Length& operator+=(const Length& other);

    int getValue() const;
    void setValue(int value);
    
private:
    int value;

    friend ostream& operator<<(ostream& stream, const Length& length);
};

ostream& operator<<(ostream& stream, const Length& length);
istream& operator>>(istream& stream, Length& length);

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

Length Length::operator+(const Length& other) const {
    return Length(value + other.value);
}

Length& Length::operator+=(const Length& other) {
    value += other.value;
    return *this;
}

ostream& operator<<(ostream& stream, const Length& length) {
    stream << length.getValue();
    return stream;
}

istream& operator>>(istream& stream, Length& length) {
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
    Length first{10};
    Length second{10};

    first += second;

    cout << first;

    return 0;
}
```

---

## 5. Exercise

### Task
Implement `operator+=` in the `Point` class so you can:
- Add another `Point` to the current one.
- Add an integer to both coordinates.

### Point.h
```cpp
#ifndef ADVANCED_POINT_H
#define ADVANCED_POINT_H

#include <ostream>

using namespace std;

class Point {
public:
    Point(int x, int y);

    int getX() const;
    int getY() const;

    void setX(int x);
    void setY(int y);

    bool operator==(const Point& other) const;
    Point operator+(const Point& other) const;
    Point operator+(int value) const;
    Point& operator+=(const Point& other);
    Point& operator+=(int value);

private:
    int x;
    int y;
};

ostream& operator<<(ostream& stream, const Point& point);

#endif
```

### Point.cpp
```cpp
#include "Point.h"

Point::Point(int x, int y) : x{x}, y{y} {}

int Point::getX() const { return x; }
int Point::getY() const { return y; }

void Point::setX(int x) { this->x = x; }
void Point::setY(int y) { this->y = y; }

bool Point::operator==(const Point& other) const {
    return (x == other.x) && (y == other.y);
}

Point Point::operator+(const Point& other) const {
    return Point(x + other.x, y + other.y);
}

Point Point::operator+(int value) const {
    return Point(x + value, y + value);
}

Point& Point::operator+=(const Point& other) {
    x += other.x;
    y += other.y;
    return *this;
}

Point& Point::operator+=(int value) {
    x += value;
    y += value;
    return *this;
}

ostream& operator<<(ostream& stream, const Point& point) {
    stream << "(" << point.getX() << ", " << point.getY() << ")";
    return stream;
}
```

---

## 6. Interview Insights

### 💬 Common Questions
- Why return a reference from `operator+=`?
- Why is `operator+=` not marked as `const`?
- How does returning `*this` enable chaining?
- When should you implement both `+` and `+=`?

### 🧠 Mental Model
```
Compound Assignment
 ├── Modifies current object
 ├── Returns reference to self
 └── Enables chaining
```

---

## 7. Quick Revision Sheet

- `+` → returns new object, `+=` → modifies current object.
- Return type for `+=` should be a **reference**.
- Pass parameters as `const &` to avoid copies.
- Return `*this` to allow chaining.
- Do not mark `+=` as `const`.

---

## 8. Best Practices

- Always implement `+=` when you implement `+` for arithmetic-like types.
- Keep operator overloads **consistent** with built-in types.
- Group related operators together in the class definition.
- Avoid unnecessary temporary objects for performance.
- Ensure `+=` modifies the object safely and predictably.

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
    Length operator+(const Length& other) const;
    Length& operator+=(const Length& other);

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

Length Length::operator+(const Length& other) const {
    return Length(value + other.value);
}

Length& Length::operator+=(cons Length& other) {
    value += other.value;

    return *this;
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
    Length first{10};
    Length second{10};

    first += second;

    cout << first;

    return 0;
}
```