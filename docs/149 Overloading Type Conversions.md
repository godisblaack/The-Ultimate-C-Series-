# 149 Overloading Type Conversions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/nq13kbigmM4?si=YP-_dZY-4yljkEB5"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Overload Conversion Operators

### 🔄 Seamless Type Conversion  
- Converting a custom type to a built-in type makes your class more **intuitive**.  
- Example:  
  ```cpp
  Length len{10};
  int x = len;   // Error without conversion operator
  ```  
  With `operator int()`, the compiler knows how to convert `Length` → `int`.

### 🛡 Implicit vs. Explicit  
- **Implicit** conversion can be handy but may lead to subtle bugs.  
- Marking your conversion **`explicit`** requires a cast, preventing unintended conversions.

---

## 2. Function Signature

### 📜 Syntax  

```cpp
// Implicit conversion
operator int() const;

// Explicit conversion (C++11+)
explicit operator int() const;
```

- No parameters.  
- No return keyword or type in name—it’s encoded in the function name.  
- Mark `const` because it doesn’t modify the object.

---

## 3. Defining the Conversion Function

### 🛠 Core Logic  
```cpp
Length::operator int() const {
    return value;  // unwrap the underlying integer
}
```

- Returns the internal `value` member.  
- Enables both assignment and arithmetic contexts where `int` is expected.

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
    Length() = default;
    explicit Length(int value);

    bool operator==(const Length& other) const;
    bool operator==(int other) const;
    strong_ordering operator<=>(const Length& other) const;
    Length operator+(const Length& other) const;
    Length& operator+=(const Length& other);
    Length& operator++();       // prefix
    Length  operator++(int);    // postfix

    // Conversion to int
    operator int() const;                 
    // Or to prevent implicit conversions:
    // explicit operator int() const;

    int getValue() const;
    void setValue(int value);

private:
    int value;
};

ostream& operator<<(ostream& os, const Length& length);
istream& operator>>(istream& is, Length& length);

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

int Length::getValue() const { return value; }
void Length::setValue(int v)   { value = v; }

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

Length& Length::operator++() {
    ++value;
    return *this;
}

Length Length::operator++(int) {
    Length tmp = *this;
    operator++();
    return tmp;
}

// Conversion operator
Length::operator int() const {
    return value;
}

ostream& operator<<(ostream& os, const Length& length) {
    os << length.getValue();
    return os;
}

istream& operator>>(istream& is, Length& length) {
    int v;
    is >> v;
    length.setValue(v);
    return is;
}
```

---

## 5. Example Usage

### Implicit Conversion
```cpp
#include "Length.h"
#include <iostream>
using namespace std;

int main() {
    Length len{42};
    int x = len;            // OK if operator int() is not explicit
    cout << x << endl;      // prints 42
}
```

### Explicit Conversion
```cpp
#include "Length.h"
#include <iostream>
using namespace std;

int main() {
    Length len{42};
    // int x = len;         // Error if conversion is explicit
    int x = static_cast<int>(len);
    cout << x << endl;      // prints 42
}
```

---

## 6. Exercise

### Task  
In your `Point` class, add a conversion operator to `double` that returns the Euclidean distance from the origin:
```cpp
explicit operator double() const;
```

#### Hint  
```cpp
double Point::operator double() const {
    return std::sqrt(x*x + y*y);
}
```

---

## 7. Interview Insights

### 💬 Common Questions  
- What’s the difference between implicit and explicit conversion operators?  
- How does `explicit operator Type()` prevent unintended conversions?  
- When is it safe or unsafe to provide implicit conversions?  
- How do conversion operators interact with overload resolution?

### 🧠 Mental Model  
```
Conversion Operator
 ├── operator Type() const      // implicit or explicit
 ├── No parameters
 └── Provides a bridge between custom and built-in types
```

---

## 8. Quick Revision Sheet

- Declare as `operator T() const` for implicit; add `explicit` to require casts.  
- No return type keyword in the signature—just `operator T()`.  
- Must be `const` if it doesn’t modify the object.  
- Use `static_cast<T>(obj)` when conversion is marked `explicit`.  
- Avoid dangerous narrowing conversions implicitly.

---

## 9. Best Practices

- Prefer **explicit** conversion operators for non-trivial or narrowing conversions.  
- Limit implicit conversions to **lossless** and **widely expected** cases.  
- Document your conversion operators clearly in the class interface.  
- Combine with `operator bool()` for safe “truth‐value” conversions (mark as explicit).  
- Remember the **Rule of Zero/Five** if your class manages resources.

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
    Length() = default;
    explicit Length(int value);

    bool operator==(const Length& other) const;
    bool operator==(int other) const;
    strong_ordering operator<=>(const Length& other) const;
    Length operator+(const Length& other) const;
    Length& operator+=(const Length& other);
    Length& operator++(); // prefix
    Length operator++(int); // postfix

    explicit operator int() const;

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

Length::operator int() const {
    return value;
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

#### If operator int() const; is defined without explicit

```cpp
#include "Length.h"

#include <iostream>

using namespace std;

int main() {
    Length first{10};
    
    int x = length;

    cout << x << endl;

    return 0;
}
```

#### If operator int() const; is defined with "explicit"


```cpp
#include "Length.h"

#include <iostream>

using namespace std;

int main() {
    Length first{10};
    
    int x = static_cast<int>(length);

    cout << x << endl;

    return 0;
}
```