# 178 A More Complex Class Template

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/laGS2oz2zIc?si=nk5NdJ5qli85pyoT"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Context

* Earlier → we built an `Array` class (with operator overloading).
* Limitation: It could **only store `int` values**.
* Now with templates → we can make it **generic**, so it can store:

  * Integers
  * Strings
  * Custom objects (`Pair<string, int>`, etc.)

---

## 2. Solution → Class Template for Array

* Add `template<typename T>` above the class.
* Replace all occurrences of `int` with **`T`**.
* Ensure operator\[] returns a **reference of type T (`T&`)**.
* Memory allocated dynamically (`new T[size]`).
* Destruction handled with `delete[]`.

---

## 3. Code Walkthrough

### Array.h → Generic Array

```cpp
template<typename T>
class Array {
public:
    explicit Array(size_t size);
    ~Array();
    
    T& operator[](size_t index);

private:
    T* values;
    size_t size;
};

template<typename T>
Array<T>::Array(size_t size) {
    values = new T[size];
    this->size = size;
}

template<typename T>
Array<T>::~Array() {
    delete[] values;
}

template<typename T>
T& Array<T>::operator[](size_t index) {
    if (index >= size) {
        throw std::invalid_argument("index");
    }
    return values[index];
}
```

* Constructor → allocates memory for `T[size]`.
* Destructor → cleans up dynamically allocated memory.
* `operator[]` → returns `T&`, with bounds checking.

---

### Using Array with Integers

```cpp
Array<int> array{10};
array[0] = 1;
cout << array[0] << endl;
```

✔ Works with integers.

---

### Using Array with Strings

```cpp
Array<string> array{10};
array[0] = "Hello World";
cout << array[0] << endl;
```

✔ Works with `std::string`.

---

### Using Array with Pair Objects

```cpp
Array<Pair<string, int>> array{10};
array[0] = {"a", 1};
cout << array[0] << endl;  // ❌ Compilation error
```

* Error: Compiler doesn’t know how to print a `Pair`.
* Solution: Overload `operator<<` for `Pair`.

---

## 4. Improving Pair Class

### Add Default Constructor

* Required because `Array` creates empty `Pair` elements before assigning.

```cpp
Pair() = default;
Pair(K key, V value);
```

---

### Add Operator<<

```cpp
template<typename K, typename V>
ostream& operator<<(ostream& os, const Pair<K, V>& p) {
    return os << p.getKey() << "=" << p.getValue();
}
```

✔ Now `cout << array[0];` works properly.

---

## 5. Visual Mental Model

```
         Array<T>
     ┌───────────────────┐
     │ values: T*        │
     │ size: size_t      │
     └───────────────────┘
            |
            v
    ┌─────────────┬─────────────┐
    │ Array<int>  │ Array<string> 
    │ Array<Pair> │ ...
```

* Compiler generates code only for **types we use**.

---

## 6. Best Practices & Insights

* ✅ Keep method definitions in the header file (same reason as Pair).
* ✅ Provide a **default constructor** for template classes used inside arrays.
* ✅ When using custom types with streams (`cout`), implement `operator<<`.
* ✅ Consolidate public/private sections for clean class design.

---

## 7. Interview Insights

* Q: *Why do we need a default constructor for `Pair` in this case?*

  * A: Because `Array` pre-allocates its elements with `new T[size]`. The compiler must know how to default-construct `Pair`.

* Q: *What happens if `operator<<` is not defined for a custom type used in `cout`?*

  * A: Compilation error → `ostream` doesn’t know how to print the object.

* Q: *How does a class template differ from inheritance for making generic arrays?*

  * A: Templates generate type-safe, compile-time specializations, unlike base-class polymorphism which uses runtime behavior.

---

## 8. Quick Revision Sheet

* Template declaration: `template<typename T>`
* Array class stores `T* values;`
* Constructor → `values = new T[size];`
* Destructor → `delete[] values;`
* Operator\[] → returns `T&` with bounds checking.
* For `Array<Pair<>>` → must define **default constructor** + `operator<<`.

---

## 10. Code from the video

### Array.h

```cpp
#ifndef ADVANCED_ARRAY_H
#define ADVANCED_ARRAY_H

#include <cstddef>
#include <stdexcept>

template<typename T>
class Array {
public:
    explicit Array(size_t size);
    ~Array();
    
    T& operator[](size_t index);

private:
    T* values;
    size_t size;
};

template<typename T>
Array<T>::Array(size_t size) {
    values = new T[size];

    this->size = size;
}

template<typename T>
Array<T>::~Array() {
    delete[] values;
}

template<typename T>
T& Array<T>::operator[](size_t index) {
    if (index >= size) {
        throw std::invalid_argument("index");
    }

    return values[index];
}

#endif
```

### Pair.h

```cpp
#ifndef ADVANCED_PAIR_H
#define ADVANCED_PAIR_H

template<typename K, typename V>
class Pair {
public:
    Pair() = default;
    Pair(K key, V value);

    K getKey() const;
    V getValue() const;

private:
    K key;
    V value;
};

template<typename K, typename V>
Pair<K, V>::Pair(K key, V value) : key(key), value(value) {}

template<typename K, typename V>
K Pair<K, V>::getKey() const {
    return key;
}

template<typename K, typename V>
V Pair<K, V>::getValue() const {
    return value;
}

#endif
```

### main.cpp

#### Integer type

```cpp
#include "Array.h"

#include <iostream>

using namespace std;

int main() {
    Array<int> array{10};

    array[0] = 1;
    
    cout << array[0] << endl;

    return 0;
}
```

#### String type

```cpp
#include "Array.h"

#include <iostream>
#include <string>

using namespace std;

int main() {
    Array<string> array{10};

    array[0] = "Hello World";
    
    cout << array[0] << endl;

    return 0;
}
```

#### Pair type

```cpp
#include "Array.h"
#include "Pair.h"

#include <iostream>
#include <string>

using namespace std;

int main() {
    Array<Pair<string, int>> array{10};

    array[0] = {"a", 1};
    
    cout << array[0] << endl; // Compilation error

    return 0;
}
```

## 11. Exercise

### Pair.h

```cpp
#ifndef ADVANCED_PAIR_H
#define ADVANCED_PAIR_H

#include <ostream>

using namespace std;

template<typename K, typename V>
class Pair {
public:
    Pair() = default;
    Pair(K key, V value);

    K getKey() const;
    V getValue() const;

private:
    K key;
    V value;
};

template<typename K, typename V>
Pair<K, V>::Pair(K key, V value) : key(key), value(value) {}

template<typename K, typename V>
K Pair<K, V>::getKey() const {
    return key;
}

template<typename K, typename V>
V Pair<K, V>::getValue() const {
    return value;
}

template<typename K, typename V>
ostream& operator<<(ostream& os, const Pair<K, V>& p) {
    return os << p.getKey() << "=" << p.getValue();
}

#endif
```