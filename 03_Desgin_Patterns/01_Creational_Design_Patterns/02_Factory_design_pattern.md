# 🏭 Factory Method Pattern – Introduction

### 📘  Definition
The **Factory Method Pattern** is a creational design pattern that **provides an interface for creating objects in a superclass**, but allows subclasses to decide which class to instantiate.


>**Factory Method Pattern** হলো একটি creational design pattern, যা object তৈরি করার জন্য একটি interface দেয়,   কিন্তু কোন class এর object তৈরি হবে সেটা নির্ধারণ করে **subclass**।

<br>

### 2️⃣ Why Factory Pattern is Needed?

❌ Problem (Without Factory):
```c#
if(type == "Circle")
    shape = new Circle();
else if(type == "Rectangle")
    shape = new Rectangle();
```

#### 🔻 Problems:

- Too many if-else
- Client code tightly coupled
- New type add করলে client modify করতে হয়

<br>

### 🎯 Goal / Purpose
- Object creation handle করা, কিন্তু creation logic বাইরে exposed না করা।  
- Subclasses কে সিদ্ধান্ত নেওয়ার সুযোগ দেওয়া—কোন object তৈরি হবে।  
- Code কে flexible, maintainable, এবং loosely-coupled করা।
- Centralized object creation logic

<br>

### 🧩 Used Case
- **Logger** (File / Database / Cloud)
- **Database Connection** (MySQL / PostgreSQL / MongoDB)
- **GUI frameworks** (Button, Dialog)
- **Game development** (Enemy, Weapon Factory)
- **Notification system** (Email / SMS / Push)
- **Payment processing** (Bkash, Stripe, PayPal)

<br>


### 🔧 How Factory Method Works (Steps)

1. Parent class declares a **factory method**.  
2. Subclasses override that method.  
3. Client requests an object from the parent → parent calls the overridden factory.  
4. Subclass decides **which object to return**.  

```arduino
Client → Factory → Object
(Client never uses new directly)
```

<br>

### ✔️ Advantages

- Removes `new` keyword from client code  
- Easy to add new product classes  
- Follows **Open/Closed Principle**  
- Cleaner, modular design 
- Centralized object creation 



### ❌ Disadvantages

- More classes needed → slightly complex  
- Requires inheritance 
- If-else still exists (Simple Factory) 

<br>
<br>



### 🎮 Example 1 : Notification System (Factory Pattern)

#### 🔔 Problem Description
System থেকে বিভিন্ন ধরনের notification পাঠাতে হবে:

- Email
- SMS  
- (Future) Push Notification

👉 Requirement:
- Notification type runtime এ decide হবে
- Future এ নতুন notification add করা সহজ হতে হবে



### ❌ Wrong Approach (Without Factory)

```csharp
class Program
{
    static void Main()
    {
        string type = "email";

        if (type == "email")
        {
            Console.WriteLine("Sending Email Notification");
        }
        else if (type == "sms")
        {
            Console.WriteLine("Sending SMS Notification");
        }
    }
}
```
### ❌ Problems (Without Factory)

- `if-else` keeps growing as new notification types are added  
- Tight coupling between client code and concrete classes  
- Adding a new notification requires modifying client code  
- Violates **Open/Closed Principle (OCP)**  


### ✅ Solution: Factory Design Pattern

- Object creation logic is moved to a **Factory**
- Client code depends only on the **interface**
- New notification types can be added with minimal changes



### ✅ Factory Pattern – Full Runnable C# Code



