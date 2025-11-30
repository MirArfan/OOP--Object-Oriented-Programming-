#   Encapsulation কী?

**Encapsulation** is the process of **bundling data and methods into a single unit (class)** and **protecting the data** using access modifiers.

### 🟩 Defination :
**Encapsulation** is one of the core principles of object-oriented programming. It is the process of **hiding the internal details** of an object and **only exposing necessary parts** through methods or properties.

This is typically done by making fields `private` and providing `public` **getters and setters** to access and update them.

>**Encapsulation** হলো Object-Oriented Programming-এর একটি বৈশিষ্ট্য, যেখানে **ডেটা (ভ্যারিয়েবল)** এবং সেগুলোর উপর কাজ করা **মেথড**গুলোকে একই ক্লাসে রাখা হয় এবং **ডেটাকে বাইরের দুনিয়া থেকে গোপন (Hide)** করে রাখা হয়।

Encapsulation helps in:
- **Protecting data from unauthorized access**.
- Making the code easier to maintain and modify.



#### Core Concepts:
| বিষয় | ব্যাখ্যা |
|------|----------|
| Data Hiding | প্রাইভেট ফিল্ড বাইরের ক্লাস থেকে সরাসরি দেখা যায় না |
| Getter/Setter | নির্দিষ্ট মেথড দিয়ে ডেটা রিড/আপডেট করা হয় |
| Secure Object | অবজেক্টের state শুধু অনুমোদিত উপায়ে পরিবর্তন হয় |




<!-- #### 📝 টিপস:
> Constructor না থাকলেও C#-এ একটি **default constructor** থাকে।  
অর্থাৎ তুমি যদি constructor না লেখো, C# নিজেই একটা implicit constructor দেয়। -->

<br>

### 🟩 What are Setter and Getter?
**Setters** and **getters** are user-defined methods that allow controlled access to private fields in a class.
- A **setter** sets or updates the value of a private field.
- A **getter** returns the value of a private field.
 This helps in achieving encapsulation by hiding internal data from direct access.


####  Example Code: Encapsulation in Action
```c#
class Student {
    private string name;
    private int age;

    public void setName(string name) {
        this.name = name;
    }

    public string getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getAge() {
        return age;
    }
}

class Program {
    static void Main() {
        Student student = new Student();
        student.setName("John"); // set name by setter
        student.setAge(20);     // set age by setter 

        Console.WriteLine("Name: " + student.getName()); // get name by getter
        Console.WriteLine("Age: " + student.getAge());
    }
}
```

✅ Output:
```
Name: John
Age: 20
```
####  Modern Approach (Auto-Implemented Properties):

```c#
class Student {
    public string Name { get; set; }
    public int Age { get; set; }
}
```
✅ Usage:
```c#
class Program {
    static void Main() {

        Student s = new Student();
        s.Name = "John";
        s.Age = 20;
        Console.WriteLine(s.Name);
        Console.WriteLine(s.Age);
   }
}
```
`
public string Name { get; set; }
`
এটা হচ্ছে auto-implemented property — যেখানে C# নিজেই ভিতরে `private` ফিল্ড তৈরি করে নিচ্ছে।

<br>
<br>


# 🟦 Inheritance in OOP

Inheritance is a feature of object-oriented programming where a **child class inherits properties and methods from a parent class**.  
It allows **code reuse** and helps organize related classes in a hierarchy.

>**Inheritance** হলো Object-Oriented Programming-এর এমন একটি বৈশিষ্ট্য, যার মাধ্যমে একটি ক্লাস (child/derived class) অন্য একটি ক্লাসের (parent/base class) প্রপার্টি এবং মেথডগুলো **উত্তরাধিকারসূত্রে গ্রহণ** করতে পারে।

<br>

####  মূল উদ্দেশ্য:

✅ **Code Reuse** (একবার লিখে বারবার ব্যবহার)  
✅ **Relationship তৈরি** (is-a relationship)  
✅ **Maintainability বাড়ানো**  
✅ **Code repeating** হলে Inheritance ব্যবহার করবো



#### C# এ Inheritance:

- C# মূলত **Single Inheritance** সাপোর্ট করে — মানে, একটি ক্লাস কেবল একটি parent/base ক্লাস থেকে inherit করতে পারে।  
- Multiple inheritance সরাসরি সাপোর্ট করে না, তবে **interface** দিয়ে achieve করা যায়।



