#  Abstraction in OOP (C#)



### ✅ Definition


**Abstraction** means **hiding the complex internal details** and **showing only the necessary features of an object**. It helps you focus on what an object does instead of how it does it.

>Abstraction হলো OOP-এর একটি principle যা **অপ্রয়োজনীয় details লুকিয়ে রেখে গুরুত্বপূর্ণ বিষয়গুলো প্রকাশ করে।**  
- User কে দেখায় **what the object does**, **how it does** তা নয়।  

### How to achieve Abstraction in C#
1. **Abstract class**  
2. **Interface**


**Keywords related:**  
`abstract`, `extends`, `implements`



### 🛠️ Purpose
- Complex system কে **simple interface** দিয়ে উপস্থাপন করা  
- Implementation details **গোপন রাখা**  
- Code **maintainability ও reusability** বাড়ানো  

<br>

### ❓ Why Abstraction is Important
- Hide complexity: ব্যবহারকারী শুধু জানবে **কীভাবে ব্যবহার করতে হবে** (what it does)  
- Implementation details লুকিয়ে রাখা: **how it works** দেখানো হবে না  
- Example: Payment system → API call, validation, error handling UI developer কে না দেখলেও চলবে  


<br>

### ❓ What is an Abstract Class?

An **abstract class** is a class that **cannot be instantiated directly**. It may contain both:

-   **Abstract methods** (methods without a body that **must be overridden** in derived classes)  
      
    
-   **Non-abstract methods** (methods with a body, shared among all child classes)  
      
    

We use abstract classes when we want to define a **common structure** for all subclasses, but **leave the specific implementation** to the subclasses.

<br>

>- `abstract` keyword দিয়ে define করা হয়  
>- **Object create করা যায় না**  
>- Abstract methods থাকতে পারে (implementation ছাড়া method)  

<br>


**❌ Example: Payment System without abstraction**
```csharp
using System;

class Payment {
    public int Ammount { get; set; }
    public string TransactionId { get; set; }
    
    public void ValidatePayment(){
        Console.WriteLine("Payment is valid");
    }
    public void ProcessPayment(){
        Console.WriteLine("Payment is processed");
    }
    public void ShowInfo(){
        Console.WriteLine("Ammount: " + Ammount);
        Console.WriteLine("TransactionId: " + TransactionId);
    }
}
class Program
{
    public static void Main()
    {
        Payment payment = new Payment();
        payment.Ammount = 100;
        payment.TransactionId = "123456789";
        payment.ValidatePayment();
        payment.ProcessPayment();
        payment.ShowInfo();
    }
}
```
- ❌ এটিতে **Abstraction** পুরোপুরি implement হয়নি। এখানে সব কিছু exposed — অর্থাৎ user সব কিছুই দেখতে ও access করতে পারছে।

- ValidatePayment() ও ProcessPayment() সব payment system এ একইভাবে কাজ করে না।

- তাই এগুলো **abstract** করা দরকার, subclass এ implement করার জন্য।


### Abstraction-এর মূল কথা 


- Parent class এ রাখা হয় common properties/methods → যেগুলো সব ধরনের child class-এ একইভাবে থাকবে।

-   -  উদাহরণ: Amount এবং TransactionId সব payment system-এর জন্য common।

- Child class এ implement করা হয় specific behavior → যেগুলো child-এর জন্য আলাদা।

- - উদাহরণ: ValidatePayment() এবং ProcessPayment() → প্রতিটি payment system ভিন্নভাবে কাজ করে।

- এই কারণে আমরা এগুলোর জন্য **abstract method** ব্যবহার করি, যেন **child class**-গুলো তাদের নিজস্ব নিয়মে এগুলো implement করে।

#### 🔹 Key Insight

>“Abstraction allows common জিনিসগুলো parent class-এ ধরে রাখতে, আর ভিন্ন জিনিসগুলো child class-এ override করে implement করতে।”



