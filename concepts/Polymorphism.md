
#  Polymorphism in OOP

Polymorphism means **“many forms.”** It allows a method or object to behave differently depending on how it is used.

> Polymorphism শব্দটির অর্থ “বহুরূপতা”। OOP-এ, একই নামের মেথড বা অবজেক্ট বিভিন্ন রকমভাবে আচরণ করতে পারে — এটাকেই Polymorphism বলে।



### Polymorphism-এর ধরন (Types)

| ধরন                   | ব্যাখ্যা                                         |
|------------------------|--------------------------------------------------|
| Compile-time Polymorphism | Method Overloading (same method, different parameters) |
| Run-time Polymorphism     | Method Overriding (base & derived class behavior change) |



###  1. Compile-time Polymorphism (Method Overloading)

### ✅ Project: Email Sender System

```csharp
using System;

class EmailSender {
    public void SendEmail(string to, string message) {
        Console.WriteLine("Sending basic email:");
        Console.WriteLine($"To: {to}");
        Console.WriteLine($"Message: {message}");
        Console.WriteLine();
    }

    public void SendEmail(string to, string subject, string message, bool isImportant) {
        Console.WriteLine("Sending email with subject:");
        Console.WriteLine($"To: {to}");
        Console.WriteLine($"Subject: {subject}");
        Console.WriteLine($"Message: {message}");
        Console.WriteLine($"Important: {(isImportant ? "Yes" : "No")}");
        Console.WriteLine();
    }

    public void SendEmail(string to, string subject, string message, string attachmentFilePath) {
        Console.WriteLine("Sending email with attachment:");
        Console.WriteLine($"To: {to}");
        Console.WriteLine($"Subject: {subject}");
        Console.WriteLine($"Message: {message}");
        Console.WriteLine($"Attached file: {attachmentFilePath}");
        Console.WriteLine();
    }
}

class Program {
    public static void Main() {
        EmailSender sender = new EmailSender();

        sender.SendEmail("user1@example.com", "Welcome to our site!");
        sender.SendEmail("user2@example.com", "Account Activation", "Please click the link to activate.", true);
        sender.SendEmail("user3@example.com", "Project Document", "Here is your report", @"C:\files\document.pdf");
    }
}
```
✅ Output:

```
To: user1@example.com
Message: Welcome to our site!

Sending email with subject:
To: user2@example.com
Subject: Account Activation
Message: Please click the link to activate.
Important: Yes

Sending email with attachment:
To: user3@example.com
Subject: Project Document
Message: Here is your report
Attached file: C:\files\document.pdf
```

###  2. Run-time Polymorphism (Method Overriding)
### ✅ উদাহরণ: Payment System

ধরো, আমাদের একটা সফটওয়্যারে বিভিন্ন ধরণের পেমেন্ট নেওয়া হয় — যেমন Credit Card, PayPal, ইত্যাদি।

```c#
using System;

class Payment {
    public virtual void MakePayment() {
        Console.WriteLine("Processing generic payment...");
    }
}

class CreditCardPayment : Payment {
    public override void MakePayment() {
        Console.WriteLine("Processing credit card payment...");
    }
}

class PaypalPayment : Payment {
    public override void MakePayment() {
        Console.WriteLine("Processing PayPal payment...");
    }
}


class Program
{
    public static void Main()
    {
        Payment payment;

        payment = new CreditCardPayment();
        payment.MakePayment();  // Output: Processing credit card payment...

        payment = new PaypalPayment();
        payment.MakePayment();  // Output: Processing PayPal payment...

    }
}

```

### সংক্ষেপে নোট
|Modifier            |Accessible From                |
|--------------------|-------------------------------|
|`virtual`           |Base class-এ ব্যবহার হয়, যাতে child override করতে পারে             |
|`override`         |Child class-এ ব্যবহার হয়, parent method কে নতুনভাবে define করতে |
|উপকার           |Code flexible হয়, বিভিন্ন class নিজস্ব কাজ করতে পারে|


<br>

### ❓ Why is Method Overloading Called Compile-time Polymorphism?


### ✅ Answer :

Method Overloading is called **compile-time polymorphism** because the decision about which method to call is made by the **compiler during compilation**, not at runtime.  

👉 When you call an overloaded method, the compiler checks:
- The **method name**  
- The **number of parameters**  
- The **types of parameters**  

Based on these, it decides exactly which version of the method to use.  

This process is called **static binding** or **early binding** because it happens *before* the program runs.

<br>



>Method Overloading-কে **Compile-time Polymorphism** বলা হয় কারণ কোন মেথড চালু হবে সেটা প্রোগ্রাম রান হওয়ার আগে, অর্থাৎ **compile time এ compiler ঠিক করে ফেলে**।  

Compiler method name আর parameter এর type/number দেখে নির্ধারণ করে দেয় কোন মেথড version call হবে।  



