
# 🏭 Abstract Factory Design Pattern

### 📘 Definition


Abstract Factory is a creational design pattern that provides an **interface for creating families of related objects** without specifying their concrete classes.


>Abstract Factory এমন একটি pattern যা **সম্পর্কিত একাধিক object (একটি family)** তৈরি করার জন্য একটি interface দেয়—  
কিন্তু কোন class এর object তৈরি হবে সেটা **concrete factory ঠিক করে**।


<br>

### 🎯 **Goal / Purpose**

- To create related objects together (UI theme, OS components, device components).  
- To ensure compatibility between created objects.  
- To avoid using `new` directly → more flexible, scalable design.  

<br>

### 🧩 **Where It’s Used**

- **UI themes** (DarkThemeFactory, LightThemeFactory)  
- **Operating systems** (MacFactory, WindowsFactory)  
- **Database families** (SQL Factory, NoSQL Factory)  
- **Gaming** (Enemy + Weapon + Vehicle sets)  

<br>
 

### 🔧 **How Abstract Factory Works (Steps)**

1. Create an **Abstract Factory Interface** (e.g., `UIFactory`).  
2. Create multiple **Concrete Factories** (e.g., `MacFactory`, `WindowsFactory`).  
3. Each Concrete Factory produces a **family of related objects**:  
   - Button  
   - Checkbox  
   - Menu  
4. Client uses **only the abstract factory**, never concrete classes.  

<br>

### ✔️ **Advantages**

- Creates **consistent object families**.  
- **No need to modify client code** when adding new variants.  
- Great for **theme switching, plugin systems**.  
- Follows **Open/Closed Principle**.  



### ❌ **Disadvantages**

- More classes + structure → more complex.  
- For small simple object creation, pattern may be overkill.  


<br>

### 3️⃣ UML Structure
```
AbstractFactory
   |--> ConcreteFactoryA
   |--> ConcreteFactoryB
ProductA          ProductB
   |                  |
ConcreteProductA1  ConcreteProductB1
ConcreteProductA2  ConcreteProductB2
````
### 4️⃣ Example Scenario

💡 ধরি, আমাদের একটা **UI Toolkit** আছে যেটা দুই ধরণের হতে পারে:  
- Windows UI  
- MacOS UI  

প্রতিটি UI তে থাকবে **Button** এবং **Checkbox**।  
Client জানবে না Windows এর UI নাকি MacOS এর UI, শুধু **factory** কে ডাকবে।  

### ❌ Wrong Approach (Without Abstract Factory)
```c#
if(os == "windows")
{
    button = new WindowsButton();
    checkbox = new WindowsCheckbox();
}
else if(os == "mac")
{
    button = new MacButton();
    checkbox = new MacCheckbox();
}
```

❌ Tight coupling + messy

<br>

### 5️⃣ Code Example (C#)

```csharp
// 1. Abstract Product
public interface IButton {
    void Render();
}
public interface ICheckbox {
    void Render();
}

// 2. Concrete Products - Windows
public class WindowsButton : IButton {
    public void Render() => Console.WriteLine("Render Windows Button");
}
public class WindowsCheckbox : ICheckbox {
    public void Render() => Console.WriteLine("Render Windows Checkbox");
}

// 3. Concrete Products - MacOS
public class MacButton : IButton {
    public void Render() => Console.WriteLine("Render Mac Button");
}
public class MacCheckbox : ICheckbox {
    public void Render() => Console.WriteLine("Render Mac Checkbox");
}

// 4. Abstract Factory
public interface IGUIFactory {
    IButton CreateButton();
    ICheckbox CreateCheckbox();
}

// 5. Concrete Factories
public class WindowsFactory : IGUIFactory {
    public IButton CreateButton()
    {
        return new WindowsButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new WindowsCheckbox();
    }
}
public class MacFactory : IGUIFactory {
    public IButton CreateButton()
    {
        return new MacButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new MacCheckbox();
    }
}

