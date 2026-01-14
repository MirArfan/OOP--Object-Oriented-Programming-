# 🟦 Singleton Design Pattern

### 📌 Introduction 


The Singleton Pattern ensures that a class has **only one instance** throughout the entire application and provides a **global access point** to that instance.

>Singleton Pattern এমন একটি Design Pattern যেখানে পুরো অ্যাপ্লিকেশনে একটি ক্লাসের **মাত্র একটাই অবজেক্ট** তৈরি হতে পারে, এবং সবার জন্য সেই **একটাই অবজেক্ট ব্যবহারের ব্যবস্থা** থাকে।

<br>

### ❗ Why Singleton? (Problem)

কিছু object থাকে যেগুলোর multiple instance হলে সমস্যা তৈরি হয়:

- দুইটি database connection খুলে গেলে **connection conflict** হতে পারে  
- দুইটি logger instance হলে **log missing বা overwrite** হতে পারে  
- দুইবার configuration load হলে **settings mismatch** হতে পারে  

👉 তাই এসব ক্ষেত্রে **একটাই shared instance** দরকার।

<br>

### 3️⃣ Goal & Purpose

🎯 Goal

- Single instance maintain করা

🎯 Purpose

- Shared resource control
- Memory waste কমানো
- Consistent state maintain

<br>

### 🔧 How Singleton Works (Steps)

1. Constructor → **private** করা হয়  
2. Class → একটি **private static instance** ধরে  
3. একটি **static method** → সেই instance return করে  
4. Instance আগেই তৈরি থাকলে → সেটাকেই return  
5. তৈরি না থাকলে → নতুন instance তৈরি করে return  

<br>

### ⭐ Key Characteristics

| Feature | Meaning |
|--------|---------|
| **Single Instance** | পুরো অ্যাপে একটাই object থাকে |
| **Global Access** | যেকোনো জায়গা থেকে সেই instance access করা যায় |
| **Lazy Initialization** | প্রয়োজন হলে তবেই object তৈরি হয় |
| **Thread Safety (Optional)** | multithreading environment-এ safe করা যায় |

<br>

### 🎯 Real-Life Use Cases

- **Database Connection Manager**  
- **Logger**  
- **Cache Manager**  
- **Configuration Settings Loader**  
- **File System Manager**  
- **API Rate Limit Controller**

<br>



### ✔️ Advantages

- **Controlled access** to the single instance  
- **Saves memory** (only one instance is created)  
- **Useful for shared resources** like database connections, loggers, cache  
- **Global access** → easy to use from anywhere in the application  



### ❌ Disadvantages

- **Harder to unit-test** due to global state  
- **Violates Single Responsibility Principle** sometimes  
- Can lead to **hidden dependencies** across the application  
- **Not suitable for large-scale multi-threaded scenarios** if not carefully implemented  


<br>

### 📌 Example 1 : (প্রেক্ষাপট)

একটি বড় অ্যাপ্লিকেশনে:

- Database connection খুলতে খরচ বেশি

- বারবার connection খুললে performance খারাপ হয়

- একাধিক connection থেকে:

   - Deadlock

   - Resource exhaustion

   - Data inconsistency হতে পারে

👉 সমাধান: পুরো অ্যাপে একটাই database connection manager


### ❌ Bad Approach Code
```c#
class DatabaseConnection
{
    public DatabaseConnection()
    {
        Console.WriteLine("Database connection opened");
    }

    public void ExecuteQuery(string query)
    {
        Console.WriteLine($"Executing: {query}");
    }
}

class Program
{
    static void Main()
    {
        DatabaseConnection db1 = new DatabaseConnection();
        db1.ExecuteQuery("SELECT * FROM users");

        DatabaseConnection db2 = new DatabaseConnection();
        db2.ExecuteQuery("INSERT INTO orders VALUES (...)");
    }
}
```
🖥 Output
```yaml
--- Database connection opened ---
--- Database connection opened ---
Are they same? False
```

❌ Multiple instance তৈরি হচ্ছে → Problem!

### ✅ Solution: Singleton Design Pattern

#### 🧠 Idea

পুরো অ্যাপ্লিকেশনে Logger-এর শুধুমাত্র একটিই instance থাকবে

🔑 Singleton Pattern — Core Rules

- 1️⃣ Constructor private হবে
- 2️⃣ একটি static instance থাকবে
- 3️⃣ Instance পাওয়ার জন্য static method থাকবে
- 4️⃣ Thread-safe হতে হবে (multi-thread app এর জন্য)

