# 🔹 SOLID Principles 

SOLID হলো পাঁচটা design principle যা object-oriented programming (OOP) এ ভালো software architecture বানাতে সাহায্য করে।

1. S → Single Responsibility Principle (SRP)

2. O → Open/Closed Principle (OCP)

3. L → Liskov Substitution Principle (LSP)

4. I → Interface Segregation Principle (ISP)

5. D → Dependency Inversion Principle (DIP)

<br>

## 1️⃣ S: Single Responsibility Principle (SRP)

### ✅ Definition 

- A class should have **only one reason to change.**

- Each class should have **one responsibility.**

### ❌ Example 1. ( Without SRP )
```c#
public class User
{
    public void SaveUserToDatabase() { /* DB code */ }
    public void SendEmail() { /* Email code */ }
}
```
- এক class এ DB + Email → দুই responsibility → SRP ভঙ্গ।
### ✅ Right
```c#
public class User
{
    public string Name { get; set; }
}

public class UserRepository
{
    public void Save(User user) { /* DB code */ }
}

public class EmailService
{
    public void SendEmail(User user) { /* Email code */ }
}
```

- তিনটি class → প্রতিটি class এর একটাই responsibility → SRP ফলো।


### ❌ Example 2. ( Without SRP )
```c#
class UserManager {
    public void CreateUser() { }
    public void SendEmail() { } // violates SRP
}
```

### ✅ Right
```c#
class UserManager {
    public void CreateUser() { }
}

class EmailService {
    public void SendEmail() { }
}
```

<br>
<br>

## 2️⃣ O: Open/Closed Principle (OCP)

### ✅ Definition :
A class should be **open for extension but closed for modification.**

>অর্থাৎ: নতুন feature যোগ করতে পারবে, কিন্তু existing code বারবার modify করা উচিত না।

### 🏗️  Example 1 : Payment Gateway System
ধরি তুমি একটা ওয়েব অ্যাপ বানাচ্ছো যেখানে বিকাশ, নগদ, এবং কার্ড পেমেন্ট নেওয়া হয়।

❌ ভুল ডিজাইন:
```c#
class PaymentProcessor
{
    public void ProcessPayment(string method)
    {
        if (method == "bKash")
        {
            Console.WriteLine("bKash Payment Processed.");
        }
        else if (method == "Card")
        {
            Console.WriteLine("Card Payment Processed.");
        }
    }
}

```
➡️ Problem: যদি "Nagad" বা অন্য কোনো payment method যোগ করতে হয়, তাহলে আবার কোড পরিবর্তন করতে হবে।

এতে OCP ভঙ্গ হয়।



### ✅ Correct Design (With OCP)
#### 1. Define Interface
```c#
using System;

//
// 1. Interface — open for extension
//
public interface IPaymentMethod
{
    void Process();
}

//
// 2. Implementations (Existing Payment Methods)
//
public class BkashPayment : IPaymentMethod
{
    public void Process()
    {
        Console.WriteLine("bKash Payment Processed.");
    }
}

public class CardPayment : IPaymentMethod
{
    public void Process()
    {
        Console.WriteLine("Card Payment Processed.");
    }
}

//
// 3. Payment Processor — doesn't know who is coming
//    Just calls `Process()`
//
public class PaymentProcessor
{
    public void ProcessPayment(IPaymentMethod paymentMethod)
    {
        paymentMethod.Process();
    }
}

//
// 4. New Feature Added — without modifying old code! (OCP ✓)
//
public class NagadPayment : IPaymentMethod
{
    public void Process()
    {
        Console.WriteLine("Nagad Payment Processed.");
    }
}

//
// -------- MAIN PROGRAM ----------
//
public class Program
{
    public static void Main()
    {
        PaymentProcessor processor = new PaymentProcessor();

        processor.ProcessPayment(new BkashPayment());
        processor.ProcessPayment(new CardPayment());
        processor.ProcessPayment(new NagadPayment());  // Added later — old code untouched 🔥

        Console.ReadLine();
    }
}
```

Output : 
```yaml
bKash Payment Processed.
Card Payment Processed.
Nagad Payment Processed.
```
➡️ পুরানো ক্লাস একটুও পরিবর্তন করার দরকার নেই! শুধু নতুন ক্লাস Add করলেই চলে — এটি হলো OCP এর বাস্তব প্রয়োগ।


<br>

