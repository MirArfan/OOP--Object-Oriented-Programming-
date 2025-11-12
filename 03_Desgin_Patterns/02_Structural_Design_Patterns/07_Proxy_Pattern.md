## 🧩 Proxy Pattern

### 🔹 Introduction

The **Proxy Pattern** provides a **substitute or placeholder** for another object to control access to it.  
It acts as an intermediary between the client and the real object.  
This pattern is often used when creating an object is expensive, or when you need extra control (like access control, lazy loading, caching, or logging).

➡️ In simple terms:  
**A proxy controls access to another object.**

<br>

### 🔹 ব্যাখ্যা

**Proxy Pattern** এমন একটি ডিজাইন প্যাটার্ন যা মূল অবজেক্টের পরিবর্তে **একটি মধ্যস্থ অবজেক্ট (proxy)** ব্যবহার করে।  
এটি ক্লায়েন্ট ও আসল অবজেক্টের মধ্যে যোগাযোগ নিয়ন্ত্রণ করে।  
যখন আসল অবজেক্ট তৈরি করা ব্যয়বহুল বা ঝুঁকিপূর্ণ, তখন Proxy তার পরিবর্তে কাজ করে।

👉 সহজভাবে বললে —  
“Proxy মানে হলো আসল অবজেক্টের প্রতিনিধি বা দারোয়ান, যে ক্লায়েন্টের অ্যাক্সেস নিয়ন্ত্রণ করে।”

<br>

### 🧠 Real-life Analogy

ধরো, তুমি কোনো **VIP ব্যক্তির সাথে দেখা করতে চাও**।  
তুমি সরাসরি তার কাছে যেতে পারবে না — আগে **Secretary (Proxy)** এর মাধ্যমে যেতে হবে।  
Secretary দেখে নেবে তুমি অনুমোদিত কিনা, তারপর তোমাকে ভিতরে পাঠাবে।

ঠিক তেমনই, Proxy মূল অবজেক্টে যাওয়ার আগে সব রিকোয়েস্ট নিয়ন্ত্রণ করে।

<br>

### 🧩 Key Points

| Concept | Description |
|----------|-------------|
| **Type** | Structural Design Pattern |
| **Purpose** | Control access to another object |
| **Approach** | Proxy implements same interface as Real Object |
| **Used When** | Direct access to real object is costly, risky, or needs control |
| **Analogy** | Security guard, secretary, or remote proxy |

<br>

### ⚙️ Class Diagram (Conceptually)
```
Client → ISubject (Interface)
   ↑
┌────────────┐
│ Proxy │ → controls access to → RealSubject
└────────────┘
```

<br>

### 💻  Example 1 :  – Proxy Pattern


তুমি একটা **Image Viewer App** বানাচ্ছো যেটা বড় বড় high-resolution ছবিগুলো লোড করে দেখায়।

প্রতিবার ইউজার যখন নতুন কোনো image দেখতে চায়, তখন ছবিটা disk থেকে লোড হয় — heavy process!  
এবং সমস্যা হলো, অ্যাপ চালু হতেই সব ছবিই লোড হয়ে যাচ্ছে 😩  

<br>

🔹 কিন্তু সমস্যা হলো —  
প্রতিবার app শুরু করলে সব image একসাথে লোড হয় 😩  
👉 ফলে সময় অনেক লাগে এবং memory consumption বেড়ে যায়।

### ❌ Bad Code (Without Proxy)

```csharp
using System;

// Interface
public interface IImage
{
    void Display();
}

// Heavy Object
public class RealImage : IImage
{
    private string fileName;

    public RealImage(string fileName)
    {
        this.fileName = fileName;
        LoadFromDisk();
    }

    private void LoadFromDisk()
    {
        Console.WriteLine($"Loading image from disk: {fileName}");
    }

    public void Display()
    {
        Console.WriteLine($"Displaying image: {fileName}");
    }
}

// Client Code
public class Program
{
    public static void Main()
    {
        // সব image শুরুতেই load হচ্ছে (unnecessary)
        IImage image1 = new RealImage("photo1.png");
        IImage image2 = new RealImage("photo2.png");

        Console.WriteLine("\n--- Images already loaded at startup ---\n");

        image1.Display();
        image2.Display();
    }
}
```
🧾 Output
```
Loading image from disk: photo1.png
Loading image from disk: photo2.png

--- Images already loaded at startup ---

Displaying image: photo1.png
Displaying image: photo2.png
```
### ⚠️ Problem
App start হতেই সব image load হচ্ছে

অনেক সময় ও মেমরি নষ্ট

Lazy loading নেই

Performance খারাপ



### ❌ Problem

- প্রতিটি image ফাইল লোড করতে সময় লাগে।  
- যদি image দেখা না লাগে, তাও লোড হয়ে যাচ্ছে (unnecessary resource usage)।  
- App ধীর হয়ে যাচ্ছে, performance খারাপ হচ্ছে।

<br>

