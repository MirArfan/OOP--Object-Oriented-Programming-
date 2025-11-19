# 🟫 Builder Design Pattern

### 📌 Introduction 

**Defination :**  
The Builder Pattern is used to construct complex objects step by step, allowing the same construction process to create different representations.

 
Builder Pattern এমন একটি Pattern যেখানে complex object ধাপে ধাপে তৈরি করা হয়, এবং একই construction process দিয়ে বিভিন্ন ধরনের object বানানো যায়।


### 📌 Why Needed?
-  যখন কোনো object এর অনেকগুলো optional parameter থাকে।
 -  যখন constructor খুব বড় হয়ে যায় **(telescoping constructor problem)**।
 - যখন object build করার সময় readability এবং maintainability দরকার।

<br>


### 📌 Real-Life Examples (Builder Pattern)

#### 👉 1 : Fast Food Meal Combo
একটি Restaurant-এ Meal Combo বানানো হয়:  
- Burger  
- Fries  
- Drink  

Director ঠিক করে Combo তৈরির step-by-step process, কিন্তু Builder পরিবর্তন করলে Veg Combo, Non-Veg Combo বা Kids Combo বানানো যায়।  

#### 👉 2 : Computer Assembly
একটি Laptop বা Gaming PC বানাতে বিভিন্ন component assemble করতে হয়:  
- CPU, RAM, GPU, Storage, OS  

Director ঠিক করে sequence, কিন্তু Builder পরিবর্তন করলে Gaming PC, Office PC বা Server PC তৈরি করা যায়।  

#### 👉 3 :  Document Creation
PDF, Word, HTML, Markdown file বানানো হয় step-by-step:  
- Header → Body → Footer  

Builder অনুযায়ী Final document format পরিবর্তন করা যায়।  

#### 👉 4 : House Construction
House তৈরি হয় step-by-step:  
- Foundation → Walls → Roof → Interior → Exterior  

Builder পরিবর্তন করলে Villa, Apartment, Bungalow বানানো যায়।  

#### 👉 5 : UI Component Builder
Web app এ complex UI generate করতে হয় step-by-step:  
- Navbar, Sidebar, Content, Footer  

Director + Builder pattern দিয়ে Light Theme UI বা Dark Theme UI তৈরি করা যায়।  


<br>

### 🎯 Goal / Purpose
- Separate object construction from its representation.  
- Build complex objects step by step.  
- Make code flexible, readable, and maintainable.  

<br>

### 📌 Use Cases
  -  When object has **too many optional parameters.**

 -  When object construction should be **step by step.**

 -  When different representations of the same product are needed (Gaming PC vs Office PC)

 -  When avoiding **telescoping constructor** problem.


<br>

### 🔧 How Builder Works (Steps)
1. **Builder Interface** → defines steps to build product.  
2. **Concrete Builders** → implement building steps.  
3. **Director** → controls building sequence.  
4. **Client** → uses Director to get final product.  

<br>

### ✔️ Advantages
- Step-by-step object construction  
- Same construction process → different representations  
- Improves readability & maintainability  
- Encapsulates construction code  

### ❌ Disadvantages
- More classes → slightly complex  
- Extra code for simple objects  


<br>




## 🟫 Example 1 : 

#### 📌 Scenario  
ধরো তুমি একটি **Computer / PC Building System** বানাচ্ছো।  
User বিভিন্ন ধরনের PC চাইতে পারে:

- Gaming PC  
- Office PC  
- Video Editing PC  

প্রতিটি PC-তে থাকে:

- CPU  
- RAM  
- GPU  
- Storage  
- PSU  

এগুলো **step-by-step assemble** করতে হয়।

<br>

### ❌ Bad Approach (Problem)

তুমি যদি একটি class-এর Constructor-এ সব properties পাঠাও, তাহলে কোড অনেক বড়, জটিল এবং কম readable হয়ে যাবে।

### 🔻 Problematic Code  
### (Without Builder – *Telescoping Constructor Problem*)