```csharp
// 1️⃣ Product Interface
interface INotification
{
    void Send();
}

// 2️⃣ Concrete Products
class EmailNotification : INotification
{
    public void Send()
    {
        Console.WriteLine("Sending Email Notification");
    }
}

class SmsNotification : INotification
{
    public void Send()
    {
        Console.WriteLine("Sending SMS Notification");
    }
}

// 3️⃣ Factory Class
class NotificationFactory
{
    public static INotification CreateNotification(string type)
    {
        if (type == "email")
            return new EmailNotification();
        else if (type == "sms")
            return new SmsNotification();
        else
            throw new ArgumentException("Invalid notification type");
    }
}

// 4️⃣ Client Code
class Program
{
    static void Main()
    {
        INotification notification1 =
            NotificationFactory.CreateNotification("email");
        notification1.Send();

        INotification notification2 =
            NotificationFactory.CreateNotification("sms");
        notification2.Send();
    }
}
```
🖥 Output
```yaml
Sending Email Notification
Sending SMS Notification
```

<br>
<br>

### 🔺 Example 2 : Drawing Application

Drawing app এ user shape select করবে:

- Circle
- Rectangle
- Triangle

👉 App জানবে না কোন class তৈরি হচ্ছে, শুধু draw করবে।

✅ Factory Pattern 
```C#
// 1️⃣ Product Interface
interface IShape
{
    void Draw();
}

// 2️⃣ Concrete Products
class Circle : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing Circle");
    }
}

class Rectangle : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing Rectangle");
    }
}

class Triangle : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing Triangle");
    }
}

// 3️⃣ Factory Class
class ShapeFactory
{
    public static IShape CreateShape(string type)
    {
        if (type == "circle")
            return new Circle();
        else if (type == "rectangle")
            return new Rectangle();
        else if (type == "triangle")
            return new Triangle();
        else
            throw new ArgumentException("Invalid shape type");
    }
}

// 4️⃣ Client Code
class Program
{
    static void Main()
    {
        IShape shape1 = ShapeFactory.CreateShape("circle");
        shape1.Draw();

        IShape shape2 = ShapeFactory.CreateShape("rectangle");
        shape2.Draw();

        IShape shape3 = ShapeFactory.CreateShape("triangle");
        shape3.Draw();
    }
}
```
🖥 Output
```yaml
Drawing Circle
Drawing Rectangle
Drawing Triangle
```


---

<br>

### ❗ Example 2 v1 :

ধরো তুমি একটি **drawing application** বানাচ্ছো।  
Application-এ বিভিন্ন ধরনের **shapes** (Circle, Square, Triangle) draw করতে হবে।  

### Challenges

- Client code কে **নির্দিষ্ট class** জানার প্রয়োজন নেই  
- যদি নতুন shape যোগ করতে চাই → **existing code change না করে কাজ হবে**  
- Object creation logic **centralized হবে না**  

### Requirement

Client শুধু "**shape তৈরি করো**" বলবে,  
কোন shape তৈরি হবে সেটা **factory ঠিক করবে**  


### ✅ Solution

1. একটি **Shape interface** থাকবে  
2. Concrete shapes (Circle, Square) **implement করবে interface**  
3. একটি **Abstract Factory (ShapeFactory)** থাকবে, যেখানে একটি **abstract `CreateShape()` method** থাকবে  
4. Concrete Factory (CircleFactory, SquareFactory) **decide করবে কোন shape তৈরি হবে**

