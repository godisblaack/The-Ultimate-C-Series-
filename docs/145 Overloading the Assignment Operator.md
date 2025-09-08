# 145 Overloading the Assignment Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/NSr17u-sR1k?si=KzHLL-VSq7QG3Q8p"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Overload the Assignment Operator?

### 🔄 Default Behavior
- By default, the compiler generates an **assignment operator** that **copies all member variables** from the source object to the target object.
- Works fine in most cases, but:
  - Sometimes you need **custom logic** (e.g., deep copying, logging, resource management).
  - Sometimes you want to **disable assignment** entirely.

### 🧠 Key Distinction
- **Copy Constructor**: Called when **initializing** a new object from another.
- **Assignment Operator**: Called when **assigning** to an **existing** object.

---

## 2. Overloading the Assignment Operator

### 📜 Function Signature
- Return type: `Length&` (reference to current object).
- Parameter: `const Length&` (pass by const reference).
- Not `const` — modifies the current object.

Example:
```cpp
Length& operator=(const Length& other);
```

---

## 3. Defining the Assignment Operator

### 🛠 Implementation
```cpp
Length& Length::operator=(const Length& other) {
    cout << "Object assigned" << endl;
    value = other.value; // Copy members
    return *this;        // Dereference `this` pointer
}
```

### 🔍 Key Points
- `this` is a pointer to the current object.
- `*this` dereferences it to return the actual object.
- Returning a reference allows **chaining**:
  ```cpp
  a = b = c;
  ```

---

## 4. Updated `Length` Class (Overloaded Assignment)

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
    Length& operator=(const Length& other);

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

Length& Length::operator+=(const Length& other) {
    value += other.value;
    return *this;
}

Length& Length::operator=(const Length& other) {
    cout << "Object assigned" << endl;
    value = other.value;
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

    // Copy constructor (new object)
    Length second = first;

    // Assignment operator (existing object)
    second = first;

    return 0;
}
```

---

## 5. Deleting the Copy Constructor and Assignment Operator

### 🚫 Why Delete?
- To prevent copying or assigning objects (e.g., for unique resources).
- Avoids **inconsistency**: If you delete one, delete the other.

### Length.h
```cpp
class Length {
public:
    Length() = default;
    explicit Length(int value);
    Length(const Length& other) = delete;            // No copy constructor
    Length& operator=(const Length& other) = delete; // No assignment

    // ... other members
};
```

### Length.cpp
```cpp
Length& Length::operator=(const Length& other) {
    cout << "Object assigned" << endl;
    value = other.value;
    return *this;
}
```
- Delete this function as well.

---

## 6. Behavior Difference

### 📌 Example
```cpp
Length first{10};

// Copy constructor → called when creating a new object
Length second = first; // Error if copy constructor deleted

// Assignment operator → called when assigning to an existing object
second = first;        // Error if assignment operator deleted
```

---

## 7. Interview Insights

### 💬 Common Questions
- Difference between copy constructor and assignment operator?
- Why return `*this` by reference?
- When should you delete both copy constructor and assignment operator?
- How does compiler-generated assignment work?

### 🧠 Mental Model
```
=  (Equal Sign)
 ├── Initialization → Copy Constructor
 └── Assignment     → Assignment Operator
```

---

## 8. Quick Revision Sheet

- **Copy Constructor**: Initializes a new object from another.
- **Assignment Operator**: Assigns to an existing object.
- Return `*this` by reference for chaining.
- Delete both copy constructor and assignment operator to prevent copying.
- Compiler auto-generates both if not defined.

---

## 9. Best Practices

- Always check if you need **custom copy/assignment** (deep copy, logging, etc.).
- Keep signatures consistent: `Type& operator=(const Type& other)`.
- Avoid self-assignment issues in complex classes (check `if (this != &other)`).
- If deleting one, delete the other for consistency.
- Understand the **Rule of Three/Five** in C++.

---

## 10. Code from the video

### Part - 1 - Overloading assignment operator

#### Length.h

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
    Length& operator=(const Length& other);

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

#### Length.cpp

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

#### main.cpp

```cpp
#include "Length.h"

#include <iostream>

using namespace std;

int main() {
    Length first{10};

    // Copy constructor (New)
    Length second = first;
    
    // Assignment operator (Existing)
    second = first;

    return 0;
}
```

### Part - 2 - Deleting the copy constructor and assignment operator

#### Length.h

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
    Length(const Length& other) = delete;

    bool operator==(const Length& other) const;
    bool operator==(int other) const;
    strong_ordering operator<=>(const Length& other) const;
    Length operator+(const Length& other) const;
    Length& operator+=(const Length& other);
    Length& operator=(const Length& other) = delete;

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

#### Length.cpp

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

#### main.cpp

```cpp
#include "Length.h"

#include <iostream>

using namespace std;

int main() {
    Length first{10};

    // Copy constructor (New) - Deleted
    Length second = first; // Compilation error
    
    // Assignment operator (Existing)
    second = first; // Compilation error

    return 0;
}
```