```csharp
using System;

public class Computer
{
    public string CPU { get; set; }
    public string RAM { get; set; }
    public string GPU { get; set; }
    public string Storage { get; set; }
    public string PSU { get; set; }

    public Computer(string cpu, string ram, string gpu, string storage, string psu)
    {
        CPU = cpu;
        RAM = ram;
        GPU = gpu;
        Storage = storage;
        PSU = psu;
    }
}

class Program
{
    static void Main()
    {
        var gamingPC = new Computer(
            "Intel i9",
            "32GB",
            "RTX 4080",
            "1TB SSD",
            "850W"
        );

        Console.WriteLine("=== BAD APPROACH OUTPUT ===");
        Console.WriteLine($"CPU: {gamingPC.CPU}");
        Console.WriteLine($"RAM: {gamingPC.RAM}");
        Console.WriteLine($"GPU: {gamingPC.GPU}");
        Console.WriteLine($"Storage: {gamingPC.Storage}");
        Console.WriteLine($"PSU: {gamingPC.PSU}");
    }
}
```
Output:
```
=== BAD APPROACH OUTPUT ===
CPU: Intel i9
RAM: 32GB
GPU: RTX 4080
Storage: 1TB SSD
PSU: 850W
```
### ❗ Issues in Bad Approach

- খুব large constructor → hard to read

- ভুল parameter order দিলে bug

- কিছু field optional হলে সমস্যা

- নতুন feature যোগ করলে constructor grow করে

- Different PC variations manage করা কঠিন

>এ কারণেই Builder Pattern ব্যবহার করা হয়।

<br>

### ✅ Builder Pattern Solution

➡️ Step by Step object তৈরির সুবিধা

➡️ Readable API

➡️ Optional part সহজে handle করা যায়

➡️ Wrong order দেওয়া যায় না

### ✔️ Builder Pattern 
```C#
using System;
///////////////////////
/// Computer Class ////
///////////////////////
public class Computer
{
    public string CPU { get; set; }
    public string RAM { get; set; }
    public string GPU { get; set; }
    public string Storage { get; set; }
    public string PSU { get; set; }

    public override string ToString()
    {
        return $"CPU: {CPU}, RAM: {RAM}, GPU: {GPU}, Storage: {Storage}, PSU: {PSU}";
    }
}
// এটি হলো Product class
// সব PC-এর common attributes এখানে define করা হয়েছে
// ToString() override করা হয়েছে যাতে simple print করা যায়


///////////////////////
//// Abstract Builder//
///////////////////////
public abstract class ComputerBuilder
{
    protected Computer computer = new Computer();

    public abstract void BuildCPU();
    public abstract void BuildRAM();
    public abstract void BuildGPU();
    public abstract void BuildStorage();
    public abstract void BuildPSU();

    public Computer GetComputer()
    {
        return computer;
    }
}

// Abstract class, যা step-by-step PC build করার method define করে
// Concrete Builder এগুলো implement করবে
// GetComputer() দিয়ে final Computer object পাওয়া যায়


///////////////////////////
//// Concrete Builders ////
////🎮 Gaming PC Builder //
////////////////////////////
public class GamingPCBuilder : ComputerBuilder
{
    public override void BuildCPU() => computer.CPU = "Intel i9";
    public override void BuildRAM() => computer.RAM = "32GB";
    public override void BuildGPU() => computer.GPU = "RTX 4080";
    public override void BuildStorage() => computer.Storage = "1TB NVMe SSD";
    public override void BuildPSU() => computer.PSU = "850W Gold";
}
////////////////////////////
/// 💼 Office PC Builder////
////////////////////////////
public class OfficePCBuilder : ComputerBuilder
{
    public override void BuildCPU() => computer.CPU = "Intel i5";
    public override void BuildRAM() => computer.RAM = "16GB";
    public override void BuildGPU() => computer.GPU = "Integrated Graphics";
    public override void BuildStorage() => computer.Storage = "512GB SSD";
    public override void BuildPSU() => computer.PSU = "500W Bronze";
}
// Step-by-step PC variation তৈরি করা হচ্ছে
// Gaming PC high-end components
// Office PC budget-friendly components



///////////////////
//// Director//////
///////////////////
public class ComputerDirector
{
    public void Build(ComputerBuilder builder)
    {
        builder.BuildCPU();
        builder.BuildRAM();
        builder.BuildGPU();
        builder.BuildStorage();
        builder.BuildPSU();
    }
}
// Director ঠিক করে কোন order এ PC assemble হবে
// কোন PC বানাবে তা জানে না → Builder dictate করে



////////////////////////////////
/// Client Code (Main Method)///
////////////////////////////////

class Program
{
    static void Main(string[] args)
    {
        var director = new ComputerDirector();

        // Build Gaming PC
        var gamingBuilder = new GamingPCBuilder();
        director.Build(gamingBuilder);
        Computer gamingPC = gamingBuilder.GetComputer();
        Console.WriteLine("Gaming PC: " + gamingPC);

        // Build Office PC
        var officeBuilder = new OfficePCBuilder();
        director.Build(officeBuilder);
        Computer officePC = officeBuilder.GetComputer();
        Console.WriteLine("Office PC: " + officePC);
    }
}
// Director তৈরি হয়েছে
// Gaming PC Builder দিয়ে Gaming PC assemble করা হয়েছে
// Office PC Builder দিয়ে Office PC assemble করা হয়েছে
// Console এ print করা হয়েছে
```

