# 🧩 Template Method Design Pattern

### 1️⃣ Definition

 
Template Method Design Pattern is a **behavioral design pattern** that defines the **skeleton of an algorithm** in a base (abstract) class and allows subclasses to **override specific steps or methods** without changing the overall algorithm structure.

  
>Template Method Design Pattern একটি **behavioral design pattern**, যা একটি algorithm-এর মূল কাঠামো (**skeleton**) একটি **base** বা **abstract class**–এ নির্ধারণ করে।
এই pattern–এ **subclasses** গুলোকে নির্দিষ্ট কিছু **step / method override** করার সুযোগ দেওয়া হয়, কিন্তু algorithm-এর **overall flow** বা **structure পরিবর্তন করা যায় না**।


<br>

### 2️⃣ Why Needed?

- Same algorithm structure, but some steps vary
- Want to avoid **copy-paste code**
- Want centralized control over algorithm flow

👉 In Template Method Pattern:

- Common flow is defined in the **parent class**
- Variable steps are handled by **child classes**

<br>

### 3️⃣ Goal & Purpose

### 🎯 Goal
- Keep the **algorithm structure fixed**

### 🎯 Purpose
- Reduce code duplication
- Maintain **consistent algorithm flow**
- Follow **Open/Closed Principle (OCP)**
- Build a reusable framework
- Allow controlled customization by subclasses


<br>

### 4️⃣ Use Cases
- Framework & library development  
- CV / resume processing workflow
- Data processing pipeline  
- Game level flow (Start → Play → End)  
- Payment workflow (Validate → Pay → Notify)  
- Report generation (Fetch → Format → Print)  


<br>

### 5️⃣ Real World Examples

#### 📘 Exam Process
```
Register → Attend Exam → Result
```

>Different exam types, but the process remains the same.

#### 🍳 Cooking Recipe
```
Prepare → Cook → Serve
```

>Dish may vary, but the cooking flow is identical.

<br>

### 6️⃣ How Template Method Pattern Works

### Main Components

1️⃣ **Abstract Class**  
→ Defines the template method and overall algorithm structure  

2️⃣ **Template Method**  
→ Calls steps in a predefined order  

3️⃣ **Concrete Classes**  
→ Override specific steps

### Algorithm Structure

```
TemplateMethod()
├── Step1()
├── Step2() ← overridden by subclass
└── Step3()
```

<br>

### 7️⃣ Advantages

✅ Increases code reuse  
✅ Keeps algorithm structure consistent  
✅ Reduces duplication  
✅ Easy to maintain  
✅ Centralized control of algorithm flow  
✅ Follows Open/Closed Principle (OCP)


### 8️⃣ Disadvantages

❌ Tight coupling due to inheritance  
❌ Risk when base class changes  
❌ Too many subclasses may increase complexity  
❌ Algorithm structure is hard to change  
❌ Strong dependency on parent class

<br>
<br>

### 📄 Example 1 : Report Generation System

আমাদের system-এ বিভিন্ন ধরনের report generate করা হয়:

- PDF Report  
- Excel Report  

👉 সব report-এর **overall flow একই**:

1. Fetch Data  
2. Format Report  
3. Print / Export Report  

কিন্তু **format করার নিয়ম আলাদা**।

<br>

### ❌ Wrong Approach (Without Template Method Pattern)

এখানে প্রতিটি report class নিজের মতো করে পুরো algorithm implement করছে, যার ফলে **code duplication** হচ্ছে।

```csharp
using System;

class PdfReport
{
    public void Generate()
    {
        Console.WriteLine("Fetching data");
        Console.WriteLine("Formatting data for PDF");
        Console.WriteLine("Exporting PDF report");
    }
}

class ExcelReport
{
    public void Generate()
    {
        Console.WriteLine("Fetching data");
        Console.WriteLine("Formatting data for Excel");
        Console.WriteLine("Exporting Excel report");
    }
}

class Program
{
    static void Main()
    {
        new PdfReport().Generate();
        new ExcelReport().Generate();
    }
}
```

### ❌ Problems (Without Template Method Pattern)

- ❌ Duplicate code (Fetching data বারবার লেখা হচ্ছে)
- ❌ Algorithm structure centralized না
- ❌ New report add করলে একই flow আবার copy করতে হবে
- ❌ Maintenance hard হয়ে যায়