// 6. Usage
class Program {
    static void Main() {
        IGUIFactory factory;

        string os = "windows"; // or "mac"

        if (os == "windows")
            factory = new WindowsFactory();
        else
            factory = new MacFactory();

        IButton button = factory.CreateButton();
        ICheckbox checkbox = factory.CreateCheckbox();

        button.Render();
        checkbox.Render();
    }
}
```
6️⃣ Output
```yaml
Render Windows Button
Render Windows Checkbox
or
Render Mac Button
Render Mac Checkbox
```

### 7️⃣ Advantages
- Client code concrete class এর উপর নির্ভরশীল নয়।
 
 - একসাথে পুরো product family change করা যায়।
 
 - System maintain করা সহজ হয়।

<br>
<br>
 
### 📌 Abstract Factory – Database Connection Example 2

### 1️⃣ Scenario (Problem Statement)

ধরি, আমরা একটা **Application** বানাচ্ছি যেটা Database এ connect করবে।  
আমাদের Application কখনো **MySQL**, কখনো **Oracle**, কখনো **MongoDB** use করবে।  

👉 Problem হলো – যদি আমরা directly concrete class (`MySQLConnection`, `OracleConnection`, `MongoConnection`) ব্যবহার করি, তাহলে code এ **everywhere অনেক জায়গায় পরিবর্তন করতে হবে**।  
Client code সরাসরি কোন database class এর সাথে **tightly coupled** হয়ে যাবে।  



### 2️⃣ ❌ Without Abstract Factory (Bad Approach)

```csharp
// Concrete Database Connection Classes
public class MySQLConnection 
{
    public void Connect() => Console.WriteLine("Connected to MySQL Database");
}

public class OracleConnection 
{
    public void Connect() => Console.WriteLine("Connected to Oracle Database");
}

public class MongoConnection
{
    public void Connect() => Console.WriteLine("Connected to MongoDB Database");
}

// Client Code
class Program {
    static void Main() {
        // Suppose user wants MySQL
        MySQLConnection mysql = new MySQLConnection();
        mysql.Connect();

        // Later requirement changed → Oracle
        OracleConnection oracle = new OracleConnection();
        oracle.Connect();

        // Later requirement changed → MongoDB
        MongoConnection mongo = new MongoConnection();
        mongo.Connect();
    }
}
```

Output 
```
Connected to MySQL Database
Connected to Oracle Database
Connected to MongoDB Database
```

### ❌ Problems

- Client code **concrete class** এর উপর নির্ভরশীল → tightly coupled  
- Database change করলে **everywhere class replace** করতে হবে  
- Extend করা কঠিন (নতুন database add করলে অনেক জিনিস পরিবর্তন লাগবে)  



### 3️⃣ With Abstract Factory (Solution)

এখন আমরা **Abstract Factory Pattern** ব্যবহার করব।  
👉 এখানে **Factory interface** থাকবে যেটা বলে দিবে কোন database এর connection বানাতে হবে।  
Client শুধু **factory** কে use করবে, concrete class কে চিনবে না।  



### ✅ Step 1: Abstract Product

```csharp
using System;
public interface IDatabaseConnection 
{
    void Connect();
}

/// ✅ Step 2: Concrete Products

public class MySQLConnection : IDatabaseConnection 
{
    public void Connect() => Console.WriteLine("Connected to MySQL Database");
}

public class OracleConnection : IDatabaseConnection 
{
    public void Connect() => Console.WriteLine("Connected to Oracle Database");
}

public class MongoConnection : IDatabaseConnection 
{
    public void Connect() => Console.WriteLine("Connected to MongoDB Database");
}


/// ✅ Step 3: Abstract Factory
/// 

public interface IDatabaseFactory 
{
    IDatabaseConnection CreateConnection();
}


/// ✅ Step 4: Concrete Factories

public class MySQLFactory : IDatabaseFactory 
{
    public IDatabaseConnection CreateConnection() => new MySQLConnection();
}

public class OracleFactory : IDatabaseFactory 
{
    public IDatabaseConnection CreateConnection() => new OracleConnection();
}

