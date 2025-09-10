# 163 Deep Inheritance Hierarchies

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/gzWeZvO5wcI?si=7Rv-5Lo2tE2hiwA8"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Problem Setup

Inheritance lets us reuse code and achieve polymorphism.
But **too much inheritance** → fragile, complex hierarchies.

Common mistake: beginners create **deep inheritance hierarchies** because they think every "is-a" relationship = inheritance.

Example scenario:

* Classes: `Student`, `Instructor`
* Common attributes → extract into `User`
* Instructor types: `RegularInstructor`, `PremiumInstructor`
* Another class: `Course`
* `User` and `Course` share attributes → extract into `Entity`

Now we have a **deep hierarchy**:

```
Entity
 ├── User
 │    ├── Student
 │    └── Instructor
 │         ├── RegularInstructor
 │         └── PremiumInstructor
 └── Course
```

Looks neat at first. But it's a **trap**.

---

## 2. The Problem with Deep Hierarchies

* Each inheritance introduces **coupling**.
* `User` & `Course` depend on `Entity`.
* If `Entity` changes (e.g., constructor gains a parameter):

  * Must change `User` and `Course` constructors.
  * May need to recompile all derived classes.

👉 Higher-level changes = **higher cost of change**.

Rule of thumb:

* Limit inheritance depth to **max 3 levels**.
* Beyond that → fragile and costly to maintain.

---

## 3. Alternative Solutions

### Option 1: Duplication over Dependency

Instead of forcing common attributes into a base class:

```cpp
class Student {
    DateTime lastLogin;
};

class Instructor {
    DateTime lastLogin;
};
```

✅ Yes, there’s duplication.
✅ But **no dependency** → classes evolve independently.

---

### Option 2: Generalization with Attributes

Do we really need separate `Student` and `Instructor` classes?
Not always.

```cpp
enum class UserType { Student, Instructor };

class User {
    UserType type;
    DateTime lastLogin;
};
```

✅ One class, flexible with `enum`.
✅ No fragile inheritance tree.

---

### Option 3: Drop `Entity`

Just because `User` and `Course` are both “entities” doesn’t mean they should inherit from `Entity`.

👉 **Composition or duplication** is often better than inheritance here.

---

## 4. Visual Mental Model

❌ Fragile Deep Hierarchy:

```
Entity
 ├── User
 │    ├── Student
 │    └── Instructor
 │         ├── RegularInstructor
 │         └── PremiumInstructor
 └── Course
```

✅ Simpler Alternative:

```
User (with type: Student | Instructor)
Course (separate, no base class)
```

Both evolve independently.

---

## 5. Best Practices & Insights

* ✅ Keep inheritance shallow (≤3 levels).
* ✅ Favor **composition and duplication** over forced inheritance.
* ✅ Don’t extract “common code” too early → premature abstraction = fragility.
* ✅ Think in terms of **independence and evolvability**, not just DRY.

---

## 6. Interview Insights

**Q:** Why avoid deep inheritance hierarchies?
**A:** They create tight coupling → small base changes ripple through many classes, causing fragility.

**Q:** Is duplication always bad?
**A:** No. Sometimes duplication is cheaper and safer than complex inheritance.

**Q:** What’s an alternative to separate `Student` and `Instructor` classes?
**A:** A `User` class with a `type` field (enum).

**Q:** Difference between composition and inheritance in terms of coupling?
**A:** Composition = weaker coupling, classes evolve independently. Inheritance = strong coupling, ripple effects.

---

## 7. Quick Revision Sheet

* 🚫 Avoid deep inheritance hierarchies (>3 levels).
* 🚫 Don’t overuse "is-a" → inheritance ≠ always required.
* ✅ Prefer **composition** or **attributes** over inheritance.
* ✅ Duplication > Fragile abstraction.
* ✅ Higher up the hierarchy → higher cost of changes.