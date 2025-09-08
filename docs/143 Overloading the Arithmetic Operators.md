# 143 Overloading the Arithmetic Operators

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/zhdhltfs3MM?si=8FerZvC3PELbuZn1"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Arithmetic Operator Overloading Matters

### ➕ Intuitive Syntax
- Operator overloading allows **custom types** to behave like **built-in types**.
- Example:  
  ```cpp
  Length third = first + second;
  ```
  feels natural and improves **readability**.

### 🧠 Real-World Analogy
- Just like adding two integers produces another integer, adding two `Length` objects should produce another `Length`.

---

## 2. Implementing `operator+` for Custom Types

### 📜 Function Signature
- Return type: **`Length`** (adding two lengths produces a new length).
- Parameters: **One** (the right-hand operand; left-hand is the current object).
- Pass parameter as **`const &`** to avoid copying.
- Mark function as **`const`** to ensure it doesn’t modify the current object.

Example:
```cpp
Length operator+(const Length& other) const;
```

---

## 3. Defining `operator+`

### 🛠 Implementation
```cpp
Length Length::operator+(const Length& other) const {
    return Length(value + other.value);
}
```

### 🔍 Key Points
- Creates and returns a **new object**.
- Uses **member access** to retrieve values from both operands.
- Keeps **immutability** of operands.

---

## 4. Overloading for Different Types

### 🔄 Adding an `int` to a `Length`
- Provide another overload:
```cpp
Length operator+(int other) const;
```
- This allows:
```cpp
Length total = lengthObj + 5;
```

---

## 5. Updated `Length` Class

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

    Length third = first + second;

    cout << third;

    return 0;
}
```

---

## 6. Exercise

### Task
Implement `operator+` in the `Point` class so you can:
- Add two points.
- Add an integer to a point.

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

    Point third = first + second;
    cout << third << endl;

    int value = 5;
    Point moved = first + value;
    cout << moved << endl;

    return 0;
}
```

---

## 7. Interview Insights

### 💬 Common Questions
- Why return a new object instead of modifying the current one?
- Why pass the parameter as `const &`?
- How do you overload for different operand types?
- What’s the difference between member and non-member operator overloads?

### 🧠 Mental Model
```
Operator Overloading
 ├── Member Function
 │    ├── Left operand = current object
 │    └── Right operand = parameter
 ├── Return new object
 └── Keep operands immutable
```

---

## 8. Quick Revision Sheet

- Use **`const &`** for parameters to avoid copies.
- Mark operator functions as **`const`** if they don’t modify the object.
- Return **new objects** for arithmetic operators.
- Overload for different operand types if needed.
- Keep code readable by grouping related operators together.

---

## 9. Best Practices

- Keep operator overloads **intuitive** and **consistent** with built-in types.
- Avoid surprising behavior — `+` should not subtract.
- Use **friend functions** for symmetric operations if needed.
- Maintain **immutability** for arithmetic operators.
- Group operators logically in the class definition for readability.

---

## 10. Code from the video

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

    Length third = first + second;

    cout << third;

    return 0;
}
```

## 11. Exercise

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

    Point third = first + second;
    
    cout << third << endl;

    int value = 5;
    
    Point moved = first + value;

    cout << moved << endl;

    return 0;
}
```