### ✅ Singleton DatabaseConnection (Thread-Safe)
```c#
using System;

// 👉 sealed মানে:
// এই class কে আর কেউ inherit করতে পারবে না
// Singleton ভাঙা যাবে না (subclass বানিয়ে new করা যাবে না)
public sealed class DatabaseConnection
{
    // 1️⃣ Single instance holder
    // 👉 এখানে একটাই object রাখা হবে
    // 👉 static → পুরো application জুড়ে shared
    // 👉 শুরুতে null থাকে (Lazy initialization)
    private static DatabaseConnection _instance = null;


    // Thread safety lock
    // 👉 Multiple thread একসাথে আসলে
    // 👉 একটামাত্র thread object তৈরি করতে পারবে
    // 👉 readonly মানে: // assign একবারই করা যাবে
    private static readonly object _lock = new object();


    // 2️⃣ Private constructor
    // বাইরে থেকে new DatabaseConnection() করা যাবে না
    private DatabaseConnection()
    {
        Console.WriteLine("Database connection opened (Only Once)");
    }

    // 3️⃣ Global access point
    public static DatabaseConnection GetInstance()
    {
        if (_instance == null)
        {
            // 👉 একসাথে একটাই thread ভিতরে ঢুকতে পারবে
            // 👉 Thread-safe নিশ্চিত করে
            lock (_lock)
            {
                if (_instance == null)
                {
                    _instance = new DatabaseConnection();
                }
            }
        }
        return _instance;
    }

    public void ExecuteQuery(string query)
    {
        Console.WriteLine($"Executing query: {query}");
    }
}
class Program
{
    static void Main()
    {
        DatabaseConnection db1 = DatabaseConnection.GetInstance();
        db1.ExecuteQuery("SELECT * FROM Users");

        DatabaseConnection db2 = DatabaseConnection.GetInstance();
        db2.ExecuteQuery("INSERT INTO Orders VALUES (...)");

        Console.WriteLine($"Same connection? {ReferenceEquals(db1, db2)}");
    }
}
```

🖥 Output
```yaml
Database connection opened (Only Once)
Executing query: SELECT * FROM Users
Executing query: INSERT INTO Orders VALUES (...)
Same connection? True
```

 <br>



### ❗ Example 2 :

Imagine you are building an application where multiple modules need to log data:

- Authentication module  
- Payment module  
- Order module  

If every module creates **its own logger object**, then:

- Logs may appear **out of order**
- Some logs may **not save properly**
- File may remain **open multiple times**
- Performance drops due to repeated object creation

👉 This is **BAD**, because logger হওয়া উচিত **একটাই globally shared object**।



### ❌ Bad Code (Multiple Logger Instances — Problem)

```csharp
using System;
using System.IO;

public class Logger
{
    private StreamWriter writer;

    public Logger()
    {
        writer = new StreamWriter("app.log", true);
    }

    public void Log(string message)
    {
        writer.WriteLine(DateTime.Now + " - " + message);
        writer.Flush();
    }
}

// ❌ Every module creates its own Logger instance
public class AuthService
{
    private Logger logger = new Logger();

    public void Login()
    {
        logger.Log("User logged in.");
    }
}

public class PaymentService
{
    private Logger logger = new Logger();

    public void Pay()
    {
        logger.Log("Payment processed.");
    }
}

class Program
{
    static void Main()
    {
        new AuthService().Login();
        new PaymentService().Pay();
    }
}
```
### ❌ Why This is Bad?

- একাধিক logger instance তৈরি হচ্ছে

- একই log file একসাথে অনেকবার open হচ্ছে

- Race condition হতে পারে

- Performance কমে

- Log inconsistent হতে পারে

👉 Logger must be one shared object, not multiple.


### ✅ Solution: Use Singleton (One Logger Only)

- Singleton নিশ্চিত করে যে:

- পুরো application-এ একটি logger instance থাকবে

- সব module সেই একটাই instance ব্যবহার করবে

- Log order, performance, file handling ঠিক থাকবে

<br>