public class MongoFactory : IDatabaseFactory 
{
    public IDatabaseConnection CreateConnection() => new MongoConnection();
}


/// ✅ Step 5: Client Code
/// 
public class Application {
    private readonly IDatabaseConnection _connection;

    public Application(IDatabaseFactory factory) {
        _connection = factory.CreateConnection();
    }

    public void Run() {
        _connection.Connect();
    }
}

class Program {
    static void Main() {
        IDatabaseFactory factory;

        // Suppose environment = MySQL
        factory = new MySQLFactory();
        Application app = new Application(factory);
        app.Run();

        // Suppose environment = Oracle
        factory = new OracleFactory();
        app = new Application(factory);
        app.Run();

        // Suppose environment = MongoDB
        factory = new MongoFactory();
        app = new Application(factory);
        app.Run();
    }
}
```
4️⃣ Output
```
Connected to MySQL Database
Connected to Oracle Database
Connected to MongoDB Database
```

### 5️⃣ Benefits 

- ✅ Client code **concrete class জানে না** → শুধু factory use করে  
- ✅ Easily **extend করা যায়** (নতুন DB add করতে হলে শুধু নতুন Factory + Product add করতে হবে)  
- ✅ **Maintenance সহজ** (code change কম করতে হয়)  
- ✅ **Loose coupling** নিশ্চিত করে  

<br>

### ✅ AbstractFactoryDatabase

```csharp
using System;

// ------------------------------
// 1. Abstract Product
// ------------------------------
// এখানে আমরা Database Connection এর জন্য একটা common interface বানাচ্ছি,
// যেটা সব ধরনের Database connection follow করবে।
public interface IDatabaseConnection {
    void Connect();
}

// ------------------------------
// 2. Concrete Products
// ------------------------------
// এখন আলাদা আলাদা Database এর জন্য আলাদা class বানাচ্ছি
// যেগুলো IDatabaseConnection implement করছে।

public class MySQLConnection : IDatabaseConnection {
    public void Connect() => Console.WriteLine("Connected to MySQL Database");
}

public class OracleConnection : IDatabaseConnection {
    public void Connect() => Console.WriteLine("Connected to Oracle Database");
}

public class MongoConnection : IDatabaseConnection {
    public void Connect() => Console.WriteLine("Connected to MongoDB Database");
}

// ------------------------------
// 3. Abstract Factory
// ------------------------------
// এখানে আমরা Factory এর একটা interface বানালাম,
// যেটা বলবে কোন Database connection তৈরি করতে হবে।
public interface IDatabaseFactory {
    IDatabaseConnection CreateConnection();
}

// ------------------------------
// 4. Concrete Factories
// ------------------------------
// প্রত্যেকটা Database এর জন্য আলাদা Factory বানানো হলো
// যেটা নির্দিষ্ট Database connection তৈরি করে দেয়।
public class MySQLFactory : IDatabaseFactory {
    public IDatabaseConnection CreateConnection() => new MySQLConnection();
}

public class OracleFactory : IDatabaseFactory {
    public IDatabaseConnection CreateConnection() => new OracleConnection();
}

public class MongoFactory : IDatabaseFactory {
    public IDatabaseConnection CreateConnection() => new MongoConnection();
}

// ------------------------------
// 5. Client Code
// ------------------------------
// Client কখনো সরাসরি MySQLConnection বা OracleConnection জানে না।
// সে শুধু Factory এর সাথে কাজ করে।
public class Application {
    private readonly IDatabaseConnection _connection;

    // Constructor এ Factory নেয়া হচ্ছে
    public Application(IDatabaseFactory factory) {
        _connection = factory.CreateConnection();
    }

    // Run method এ connect করা হচ্ছে
    public void Run() {
        _connection.Connect();
    }
}

