# 🧬 Prototype Design Pattern

## 📌 Definition

**English:**  
Prototype is a Creational Design Pattern that allows us to create new objects by copying an existing object (prototype) instead of creating them from scratch.

**বাংলা:**  
Prototype হলো একটা Creational Design Pattern, যেখানে নতুন object তৈরি করার সময় শুরু থেকে বানানো হয় না, বরং একটা existing object (prototype) কে clone করে নতুন object বানানো হয়।

---

## 🎯 Goal / Purpose

- To avoid costly object creation from scratch.  
- To enable cloning of objects with existing state.  
- To provide a way to copy objects dynamically at runtime.  

---

## 🧩 Where It’s Used

- Object caching / memory-intensive objects  
- Game development (cloning characters, enemies, weapons)  
- Document editors (copy/paste complex objects)  
- UI components (duplicate widgets)  

---

## 🔧 How Prototype Works (Steps)

1. Create a Prototype Interface with a `clone()` method.  
2. Concrete classes implement the `clone()` method.  
3. Client calls `clone()` to get a copy of the object.  
4. New object is independent but has same state as original.  

---

## ✔️ Advantages

- Faster object creation (clone instead of `new`)  
- Reduces repetitive initialization  
- Runtime flexibility (create object dynamically)  
- Avoid subclassing for object creation  

---

## ❌ Disadvantages

- Cloning complex objects can be tricky  
- Deep copy vs shallow copy needs attention  
- Can be harder to maintain if object graph is large  

---

## 📌 Real-Life Examples (Prototype Pattern)

**Graphic Editor / Drawing Applications**  
- একটি circle, rectangle বা shape তৈরি করেছেন।  
- নতুন একই ধরনের shape বানাতে চাইলে, আবার সব property manually set করার পরিবর্তে existing shape clone করে ব্যবহার করা হয়।  
- Example: Photoshop, Illustrator এ “Duplicate Layer” feature।  

**Game Development**  
- Enemy character, weapon, vehicle ইত্যাদি clone করা হয়।  
- প্রতিবার নতুন object তৈরি না করে, existing prototype copy করা হয়।  
- Example: Video games like Clash of Clans, Fortnite—same type units multiple times generate।  

**Document Editors / Word Processors**  
- Table, Paragraph বা Chart create করা হয়েছে।  
- একই ধরনের object আবার বানাতে চাইলে, clone করে নতুন instance পাওয়া যায়।  
- Example: MS Word এ Copy + Paste complex object।  

**Web UI Components**  
- Widget, card, modal, button ইত্যাদি clone করে পুনরায় reuse করা হয়।  
- Example: React components copy করে dynamic UI generate।  

**Memory-Intensive Objects**  
- Large configuration objects বা database records clone করা হয়, instead of expensive creation.  
- Example: Cache systems বা Object Pool pattern এ use করা হয়।  


# 📌 Scenario

ধরুন কোনো কোম্পানিতে নতুন employee add করতে হবে।  
প্রতিবার নতুন employee বানাতে গেলে constructor দিয়ে সব details (department, role, base salary, benefits ইত্যাদি) লিখতে হবে।  

এতে duplication হবে + time consuming হবে।  

👉 সমাধান?  
একটা default employee profile বানানো হবে → যেটা Prototype।  
তারপর `Clone()` করে নতুন employee তৈরি করা যাবে, শুধু কিছু property (যেমন Name, EmployeeID) change করলেই হবে।  

---

## ❌ Anti-Pattern Example (Without Prototype)

```csharp
public class Employee {
    public string Name { get; set; }
    public string Department { get; set; }
    public string Role { get; set; }

    public Employee(string name, string department, string role) {
        Name = name;
        Department = department;
        Role = role;
    }

    public void ShowInfo() {
        Console.WriteLine($"Name: {Name}, Department: {Department}, Role: {Role}");
    }
}

class Program {
    static void Main() {
        // Suppose অনেক employee বানাতে হবে
        Employee emp1 = new Employee("Alice", "IT", "Developer");
        Employee emp2 = new Employee("Bob", "IT", "Developer");
        Employee emp3 = new Employee("Charlie", "IT", "Developer");

        emp1.ShowInfo();
        emp2.ShowInfo();
        emp3.ShowInfo();
    }
}
```

Output
```
Name: Alice, Department: IT, Role: Developer
Name: Bob, Department: IT, Role: Developer
Name: Charlie, Department: IT, Role: Developer
```
# ❌ Problems

- **Repetition** → বারবার constructor call করতে হচ্ছে।  
- **Performance issue** → জটিল object হলে heavy initialization।  
- **Boilerplate code** → সবসময় নতুন করে বানাতে হচ্ছে।  

---

## ✅ Solution → Prototype Pattern

- আমরা একটা Prototype object বানাবো।  
- পরে সেটা `Clone()` করে নতুন object বানাবো।  
- একই configuration এর জন্য repeated code কমে যাবে।  

---

## 📌 Implementation

