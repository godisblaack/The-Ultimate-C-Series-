# 165 Introduction

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/5IhMT4UMi0o?si=6cIWkTPdNN2qqcaj"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Introduction

* **Purpose of Exceptions**: Mechanism to **signal and handle errors** in a structured way.
* Without exceptions → error handling is often messy (return codes, flags).
* With exceptions → errors are **explicit**, **propagate automatically**, and can be handled in one place.

---

## 2. What You’ll Learn in This Section

1. **What exceptions are** and how they differ from normal control flow.
2. **How to throw exceptions** when an error occurs.
3. **How to catch exceptions** and handle them safely.
4. **Creating custom exception classes** for more descriptive error reporting.
5. **Using `noexcept` keyword** to enforce or document functions that should not throw exceptions.

---

## 3. Visual Mental Model

```
[Error occurs] → throw exception
                 ↓
        [Stack unwinds automatically]
                 ↓
        [catch block handles it]
```

* Instead of returning error codes, you *throw* an object.
* Runtime searches for a matching `catch`.
* If none is found → program terminates.

---

## 4. Best Practices & Insights

* Use exceptions for **exceptional situations** (not regular control flow).
* Keep them **rare but meaningful** (invalid input, failed resource allocation, logic violations).
* Keep normal code separate from error-handling code → improves readability.

---

## 5. Interview Insights

**Q:** Why use exceptions instead of error codes?
**A:** Exceptions separate error-handling from normal logic, propagate automatically, and make code cleaner.

**Q:** What happens if no `catch` handles an exception?
**A:** The program calls `std::terminate()` and exits.

**Q:** What is `noexcept`?
**A:** A keyword in C++11+ that marks a function as not throwing exceptions, improving performance and clarity.

---

## 6. Quick Revision

* **throw** → to signal an error.
* **catch** → to handle the error.
* **Custom exceptions** → for domain-specific error messages.
* **noexcept** → promise that a function won’t throw.

---

👉 Next, we’ll dive into the actual **syntax of throwing and catching exceptions** with code examples.