# 140 Overloading the Stream Insertion Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/QR9LRW92B_g?si=OcxwY0xmWlZhuZSm"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Overview
- **Goal:** Allow objects to be sent directly to output streams like `std::cout`.
- **Why:** Improves readability and integrates custom types with the C++ I/O system.
- **Key Rule:** Must be a **non‑member** (global) function if the left operand is not your class type.

---

## 2. Function Signature
```cpp
ostream& operator<<(ostream& stream, const Length& length);
```
- **Return type:** `ostream&` → allows chaining (`cout << a << b;`).
- **First parameter:** `ostream&` → the output stream to write to.
- **Second parameter:** `const&` to your object → avoids copying and prevents modification.

---

## 3. Why Non‑Member?
- For `a << b`, the left operand (`a`) is an `ostream` object, not your class.
- You can’t modify `std::ostream`’s source, so the operator must be defined outside your class.

---

## 4. Implementation Steps
1. Add a **getter** for the private data you want to output.
2. Define the operator function outside the class.
3. Write the object’s data to the stream.
4. Return the stream to enable chaining.

---

## 5. Example – `Length` Class

### Length.h
```cpp
class Length {
public:
    explicit Length(int value);
    int getValue() const;
    void setValue(int value);
    // other operators...
private:
    int value;
};

ostream& operator<<(ostream& stream, const Length& length);
```

### Length.cpp
```cpp
ostream& operator<<(ostream& stream, const Length& length) {
    stream << length.getValue();
    return stream;
}
```

---

## 6. Chaining Explained
```cpp
cout << first << second;
```
- First call: `operator<<(cout, first)` returns `cout`.
- Second call: `operator<<(cout, second)` runs on the returned `cout`.

---

## 7. Exercise – `Point` Class
```cpp
ostream& operator<<(ostream& stream, const Point& point) {
    stream << "(" << point.getX() << ", " << point.getY() << ")";
    return stream;
}
```

---

## 8. Mental Model

```
std::ostream (e.g., cout)
        │
        ▼
operator<<(ostream& stream, const Length& obj)   // non-member
        │
        ├─ Accesses obj's public getters (or private data if friend)
        │
        ├─ Writes formatted data into 'stream'
        │
        └─ Returns 'stream' by reference
                │
                ▼
        Enables chaining: cout << a << b;
```

**Key points in the model:**
- **Non‑member function**:  
  - Left operand is `ostream`, so the operator must live outside your class.
  - Can be a **friend** if it needs direct access to private members.
- **Return `ostream&`**:  
  - Allows the returned stream to be used for the next `<<` in a chain.
- **Access pattern**:  
  - Uses **getters** (preferred) or friendship to retrieve internal state.
- **Execution flow in chaining**:
  1. `cout << first` → calls `operator<<(cout, first)` → returns `cout`.
  2. Returned `cout` is fed into `operator<<(cout, second)`.

**Mental shortcut:**  
Think of `<<` as a **data printer** that:
1. Takes a stream and your object.
2. Prints the object’s representation into the stream.
3. Hands the same stream back so printing can continue.

---

## 9. Quick Revision Sheet
- Always return `ostream&` for chaining.
- Pass object by `const&`.
- Must be non‑member if left operand isn’t your class.
- Use getters to access private data.

---

## 10. Interview Insights
💬 **Possible Questions**
- Why must `<<` be a non‑member for `ostream`?
- Why return `ostream&` instead of `void`?
- How does chaining work internally?
- Can you make it a `friend` function instead?

---

## 11. Code from the video

### Length.h

```cpp
#ifndef ADVANCED_LENGTH_H
#define ADVANCED_LENGTH_H

#include <compare>
#include <ostream>

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
};

ostream& operator<<(orstream& stream, const Length& length);

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

    return stream; // To chain insertion operator multiple times.
}

// This will help us do 
// cout << 1 << 2; -> cout << 2; -> cout
```

### main.cpp

```cpp
#include "Length.h"

#include <iostream>

using namespace std;

int main() {
    Length first{10};
    
    cout << first << endl;

    return 0;
}
```

## 12. Exercise

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
    Point second{10, 20};

    cout << first << endl;

    return 0;
}
```