```csharp
using System;

// 1. Prototype Interface
public interface IPrototype<T> {
    T Clone();
}

// 2. Concrete Class
public class Employee : IPrototype<Employee> {
    public string Name { get; set; }
    public string Department { get; set; }
    public string Role { get; set; }

    public Employee(string name, string department, string role) {
        Name = name;
        Department = department;
        Role = role;
    }

    // Clone method
    public Employee Clone() {
        return (Employee)this.MemberwiseClone(); // Shallow copy
    }

    public void ShowInfo() {
        Console.WriteLine($"Name: {Name}, Department: {Department}, Role: {Role}");
    }
}

// 3. Client
class Program {
    static void Main() {
        // Create prototype
        Employee prototype = new Employee("Default", "IT", "Developer");

        // Clone from prototype
        Employee emp1 = prototype.Clone();
        emp1.Name = "Alice";

        Employee emp2 = prototype.Clone();
        emp2.Name = "Bob";

        Employee emp3 = prototype.Clone();
        emp3.Name = "Charlie";

        emp1.ShowInfo();
        emp2.ShowInfo();
        emp3.ShowInfo();
    }
}
```
📌 Output
```
Name: Alice, Department: IT, Role: Developer
Name: Bob, Department: IT, Role: Developer
Name: Charlie, Department: IT, Role: Developer
```
# 🧬 Prototype Pattern – Deep Copy Example (Products)

## ✅ Key Points
- **Prototype = Object cloning** → নতুন object বানাতে existing object copy করা।  
- Avoids expensive initialization।  
- Reduces boilerplate code।  
- **Two types of copy:**
  - **Shallow Copy** → শুধু reference copy হয়।  
  - **Deep Copy** → nested object সহ copy হয়।  

## 📌 Use Cases
- **GUI Elements** → button, window clone করা।  
- **Game Development** → একই type এর enemy/barrier তৈরি।  
- **Business** → Default template থেকে invoice/employee profile তৈরি।  

---

## 📌 Problem with Naive Copy

```csharp
Product copy = new Product();
copy = product; // Shallow copy
```
- Tags এর মতো nested objects shared হবে → changes affect all copies

- Expensive: নতুন List, Images, etc. প্রতিটি product এ recreate করতে হবে

- Private fields direct copy করা যায় না

- Large objects → performance issue

### 📌 Solution: Clone with Deep Copy
```csharp
using System;
using System.Collections.Generic;

// Product class with Clone method
public class Product {
    public string Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Category { get; set; }
    public string Description { get; set; }
    public string ImageUrl { get; set; }
    public bool IsAvailable { get; set; }
    public int Stock { get; set; }
    public string CreatedBy { get; set; }
    public string UpdatedBy { get; set; }
    public IList<string> Tags { get; set; }

    public Product Clone() {
        Product copy = (Product)this.MemberwiseClone(); // shallow copy
        copy.Tags = new List<string>(this.Tags);        // deep copy of list
        return copy;
    }
}

// Service to handle cloning
public interface IProductService {
    IList<Product> CopyProducts(IList<Product> products);
    void ShowProduct(Product product);
}

public class ProductService : IProductService {
    public IList<Product> CopyProducts(IList<Product> products) {
        IList<Product> productsCopy = new List<Product>();
        foreach (Product product in products) {
            Product productCopy = product.Clone();
            productCopy.Id = Guid.NewGuid().ToString(); // assign new Id
            productsCopy.Add(productCopy);
        }
        return productsCopy;
    }

    public void ShowProduct(Product product) {
        Console.WriteLine("Product Id: " + product.Id);
        Console.WriteLine("Product Name: " + product.Name);
        Console.WriteLine("Product Price: " + product.Price);
        Console.WriteLine("Product Category: " + product.Category);
        Console.WriteLine("Product Description: " + product.Description);
        foreach (string tag in product.Tags) {
            Console.WriteLine("Product Tag: " + tag);
        }
    }
}

// Client code
class Program {
    public static void Main() {
        IList<Product> products = new List<Product>() {
            new Product() { Id="p1", Name="Product 1", Price=100, Category="Category 1", Description="Description 1", ImageUrl="http://localhost/images/product1.jpg", Tags=new List<string>{"Tag 1","Tag 2","Tag 3"} },
            new Product() { Id="p2", Name="Product 2", Price=200, Category="Category 2", Description="Description 2", ImageUrl="http://localhost/images/product2.jpg", Tags=new List<string>{"Tag 1","Tag 2","Tag 3"} },
            new Product() { Id="p3", Name="Product 3", Price=300, Category="Category 3", Description="Description 3", ImageUrl="http://localhost/images/product3.jpg", Tags=new List<string>{"Tag 1","Tag 2","Tag 3"} }
        };

        IProductService productService = new ProductService();
        IList<Product> productsCopy = productService.CopyProducts(products);

        Console.WriteLine("Original Products:");
        foreach(Product product in products) {
            productService.ShowProduct(product);
            Console.WriteLine("");
        }

        Console.WriteLine("Copied Products:");
        foreach(Product product in productsCopy) {
            productService.ShowProduct(product);
            Console.WriteLine("");
        }
    }
}
```
✅ Benefits
- Avoids creating multiple expensive objects manually

- Supports deep copy of nested objects (like Tags)

- Each cloned object independent from the original

- Efficient for multiple similar objects
