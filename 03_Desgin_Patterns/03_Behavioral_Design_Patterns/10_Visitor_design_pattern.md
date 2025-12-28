# 🔹 Visitor Design Pattern

### 1️⃣ Definition

  
The Visitor Design Pattern is a **behavioral design pattern** that allows you to **add new operations or methods** to a group of related classes **without modifying their existing structures**.  

In simple words: You can introduce new functionality to classes **without changing their code**.

  
>Visitor Design Pattern একটি **behavioral design pattern**, যা আপনাকে একাধিক সম্পর্কিত class-এ নতুন **operations বা methods** যোগ করার সুযোগ দেয় তাদের মূল structure পরিবর্তন না করে।  
সহজভাবে বলতে গেলে: আপনি class-এর কোড না বদলে নতুন ফাংশনালিটি যুক্ত করতে পারবেন।

<br>

### 2️⃣ Why Visitor Pattern is Needed?

- You do not want to repeatedly add new methods to existing classes  
- Modifying existing code violates the **Open/Closed Principle (OCP)**  
- Object structure remains stable, but operations change frequently  

👉 With the **Visitor Pattern**:

- New operations are moved into separate **Visitor classes**  
- Original classes remain **unchanged** 

<br>

### 3️⃣ Goal & Purpose

#### 🎯 Goal
- Add new operations **without modifying existing classes**

#### 🎯 Purpose
- Follow the **Open/Closed Principle (OCP)**
- Centralize operation logic
- Maintain clean **Separation of Concerns** 

<br>

### 4️⃣ Use Cases

- Error log, Info log  
- Tax / Discount calculation  
- Report generation  
- Compiler AST (Syntax Tree traversal)  
- Object structure traversal  
- Maintain OCP  
- Easy to extend  

<br>

### 5️⃣ Real World Examples

- Shopping Cart → `PriceVisitor`, `DiscountVisitor`  
- Document → `PrintVisitor`, `ExportVisitor`  
- Employee → `SalaryVisitor`, `BonusVisitor`  

<br>

### 6️⃣ How Visitor Pattern Works

### Main Components

1️⃣ **Element Interface**  
→ Defines `Accept(visitor)` method  

2️⃣ **Concrete Elements**  
→ Accept the visitor  

3️⃣ **Visitor Interface**  
→ Defines `Visit` methods  

4️⃣ **Concrete Visitors**  
→ Implement actual operation  

**Flow:**
```scss
Element.accept(visitor)
        ↓
visitor.visit(Element)
```

<br>

### 7️⃣ Advantages
- ✅ New operations can be added without modifying existing classes  
- ✅ Follows Single Responsibility Principle  
- ✅ Operation logic is centralized  
- ✅ Improves maintainability and extensibility  



### 8️⃣ Disadvantages

- ❌ Adding new element types is difficult (all visitors must be updated)
- ❌ Design becomes complex
- ❌ Requires multiple classes
- ❌ Can break encapsulation
- ❌ Tight coupling between visitor and element 

<br>

### 🛒 Example 1 : Shopping Cart System

In our shopping cart system, we have different types of products:

- **Book**
- **Electronics**

Now, new requirements are coming:

- Calculate **Tax**
- Calculate **Discount**

👉 **Problem:**  
In the future, more operations may be added, such as:
- Shipping cost calculation
- Insurance cost calculation
- Loyalty points calculation  


### ❌ Wrong Approach (Without Visitor Pattern)

Each product class contains its own operation logic.

### ❌ Problematic Code

```csharp
class Book
{
    public int Price = 500;

    public double CalculateTax()
    {
        return Price * 0.05;
    }

    public double CalculateDiscount()
    {
        return Price * 0.10;
    }
}

class Electronics
{
    public int Price = 2000;

    public double CalculateTax()
    {
        return Price * 0.15;
    }

    public double CalculateDiscount()
    {
        return Price * 0.05;
    }
}
```

### ❌ Problems (Without Visitor Pattern)

- ❌ Every time a new operation is added, **all product classes must be modified**
- ❌ Violates the **Open/Closed Principle (OCP)**
- ❌ Code becomes **messy and unmaintainable**
- ❌ Business logic is tightly coupled with product classes



### ✅ Solution: Visitor Design Pattern

- All operations are moved into **separate Visitor classes**
- Product classes remain **unchanged**
- New operations can be added easily without touching existing product code

<br>

### ✅ Visitor Pattern Implementation (C#)