### 🏗️  Example 2 : Notification System
#### ❌ Problem (Without OCP)
```c#
using System;
using System.Collections.Generic;

public interface INotify {
    void Send();
    void Log();
    void Save();
}

public class EmailNotify : INotify {
    public string Email { get; set; }
    public void Send() => Console.WriteLine("Email Send to " + Email);
    public void Log() => Console.WriteLine("Email Log : " + Email);
    public void Save() => Console.WriteLine("Email Save to DB");
}

public class SMSNotify : INotify {
    public string Phone { get; set; }
    public void Send() => Console.WriteLine("SMS Sending to " + Phone);
    public void Log() => Console.WriteLine("SMS Log : " + Phone);
    public void Save() => Console.WriteLine("SMS Save to DB");
}

class Program {
    public static void Main() {   
        IList<INotify> notifyList = new List<INotify>() {
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
👉 আগে যদি নতুন feature (যেমন Push Notification) add করতে চাই, তাহলে Main method এ modify করতে হবে → যা OCP ভঙ্গ করে।
### ✅ Solution (With OCP)

#### 1. Common Interface
```c#
public interface INotify {
    void Send();
    void Log();
    void Save();
}

////
//// 2. Different Notification Implementations
////

public class EmailNotify : INotify {
    public string Email { get; set; }

    public void Send() => Console.WriteLine("Email Send to : " + Email);
    public void Log() => Console.WriteLine("Email Log : " + Email);
    public void Save() => Console.WriteLine("Email Save to DB");
}

public class SMSNotify : INotify {
    public string Phone { get; set; }

    public void Send() => Console.WriteLine("SMS Sending to : " + Phone);
    public void Log() => Console.WriteLine("SMS Log : " + Phone);
    public void Save() => Console.WriteLine("SMS Save to DB");
}

public class PushNotify : INotify {
    public string Token { get; set; }
    public void Send() => Console.WriteLine("Push Sending to : " + Token);
    public void Log() => Console.WriteLine("Push Log : " + Token);
    public void Save() => Console.WriteLine("Push Save to DB");
}


////
//// 3. Context Class (Processor)
////

public class NotifyContext {
    private readonly INotify _notify;

    public NotifyContext(INotify notify) {
        _notify = notify;
    }

    public void Process() {
        _notify.Send();
        _notify.Log();
        _notify.Save();
    }
}


////
//// 4. Client Code (Main Program)
////

class Program {
    public static void Main() {
        INotify emailNotify = new EmailNotify { Email = "test@test.com" };
        INotify smsNotify = new SMSNotify { Phone = "09123456789" };
        INotify pushNotify = new PushNotify { Token = "afoijfo56789gsgg493498bbvrvv" };

        var notifyContexts = new List<NotifyContext> {
            new NotifyContext(emailNotify),
            new NotifyContext(smsNotify),
            new NotifyContext(pushNotify)
        };

        foreach (var notifyContext in notifyContexts) {
            notifyContext.Process();
        }
    }
}
```
#### Output
```yaml
Email Send to : test@test.com
Email Log : test@test.com
Email Save to DB

SMS Sending to : 09123456789
SMS Log : 09123456789
SMS Save to DB

Push Sending to : afoijfo56789gsgg493498bbvrvv
Push Log : afoijfo56789gsgg493498bbvrvv
Push Save to DB
```

আমি তোমার OCP Notification System এ নতুন একটা WhatsAppNotify class যোগ করে দেখাচ্ছি। এতে বোঝা যাবে কিভাবে OCP principle কাজ করছে।

#### 🟢 New Feature Add : WhatsApp Notification

```c#
public class WhatsAppNotify : INotify
{
    public string WhatsAppNumber { get; set; }

    public void Send()
    {
        Console.WriteLine("WhatsApp Sending to " + WhatsAppNumber);
    }

    public void Log()
    {
        Console.WriteLine("WhatsApp Log : " + WhatsAppNumber);
    }

    public void Save()
    {
        Console.WriteLine("WhatsApp Save to DB");
    }
}
```

Main Program (Client Code):
```c#
class Program
{
    public static void Main()
    {
        INotify emailNotify = new EmailNotify { Email = "test@test.com" };
        INotify smsNotify = new SMSNotify { Phone = "09123456789" };
        INotify pushNotify = new PushNotify { Token = "afoijfo56789gsgg493498bbvrvv" };
        // New feature added
        INotify whatsappNotify = new WhatsAppNotify { WhatsAppNumber = "+8801712345678" };

        var notifyContexts = new List<NotifyContext>
        {
            new NotifyContext(emailNotify),
            new NotifyContext(smsNotify),
            new NotifyContext(pushNotify),
            new NotifyContext(whatsappNotify) // ✅ New WhatsApp added
        };

        foreach (var notifyContext in notifyContexts)
        {
            notifyContext.Process();
        }
    }
}
```
#### 🌟 Output:
```yaml
Email Send to : test@test.com
Email Log : test@test.com
Email Save to DB

