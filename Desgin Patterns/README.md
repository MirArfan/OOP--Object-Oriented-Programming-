

## 🏗️ What is a Design Pattern?

### 🔹 Definition 
A **Design Pattern** is a **proven and reusable solution** to a **common software design problem**.  
It’s not code you copy-paste — it’s a **reusable template or idea** for solving a specific type of problem.

**Design Pattern** হলো সফটওয়্যার ডিজাইনে বারবার দেখা যাওয়া সমস্যার জন্য **একটি পরীক্ষিত ও পুনর্ব্যবহারযোগ্য সমাধান (solution template)**।  
এটি কোনো নির্দিষ্ট কোড নয় — বরং একটি **আইডিয়া বা ফরম্যাট**, যা নির্দিষ্ট সমস্যার জন্য প্রয়োগ করা যায়।



<br>



### 🎯 Why Use Design Patterns?

| Reason |  Explanation |  ব্যাখ্যা |
|--------|---------------------|----------------|
| **1. Reusability** | Use tried-and-tested solutions | আগে থেকে ব্যবহৃত কার্যকর সমাধান পুনরায় ব্যবহার করা |
| **2. Maintainability** | Makes future code changes easier | ভবিষ্যতে কোড পরিবর্তন সহজ হয় |
| **3. Communication** | Provides a shared vocabulary for developers | ডেভেলপারদের মধ্যে কমন ভাষা তৈরি করে |
| **4. Efficiency** | Saves time in problem-solving | সমস্যার সমাধানে সময় কম লাগে |
| **5. Reliability** | Patterns are tested and verified | এই প্যাটার্নগুলো আগেই পরীক্ষিত ও নির্ভরযোগ্য |

<br>

### 🧩 Types of Design Patterns (Based on Functionality)

| Category | Purpose | Example Patterns |  ব্যাখ্যা |
|-----------|----------|------------------|----------------|
| **Creational** | Object তৈরির প্রক্রিয়া সহজ করা | Singleton, Factory, Builder, Prototype | অবজেক্ট তৈরি করার পদ্ধতি নির্ধারণ করে |
| **Structural** | Object ও class গুলো কীভাবে সংযুক্ত হবে তা নির্ধারণ করে | Adapter, Decorator, Facade, Proxy, Composite | কোডের গঠন বা কাঠামো নির্ধারণ করে |
| **Behavioral** | Object গুলো কিভাবে একে অপরের সাথে যোগাযোগ করবে তা নির্ধারণ করে | Observer, Strategy, Command, Iterator, State | অবজেক্টের interaction নিয়ন্ত্রণ করে |



<br>

### 🧱 Examples of Popular Design Patterns

| Pattern | Purpose |  ব্যাখ্যা |
|----------|----------|----------------|
| **Singleton** | Ensure only one instance of a class exists | একটি ক্লাসের মাত্র একটি instance তৈরি হয় |
| **Factory** | Create objects without exposing creation logic | অবজেক্ট তৈরি করার প্রক্রিয়া লুকিয়ে রাখা |
| **Observer** | Notify other objects when one changes | একটি অবজেক্ট পরিবর্তিত হলে অন্যদের জানানো |
| **Strategy** | Switch between different algorithms at runtime | রানটাইমে অ্যালগরিদম পরিবর্তন করা যায় |
| **Decorator** | Add behavior to objects dynamically | অবজেক্টে নতুন ফিচার ডাইনামিকভাবে যোগ করা |




<br>





### 🔹 Examples of Design Patterns:

| Pattern | Description |
|----------|--------------|
| **Singleton Pattern** | Ensure only one instance of a class exists |
| **Factory Pattern** | Create objects without specifying the exact class |
| **Observer Pattern** | Notify multiple objects when one changes |
| **Strategy Pattern** | Change algorithms at runtime |
| **Decorator Pattern** | Add new behavior to objects dynamically |



<br>

### ⚖️ Difference Between Design Principle & Design Pattern

| Aspect | Design Principle | Design Pattern |
|--------|------------------|----------------|
| **Focus** | Conceptual guideline | Practical solution |
| **Purpose** | Clean, maintainable code | Solve recurring design problems |
| **Level** | Theoretical | Implementation level |
| **Example** | SOLID, DRY, KISS | Singleton, Factory, Observer |



<br>




### 🧠 Simple Analogy

| Concept | Explanation |
|----------|--------------|
| **Design Principle** | Like rules of good cooking — “Use fresh ingredients”, “Don’t overcook”, “Taste before serving.” |
| **Design Pattern** | Like specific recipes — “How to make pizza”, “How to make pasta.” You apply principles to make patterns work well. |


<br>



👉 In short:  
**Principles** teach you *how to think* while designing.  
**Patterns** show you *how to implement* those ideas.