// ------------------------------
// 6. Main Program
// ------------------------------
// এখন আমরা দেখাবো কিভাবে আলাদা আলাদা Database এর জন্য
// আলাদা Factory ব্যবহার করা যায়।
class Program {
    static void Main() {
        IDatabaseFactory factory;

        // Suppose environment = MySQL
        factory = new MySQLFactory();
        Application app = new Application(factory);
        app.Run(); // Output: Connected to MySQL Database

        // Suppose environment = Oracle
        factory = new OracleFactory();
        app = new Application(factory);
        app.Run(); // Output: Connected to Oracle Database

        // Suppose environment = MongoDB
        factory = new MongoFactory();
        app = new Application(factory);
        app.Run(); // Output: Connected to MongoDB Database
    }
}
```
<br>

---

<br>


### Learn Details
### Bad Code / Anti-pattern 

```csharp
using System;
using System.Collections.Generic;

public interface ISend {
    public void Send();
}

public interface ILog {
    public void Log();
}

public interface ISave {
    public void Save();
}

public class EmailNotify : ISend, ILog, ISave {
    public string Email { get; set; }

    public void Send() {
        Console.WriteLine("Sending email to " + Email);
    }

    public void Log() {
        Console.WriteLine("Logging email to " + Email);
    }

    public void Save() {
        Console.WriteLine("Saving db to " + Email);
    }
}

public class SMSNotify : ISend, ILog, ISave {
    public string Phone { get; set; }

    public void Send() {
        Console.WriteLine("Sending SMS to " + Phone);
    }

    public void Log() {
        Console.WriteLine("Logging SMS to " + Phone);
    }

    public void Save() {
        Console.WriteLine("Saving db to " + Phone);
    }
}

public class PushNotify : ISend, ILog {
    public string Token { get; set; }

    public void Send() {
        Console.WriteLine("Sending Push to " + Token);
    }

    public void Log() {
        Console.WriteLine("Logging Push to " + Token);
    }
}

public class NotifyContext {
    public ISend send { get; set; }
    public ILog log { get; set; }
    public ISave save { get; set; }

    public NotifyContext(ISend send, ILog log, ISave save) {
        this.send = send;
        this.log = log;
        this.save = save;
    }

    public void Process() {
        send.Send();
        log.Log();

        if(save != null) {
            save.Save();
        }
    }
}

public interface INotificationContextCreator {
    public NotifyContext Create();
}

public class EmailNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() {
        return new NotifyContext (
            new EmailNotify { Email = "test@test.com" },
            new EmailNotify { Email = "test@test.com" },
            new EmailNotify { Email = "test@test.com" }
        );
    }
}

public class SMSNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() {
        return new NotifyContext (
            new SMSNotify { Phone = "123456789" },
            new SMSNotify { Phone = "123456789" },
            new SMSNotify { Phone = "123456789" }
        );
    }
}

public class PushNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() {
        return new NotifyContext (
            new PushNotify { Token = "123456789" },
            new PushNotify { Token = "123456789" },
            null
        );
    }
}

class Program {
    public static void Main () {
        INotificationContextCreator emailCreator = new EmailNotificationContextCreator();
        NotifyContext emailContext = emailCreator.Create();

        INotificationContextCreator smsCreator = new SMSNotificationContextCreator();
        NotifyContext smsContext = smsCreator.Create();

        INotificationContextCreator pushCreator = new PushNotificationContextCreator();
        NotifyContext pushContext = pushCreator.Create();

        emailContext.Process();
        smsContext.Process();
        pushContext.Process();
    }
}
````

### Initial Problem: Tightly Coupled Object Creation

### Bad Code Example (Problem)

```csharp
public class EmailNotificationContextCreator : INotificationContextCreator 
{
    public NotifyContext Create() 
    {
        // PROBLEMS:
        // 1. Direct object creation with hardcoded values
        // 2. Multiple identical objects created unnecessarily
        // 3. No validation or business logic
        // 4. Tight coupling between creator and concrete classes
        return new NotifyContext (
            new EmailNotify { Email = "test@test.com" },
            new EmailNotify { Email = "test@test.com" },
            new EmailNotify { Email = "test@test.com" }
        );
    }
}
```
### Issues with this approach

- Violates **Open/Closed Principle** (hard to extend)  
- **Code duplication** across creators  
- **Difficult to test** (hard dependencies)  
- No **central control** over object creation  
- Changes require modifying **multiple places**  