SMS Sending to : 09123456789
SMS Log : 09123456789
SMS Save to DB

Push Sending to : afoijfo56789gsgg493498bbvrvv
Push Log : afoijfo56789gsgg493498bbvrvv
Push Save to DB

WhatsApp Sending to : +8801712345678
WhatsApp Log : +8801712345678
WhatsApp Save to DB
```
#### 🌟 Why OCP Here?

- Open for Extension → নতুন Notification (যেমন WhatsAppNotify, SlackNotify) add করা যাবে সহজেই।

- Closed for Modification → NotifyContext বা Main method এ কোনো পরিবর্তন দরকার নেই।



<br>
<br>


## 4️⃣ I – Interface Segregation Principle (ISP)
### 🔹 Definition 

- Clients should not be forced to depend on methods they don’t use.

- অর্থাৎ: একটা বড়ো, জটিল interface না বানিয়ে ছোট ছোট specific interface বানাও।

### 🏗️ Example: Payment Gateway (ISP Violation & Fix)
ধরি আমরা আমাদের IPaymentMethod interface এ অনেকগুলো responsibility
#### ❌ Problem (Without ISP)
```c#
interface IPaymentMethod
{
    void Process();
    void Refund();   // সব payment method refund support করে না
   // void Cancel();   // সব method cancel support করে না
}

class BkashPayment : IPaymentMethod
{
    public void Process()
    {
        Console.WriteLine("bKash Payment Processed.");
    }

    // Problem: যদি bKash refund support না করে, তবুও implement করতে হলো
    public void Refund() { }
}

class CardPayment : IPaymentMethod
{
    public void Process()
    {
        Console.WriteLine("Card Payment Processed.");
    }

    public void Refund()
    {
        Console.WriteLine("Refund system Processed.");
    }
}

class NagadPayment : IPaymentMethod
{
    public void Process()
    {
        Console.WriteLine("Nagad Payment Processed.");
    }

    // Problem: If Nagad refund support না করে, তবুও implement করতে হলো
    public void Refund() { }
}
```
#### 🔴 Problem Explanation

- Interface এ Refund() method রাখা হয়েছে।

- যদি Bkash বা Nagad এ refund support না করে, তাও তাকে Refund() implement করতে হবে (হয়তো শুধু throw new NotImplementedException() দিয়ে ফাঁকি দেবে)।
- মানে class গুলোকে এমন method implement করতে বাধ্য করা হচ্ছে যা তাদের দরকার নেই → ISP violation।


### ✅ Solution (With ISP)

👉 Interface গুলোকে ছোট ছোট ভাগ করা হলো —

IPaymentProcess → শুধু Process এর জন্য।

IRefundable → Refund এর জন্য (যারা refund support করে)।
```c#
using System;

//
// ================= INTERFACES =================
//

public interface IPaymentProcess
{
    void Process();
}

public interface IRefundable
{
    void Refund();
}

//
// ============== IMPLEMENTATIONS ==============
//

public class BkashPayment : IPaymentProcess
{
    public void Process()
    {
        Console.WriteLine("bKash Payment Processed.");
    }
}

public class CardPayment : IPaymentProcess, IRefundable
{
    public void Process()
    {
        Console.WriteLine("Card Payment Processed.");
    }

    public void Refund()
    {
        Console.WriteLine("Card Refund Processed.");
    }
}

public class NagadPayment : IPaymentProcess
{
    public void Process()
    {
        Console.WriteLine("Nagad Payment Processed.");
    }
}

//
// ============== PAYMENT PROCESSOR ==============
//

public class PaymentProcessor
{
    public void ProcessPayment(IPaymentProcess paymentMethod)
    {
        paymentMethod.Process();
    }

    public void ProcessRefund(IRefundable refundableMethod)
    {
        refundableMethod.Refund();
    }
}

//
// ====================== MAIN ======================
// RUN & SEE OUTPUT
//