✅ Example (C# Code)
```c#
using System;
using System.IO;

// Product
public interface IShape {
    void Draw();
}

// Concrete Products
public class Circle : IShape {
    public void Draw() {
        Console.WriteLine("Drawing Circle");
    }
}

public class Square : IShape {
    public void Draw() {
        Console.WriteLine("Drawing Square");
    }
}

// Creator
public abstract class ShapeFactory {
    public abstract IShape CreateShape();
}

// Concrete Creators
public class CircleFactory : ShapeFactory {
    public override IShape CreateShape() {
        return new Circle();
    }
}

public class SquareFactory : ShapeFactory {
    public override IShape CreateShape() {
        return new Square();
    }
}

// Client
class Program {
    static void Main() {
        ShapeFactory factory = new CircleFactory();
        IShape shape = factory.CreateShape();
        shape.Draw();   // Output: Drawing Circle

        factory = new SquareFactory();
        shape = factory.CreateShape();
        shape.Draw();   // Output: Drawing Square
    }
}
```

✅ Output
```yaml
Drawing Circle
Drawing Square
```

## 📊 When to Use Factory Method?

- যখন **object creation logic বারবার repeat** হচ্ছে  
- যখন **client code কে নির্দিষ্ট class এর উপর নির্ভরশীল না করতে চাই**  
- যখন **system কে extensible করতে চাই** (new product যোগ করলে client code পরিবর্তন হবে না)  



### ❌ Bad Example

যে জায়গাগুলোতে **object creation logic বারবার repeat** হচ্ছে (Problematic code):  

নিচের কোডে দুইটা আলাদা সার্ভিস (`OrderServiceA`, `OrderServiceB`) প্রত্যেকেই **সরাসরি concrete Pizza ক্লাসগুলো `new` করে** —  

- অর্থাৎ creation logic অনেক জায়গায় repeated  
- Client/Service গুলো **concrete class-এ নির্ভরশীল**  
- যদি নতুন `ChickenPizza` যোগ করতে হয় → দুইটা সার্ভিসেই কোড **বদলাতে হবে** → extensible নয়


```c#
using System;

// Product
public abstract class Pizza {
    public abstract void Prepare();
}

// Concrete Products
public class CheesePizza : Pizza {
    public override void Prepare() {
        Console.WriteLine("Preparing Cheese Pizza");
    }
}

public class VegPizza : Pizza {
    public override void Prepare() {
        Console.WriteLine("Preparing Veg Pizza");
    }
}

// ******** Problem: creation logic repeated in multiple places ********

// Service A (creates pizza directly)
public class OrderServiceA {
    public void PlaceOrder(string type) {
        Pizza pizza;
        if (type == "cheese") {
            pizza = new CheesePizza();   // direct dependency on concrete class
        } else if (type == "veg") {
            pizza = new VegPizza();
        } else {
            Console.WriteLine("Unknown pizza type");
            return;
        }
        pizza.Prepare();
    }
}

// Service B (same repeated logic again)
public class OrderServiceB {
    public void PlaceOrder(string type) {
        Pizza pizza;
        if (type == "cheese") {
            pizza = new CheesePizza();   // repeated
        } else if (type == "veg") {
            pizza = new VegPizza();      // repeated
        } else {
            Console.WriteLine("Unknown pizza type");
            return;
        }
        pizza.Prepare();
    }
}

// Client
class Program {
    static void Main() {
        var a = new OrderServiceA();
        a.PlaceOrder("cheese"); // Preparing Cheese Pizza

        var b = new OrderServiceB();
        b.PlaceOrder("veg");    // Preparing Veg Pizza

        // Problem: যদি ChickenPizza যোগ করতে চাই => উভয় জায়গায় if-block update করতে হবে
    }
}
```

Output
```yaml
Preparing Cheese Pizza
Preparing Veg Pizza
```


### 🛠️ Problem Summary (সমস্যাগুলো সহজভাবে)

- **Object creation logic repeated** — ভালো নয়  
- **Client/Service গুলো concrete classes-এ tightly coupled**  
- **Extensibility খারাপ** — নতুন ধরনের pizza যোগ করতে হলে অনেক জায়গায় পরিবর্তন করতে হবে  



### ✅ Solution — Factory Method ব্যবহার করে ঠিক করা

### Step-by-Step (Good Example)

1. একটি **PizzaStore** নামক Creator বানানো হবে  
2. **`OrderPizza(type)`** মেথড থাকবে client-এর জন্য  
3. **`CreatePizza(type)`** হবে **protected abstract**  
4. প্রতি **ConcreteStore** (যেমন `NewYorkPizzaStore`, `ChicagoPizzaStore`) সেখানে নিজের **creation logic** দিবে  

### ফলাফল

- Client শুধু **PizzaStore** ব্যবহার করবে  
- Creation logic একটি **জায়গায় কেন্দ্রীভূত** থাকবে  
- নতুন pizza যোগ করা **সহজ** হবে



```c#
using System;

// ---------------- Product ----------------
public abstract class Pizza {
    public abstract void Prepare();
}

public class CheesePizza : Pizza {
    public override void Prepare() {
        Console.WriteLine("Preparing Cheese Pizza");
    }
}

public class VegPizza : Pizza {
    public override void Prepare() {
        Console.WriteLine("Preparing Veg Pizza");
    }
}

// নতুন pizza — একবারই তৈরি করলে হবে
public class ChickenPizza : Pizza {
    public override void Prepare() {
        Console.WriteLine("Preparing Chicken Pizza");
    }
}

// ---------------- Creator (Factory Method here) ----------------
public abstract class PizzaStore {
    // Client calls this
    public Pizza OrderPizza(string type) {
        // Factory Method is called here — subclasses decide which concrete Pizza to create
        Pizza pizza = CreatePizza(type);
        if (pizza == null) {
            Console.WriteLine("Unknown pizza type");
            return null;
        }
        // Common steps after creation
        pizza.Prepare();
        // bake/cut/box could be here too
        return pizza;
    }

    // Factory Method — subclasses override this
    protected abstract Pizza CreatePizza(string type);
}

// ---------------- Concrete Creators ----------------
public class NewYorkPizzaStore : PizzaStore {
    protected override Pizza CreatePizza(string type) {
        if (type == "cheese") {
            // NY style cheese (could be different subclass)
            return new CheesePizza();
        } else if (type == "veg") {
            return new VegPizza();
        } else if (type == "chicken") {
            return new ChickenPizza();
        } else {
            return null;
        }
    }
}

public class ChicagoPizzaStore : PizzaStore {
    protected override Pizza CreatePizza(string type) {
        if (type == "cheese") {
            return new CheesePizza();
        } else if (type == "veg") {
            return new VegPizza();
        } else {
            // Chicago store এখন chicken বানায় না
            return null;
        }
    }
}

// ---------------- Client ----------------
class Program {
    static void Main() {
        PizzaStore ny = new NewYorkPizzaStore();
        ny.OrderPizza("chicken"); // Preparing Chicken Pizza

        PizzaStore chicago = new ChicagoPizzaStore();
        chicago.OrderPizza("veg"); // Preparing Veg Pizza

        // Client never directly uses `new ChickenPizza()` — only uses PizzaStore
    }
}
```
Output
```yaml
Preparing Chicken Pizza
Preparing Veg Pizza
```
### Step-by-Step: কেন এটা ভালো & কিভাবে উপরের ৩টা সমস্যা মিটায়

### 1. Product (Pizza)
- একটি **abstract/interface** রাখা হলো যার উপর client কাজ করে (`Prepare()`, ইত্যাদি)  

### 2. Concrete Products
- `CheesePizza`, `VegPizza`, `ChickenPizza` ইত্যাদি  
- এগুলো শুধু **Product-এর implementation**  

### 3. Creator (PizzaStore)
- `OrderPizza(type)` → **public method**  
- Factory Method → **protected abstract `CreatePizza(type)`**  

### 4. OrderPizza Workflow
- Common workflow (`prepare`, `bake`, `cut`, `box`) রাখা হলো  
- Object creation থেকে workflow আলাদা রাখা হয়েছে  

### 5. ConcreteCreators
- `NewYorkPizzaStore`, `ChicagoPizzaStore`  
- প্রতিটি store **নির্দিষ্টভাবে `CreatePizza` implement** করে  
- কোন store কোন pizza বানাবে তা **কেন্দ্রীয়ভাবে** ঠিক থাকে  



### 💡 Problem Solving

### 1. Repeated creation logic
- আগে একই if/switch multiple services-এ ছিল  
- এখন creation logic প্রতিটি store-এর `CreatePizza`-এ আছে  
- সার্ভিসে **copy-paste নেই**  

### 2. Client dependency on concrete classes
- Client কেবল **PizzaStore** এবং **Pizza interface** ব্যবহার করে  
- **Loose coupling** achieved  

### 3. Extensible
- নতুন `ChickenPizza` যোগ করতে হলে:  
  1. নতুন `ChickenPizza` class বানানো  
  2. যে store chicken বানাবে, সেখানে `CreatePizza`-এ branch যোগ করা  
- Client code **অপরিবর্তিত**  


### ⚖️ Advantages
- Loose coupling (client depends on interface, not implementation)  
- Easy to add new product classes  
- Code maintainable হয়  

### ❌ Disadvantages
- Class structure জটিল হয়ে যায় (extra subclass তৈরি করতে হয়)  

---

### 🍕 Real-Life Example – Pizza Store (Factory Method)

ধরি, আমাদের একটা **Pizza Store** আছে।  
প্রতিটি ব্রাঞ্চ (`NewYorkPizzaStore`, `ChicagoPizzaStore`) আলাদা স্টাইলে pizza বানায়।  
কিন্তু client শুধু **PizzaStore** থেকে pizza অর্ডার করে, কিভাবে বানানো হচ্ছে সেটা জানে না।  



### UML-like Structure


```
Pizza (Product)
 ├── prepare(), bake(), cut(), box()

ConcretePizza (Concrete Products)
 ├── CheesePizza, VegPizza, etc.

PizzaStore (Creator)
 └── orderPizza()
 └── createPizza(type)   <-- Factory Method

ConcretePizzaStore (Concrete Creators)
 ├── NewYorkPizzaStore
 ├── ChicagoPizzaStore

````


### ✅ Example Code (C#)

```csharp
// Product
public abstract class Pizza {
    public abstract void Prepare();
}

// Concrete Products
public class CheesePizza : Pizza {
    public override void Prepare() {
        Console.WriteLine("Preparing Cheese Pizza");
    }
}

public class VegPizza : Pizza {
    public override void Prepare() {
        Console.WriteLine("Preparing Veg Pizza");
    }
}

// Creator
public abstract class PizzaStore {
    public void OrderPizza() {
        Pizza pizza = CreatePizza();
        pizza.Prepare();
    }

    // Factory Method
    protected abstract Pizza CreatePizza();
}

// Concrete Creators
public class NewYorkPizzaStore : PizzaStore {
    protected override Pizza CreatePizza() {
        return new CheesePizza(); // NY style
    }
}

public class ChicagoPizzaStore : PizzaStore {
    protected override Pizza CreatePizza() {
        return new VegPizza(); // Chicago style
    }
}

// Client
class Program {
    static void Main() {
        PizzaStore nyStore = new NewYorkPizzaStore();
        nyStore.OrderPizza(); 
        // Output: Preparing Cheese Pizza

        PizzaStore chicagoStore = new ChicagoPizzaStore();
        chicagoStore.OrderPizza(); 
        // Output: Preparing Veg Pizza
    }
}
```

Output
```yaml
Preparing Chicken Pizza
Preparing Veg Pizza
```
### 🍕 Explanation

- Client শুধু **`PizzaStore.OrderPizza()`** use করছে  
- কোন store থেকে অর্ডার দিচ্ছে তার উপর নির্ভর করছে কোন pizza বানানো হবে  
- Client কে **CheesePizza vs VegPizza vs অন্য কিছু** জানতেই হচ্ছে না  


### 🎯 Real-World Benefit

- যদি নতুন Pizza যোগ করতে হয় (e.g., "ChickenPizza") →  
  শুধু নতুন **ConcretePizza** এবং **ConcretePizzaStore** এর rule update করলেই হবে  
- Client code এ **কোনো পরিবর্তন** করতে হবে না


### 🏗  Factory Method (with Singleton + Strategy) 

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

### 🔔 Notification System Example (Combined Design Patterns)

এই উদাহরণে আমরা **তিনটি প্যাটার্ন একসাথে ব্যবহার** করছি:

- **Singleton** → Logger কে শুধু একটা instance রাখতে (thread-safe)  
- **Strategy** → `NotifyContext` বিভিন্ন আচরণ (`send`, `log`, `save`) **composition** করে ব্যবহার করে (ইন্টারফেস ভিত্তিক)  
- **Factory Method** → `INotificationContextCreator` এর **concrete creator** গুলো `NotifyContext` তৈরি করবে; Client কে object তৈরি নিয়ে চিন্তা করতে হবে না  



### 1️⃣ Initial Problem

প্রথমে আমরা Notification system বানাই।  
এখানে **Email, SMS, Push notification** পাঠানো যায়।  

```csharp
public class NotifyContext {
    public ISend Notify { get; set; }
    public ILog Log { get; set; }
    public ISave Save { get; set; }

    public NotifyContext(ISend send, ILog log, ISave save) {
        Notify = send;
        Log = log;
        Save = save;
    }

    public void Process() {
        Notify.Send();
        Log.Log();
        if (Save != null) {
            Save.Save();
        }
    }
}
```
### ❌ Problem

- `Main()` এ Developer কে **object নিজে বানাতে হচ্ছে**  
- কখন `null` দিতে হবে, কখন দিতে হবে না — এগুলো **মনে রাখতে লাগছে**  
  (যেমন PushNotify তে Save নাই, তাই `null` দিতে হবে)  
- 👉 **Hard to use এবং error-prone**  



### 2️⃣ Factory Method Idea

সমাধান হলো, **object creation এর দায়িত্ব অন্য class কে দেওয়া**।  
Developer শুধু **factory class** call করবে, factory **সঠিক object** বানিয়ে দিবে  

```csharp
public interface INotificationContextCreator {
    public NotifyContext Create();
}

// Example Factory
public class EmailNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() {
        return new NotifyContext(
            new EmailNotify { Email = "test@test.com" },
            new EmailNotify { Email = "test@test.com" },
            new EmailNotify { Email = "test@test.com" }
        );
    }
}
```
👉 এখন developer কে object বানানোর চিন্তা করতে হবে না। শুধু factory call করলেই কাজ হবে।


### 3️⃣ Singleton যোগ করা

- Logging এর ক্ষেত্রে আমরা চাই **Logger এর একটাই object** পুরো application এ ব্যবহার হোক  
- তাই এখানে **Singleton Pattern** use করা হলো  

```csharp
public class Logger : ILogger {
    private Logger() {
        Console.WriteLine("Logger created");
    }