### Inheritance এর ধরনসমূহ:

| Type          | Structure        | Meaning                          |
|---------------|------------------|----------------------------------|
| **Single**     | A → B            | One parent, one child           |
| **Multilevel** | A → B → C        | Chain form inheritance          |
| **Multiple**   | A + B → C        | Multiple parents                |
| **Hierarchical** | A → B, C, D    | One parent, many children       |
| **Hybrid**     | Mixed            | Combination of multiple types   |





<br>

### ✅ 1. Single Inheritance 

A child class inherits from only one parent class.

>**Single Inheritance** হলো এমন একটি সম্পর্ক যেখানে একটি ক্লাস (child) শুধুমাত্র একটি ক্লাস (parent/base) থেকে প্রপার্টি ও মেথড উত্তরাধিকারসূত্রে পায়।



#### ✅ Example Code: Single Inheritance
```c#
using System;

class Person
{
    public string Name;
    public int Age;

    public void ShowBasicInfo()
    {
        Console.WriteLine("Name: " + Name);
        Console.WriteLine("Age: " + Age);
    }
}

class Student : Person  // Single Inheritance
{
    public int Roll;
    public string Subject;

    public void ShowStudentInfo()
    {
        Console.WriteLine("Roll: " + Roll);
        Console.WriteLine("Subject: " + Subject);
    }
}

class Program
{
    static void Main()
    {
        Student s1 = new Student();

        // Parent class এর প্রপার্টি
        s1.Name = "Mir Arfan";
        s1.Age = 22;

        // Own প্রপার্টি
        s1.Roll = 101;
        s1.Subject = "Computer Science";

        // Method Call
        s1.ShowBasicInfo();      // inherited from Person
        s1.ShowStudentInfo();    // defined in Student
    }
}
```
✅ Output:
```
Name: Mir Arfan  
Age: 22  
Roll: 101  
Subject: Computer Science
```
> Constructor ব্যবহার করে parent class-এর private ফিল্ডেও মান সেট করা যায় — কিন্তু সেটা করতে হয় constructor chaining এর মাধ্যমে।

আগে মনে রাখি:

- `private` ফিল্ড child class থেকে সরাসরি access করা যায় না ❌
- কিন্তু constructor এর মাধ্যমে মান পাঠানো যায় ✅
- এজন্য parent class-এ parameterized constructor থাকতে হবে
- child class থেকে `base(...)` keyword দিয়ে সেই constructor-এ value পাঠাতে হয়

#### Constructor দিয়ে Inheritance
```C#
using System;

class Person {
    private string name;
    private int age;

    // Parameterized constructor
    public Person(string name, int age) {
        this.name = name;
        this.age = age;
    }

    public void ShowBasicInfo() {
        Console.WriteLine("Name: " + name);
        Console.WriteLine("Age: " + age);
    }
}

class Student : Person {
    private int roll;

    // Student constructor → calls parent constructor using base()
    public Student(string name, int age, int roll) : base(name, age) {
        this.roll = roll;
    }

    public void ShowStudentInfo() {
        ShowBasicInfo(); // from Person/Parent class
        Console.WriteLine("Roll: " + roll);
    }
}

class Program {
    static void Main() {
        Student s1 = new Student("Arfan", 28, 101);
        s1.ShowStudentInfo();
    }
}
```

✅ Output:
```
Name: Arfan  
Age: 22  
Roll: 101
```

#### ⚠️ Warning:
যদি parent class এ শুধু parameterized constructor থাকে,
এবং তুমি child class-এ constructor না লেখো — তাহলে error হবে।


<br>

### ✅ 2. Multilevel Inheritance (Parent → Child → Grandchild)
📌 Definition 

Inheritance happens in multiple levels, forming a chain.

```c#
using System;

class Person      // Level 1 Parent
{
    public string Name;

    public void ShowName()
    {
        Console.WriteLine("Name: " + Name);
    }
}

class Student : Person    // Level 2 Child
{
    public int Roll;

    public void ShowRoll()
    {
        Console.WriteLine("Roll: " + Roll);
    }
}

class UniversityStudent : Student   // Level 3 Child
{
    public string Major;

    public void ShowMajor()
    {
        Console.WriteLine("Major: " + Major);
    }
}

class Program
{
    static void Main()
    {
        UniversityStudent u1 = new UniversityStudent();

        u1.Name = "Arfan";      // From Person
        u1.Roll = 101;          // From Student
        u1.Major = "CSE";       // Own property

        u1.ShowName();
        u1.ShowRoll();
        u1.ShowMajor();
    }
}
```