public class Program
{
    public static void Main()
    {
        var processor = new PaymentProcessor();

        // Processing Payments
        processor.ProcessPayment(new BkashPayment());
        processor.ProcessPayment(new CardPayment());
        processor.ProcessPayment(new NagadPayment());

        Console.WriteLine("-------------------------");

        // Refund only possible for CardPayment
        processor.ProcessRefund(new CardPayment());

        Console.ReadLine();
    }
}
```

Output : 
```yaml
bKash Payment Processed.
Card Payment Processed.
Nagad Payment Processed.
-------------------------
Card Refund Processed.
```
### 🌟 Why This Fixes ISP?

- Bkash & Nagad → শুধু Process() implement করেছে (refund নাই)।

- Card → Process() + Refund() দুটোই করেছে।

- কেউ অপ্রয়োজনীয় method implement করছে না।

- Interface ছোট আর নির্দিষ্ট → ISP principle follow ✅

<br>

### 🏗️Example 2 :  Notification System with ISP (PushNotify doesn’t need Save)

যদি এমন একটা scenario তৈরি হয়, PushNotify এর মধ্যে token ডাটাবেইজ এ save (❌) করা যাবে না

```c#
using System;
using System.Collections.Generic;

public interface INotify
{
    void Send();
    void Log();
    void Save(); // সব notification এই Save করতে বাধ্য হয়
}

public class EmailNotify : INotify
{
    public string Email { get; set; }
    public void Send()  => Console.WriteLine("Email Send to " + Email);
    public void Log()   => Console.WriteLine("Email Log : " + Email);
    public void Save()  => Console.WriteLine("Email Save to DB");
}

public class SMSNotify : INotify
{
    public string Phone { get; set; }
    public void Send()  => Console.WriteLine("SMS Sending to " + Phone);
    public void Log()   => Console.WriteLine("SMS Log : " + Phone);
    public void Save()  => Console.WriteLine("SMS Save to DB");
}

public class PushNotify : INotify
{
    public string Token { get; set; }
    public void Send()  => Console.WriteLine("Push Sending to " + Token);
    public void Log()   => Console.WriteLine("Push Log : " + Token);
    public void Save()  { } // PushNotify DB save করতে চায় না, তবুও implement করতে হলো
}

public class NotifyContext
{
    public INotify Notify { get; set; }

    public NotifyContext(INotify notify)
    {
        Notify = notify;
    }

    public void Process()
    {
        Notify.Send();
        Notify.Log();
        Notify.Save(); // সব notification এর Save কল হবে
    }
}

class Program
{
    public static void Main()
    {
        IList<NotifyContext> notifyList = new List<NotifyContext>()
        {
            new NotifyContext(new EmailNotify() { Email = "test@test.com" }),
            new NotifyContext(new SMSNotify() { Phone = "0912345678" }),
            new NotifyContext(new PushNotify() { Token = "123456789" })
        };

        foreach (var notify in notifyList)
        {
            notify.Process();
        }
    }
}
```
### 🔴 Problem Explanation

- PushNotify শুধু Send ও Log করতে চায়, কিন্তু interface Save() include করার কারণে forced হয়ে empty implementation দিতে হলো।

- এটা Interface Segregation Principle (ISP) ভঙ্গ করছে।

- Clean Code অনুযায়ী unused methods থাকা উচিত না।


### ✅ Solution (With ISP)
```c#
using System;
using System.Collections.Generic;

public interface ISend
{
    void Send();
}

public interface ILog
{
    void Log();
}

public interface ISave
{
    void Save();
}

// ---------------- Notification Classes ----------------
public class EmailNotify : ISend, ILog, ISave
{
    public string Email { get; set; }
    public void Send()  => Console.WriteLine("Email Send to " + Email);
    public void Log()   => Console.WriteLine("Email Log : " + Email);
    public void Save()  => Console.WriteLine("Email Save to DB");
}

public class SMSNotify : ISend, ILog, ISave
{
    public string Phone { get; set; }
    public void Send()  => Console.WriteLine("SMS Sending to " + Phone);
    public void Log()   => Console.WriteLine("SMS Log : " + Phone);
    public void Save()  => Console.WriteLine("SMS Save to DB");
}

public class PushNotify : ISend, ILog
{
    public string Token { get; set; }
    public void Send() => Console.WriteLine("Push Sending to " + Token);
    public void Log()  => Console.WriteLine("Push Log : " + Token);
}

// ---------------- Context Class ----------------
public class NotifyContext
{
    public ISend Notify { get; set; }
    public ILog Log { get; set; }
    public ISave Save { get; set; }

