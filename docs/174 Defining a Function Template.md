# 174 Defining a Function Template

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/9NSqEgqCpiA?si=cDlQM9BukPJZTUGH"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

Got it ✅
I’ll carefully base the notes on the **transcript + provided code**, keep them structured in our style, and emphasize the key explanations (like `typename`, executable size, operator requirement, etc.).

---

# 182 Function Templates

## 1. Problem Context

* Need a function to return the **larger of two values**.
* A simple `int larger(int, int)` works fine for integers.
* Problem:

  * Passing doubles → implicit conversion to `int` → ❌ precision loss.
  * Passing strings → ❌ no overload exists.
* Fix with **function overloading**:

  * Write one version for `int`, another for `double`, another for `string`…
  * ❌ Leads to **code duplication & maintenance overhead**.

---

## 2. Solution → Function Templates

* Use **function templates** to write **generic functions**.
* Define the function **once**, and let the compiler generate type-specific versions as needed.
* Works for any type that supports the operations used inside the function (here, the `>` operator).

---

## 3. Code Walkthrough

### ❌ Overloading (code duplication)

```cpp
int larger(int first, int second) {
    return (first > second) ? first : second;
}

// Overloaded for type double
double larger(double first, double second) {
    return (first > second) ? first : second;
}
```

* If we want support for `string` too → must **duplicate** again.
* If a bug exists → must **fix in multiple places**.

---

### ✅ Function Template (generic solution)

```cpp
template<typename T>   // template<class T> also works
T larger(T first, T second) {
    return (first > second) ? first : second;
}

int main() {
    larger(1.1, 2.2);    // compiler generates double version
    larger(1, 2);        // compiler generates int version
    larger("a", "b");    // compiler generates const char* version
}
```

* `typename T` → declares a **template parameter** (can be any type).
* `class T` is equivalent, but `typename` is preferred in modern C++.
* The compiler generates an instance of the function **only if used**.

---

## 4. Benefits of Function Templates

* ✅ **No duplication** → single implementation works for all types.
* ✅ **Easier maintenance** → fix bugs in one place only.
* ✅ **Smaller executables** (potentially):

  * If function is **never called** → no code is generated.
  * If function is called with `double` twice → compiler **reuses** the same instance, doesn’t duplicate.
* ✅ Works for both primitive types and objects (e.g., `string`).

---

## 5. Visual Mental Model

```
Overloading Approach:             Template Approach:
--------------------------------  -------------------------------
int larger(int, int);             template<typename T>
double larger(double, double);    T larger(T, T);
string larger(string, string);    // Compiler generates versions 
                                  // only when needed
```

---

## 6. Best Practices & Insights

* ✅ Prefer `typename T` (more descriptive than `class T`).
* ✅ Use function templates when logic is **identical across types**.
* ⚠️ Works only if the type supports required operators (e.g., `>` here).
* ⚠️ Don’t overuse → sometimes overloading with different logic per type is better.

---

## 7. Interview Insights

* **Q:** What problem do function templates solve?
  **A:** Eliminate code duplication by writing type-independent, generic functions.

* **Q:** When does the compiler generate instances of a template function?
  **A:** At **compile time**, only when the function is used with a specific type.

* **Q:** What’s the difference between `typename` and `class` in templates?
  **A:** Both mean “template type parameter,” but `typename` is preferred for clarity.

---

## 8. Quick Revision Sheet

* Define once:

  ```cpp
  template<typename T>
  T func(T a, T b);
  ```
* Compiler generates concrete functions at compile time.
* Reduces duplication, improves maintainability, may reduce executable size.
* Works for any type supporting required operators.

---

👉 Do you want me to continue with the next part: **Class Templates**, or should I expand this section further with more **edge cases** (like mixed types, constraints)?

## 9. Code from the video

### main.cpp

#### Overloading for different types

```cpp
#include <iostream>

using namespace std;

int larger(int first, int second) {
    return (first > second) ? first : second;
}

// Overloaded for type double
double larger(double first, double second) {
    return (first > second) ? first : second;
}

int main() {
    larger(1.1, 2.2);

    return 0;
}
```

#### Function template

```cpp
#include <iostream>

using namespace std;

template<typename T> // template<class T>
T larger(T first, T second) {
    return (first > second) ? first : second;
}

int main() {
    larger(1.1, 2.2);
    larger(1, 2);
    larger("a", "b");

    return 0;
}
```