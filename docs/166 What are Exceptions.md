# 166 What are Exceptions

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/-sdXTs-E0ls?si=_fJc0byriddz4QXR"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Introduction

* **Exception** = an **object** used to report errors during runtime.

* Can technically be of *any type* (e.g., `int`, `string`), but most often:
  → An instance of a class derived (directly or indirectly) from **`std::exception`**.

* **C++ Standard Library exception hierarchy**:

  * **`std::exception`** (root)

    * **`std::logic_error`** → internal program logic errors (e.g., invalid usage).

      * `std::invalid_argument` → wrong parameter passed.
      * `std::out_of_range` → index outside bounds.
    * **`std::runtime_error`** → runtime problems (e.g., failing operations).

---

## 2. Custom Exceptions

* Developers can define their own exceptions for domain-specific problems:

  * `EmployeeNotFound`
  * `AccountLocked`
  * `DailyQuotaReached`

➡ This makes error reporting **meaningful** and **specific** to the business logic.

---

## 3. Volleyball Analogy (Throw & Catch)

* **Throwing** = one player serves the ball → function encounters a problem and "throws" the exception object.

* **Catching** = other player receives it → another part of the program handles it.

* Exception objects carry **information about the error**:

  * Used to inform the user.
  * Logged for diagnostics.
  * Used to retry or gracefully terminate.

---

## 4. Visual Mental Model

```
Function detects error
      ↓
 throws exception (object)
      ↓
 Runtime looks for a matching
       catch block
      ↓
   Error handled (log, message, retry)
```

---

## 5. Best Practices & Insights

* Use **standard exceptions** when possible → easier for others to understand.
* Create **custom exceptions** only when the error is *domain-specific*.
* Don’t throw primitive types (like `int` or `char*`) → prefer class-based exceptions.
* Exception = information container → always add context.

---

## 6. Interview Insights

**Q:** Why are exceptions objects in C++?
**A:** Objects can carry rich error information (message, type, context), unlike error codes.

**Q:** Difference between `logic_error` and `runtime_error`?
**A:**

* `logic_error`: bugs in the program’s design (e.g., passing an invalid argument).
* `runtime_error`: unexpected issues during execution (e.g., file not found).

**Q:** Can we throw any type in C++?
**A:** Yes, but best practice is to throw exceptions derived from `std::exception`.

---

## 7. Quick Revision

* Exception = object for error reporting.
* `std::exception` = root of standard exception hierarchy.
* Common derived types:

  * `invalid_argument`, `out_of_range` (logic errors).
  * `runtime_error` (runtime failures).
* Throw = signal error.
* Catch = handle error.

---

👉 Next up: we’ll see the **syntax for throwing and catching exceptions** in C++.