### কেন সমস্যা?

- **Hard-coupling:** ক্লায়েন্ট কোড কনক্রিট ক্লাসের উপর নির্ভর করছে (EmailNotify)।  
- **Duplication:** object creation logic বারবার ছড়ানো আছে — validation বা config করার জায়গা নেই।  
- **Single Responsibility ভঙ্গ:** creation, validation, configuration আলাদা জায়গায় হওয়া উচিত — এখন mixed।  
- **Extend/Change করলে ক্লায়েন্টে হাত লাগাতে হবে** — OCP ভঙ্গ (Open/Closed Principle)।  
- **Dependency management সমস্যা:** রিয়েল ওয়ার্ল্ডে creation-এর আগে অনেক কাজ লাগে (validate, inject logger, config), সরাসরি `new` দিলে সেটা করা কঠিন।  

<br>

### Solution: Abstract Factory Pattern

### Step 1: Define the Abstract Factory Interface

```csharp
public interface INotificationFactory
{
    public ISend CreateSend();    // Factory method for Send
    public ILog CreateLog();      // Factory method for Log
    public ISave CreateSave();    // Factory method for Save
}
```


### Step 2: Implement Concrete Factories

```csharp
// Email Notification Factory
public class EmailNotificationFactory : INotificationFactory
{
    public ISend CreateSend() 
    { 
        // Can add validation, configuration, etc.
        return new EmailNotify(); 
    }
    
    public ILog CreateLog() 
    { 
        return new EmailNotify(); 
    }
    
    public ISave CreateSave() 
    { 
        return new EmailNotify(); 
    }
}

// SMS Notification Factory
public class SMSNotificationFactory : INotificationFactory
{
    public ISend CreateSend() 
    { 
        return new SMSNotify(); 
    }
    
    public ILog CreateLog() 
    { 
        return new SMSNotify(); 
    }
    
    public ISave CreateSave() 
    { 
        return new SMSNotify(); 
    }
}

// Push Notification Factory (handles null case gracefully)
public class PushNotificationFactory : INotificationFactory
{
    public ISend CreateSend() 
    { 
        return new PushNotify(); 
    }
    
    public ILog CreateLog() 
    { 
        return new PushNotify(); 
    }
    
    public ISave CreateSave() 
    { 
        return null; // Push doesn't support Save
    }
}
```
### Step 3: Enhanced Context Creators using Factories

```csharp
public class EmailNotificationContextCreator : INotificationContextCreator 
{
    public NotifyContext Create() 
    {
        // BENEFITS:
        // 1. Single factory instance controls all object creation
        // 2. Can add validation and business logic
        // 3. Easy to extend and maintain
        INotificationFactory factory = new EmailNotificationFactory();
        
        return new NotifyContext (
            factory.CreateSend(),  // Factory creates Send object
            factory.CreateLog(),   // Factory creates Log object  
            factory.CreateSave()   // Factory creates Save object
        );
    }
}
```
### Final Implementation with WhatsApp Extension

### Step 4: Adding New Notification Type (Open/Closed Principle)

```csharp
// New WhatsApp Notification class
public class WhatsAppNotify : ISend, ILog
{
    public string Phone { get; set; }
    public void Send() { Console.WriteLine("Sending WhatsApp to " + Phone); }
    public void Log() { Console.WriteLine("Logging WhatsApp to " + Phone); }
}

// New WhatsApp Factory (extends without modifying existing code)
public class WhatsAppNotificationFactory : INotificationFactory
{
    public ISend CreateSend() { return new WhatsAppNotify(); }
    public ILog CreateLog() { return new WhatsAppNotify(); }
    public ISave CreateSave() { return null; } // WhatsApp doesn't support Save
}

// New WhatsApp Context Creator
public class WhatsAppNotificationContextCreator : INotificationContextCreator
{
    public NotifyContext Create()
    {
        INotificationFactory factory = new WhatsAppNotificationFactory();
        return new NotifyContext(
            factory.CreateSend(),
            factory.CreateLog(), 
            factory.CreateSave()
        );
    }
}
```
### Key Design Pattern Benefits

