#  Class and Object in OOP

### What is a Class?

**A class** is a blueprint or template used to create objects.  
It defines the properties (fields) and behaviors (methods) that the objects created from it will have.

>**Class** হলো একটি **blueprint** যা দিয়ে আমরা Object তৈরি করি। Class-এ আমরা একটি বস্তু (object)-এর বৈশিষ্ট্য (properties) এবং আচরণ (methods) সংজ্ঞায়িত করি।

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

###  What is an Object?
An object is an **instance** of a class.

It represents a real-world entity and contains:

- State (data/attributes)

- Behavior (methods/functions)

Objects allow you to model real-world things in code.


It is created based on the structure defined by the class and holds real values in its fields. Using an object, we can access the class’s methods and properties.



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

### ❓ Instance কী?
Student s1 = new Student();
এখানে s1 হচ্ছে একটি instance (বা object) — Student ক্লাসের ছাঁচ ব্যবহার করে বানানো একটি বাস্তব ছাত্র।

Instance is a concrete object created based on the structure defined by a class.

Instance মানে হলো “class থেকে তৈরি হওয়া নির্দিষ্ট কপি।”
এই কপিটি যখন **memory তে তৈরি হয়**, তখন সেটিকে আমরা object বলি।

অর্থাৎ—

- প্রতিটি object হলো instance

- কিন্তু "instance" → class এর সাথে সম্পর্ক বোঝায়

- আর "object" → memory তে থাকা আসল জিনিস বোঝায়

**Example**:

`Car myCar = new Car()`;

- myCar হলো object, কারণ এটা মেমোরিতে আছে

- একই সাথে myCar হলো Car class এর instance

<br>

|Term |	Focus	|Meaning | 
|----|----| ----|
|Instance|	Relation to class	|A specific copy of a class|
|Object|	Existence in memory	|The instance stored in memory|

<br>


### ❓ Constructor কী ?



A **constructor** is a special method in a class that is **automatically called** when an object is created.  
Its purpose is to initialize the object's data (fields).  
A constructor has the **same name as the class** and does **not have any return type** — not even `void`.



###  Constructor এর ধরন (Types):

1. ✅ **Default Constructor**: Takes no parameters.

2. ✅ **Parameterized Constructor**: Takes parameters to set values during object creation.