    public NotifyContext(ISend send, ILog log, ISave save)
    {
        Notify = send;
        Log = log;
        Save = save; // PushNotify ক্ষেত্রে null হতে পারে
    }

    public void Process()
    {
        Notify.Send();
        Log.Log();
        if (Save != null)
        {
            Save.Save();
        }
    }
}

// ---------------- Main Program ----------------
class Program
{
    public static void Main()
    {
        var notifyContexts = new List<NotifyContext>
        {
            new NotifyContext(
                new EmailNotify() { Email = "test@test.com" },
                new EmailNotify() { Email = "test@test.com" },
                new EmailNotify() { Email = "test@test.com" }
            ),
            new NotifyContext(
                new SMSNotify() { Phone = "0912345678" },
                new SMSNotify() { Phone = "0912345678" },
                new SMSNotify() { Phone = "0912345678" }
            ),
            new NotifyContext(
                new PushNotify() { Token = "123456789" },
                new PushNotify() { Token = "123456789" },
                null // PushNotify doesn’t need Save
            )
        };

        foreach (var context in notifyContexts)
        {
            context.Process();
        }
    }
}
```
#### ✅ Output
```yaml
Email Send to test@test.com
Email Log : test@test.com
Email Save to DB

SMS Sending to 0912345678
SMS Log : 0912345678
SMS Save to DB

Push Sending to 123456789
Push Log : 123456789
```

### 🌟 Key Points

- PushNotify এর Save optional → ISP respected

- NotifyContext flexible → শুধুমাত্র দরকারি interface inject করা হয়েছে

- Clean Code → Empty method avoid করা হয়েছে

### ✅ It can be done in another way.

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

```yaml
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


<br>
<br>

## 5️⃣ D – Dependency Inversion Principle (DIP)
### 🔹 Definition 

- High-level modules should not depend on low-level modules.

- Both should depend on abstractions (interfaces).

- Abstractions should not depend on details. Details should depend on abstractions.

>High-level modules (যেমন business logic) সরাসরি low-level modules (যেমন database, file, API) এর উপর নির্ভর করবে না।

>উভয়কে interface/abstraction এর উপর depend করতে হবে।

>Implementation (low-level) interface অনুযায়ী তৈরি হবে, high-level module শুধু interface ব্যবহার করবে।

### ❌ Example 1 :  Wrong Example (Without DIP)
```c#
class MySQLDatabase
{
    public void Save(string data)
    {
        Console.WriteLine("Saved to MySQL DB: " + data);
    }
}

class UserService
{
    private MySQLDatabase db = new MySQLDatabase(); // Direct dependency

    public void AddUser(string username)
    {
        db.Save(username);
        Console.WriteLine("User added: " + username);
    }
}
```
#### 🔴 Problem

- UserService এখন MySQLDatabase এর উপর tightly coupled।

- যদি PostgreSQL বা MongoDB ব্যবহার করতে চাই, UserService modify করতে হবে।

- DIP ভঙ্গ হয়েছে।

#### ✅ Correct Example (With DIP)
```c#
using System;

// 1. High-level module depends on abstraction
public interface IDatabase
{
    void Save(string data);
}

// 2. Low-level modules implement abstraction
public class MySQLDatabase : IDatabase
{
    public void Save(string data)
    {
        Console.WriteLine("Saved to MySQL DB: " + data);
    }
}

public class MongoDatabase : IDatabase
{
    public void Save(string data)
    {
        Console.WriteLine("Saved to MongoDB: " + data);
    }
}

// 3. High-level module depends only on interface
public class UserService
{
    private readonly IDatabase _database;

    public UserService(IDatabase database)
    {
        _database = database; // Inject via constructor
    }

    public void AddUser(string username)
    {
        _database.Save(username);
        Console.WriteLine("User added: " + username);
    }
}

// ----------------- Usage -----------------
class Program
{
    public static void Main()
    {
        IDatabase mysql = new MySQLDatabase();
        IDatabase mongo = new MongoDatabase();

        UserService service1 = new UserService(mysql);
        UserService service2 = new UserService(mongo);

        service1.AddUser("Alice");
        service2.AddUser("Bob");
    }
}
```

Output : 
```yaml
Saved to MySQL DB: Alice
User added: Alice
Saved to MongoDB: Bob
User added: Bob
```

>In C#, the `readonly` keyword is commonly used when applying **Dependency Injection**.
It ensures that the dependency is assigned only once — inside the constructor — and cannot be changed later during runtime.

