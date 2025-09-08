# 150 Inline Functions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/VQR_Pt2K6iA?si=LKljbODkQYMBcEmy"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Use Inline Functions

Inline functions let you hint to the compiler that small, frequently called routines should be expanded “in place” rather than invoked through a normal call/return sequence.  
By replacing the call with the function body, you can eliminate the overhead of pushing arguments, jumping to code, and returning back.

---

## 2. Syntax and Declaration

To declare or define an inline function, you use the `inline` keyword:

- In a header (definition in-class):  
  ```cpp
  inline int getValue() const { return value; }
  ```
- In an implementation file (external definition):  
  ```cpp
  inline int Length::getValue() const {
      return value;
  }
  ```

Either form requests that the compiler consider inlining calls to `getValue()`.

---

## 3. Definition Location: Header vs. Implementation

Defining functions inside the header makes them implicitly inline by virtue of being in-class definitions.  

However, exposing logic in the header:

- Forces recompilation of every dependent translation unit whenever you change the implementation.  
- Clutters the class interface with implementation details, reducing readability.

Moving definitions to the `.cpp` file keeps the header stable and concise, and you can still qualify the function with `inline`.

---

## 4. Pros and Cons

Pros  
- Removes call/return overhead for very small functions.  
- Can enable better compiler optimizations by exposing more context.  

Cons  
- Increases binary size if overused (each call site expands the code).  
- Can degrade performance if large functions are inlined repeatedly.  
- Exposing implementation in headers increases compile-time dependencies and rebuild scope.  

---

## 5. Best Practices

- Use inlining sparingly for one-liner getters, setters, or trivial operators.  
- Trust modern compilers to make inlining decisions automatically—manual hints are often unnecessary.  
- Keep headers focused on the **interface**; avoid embedding complex logic.  
- If you must inline from a `.cpp`, mark the definition with `inline` to avoid multiple‐definition errors during linking.  

---

## 6. Updated `Length` Class Declarations

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

    explicit operator int() const;

    int getValue() const;       // declaration only
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

// Inline definition in .cpp
inline int Length::getValue() const {
    return value;
}

void Length::setValue(int v) {
    value = v;
}

// ... other member definitions ...
```

---

## 7. Example Usage

```cpp
#include "Length.h"
#include <iostream>

using namespace std;

int main() {
    Length length{10};
    int v = length.getValue();   // call may be inlined
    cout << v << endl;           // prints 10
    return 0;
}
```

---

## 8. Quick Revision Sheet

- `inline` keyword requests in-place expansion of function calls.  
- In-class definitions are implicitly inline.  
- External definitions need `inline` to avoid duplicate symbols.  
- Best for trivial, one-line routines.  
- Overusing inline can bloat binaries and slow compile times.  

---

## 9. Interview Insights

**Common Questions**  
1. What does `inline` actually guarantee versus what the compiler may choose?  
2. How does inlining affect linkage and One Definition Rule?  
3. When would you prefer header definitions over external definitions?  
4. How can excessive inlining degrade performance?  

**Mental Model**  
```
Function Call
    │
    ├─ Normal: push args → jump → execute → return
    └─ Inline: expand body here → no jump overhead
```

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

    // Inline function
    // int getValue() const {
    //     return value;
    // }

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

inline int Length::getValue() const {
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

```cpp
#include "Length.h"

#include <iostream>

using namespace std;

int main() {
    Length length{10};
    
    length.getValue();
}
```