Output :
```
Name: Arfan
Roll: 101
Major: CSE
```

<br>

### ✅ 3. Hierarchical Inheritance (One Parent → Many Children)
### 📌 Definition

Multiple child classes inherit from the same parent class.

### Example :
```c#
🧩 C# Example
using System;

class Shape     // Parent Class
{
    public string Color;

    public void ShowColor()
    {
        Console.WriteLine("Color: " + Color);
    }
}

class Circle : Shape     // Child 1
{
    public double Radius;

    public void ShowRadius()
    {
        Console.WriteLine("Radius: " + Radius);
    }
}

class Rectangle : Shape  // Child 2
{
    public double Width;
    public double Height;

    public void ShowDimensions()
    {
        Console.WriteLine("Width: " + Width);
        Console.WriteLine("Height: " + Height);
    }
}

class Program
{
    static void Main()
    {
        Circle c1 = new Circle();
        c1.Color = "Red";  // Inherited from Shape
        c1.Radius = 10;
        c1.ShowColor();
        c1.ShowRadius();

        Rectangle r1 = new Rectangle();
        r1.Color = "Blue"; // Inherited from Shape
        r1.Width = 5;
        r1.Height = 8;
        r1.ShowColor();
        r1.ShowDimensions();
    }
}
```

Output :
```
Color: Red
Radius: 10
Color: Blue
Width: 5
Height: 8
```

<br>

### ✅ 4. Multiple Inheritance (Using Interfaces)
📌 Definition 
A class inherits from multiple parents.
 
⚠️  C# does not support multiple class inheritance, but it supports multiple interface inheritance.

🧩 
Example
```c#
using System;

interface ICamera
{
    void TakePhoto();
}

interface IMusicPlayer
{
    void PlayMusic();
}

class SmartPhone : ICamera, IMusicPlayer   // Multiple Inheritance
{
    public void TakePhoto()
    {
        Console.WriteLine("Taking photo...");
    }

    public void PlayMusic()
    {
        Console.WriteLine("Playing music...");
    }
}

class Program
{
    static void Main()
    {
        SmartPhone s1 = new SmartPhone();
        s1.TakePhoto();
        s1.PlayMusic();
    }
}
```

Output :
```
Taking photo...
Playing music...
```

<br>

### ✅ 5. Hybrid Inheritance (Combination: Multilevel + Multiple)
📌 Definition 

A mix of more than one type of inheritance.

In C#, hybrid inheritance is implemented using **classes + interfaces** together.

🧩  Example
```
         Person
            |
         Employee
        /        \
   Programmer   IWorker (Interface)
            \     /
             LeadProgrammer
```

### 🧩 Example
```c#
using System;

class Person
{
    public string Name;

    public void ShowName()
    {
        Console.WriteLine("Name: " + Name);
    }
}

class Employee : Person
{
    public int EmployeeID;

    public void ShowEmployeeID()
    {
        Console.WriteLine("Employee ID: " + EmployeeID);
    }
}

interface IWorker
{
    void Work();
}

class LeadProgrammer : Employee, IWorker   // Hybrid (Class + Interface)
{
    public string Project;

    public void ShowProject()
    {
        Console.WriteLine("Project: " + Project);
    }

    public void Work()
    {
        Console.WriteLine("Working on project...");
    }
}

class Program
{
    static void Main()
    {
        LeadProgrammer lp = new LeadProgrammer();

        lp.Name = "Arfan";         // From Person
        lp.EmployeeID = 1001;      // From Employee
        lp.Project = "AI System";  // Own
       
        lp.ShowName();
        lp.ShowEmployeeID();
        lp.ShowProject();
        lp.Work();
    }
}
```
Output :
```
Name: Arfan
Employee ID: 1001
Project: AI System
Working on project...
```

<br>
<br>

[⬅️ **Previous**: Class and Object](./01_Class-and-Object.md) <<---------------------------->>
[ **Next**: Polymorphism ➡️](./03_Polymorphism.md)