### 1. Encapsulation

```csharp
// Object creation logic is encapsulated in factories
// Clients don't need to know creation details
public ISend CreateSend() 
{ 
    // Can add complex initialization logic here
    var notify = new EmailNotify();
    notify.Email = ValidateAndFormatEmail("test@test.com");
    return notify;
}
```
2. Loose Coupling
```c#
// Context creators depend on interfaces, not concrete classes
INotificationFactory factory = new EmailNotificationFactory();
// Instead of: new EmailNotify(), new EmailNotify(), new EmailNotify()
```
#### 3. Single Responsibility
- Factories handle object creation

- Context creators handle context assembly

- Notification classes handle their specific behavior

#### 4. Open/Closed Principle
```c#
// To add new notification type:
// 1. Create new notification classes ✓
// 2. Create new factory ✓  
// 3. Create new context creator ✓
// NO existing code modification required! ✓
````
### When to Use Abstract Factory Pattern

### When to Use
- System needs to be independent from how objects are created  
- System needs to be configured with multiple families of objects  
- Family of related objects needs to be used together  

### Key Advantages
- **Isolates concrete classes** – Client code works with interfaces only  
- **Makes exchanging product families easy** – Just change the factory  
- **Promotes consistency among products** – Factory ensures compatible objects  
- **Supports Open/Closed Principle** – Easy to add new variants  

### Real-world Analogies
- **Car Factory:** Different factories (SedanFactory, SUVFactory) create compatible components (Engine, Wheels, Seats)  
- **UI Toolkit:** Different theme factories (DarkThemeFactory, LightThemeFactory) create consistent UI components  

### Code Demonstration

```csharp
class Program 
{
    public static void Main () 
    {
        // Client code is clean and simple
        var creators = new INotificationContextCreator[] 
        {
            new EmailNotificationContextCreator(),
            new SMSNotificationContextCreator(), 
            new PushNotificationContextCreator(),
            new WhatsAppNotificationContextCreator() // NEW: Added without changing existing code
        };
        
        foreach (var creator in creators)
        {
            NotifyContext context = creator.Create();
            context.Process();
        }
    }
}
```
>This pattern demonstrates how to create flexible, maintainable, and extensible object creation systems that follow SOLID principles, especially the Open/Closed Principle.

<br>

### Abstract Factory Pattern – Full Notification System Example

This example demonstrates **Abstract Factory + Factory Method + SOLID principles** in a notification system with Email, SMS, Push, and WhatsApp notifications.

```csharp
using System;
using System.Collections.Generic;

// =============================================================================
// STEP 1: Define core interfaces
// =============================================================================
public interface ISend { void Send(); }
public interface ILog { void Log(); }
public interface ISave { void Save(); }

// =============================================================================
// STEP 2: Concrete Notification Classes
// =============================================================================
public class EmailNotify : ISend, ILog, ISave {
    public string Email { get; set; }
    public void Send() => Console.WriteLine("Sending email to " + Email);
    public void Log() => Console.WriteLine("Logging email to " + Email);
    public void Save() => Console.WriteLine("Saving email to database: " + Email);
}

public class SMSNotify : ISend, ILog, ISave {
    public string Phone { get; set; }
    public void Send() => Console.WriteLine("Sending SMS to " + Phone);
    public void Log() => Console.WriteLine("Logging SMS to " + Phone);
    public void Save() => Console.WriteLine("Saving SMS to database: " + Phone);
}

public class PushNotify : ISend, ILog {
    public string Token { get; set; }
    public void Send() => Console.WriteLine("Sending Push notification to " + Token);
    public void Log() => Console.WriteLine("Logging Push notification to " + Token);
}

public class WhatsAppNotify : ISend, ILog {
    public string Phone { get; set; }
    public void Send() => Console.WriteLine("Sending WhatsApp to " + Phone);
    public void Log() => Console.WriteLine("Logging WhatsApp to " + Phone);
}

