# 120 An Introduction to Object-oriented Programming

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/gGaHUU3SWq8?si=TAY81-j1-RKe90b_" 
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Programming Paradigms
- **Definition:** A paradigm is a style or methodology for writing software.
- **Common paradigms:**
  - **Procedural:** Organize code into procedures/functions.
  - **Functional:** Build software by composing pure functions.
  - **Object-Oriented:** Build software by creating and interacting with objects.
  - **Event-Driven:** Code reacts to events (e.g., GUI clicks, network messages).
- **Key Insight:** No single paradigm is “best” — choose based on the problem.
- **Real-world practice:** Applications often mix paradigms.

---

## 2. Why OOP in C++
- C++ has **strong support** for OOP.
- Many frameworks and large-scale applications are designed using OOP principles.
- As a professional C++ developer, mastering OOP is essential.

---

## 3. OOP Mindset
- **Functional programming:** Focus on *functions* and their composition.
- **OOP:** Focus on *objects* — entities that combine **data** and **behavior**.
- **Objects work together** to solve problems, each with a clear responsibility.
- Analogy: People in an organization (e.g., a restaurant) — each has a role.

---

## 4. What is an Object?
- **Definition:** A software entity that contains:
  - **Attributes (Properties):** Data describing the object’s state.
  - **Methods (Functions):** Actions the object can perform.
- **Examples:**
  - **Dialog Box:**
    - Attributes: size, position
    - Methods: show(), hide(), resize(), move()
  - **Video Player:**
    - Attributes: size, current position, playback speed
    - Methods: play(), pause(), stop()

- Objects are **not always visual** — they can represent:
  - Data storage/retrieval
  - Sending emails/notifications
  - Performing calculations

---

## 5. What is a Class?
- **Definition:** A blueprint or recipe for creating objects.
- **Analogy:** Recipe → many cakes; Class → many objects.
- **In UML (Unified Modeling Language):**
  ```
  +----------------------+
  | ClassName            |  ← Name
  +----------------------+                  -----
  | attributes           |  ← Data members      |
  +----------------------+                      | ← Members of a class
  | methods              |  ← Member functions  |
  +----------------------+                  -----
  ```
- **Members of a class:**
  - **Variables (attributes)**
  - **Functions (methods)**

---

## 6. Classes vs. Structures in C++
- **Structures (`struct`):**
  - Often used as **simple data containers**.
  - Default access: `public`.
- **Classes (`class`):**
  - Combine **data + functionality**.
  - Default access: `private`.
- **Encapsulation:** Core OOP principle — bundle data and the functions that operate on it into one unit.

---

## 7. Classes vs. Objects
- **Class:** The definition/blueprint.
- **Object:** An **instance** of a class (created from the blueprint).

---

## 8. Interview & Revision Pointers
- Be able to **define OOP** and explain **why it’s used**.
- Know the **difference between class and object**.
- Understand **encapsulation** and why it matters.
- Be ready to give **real-world analogies** (restaurant, recipe, blueprint).
- Recognize that **OOP and functional programming can coexist** in C++.

---

## 9. Quick Mental Map
```
Programming Paradigms
 ├── Procedural
 ├── Functional
 ├── Object-Oriented
 │    ├── Class (Blueprint)
 │    │    ├── Attributes (Data)
 │    │    └── Methods (Behavior)
 │    └── Object (Instance)
 └── Event-Driven
```