    private static Logger _instance = null;
    private static readonly object _lock = new object();

    public static Logger GetInstance() {
        if (_instance == null) {
            lock (_lock) {
                if (_instance == null) {
                    _instance = new Logger();
                }
            }
        }
        return _instance;
    }

    public void Log(string message) {
        Console.WriteLine("This is from log - {0}", message);
    }
}
````

### 3️⃣ Singleton যোগ করা

- Logging এর ক্ষেত্রে আমরা চাই **Logger এর একটাই object** পুরো application এ ব্যবহার হোক  
- তাই এখানে **Singleton Pattern** use করা হলো  

```csharp
public class Logger : ILogger {
    private Logger() {
        Console.WriteLine("Logger created");
    }

    private static Logger _instance = null;
    private static readonly object _lock = new object();

    public static Logger GetInstance() {
        if (_instance == null) {
            lock (_lock) {
                if (_instance == null) {
                    _instance = new Logger();
                }
            }
        }
        return _instance;
    }

    public void Log(string message) {
        Console.WriteLine("This is from log - {0}", message);
    }
}
```
👉 এখন যেকোনো notification এর log করার সময় same Logger object use হবে।

### 4️⃣ Final Example – Factory Method + Singleton + Strategy
```C#
using System;

public interface ILogger {
    public void Log(string message);
}

public class Logger : ILogger {
    private Logger() {
        Console.WriteLine("Logger created");
    }

    private static Logger _instance = null;
    private static readonly object _lock = new object();

    public static Logger GetInstance() {
        if (_instance == null) {
            lock (_lock) {
                if (_instance == null) {
                    _instance = new Logger();
                }
            }
        }
        return _instance;
    }

    public void Log(string message) {
        Console.WriteLine("This is from log - {0}", message);
    }
}

// Common Interfaces
public interface ISend { void Send(); }
public interface ILog { void Log(); }
public interface ISave { void Save(); }

// Notifications
public class EmailNotify : ISend, ILog, ISave {
    public string Email { get; set; }
    public void Send() => Console.WriteLine("Sending Email to " + Email);
    public void Log() => Logger.GetInstance().Log("Email Log: " + Email);
    public void Save() => Console.WriteLine("Email Saved to DB");
}

public class SMSNotify : ISend, ILog, ISave {
    public string Phone { get; set; }
    public void Send() => Console.WriteLine("Sending SMS to " + Phone);
    public void Log() => Logger.GetInstance().Log("SMS Log: " + Phone);
    public void Save() => Console.WriteLine("SMS Saved to DB");
}

public class PushNotify : ISend, ILog {
    public string Token { get; set; }
    public void Send() => Console.WriteLine("Sending Push to " + Token);
    public void Log() => Logger.GetInstance().Log("Push Log: " + Token);
}

public class WhatsappNotify : ISend, ILog {
    public string Phone { get; set; }
    public void Send() => Console.WriteLine("Sending Whatsapp to " + Phone);
    public void Log() => Logger.GetInstance().Log("Whatsapp Log: " + Phone);
}

// Context
public class NotifyContext {
    private readonly ISend send;
    private readonly ILog log;
    private readonly ISave save;

    public NotifyContext(ISend send, ILog log, ISave save) {
        this.send = send;
        this.log = log;
        this.save = save;
    }

    public void Process() {
        send.Send();
        log.Log();
        save?.Save();
    }
}

// Factory
public interface INotificationContextCreator {
    public NotifyContext Create();
}

public class EmailNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() => new NotifyContext(
        new EmailNotify { Email = "test@test.com" },
        new EmailNotify { Email = "test@test.com" },
        new EmailNotify { Email = "test@test.com" }
    );
}

public class SMSNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() => new NotifyContext(
        new SMSNotify { Phone = "123456789" },
        new SMSNotify { Phone = "123456789" },
        new SMSNotify { Phone = "123456789" }
    );
}

public class PushNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() => new NotifyContext(
        new PushNotify { Token = "987654321" },
        new PushNotify { Token = "987654321" },
        null
    );
}

public class WhatsappNotificationContextCreator : INotificationContextCreator {
    public NotifyContext Create() => new NotifyContext(
        new WhatsappNotify { Phone = "5555555" },
        new WhatsappNotify { Phone = "5555555" },
        null
    );
}

// Main
class Program {
    public static void Main() {
        var emailContext = new EmailNotificationContextCreator().Create();
        var smsContext = new SMSNotificationContextCreator().Create();
        var pushContext = new PushNotificationContextCreator().Create();
        var whatsappContext = new WhatsappNotificationContextCreator().Create();

        emailContext.Process();
        smsContext.Process();
        pushContext.Process();
        whatsappContext.Process();
    }
}
```

Output
```yaml
Sending Email to test@test.com
Logger created
This is from log - Email Log: test@test.com
Email Saved to DB
Sending SMS to 123456789
This is from log - SMS Log: 123456789
SMS Saved to DB
Sending Push to 987654321
This is from log - Push Log: 987654321
Sending Whatsapp to 5555555
This is from log - Whatsapp Log: 5555555
```




### 📌 Key Takeaways

- **Factory Method** → object creation এর দায়িত্ব আলাদা class কে দেয়া হলো  
- **Singleton** → Logger class এর একটাই instance হলো, সবার জন্য common  
- **Strategy** → আলাদা আলাদা Notification class (`EmailNotify`, `SMSNotify`, `PushNotify`, etc.) একই **interface** follow করছে, তাই context easily use করতে পারছে
