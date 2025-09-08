# 139 Overloading the Spaceship Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/FePajOz7HRA?si=FNjOyj9eNS86SS-F"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>



---

## 1. Concept Overview
- **Purpose:** Compare two objects in a single operation and determine if one is less, equal, or greater.
- **Why:**  
  - Reduces multiple comparisons to one.
  - Lets the compiler auto‑generate all relational operators (`<`, `>`, `<=`, `>=`, `!=`) when combined with `operator==`.
- **Nickname:** Spaceship or three‑way comparison operator — looks like `<=>`.

---

## 2. How It Works
- Returns a **comparison category type** (e.g., `std::strong_ordering`).
- Possible results:
  - `strong_ordering::less`
  - `strong_ordering::equal`
  - `strong_ordering::greater`
  - `strong_ordering::equivalent` (for some types)
- Example:
```cpp
auto result = x <=> y;
if (result == std::strong_ordering::less) { 
    // This is as same as doing x < y 
} else if (result == std::strong_ordering::greater) { 
    // This is as same as doing x > y 
} else { 
    // This is as same as doing x = y 
}
```

---

## 3. Benefits
- **Single comparison** instead of multiple `if` checks.
- **Performance**: Potentially cheaper for complex objects.
- **Maintainability**: Only need to implement `operator<=>` and `operator==`.

---

## 4. Using `<=>` in Classes

### Declaration
```cpp
#include <compare>

class Length {
public:
    explicit Length(int value);

    bool operator==(const Length& other) const;
    bool operator==(int other) const;
    std::strong_ordering operator<=>(const Length& other) const;

private:
    int value;
};
```

### Definition
```cpp
Length::Length(int value) : value(value) {}

bool Length::operator==(const Length& other) const {
    return value == other.value;
}

bool Length::operator==(int other) const {
    return value == other;
}

std::strong_ordering Length::operator<=>(const Length& other) const {
    return value <=> other.value;
}
```

---

## 5. Compiler‑Generated Operators
When you define:
- `operator==`
- `operator<=>`

…the compiler will auto‑generate:
- `<`, `>`, `<=`, `>=`, `!=`

---

## 6. Example Usage
```cpp
Length first{10};
Length second{20};

if (first < second) {
    std::cout << "First is smaller\n";
}
```

---

Here’s the **mental model** for the C++20 `<=>` (spaceship) operator in the same style as your other notes — so it’s quick to visualize and recall during interviews or revision.

---

## 7. Mental Model

```
Length
 ├── private: int value
 ├── public:
 │    ├── operator==(const Length&) const        // equality check
 │    ├── operator==(int) const                  // equality with primitive
 │    └── operator<=>(const Length&) const       // three-way comparison
 │
 ├── operator<=> returns: std::strong_ordering
 │       ├── less       → behaves like <
 │       ├── equal      → behaves like ==
 │       └── greater    → behaves like >
 │
 └── Compiler auto-generates:
        <   >   <=   >=   !=
        (based on operator== and operator<=>)
```

**Flow of comparison logic:**
1. **You** implement:
   - `operator==` → defines equality.
   - `operator<=>` → defines ordering.
2. **Compiler** uses these two to synthesize:
   - All other relational operators.
3. **Usage**:
   - `a < b` → calls `<=>` internally, checks for `less`.
   - `a > b` → calls `<=>`, checks for `greater`.
   - `a != b` → calls `operator==` and negates result.

**Mental shortcut:**  
Think of `<=>` as a **single “comparison engine”** that feeds all other relational operators, with `operator==` as the equality gatekeeper.

---

## 8. Quick Revision Sheet
- `<=>` returns a comparison category type, not `bool`.
- Include `<compare>` for `std::strong_ordering`.
- Implement `operator==` alongside `<=>` for full comparison support.
- Compiler auto‑generates other relational operators.

---

## 9. Interview Insights
💬 **Possible Questions**
- What does `<=>` return and why isn’t it `bool`?
- How does `<=>` improve maintainability?
- What’s the difference between `strong_ordering`, `weak_ordering`, and `partial_ordering`?
- Why do you still need `operator==` when using `<=>`?

---

## 10. Code from the video

### main.cpp

```cpp
#include <iostream>

int main() {
    int x = 10;
    int y = 20;

    auto result = x <=> y;

    if (result == strong_ordering::less) { 
        // This is same as doing x < y
    } else if (result == strong_ordering::greater) { 
        // This is as same as doing x > y
    } else {
        // This is as same as doing x = y
    }

    return 0;
}
```

### Length.h

```cpp
#ifndef ADVANCED_LENGTH_H
#define ADVANCED_LENGTH_H

#include <compare>

class Length {
public:
    explicit Length(int value);

    bool operator==(const Length& other) const;
    bool operator==(int other) const;
    std::strong_ordering operator<=>(const Length& other) const;
    
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

bool Length::operator==(int other) const {
    return value == other;
}

std::strong_ordering Length::operator<=>(const Length& other) const {
    return value <=> other.value;
}
```

### main.cpp

```cpp
#include "Length.h"

#include <iostream>

using namespace std;

int main() {
    Length first{10};
    Length second{20};

    if (first < second) {
        cout << "First is smaller" << endl;
    }

    return 0;
}
```