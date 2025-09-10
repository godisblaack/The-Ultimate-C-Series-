# 173 Introduction

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/9mGLWuVO25o?si=mDjUiSmgWtqPxnfF"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Context

* Often in C++, we write **functions/classes** that look almost identical, but differ only in the **data type** they operate on.

  * Example:

    * A `max(int a, int b)` function.
    * A `max(double a, double b)` function.
    * A `max(string a, string b)` function.
* Writing separate functions for every type → ❌ **duplication**, ❌ **maintenance overhead**.

---

## 2. Solution → Templates

* Templates allow us to write **generic code**.
* Instead of hardcoding types (e.g., `int`, `double`), we define a **placeholder type**.
* At compile time, the compiler generates the **concrete implementation** for the type actually used.

---

## 3. Key Benefits

* ✅ Avoid code duplication.
* ✅ Improve reusability.
* ✅ Strongly typed (compiler checks correctness at instantiation).
* ✅ Used for both **functions** and **classes**.

---

## 4. Roadmap for This Section

1. **Function Templates** → Writing generic functions.
2. **Class Templates** → Writing generic classes (e.g., containers).
3. **Specialization** → Customizing templates for specific types.
4. **Best Practices** → When and how to use templates effectively.

---

## 5. Visual Mental Model

```
Without Templates:      With Templates:
-------------------     -------------------
int max(int, int);      template <typename T>
double max(double, double);   T max(T, T);

string max(string, string);   // Compiler generates versions
```

---

## 6. Interview Insights

* **Q:** What problem do templates solve?
  **A:** Eliminate code duplication by enabling generic programming.

* **Q:** When are template functions/classes generated?
  **A:** At **compile time** when they are instantiated with a specific type.

* **Q:** What’s the difference between templates and macros?
  **A:**

  * Templates are **type-safe**, resolved at compile time.
  * Macros are just text substitutions (no type safety).

---

## 7. Quick Revision Sheet

* Templates = **blueprints for code**.
* Syntax:

  ```cpp
  template <typename T>
  T functionName(T param);
  ```
* Compiler generates actual implementation when used.
* Works for functions **and** classes.