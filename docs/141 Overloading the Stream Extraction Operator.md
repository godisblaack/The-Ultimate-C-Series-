# 141 Overloading the Stream Extraction Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/_V7hbKEOwZ8?si=QwDsV6t7jQpehGWu"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Overview
- **Purpose:** Read data from an input stream into your object.
- **Analogy:** Just like `cin >> x;` works for built‑in types, we make it work for custom types.
- **Key Rule:** Must be a **non‑member** function if the left operand is not your class type.

---

## 2. Function Signature
```cpp
istream& operator>>(istream& stream, Length& length);
```
- **Return type:** `istream&` → allows chaining (`cin >> a >> b;`).
- **First parameter:** `istream&` → the input stream to read from.
- **Second parameter:** non‑const reference to your object → we need to modify it.

---

## 3. Implementation Steps
1. Declare a temporary variable to hold the incoming value.
2. Use `stream >> temp;` to read from the input.
3. Call the object’s setter to update its state.
4. Return the stream to enable chaining.

---

## 4. Example – `Length` Class

### Length.h
```cpp
istream& operator>>(istream& stream, Length& length);
```

### Length.cpp
```cpp
istream& operator>>(istream& stream, Length& length) {
    int value;
    stream >> value;
    length.setValue(value);
    return stream;
}
```

---

## 5. Chaining Explained
```cpp
cin >> length1 >> length2;
```
- First call: `operator>>(cin, length1)` returns `cin`.
- Second call: `operator>>(cin, length2)` runs on the returned `cin`.

---

## 6. Mental Model

```
std::istream (e.g., cin)
        │
        ▼
operator>>(istream& stream, Length& obj)   // non-member
        │
        ├─ Reads raw data from 'stream' into a temp variable
        ├─ Uses obj's public setter (or friend access) to update state
        │
        └─ Returns 'stream' by reference
                │
                ▼
        Enables chaining: cin >> a >> b;
```

**Key points in the model:**
- **Non‑member function**:  
  - Left operand is `istream`, so the operator must be defined outside your class.
  - Can be a **friend** if it needs direct access to private members instead of using setters.
- **Return `istream&`**:  
  - Allows the returned stream to be used for the next `>>` in a chain.
- **Access pattern**:  
  - Reads into a temporary variable → updates object state.
- **Execution flow in chaining**:
  1. `cin >> first` → calls `operator>>(cin, first)` → returns `cin`.
  2. Returned `cin` is fed into `operator>>(cin, second)`.

**Mental shortcut:**  
Think of `>>` as a **data loader** that:
1. Pulls data from the stream.
2. Injects it into your object.
3. Hands the same stream back so more data can be loaded.

---

## 7. Quick Revision Sheet
- Always return `istream&` for chaining.
- Pass object by non‑const reference.
- Must be non‑member if left operand isn’t your class.
- Use setters to update private data.

---

## 8. Interview Insights
💬 **Possible Questions**
- Why must `>>` be a non‑member for `istream`?
- Why return `istream&` instead of `void`?
- How does chaining work internally?
- How would you handle invalid input or stream errors?

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

    int getValue() const;
    void setValue(int value);
    
private:
    int value;
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

ostream& operator<<(ostream& stream, const Length& length) {
    stream << length.getValue();

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
    
    cout << "Length: ";

    cin >> length;

    cout << length;

    return 0;
}
```