✅ Output Example
```
Gaming PC: CPU: Intel i9, RAM: 32GB, GPU: RTX 4080, Storage: 1TB NVMe SSD, PSU: 850W Gold

Office PC: CPU: Intel i5, RAM: 16GB, GPU: Integrated Graphics, Storage: 512GB SSD, PSU: 500W Bronze
```
<br>

## 🟩 Example 2 : Pizza Ordering System
📌 Scenario

ধরো তুমি একটি Pizza Ordering System বানাচ্ছো যেখানে বিভিন্ন কাস্টমাইজেশন লাগে:

- Size (Small / Medium / Large)

- Crust (Thin / Thick / Cheese Burst)

- Toppings (Olive, Mushroom, Pepperoni)

- Extra Cheese

- Sauce type

- Customers আলাদা আলাদা preference দেয়।

>👉 এখানে object তৈরি জটিল হয়, কারণ সব toppings optional এবং order আলাদা।

### ❌ Bad Approach: Telescoping Constructor Problem
🔻 Problematic Code
```c#
using System;

public class Pizza
{
    public string Size { get; set; }
    public string Crust { get; set; }
    public string Toppings { get; set; }
    public bool ExtraCheese { get; set; }
    public string Sauce { get; set; }

    public Pizza(string size, string crust, string toppings, bool extraCheese, string sauce)
    {
        Size = size;
        Crust = crust;
        Toppings = toppings;
        ExtraCheese = extraCheese;
        Sauce = sauce;
    }
}

class Program
{
    static void Main()
    {
        var pizza = new Pizza(
            "Large",
            "Cheese Burst",
            "Olive, Pepperoni",
            true,
            "Tomato"
        );

        Console.WriteLine("=== BAD APPROACH OUTPUT ===");
        Console.WriteLine("Size: " + pizza.Size);
        Console.WriteLine("Crust: " + pizza.Crust);
        Console.WriteLine("Toppings: " + pizza.Toppings);
        Console.WriteLine("Extra Cheese: " + pizza.ExtraCheese);
        Console.WriteLine("Sauce: " + pizza.Sauce);
    }
}
```
### ✅ Output

```
=== BAD APPROACH OUTPUT ===
Size: Large
Crust: Cheese Burst
Toppings: Olive, Pepperoni
Extra Cheese: True
Sauce: Tomato
```
### ❗ Problems in Bad Approach

- Constructor-এ অনেক parameter → ভুল দিলে bug

- Toppings optional হলে constructor mess হয়

- Human readable না

- নতুন feature যোগ করলে constructor বাড়তেই থাকে