```csharp
using System;
using System.Collections.Generic;

// 1️⃣ Element Interface
interface IProduct
{
    void Accept(IVisitor visitor);
}

// 2️⃣ Concrete Elements
class Book : IProduct
{
    public int Price = 500;

    public void Accept(IVisitor visitor)
    {
        visitor.Visit(this);
    }
}

class Electronics : IProduct
{
    public int Price = 2000;

    public void Accept(IVisitor visitor)
    {
        visitor.Visit(this);
    }
}

// 3️⃣ Visitor Interface
interface IVisitor
{
    void Visit(Book book);
    void Visit(Electronics electronics);
}

// 4️⃣ Concrete Visitor – Tax Calculator
class TaxVisitor : IVisitor
{
    public void Visit(Book book)
    {
        Console.WriteLine($"Book Tax: {book.Price * 0.05}");
    }

    public void Visit(Electronics electronics)
    {
        Console.WriteLine($"Electronics Tax: {electronics.Price * 0.15}");
    }
}

// 5️⃣ Concrete Visitor – Discount Calculator
class DiscountVisitor : IVisitor
{
    public void Visit(Book book)
    {
        Console.WriteLine($"Book Discount: {book.Price * 0.10}");
    }

    public void Visit(Electronics electronics)
    {
        Console.WriteLine($"Electronics Discount: {electronics.Price * 0.05}");
    }
}

// 6️⃣ Client Code (Main Method)
class Program
{
    static void Main()
    {
        List<IProduct> products = new List<IProduct>
        {
            new Book(),
            new Electronics()
        };

        IVisitor taxVisitor = new TaxVisitor();
        IVisitor discountVisitor = new DiscountVisitor();

        Console.WriteLine("---- Tax Calculation ----");
        foreach (var product in products)
        {
            product.Accept(taxVisitor);
        }

        Console.WriteLine("\n---- Discount Calculation ----");
        foreach (var product in products)
        {
            product.Accept(discountVisitor);
        }
    }
}
```
🖥 Sample Output
```yaml
---- Tax Calculation ----
Book Tax: 25
Electronics Tax: 300

---- Discount Calculation ----
Book Discount: 50
Electronics Discount: 100
```

<br>

### 🔷 Example 2 : Shape System (Visitor Design Pattern)

We have different types of shapes:

- **Circle**
- **Rectangle**

New requirements keep coming:

- Calculate **Area**
- **Draw** shapes
- **Export** shapes (future)

👉 **Problem:**  
The shape structure is stable, but **operations change frequently**.



### ✅ Solution: Visitor Design Pattern

- Shape classes only know how to **accept a visitor**
- All operations are moved into **separate Visitor classes**
- Shape classes remain **unchanged**



### ✅ Visitor Pattern Implementation



```csharp
using System;
using System.Collections.Generic;

// 1️⃣ Element Interface (Shape)
interface IShape
{
    void Accept(IShapeVisitor visitor);
}

// 2️⃣ Concrete Elements
class Circle : IShape
{
    public double Radius = 5;

    public void Accept(IShapeVisitor visitor)
    {
        visitor.Visit(this);
    }
}

class Rectangle : IShape
{
    public double Width = 4;
    public double Height = 6;

    public void Accept(IShapeVisitor visitor)
    {
        visitor.Visit(this);
    }
}

// 3️⃣ Visitor Interface
interface IShapeVisitor
{
    void Visit(Circle circle);
    void Visit(Rectangle rectangle);
}

// 4️⃣ Concrete Visitor – Area Calculator
class AreaVisitor : IShapeVisitor
{
    public void Visit(Circle circle)
    {
        Console.WriteLine(
            $"Circle Area: {Math.PI * circle.Radius * circle.Radius}"
        );
    }

    public void Visit(Rectangle rectangle)
    {
        Console.WriteLine(
            $"Rectangle Area: {rectangle.Width * rectangle.Height}"
        );
    }
}

// 5️⃣ Concrete Visitor – Draw Visitor
class DrawVisitor : IShapeVisitor
{
    public void Visit(Circle circle)
    {
        Console.WriteLine("Drawing Circle");
    }

    public void Visit(Rectangle rectangle)
    {
        Console.WriteLine("Drawing Rectangle");
    }
}

// 6️⃣ Client Code (Main Method)
class Program
{
    static void Main()
    {
        List<IShape> shapes = new List<IShape>
        {
            new Circle(),
            new Rectangle()
        };

        IShapeVisitor areaVisitor = new AreaVisitor();
        IShapeVisitor drawVisitor = new DrawVisitor();

        Console.WriteLine("---- Area Calculation ----");
        foreach (var shape in shapes)
        {
            shape.Accept(areaVisitor);
        }

        Console.WriteLine("\n---- Drawing Shapes ----");
        foreach (var shape in shapes)
        {
            shape.Accept(drawVisitor);
        }
    }
}
```
### 🖥 Sample Output
```mathematica
---- Area Calculation ----
Circle Area: 78.53981633974483
Rectangle Area: 24

---- Drawing Shapes ----
Drawing Circle
Drawing Rectangle
```
