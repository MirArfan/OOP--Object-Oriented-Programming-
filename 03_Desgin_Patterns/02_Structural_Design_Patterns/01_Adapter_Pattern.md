## 🔌 Adapter Pattern


### 🔹 Introduction 

The **Adapter Pattern** allows incompatible classes or **interfaces to work together** by acting as a bridge between them.  
It converts the interface of one class into another interface that a client expects.  

**In simple terms:**  
It helps two classes work together even if their interfaces don’t match.



### 🔹  ব্যাখ্যা

Adapter Pattern এমন একটি প্যাটার্ন যা দুইটি অমিল ক্লাস বা ইন্টারফেসকে একসাথে কাজ করতে সাহায্য করে।  
এটি এক ক্লাসের ইন্টারফেসকে অন্য ক্লাসের প্রয়োজনীয় ফরম্যাটে রূপান্তর করে।  

**সহজভাবে বললে:**  
“Adapter হলো সংযোগকারী ব্রিজ, যা দুইটি অমিল ক্লাসকে একসাথে কাজ করায়।”

<br>

### 🧠 Real-life Analogy

Imagine your laptop charger plug 🧱 and wall socket ⚡ don’t match.  
You use a plug adapter so the charger fits the socket — that’s exactly what the Adapter Pattern does in software.

<br>

### 🧩 Key Points

| Concept | Description |
|---------|-------------|
| Type | Structural Design Pattern |
| Purpose | Make incompatible interfaces work together |
| Approach | Wrap one class inside another (Adapter) |
| Analogy | Like a power plug adapter or language translator |
| Used When | You want to reuse existing code with a different interface |

<br>

### ⚙️ Class Diagram (Conceptually)

```
Client → Target (Interface)
           ↑
        Adapter
           ↑
        Adaptee (Existing class)
````
### 🔌 Example 1 : Adapter Pattern
```c#
using System;
using System.Xml.Linq;
using Newtonsoft.Json;

// Step 1: Target Interface (client expects this)
public interface IXmlData {
    string GetXmlData();
}

// Step 2: Adaptee (incompatible class)
public class JsonData {
    private string _json;

    public JsonData(string json) {
        _json = json;
    }

    public string GetJsonData() {
        return _json;
    }
}

// Step 3: Adapter Class
public class JsonToXmlAdapter : IXmlData {
    private JsonData _jsonData;

    public JsonToXmlAdapter(JsonData jsonData) {
        _jsonData = jsonData;
    }

    public string GetXmlData() {
        // Convert JSON → XML
        string json = _jsonData.GetJsonData();
        var xml = JsonConvert.DeserializeXNode("{\"Root\":" + json + "}", "Root");
        return xml.ToString();
    }
}

// Step 4: Client
class Program {
    static void Main(string[] args) {
        // Suppose our new system gives JSON:
        string json = "{ \"name\": \"Mir Arfan\", \"role\": \"Developer\" }";

        // Old system expects XML, so we use adapter
        JsonData jsonData = new JsonData(json);
        IXmlData xmlAdapter = new JsonToXmlAdapter(jsonData);

        Console.WriteLine("---- JSON Data ----");
        Console.WriteLine(jsonData.GetJsonData());

        Console.WriteLine("\n---- Converted XML Data ----");
        Console.WriteLine(xmlAdapter.GetXmlData());
    }
}

```
🧾 Output
```
---- JSON Data ----
{ "name": "Mir Arfan", "role": "Developer" }

---- Converted XML Data ----
<Root>
  <name>Mir Arfan</name>
  <role>Developer</role>
</Root>
```
### 🔍 Explanation Table

| Step | Class / Interface | Description |
|------|--------------------|--------------|
| 1️⃣ | `IXmlData` | Target interface (old system expects XML from this). |
| 2️⃣ | `JsonData` | New class that produces JSON data. |
| 3️⃣ | `JsonToXmlAdapter` | Converts JSON → XML so old system can still work. |
| 4️⃣ | `Program` | Client — uses the adapter to get XML from JSON. |



### 🧠  ব্যাখ্যা

আমাদের পুরনো সিস্টেম **XML data** নিয়ে কাজ করে।  
কিন্তু নতুন সিস্টেম data দেয় **JSON format** এ।  
দুটো format একে অপরের সাথে **compatible না**।  

তাই **Adapter (`JsonToXmlAdapter`)** ব্যবহার করে **JSON কে XML-এ রূপান্তর** করা হলো।  

👉 মানে Adapter দুই দিকের মধ্যে **translator হিসেবে কাজ করছে** —  
পুরনো সিস্টেম (XML) এবং নতুন সিস্টেম (JSON) এর মধ্যে সংযোগ স্থাপন করে।


### 🔌 Example : Adapter Pattern

```csharp
using System;

