# 172 Creating Custom Exceptions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/fW3eq5gVbXs?si=7x8qnA2vHBXzvZWc"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Context

* Built-in exceptions (`invalid_argument`, `out_of_range`) are useful, but **not always expressive enough**.
* Some errors are **domain-specific** (e.g., *user account locked*).

  * Such cases can’t be represented clearly with standard exceptions.
* ✅ Solution → Create **custom exceptions**.

---

## 2. Solution → Define a Custom Exception Class

* Create a class that **inherits from `std::exception`** (not compulsory, but highly recommended).

  * Allows centralized exception handling → you can `catch (const exception& ex)` in `main`.
* Add a **domain-specific error message** by overriding `what()`.

---

## 3. Deep Dive → `const char* what() const noexcept override`

### Function signature breakdown:

```cpp
const char* what() const noexcept override
```

1. **`const char*`** → return type

   * Returns a **C-style string** (`const char*`) containing the error message.
   * Example: `"Your account is locked! Contact the admin."`

2. **`what()`** → function name from `std::exception`.

   * Every exception class in C++ should provide a `what()` for description.

3. **`const`** (after method name)

   * Marks the method as **read-only** (cannot modify the object).
   * Makes sense → retrieving an error message shouldn’t change the exception object.

4. **`noexcept`**

   * Declares that this function **will not throw exceptions**.
   * Critical because `what()` is often called **inside a `catch` block**.
   * Throwing another exception here would break error handling → undefined behavior.

   ⚠️ If you accidentally write code that throws inside `what()`, the compiler warns you:
   `"what() has a non-throwing exception specification but can still throw"`.

5. **`override`**

   * Ensures we are properly overriding the base class method.
   * Compiler will error if the signature doesn’t match exactly.

📌 Important detail: The `noexcept` **must appear between `const` and `override`**.

* ✅ `const noexcept override` → correct.
* ❌ `const override noexcept` → compilation error.

---

## 4. Code Walkthrough

### ❌ Wrong — Throws inside `what()`

```cpp
class AccountLocked : public exception {
public:
    const char* what() const noexcept override {
        throw invalid_argument{""}; // BAD: breaks noexcept guarantee
        return "Your account is locked! Contact the admin.";
    }
};
```

* Compiles, but generates a **warning**.
* Dangerous: `what()` should never throw.

---

### ✅ Correct Implementation

```cpp
class AccountLocked : public exception {
public:
    const char* what() const noexcept override {
        return "Your account is locked! Contact the admin.";
    }
};
```

### Usage in Main

```cpp
void login() {
    throw AccountLocked{};
}

int main() {
    try {
        login();
    }
    catch (const exception& ex) {   // catches AccountLocked too
        cout << ex.what();
    }
}
```

---

## 5. Visual Mental Model

```
   AccountLocked (custom exception)
          |
          v
   inherits from std::exception
          |
          v
   Overrides what()
        |
        v
   Returns → "Your account is locked! Contact the admin."
```

* Works exactly like built-in exceptions.
* Can be caught individually (`catch(AccountLocked&)`) or generally (`catch(exception&)`).

---

## 6. Best Practices & Insights

* ✅ Always derive from `std::exception` (for centralized handling).
* ✅ Override `what()` with **clear, meaningful messages**.
* ✅ Use `const noexcept override` to match the base declaration.
* ✅ Keep `what()` **simple and non-throwing** (only return strings).
* ✅ Use descriptive names for custom exceptions → `AccountLocked`, `EmployeeNotFound`, `QuotaExceeded`.
* ⚠️ Never throw exceptions from within `what()`.

---

## 7. Interview Insights

* **Q:** Why inherit from `std::exception` when creating custom exceptions?
  **A:** To maintain compatibility with generic `catch (const exception&)` handlers.

* **Q:** What is the purpose of `noexcept` in `what()`?
  **A:** Guarantees the method won’t throw — critical in `catch` blocks.

* **Q:** Why does `what()` return `const char*` instead of `std::string`?
  **A:** Simplicity, efficiency, and C++ standard library design (avoids memory allocations inside exception reporting).

* **Q:** Where must `noexcept` be placed?
  **A:** Between `const` and `override`.

---

## 8. Quick Revision Sheet

* Custom exceptions = domain-specific error reporting.
* Always inherit from `std::exception`.
* Override `what()` → `const char* what() const noexcept override`.
* Return **C-style string** describing the error.
* Never throw from `what()`.

---

### AccountLocked.h

#### Part 1 - throw in a noexcept function

```cpp
#ifndef ADVANCED_ACCOUNTLOCKED_H
#define ADVANCED_ACCOUNTLOCKED_H

#include <stdexcept>

using namespace std;

class AccountLocked : public exception {
public:
    const char* what() const noexcept override {
        // Shows warning but still compiles
        throw invalid_argument{""}; 
         
        return "Your account is locked! Contact the admin.";
    }
};

#endif
```

#### Part 2 - fix

```cpp
#ifndef ADVANCED_ACCOUNTLOCKED_H
#define ADVANCED_ACCOUNTLOCKED_H

#include <stdexcept>

using namespace std;

class AccountLocked : public exception {
public:
    const char* what() const noexcept override {
        return "Your account is locked! Contact the admin.";
    }
};

#endif
```

### main.cpp

```cpp
#include "AccountLocked.h"

using namespace std;

void login() {
    throw AccountLocked{};
}

int main() {
    try {
        login();
    }
    catch (const exception& ex) {
        cout << ex.what();
    }

    return 0;
}
```