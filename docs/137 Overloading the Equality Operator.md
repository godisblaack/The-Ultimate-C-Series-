# 137 Overloading the Equality Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/Vc1r-sbfiwM?si=h3wNvYh3BVxulqTQ"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Overview
- **Goal:** Allow built‑in operators (`==`, `!=`) to work with user‑defined types.
- **Why:** Improves readability and makes objects behave like built‑in types.
- **Key Idea:** `operator==` is just another function — the compiler rewrites `a == b` as `a.operator==(b)`.

---

## 2. Equality Operator (`==`)

### Declaration
```cpp
bool operator==(const Length& other) const;
```
- **`const Length& other`** → pass by const reference for efficiency.
- **`const` at end** → guarantees no modification of the current object.

### Definition
```cpp
bool Length::operator==(const Length& other) const {
    return value == other.value;
}
```

---

## 3. Overloading for Different Types
- You can overload `operator==` to compare with **primitive types**:
```cpp
bool operator==(int other) const {
    return value == other;
}
```
- For primitives, pass **by value** (no performance gain from references).

---

## 4. Inequality Operator (`!=`)
- Modern compilers can auto‑generate `!=` from `==`.
- If not, implement in terms of `==`:

```cpp
bool operator!=(const Length& other) const {
    return !(value == other.value);
}
```

Mosh is comparing two integers directly in the above implementation, so in future if there is any change in the logic of comparision for operator== then he has to make changes in several places. To avoid this we can use the implementation below:

```cpp
bool operator!=(const Length& other) const {
    return !(*this == other);
}
// This will call the overloaded operator==(const Length& other) const
```

Similarly,

```cpp
bool operator!=(int other) const {
    return !(value == other);
}
```

As explained before, we should use the implementation below: 

```cpp
bool operator!=(int other) const {
    return !(*this == other);
}
// This will call the overloaded operator==(int other) const
```

- This avoids duplicating comparison logic.

---

## 5 Understanding `==` Calls, Function Syntax, and Operator Precedence

### 5.1. `if (first == second)`
- **Without operator overloading**: The compiler doesn’t know how to compare two `Length` (or `Point`) objects — there’s no built‑in rule for that.
- **With `operator==` defined**: The compiler rewrites this as:
  ```cpp
  if (first.operator==(second))
  ```
  This is why overloading `==` makes the comparison possible.

---

### 5.2. `if (first.operator==(second))`
- This is the **explicit function call form** of the overloaded operator.
- `operator==` is **just a member function** with a special name — the `a == b` syntax is syntactic sugar.
- Both forms are equivalent:
  ```cpp
  first == second;           // Preferred, cleaner
  first.operator==(second);  // Verbose, rarely used
  ```

---

### 5.3. `if (first = 10)`
- **⚠️ Common mistake**: This uses the **assignment operator** (`=`), not equality (`==`).
- Unless you’ve overloaded `operator=` to take an `int`, this will either:
  - Assign `10` to `first` (if such an overload exists), or
  - Fail to compile (if no matching assignment operator exists).
- To compare with `10`, you must write:
  ```cpp
  if (first == 10) // Requires operator==(int) overload
  ```

---

### 5.4. `cout << (first == second) << endl;`
- **Why parentheses?**  
  The stream insertion operator `<<` has **higher precedence** than `==`.
- Without parentheses:
  ```cpp
  cout << first == second; // Interpreted as (cout << first) == second → likely a compile error
  ```
- With parentheses:
  ```cpp
  cout << (first == second) << endl; // Correct: evaluates comparison first, then outputs result
  ```

---

## 6. Example – `Length` Class

### Length.h
```cpp
class Length {
    public:
    explicit Length(int value);

    bool operator==(const Length& other) const;
    bool operator!=(const Length& other) const;
    bool operator==(int other) const;
    bool operator!=(int other) const;

private:
    int value;
};
```

### Length.cpp
```cpp
Length::Length(int value) : value(value) {}

bool Length::operator==(const Length& other) const {
    return value == other.value;
}

bool Length::operator!=(const Length& other) const {
    return !(*this == other);
}

bool Length::operator==(int other) const {
    return value == other;
}

bool Length::operator!=(int other) const {
    return !(*this == other);
}
```

---

## 7. Exercise – `Point` Class

### Point.h
```cpp
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
```

### Point.cpp
```cpp
Point::Point(int x, int y) : x{x}, y{y} {}

bool Point::operator==(const Point& other) const {
    return (x == other.x) && (y == other.y);
}
```

---

## 8. Mental Model
```
Length
 ├── private: int value
 ├── public:
 │    ├── operator==(const Length&) const
 │    ├── operator!=(const Length&) const
 │    ├── operator==(int) const
 │    └── operator!=(int) const
 └── Equality logic centralized in operator==
```
- `!=` simply **negates** the result of `==`.

---

## 9. Quick Revision Sheet
- **Always** mark comparison operators `const`.
- Pass user‑defined types by `const&`, primitives by value.
- Implement `!=` in terms of `==` to avoid duplication.
- Overload for multiple parameter types if needed.
- Remember: `operator==` is just a function — `a == b` → `a.operator==(b)`.

---

### 10. Quick Takeaways
- **Operator overloading** lets you use natural syntax (`==`) with custom types.
- The compiler simply calls a specially‑named function (`operator==`).
- Always be mindful of **operator precedence** when mixing `<<` with comparisons.
- Avoid accidental `=` vs `==` mix‑ups — they mean very different things.

---

## 11. Interview Insights
💬 **Possible Questions**
- Why pass objects by `const&` in operator overloads?
- How to avoid code duplication between `==` and `!=`?
- Can you overload `==` for different parameter types?
- What happens if you don’t mark `operator==` as `const`?

---

## 12. Code from the video

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
```

### main.cpp

```cpp
#include "Length.h"

#include <iostream>

int main() {
    Length first{10};
    Length second{10};

    if (first == second) // We cannot do this type of comparision without operator overloading

    if (first.operator==(second)) // This is as same as the condition in the "if" above. It is same as calling a function, we can do it beacuse operator== is just an another function.

    if (first == 10) // Overloading operator== again

    if (first != 10)

    return 0;
}
```

## 13. Exercise

### Point.h

```cpp
#ifndef ADVANCED_POINT_H
#define ADVANCED_POINT_H

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
```

### main.cpp

```cpp
#include "Point.h"

#include <iostream>

using namespace std;

int main() {
    Point first{10, 20};
    Point second{10, 20};

    cout << (first == second) << endl; // Priority of << is greater than ==, so we need parentheses.

    return 0;
}
```