👉 এখানেই **Template Method Design Pattern** দরকার।



### ✅ Solution: Using Template Method Design Pattern


```csharp
using System;

// 🧱 Step 1: Abstract Base Class (Template)
abstract class ReportGenerator
{
    // Template Method (Fixed Algorithm Structure)
    public void GenerateReport()
    {
        FetchData();
        FormatReport();
        ExportReport();
    }

    // Common step (shared by all reports)
    protected void FetchData()
    {
        Console.WriteLine("Fetching data");
    }

    // Variable step (subclass must implement)
    protected abstract void FormatReport();

    // Optional step (subclass may override)
    protected virtual void ExportReport()
    {
        Console.WriteLine("Exporting report");
    }
}

// 👉 GenerateReport() → Fixed algorithm structure
// 👉 FormatReport() → Subclass customize করবে

// 🧱 Step 2: Concrete Classes
class PdfReport : ReportGenerator
{
    protected override void FormatReport()
    {
        Console.WriteLine("Formatting data for PDF");
    }

    protected override void ExportReport()
    {
        Console.WriteLine("Exporting PDF report");
    }
}

class ExcelReport : ReportGenerator
{
    protected override void FormatReport()
    {
        Console.WriteLine("Formatting data for Excel");
    }

    protected override void ExportReport()
    {
        Console.WriteLine("Exporting Excel report");
    }
}

// ▶ Usage (Main Program)
class Program
{
    static void Main()
    {
        ReportGenerator pdf = new PdfReport();
        pdf.GenerateReport();

        Console.WriteLine();

        ReportGenerator excel = new ExcelReport();
        excel.GenerateReport();
    }
}
```
### 🖥 Sample Output
```yaml
Fetching data
Formatting data for PDF
Exporting PDF report

Fetching data
Formatting data for Excel
Exporting Excel report
```

<br>

### 🍽 Example 2 : Cooking Recipe System

আমাদের দুইটা recipe আছে:

- ☕ Tea  
- ☕ Coffee  

👉 Cooking process এর **flow একই**, কিন্তু কিছু step আলাদা।

### Common Flow
1. Boil Water  
2. Brew  
3. Pour in Cup  
4. Add Condiments  

👉 **Brew** এবং **Add Condiments** step আলাদা হবে।



### ✅ Solution: Using Template Method Pattern


```csharp
using System;

// 🧱 Step 1: Abstract Class (Template)
abstract class OrderTemplate
{
    // Template Method
    public void ProcessOrder()
    {
        ValidateOrder();
        CalculatePrice();
        ProcessPayment();
        SendConfirmation();
    }

    protected void ValidateOrder()
    {
        Console.WriteLine("Order validated");
    }

    protected abstract void CalculatePrice();
    protected abstract void ProcessPayment();

    protected void SendConfirmation()
    {
        Console.WriteLine("Confirmation sent");
    }
}


// 🧱 Step 2: Concrete Classes
class RegularOrder : OrderTemplate
{
    protected override void CalculatePrice()
    {
        Console.WriteLine("Calculating regular price");
    }

    protected override void ProcessPayment()
    {
        Console.WriteLine("Processing regular payment");
    }
}

class PremiumOrder : OrderTemplate
{
    protected override void CalculatePrice()
    {
        Console.WriteLine("Calculating discounted price");
    }

    protected override void ProcessPayment()
    {
        Console.WriteLine("Processing premium payment");
    }
}

// ▶ Usage (Main Program)
class Program
{
    static void Main()
    {
        OrderTemplate regularOrder = new RegularOrder();
        Console.WriteLine("---- Regular Order ----");
        regularOrder.ProcessOrder();

        Console.WriteLine();

        OrderTemplate premiumOrder = new PremiumOrder();
        Console.WriteLine("---- Premium Order ----");
        premiumOrder.ProcessOrder();
    }
}
```
🖥 Sample Output
```yaml
---- Regular Order ----
Order validated
Calculating regular price
Processing regular payment
Confirmation sent

---- Premium Order ----
Order validated
Calculating discounted price
Processing premium payment
Confirmation sent
```