### ✅ Benefits

- High-level module (UserService) directly database এ depend করে না।

- New database implement করতে হবে → শুধুই নতুন class implement করতে হবে, existing code modify করতে হবে না → OCP + DIP respected।

- Code flexible, testable, maintainable।


### ❌ Example 2 :  DIP না মানা ডিজাইন
```c#
using System;

public class Email
{
    public void SendEmail(string to)
    {
        Console.WriteLine("Email sent to " + to);
    }
}

public class Notification
{
    private Email email = new Email(); // সরাসরি নির্ভরশীল

    public void Send(string to)
    {
        email.SendEmail(to);
    }
}

class Program
{
    static void Main()
    {
        Notification notification = new Notification();
        notification.Send("test@test.com");
    }
}
```
#### 🔍 Problem

- Notification ক্লাস Email class-এর উপর tightly coupled।

- যদি SMS বা Push notification যোগ করতে চাও → Notification modify করতে হবে।

- DIP ভঙ্গ হয়েছে।

#### ✅ DIP অনুযায়ী ভালো ডিজাইন
```c#
using System;

// Step 1: Interface তৈরি করি
public interface IMessageSender
{
    void Send(string to);
}

// Step 2: Low-level modules implement করে
public class EmailSender : IMessageSender
{
    public void Send(string to)
    {
        Console.WriteLine("Email sent to : " + to);
    }
}

public class SMSSender : IMessageSender
{
    public void Send(string to)
    {
        Console.WriteLine("SMS sent to : " + to);
    }
}

// নতুন feature: Push Notification
public class PushSender : IMessageSender
{
    public void Send(string to)
    {
        Console.WriteLine("Push Notification sent to : " + to);
    }
}

// Step 3: High-level module Notification class
public class Notification
{
    private IMessageSender messageSender;

    public Notification(IMessageSender messageSender)
    {
        this.messageSender = messageSender; // Abstraction inject
    }

    public void Notify(string to)
    {
        messageSender.Send(to);
    }
}

// Step 4: Main method
class Program
{
    static void Main()
    {
        IMessageSender emailSender = new EmailSender();
        IMessageSender smsSender = new SMSSender();
        IMessageSender pushSender = new PushSender();

        Notification notification1 = new Notification(emailSender);
        notification1.Notify("test@test.com");

        Notification notification2 = new Notification(smsSender);
        notification2.Notify("01712345678");

        Notification notification3 = new Notification(pushSender);
        notification3.Notify("device_token_123");
    }
}
```


Output : 
```yaml
Email sent to : test@test.com
SMS sent to : 01712345678
Push Notification sent to : device_token_123
```
#### 🔹 Benefits

- High-level module (Notification) directly low-level class এ depend করে না।

- নতুন sender যোগ করতে শুধু নতুন class implement করে injection করতে হবে।

- Existing code modify করার দরকার নেই → OCP + DIP respected।


#### 📌 One-liner Summary:
- DIP: “High-level modules should depend on abstractions, not on low-level modules.” ✅



<br>
<br>


## 2️⃣ Liskov Substitution Principle (LSP)

### 🔹 Definition:

- Subtypes must be substitutable for their base types without breaking the program.

- Subclass must be completely substitutable for its superclass

#### 🎯 সহজ বাংলায়:
>👉 Parent (base) class-এর জায়গায় তার child (derived) class-কে বসালেও প্রোগ্রাম যেন ঠিকমতো কাজ করে।
- Subclass যদি Parent class-এর behavior পরিবর্তন করে দেয়, তাহলে সেটা LSP ভাঙছে।

- Subclass যতই extend করুক না কেন, তা যেন parent class-এর contract ভাঙে না।

####  ❌ Wrong Design : Example 1 


```c#
public class Employee
{
    public virtual int CalculateSalary()
    {
        return 1000000;
    }

    public virtual int Bonus()
    {
        return 100000;
    }
}

public class PermanentEmployee : Employee
{
    public override int CalculateSalary()
    {
        return 2000000;
    }
}

public class ContractEmployee : Employee
{
    public override int CalculateSalary()
    {
        return 1000000;
    }

    public override int Bonus()
    {
        throw new NotImplementedException(); // Problem ❌ // Bonus নেই
    }
}

class Program
{
    public static void Main()
    {
        List<Employee> employees = new List<Employee>
        {
            new PermanentEmployee(),
            new ContractEmployee()
        };

        foreach (var emp in employees)
        {
            Console.WriteLine($"Salary: {emp.CalculateSalary()}");
            Console.WriteLine($"Bonus: {emp.Bonus()}"); // Crash for ContractEmployee
        }
    }
}

```
#### 🔴  Problems

