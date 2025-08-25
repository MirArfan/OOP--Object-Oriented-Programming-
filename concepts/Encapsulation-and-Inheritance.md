#  Encapsulation কী?

**Encapsulation** হলো Object-Oriented Programming-এর একটি বৈশিষ্ট্য, যেখানে **ডেটা (ভ্যারিয়েবল)** এবং সেগুলোর উপর কাজ করা **মেথড**গুলোকে একই ক্লাসে রাখা হয় এবং **ডেটাকে বাইরের দুনিয়া থেকে গোপন (Hide)** করে রাখা হয়।

---

####  সহজভাবে:
**Encapsulation** মানে হচ্ছে **ডেটা লুকানো ( Data hiding ) এবং কন্ট্রোলের মাধ্যমে সুরক্ষা দেওয়া।**


- এটি OOP-এর একটি **মূল বৈশিষ্ট্য**।
- ডেটা (variable) `private` করে রাখা হয়।
- বাইরের ক্লাস থেকে `public` method **(getter/setter)** দিয়ে access/control করা হয়।
- এটি ডেটাকে **সুরক্ষিত** রাখে এবং **অনাকাঙ্ক্ষিত পরিবর্তন** থেকে বাঁচায়।

<br>

**Encapsulation** is one of the core principles of object-oriented programming. It is the process of **hiding the internal details** of an object and **only exposing necessary parts** through methods or properties. This is typically done by making fields `private` and providing `public` **getters and setters** to access and update them.

Encapsulation helps in:
- **Protecting data from unauthorized access**.
- Making the code easier to maintain and modify.



#### Core Concepts:
| বিষয় | ব্যাখ্যা |
|------|----------|
| Data Hiding | প্রাইভেট ফিল্ড বাইরের ক্লাস থেকে সরাসরি দেখা যায় না |
| Getter/Setter | নির্দিষ্ট মেথড দিয়ে ডেটা রিড/আপডেট করা হয় |
| Secure Object | অবজেক্টের state শুধু অনুমোদিত উপায়ে পরিবর্তন হয় |



#### 📝 টিপস:
> Constructor না থাকলেও C#-এ একটি **default constructor** থাকে।  
অর্থাৎ তুমি যদি constructor না লেখো, C# নিজেই একটা implicit constructor দেয়।

### What are Setter and Getter?
Answer:
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


### Inheritance in OOP (C#)

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

| ধরন | ব্যাখ্যা |
|------|---------|
| **Single Inheritance** | এক ক্লাস একটার থেকে inherit করে |
| **Multi-level Inheritance** | ক্লাস B → A থেকে, ক্লাস C → B থেকে |
| **Hierarchical Inheritance** | এক parent থেকে অনেক child |
| **Multiple Inheritance** | C# এ সরাসরি নেই, তবে interface দিয়ে করা যায় |



### 1. Single Inheritance 

**Single Inheritance** হলো এমন একটি সম্পর্ক যেখানে একটি ক্লাস (child) শুধুমাত্র একটি ক্লাস (parent/base) থেকে প্রপার্টি ও মেথড উত্তরাধিকারসূত্রে পায়।



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


[⬅️ Previous: Class and Object](./class-and-object.md) <<---------------------------->>
[ Next: Polymorphism ➡️](./Polymorphism.md)