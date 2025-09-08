# 136 Introduction

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/gmJxnXOdOzE?si=WoiGNZjQGYYz9Uvj"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Learning Objectives
By the end of this section, you will be able to:
- Understand **what operator overloading is** and why it’s useful.
- Implement **equality** (`==`) and **comparison** (`<`, `>`, `<=`, `>=`) operators.
- Use the **spaceship operator** (`<=>`) for unified comparisons.
- Overload **arithmetic operators** (`+`, `-`, `*`, `/`, `%`, etc.).
- Implement the **subscript operator** (`[]`) for custom indexing.
- Correctly define the **assignment operator** (`=`) and compound assignments (`+=`, `-=`, etc.).
- Follow **best practices** to make overloaded operators intuitive and safe.

---

## 2. What is Operator Overloading?
- **Definition:** A way to redefine the meaning of C++ operators for user‑defined types (classes/structs).
- **Purpose:** Makes objects behave like built‑in types, improving **readability** and **expressiveness**.
- **Key Rule:** You can’t create new operators — only redefine existing ones.

---

## 3. Operators to Cover

| Category | Operators | Notes |
|----------|-----------|-------|
| **Equality & Comparison** | `==`, `!=` | Often implemented together for consistency. |
| **Spaceship** | `<=>` | Introduced in C++20, generates all comparison operators automatically. |
| **Arithmetic** | `+`, `-`, `*`, `/`, `%` | Can be member or non‑member functions. |
| **Subscript** | `[]` | Must be a member function; can return reference for assignment. |
| **Assignment** | `=` | Handle deep copies, self‑assignment, and resource management. |
| **Compound Assignment** | `+=`, `-=`, `*=`, `/=`, `%=` | Usually implemented in terms of basic arithmetic operators. |

---

## 4. Next Steps
In the upcoming lessons, we’ll:
1. Implement each operator step‑by‑step.
2. Discuss **member vs non‑member** operator functions.
3. Explore **friend functions** for accessing private members.
4. Look at **common pitfalls** and how to avoid them.