# 176 Templates with Multiple Parameters

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/7moNzLLTELg?si=vQ8d_g4LpTAX8Eci"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Context

* A **single-type template parameter** (`T`) works when all arguments are of the same type.
* But sometimes, we need **different types** for parameters.

  * Example: A `key–value pair`.
  * Key could be a `string`, `int`, or other type.
  * Value could be `int`, `double`, or `string`.
* Without templates → we’d need multiple overloaded versions.

---

## 2. Solution → Multiple Template Parameters

* Use **two type parameters** in the template declaration:

  ```cpp
  template<typename K, typename V>
  ```

  * `K` = Key type (descriptive name).
  * `V` = Value type.
* Replace fixed types (`string`, `int`) with template parameters.

---

## 3. Code Walkthrough

### Step 1 → Basic (Non-Template) Implementation

```cpp
void display(string key, int value) {
    cout << key << "=" << value << endl;
}
```

* Works only with `(string, int)` pairs.
* Not flexible → fails with `(int, int)` or `(string, double)`.

---

### Step 2 → Template Implementation

```cpp
template<typename K, typename V>
void display(K key, V value) {
    cout << key << "=" << value << endl;
}
```

* Now works with **any types** that support the `<<` operator.

---

### Full Example

```cpp
#include <iostream>
using namespace std;

template<typename T>
T larger(T first, T second) {
    return (first > second) ? first : second;
}

template<typename K, typename V>
void display(K key, V value) {
    cout << key << "=" << value << endl;
}

int main() {
    display("a", 1);       // K = const char*, V = int
    display(1, 1);         // K = int, V = int
    display("pi", 3.14);   // K = const char*, V = double

    return 0;
}
```

---

## 4. Visual Mental Model

```
         ┌───────────────┐
         │ display(K,V)  │
         └───────┬───────┘
                 │
   ┌─────────────┼────────────────┐
   │             │                │
K = string   K = int          K = const char*
V = int      V = int          V = double
```

* At compile time, `K` and `V` get replaced with **actual types** based on function arguments.

---

## 5. Best Practices & Insights

* ✅ Use **descriptive template parameter names** (`K`, `V`) instead of generic (`T`, `U`) when the role is clear.
* ✅ Templates allow **type flexibility** without needing multiple overloads.
* ✅ Function only compiles if the types support required operations (e.g., `<<` insertion here).
* ⚠️ Passing unsupported types → compilation error, not runtime error.

---

## 6. Interview Insights

* Q: *Why use multiple template parameters instead of overloading?*

  * A: Reduces code duplication, more generic, easier to maintain.

* Q: *What happens if a type doesn’t support the operator used in the template?*

  * A: Compilation error → template instantiation fails.

* Q: *Why are descriptive parameter names (`K`, `V`) preferred?*

  * A: Improves readability, especially when parameters have semantic roles (like key–value pairs).

---

## 7. Quick Revision Sheet

* Single type parameter:

  ```cpp
  template<typename T>
  T larger(T a, T b);
  ```

* Multiple type parameters:

  ```cpp
  template<typename K, typename V>
  void display(K key, V value);
  ```

* Usage:

  ```cpp
  display("a", 1);       // K = const char*, V = int
  display(42, "hi");     // K = int, V = const char*
  display("pi", 3.14);   // K = const char*, V = double
  ```

---

## 8. Code from the video

### main.cpp

#### Basic implementation

```cpp
#include <iostream>

using namespace std;

template<typename T>
T larger(T first, T second) {
    return (first > second) ? first : second;
}

void display(string key, int value) {
    cout << key << "=" << value << endl;
} 

int main() {
    

    return 0;
}
```

#### Template implementation

```cpp
#include <iostream>

using namespace std;

template<typename T>
T larger(T first, T second) {
    return (first > second) ? first : second;
}

template<typename K, typename V>
void display(K key, V value) {
    cout << key << "=" << value << endl;
} 

int main() {
    display("a", 1);
    display(1, 1);    

    return 0;
}
```