### ✅  Example: Calculator Class

```csharp
public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public double Add(double a, double b)
    {
        return a + b;
    }
}

class Program
{
    public static void Main()
    {
        Calculator calc = new Calculator();

        calc.Add(5, 10);       // Calls Add(int, int)
        calc.Add(5.5, 3.2);    // Calls Add(double, double)
    }
}
```
#### 📌 Explanation:

- calc.Add(5, 10) → এখানে integer দেওয়া আছে → তাই compiler Add(int, int) বেছে নেবে।

- calc.Add(5.5, 3.2) → এখানে double দেওয়া আছে → তাই compiler Add(double, double) বেছে নেবে।

সবকিছু compiler আগেই ঠিক করে দিচ্ছে → তাই একে বলা হয় **Compile-time Polymorphism / Static Polymorphism।**
<br>

#### 📝 Summary:

- Method Overloading → multiple methods with the same name but different parameters.

- The compiler decides which method to call before execution.

- Hence, it is called compile-time polymorphism or static polymorphism.

<br>

### ❓ What is Static Binding?


### 🔹 Answer:

**Static binding** (also called **early binding**) is the process where the **compiler determines** which method or function to call at **compile time**, before the program runs.  

> **Static Binding** বা **Early Binding** মানে হলো method call কোনটা execute হবে সেটা **compiler compile time এ ঠিক করে ফেলে**।  
Runtime এ গিয়ে আর খুঁজতে হয় না। 

👉 This happens when:
- The method call is resolved based on the **reference type**  
- The method signature is known **in advance**  

Static binding is used with:
- **Method overloading**  
- **Non-virtual methods**  

ফলে program execution দ্রুত হয়।  



### ✅ Example: Calculator Class

```csharp
public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Multiply(int a, int b)
    {
        return a * b;
    }
}

class Program
{
    static void Main()
    {
        Calculator calc = new Calculator();

        int sum = calc.Add(5, 3);          // Compiler knows exactly which Add to call
        int product = calc.Multiply(4, 2); // Compiler knows exactly which Multiply to call

        Console.WriteLine(sum);
        Console.WriteLine(product);
    }
}
```
>The compiler fixes the method calls (Add and Multiply) during compilation itself — this is static binding.

#### 📌 Explanation:

- calc.Add(5, 3) → compiler already জানে Add(int, int) call হবে।

- calc.Multiply(4, 2) → compiler আগেই resolve করে দিয়েছে।

Compiler compilation stage এ fixed করে দেয় method call → এটাই হলো Static Binding।


#### 📝 Summary:
- Static binding means method calls are resolved during compilation.


- It makes program execution faster because no method lookup is needed at runtime.


- It is the opposite of dynamic binding (runtime binding), where method calls are resolved during program execution.

<br>


### ❓ Why is Method Overriding Called Runtime Polymorphism?


### ✅ Answer:

**Method overriding** is called **runtime polymorphism** because the decision about **which method to call** is made at **runtime**, not at compile time.  

👉 When a **base class reference** points to a **derived class object**, the overridden method in the derived class is called **dynamically** based on the **actual object type**.  
This is known as **dynamic binding** or **late binding**.  

<br>

**Method Overriding** কে **Runtime Polymorphism** বলা হয় কারণ method call **program চলাকালীন সময়ে (runtime)** ঠিক হয়।  
Compile time এ compiler জানে না কোন version run হবে।  

- Base class reference → যদি Derived class object কে reference করে  
- তখন overridden method → derived class এর method execute হবে  

এটাকেই বলে **Dynamic Binding / Late Binding**।  



### ✅ Example: Course Class

```csharp
public class Course
{
    public virtual void ShowDetails()
    {
        Console.WriteLine("General Course details");
    }
}

public class JobInterviewCourse : Course
{
    public override void ShowDetails()
    {
        Console.WriteLine("Job Interview Course details");
    }
}

class Program
{
    static void Main()
    {
        Course course = new JobInterviewCourse();

        // Runtime এ সিদ্ধান্ত হবে কোন ShowDetails() কল হবে
        course.ShowDetails();  
    }
}
```
>Even though course is declared as a Course type, the actual method called depends on the runtime type (JobInterviewCourse), not the compile-time type. That’s why it is called runtime polymorphism.

### 📌 Explanation:

- যদিও course → Course টাইপ দিয়ে declare করা

- কিন্তু আসল object হলো JobInterviewCourse

- তাই runtime এ JobInterviewCourse.ShowDetails() method execute হবে

এটাই হলো Runtime Polymorphism।

### 📝 Summary:

- Method Overriding → child class নিজের version দেয়।

- Method call resolve হয় → runtime এ।

- Base class reference → যদি derived object ধরে, তবে overridden method run হয়।

- একে বলে Runtime Polymorphism বা Dynamic Binding।


<br>

### ❓ What is Dynamic Binding ?