- ContractEmployee Bonus() support করে না → LSP ভঙ্গ।

- High-level code (Payroll) directly low-level class Employee এ depend করছে → DIP ভঙ্গ।
###  ✅ Solution (With Interface – DIP + LSP)
```c#
using System;

//Step 1: Interface তৈরি করি
public interface IEmployee
{
    int CalculateSalary();
}

public interface IBonusEligibleEmployee : IEmployee
{
    int Bonus();
}

// Step 2: Employee classes implement করবে interface অনুযায়ী
public class PermanentEmployee : IBonusEligibleEmployee
{
    public int CalculateSalary()
    {
        return 2000000;
    }

    public int Bonus()
    {
        return 200000;
    }
}

public class ContractEmployee : IEmployee
{
    public int CalculateSalary()
    {
        return 1000000;
    }
    // Bonus নেই → Interface implement করছে না → safe
}

// Step 3: PayrollSystem class (High-level module)
public class PayrollSystem
{
    public void ProcessSalary(IEmployee employee)
    {
        Console.WriteLine($"Salary: {employee.CalculateSalary()}");
    }

    public void ProcessBonus(IBonusEligibleEmployee employee)
    {
        Console.WriteLine($"Bonus: {employee.Bonus()}");
    }
}

// Step 4: Main method
class Program
{
    static void Main()
    {
        IEmployee contract = new ContractEmployee();
        IBonusEligibleEmployee permanent = new PermanentEmployee();

        PayrollSystem payroll = new PayrollSystem();

        payroll.ProcessSalary(contract);     // ✅ Works
        payroll.ProcessSalary(permanent);    // ✅ Works

        payroll.ProcessBonus(permanent);     // ✅ Works
        // payroll.ProcessBonus(contract);  // ❌ Call not allowed → safe
    }
}
```

Output : 
```yaml
Salary: 1000000
Salary: 2000000
Bonus: 200000
```
#### 🟢 Benefits

- ContractEmployee কে bonus call করা যাবে না, LSP maintained।

- PayrollSystem interface abstraction উপর depend করছে, low-level class direct depend নয় → DIP respected।

- নতুন Employee type যোগ করা সহজ।

<br>

### 💡 Another Example
 #### ❌ Problem – Coffee Shop Example (LSP ভাঙা)
```c#
using System;

namespace LSPExample
{
    // Parent class
    public abstract class Coffee
    {
        public abstract string Serve();
    }

    // Subclass 1: BlackCoffee
    public class BlackCoffee : Coffee
    {
        public override string Serve()
        {
            return "Serving Black Coffee ☕";
        }
    }

    // Subclass 2: Tea (LSP ভাঙছে)
    public class Tea : Coffee
    {
        public override string Serve()
        {
            return "Serving Tea 🍵 instead of Coffee!"; // Problem: Coffee expect করা হচ্ছে, কিন্তু Tea আসছে
        }
    }

    class Program
    {
        public static void ServeCoffee(Coffee coffee)
        {
            Console.WriteLine(coffee.Serve());
        }

        static void Main(string[] args)
        {
            BlackCoffee black = new BlackCoffee();
            Tea tea = new Tea();

            ServeCoffee(black);  // Output: Serving Black Coffee ☕ ✅
            ServeCoffee(tea);    // Output: Serving Tea 🍵 instead of Coffee! ❌ LSP ভাঙছে
        }
    }
}
```

#### Problem Note:

>Tea class হলো Coffee এর subclass, কিন্তু Coffee expect করা হচ্ছে coffee, Tea এসেছে → unexpected behavior। তাই Liskov Substitution Principle ভাঙছে।

#### ✅ Solution – LSP মানা
```c#
using System;

// Base (Parent) Class (Abstraction)
public abstract class Drink
{
    public abstract string Serve();
}

// Child 1
public class Coffee : Drink
{
    public override string Serve()
    {
        return "Serving Coffee ☕";
    }
}

// Child 2
public class Tea : Drink
{
    public override string Serve()
    {
        return "Serving Tea 🍵";
    }
}



public class DrinkService
{
  
    public void ServeDrink(Drink drink)
    {
        Console.WriteLine(drink.Serve());
    }
}



class Program
{
    static void Main()
    {
       
        Drink coffee = new Coffee();
        Drink tea = new Tea();

        DrinkService drinkService = new DrinkService();

        drinkService.ServeDrink(coffee); // Output: Serving Coffee ☕
        drinkService.ServeDrink(tea);    // Output: Serving Tea 🍵

        Console.ReadLine();
    }
}
```
Output :
```yaml
Serving Coffee ☕
Serving Tea 🍵
```