// =============================================================================
// STEP 3: Notification Context
// =============================================================================
public class NotifyContext {
    public ISend SendComponent { get; set; }
    public ILog LogComponent { get; set; }
    public ISave SaveComponent { get; set; }

    public NotifyContext(ISend send, ILog log, ISave save) {
        SendComponent = send;
        LogComponent = log;
        SaveComponent = save;
    }

    public void Process() {
        SendComponent.Send();
        LogComponent.Log();
        if(SaveComponent != null) SaveComponent.Save();
    }
}

// =============================================================================
// STEP 4: Abstract Factory
// =============================================================================
public interface INotificationFactory {
    ISend CreateSend();
    ILog CreateLog();
    ISave CreateSave();
}

// =============================================================================
// STEP 5: Concrete Factories
// =============================================================================
public class EmailNotificationFactory : INotificationFactory {
    public ISend CreateSend() => new EmailNotify { Email = "test@test.com" };
    public ILog CreateLog() => new EmailNotify { Email = "test@test.com" };
    public ISave CreateSave() => new EmailNotify { Email = "test@test.com" };
}

public class SMSNotificationFactory : INotificationFactory {
    public ISend CreateSend() => new SMSNotify { Phone = "123456789" };
    public ILog CreateLog() => new SMSNotify { Phone = "123456789" };
    public ISave CreateSave() => new SMSNotify { Phone = "123456789" };
}

public class PushNotificationFactory : INotificationFactory {
    public ISend CreateSend() => new PushNotify { Token = "push_token_123" };
    public ILog CreateLog() => new PushNotify { Token = "push_token_123" };
    public ISave CreateSave() => null;
}

public class WhatsAppNotificationFactory : INotificationFactory {
    public ISend CreateSend() => new WhatsAppNotify { Phone = "987654321" };
    public ILog CreateLog() => new WhatsAppNotify { Phone = "987654321" };
    public ISave CreateSave() => null;
}

// =============================================================================
// STEP 6: Context Creators (Factory Method)
// =============================================================================
public interface INotificationContextCreator {
    NotifyContext Create();
}

public class EmailNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() {
        INotificationFactory factory = new EmailNotificationFactory();
        return new NotifyContext(factory.CreateSend(), factory.CreateLog(), factory.CreateSave());
    }
}

public class SMSNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() {
        INotificationFactory factory = new SMSNotificationFactory();
        return new NotifyContext(factory.CreateSend(), factory.CreateLog(), factory.CreateSave());
    }
}

public class PushNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() {
        INotificationFactory factory = new PushNotificationFactory();
        return new NotifyContext(factory.CreateSend(), factory.CreateLog(), factory.CreateSave());
    }
}

public class WhatsAppNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() {
        INotificationFactory factory = new WhatsAppNotificationFactory();
        return new NotifyContext(factory.CreateSend(), factory.CreateLog(), factory.CreateSave());
    }
}

// =============================================================================
// STEP 7: Client Code
// =============================================================================
class Program {
    public static void Main() {
        Console.WriteLine("=== Abstract Factory Pattern Demo ===\n");

        var creators = new INotificationContextCreator[] {
            new EmailNotificationContextCreator(),
            new SMSNotificationContextCreator(),
            new PushNotificationContextCreator(),
            new WhatsAppNotificationContextCreator()
        };

        foreach(var creator in creators) {
            NotifyContext context = creator.Create();
            context.Process();
            Console.WriteLine();
        }

        Console.WriteLine("=== Demo Complete ===");
    }
}
```
### ✅ Key Benefits Demonstrated

- **Loose Coupling:** Client depends on interfaces, not concrete classes  
- **Single Responsibility:** Factories create objects; context orchestrates process  
- **Open/Closed Principle:** Add new notifications (WhatsApp) without modifying existing code  
- **Consistency:** Factories ensure related objects work together  
- **Maintainability:** Centralized object creation  
- **Testability:** Easy to mock factories for unit tests  
- **Flexibility:** Swap factories to change behavior