### ✅ Good Code (Singleton Logger in C#)
```c#
using System;
using System.IO;

public class Logger
{
    private static Logger instance;
    private static readonly object padlock = new object();
    private StreamWriter writer;

    // Private constructor
    private Logger()
    {
        Console.WriteLine("Logger instance created!");
        writer = new StreamWriter("app.log", true);
    }

    // Global access
    public static Logger Instance
    {
        get
        {
            lock (padlock)
            {
                if (instance == null)
                {
                    instance = new Logger();
                }
                return instance;
            }
        }
    }

    public void Log(string message)
    {
        string logMessage = DateTime.Now + " - " + message;
        Console.WriteLine("Logging: " + logMessage); // <-- Output in console
        writer.WriteLine(logMessage);
        writer.Flush();
    }
}

// Modules using Logger
public class AuthService
{
    public void Login()
    {
        Logger.Instance.Log("User logged in.");
    }
}

public class PaymentService
{
    public void Pay()
    {
        Logger.Instance.Log("Payment processed.");
    }
}

class Program
{
    static void Main()
    {
        Console.WriteLine("=== Application Start ===");

        new AuthService().Login();
        new PaymentService().Pay();
        new AuthService().Login();

        Console.WriteLine("=== Application End ===");
    }
}
```

✅ Output
```yaml
=== Application Start ===
Logger instance created!
Logging: 2025-11-16 21:30:00 - User logged in.
Logging: 2025-11-16 21:30:01 - Payment processed.
Logging: 2025-11-16 21:30:02 - User logged in.
=== Application End ===
```


### 🎯 What Improved?

| Problem (Before)                  | Solution (After Singleton) |
|----------------------------------|----------------------------|
| Multiple Logger instances         | Only one instance          |
| File opened many times            | File opens once            |
| Logs inconsistent                 | Logs are consistent        |
| High memory usage                 | Low memory                 |
| Hard to manage                    | Easy global access         |


<br>
<br>



<br>
<br>
<br>

---

### 📝 Step 1: Example Code Without Singleton

```csharp
using System;

public class Logger {
    public Logger() {
        Console.WriteLine("Logger created");
    }
    public void Log(string message) {
        Console.WriteLine(message);
    }
}

class Program {
    public static void Main() {
        Logger logger1 = new Logger();
        logger1.Log("Hello World");

        Logger logger2 = new Logger();
        logger2.Log("Hello World 2");
    }
}
```
Output:
```
Logger created
Hello World
Logger created
Hello World 2
```

👉 এখানে প্রতিবার `new Logger()` করলে নতুন Object তৈরি হচ্ছে।
Memory নষ্ট হচ্ছে।

যদি Database Connection হয়, তাহলে **server overload** হতে পারে।
Logger-এ **consistency** নষ্ট হবে।

<br>

### ❌ Step 2: Problems of Multiple Object Creation

### 1. Multiple Object Creation
- প্রতিবার `new Logger()` করলে আলাদা আলাদা object তৈরি হচ্ছে।  
- উদাহরণ: `logger1` এবং `logger2` → দুইটি আলাদা object।  
- পুরো project-এ logger একবারই দরকার।

### 2. Memory Waste
- প্রতিবার নতুন object তৈরি করলে অপ্রয়োজনীয় memory খরচ হয়।  
- এটি server performance কে প্রভাবিত করতে পারে।

### 3. Inconsistent Behavior
- ধরুন, Database Connection class এর ক্ষেত্রে একাধিক object আলাদা আলাদা connection তৈরি করবে।  
- ফলাফল: connection leak বা server overload হতে পারে।  

### 4. Hard to Maintain
- Class update বা change করলে project জুড়ে আলাদা আলাদা object-এ mismatch হতে পারে।  
- ফলে maintenance কঠিন হয়ে যায়।


### ✅ Step 3: Solution Code (With Singleton)
```c#
using System;

public class Logger {
    private static Logger _instance = null;  // একটাই instance রাখবে

    private Logger() { // বাইরে থেকে new Logger() করা যাবে না
        Console.WriteLine("Logger created");
    }

    // Global access point
    public static Logger GetInstance() {
        if (_instance == null) {
            _instance = new Logger();
        }
        return _instance;
    }

    public void Log(string message) {
        Console.WriteLine(message);
    }
}

class Program {
    public static void Main() {
        Logger logger1 = Logger.GetInstance();
        logger1.Log("First log");

        Logger logger2 = Logger.GetInstance();
        logger2.Log("Second log");

        Console.WriteLine(Object.ReferenceEquals(logger1, logger2)); // True
    }
}
```
Output:
```
Logger created
First log
Second log
True
```

### 🎯 Step 4: How Singleton Solves the Problem

