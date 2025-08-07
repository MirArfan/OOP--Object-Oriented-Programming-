#  Class and Object in OOP (C#)

## What is a Class?

**A class** is a blueprint or template used to create objects.  
It defines the properties (fields) and behaviors (methods) that the objects created from it will have.

**Class** হলো একটি **blueprint** যা দিয়ে আমরা Object তৈরি করি। Class-এ আমরা একটি বস্তু (object)-এর বৈশিষ্ট্য (properties) এবং আচরণ (methods) সংজ্ঞায়িত করি।

 the following Student class contains fields like name, id, and email, along with a method ShowInfo() to display student details:


✅ **উদাহরণ:**
```csharp
using System;

class Student {
    public string name;
    public int age;

    public void ShowInfo() {
        Console.WriteLine($"Name: {name}, Age: {age}");
    }
}

```
<br>

##  What is an Object?
An object is an **instance** of a class.  
It is created based on the structure defined by the class and holds real values in its fields. Using an object, we can access the class’s methods and properties.

Object হলো class-এর একটি বাস্তব রূপ (instance)। class কেবল ধারণা দেয়, কিন্তু object সেই ধারণার ভিত্তিতে তৈরি একটি আসল বস্তু। আমরা class থেকে যত খুশি ততগুলো object তৈরি করতে পারি।


For example, using the Student class:


✅ **উদাহরণ:**

```csharp
class Program {
    public static void Main(string[] args) {

        // We can create an object like this:
        Student student = new Student();

        student.name = "John";
        student.age = 20;
        student.ShowInfo();
    }
}
```
✅ **Output:** 
```
Name: Alice, Age: 20 
```

**Explanation** :
Here, student is an object of the Student class. It holds data for a specific student and can call the ShowInfo() method to display that data.
<br>
<br>

## Instance কী?
Student s1 = new Student();
এখানে s1 হচ্ছে একটি instance (বা object) — Student ক্লাসের ছাঁচ ব্যবহার করে বানানো একটি বাস্তব ছাত্র।

Instance is a concrete object created based on the structure defined by a class.
<br>
<br>


## Constructor কী?

**Constructor** হচ্ছে একটি special method যেটা কোনো ক্লাস থেকে অবজেক্ট তৈরি করার সময় **স্বয়ংক্রিয়ভাবে (automatically)** কল হয়।  
এর কাজ হলো অবজেক্ট বানানোর সময় প্রপার্টিগুলোর initial value (প্রাথমিক মান) সেট করে দেওয়া।



A **constructor** is a special method in a class that is automatically called when an object is created.  
Its purpose is to initialize the object's data (fields).  
A constructor has the **same name as the class** and does **not have any return type** — not even `void`.



###  Constructor এর ধরন (Types):

1. ✅ **Default Constructor**: Takes no parameters.

2. ✅ **Parameterized Constructor**: Takes parameters to set values during object creation.



####  সাধারণ নিয়ম:
- কনস্ট্রাক্টরের নাম ক্লাসের নামের মতোই হতে হবে।  or __constructor() // in php
- এতে **return type থাকে না** (void, int ইত্যাদি কিছুই না)।  
- চাইলে parameter সহ বা parameter ছাড়া constructor বানাতে পারো।


<br>
<br>

✅  **উদাহরণ:** (C# Code):

### 1. Default Constructor:
```csharp
class Student {
    public string name;
    public int age;

    // Constructor
    public Student() {
        name = "Unknown";
        age = 0;
    }

    public void ShowInfo() {
        Console.WriteLine("Name: {0}, Age: {1}", name, age);
    }
}

class Program {
    static void Main() {
        Student s1 = new Student();  // Constructor will be called automatically
        s1.ShowInfo();  // Output: Name: Unknown, Age: 0
    }
}
```

### 2. Parameterized Constructor (constructor e value pathano jay):
```csharp
class Student {
    public string name;
    public int age;

    // Constructor with parameters
    public Student(string n, int a) {
        name = n;
        age = a;
    }

    public void ShowInfo() {
        Console.WriteLine("Name: {0}, Age: {1}", name, age);
    }
}

class Program {
    static void Main() {
        Student s1 = new Student("Arfan", 22);
        s1.ShowInfo();  // Output: Name: Arfan, Age: 28 
    }
}

```
####  Constructor এর সুবিধা:

✅ অবজেক্ট তৈরি করার সময় ডেটা সেট করার সুযোগ দেয়।  
✅ বারবার মান সেট করতে হয় না — একবারেই initialize করা যায়।  
✅ ক্লাস use করার সময় error কম হয় কারণ values set করা থাকেই।



####  পয়েন্ট:

- Constructor শুধুমাত্র **new keyword** দিয়ে object তৈরি করার সময় call হয়।
- একই ক্লাসে তুমি **একাধিক constructor** রাখতে পারো → একে বলে **constructor overloading**।


<br>

##  Access Modifier কী?

Access modifiers in OOP define the **visibility and accessibility** of classes, methods, and variables.  
They help enforce **encapsulation** by controlling where a member can be accessed from.

**Access Modifier** নির্ধারণ করে কোন জায়গা থেকে একটি class, method, variable বা property-তে access (প্রবেশাধিকার) থাকবে।  
মানে, **কে কোন জিনিস দেখতে/ব্যবহার করতে পারবে — সেটি Access Modifier দ্বারা ঠিক করা হয়।**




####  Modifier vs Accessibility Summary Table:

| Modifier              | Accessible From                     |
|-----------------------|--------------------------------------|
| `private`             | Same class only                     |
| `protected`           | Same class and derived classes      |
| `public`              | Anywhere                            |
| `internal`            | Same project or assembly            |
| `protected internal`  | Same project or derived classes     |


####  কেন Access Modifier দরকার?

✅ Encapsulation বজায় রাখতে  
✅ Object কে বাইরের world থেকে protect করতে  
✅ শুধুমাত্র প্রয়োজন অনুযায়ী access দিতে  



#### টিপস:

- সাধারণত sensitive data `private` রাখা হয়।  
- যখন subclass access দরকার, তখন `protected` ব্যবহার করো।  
- পুরো solution থেকে access দরকার হলে `public` বা `internal` বেছে নাও।  
- একাধিক modifier combine করেও ব্যবহার করা যায় (যেমন: `protected internal`)



#### ✅ উদাহরণ (C#):
```csharp
class Student {
    public string name;          // Can be accessed from anywhere
    private int age;             // Can only be accessed inside this class
    protected string email;      // Can be accessed in this class and derived classes

    public void SetAge(int age) {
        this->age = age;
    }

    public void ShowInfo() {
        Console.WriteLine($"Name: {name}, AGE: {age}, Email: {email}");
    }

}
```

#### ✅ Usage

```csharp
class Student: Person {
    public void ShowEmail() {
        Console.WriteLine(email);  // ✅ Access from child class
    }
}

Student s1 = new Student();
s1.name = "Nafi";                 // ✅ (public)
// s1.age = 32;                   // ❌ (private) Error: 'age' is inaccessible due to its protection level

// s1.email = "a@b.com";          // ❌ Error (protected)

s1.SetAge(32);                    // ✅ (uses method to set private field)
s1.ShowInfo();


```
কিন্তু SetAge() এবং GetAge() method দিয়ে করা যাবে — এটাকেই বলে **Encapsulation**।

✅ Output:
```
Name: Nafi, AGE: 32, Email:
```

**Explanation**:
Access modifiers help restrict access to data and methods, increasing **security, reusability**, and **modularity** of your code.

