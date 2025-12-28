### 1️⃣ What is a Design Pattern? Why is it Needed?

### ✅ Answer

A **Design Pattern** is a **proven, reusable solution** for common problems that occur frequently in software design.

👉 It is **not code**, but a **problem → solution approach** that helps structure and organize code effectively.

### 🔹 Why Do We Need Design Patterns?

- Code becomes **clean and readable**
- Easier to **maintain and extend**
- Provides **reusable and scalable design**
- Improves **team communication**  
  (Everyone understands the solution by its pattern name)

---

### 2️⃣ Difference Between Design Pattern and Algorithm

| Design Pattern | Algorithm |
|---------------|-----------|
| Focuses on **software design structure** | Focuses on **step-by-step problem solving** |
| High-level solution | Low-level implementation |
| Architecture & design oriented | Performance & computation oriented |
| Provides an idea for code reuse | Provides exact logic/code |
| Defines **how to organize code** | Defines **how to solve a problem** |

👉 **Algorithm** = How to solve a problem  
👉 **Design Pattern** = How to organize and design the code

--- 

### 3️⃣ Benefits of Using Design Patterns

✅ Code becomes **loosely coupled**  
✅ Improves **maintainability**  
✅ Easier **scalability**  
✅ Simplifies **testing**  
✅ Creates **common understanding** among team members  
✅ Encourages following **best practices**

---

### 4️⃣ Problems Without Using Design Patterns

❌ Code becomes **tightly coupled**  
❌ Excessive **if-else / switch statements**  
❌ Code breaks with **future changes**  
❌ **Bug fixing** becomes harder  
❌ Reduces **readability and maintainability**  
❌ Code **cannot be reused**  

👉 In **long-term projects**, technical debt increases.

---

### 5️⃣ Gang of Four (GoF) Design Patterns

**Gang of Four (GoF)** refers to 4 authors:

- Erich Gamma  
- Richard Helm  
- Ralph Johnson  
- John Vlissides  

They wrote the book:  
**“Design Patterns: Elements of Reusable Object-Oriented Software”**

📌 The book defines **23 classic design patterns**  
👉 These are called **GoF Design Patterns**.

---

### 6️⃣ Types of GoF Design Patterns

GoF Design Patterns are divided into **3 categories**:

####  1️⃣ Creational Patterns
Focus on **object creation mechanisms**

**Examples:**  
- Singleton  
- Factory Method  
- Abstract Factory  
- Builder  
- Prototype  

#### 2️⃣ Structural Patterns
Focus on **class/object structure and relationships**

**Examples:**  
- Adapter  
- Decorator  
- Facade  
- Composite  
- Proxy  
- Bridge  

#### 3️⃣ Behavioral Patterns
Focus on **communication and behavior between objects**

**Examples:**  
- Strategy  
- Observer  
- State  
- Command  
- Template Method  
- Visitor


<br>
<br>

# part 2

### 1️⃣ General Questions

### ❓ Why Creational Patterns are Needed?

- To make **object creation flexible and reusable**  
- To keep **code loosely coupled**  
- To simplify **future changes**  
- Using `new` keyword directly creates **tight coupling**

---

### ❓ Normal Object Creation vs Pattern-based Creation

| Normal Way | Pattern Way |
|-----------|-------------|
| Create object directly using `new` keyword | Use a **pattern class or method** to create object |
| Tight coupling | Loose coupling |
| Changing class requires modifying code everywhere | Easy to extend & maintain |
| Fine for small/simple projects | Preferable for large-scale/scalable projects |

---

### 2️⃣ Specific Patterns

### 🔹 Singleton Pattern

**Definition:**  
A class has **only one instance**, accessible **globally**.

**Use Case:**  
Logging, Configuration, Cache, Thread Pool

**Drawbacks:**  
- Hard to unit test  
- Must handle multi-threading safety  
- Global state may increase tight coupling

---

### 🔹 Factory Pattern

**Definition:**  
Centralizes object creation logic and hides class details from client code.

**Simple Factory vs Factory Method**

| Simple Factory | Factory Method |
|---------------|----------------|
| One class creates all objects | Subclass/interface decides which object to create |
| Simple, good for small projects | Flexible, scalable |
| Modify factory class if new type needed | Add new type by creating subclass |

---

### 🔹 Abstract Factory Pattern

**Definition:**  
Helps to create a **family of related objects**.  
Also called **Factory of Factories**.

**Difference from Factory:**  
- Factory → creates **one type of object**  
- Abstract Factory → creates **related multiple object types together**

**Use Case:**  
GUI toolkit: Button, Checkbox, Menu with same theme

---

### 🔹 Builder Pattern

**Definition:**  
Used to **construct complex objects step-by-step**.  
Separates **object creation process** from final object.