### 1. Single Instance
- Logger থেকে একবারই object তৈরি হয়।  
- `logger1` এবং `logger2` একই object share করে।  

### 2. Memory Efficient
- বারবার নতুন object বানানো বন্ধ।  
- একটাই instance ব্যবহার করে memory save হয়।  

### 3. Consistent Behavior
- পুরো project-এ logger/config/database → একই object ব্যবহার করে।  
- Data consistent থাকে।  

### 4. Thread Safe (Optional Upgrade)
- Multi-threaded project হলে lock ব্যবহার করে thread-safety যোগ করা যায়।


### 🎯 Singleton Real-Life Use Cases



### 1️⃣ Logger System – One Logger for Whole Project

**Problem:**  
Project এ একসাথে অনেক জায়গায় log করতে হয় (errors, warnings, debug info)।  
যদি প্রতিবার নতুন Logger বানানো হয় → অনেক instance হবে → unnecessary memory usage।  
Log file / Console এ inconsistent output আসতে পারে।  

**Solution (Singleton):**  
পুরো project এ একটাই Logger instance রাখা হয়।  
সব class / component সেই একই instance ব্যবহার করে।  
ফলে log output consistent থাকে, duplicate object হয় না।  

**Example:**
```csharp
Logger logger = Logger.GetInstance();
logger.Log("User logged in");
```

>👉 যতবারই call করা হোক না কেন, সবসময় একই Logger instance use হবে।

Similarly


### 2️⃣ Database Connection Pool – Limited Connections

**Problem:**  
Database server এ connection সংখ্যা সীমিত।  
যদি প্রতিবার নতুন object বানিয়ে connection খোলা হয় →  

- Server overload হতে পারে  
- Connection limit cross করলে error দিবে  
- Performance down হবে  

**Solution (Singleton):**  
Database connection pool কে Singleton হিসেবে design করা হয়।  
একবার pool তৈরি হয়, এরপর project জুড়ে সেই pool থেকে connection নেওয়া হয়।  
Connection গুলো reuse হয়, নতুন connection unnecessarily খোলা হয় না।  

**Example:**
```csharp
DatabaseConnectionPool pool = DatabaseConnectionPool.GetInstance();
var connection = pool.GetConnection();
```
>👉 সবসময় একই pool থেকে connection পাওয়া যাবে।

### 3️⃣ Configuration Manager – Shared Settings

**Problem:**  
Project এ অনেক configuration settings থাকে (API keys, DB connection strings, server URLs, feature flags ইত্যাদি)।  
যদি প্রতিবার নতুন Config object load হয় →  

- Value mismatch হতে পারে  
- Performance slow হয় (কারণ বারবার file থেকে পড়তে হবে)  

**Solution (Singleton):**  
Configuration একবার load হয় → Singleton instance এ save থাকে।  
Project এর যেকোনো জায়গা থেকে সেই একই config access করা যায়।  

**Example:**
```csharp
ConfigManager config = ConfigManager.GetInstance();
string dbUrl = config.Get("DatabaseURL");
```
>👉 একবার load → project জুড়ে সবার জন্য same config।


### 4️⃣ Cache Manager – One Shared Cache System

**Problem:**  
Project এ কিছু frequently used data থাকে (user session, product list, recent searches)।  
যদি প্রতিবার নতুন cache object বানানো হয় → data inconsistency হবে।  
এক জায়গায় cache update হলে, অন্য জায়গায় outdated data থাকতে পারে।  

**Solution (Singleton):**  
একটাই Cache Manager instance রাখা হয়।  
সব জায়গা থেকে একই cache object access করা হয়।  
Cache একবার update হলে project এর সব জায়গায় consistent data থাকে।  

**Example:**
```csharp
CacheManager cache = CacheManager.GetInstance();
cache.Set("UserToken", "xyz123");
```
>👉 একটাই cache → সব component reuse করে।

<br>
<br>


-----

### 🏗️ Singleton Pattern – Logger Example

### 📝 Problem Without Singleton
ধরুন একটি বড় project চলছে। একসাথে অনেক task হচ্ছে যেমন write, get, আবার write।  

প্রতিটি task যদি আলাদা Logger object তৈরি করে →  

- আলাদা object গুলো একই log file এ লিখবে  
- Log sequence হারিয়ে যাবে  
- কোন event আগে ঘটেছে আর কোনটা পরে, track করা কঠিন হবে  

