# 177 Defining a Class Template

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/B-DTHzcNHGE?si=AO4NjNsYsvKUhziL"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Context

* In earlier lessons:

  * We used `numeric_limits<int>::max()` → behind the scenes, `numeric_limits` is a **class template**.
* Idea:

  * Functions aren’t the only things we can make **generic**.
  * Classes can also be **parameterized with types**.
* Example: A `Pair` class (key–value pair).

  * Without templates → fixed to `string key` and `int value`.
  * Limitation → not reusable with other types.

---

## 2. Solution → Class Templates

* Define a class with **type parameters**:

  ```cpp
  template<typename K, typename V>
  class Pair { … };
  ```

* Allows creating pairs of any type (`Pair<string, int>`, `Pair<int, double>`, …).

* **Compiler generates code only for used types** → like function templates.

---

## 3. Code Walkthrough

### Step 1 → Basic (Non-Template) Implementation

```cpp
class Pair {
private:
    string key;
    int value;
};
```

* Hardcoded to `string` + `int`.
* Not flexible.

---

### Step 2 → Template Implementation

```cpp
template<typename K, typename V>
class Pair {
public:
    Pair(K key, V value);
    K getKey() const;
    V getValue() const;
private:
    K key;
    V value;
};
```

* `K` → type of key.
* `V` → type of value.
* **Generalized** to work with any type.

---

### Step 3 → Defining Methods Outside the Class

```cpp
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
```

* Each method must be **preceded by the template declaration**.
* Use `Pair<K, V>::methodName` to define.

---

### Step 4 → Usage in `main.cpp`

```cpp
#include "Pair.h"
#include <iostream>
#include <string>
using namespace std;

int main() {
    Pair<string, int> pair{"a", 1};
    cout << pair.getKey() << "=" << pair.getValue() << endl;
    return 0;
}
```

* Compiler generates a specialized version:

  * `K = string`
  * `V = int`

---

## 4. Key Detail → Method Definitions in Class Templates

⚠️ Limitation:

* With class templates, **method definitions must remain in the header file**.
* If placed in a `.cpp` file → linker errors (compiler won’t generate template instances).
* Workarounds exist but are messy → avoided in practice.

✅ Best practice:

* Put declarations at the top, definitions at the bottom of the same header file.

---

## 5. Visual Mental Model

```
        Pair<K, V>
        ┌───────────┐
   Key: │     K     │  e.g., string, int, double
 Value: │     V     │  e.g., int, double, string
        └───────────┘

Examples:
Pair<string, int>
Pair<int, double>
Pair<string, string>
```

---

## 6. Best Practices & Insights

* ✅ Use class templates when you need **reusable containers or structures**.
* ✅ Keep implementation in the header file.
* ✅ Name template parameters meaningfully (`K`, `V` for key/value).
* ✅ Compiler generates only needed specializations → efficient.

---

## 7. Interview Insights

* Q: *What’s the difference between function templates and class templates?*

  * A: Functions → generic behavior; Classes → generic data structures.

* Q: *Why must method definitions stay in the header file?*

  * A: Because the compiler needs the full definition at instantiation time to generate code.

* Q: *Where do you see class templates in the STL?*

  * A: `std::pair<K, V>`, `std::vector<T>`, `std::map<K, V>`, `std::unique_ptr<T>`.

---

## 8. Quick Revision Sheet

* Define:

  ```cpp
  template<typename K, typename V>
  class Pair {
      Pair(K key, V value);

      K key;
      V value;
  };
  ```

* Methods:

  ```cpp
  template<typename K, typename V>
  K Pair<K, V>::getKey() const { return key; }
  ```

* Usage:

  ```cpp
  Pair<string, int> p{"id", 100};
  ```

---

## 10. Code from the video

### Pair.h

#### Basic implementation

```cpp
#ifndef ADVANCED_PAIR_H
#define ADVANCED_PAIR_H

#include <string>

using namespace std;

class Pair {
private:
    string key;
    int value;
};

#endif
```

#### Template implementation

```cpp
#ifndef ADVANCED_PAIR_H
#define ADVANCED_PAIR_H

template<typename K, typename V>
class Pair {
public:
    Pair(K key, V value);

    K getKey() const;
    V getValue() const;

// Inline definition -> Not recommended
//     Pair(K key, V value) : key(key), value(value) {}
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

```cpp
#include "Pair.h"

#include <iostream>
#include <string>

using namespace std;

int main() {
    Pair pair{"a", 1};

    return 0;
}
```