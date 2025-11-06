# 🎨 Decorator Pattern


### 🔹 Introduction
The **Decorator Pattern** allows you to add new functionality (behavior) to an existing object dynamically, without modifying its original structure or source code.  
➡️ It’s used when you want to extend an object’s features **at runtime** in a flexible way.


Existing object আছে, সে object structure change না করে তার সাথে আরো functionality add করতে যে pattern টা help করে সেটাই **Decorator Pattern**

**Key Idea:**  
Wrap an object inside another object (decorator) to add new behavior.


<br>

### 🔹  ব্যাখ্যা
**Decorator Pattern** ব্যবহার করা হয় কোনো অবজেক্টে নতুন ফিচার বা আচরণ (behavior) যোগ করার জন্য,  
যেখানে পুরনো ক্লাস পরিবর্তন করতে হয় না।  

সহজভাবে বললে —  
**“পুরনো কোড না বদলে, অতিরিক্ত ফিচার প্যাক করে দেওয়া।”**  

**Decorator** মানে হলো “মোড়ক” —  
যেভাবে উপহার মোড়ানো হয়, তেমনি অবজেক্টটিকে মোড়ানো হয় নতুন ফিচারসহ।

<br>

### 🧠 Real-life Analogy
Imagine you order a **coffee ☕**

| Step | Example | Explanation |
|------|----------|-------------|
| 1️⃣ | Basic coffee | Core object |
| 2️⃣ | Add milk | Decorator |
| 3️⃣ | Add sugar | Another decorator |
| 4️⃣ | Add cream | Another decorator |

Each decorator adds extra functionality without changing the original coffee class.

<br>

### 🧩 Key Points

| Concept | Description |
|----------|--------------|
| **Type** | Structural Design Pattern |
| **Goal** | Add or modify behavior of objects dynamically |
| **Approach** | Wrap existing objects with decorator classes |
| **Used When** | You want to extend functionality without subclassing |
| **Relation With Inheritance** | Composition over inheritance (preferred way) |

<br>

### ⚙️ Class Diagram (Conceptually)

```
Component (Interface)
     ↑
ConcreteComponent (Main Class)
     ↑
Decorator (Abstract Class)
     ↑
ConcreteDecorator (Adds behavior)
```

Example
```c#
using System;

// Step 1: Base Interface
public interface ICoffee
{
    string GetDescription();
    double GetCost();
}

// Step 2: Concrete Component
public class SimpleCoffee : ICoffee
{
    public string GetDescription()
    {
        return "Simple Coffee";
    }

    public double GetCost()
    {
        return 5.0;
    }
}

// Step 3: Base Decorator
public abstract class CoffeeDecorator : ICoffee
{
    protected ICoffee coffee;

    public CoffeeDecorator(ICoffee coffee)
    {
        this.coffee = coffee;
    }

    public virtual string GetDescription()
    {
        return coffee.GetDescription();
    }

    public virtual double GetCost()
    {
        return coffee.GetCost();
    }
}

// Step 4: Concrete Decorators
public class MilkDecorator : CoffeeDecorator
{
    public MilkDecorator(ICoffee coffee) : base(coffee) { }

    public override string GetDescription()
    {
        return coffee.GetDescription() + ", Milk";
    }

    public override double GetCost()
    {
        return coffee.GetCost() + 1.5;
    }
}

public class SugarDecorator : CoffeeDecorator
{
    public SugarDecorator(ICoffee coffee) : base(coffee) { }

    public override string GetDescription()
    {
        return coffee.GetDescription() + ", Sugar";
    }

    public override double GetCost()
    {
        return coffee.GetCost() + 0.5;
    }
}

// Step 5: Client Code
public class Program
{
    public static void Main()
    {
        ICoffee myCoffee = new SimpleCoffee();
        Console.WriteLine($"{myCoffee.GetDescription()} = ${myCoffee.GetCost()}");

        myCoffee = new MilkDecorator(myCoffee);
        myCoffee = new SugarDecorator(myCoffee);

        Console.WriteLine($"{myCoffee.GetDescription()} = ${myCoffee.GetCost()}");
    }
}
```
🧾 Output
```
Simple Coffee = $5
Simple Coffee, Milk, Sugar = $7
```


<br>

### 🔍 Explanation

| Step | What Happens |
|------|----------------|
| 1️⃣ | **SimpleCoffee** is the base coffee. |
| 2️⃣ | **MilkDecorator** adds milk feature. |
| 3️⃣ | **SugarDecorator** adds sugar feature. |
| 4️⃣ | The decorators wrap the base coffee object one by one. |
| 5️⃣ | Each decorator adds its own cost and description dynamically. |

<br>

### 💡 When to Use Decorator Pattern

✅ When you want to add responsibilities **dynamically at runtime**  
✅ When **subclassing creates too many child classes**  
✅ When you need **flexibility to mix features** without touching base class  

<br>

### 🧩 Examples in Real Life Code

- **React.js:** Higher-Order Components (HOC) — `withAuth()`, `withRouter()`  
- **Java I/O:** `BufferedReader`, `InputStreamReader`, `FileReader` (all wrap each other)  
- **Python:** `@decorator` syntax (adds extra behavior to functions)

<br>

### ⚖️ Decorator Pattern — Advantages & Disadvantages

| 🔹 Advantages (সুবিধা) | 🔸 Disadvantages (অসুবিধা) |
|--------------------------|-----------------------------|
| ✅ **Follows Open/Closed Principle (OCP)** – New functionality can be added without modifying existing code.<br>🔹 পুরনো কোড না বদলে নতুন ফিচার যোগ করা যায়। | ❌ **Increased Complexity** – Too many small decorator classes make code harder to read and maintain.<br>🔹 অনেক ছোট ক্লাসের কারণে কোড জটিল হয়ে যায়। |
| ✅ **Flexible and Reusable** – Multiple decorators can be combined in different ways.<br>🔹 বিভিন্ন ডেকোরেটর মিশিয়ে নতুন ফিচার তৈরি করা যায়। | ❌ **Debugging is Hard** – Since objects are wrapped inside multiple layers, debugging becomes difficult.<br>🔹 একাধিক মোড়কের কারণে debugging কঠিন হয়ে যায়। |
| ✅ **Avoids Class Explosion** – No need to create many subclasses for every feature combination.<br>🔹 অতিরিক্ত subclass তৈরি না করেই নতুন ফিচার যোগ করা যায়। | ❌ **Order Matters** – Applying decorators in the wrong order can change the final output.<br>🔹 ডেকোরেটরের ক্রম ভুল হলে ভুল আউটপুট আসতে পারে। |
| ✅ **Runtime Behavior Modification** – Can add or remove features dynamically at runtime.<br>🔹 রানটাইমে ফিচার যোগ বা বাদ দেওয়া যায়। | ❌ **Slight Runtime Overhead** – Each decorator adds a wrapper, increasing processing layers slightly.<br>🔹 প্রতিটি মোড়কের কারণে সামান্য পারফরম্যান্স কমে যায়। |
| ✅ **Promotes Composition Over Inheritance** – Encourages flexible, maintainable design.<br>🔹 ইনহেরিটেন্সের পরিবর্তে কম্পোজিশন ব্যবহার করে, কোড আরও maintainable হয়। | ❌ **Complex Object Structure** – Understanding the flow through multiple decorators can be confusing.<br>🔹 অবজেক্ট ফ্লো জটিল হয়ে যায়। |

<br>

✨ **In Short:**  
The **Decorator Pattern** makes your code **more flexible and extendable**,  
but if overused — it can make your project **harder to read and debug**.