**Example:**  
- Task A → New Logger → "Start writing"  
- Task B → New Logger → "Fetching data"  
- Task C → New Logger → "Writing finished"  

এখানে কে আগে কে পরে হলো দেখা প্রায় impossible।  

### ✅ Solution With Singleton
- পুরো project এ **একটাই Logger object** থাকবে  
- সব task একই object ব্যবহার করবে → সব log এক জায়গায়, সঠিক sequence এ  

**Example:**  
- Task A → Singleton Logger → "Start writing"  
- Task B → Singleton Logger → "Fetching data"  
- Task C → Singleton Logger → "Writing finished"  



### 🏗️ Singleton Pattern (Logger Example with Thread Safety)

### 📝 Step 1: Simple Singleton Code (No Multithreading)
```csharp
using System;

public class Logger {
    private Logger() {
        Console.WriteLine("Logger created");
    }

    public static Logger _instance = null;

    public static Logger GetInstance() {
        if (_instance == null) {
            _instance = new Logger();
        }
        return _instance;
    }

    public void Log(string message) {
        Console.WriteLine(message);
    }
}

class Program {
    public static void Main() {
        Logger logger1 = Logger.GetInstance();
        Logger logger2 = Logger.GetInstance();
    }
}
```
### ❌ Problem 1 : Multi-thread Environment এ Issue

উপরের কোড single-threaded হলে কাজ করে। কিন্তু project এ সাধারণত একসাথে অনেক কাজ (multi-threading) হয়।

- যদি দুইটা thread একসাথে _instance == null check করে, তখন দুইজনই new Logger() execute করে ফেলতে পারে।


- ফলে দুইবার object তৈরি হবে।

📝 Step 2: Multithreaded Test
```c#
using System;
using System.Threading;

public class Logger {
    private Logger() {
        Console.WriteLine("Logger created");
    }

    public static Logger _instance = null;

    public static Logger GetInstance() {
        if (_instance == null) {
            _instance = new Logger();
        }
        return _instance;
    }
}

class Program {
    public static void Main() {
        Thread thread1 = new Thread(() => {
            Logger log1 = Logger.GetInstance();
        });
        Thread thread2 = new Thread(() => {
            Logger log2 = Logger.GetInstance();
        });

        thread1.Start();
        thread2.Start();

        thread1.Join();
        thread2.Join();

        Console.WriteLine($"Same connection? {ReferenceEquals(thread1, thread2)}");
    }
}
```
❌ Unexpected Output:

```yaml
Logger created
Logger created
Same connection? False
```
দুইবার instance তৈরি হলো → Thread safety সমস্যা।

➡️ দুইবার instance তৈরি হলো, অথচ Logger তো ideally একবারই তৈরি হওয়ার কথা।

**Reason:**
- Thread1 check করলো _instance == null → true


- Thread2 check করলো _instance == null → true


- দুইজনই নতুন Logger বানিয়ে ফেললো → দুইটা instance তৈরি হয়ে গেল


### 📝 Step 3: Thread-Safe Singleton (Solution with Lock)
Critical section এ lock ব্যবহার করা হবে

Double-check locking technique → lock এর ভেতরে আবার check

```csharp

using System;
using System.Threading;

public class Logger {
    private Logger() {
        Console.WriteLine("Logger created");
    }

    public static Logger _instance = null;
    public static object _lock = new object();

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
        Console.WriteLine(message);
    }
}

class Program {
    public static void Main() {
        Thread thread1 = new Thread(() => {
            Logger log1 = Logger.GetInstance();
        });
        Thread thread2 = new Thread(() => {
            Logger log2 = Logger.GetInstance();
        });

        thread1.Start();
        thread2.Start();

        thread1.Join();
        thread2.Join();
    }
}
```
✅ Correct Output:

```
Logger created
```
একটাই Logger instance তৈরি হলো।

➡️ এবার শুধুমাত্র একটাই Logger instance তৈরি হলো, যেটা Singleton এর মূল উদ্দেশ্য।


### 📌 Final Takeaways

### Why Logger needs Singleton?
- Logger file একটাই → একাধিক object তৈরি হলে log sequence গুলিয়ে যাবে।
- Singleton দিয়ে project জুড়ে consistent log maintain করা যায়।

### Why Lock needed?
- Multi-threaded project এ একই সময়ে দুইটা object তৈরি হওয়া ঠেকাতে।

### Real-world Same Problem:
- Database connection pool
- Cache manager
- Configuration manager