**Use Case:**  
- Complex objects like House, Car, Meal  
- When object has **many optional parameters**

---

### 3️⃣ Common Scenario Question

**Scenario:**  
Payment system with multiple methods (Bkash, Nagad, Card)  

**Answer:**  
Use **Factory Pattern** because:  
- Payment type is **dynamic**  
- Client does **not need class details**  
- Adding new payment method is **easy**

**Optional:**  
If payment processing has **many steps**, you can combine **Builder + Strategy** pattern.

<br>
<br>

# part 3

### 1️⃣ Structural Patterns – General

**Question:** What problem do Structural Patterns solve?  

**Answer:**  
- Simplify **class/object relationships**  
- Provide **flexible and maintainable structure** for large systems  
- Make **object composition** easier  
- Favor **composition over inheritance** for code reuse  

**Summary:** Structure-focused; prefer composition over inheritance

---

### 2️⃣ Adapter Pattern

**Definition:**  
- Makes one interface **compatible with another**  
- Helps **reuse existing code**  

**Real Life Example:**  
- Power plug adapter: use a 110V device in a 220V socket  
- Software: Make Payment API compatible with a new system interface  

**Interview Tip:**  
> Adapter = Wrapper that converts interface

---

### 3️⃣ Decorator Pattern

**Definition:**  
- Dynamically **add functionality to objects at runtime**  
- More flexible than inheritance, because no class modification needed  

**Example:**  
- Coffee + Milk + Sugar → multiple combinations using decorators  

**Why better than Inheritance:**  
- Inheritance → fixed at compile time  
- Decorator → runtime, allows multiple combinations easily  

---

### 4️⃣ Facade Pattern

**Definition:**  
- Provides a **simple interface** to a **complex subsystem**  
- Clients can use subsystem easily  

**Real Project Example:**  
- Payment gateway: Bkash, Nagad, Card → Facade class provides `pay(amount)`  
- Database subsystem → Facade method `saveData(obj)`  

**Tip:**  
> Facade hides complexity

---

### 5️⃣ Composite Pattern

**Definition:**  
- Organizes objects into a **tree structure**  
- Treat **individual and group objects uniformly**  

**Tree Structure Examples:**  
- File System: Folder → File1, File2, Subfolder → File3  
- GUI Components: Panel → Button, TextBox; Window → Panel  

**Interview Tip:**  
> Composite = part-whole hierarchy, treat leaf & composite the same way

<br>
<br>

# part 4

### 1️⃣ General Questions

**❓ What is a Behavioral Pattern?**  
- Handles **communication and interaction** between objects or classes  
- Centralizes behavior and provides **loosely coupled design**  
- Focus: How objects interact & behave  

**❓ What problem does Strategy Pattern solve?**  
- Helps **select different algorithms/behaviors at runtime**  
- Reduces large if-else / switch statements → clean & maintainable code  

**❓ State vs Strategy – Differences**  

| State | Strategy |
|-------|----------|
| Behavior changes according to object state | Object behavior selected at runtime |
| Focus: State-dependent behavior | Focus: Algorithm/behavior encapsulation |
| Context usually changes state internally | Client externally sets the strategy |

---

### 2️⃣ Very Common Patterns

### 🔹 Observer Pattern
**Definition:**  
- When one object changes, related objects **automatically update**  
- Implements **Publish-Subscribe model**  

**Real Life Examples:**  
- Facebook / Instagram notifications  
- Stock market price updates  
- News app subscriptions  

**Pub-Sub system:**  
- Publisher = Stock / News source  
- Subscriber = User / App components  

---

### 🔹 Strategy Pattern
**Use Cases:**  
- Payment system: Bkash, Nagad, Card → runtime choose  
- Sorting algorithms: Bubble, Quick, Merge → runtime select  

---

### 🔹 Template Method Pattern
**Definition:**  
- Defines **algorithm skeleton** in base class  
- Subclass overrides specific steps  

**Example:** Data processing  
1. Load data  
2. Process data → override  
3. Save result  

---

### 🔹 Command Pattern
**Definition:**  
- Encapsulates **request as an object**  
- Makes **undo/redo, queue, log** operations easy  

**Example:** GUI buttons / menu actions  

---

### 🔹 Visitor Pattern
**Definition:**  
- Add new operation **without modifying the class**  
- Uses **visitor class**  

**Object Structure:** `accept(visitor)` method  

**Why `accept(visitor)` needed:**  
- Element accepts the visitor  
- Visitor accesses element data and performs operation  

---

### 3️⃣ Scenario Question (Very Common)

**Question:** “I want to remove if-else / switch – which pattern should I use?”  

**Answer:**  
- Use **Strategy Pattern** → runtime behavior selection, clean & extendable code  

**Optional:**  
- For **complex object structure + multiple operations** → use **Visitor Pattern**