namespace AdapterPatternDemo
{
    // Step 1: Target Interface
    public interface IMediaPlayer
    {
        void Play(string audioType, string fileName);
    }

    // Step 2: Adaptee Class (Existing incompatible class)
    public class Mp4Player
    {
        public void PlayMp4(string fileName)
        {
            Console.WriteLine("Playing mp4 file: " + fileName);
        }
    }

    // Step 3: Adapter Class (Bridge between IMediaPlayer and Mp4Player)
    public class MediaAdapter : IMediaPlayer
    {
        private Mp4Player mp4Player = new Mp4Player();

        public void Play(string audioType, string fileName)
        {
            if (audioType.Equals("mp4", StringComparison.OrdinalIgnoreCase))
            {
                mp4Player.PlayMp4(fileName);
            }
        }
    }

    // Step 4: Client Class
    public class AudioPlayer : IMediaPlayer
    {
        private MediaAdapter mediaAdapter;

        public void Play(string audioType, string fileName)
        {
            if (audioType.Equals("mp3", StringComparison.OrdinalIgnoreCase))
            {
                Console.WriteLine("Playing mp3 file: " + fileName);
            }
            else if (audioType.Equals("mp4", StringComparison.OrdinalIgnoreCase))
            {
                mediaAdapter = new MediaAdapter();
                mediaAdapter.Play(audioType, fileName);
            }
            else
            {
                Console.WriteLine("Invalid media type: " + audioType);
            }
        }
    }

    // Step 5: Test / Main Program
    class Program
    {
        static void Main(string[] args)
        {
            AudioPlayer player = new AudioPlayer();

            player.Play("mp3", "song1.mp3");
            player.Play("mp4", "video1.mp4");
            player.Play("avi", "movie1.avi");

            Console.ReadLine();
        }
    }
}
```
🧾 Output
```
Playing mp3 file: song1.mp3
Playing mp4 file: video1.mp4
Invalid media type: avi
```
<br>

### 🔍 Adapter Pattern — Explanation

| Step | Description |
|------|-------------|
| 1️⃣ | `IMediaPlayer` defines the common interface expected by the client. |
| 2️⃣ | `Mp4Player` is an existing class with a different method (`PlayMp4`). |
| 3️⃣ | `MediaAdapter` bridges the gap by implementing `IMediaPlayer` and internally using `Mp4Player`. |
| 4️⃣ | `AudioPlayer` acts as the client, using the adapter to play MP4 files. |
| 5️⃣ | The adapter allows `AudioPlayer` to use `Mp4Player` without changing either class. |

<br>

### 🧱 SOLID Principle Applied

| Principle | How It’s Applied |
|-----------|----------------|
| Single Responsibility Principle (SRP) | Each class has a clear, single purpose (e.g., `AudioPlayer`, `MediaAdapter`). |
| Open/Closed Principle (OCP) | You can add support for new formats (e.g., `AVIPlayer`) by adding a new adapter, not by modifying existing code. |

<br>

### ⚖️ Advantages & Disadvantages

| ✅ Advantages (সুবিধা) | ❌ Disadvantages (অসুবিধা) |
|------------------------|----------------------------|
| Enables reuse of existing classes with incompatible interfaces. <br>🔹 অমিল ক্লাস একসাথে কাজ করতে পারে। | Increases code complexity due to extra layer (`Adapter` class). <br>🔹 অতিরিক্ত ক্লাসের কারণে কোড জটিল হয়। |
| Follows Open/Closed Principle — no need to change existing code. <br>🔹 পুরনো কোড না বদলে নতুন ফরম্যাট যোগ করা যায়। | Can reduce performance slightly due to extra calls. <br>🔹 অতিরিক্ত function call এর কারণে সামান্য পারফরম্যান্স কমে যায়। |
| Improves code reusability and flexibility. <br>🔹 পুনরায় ব্যবহারযোগ্য এবং flexible কোড তৈরি হয়। | If too many adapters are used, the design becomes hard to maintain. <br>🔹 অনেক অ্যাডাপ্টার থাকলে maintenance কঠিন হয়। |
| Promotes loose coupling between client and existing class. <br>🔹 ক্লায়েন্ট এবং ক্লাসের মধ্যে dependency কমায়। | Difficult to understand for beginners. <br>🔹 শুরুতে বোঝা কিছুটা কঠিন হতে পারে। |

<br>

### 🧠 Summary

| Aspect | Description |
|--------|-------------|
| Pattern Type | Structural |
| Purpose | Connect incompatible interfaces |
| Main Principle | Open/Closed Principle |
| Real-life Analogy | Plug adapter / language translator |
| Used In | Integrating old code with new APIs, third-party library integration |
