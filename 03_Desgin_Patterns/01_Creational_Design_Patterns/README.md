# 🏗️ Creational Design Patterns

### 📌 Definition 
Creational Design Patterns are design patterns that focus on **how objects are created**.  
They provide flexible, reusable, and efficient ways to instantiate objects without exposing the creation logic to the client.

###  
Creational Design Pattern এমন কিছু pattern যেগুলো **object কীভাবে তৈরি হবে** সেটাকে smartভাবে manage করে।  
এগুলো object creation কে flexible, maintainable এবং loosely coupled করে তোলে।

<br>

### 🎯 Goal / Purpose of Creational Design Patterns

### ✔️
- To control object creation in a flexible way  
- To reduce code duplication during object creation  
- To hide complex construction logic  
- To make the system loosely coupled  
- To promote reusability and consistent object building 

<br>

### 🧩 Why Do We Need Creational Patterns?

### ✔️ 
- Direct object creation using `new` makes the system tightly coupled  
- Hard to change object creation logic later  
- Difficult to create families of related objects  
- Complex constructors become unreadable  (Constructor বড় হলে code বোঝা কঠিন হয় ) 
- Cloning heavy objects manually is expensive  (Heavy object বারবার বানানো ব্যয়বহুল  )


<br>

### 🧱 Types of Creational Design Patterns

Creational Pattern মোট **৫টি**:

| Pattern |  Description |  |
|--------|-------------------|--------|
| **1. Singleton** | Ensures only one instance exists | একটাই object থাকবে |
| **2. Factory Method** | Subclass decides which object to create | Object creation subclass নির্ধারণ করে |
| **3. Abstract Factory** | Creates families of related objects | Related object family তৈরি করে |
| **4. Builder** | Step-by-step object creation | জটিল object তৈরি স্টেপ বাই স্টেপ |
| **5. Prototype** | Clone existing objects | Object clone করা (copy) |

<br>

### 🧠 One-Line Summary 

- **Singleton** → One object for whole application  
  **(একটাই object পুরো system এ)**  

- **Factory Method** → Delegate object creation to subclasses  
  **(কোন object বানাবে সেটা subclass ঠিক করে)**  

- **Abstract Factory** → Create related families together  
  **(একসাথে সম্পূর্ণ object family তৈরি করে)**  

- **Builder** → Build complex object step-by-step  
  **(Step-by-step object বানানো)**  

- **Prototype** → Copy existing objects quickly  
  **(পুরনো object এর clone)**  

<br>

### 🎯 Overall Goal Summary Table

| Goal | Explanation | Explanation  |
|------|------------------------|------------------------|
| Flexibility | Change creation logic anytime | Object বানানোর নিয়ম সহজে পরিবর্তন |
| Loosely Coupled | Remove dependency on concrete classes | নতুন class পরিবর্তন করলেও পুরনো code safe থাকে |
| Reusability | Reuse construction process | Code repeat কমে |
| Simplify | Simplify complex construction | জটিল object simple ভাবে তৈরি |
| Efficiency | Avoid heavy initialization repeatedly | Heavy object বারবার বানানো avoid |

<br>

### 💬 Real-Life Examples

| Pattern | Real-Life Example |  Explanation |
|--------|-------------------|---------------------|
| Singleton | Government Prime Minister | দেশে একজনই PM → একটাই object |
| Factory Method | Pizza Shop | তুমি বলো “Pizza চাই”, shop decide করে type |
| Abstract Factory | Furniture Set (Sofa+Chair+Table) | একই brand এর সম্পূর্ণ furniture set |
| Builder | Make your own Burger | নিজের মতো করে step-by-step বানানো |
| Prototype | Photocopy Machine | Existing document এর clone |

<br>

### ✅ Summary

Creational patterns help you:  
- control object creation  
- reduce code complexity  
- increase flexibility  
- improve maintainability  
- avoid tight coupling  