#### Solution Note :

>এখন parent class হলো Drink। Coffee এবং Tea দুটোই Drink এর subclass।
Function ServeDrink যে কোনো Drink safely handle করতে পারে।
✅ Liskov Substitution Principle মানা হচ্ছে।

<br>

### ❌ Example 3 : CashOnDelivery (COD)
 অনলাইন পেমেন্ট করতে পারে না। কিন্তু IPayment চায় অনলাইন payment করতে।
ফলে COD–কে IPayment এর জায়গায় বসালে সিস্টেম crash হয়।

#### ❌ WRONG DESIGN (LSP Violation)
```c#
using System;

public interface IPayment
{
    void Pay(decimal amount);
}

public class CreditCardPayment : IPayment
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using Credit Card.");
    }
}

public class PaypalPayment : IPayment
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using PayPal.");
    }
}

// ❌ COD cannot process online payment but still forced to implement Pay()
public class CashOnDelivery : IPayment
{
    public void Pay(decimal amount)
    {
        // LSP violation → unexpected behavior
        throw new NotSupportedException("COD cannot process online payment!");
    }
}

public class PaymentProcessor
{
    public void ProcessPayment(IPayment paymentMethod, decimal amount)
    {
        paymentMethod.Pay(amount); // ❌ COD will break here
    }
}

class Program
{
    static void Main()
    {
        PaymentProcessor processor = new PaymentProcessor();

        IPayment p1 = new CreditCardPayment();
        IPayment p2 = new PaypalPayment();
        IPayment p3 = new CashOnDelivery(); // ❌ will crash

        processor.ProcessPayment(p1, 500);
        processor.ProcessPayment(p2, 800);

        // ❌ This will throw exception (LSP broken)
        processor.ProcessPayment(p3, 1000);
    }
}
```

### ✅ CORRECT DESIGN (LSP Compliant Full Code)

Solution:

 - 👉 COD আলাদা type এর payment
 - 👉 Online payments আলাদা interface
 - 👉 কোনো class অন্য class এর constraint ভঙ্গ করছে না
 - 👉 সিস্টেমে কোথাও crash নেই

### ✅ CORRECT DESIGN (LSP Compliant)
```c#
using System;

// Base payment (common behavior)
public interface IPayment
{
    void Pay(decimal amount);
}

// Online payments
public interface IOnlinePayment : IPayment
{
    void PayOnline(decimal amount);
}

public class CreditCardPayment : IOnlinePayment
{
    public void Pay(decimal amount)
    {
        PayOnline(amount);
    }

    public void PayOnline(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using Credit Card.");
    }
}

public class PaypalPayment : IOnlinePayment
{
    public void Pay(decimal amount)
    {
        PayOnline(amount);
    }

    public void PayOnline(decimal amount)
    {
        Console.WriteLine($"Paid {amount} using PayPal.");
    }
}

// COD works perfectly without violating rules
public class CashOnDelivery : IPayment
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"COD: Please pay {amount} during delivery.");
    }
}

public class PaymentProcessor
{
    public void ProcessPayment(IPayment paymentMethod, decimal amount)
    {
        paymentMethod.Pay(amount);
    }
}

class Program
{
    static void Main()
    {
        PaymentProcessor processor = new PaymentProcessor();

        IPayment pay1 = new CreditCardPayment();
        IPayment pay2 = new PaypalPayment();
        IPayment pay3 = new CashOnDelivery();  // ✔ Works fine

        processor.ProcessPayment(pay1, 500);
        processor.ProcessPayment(pay2, 1000);
        processor.ProcessPayment(pay3, 800);  // ✔ No crash, LSP maintained
    }
}
```

Output : 
```yaml
Paid 500 using Credit Card.
Paid 1000 using PayPal.
COD: Please pay 800 during delivery.
```

#### 💡 Takeaway (Note):

- Subclass কে parent এর behaviour অনুযায়ী কাজ করতে হবে।

- Parent class expectation ভাঙলে LSP ভেঙে যায়।

- Abstract/general parent বানালে future extension সহজ হয়।