### ✅ Good Code (With Proxy)
```c#
using System;

// Step 1: Common Interface
public interface IImage
{
    void Display();
}

// Step 2: Heavy Real Object
public class RealImage : IImage
{
    private string fileName;

    public RealImage(string fileName)
    {
        this.fileName = fileName;
        LoadFromDisk(); // heavy operation
    }

    private void LoadFromDisk()
    {
        Console.WriteLine($"Loading image from disk: {fileName}");
    }

    public void Display()
    {
        Console.WriteLine($"Displaying image: {fileName}");
    }
}

// Step 3: Proxy Class
public class ProxyImage : IImage
{
    private RealImage realImage;
    private string fileName;

    public ProxyImage(string fileName)
    {
        this.fileName = fileName;
    }

    public void Display()
    {
        if (realImage == null)
        {
            realImage = new RealImage(fileName); // load only when needed
        }
        realImage.Display();
    }
}

// Step 4: Client Code
public class Program
{
    public static void Main()
    {
        IImage image1 = new ProxyImage("photo1.png");
        IImage image2 = new ProxyImage("photo2.png");

        Console.WriteLine("\n--- Images created but not loaded yet ---\n");

        // Lazy Loading in action
        image1.Display();  // loads & displays
        Console.WriteLine();
        image2.Display();  // loads & displays
        Console.WriteLine();
        image1.Display();  // already loaded
    }
}
```

🧾 Output
```
--- Images created but not loaded yet ---

Loading image from disk: photo1.png
Displaying image: photo1.png

Loading image from disk: photo2.png
Displaying image: photo2.png

Displaying image: photo1.png
```

### 🔍 Explanation (Step-by-Step)

| Step | Class | Description |
|------|--------|-------------|
| 1️⃣ | **IImage** | Common interface for both Real and Proxy images |
| 2️⃣ | **RealImage** | Heavy object — loads image from disk (slow) |
| 3️⃣ | **ProxyImage** | Lightweight wrapper — loads RealImage only when needed |
| 4️⃣ | **Program** | Client — interacts only with ProxyImage |

<br>

### 🧠 Why Proxy is Better Here

- ✅ **Lazy Loading** → Image load হবে কেবল তখনই যখন দরকার  
- ✅ **Saves Memory** → Unused images load হয় না  
- ✅ **Better Performance** → Faster app startup  
- ✅ **Access Control / Logging** ও যোগ করা যায় সহজে  

<br>

### ⚖️ Advantages & Disadvantages

| ✅ Advantages | ❌ Disadvantages |
|---------------|------------------|
| Lazy loading improves performance | Adds extra class |
| Controls access to real object | Slightly slower due to delegation |
| Reduces memory usage | Increases code complexity |
| Adds logging, caching, or security easily | Harder to debug if many proxies used |
| Supports Open/Closed principle | — |




<br>

### 🧠 When to Use Proxy Pattern

✅ When object creation is **resource-heavy** (e.g., loading files, DB connections)  
✅ When you need **access control or security checks**  
✅ When you want **lazy loading or caching**  
✅ When you need to **log requests or usage tracking**

<br>

### ⚖️ Advantages & Disadvantages

| ✅ **Advantages (সুবিধা)** | ❌ **Disadvantages (অসুবিধা)** |
|------------------------------|-------------------------------|
| Controls access to the real object | Increases code complexity |
| Implements lazy loading — loads objects only when needed | Adds extra layer of indirection |
| Improves performance for expensive objects | Slightly slower due to delegation |
| Adds extra security, logging, or caching features | Can be hard to debug when multiple proxies are involved |
| Promotes single responsibility (separation of concern) | Needs more classes and interfaces |

<br>

### 🧩 Common Use Cases of Proxy Pattern

| Use Case|  Description | | Example |
|----------|-------------|---------|--|
| **Virtual Proxy** | Delays creation of heavy object until it’s really needed|যখন কোনো object তৈরি করা বা load করা খুব heavy — তখন proxy lazy-load করে | বড় image, video, বা database record — “load on demand” |
| **Protection Proxy** |Controls access based on permissions.| যখন access control দরকার — কিছু user কে allow, কিছু কে deny | Admin-only feature, authentication system |
| **Remote Proxy** |Represents an object located in a different address space (e.g., network).| যখন object অন্য server বা network এ থাকে — proxy local interface দেয় | API client, remote method invocation |
| **Caching Proxy** |  Stores/cache data to improve performance for repeated access.|যখন বারবার data আনলে performance কমে যায় — proxy cache করে রাখে | Web content caching |
| **Logging / Monitoring Proxy** |Adds logging or monitoring functionality when accessing an object.  |যখন system call গুলো track করতে চাও | API call log, performance monitoring proxy |
| **Smart Reference Proxy** |Adds extra actions when an object is accessed (e.g., reference counting, logging).| Object এর lifecycle manage করে | Reference counting, memory management |

<br>

### 🧠 Real-Life Examples (Easy to Visualize)

| বাস্তব উদাহরণ | Proxy কীভাবে কাজ করছে |
|---------------|---------------------|
| 🔒 **Bank ATM** | তুমি ব্যাংক ম্যানেজারের সাথে সরাসরি cash তুলতে পারো না — ATM (proxy) এর মাধ্যমে তুলো |
| 📺 **Netflix / YouTube** | ভিডিও তখনই stream হয় যখন play চাপো → lazy loading proxy |
| 🌐 **Internet Proxy Server** | তোমার request আগে proxy server এ যায়, তারপর বাইরে যায় → filtering, caching |
| 👮 **Security Guard** | অফিসে ঢোকার আগে security guard check করে — authorized হলে ঢুকতে দেয় (protection proxy) |

<br>


### 🧠 Summary

| Aspect | Description |
|--------|--------------|
| **Pattern Type** | Structural |
| **Purpose** | Control access to objects |
| **Real-life Example** | Security guard, secretary, remote access |
| **Key Benefit** | Lazy loading, security, and access control |
| **Principle Used** | Single Responsibility, Open/Closed Principle |


