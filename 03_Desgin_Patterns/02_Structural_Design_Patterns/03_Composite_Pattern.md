# 🟦 Composition Pattern (Object Composition)



### Definition 

  
Composition is a design principle where a class is composed of one or more objects of other classes to **achieve has-a relationship**.  
It allows building complex objects by combining simple objects.

  
Composition হলো একটি design principle, যেখানে একটি class অন্য class এর object গুলি use করে তৈরি হয়।  
এটি has-a relationship নির্দেশ করে। উদাহরণ: Car has-a Engine।

<br>

### Why Needed 

- Inheritance শুধু use করা যায় না সবসময়।  
- কিছু situation এ “is-a” relationship মানানসই নয়।  
- Flexible design করতে সহজ হয়।  
- Code reuse ও loose coupling পাওয়া যায়।  

<br>

### Goal / Purpose

- Avoid d**eep inheritance hierarchies**.  
- Build **complex objects from simpler ones**
- Promote flexibility and loose coupling  
- Class-এর behavior reuse করা

<br>

### 🏠 Real-life Example

- Car has Engine, Wheels, Transmission  
- House has Door, Window, Room  
- Computer has CPU, RAM, GPU, Storage 

#### 1️⃣ File System – Folder and Files

**English:** A Folder has-a collection of Files and sub-Folders.  
**Use:** Folder delegates operations like open, delete, copy to its Files and sub-Folders.



#### 2️⃣ UI Component – Panel, Button, TextBox  

**English:** A UI Panel has-a collection of Buttons, TextBoxes, Labels, etc.  
**Use:** Panel delegates rendering, click events, and input handling to its child components.



#### 3️⃣ Company Hierarchy – Departments, Employees, Teams  

**English:** A Company has-a collection of Departments, each Department has-a Employees or Teams.  
**Use:** Company delegates work allocation, salary calculation, and team management to Departments and Employees.

#### 4️⃣ House – Rooms, Doors, Windows

**English:** A House has-a Room, Door, Window.  
**Use:** House delegates open/close doors, turn on lights in rooms, etc., to the respective objects.



####  5️⃣ Smartphone – Battery, Camera, Screen

**English:** A Smartphone has-a Battery, Camera, and Screen.  
**Use:** Battery provides power, Camera captures pictures, Screen displays output.


<br>

### 📌 Use Cases

- **GUI Design**: Window has Button, TextField, Checkbox  
- **Game Development**: Player has Weapon, Armor, Skills  
- **Business Apps**: Order has Products, Customer, ShippingInfo  
- **Computer Systems**: Computer has CPU, RAM, GPU  

<br>

### 🔧 How It Works / কিভাবে কাজ করে?

- Class A contains object(s) of Class B  
- Class A calls methods or accesses properties of Class B  
- Class B can change independently → loosely coupled system  
- Behavior of Class A depends on Class B, but doesn’t inherit  

<br>

### 🌟 Advantages / সুবিধা

- Reuse সহজ  
- Tight coupling কম  
- Inheritance hierarchy avoid করা যায়  
- Runtime behavior change করা সম্ভব  



### ❌ Disadvantages / অসুবিধা

- Slightly more complex structure  
- Object creation boilerplate বেশি  
- Deep composition hierarchy হলে harder to maintain


<br>



### 🟩 Example 1 : Restaurant Menu Card (Composition Pattern)

 

You are building a **Restaurant Menu System**.  

- Menu contains **Sections** like Starters, Main Course, Desserts. 
- Each Section contains **multiple MenuItems.**  
- Each MenuItem has **name, price, description**.  

**Why Composition?**  
Using inheritance for this is **not ideal**, because Sections and Items are **different types of objects**.  
We want:  
- Menu **has-a Section**  
- Section **has-a MenuItem**  

Composition allows building this hierarchy naturally without forcing an "is-a" relationship.