### 🟢 Builder Pattern – Correct Approach
```c#
using System;
using System.Collections.Generic;

//// ✔ Step 1: Pizza Class
public class Pizza
{
    public string Size { get; set; }
    public string Crust { get; set; }
    public List<string> Toppings { get; set; } = new List<string>();
    public bool ExtraCheese { get; set; }
    public string Sauce { get; set; }

    public override string ToString()
    {
        string toppings = Toppings.Count > 0 ? string.Join(", ", Toppings) : "No Toppings";
        return $"Size: {Size}, Crust: {Crust}, Toppings: {toppings}, " +
               $"Extra Cheese: {ExtraCheese}, Sauce: {Sauce}";
    }
}
// Pizza-এর সব attributes আছে (Size, Crust, Toppings, Sauce…)
// ToString() override করে সুন্দরভাবে Pizza-এর details print করার ব্যবস্থা করা হয়েছে।



//// ✔ Step 2: Builder Base Class
public abstract class PizzaBuilder
{
    protected Pizza pizza = new Pizza();

    public abstract void SetSize();
    public abstract void SetCrust();
    public abstract void AddToppings();
    public abstract void AddSauce();
    public abstract void AddCheese();

    public Pizza GetPizza()
    {
        return pizza;
    }
}
// সব Pizza-ই কিছু common steps follow করে:

        // Size দেওয়া
        // Crust দেওয়া
        // Toppings যোগ করা
        // Sauce যোগ করা
        // Cheese যোগ করা

// তাই এই steps গুলো abstract রাখা হয়েছে।
// Concrete Builder এগুলো implement করবে।




//// ✔ Step 3: Concrete Builders
public class PepperoniPizzaBuilder : PizzaBuilder
{
    public override void SetSize() => pizza.Size = "Large";
    public override void SetCrust() => pizza.Crust = "Cheese Burst";
    public override void AddToppings()
    {
        pizza.Toppings.Add("Pepperoni");
        pizza.Toppings.Add("Olives");
    }
    public override void AddSauce() => pizza.Sauce = "Tomato";
    public override void AddCheese() => pizza.ExtraCheese = true;
}

// Large Cheese-Burst Pizza
// Toppings: Pepperoni + Olives
// Extra Cheese: true



public class VeggiePizzaBuilder : PizzaBuilder
{
    public override void SetSize() => pizza.Size = "Medium";
    public override void SetCrust() => pizza.Crust = "Thin Crust";
    public override void AddToppings()
    {
        pizza.Toppings.Add("Mushroom");
        pizza.Toppings.Add("Onion");
        pizza.Toppings.Add("Capsicum");
    }
    public override void AddSauce() => pizza.Sauce = "Pesto";
    public override void AddCheese() => pizza.ExtraCheese = false;
}
// Medium Thin-Crust Veg Pizza
// Toppings: Mushroom + Onion + Capsicum
// Extra Cheese = false



//// ✔ Step 4: Director
public class PizzaDirector
{
    public void BuildPizza(PizzaBuilder builder)
    {
        builder.SetSize();
        builder.SetCrust();
        builder.AddToppings();
        builder.AddSauce();
        builder.AddCheese();
    }
}

// Steps ঠিক কোন order এ রান হবে director তা control করে
// কোন pizza বানাবে সেটা নির্ভর করে কোন builder দেওয়া হয়েছে



//// ✔ Step 5: Client Code
class Program
{
    static void Main(string[] args)
    {
        var director = new PizzaDirector();

        var pepperoniBuilder = new PepperoniPizzaBuilder();
        director.BuildPizza(pepperoniBuilder);
        Pizza pepperoni = pepperoniBuilder.GetPizza();

        var veggieBuilder = new VeggiePizzaBuilder();
        director.BuildPizza(veggieBuilder);
        Pizza veggie = veggieBuilder.GetPizza();

        Console.WriteLine("Pepperoni Pizza → " + pepperoni);
        Console.WriteLine("Veggie Pizza → " + veggie);
    }
}

// Director ব্যবহার করে Pepperoni Pizza বানানো হলো
// তারপর Veggie Pizza বানানো হলো
// দুটোই সুন্দরভাবে print হচ্ছে
```

### 🎉 Output
```
Pepperoni Pizza → Size: Large, Crust: Cheese Burst, Toppings: Pepperoni, Olives, Extra Cheese: True, Sauce: Tomato

Veggie Pizza → Size: Medium, Crust: Thin Crust, Toppings: Mushroom, Onion, Capsicum, Extra Cheese: False, Sauce: Pesto
```