### ✅ Answer:

**Dynamic binding** (also called **late binding**) is the process where the decision about **which method to call** is made at **runtime** instead of compile time.  

👉 This usually happens in **method overriding**, where a **base class reference** points to a **derived class object**, and the program decides at runtime which method to execute based on the actual object type.  



<br>

**Dynamic Binding** হলো এমন একটি process যেখানে কোন method call হবে তা **runtime এ ঠিক হয়**।  

- সাধারণত **method overriding** এ হয়।  
- Base class reference → যদি derived class object কে point করে  
- তখন runtime এ আসল object এর overridden method execute হয়।  

এটাকেই বলে **Late Binding** বা **Dynamic Binding**।  

<br>

#### ✅ Example: Course Class

```csharp
public class Course
{
    public virtual void ShowDetails()
    {
        Console.WriteLine("General course details");
    }
}

public class CompetitiveProgrammingCourse : Course
{
    public override void ShowDetails()
    {
        Console.WriteLine("Competitive Programming course details");
    }
}

class Program
{
    static void Main()
    {
        Course course = new CompetitiveProgrammingCourse();

        // Runtime এ সিদ্ধান্ত হবে কোন ShowDetails() কল হবে
        course.ShowDetails();  
    }
}
```
### 📌 Explanation:

- course → declare করা হয়েছে Course টাইপ দিয়ে

- কিন্তু আসল object হলো → CompetitiveProgrammingCourse

- তাই runtime এ call হবে → CompetitiveProgrammingCourse.ShowDetails()

### 📝 Summary:

- Dynamic Binding = method call resolve হয় runtime এ।

- মূলত method overriding এ ব্যবহৃত হয়।

- এটি Runtime Polymorphism সম্ভব করে।

- Static Binding এর বিপরীত → যেটা compile time এ resolve হয়।

<br>

### ❓ Difference Between Method Overloading and Method Overriding ?


### ✅ Answer 

### 🔹 Method Overloading (Compile-time Polymorphism)
- 👉 Same method name, but **different parameters (number/type/order)**  
- 👉 Must be inside the **same class**  
- 👉 **Compiler** decides which method to call (compile time)  
- 👉 Known as **Early Binding / Static Binding**  

### In short:

- Overloading = same method name, different parameters, same class, decided at compile time.


- Overriding = same method signature, different class (child overrides parent), decided at runtime.


<br>

**Example (C#):**
```csharp
public class Calculator
{
    public int Add(int a, int b) 
    {
        return a + b;
    }

    public double Add(double a, double b) 
    {
        return a + b;
    }
}

class Program
{
    static void Main()
    {
        Calculator calc = new Calculator();
        Console.WriteLine(calc.Add(2, 3));       // int version
        Console.WriteLine(calc.Add(2.5, 3.7));   // double version
    }
}
```
### 🔹 Method Overriding (Runtime Polymorphism)

-   👉 Same method name & **same parameters** (method signature must match)
-   👉 Happens between **parent class & child class** 
-   👉 **Runtime** decides which method to call, based on actual object type    
-   👉 Known as **Late Binding / Dynamic Binding**

```c#
public class Parent
{
    public virtual void Show()
    {
        Console.WriteLine("Parent");
    }
}

public class Child : Parent
{
    public override void Show()
    {
        Console.WriteLine("Child");
    }
}

class Program
{
    static void Main()
    {
        Parent obj = new Child();
        obj.Show();   // Runtime এ Child class এর Show() কল হবে
    }
}
```

<br>



### ❓ What is a Virtual Method ?



### ✅  Answer
- 👉 A **virtual method** is a method in the **base class** that is declared with the `virtual` keyword.  
- 👉 This means it can be **overridden** by a **derived (child) class** using the `override` keyword.  
- 👉 It enables **runtime polymorphism** (dynamic binding).  
- 👉 At runtime, the **actual object type** decides which method will be executed, not the reference type.  

#### ✅ In short:
>A virtual method is a base class method that can be overridden in a child class to provide specific behavior, enabling dynamic method dispatch at runtime.

#### ✅ Example 

```csharp
public class Course
{
    public virtual void ShowDetails()
    {
        Console.WriteLine("General course details");
    }
}

public class JobInterviewCourse : Course
{
    public override void ShowDetails()
    {
        Console.WriteLine("Job Interview course details");
    }
}

class Program
{
    static void Main()
    {
        Course c = new JobInterviewCourse();
        c.ShowDetails(); 
        // Output: Job Interview course details
    }
}
```
 Even though c is declared as Course, runtime এ JobInterviewCourse এর overridden method execute হবে।


<br>
<br>



[⬅️ **Previous**: Encapsulation and Inheritance](./Encapsulation-and-Inheritance.md) <<---------------------------->>
[ **Next**: Abstruction and Interface ➡️](./Encapsulation-and-Inheritance.md)