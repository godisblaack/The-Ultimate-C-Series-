# 147 Overloading the Subscript Operator

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/TDGk1ooEK3c?si=M7TKDCXO4nSy_4jp"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Why Overload the Subscript Operator?

### 🔍 Purpose
- The **subscript operator** (`[]`) allows **array-like access** to elements in a custom class.
- Useful for:
  - Classes that **wrap arrays**.
  - **Custom containers** (e.g., dynamic arrays, matrices).
- Example:
  ```cpp
  array[0] = 42;
  cout << array[0];
  ```

---

## 2. Function Signature

### 📜 Syntax
```cpp
int& operator[](size_t index);
```
- **Return type:** `int&` → reference to the element (allows both read & write).
- **Parameter:** `size_t index` → supports large indices, matches standard library conventions.
- **Not `const`** → modifies the element.

---

## 3. Implementation Steps

### 🛠 Core Logic
1. **Validate index**:
   - If `index >= size`, throw `std::invalid_argument("index")`.
2. **Return reference** to the element:
   - `return values[index];`

---

## 4. Updated `Array` Class

### Array.h
```cpp
#ifndef ADVANCED_ARRAY_H
#define ADVANCED_ARRAY_H

#include <cstddef> // for size_t

class Array {
public:
    explicit Array(size_t size);
    ~Array();
    
    int& operator[](size_t index);

private:
    int* values;
    size_t size;
};

#endif
```

### Array.cpp
```cpp
#include "Array.h"
#include <stdexcept> // for std::invalid_argument

Array::Array(size_t size) {
    values = new int[size]; // allocate dynamic array
    this->size = size;
}

Array::~Array() {
    delete[] values; // free memory
}

int& Array::operator[](size_t index) {
    if (index >= size) {
        throw std::invalid_argument("index");
    }
    return values[index];
}
```

---

## 5. Example Usage

### main.cpp
```cpp
#include "Array.h"
#include <iostream>
using namespace std;

int main() {
    Array array{10};

    array[0] = 1; // set first element
    cout << array[0] << endl; // get first element

    // Uncomment to test exception
    // cout << array[10] << endl; // throws invalid_argument

    return 0;
}
```

---

## 6. Key Points

- **Dynamic Allocation**:
  - Use `new[]` in constructor.
  - Use `delete[]` in destructor.
- **Bounds Checking**:
  - Prevents undefined behavior.
  - Throws exception for invalid index.
- **Return by Reference**:
  - Allows both reading and writing:
    ```cpp
    array[0] = 5;     // write
    int x = array[0]; // read
    ```

---

## 7. Interview Insights

### 💬 Common Questions
- Why return by reference instead of by value?
- Why use `size_t` for the index?
- How to handle out-of-bounds access?
- How would you implement a `const` version of `operator[]`?

### 🧠 Mental Model
```
Array
 ├── values[] (dynamic memory)
 ├── size
 └── operator[](index)
      ├── Validate index
      └── Return reference to element
```

---

## 8. Quick Revision Sheet

- Use `size_t` for index parameters.
- Always perform **bounds checking**.
- Return **reference** for read/write access.
- Free allocated memory in destructor.
- Consider adding a **const overload**:
  ```cpp
  const int& operator[](size_t index) const;
  ```

---

## 9. Best Practices

- Implement both **non-const** and **const** versions of `operator[]`.
- Avoid memory leaks — follow **Rule of Three/Five** if class manages resources.
- Throw meaningful exceptions for invalid access.
- Prefer `std::vector` in production — custom array is for learning/demo purposes.

---

## 10. Code from the video

### Array.h

```cpp
#ifndef ADVANCED_ARRAY_H
#define ADVANCED_ARRAY_H

#include <cstddef>

class Array {
public:
    explicit Array(size_t size);
    ~Array();
    
    int& operator[](size_t index);

private:
    int* values;
    size_t size;
};

#endif
```

### Array.cpp

```cpp
#include "Array.h"

#include <stdexcept>

Array::Array(size_t size) {
    values = new int[size];

    this->size = size;
}

Array::~Array() {
    delete[] values;
}

int& Array::operator[](size_t index) {
    if (index >= size) {
        throw std::invalid_argument("index");
    }

    return values[index];
}
```

### main.cpp

```cpp
#include "Array.h"

#include <iostream>

using namespace std;

int main() {
    Array array{10};

    array[0] = 1;
    
    cout << array[0] << endl;

    return 0;
}
```