#### ✅ Example
```C#
using System;

public abstract class Payment {
    public int Ammount { get; set; }
    public string TransactionId { get; set; }
    
    public abstract void ValidatePayment();
    public abstract void ProcessPayment();
    
    public void ShowInfo(){
        Console.WriteLine("Ammount: " + Ammount);
        Console.WriteLine("TransactionId: " + TransactionId);
    }
}

class CreditCardPayment : Payment{
    public string CardNumber { get; set; }

    public override void ValidatePayment(){
        Console.WriteLine("Validating Credit Card Payment");
    }
    public override void ProcessPayment(){
        Console.WriteLine("Processing Credit Card Payment");
    }
}

public class BkashPayment : Payment{
    public string BkashNumber { get; set; }
    
    public override void ValidatePayment(){
        Console.WriteLine("Validating Bkash Payment");
    }
    public override void ProcessPayment(){
        Console.WriteLine("Processing Bkash Payment");
    }
}

class Program
{
    public static void Main()
    {
        Payment CreditCardPayment = new CreditCardPayment(){
            Ammount = 2000,
            TransactionId = "##123456789###",
            CardNumber = "123456789949"
        };
        CreditCardPayment.ValidatePayment();
        CreditCardPayment.ProcessPayment();
        CreditCardPayment.ShowInfo();
        Console.WriteLine("");

       Payment BkashPayment = new BkashPayment(){
            Ammount = 2000,
            TransactionId = "1234567893223343",
            BkashNumber = "01812345678"
       };
        BkashPayment.ValidatePayment();
        BkashPayment.ProcessPayment();
        BkashPayment.ShowInfo();
    }
}
```
#### ✅ Output

```
Validating Credit Card Payment
Processing Credit Card Payment
Ammount: 2000
TransactionId: ##123456789###

Validating Bkash Payment
Processing Bkash Payment
Ammount: 2000
TransactionId: 1234567893223343
```

#### ✅ Key Learning Points

-   `abstract class` **cannot be instantiated directly**
    
-   Common **properties/methods** → abstract class-এ রাখা
    
-   **Abstract methods** → subclasses এ define করতে হবে, যেগুলো প্রত্যেক ধরনের behavior আলাদা
    
-   Real-life use case: Payment system where behavior varies per type
    
-   **Polymorphism in action:** Payment type variable holds different derived class objects


<br>
<br>
<br>

# Interface in C#
### ✅ Definition

An interface is a contract that defines what a class must do, but not how it will do it.
> অর্থাৎ interface-এ শুধু method-এর signature থাকে, কিন্তু implementation থাকে না।



-   It only contains method signatures (no implementation).  
      
    
-   A class that implements the interface must provide the implementation for all its methods.  
      
    
-   Interfaces support multiple inheritance (a class can implement multiple interfaces).  
      
    

Interfaces are used when you want to define common behavior across unrelated classes.


### ✅ কখন Interface ব্যবহার করবে?

-   যখন তুমি শুধু **structure বা contract** define করতে চাও।
    
-   যখন **multiple class** একই interface implement করবে।
    
-   যখন তুমি **loosely coupled** এবং **easily testable code** লিখতে চাও।


### ✅ Example
```c#
using System;

public interface Payment {
    void pay();
    void cancel();
    void status();
}

// Class 1
public class CreditCardPayment : Payment {
    public void pay() {
        Console.WriteLine("Paying with Credit Card");
    }
    public void cancel() {
        Console.WriteLine("Canceling Credit Card");
    }
    public void status() {
        Console.WriteLine("Status of Credit Card");
    }
}

// Class 2
public class BkashPayment : Payment {
    public void pay() {
        Console.WriteLine("Paying with Bkash");
    }
    public void cancel() {
        Console.WriteLine("Canceling Bkash");
    }
    public void status() {
        Console.WriteLine("Status of Bkash");
    }
}

class Program {
    public static void Main() {
        Payment payment;

        // Using Credit Card Payment
        payment = new CreditCardPayment();
        payment.pay();
        payment.cancel();
        payment.status();

        Console.WriteLine("-------------------");

        // Using Bkash Payment
        payment = new BkashPayment();
        payment.pay();
        payment.cancel();
        payment.status();
    }
}
```

### ✅ Output
```
Paying with Credit Card
Canceling Credit Card
Status of Credit Card
-------------------
Paying with Bkash
Canceling Bkash
Status of Bkash
```



### 👉 Interface vs Abstract Class:
  1. **Interface**:
    Definition: > <br>
      **Interface** হলো এমন একটি blueprint, যেখানে কোনো method signature থাকে,
      কিন্তু তার implementation থাকে না।
    Rules:
      - যদি কোনো class interface implement করে,
        তাহলে interface-এর সমস্ত method অবশ্যই `override` করতে হবে।
      - **Interface** implement করলে সব method-এর implementation দেওয়া বাধ্যতামূলক। <br>
      - **Example_Use:** ->
      যখন তুমি শুধু structure বা contract define করতে চাও, তখন interface ব্যবহার করা হয়।

  2. **Abstract_Class:** 
    Definition: > <br>
      **Abstract class** হলো এমন একটি class যেটি পুরোপুরি ব্যবহার করা যায় না,
      কিন্তু অন্য class-এর জন্য base class হতে পারে।
    Rules:
      - **Abstract class**-এর মধ্যে কিছু method abstract হতে পারে,
        যেগুলো অবশ্যই child class-এ override করতে হবে।
      - Abstract class-এ concrete methods ও থাকতে পারে,
        যেগুলো override করা বাধ্যতামূলক নয় (যদি behavior change না করতে চাও)। <br>
     - Example_Use: >
      যখন তুমি common behavior define করতে চাও কিন্তু কিছু method child class-এ 
      customize করতে হবে, তখন abstract class ব্যবহার করা হয়।

  ### 🔑 Key_Difference
   
 - "Interface implement করলে সব method override করতে হয়।"
 - "Abstract class extend করলে শুধুমাত্র abstract methods override করতে হয়,
    কিন্তু non-abstract methods override করা বাধ্যতামূলক নয়।"