### ❌ Bad Approach (Without Composition)
```c#
using System;

public class Menu
{
    public string StartersName;
    public string MainCourseName;
    public string DessertName;
    public double StartersPrice;
    public double MainCoursePrice;
    public double DessertPrice;

    public void ShowMenu()
    {
        Console.WriteLine($"Starters: {StartersName} - {StartersPrice}");
        Console.WriteLine($"Main Course: {MainCourseName} - {MainCoursePrice}");
        Console.WriteLine($"Dessert: {DessertName} - {DessertPrice}");
    }
}

class Program
{
    static void Main()
    {
        Menu menu = new Menu();

        // Assign values
        menu.StartersName = "Spring Rolls";
        menu.StartersPrice = 5.99;

        menu.MainCourseName = "Grilled Steak";
        menu.MainCoursePrice = 18.99;

        menu.DessertName = "Vanilla Ice Cream";
        menu.DessertPrice = 4.50;

        // Show menu
        menu.ShowMenu();
    }
}
```


✅ output
```
Starters: Spring Rolls - 5.99
Main Course: Grilled Steak - 18.99
Dessert: Vanilla Ice Cream - 4.5
```
❗ **Problems**

- Menu class **does too much** → **God class**
- Hard to add more Sections or Items  
- Not flexible → Every new item requires code change in Menu  

✅ **Composition Solution (Has-a Relationship)**

- Menu has-a list of Sections  
- Section has-a list of MenuItems  
- Easy to add/remove items or sections

### ✅  Good Approach
```c#
using System;
using System.Collections.Generic;

// MenuItem Class
public class MenuItem
{
    public string Name { get; set; }
    public string Description { get; set; }
    public double Price { get; set; }

    public MenuItem(string name, string description, double price)
    {
        Name = name;
        Description = description;
        Price = price;
    }

    public void ShowItem()
    {
        Console.WriteLine($"{Name} - {Description} : ${Price}");
    }
}

// Section Class
public class Section
{
    public string Name { get; set; }
    private List<MenuItem> Items = new List<MenuItem>();

    public Section(string name)
    {
        Name = name;
    }

    public void AddItem(MenuItem item)
    {
        Items.Add(item);
    }

    public void ShowSection()
    {
        Console.WriteLine($"\n--- {Name} ---");
        foreach (var item in Items)
        {
            item.ShowItem();
        }
    }
}

// Menu Class
public class Menu
{
    private List<Section> Sections = new List<Section>();

    public void AddSection(Section section)
    {
        Sections.Add(section);
    }

    public void ShowMenu()
    {
        Console.WriteLine("==== Restaurant Menu ====");
        foreach (var section in Sections)
        {
            section.ShowSection();
        }
    }
}

// Client Code
class Program
{
    static void Main()
    {
        // Create Items
        MenuItem springRolls = new MenuItem("Spring Rolls", "Crispy vegetable rolls", 5.99);
        MenuItem steak = new MenuItem("Grilled Steak", "Juicy beef steak", 18.99);
        MenuItem iceCream = new MenuItem("Vanilla Ice Cream", "Creamy dessert", 4.50);

        // Create Sections and add Items
        Section starters = new Section("Starters");
        starters.AddItem(springRolls);

        Section mainCourse = new Section("Main Course");
        mainCourse.AddItem(steak);

        Section desserts = new Section("Desserts");
        desserts.AddItem(iceCream);

        // Create Menu and add Sections
        Menu menu = new Menu();
        menu.AddSection(starters);
        menu.AddSection(mainCourse);
        menu.AddSection(desserts);

        // Show Menu
        menu.ShowMenu();
    }
}
```

### ✅ Output
```
==== Restaurant Menu ====

--- Starters ---
Spring Rolls - Crispy vegetable rolls : $5.99

--- Main Course ---
Grilled Steak - Juicy beef steak : $18.99

--- Desserts ---
Vanilla Ice Cream - Creamy dessert : $4.5
```


📝 **Explanation**

- Menu contains multiple **Sections** → Menu **has-a Section**  
- Section contains multiple **MenuItems** → Section has-a MenuItem  
- Adding new items or sections **doesn’t require changing Menu** class  
- Flexible and **loosely coupled** design
