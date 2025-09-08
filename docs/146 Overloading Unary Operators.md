# 146 Overloading Unary Operators

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/mN0qnAgoi9s?si=xWJ_ppOSGz5AYT6i"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Overview

### 🔹 Unary Operators
- Operate on **a single operand**.
- Examples: `++` (increment), `--` (decrement), unary `-`, logical `!`.

### 🔹 Prefix vs Postfix Increment
| Form      | Syntax  | Execution Order |
|-----------|---------|-----------------|
| **Prefix** | `++x`   | Increment first, then return the updated object |
| **Postfix**| `x++`   | Return a copy first, then increment the original |

**Example with `int`:**
```cpp
int x = 10, y;

// Prefix
y = ++x; // x = 11, y = 11

// Postfix
x = 10;
y = x++; // y = 10, x = 11
```

---

## 2. Function Signatures

### Prefix Increment
```cpp
Length& operator++(); // No parameters
```
- Returns **reference** to current object (`Length&`).
- Modifies the existing object.

### Postfix Increment
```cpp
Length operator++(int); // Dummy int parameter
```
- Returns **by value** (copy).
- Dummy `int` differentiates it from prefix form.
- Does **not** modify the returned object (modifies the original after copying).

---

## 3. Implementation — `Length` Class

### Length.h
```cpp
class Length {
public:
    Length() = default;
    explicit Length(int value);

    // Prefix
    Length& operator++();

    // Postfix
    Length operator++(int);

    int getValue() const;
    void setValue(int value);

private:
    int value{};
};
```

### Length.cpp
```cpp
#include "Length.h"
#include <iostream>

Length::Length(int value) : value(value) {}

int Length::getValue() const { return value; }
void Length::setValue(int value) { this->value = value; }

// Prefix: increment, then return *this
Length& Length::operator++() {
    value++;
    return *this;
}

// Postfix: copy, increment original, return copy
Length Length::operator++(int) {
    Length copy = *this; // Requires copy constructor
    operator++();        // Delegate to prefix
    return copy;
}
```

---

## 4. Testing

### main.cpp
```cpp
#include "Length.h"
#include <iostream>
using namespace std;

int main() {
    Length first{10};

    // Postfix
    Length second = first++;
    cout << "First: " << first.getValue() << endl;  // 11
    cout << "Second: " << second.getValue() << endl; // 10

    // Prefix
    second = ++first;
    cout << "First: " << first.getValue() << endl;  // 12
    cout << "Second: " << second.getValue() << endl; // 12
}
```

---

## 5. Key Points & Best Practices

- **Postfix needs a dummy `int` parameter** to differentiate from prefix.
- **Return type matters**:
  - Prefix → `Type&` (reference to current object).
  - Postfix → `Type` (copy).
- **Delegate logic**: Postfix should call prefix to avoid code duplication.
- **Copy constructor must be available** for postfix to work.
- If you delete the copy constructor, postfix increment won’t compile.

---

## 6. Exercise — `Point` Class Increment

### Correct Implementation
```cpp
Point& Point::operator++() {
    x++;
    y++;
    return *this;
}

Point Point::operator++(int) {
    Point copy = *this;
    operator++();
    return copy;
}
```
- Increment both `x` and `y` in prefix.
- Postfix returns the original state before increment.

---

## 7. Interview Insights

**Possible Questions:**
- Why does postfix take an `int` parameter?
- Why return by reference in prefix but by value in postfix?
- How to avoid code duplication between prefix and postfix?
- What happens if copy constructor is deleted?

**Mental Model:**
```
Prefix (++x)   → Increment → Return updated object
Postfix (x++)  → Copy → Increment original → Return copy
```

---

## 8. Code from the video

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
    Length() = default;
    explicit Length(int value);

    bool operator==(const Length& other) const;
    bool operator==(int other) const;
    strong_ordering operator<=>(const Length& other) const;
    Length operator+(const Length& other) const;
    Length& operator+=(const Length& other);
    Length& operator++(); // prefix
    Length operator++(int); // postfix

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

#include <iostream>

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

Length& Length::operator=(cons Length& other) {
    cout << "Object assigned" << endl;
    value = other.value;

    return *this;
}

Length& Length::operator++() {
    value++;

    return *this;
}

Length Length::operator++(int) {
    Length copy = *this;

    operator++();

    return copy;
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
    Length second = first++;

    cout << "First: " << first << endl;
    cout << "Second: " << second << endl;

    second = ++first;

    cout << "First: " << first << endl;
    cout << "Second: " << second << endl;

    return 0;
}
```


## 9. Exercise

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
    Point& operator++();
    Point operator++(int);

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

int Point::getX() const {
    return x;
}

int Point::getY() const {
    return y;
}

void Point::setX(int x) {
    this->x = x;
}

void Point::setY(int y) {
    this->y = y;
}

bool Point::operator==(const Point& other) const {
    return (x == other.x) && (y == other.y);
}

Point Point::operator+(const Point& other) const {
    return Point((x + other.x), (y + other.y));
}

Point Point::operator+(int value) const {
    return Point((x + value), (y + value));
}

Point& Point::operator++() {
    x++;
    y++;

    return *this;
}

Point Point::operator++(int) {
    Point copy = *this;

    operator++();

    return copy;
}

ostream& operator<<(ostream& stream, const Point& point) {
    stream << "(" << point.getX() << ", " << point.getY() << ")";
    
    return stream;
}
```

### main.cpp

```cpp
#include "Point.h"

#include <iostream>

using namespace std;

int main() {
    Point first{10, 20};
    Point second = first++;

    cout << "First: " << first << endl;
    cout << "Second: " << second << endl;

    second = ++first;

    cout << "First: " << first << endl;
    cout << "Second: " << second << endl;

    return 0;
}
```