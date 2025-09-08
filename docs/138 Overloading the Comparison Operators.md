# 138 Overloading the Comparison Operators

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/PWIeHV0CmMA?si=QKYO6oVxO-P93Uq9"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>


## 1. Concept Overview
- **Goal:** Enable `<`, `>`, `<=`, `>=` for custom classes.
- **Why:** Makes objects comparable in a natural way (e.g., sorting).
- **Approach:**  
  - Implement `<` and `>` directly.  
  - Implement `<=` in terms of `>` and `>=` in terms of `<` to avoid duplication.

---

## 2. Less‑Than Operator (`<`)

### Declaration
```cpp
bool operator<(const Length& other) const;
```
- Pass by `const&` for efficiency.
- Mark as `const` to ensure no modification.

### Definition
```cpp
bool Length::operator<(const Length& other) const {
    return value < other.value;
}
```

---

## 3. Greater‑Than Operator (`>`)

### Declaration
```cpp
bool operator>(const Length& other) const;
```

### Definition
```cpp
bool Length::operator>(const Length& other) const {
    return value > other.value;
}
```

---

## 4. Derived Operators

### Less‑Than or Equal (`<=`)
```cpp
bool Length::operator<=(const Length& other) const {
    return !(value > other.value);
}
```

Mosh is comparing two integers directly in the above implementation, so in future if there is any change in the logic of comparision for operator< then he has to make changes in several places. To avoid this we can use the implementation below:

```cpp
bool Length::operator<=(const Length& other) const {
    return !(*this > other);
}
// This will call the overloaded operator>(const Length& other)
```

### Greater‑Than or Equal (`>=`)
```cpp
bool Length::operator>=(const Length& other) const {
    return !(value < other.value);
}
```

As explained before, we should use the implementation below: 

```cpp
bool Length::operator>=(const Length& other) const {
    return !(*this < other);
}
// This will call the overloaded operator<(const Length& other)
```

---

## 5. Why This Pattern?
- **Maintainability:** Change logic in `<` or `>` once, and all related operators stay consistent.
- **Readability:** Clear delegation of logic.
- **Performance:** Avoids repeating comparison code.

---

## 6. Example – `Length` Class

### Length.h
```cpp
class Length {
public:
    explicit Length(int value);

    bool operator<(const Length& other) const;
    bool operator>(const Length& other) const;
    bool operator<=(const Length& other) const;
    bool operator>=(const Length& other) const;

private:
    int value;
};
```

### Length.cpp
```cpp
Length::Length(int value) : value(value) {}

bool Length::operator<(const Length& other) const {
    return value < other.value;
}

bool Length::operator>(const Length& other) const {
    return value > other.value;
}

bool Length::operator<=(const Length& other) const {
    return !(*this > other);
}

bool Length::operator>=(const Length& other) const {
    return !(*this < other);
}
```

---

## 7. Mental Model
```
Length
 ├── private: int value
 ├── public:
 │    ├── operator<(const Length&) const   // core
 │    ├── operator>(const Length&) const   // core
 │    ├── operator<=(const Length&) const  // derived from >
 │    └── operator>=(const Length&) const  // derived from <
 └── DRY principle: core ops define ordering, derived ops reuse them
```

---

## 8. Quick Revision Sheet
- Implement `<` and `>` directly.
- Derive `<=` from `>` and `>=` from `<`.
- Always mark as `const` and pass objects by `const&`.
- Keep logic DRY (Don’t Repeat Yourself).

---

## 9. Interview Insights
💬 **Possible Questions**
- Why not implement all four relational operators separately?
- How does implementing `<=` in terms of `>` improve maintainability?
- Can relational operators be overloaded as non‑member functions?
- How does operator precedence affect relational expressions?

---

## 10. Code from the video

### Length.h

```cpp
#ifndef ADVANCED_LENGTH_H
#define ADVANCED_LENGTH_H

class Length {
public:
    explicit Length(int value);

    bool operator==(const Length& other) const;
    bool operator!=(const Length& other) const;
    bool operator==(int other) const;
    bool operator!=(int other) const;
    bool operator<(const Length& other) const;
    bool operator<=(const Length& other) const;
    bool operator>(const Length& other) const;
    bool operator>=(const Length& other) const;
    
private:
    int value;
};

#endif
```

### Length.cpp

```cpp
#include "Length.h"

Length::Length(int value) : value(value) {}

bool Length::operator==(const Length& other) const {
    return value == other.value; 
}

bool Length::operator!=(const Length& other) const {
    return !(value == other.value);
}

bool Length::operator==(int other) const {
    return value == other;
}

bool Length::operator!=(int other) const {
    return !(value == other);
}

bool Length::operator<(const Length& other) const {
    return value < other.value;
}

bool Length::operator<=(const Length& other) const {
    return !(value > other.value);
}

bool Length::operator>(const Length& other) const {
    return value > other.value;
}

bool Length::operator>=(const Length& other) const {
    return !(value < other.value);
}
```

### main.cpp

```cpp
#include "Length.h"

#include <iostream>

int main() {
    Length first{10};
    Length second{10};

    if (first < second)

    return 0;
}
```