####  সাধারণ নিয়ম:
- কনস্ট্রাক্টরের নাম ক্লাসের নামের মতোই হতে হবে ।  or `__constructor()`  in php
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
    public Student() 
    {
        name = "Unknown";
        age = 0;
    }

    public void ShowInfo() 
    {
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
        s1.ShowInfo();  // Output: Name: Arfan, Age: 17 
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




### ❓ Destructor কী?

**Destructor** হলো একটি special method যা **object destroy হওয়ার সময় automatically call হয়**।

>A **Destructor** is a special method in a class that is automatically called when an object is destroyed.  
Its main purpose is to release resources and perform cleanup before the object is removed from memory.


এর কাজ হলো object ব্যবহার শেষ হলে **resources clean করা** (যেমন: memory, file handle, database connection ইত্যাদি)।

সহজভাবে:

- **Constructor → Object তৈরি হওয়ার সময় call হয়**
- **Destructor → Object destroy হওয়ার সময় call হয়**

<br>

### Destructor এর সাধারণ নিয়ম (Rules)

1. Destructor এর নাম class এর নামের আগে `~` দিয়ে লিখতে হয়।
2. Destructor এ **কোন parameter থাকে না**।
3. Destructor এর **কোন return type থাকে না**।
4. Destructor **automatic ভাবে call হয়** (Garbage Collector এর মাধ্যমে)।
5. এক ক্লাসে **শুধু একটি Destructor থাকতে পারে**।

### Syntax

```csharp
~ClassName()
{
    // cleanup code
}
```



### ✅ উদাহরণ (Example in C#)

```csharp
using System;

class Student
{
    public string name;

    // Constructor
    public Student(string n)
    {
        name = n;
        Console.WriteLine("Constructor called. Object Created");
    }

    // Destructor
    ~Student()
    {
        Console.WriteLine("Destructor called. Object Destroyed");
    }

    public void ShowInfo()
    {
        Console.WriteLine("Name: " + name);
    }
}

class Program
{
    static void Main()
    {
        Student s1 = new Student("Arfan");
        s1.ShowInfo();
    }
}
```

### Output (Conceptually)

```
Constructor called. Object Created
Name: Arfan
Destructor called. Object Destroyed
```

⚠️ **Note:**  
C# এ Destructor কখন execute হবে তা নির্দিষ্ট না, কারণ এটি **Garbage Collector** এর উপর নির্ভর করে।



### Destructor এর ব্যবহার (Use Cases)

Destructor সাধারণত ব্যবহার করা হয় যখন:

- File close করতে হয়
- Database connection release করতে হয়
- Network resource free করতে হয়
- Unmanaged resources clean করতে হয়

<br>

### Constructor vs Destructor

| Constructor | Destructor |
|-------------|------------|
| Object তৈরি হলে call হয় | Object destroy হলে call হয় |
| Class name এর মতো | `~ClassName()` |
| Parameter থাকতে পারে | Parameter থাকে না |
| একাধিক থাকতে পারে | শুধু একটি |

---

#### গুরুত্বপূর্ণ পয়েন্ট

- Destructor **manually call করা যায় না**।
- Destructor **Garbage Collector call করে**।
- এক ক্লাসে **একটি মাত্র Destructor থাকতে পারে**।

<br>
<br>


### ❓ Access Modifier কী?

Access modifiers in OOP define the **visibility and accessibility** of classes, methods, and variables.  
They help enforce **encapsulation** by controlling where a member can be accessed from.

>**Access Modifier** নির্ধারণ করে কোন জায়গা থেকে একটি class, method, variable বা property-তে access (প্রবেশাধিকার) থাকবে।  
মানে, **কে কোন জিনিস দেখতে/ব্যবহার করতে পারবে — সেটি Access Modifier দ্বারা ঠিক করা হয়।**




####  Modifier vs Accessibility Summary Table:

| Modifier              | Accessible From                     |
|-----------------------|--------------------------------------|
| `private`             | Same class only                     |
| `protected`           | Same class and derived classes      |
| `public`              | Anywhere                            |
| `internal`            | Same project or assembly            |
| `protected internal`  | Same project or derived classes     |

<br>

####  কেন Access Modifier দরকার ❓

✅ Encapsulation বজায় রাখতে  
✅ Object কে বাইরের world থেকে protect করতে  
✅ শুধুমাত্র প্রয়োজন অনুযায়ী access দিতে  



#### 📝 টিপস:

- সাধারণত sensitive data `private` রাখা হয়।  
- যখন subclass access দরকার, তখন `protected` ব্যবহার করো।  
- পুরো solution থেকে access দরকার হলে `public` বা `internal` বেছে নাও।  
- একাধিক modifier combine করেও ব্যবহার করা যায় (যেমন: `protected internal`)



#### ✅ উদাহরণ (C#):
```csharp
using System;

class Person {
    public string name;          // Can be accessed from anywhere
    private int age;             // Can only be accessed inside this class
    protected string email;      // Can be accessed in this class and derived classes

    public void SetAge(int age) {
        this.age = age;
    }

    public void ShowInfo() {
        Console.WriteLine($"Name: {name}, AGE: {age}, Email: {email}");
    }

}

class Student: Person {
    public void SetEmail(string mail) {
        this.email = mail;   // allowed (protected)
    }
}
```

#### ✅ Usage

```csharp
class Program
{
    static void Main()
    {
        Student s1 = new Student();
        s1.name = "Nafi";                 // ✅ (public)
        // s1.age = 32;                   // ❌ (private) Error: 'age' is inaccessible due to its protection level

        // s1.email = "a@b.com";          // ❌ Error (protected)
        s1.SetEmail("a@b.com");         // ✅ (uses method to set protected field)
        s1.SetAge(32);                    // ✅ (uses method to set private field)
        s1.ShowInfo();
    }
}
```

কিন্তু SetAge() এবং GetAge() method দিয়ে করা যাবে — এটাকেই বলে **Encapsulation**।

✅ Output:
```
Name: Nafi, AGE: 32, Email: a@b.com
```

**Explanation**:
Access modifiers help restrict access to data and methods, increasing **security, reusability**, and **modularity** of your code.


<br>
<br>


[⬅️ **Previous**: OOP Introduction](/01_OOP) <<---------------------------->>
[ **Next**: Encapsulation and Inheritance ➡️](./02_Encapsulation-and-Inheritance.md)