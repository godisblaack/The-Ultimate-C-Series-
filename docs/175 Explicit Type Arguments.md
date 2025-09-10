# 175 Explicit Type Arguments

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/h6rdRzEfDdQ?si=nDkSblU9wwoD2ntz"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Automatic Type Deduction

* The compiler can usually **deduce** the type of `T` from the arguments.
* Example:

```cpp
template<typename T>
T larger(T first, T second) {
    return (first > second) ? first : second;
}

int main() {
    auto result = larger(1.1, 2.2);  // T deduced as double
    result = larger(1, 2);           // T deduced as int
}
```

* `auto` helps confirm what type the compiler deduced:

  * Passing two `double`s → `result` is `double`.
  * Passing two `int`s → `result` is `int`.

---

## 2. Ambiguity Problem

* What if the arguments have **different types**?

```cpp
larger(1, 2.1);   // int + double
```

* ❌ Compiler error: It cannot deduce `T` because both parameters must be of the **same type**, but we supplied different ones.
* Should it generate a version with `int` or with `double`? → ambiguous.

---

## 3. Explicit Template Arguments

* Solution: explicitly tell the compiler what `T` should be.

```cpp
larger<double>(1, 2.1);  // T is double
```

* Compiler generates a function instance that takes `(double, double)` and returns `double`.
* The `int` argument (`1`) gets implicitly converted to `double`.

---

## 4. Real-World Analogy

* This is the same syntax we’ve already used with standard library functions:

```cpp
auto ptr = make_unique<int>();   // explicitly says: T = int
```

* Now it makes sense → **make\_unique is just a function template**.

---

## 5. Best Practices & Insights

* ✅ Prefer **implicit deduction** whenever possible (cleaner code).
* ✅ Use `auto` for result storage → avoids worrying about exact type.
* ✅ Use **explicit template arguments** when:

  * Arguments are of **different types**.
  * You want to force a **wider/narrower type** (e.g., `larger<double>(1, 2.1)`).
* ⚠️ Without explicit type, compiler cannot “mix types” — templates require a **single consistent type parameter**.

---

## 6. Quick Revision Sheet

* Compiler usually deduces `T`:

  ```cpp
  auto x = larger(2, 3);      // int
  auto y = larger(2.2, 3.3);  // double
  ```

* Ambiguity case → must specify explicitly:

  ```cpp
  auto z = larger<double>(2, 3.3);
  ```

* Common in STL:

  ```cpp
  auto p = make_unique<int>();
  ```

---

## 7. Code from the video

### main.cpp

```cpp
#include <iostream>

using namespace std;

template<typename T>
T larger(T first, T second) {
    return (first > second) ? first : second;
}

int main() {
    auto result = larger(1.1, 2.2);
    result = larger(1, 2);

    result = larger<double>(1, 2.1);

    // Similarly, we have used make_unique<int>() to create smart pointers.

    return 0;
}
```