### ✅ Example
```C#
// Interface
public interface IAnimal
{
    void MakeSound(); // Method signature only, no implementation
}

// Abstract Class
public abstract class Animal
{
    public abstract void MakeSound(); // Abstract method (must be overridden)

    public void Sleep() // Concrete method (no need to override)
    {
        Console.WriteLine("The animal is sleeping.");
    }
}

// Concrete Class implementing (:) Interface
public class Dog : IAnimal
{
    public void MakeSound()
    {
        Console.WriteLine("Woof!");
    }
}

// Concrete Class extending (:) Abstract Class
public class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Meow!");
    }
}
```

এখানে:

-   Dog class IAnimal interface implement করেছে এবং MakeSound() method override করেছে।  
      
    
-   Cat class Animal abstract class কে extend করেছে এবং MakeSound() method override করেছে, কিন্তু Sleep() method override করতে হয়নি, কারণ সেটি concrete method।


### ✅ C# Interface vs Abstract Class

নিচে C#-এর **Interface** এবং **Abstract Class** এর মধ্যে পার্থক্য একটি টেবিল আকারে দেওয়া হলো:

| বিষয়                | Abstract Class                                                                 | Interface                                                                 |
|----------------------|--------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| **সংজ্ঞা**           | abstract ও concrete (implemented) method থাকতে পারে                          | শুধুমাত্র method-এর signature থাকে                                        |
| **Constructor**      | constructor থাকতে পারে                                                        | constructor থাকে না                                                        |
| **Field/Variable**   | fields বা properties থাকতে পারে                                               | কোনো field/variable থাকে না (C# 8.0+ এ কিছু সীমিত static member থাকতে পারে) |
| **Access Modifier**  | method-এর access modifier নির্ধারণ করা যায়                                   | সব method ডিফল্টভাবে public                                                |
| **Method Implementation** | abstract ও implemented method দুটোই থাকতে পারে                         | কোনো method এর body (implementation) থাকে না                              |
| **Inheritance/Implementation** | একটিমাত্র abstract class extend করা যায়                           | একাধিক interface implement করা যায়                                       |
| **Override প্রয়োজন** | শুধু abstract methods override করা বাধ্যতামূলক                                | সব method implement/override করা বাধ্যতামূলক                               |
| **উদ্দেশ্য**         | code reuse + contract + কিছু common logic দেওয়ার জন্য                         | শুধুমাত্র একটি contract তৈরি করা                                           |
| **Property Support** | full property (get/set logic সহ) support করে                                | শুধুমাত্র auto-property or method declaration                             |
| **Multiple Inheritance** | না, multiple class inheritance সম্ভব নয়                                 | হ্যাঁ, interface এর multiple implementation সম্ভব                          |



✅ **Quick Summary**  
- **Abstract Class** → Base class + কিছু implementation + contract → code reuse & partial abstraction.  
- **Interface** → শুধু contract → pure abstraction & multiple inheritance সম্ভব।  


<br>

### 👉 Polymorphism এর Override বনাম Abstract Class এর Override

নিচে **Polymorphism এর Override** এবং **Abstract Class এর Override** এর মধ্যে পার্থক্য টেবিল আকারে দেওয়া হলো:

| বিষয়                     | Polymorphism এর Override                                           | Abstract Class এর Override                                |
|---------------------------|-------------------------------------------------------------------|----------------------------------------------------------|
| **উদ্দেশ্য (Purpose)**    | Parent class এর method কে child class এ নতুনভাবে define করা       | Abstract method এর বাধ্যতামূলক implementation             |
| **Method override বাধ্যতামূলক কি?** | না। শুধুমাত্র প্রয়োজন হলে override করা হয়                         | হ্যাঁ, override করা বাধ্যতামূলক (abstract method গুলোর জন্য) |
| **Parent class এ method থাকে কিনা** | থাকে এবং implementation সহ থাকে                                | abstract method গুলোর implementation থাকে না              |
| **Example**               | `virtual` method → override করে behavior change করা যায়          | `abstract` method → override না করলে error                |
| **Compile time error হয় কিনা** | না, override না করলেও চলে                                         | হ্যাঁ, override না করলে compile error                     |
| **Use case**              | Run-time behavior পরিবর্তনের জন্য                                 | Force করা child class কে method implement করতে            |


<br>

✅ **Quick Summary**  
- **Polymorphism Override (virtual → override):**  
  - Parent class এ already implementation থাকে।  
  - Child class চাইলে override করে আলাদা behavior দিতে পারে, চাইলে না-ও করতে পারে।  

- **Abstract Override:**  
  - Parent class এ কোনো implementation থাকে না (শুধু signature থাকে)।  
  - Child class **অবশ্যই** override করবে, নাহলে compile-time error হবে।  


### ✅ উদাহরণ:
### 1. Polymorphism:
```c#
class Animal {
    public virtual void Speak() {
        Console.WriteLine("Animal sound");
    }
}

class Dog : Animal {
    public override void Speak() {
        Console.WriteLine("Dog barks");
    }
}
```

>এখানে Speak() method override না করলেও চলতো। override করলে behavior change হয় (polymorphism)।

### 2. Abstract class:

```c#
abstract class Animal {
    public abstract void Speak();
}

class Dog : Animal {
    public override void Speak() {
        Console.WriteLine("Dog barks");
    }
}
```

>এখানে override করা বাধ্যতামূলক। না করলে compile-time error।

### 📌 সারসংক্ষেপে:

-   Polymorphism এর override = ঐচ্ছিক, যদি আপনি behavior change করতে চান।  
      
    
-   Abstract method এর override = বাধ্যতামূলক, কারণ base class method define ই করেনি।


### ✅ Step : 1
```c#
using System;

public interface INotify{
    public void Send();
    public void Log();
    public void Save();
}

public class EmailNotify : INotify{
    
    public string Email { get; set; }
    
    public void Send(){
        Console.WriteLine("Email Send to " + Email);
    }
    public void Log(){
        Console.WriteLine("Email Log : " + Email);
    }
    public void Save(){
        Console.WriteLine("Email Save to DB");
    }
}

public class SMSNotify : INotify{
    
    public string Phone { get; set; }

    public void Send(){
        Console.WriteLine("SMS Sending to " + Phone);
    }
    public void Log(){
        Console.WriteLine("SMS Log : " + Phone);
    }
    public void Save(){
        Console.WriteLine("SMS Save to DB");
    }
}

class Program
{
    public static void Main()
    {   
        INotify emailNotify = new EmailNotify(){
            Email = "test@test.com"
        };
        emailNotify.Send();
        emailNotify.Log();
        emailNotify.Save();

        Console.WriteLine("----------------------------");

        INotify smsNotify = new SMSNotify(){
            Phone = "010-1234-5678"
        };
        smsNotify.Send();
        smsNotify.Log();
        smsNotify.Save();
    }
}
```
### ✅ Output
```
Email Send to test@test.com
Email Log : test@test.com
Email Save to DB
----------------------------
SMS Sending to 010-1234-5678
SMS Log : 010-1234-5678
SMS Save to DB
```

#### 🔷 এই কোডটা "Notification System" simulate করছে।

"একটা Notification System বানিয়েছি যেটা Email এবং SMS এর জন্য আলাদা কাজ করে, কিন্তু একই interface (INotify) ব্যবহার করে।"


### 🛑 ei কোডে কী সমস্যা ache…….

1. **Manual Object Handling:**  
 তুমি EmailNotify ও SMSNotify আলাদা আলাদা object বানিয়ে কাজ করছিলে।  
 যেমন:  
 INotify emailNotify = new EmailNotify();  
 INotify smsNotify = new SMSNotify();

2. **Code Duplication (Repeat)**:  
 প্রতিটা object-এর জন্য বারবার method call করতে হয়েছে:  
 emailNotify.Send();  
 emailNotify.Log();  
 emailNotify.Save();

 এরপর smsNotify এর জন্য আবার সেই তিনটা call!

3. **Scalability Issue**:  
 ধরো তোমার future-এ ১০টা notification লাগবে, তাহলে ১০টা variable আর ৩০টা method call লিখতে হবে!  
 Code বড়, বিরক্তিকর আর maintenance-এ কঠিন।

4. **Extensibility কম**:  
 নতুন notification type (যেমন: WhatsAppNotify) যোগ করলে main() method-এ নতুন করে আরেকটা variable, আর ৩টা method call যুক্ত করতে হবে।

5. **Polymorphism-এর পূর্ণ ব্যবহার হয়নি**:  
 Interface টাইপ use করা হলেও, polymorphism-এর power (একসাথে অনেক object handle) ঠিকমতো ইউজ করা হয়নি।


<br>

### ✅ সমাধান , যেভাবে (List + Loop) |

1. **Collection** ব্যবহার করে সব object একসাথে রাখা :  
 INotify টাইপের List বানানো :  
 List<INotify> notifyList = new List<INotify>();

 এতে যেকোন type এর notifier (Email, SMS...) add করা যায়।

2. **Loop** দিয়ে সব কিছু একসাথে handle করা :  
 foreach(INotify notify in notifyList)  
  notify.Send();  
  notify.Log();  
  notify.Save();

 এর ফলে code কম হবে, cleaner হবে, এবং সব object-কে একসাথে handle করা হবে।

3. **Scalability** বাড়বে:  
 ১০টা object হোক বা ১০০টা, loop একবারেই handle করে ফেলবে। নতুন কিছু add করতে main code change করতে হবে না।

4. **Extensible & Maintainable**:  
 নতুন notifier add করলে শুধু List-এ নতুন object দিলেই চলবে। main method-এর logic ঠিক রাখতে হবে না।

5. **True Polymorphism in action**:  
 একটা Interface টাইপ (INotify) দিয়ে সব object কে আলাদা behavior সহ execute করা যাবে — যেটা polymorphism-এর best practice।


```c#
using System;
using System.Collections.Generic;

public interface INotify{
    public void Send();
    public void Log();
    public void Save();
}

public class EmailNotify : INotify{
    
    public string Email { get; set; }
    
    public void Send(){
        Console.WriteLine("Email Send to " + Email);
    }
    public void Log(){
        Console.WriteLine("Email Log : " + Email);
    }
    public void Save(){
        Console.WriteLine("Email Save to DB");
    }
}

public class SMSNotify : INotify{
    
    public string Phone { get; set; }
    
    public void Send(){
        Console.WriteLine("SMS Sending to " + Phone);
    }
    public void Log(){
        Console.WriteLine("SMS Log : " + Phone);
    }
    public void Save(){
        Console.WriteLine("SMS Save to DB");
    }
}


class Program
{
    public static void Main()
    {   
        IList<INotify> notifyList = new List<INotify>(){
            new EmailNotify(){Email = "test@test.com"},
            new SMSNotify(){Phone = "01012345678"}
        };

        foreach(INotify notify in notifyList){
            notify.Send();
            notify.Log();
            notify.Save();
        }
    }
    
}
```





### 🛑 ei কোডে (List<INotify> দিয়ে কাজ করেছিলাম) কী কী সমস্যা ছিল?

1. 🔁 **Code Duplication:**  
 প্রতিটা notify object-এর জন্য Main method-এ বারবার Send(), Log(), Save() কল করতে হয়েছে।

2. 🛠 **Single Responsibility Violated**:  
 Main method-এ notification প্রসেসের সব কাজ হচ্ছে — অথচ Main method-এর কাজ হওয়া উচিত orchestration/control, না যে actual কাজ করবে।

3. 🔗 **Tightly Coupled Code:**  
 Main method জানে notification কীভাবে কাজ করে।  
 যদি কাজের logic বা sequence change করতে হয়, তাহলে Main method-এ modification লাগbe।

4. ⚠️ **Maintainability Problem (Order Dependency):** 
 Send → Log → Save এই order Main method-এ লেখা ছিল।  
 যদি future-এ order পরিবর্তন করতে হয়, Main method-এ গিয়ে change করতে হতো — বারবার।  
 বড় প্রজেক্টে এটা খুব ঝামেলা হয়ে দাঁড়ায়।

5. ➕ **Extensibility কম**:  
 নতুন feature/process (যেমন Archive বা Retry) যোগ করতে গেলে Main method-এ code modify করতে হতো।

  
  <br>

✅ এখনকার সমাধান (NotifyContext ব্যবহার করে) কীভাবে উপরের সমস্যা দূর করেছে?

1. 🧼 **Clean Abstraction (NotifyContext):**  
 Send, Log, Save — সব কাজ NotifyContext.Process() method-এ গিয়ে centralized হয়েছে।

2. ✅ **SRP (Single Responsibility Principle) Maintain:**  
 Main method শুধুমাত্র object তৈরি ও প্রসেস চালায়।  
 Business logic NotifyContext class-এ স্থানান্তর হয়েছে।

3. 🔓 **Loose Coupling:**  
 Main method আর জানে না notification-এর ভিতরে কী হচ্ছে।  
 INotify interface + context object → সব কিছু abstraction এর মাধ্যমে চালানো হচ্ছে।

4. 🔁 **Order Change Easily Possible:**  
 কাজের order পরিবর্তন করতে চাইলে শুধু NotifyContext.Process() method-এ গিয়ে একবারে change করলেই হবে।

5. 📈 **Extensible & Maintainable**:  
 নতুন প্রসেস (যেমন Archive()) চাইলে শুধু NotifyContext এ add করলেই চলবে। Main method touch করতে হবে না।

6. 🧪 **Testability Improved:**
 NotifyContext class-কে সহজে আলাদা করে test করা যাবে।


```c#
using System;
using System.Collections.Generic;

public interface INotify{
    public void Send();
    public void Log();
    public void Save();
}

public class EmailNotify : INotify{

    public string Email { get; set; }

    public void Send(){
        Console.WriteLine("Email Send to " + Email);
    }
    public void Log(){
        Console.WriteLine("Email Log : " + Email);
    }
    public void Save(){
        Console.WriteLine("Email Save to DB");
    }
}

public class SMSNotify : INotify{

    public string Phone { get; set; }

    public void Send(){
        Console.WriteLine("SMS Sending to " + Phone);
    }
    public void Log(){
        Console.WriteLine("SMS Log : " + Phone);
    }
    public void Save(){
        Console.WriteLine("SMS Save to DB");
    }
}

public class NotifyContext{
    public INotify notify { get; set; } // এই লাইনটা object তৈরি করে না। এটা শুধু জায়গা তৈরি করে future এ object রাখার জন্য।
    public NotifyContext(INotify notify){
        this.notify = notify;
    }
    public void Process(){
        notify.Send();
        notify.Log();
        notify.Save();
    }
}

class Program
{
    public static void Main()
    {   
        
        INotify emailNotify = new EmailNotify(){
            Email = "test@test.com"
        };
        INotify smsNotify = new SMSNotify(){
            Phone = "09123456789"
        };
        NotifyContext emailNotifyContext = new NotifyContext(emailNotify);
        NotifyContext smsNotifyContext = new NotifyContext(smsNotify);

        IList<NotifyContext> notifyContexts = new List<NotifyContext>(){
            emailNotifyContext,
            smsNotifyContext
        };

        foreach(NotifyContext notifyContext in notifyContexts)
        {
            notifyContext.Process();
        }

    }

}
```
আমাদের এই কাঠামোতে চাইলে নতুন কোনো feature খুব সহজেই যোগ করা যায়।

যেমন, আমি যদি Push Notification যোগ করতে চাই, তবে সেটা আলাদা class বা module আকারে implement করে সহজেই সিস্টেমে integrate করা যাবে।

```C#
using System;
using System.Collections.Generic;

public interface INotify{
    public void Send();
    public void Log();
    public void Save();
}

public class EmailNotify : INotify{

    public string Email { get; set; }

    public void Send(){
        Console.WriteLine("Email Send to " + Email);
    }
    public void Log(){
        Console.WriteLine("Email Log : " + Email);
    }
    public void Save(){
        Console.WriteLine("Email Save to DB");
    }
}

public class SMSNotify : INotify{

    public string Phone { get; set; }

    public void Send(){
        Console.WriteLine("SMS Sending to " + Phone);
    }
    public void Log(){
        Console.WriteLine("SMS Log : " + Phone);
    }
    public void Save(){
        Console.WriteLine("SMS Save to DB");
    }
}

public class PushNotify : INotify{
    public string Token { get; set; }
    public void Send(){
        Console.WriteLine("Push Sending to " + Token);
    }
    public void Log(){
        Console.WriteLine("Push Log : " + Token);
    }
    public void Save(){
        Console.WriteLine("Push Save to DB");
    }
    
}


public class NotifyContext{
    public INotify notify { get; set; }
    public NotifyContext(INotify notify){
        this.notify = notify;
    }
    public void Process(){
        notify.Send();
        notify.Log();
        notify.Save();
    }
}

class Program
{
    public static void Main()
    {   
        
        INotify emailNotify = new EmailNotify(){
            Email = "test@test.com"
        };
        INotify smsNotify = new SMSNotify(){
            Phone = "09123456789"
        };

        INotify pushNotify = new PushNotify(){
            Token = "afoijfo56789gsgg493498bbvrvv"
        };
        NotifyContext emailNotifyContext = new NotifyContext(emailNotify);
        NotifyContext smsNotifyContext = new NotifyContext(smsNotify);
        NotifyContext pushNotifyContext = new NotifyContext(pushNotify);

        IList<NotifyContext> notifyContexts = new List<NotifyContext>(){
            emailNotifyContext,
            smsNotifyContext,
            pushNotifyContext
        };

        foreach(NotifyContext notifyContext in notifyContexts)
        {
            notifyContext.Process();
        }
    }
}
```

❓ কিন্তু সমস্যা কোথায়?

জটিলতা শুরু হয় তখন, যখন আমরা বলি —
"PushNotify এর ভেতরে আমি token Database এ save করব না।"

মানে, design এর সময় যদি আমরা কিছু জিনিস hard-code করে ফেলি (যেমন PushNotify class-এর ভেতরেই token store করার লজিক লিখে দিই), তবে সেটা flexibility কমিয়ে দেয়।


```c#
using System;
using System.Collections.Generic;

public interface INotify{
    public void Send();
    public void Log();
    public void Save();
}

public class EmailNotify : INotify{

    public string Email { get; set; }

    public void Send(){
        Console.WriteLine("Email Send to " + Email);
    }
    public void Log(){
        Console.WriteLine("Email Log : " + Email);
    }
    public void Save(){
        Console.WriteLine("Email Save to DB");
    }
}

public class SMSNotify : INotify{

    public string Phone { get; set; }

    public void Send(){
        Console.WriteLine("SMS Sending to " + Phone);
    }
    public void Log(){
        Console.WriteLine("SMS Log : " + Phone);
    }
    public void Save(){
        Console.WriteLine("SMS Save to DB");
    }
}

public class PushNotify : INotify{
    public string Token { get; set; }
    public void Send(){
        Console.WriteLine("Push Sending to " + Token);
    }
    public void Log(){
        Console.WriteLine("Push Log : " + Token);
    }
    public void Save(){}
    
}


public class NotifyContext{
    public INotify notify { get; set; }
    public NotifyContext(INotify notify){
        this.notify = notify;
    }
    public void Process(){
        notify.Send();
        notify.Log();
        notify.Save();
    }
}

class Program
{
    public static void Main()
    {   
        
        INotify emailNotify = new EmailNotify(){
            Email = "test@test.com"
        };
        INotify smsNotify = new SMSNotify(){
            Phone = "09123456789"
        };

        INotify pushNotify = new PushNotify(){
            Token = "afoijfo56789gsgg493498bbvrvv"
        };
        NotifyContext emailNotifyContext = new NotifyContext(emailNotify);
        NotifyContext smsNotifyContext = new NotifyContext(smsNotify);
        NotifyContext pushNotifyContext = new NotifyContext(pushNotify);

        IList<NotifyContext> notifyContexts = new List<NotifyContext>(){
            emailNotifyContext,
            smsNotifyContext,
            pushNotifyContext
        };

        foreach(NotifyContext notifyContext in notifyContexts)
        {
            notifyContext.Process();
        }
    }
}
```

আমরা NotifyContext.Process() method এ তিনটা কাজ fixed করে রেখেছি:
```c#
public void Process() {
    notify.Send();
    notify.Log();
    notify.Save();
}
```
এটা ধরেই নিচ্ছে সব ধরনের notification এই তিনটা কাজই করবে।
কিন্তু যদি নতুন কোনো notification type (যেমন PushNotify) আসে যা শুধুমাত্র Send ও Log করবে — Save করবে না,
 তাহলে forced ভাবে Save() call হবে — 
 
যেটা হয়তো প্রয়োজন নেই, কিংবা void implementation দিতে হবে — **যেটা Clean Code না।**

#### ✅ Possible Solutions:
এখন তোমার জন্য ৩টি practical solution থাকছে, তোমার use-case অনুযায়ী যেটা best fit হয় সেটা বেছে নিতে পারো:

### 🔹 Option 1: Default/Empty Implementation Approach (❌ Not Recommended for Clean Code) 
এটাকে বলা হয় **Interface Segregation problem**
PushNotify class-এ Save() method override করে শুধু empty রেখো:

```c#
public class PushNotify : INotify {
    public string Token { get; set; }

    public void Send() {
        Console.WriteLine("Push sent to " + Token);
    }

    public void Log() {
        Console.WriteLine("Push log for " + Token);
    }

    public void Save() {
        // Do nothing
    }
}
```
এটা technically কাজ করবে, কিন্তু এটা **Clean Code না** — কারণ এমন method call করা হচ্ছে যার কোনো কাজই নাই।

```c#
using System;
using System.Collections.Generic;

public interface ISend{
    public void Send();
}
public interface ILog{
    public void Log();
}
public interface ISave{
    public void Save();
}

public class EmailNotify : ISend, ILog, ISave{

    public string Email { get; set; }

    public void Send(){
        Console.WriteLine("Email Send to " + Email);
    }
    public void Log(){
        Console.WriteLine("Email Log : " + Email);
    }
    public void Save(){
        Console.WriteLine("Email Save to DB");
    }
}

public class SMSNotify : ISend, ILog, ISave{

    public string Phone { get; set; }

    public void Send(){
        Console.WriteLine("SMS Sending to " + Phone);
    }
    public void Log(){
        Console.WriteLine("SMS Log : " + Phone);
    }
    public void Save(){
        Console.WriteLine("SMS Save to DB");
    }
}

public class PushNotify : ISend, ILog{
    public string Token { get; set; }
    public void Send(){
        Console.WriteLine("Push Sending to " + Token);
    }
    public void Log(){
        Console.WriteLine("Push Log : " + Token);
    } 
    
}


public class NotifyContext{
    public ISend Notify { get; set; }
    public ILog Log { get; set; }
    public ISave Save { get; set; }

    public NotifyContext(ISend send, ILog log, ISave save)
    {
        Notify = send;
        Log = log;
        Save = save;
    }
    public void process()
    {
        Notify.Send();
        Log.Log();
        if(Save != null){
            Save.Save();
        }
    }
    
}


class Program
{
    public static void Main()
    {   
        
        NotifyContext notifyContextEmail = new NotifyContext(
            new EmailNotify(){ Email = "test@test.com" },
            new EmailNotify(){ Email = "test@test.com" },
            new EmailNotify(){ Email ="test@test.com"}
        );
        NotifyContext notifyContextSMS = new NotifyContext(
            new SMSNotify(){ Phone = "0912345678" },
            new SMSNotify(){ Phone = "0912345678" },
            new SMSNotify(){ Phone = "0912345678" }
        );
        NotifyContext notifyContextPush = new NotifyContext(
            new PushNotify(){ Token = "123456789" },
            new PushNotify(){ Token = "123456789" },
            null
        );
        notifyContextEmail.process();
        notifyContextSMS.process();
        notifyContextPush.process();

    }

}
```

###  ✅ Output
```
Email Send to test@test.com
Email Log : test@test.com
Email Save to DB
SMS Sending to 0912345678
SMS Log : 0912345678
SMS Save to DB
Push Sending to 123456789
Push Log : 123456789
```

 ✅ It can be done in another way.

```c#
using System;
using System.Collections.Generic;

public interface ISend{
    public void Send();
}
public interface ILog{
    public void Log();
}
public interface ISave{
    public void Save();
}

public class EmailNotify : ISend, ILog, ISave{

    public string Email { get; set; }

    public void Send(){
        Console.WriteLine("Email Send to " + Email);
    }
    public void Log(){
        Console.WriteLine("Email Log : " + Email);
    }
    public void Save(){
        Console.WriteLine("Email Save to DB");
    }
}

public class SMSNotify : ISend, ILog, ISave{

    public string Phone { get; set; }

    public void Send(){
        Console.WriteLine("SMS Sending to " + Phone);
    }
    public void Log(){
        Console.WriteLine("SMS Log : " + Phone);
    }
    public void Save(){
        Console.WriteLine("SMS Save to DB");
    }
}

public class PushNotify : ISend, ILog{
    public string Token { get; set; }
    public void Send(){
        Console.WriteLine("Push Sending to " + Token);
    }
    public void Log(){
        Console.WriteLine("Push Log : " + Token);
    } 
    
}

// Context class — বুঝে নেয় কে কোন কাজটা করতে পারে
public class NotifyContext {
    public object notify;

    public NotifyContext(object notify) {
        this.notify = notify;
    }

    public void Process() {
        if (notify is ISend sender) {
            sender.Send();
        }

        if (notify is ILog logger) {
            logger.Log();
        }

        if (notify is ISave saver) {
            saver.Save();
        }
    }
}

class Program
{
    public static void Main()
    {   

         var emailNotify = new EmailNotify() { Email = "test@test.com" };
         var smsNotify = new SMSNotify() { Phone = "09123456789" };
         var pushNotify = new PushNotify() { Token = "ABC123#4$#@#$$456" };

         // NotifyContext-এ wrap করে List এ রাখা
         IList<NotifyContext> notifyContexts = new List<NotifyContext>() {
             new NotifyContext(emailNotify),
             new NotifyContext(smsNotify),
             new NotifyContext(pushNotify)
         };

         // Loop দিয়ে Process() কল করা — কাজ অনুযায়ী যে যারটা করবে
         foreach (var context in notifyContexts) {
             context.Process();
             Console.WriteLine("----------------------------------");
         }
         
    }

}
```
### ✅ Output

```
Email Send to test@test.com
Email Log : test@test.com
Email Save to DB
----------------------------------
SMS Sending to 09123456789
SMS Log : 09123456789
SMS Save to DB
----------------------------------
Push Sending to ABC123#4$#@#$$456
Push Log : ABC123#4$#@#$$456
----------------------------------
```




[⬅️ **Previous**: Polymorpism ](./Polymorphism.md) <<---------------------------->>
[ **Next**: SOLID ➡️](./Encapsulation-and-Inheritance.md)