# 🏗️ Design Patterns: Core Short Notes

> 📌 Reusable solutions to common software design problems.
>
> **Design Patterns are templates, not final code.**  
> They help build maintainable, scalable, and reusable software.
>
> **MCQ Focus:** Mainly tests your understanding of **Object-Oriented Design (OOD)** and **Design Patterns**.

---

# 1. Categories of Design Patterns ⭐

The **Gang of Four (GoF)** classified **23 Design Patterns** into **3 categories**.

| Category | Focus | Examples |
|----------|-------|----------|
| **Creational** | How objects are created | Singleton, Factory, Builder |
| **Structural** | How classes/objects are composed | Adapter, Decorator, Facade |
| **Behavioral** | How objects communicate | Observer, Strategy |

---

# 2. Must-Know Creational Patterns ⭐

## 1.1 Singleton Pattern

### Purpose

Ensures that a class has **only one instance** and provides a **global access point**.

### Real-world Uses

- Database Connection
- Logger
- Configuration Manager
- Cache Manager

### MCQ Clues

- Only one object exists
- Private constructor
- Static instance
- Static `getInstance()` method

---

## 1.2 Factory Method Pattern

### Purpose

Creates objects **without exposing the creation logic**.

Subclasses decide which object to instantiate.

### Real-world Uses

- Transport Factory → Truck / Ship
- Notification Factory → Email / SMS
- Payment Factory → Bkash / Card

### MCQ Clues

- Eliminates direct `new ClassName()`
- Promotes **Loose Coupling**
- Object creation delegated to subclasses

---

# 3. Must-Know Structural Patterns ⭐

## 2.1 Adapter Pattern

### Purpose

Allows **incompatible interfaces** to work together.

Acts as a **translator**.

### Real-world Example

Your application uses **JSON**, but a third-party payment gateway uses **XML**.

Adapter converts one format to another.

### MCQ Clues

- Wrapper Pattern
- Translator
- Compatibility between interfaces

---

## 2.2 Decorator Pattern

### Purpose

Adds new behavior to an object **dynamically** without modifying the original class.

### Real-world Example

Coffee Shop:

- Coffee
- Coffee + Milk
- Coffee + Cream
- Coffee + Chocolate

Each feature is added dynamically.

### MCQ Clues

- Alternative to subclassing
- Runtime feature addition
- Follows **Open-Closed Principle (OCP)**

---

# 4. Must-Know Behavioral Patterns ⭐

## 3.1 Observer Pattern

### Purpose

Defines a **one-to-many relationship**.

When one object changes, all dependent objects are automatically notified.

### Real-world Uses

- Facebook Notifications
- YouTube Subscribers
- Event Handling
- Redux
- RxJS

### MCQ Clues

- Publisher → Subscribers
- Event-driven architecture
- Automatic notification

---

## 3.2 Strategy Pattern

### Purpose

Defines multiple algorithms and allows switching between them at runtime.

### Real-world Example

Checkout Page:

- Credit Card
- PayPal
- Bkash
- Nagad

User selects one strategy during runtime.

### MCQ Clues

- Runtime algorithm selection
- Interchangeable behaviors

---

# 📝 Common MCQ

### Question

Which design pattern limits the instantiation of a class to only one object?

- A) Factory
- B) Prototype
- ✅ C) Singleton
- D) Proxy

**Answer:** **C) Singleton**

---

# ⭐ Additional Patterns 

These patterns may also appear in MCQs.

---

## Builder Pattern (Creational)

### Purpose

Creates complex objects **step-by-step**.

### Example

Creating a Computer:

- CPU
- RAM
- SSD
- GPU

Instead of a huge constructor, build the object gradually.

### MCQ Clue

> **Complex object creation step-by-step**

---

## Facade Pattern (Structural)

### Purpose

Provides **one simple interface** to a complex subsystem.

### Example

Computer Startup

Internally:

- CPU
- RAM
- Hard Disk
- BIOS

User only presses:

```text
Power Button
```

### MCQ Clue

> **Unified, simplified interface to a complex subsystem**

---

# 🎯 Category Identification (Common MCQ)

Know which pattern belongs to which category.

## Creational

- Singleton
- Factory Method
- Abstract Factory
- Builder
- Prototype

---

## Structural

- Adapter
- Decorator
- Facade
- Proxy
- Composite

---

## Behavioral

- Observer
- Strategy
- Command
- State
- Iterator

---

# 📐 SOLID Principles ⭐⭐⭐

> Design Patterns are built on top of the **SOLID Principles**.

Knowing SOLID significantly increases your chances in MCQs.

---

## S — Single Responsibility Principle (SRP)

A class should have **only one reason to change**.

> One Class → One Responsibility

---

## O — Open-Closed Principle (OCP)

Software entities should be:

- **Open for Extension**
- **Closed for Modification**

Add new features without changing existing code.

---

## L — Liskov Substitution Principle (LSP)

A subclass should be able to replace its parent class **without breaking the program**.

---

## I — Interface Segregation Principle (ISP)

Clients should not depend on interfaces they do not use.

> Prefer **small, focused interfaces** over one large interface.

---

## D — Dependency Inversion Principle (DIP)

High-level modules should **not depend on low-level modules**.

Both should depend on **abstractions (interfaces)**.

---

# 🚀 Quick Revision

## Design Pattern Categories

| Category | Patterns |
|----------|----------|
| **Creational** | Singleton, Factory, Builder, Prototype, Abstract Factory |
| **Structural** | Adapter, Decorator, Facade, Proxy, Composite |
| **Behavioral** | Observer, Strategy, Command, State, Iterator |

---

## Pattern Cheat Sheet

| Pattern | Purpose | MCQ Keyword |
|----------|----------|-------------|
| Singleton | One object only | Single Instance |
| Factory | Object creation | Loose Coupling |
| Builder | Step-by-step creation | Complex Object |
| Adapter | Compatibility | Wrapper / Translator |
| Decorator | Add behavior dynamically | Runtime Extension |
| Facade | Simplify complex subsystem | Unified Interface |
| Observer | Automatic notifications | Publisher–Subscriber |
| Strategy | Runtime algorithm selection | Interchangeable Algorithms |

---

# 📝 Common MCQ Keywords

| Keyword | Pattern |
|----------|----------|
| One Instance | Singleton |
| Loose Coupling | Factory |
| Wrapper | Adapter |
| Runtime Feature Addition | Decorator |
| Publisher–Subscriber | Observer |
| Runtime Algorithm Selection | Strategy |
| Step-by-Step Object Creation | Builder |
| Simplified Interface | Facade |

---

# 📚 SOLID Cheat Sheet

| Principle | Meaning |
|-----------|---------|
| **S** | Single Responsibility Principle |
| **O** | Open-Closed Principle |
| **L** | Liskov Substitution Principle |
| **I** | Interface Segregation Principle |
| **D** | Dependency Inversion Principle |

---

# 🧪 Practice MCQ

### Question

> **"A class should be open for extension but closed for modification."**  
> Which SOLID principle does this describe?

- A) SRP
- ✅ B) OCP
- C) LSP
- D) DIP

**Answer:** **B) Open-Closed Principle (OCP)**

---

# 🎯 Last-Minute Revision (30 Seconds)

### Categories

```text
Creational → Create Objects
Structural → Compose Objects
Behavioral → Communicate Objects
```

### Patterns

```text
Singleton → One Instance
Factory → Object Creation
Builder → Step-by-Step Creation
Adapter → Wrapper
Decorator → Add Features Dynamically
Facade → Simple Interface
Observer → Notify Everyone
Strategy → Change Algorithm at Runtime
```

### SOLID

```text
S → One Responsibility
O → Open for Extension, Closed for Modification
L → Replace Parent with Child Safely
I → Small Interfaces
D → Depend on Abstractions
```