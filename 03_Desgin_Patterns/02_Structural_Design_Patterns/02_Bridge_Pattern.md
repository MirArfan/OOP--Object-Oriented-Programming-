## 🌉 Bridge Design Pattern

### 🔹 Introduction 
The Bridge Pattern is a **Structural Design Pattern** that separates an abstraction from its implementation so that the two can vary independently.  

➡️ **Simple Terms:**  
It lets you **change abstractions and implementations independently** without modifying existing code.  

**Key Idea:**  
- Split a large class or closely related set of classes into two separate hierarchies:  
  1. **Abstraction** – defines the high-level control/logic  
  2. **Implementation** – provides the low-level details  

<br>

### 🔹 ব্যাখ্যা
Bridge Pattern হলো একটি Structural Pattern যা abstraction (উচ্চ স্তরের ধারণা) এবং implementation (বিস্তারিত বাস্তবায়ন) আলাদা রাখে।  
➡️ অর্থাৎ, abstraction এবং implementation আলাদাভাবে পরিবর্তন করা যায়, কোডের ওপর প্রভাব ছাড়াই।  

**সহজভাবে বললে:**  
“Bridge হলো abstraction আর implementation এর মধ্যে একটা সেতু। দুইটা independently পরিবর্তনযোগ্য হয়।”

<br>

### 🧠 Real-life Analogy
- TV Remote (Abstraction) ↔ TV Brands (Implementation)  
- Remote works with different TVs without changing the remote or TV brand’s internal code.  

<br>



### 🧩 Key Points

| Concept | Description |
|---------|-------------|
| Type | Structural Design Pattern |
| Purpose | Decouple abstraction from implementation |
| Approach | Use composition: Abstraction has a reference to Implementation |
| Used When | You want to vary both abstraction and implementation independently |

<br>

### 🎯 Scenario & Problem

ধরা যাক, তুমি একটা drawing app বানাচ্ছো।  
তোমার requirement: বিভিন্ন Shape (Circle, Square) থাকতে হবে এবং প্রতিটা shape বিভিন্ন Color (Red, Blue) support করবে।

❌ Problem with Bad Code (Using Inheritance Only)

Inheritance দিয়ে করতে গেলে class explosion হবে:

- RedCircle
- BlueCircle
- RedSquare
- BlueSquare
- GreenCircle
- GreenSquare
- …etc

প্রতি combination এর জন্য নতুন class তৈরি করতে হবে → Code maintain করা কঠিন, scalable নয়, repetitive। 😩

### ❌ Bad Code (Class Explosion Example)
```c#
using System;

abstract class Shape
{
    public abstract void Draw();
}

// Red Shapes
class RedCircle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing Circle in Red");
    }
}

class RedSquare : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing Square in Red");
    }
}

// Blue Shapes
class BlueCircle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing Circle in Blue");
    }
}

class BlueSquare : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing Square in Blue");
    }
}

// Client
class Program
{
    static void Main(string[] args)
    {
        Shape shape1 = new RedCircle();
        Shape shape2 = new BlueSquare();

        shape1.Draw();
        shape2.Draw();
    }
}
```
### 🔍 Problem with this approach:

- **Class Explosion**: প্রতিটি color + shape combination এর জন্য নতুন class তৈরি করতে হবে।

- RedCircle, BlueCircle, RedSquare, BlueSquare → 4 classes, যদি আরও colors/shape add করি তাহলে class সংখ্যা বাড়বে exponentially।

- **Maintenance nightmare**: নতুন color বা shape যোগ করলে existing code modify করতে হবে।

- **Rigid code:** Composition বা runtime flexibility নেই।

<br>

### 💡 Solution (Good Code — Bridge Pattern)

Bridge pattern বলছে:

1. দুইটি hierarchy আলাদা করো:
   - Shape (Abstraction)
   - Color (Implementor)

2. Shape class এর মধ্যে Composition দিয়ে Color রাখো (has-a relationship)

3. Client চাইলে যেকোনো Shape + যেকোনো Color combine করতে পারবে, নতুন class বানানোর দরকার নেই।

<br>

🧩 Structure Diagram
```
Abstraction (Shape)
   ↓ has-a
Implementor (Color)
```

Shape “has a” Color → আলাদা করে পরিবর্তনযোগ্য এবং scalable।

<br>

### 💻 Example ( — Bridge Pattern)

```csharp
using System;

// Implementor
public interface IColor
{
    void ApplyColor();
}

// Concrete Implementors
public class RedColor : IColor
{
    public void ApplyColor()
    {
        Console.WriteLine("Applying Red Color");
    }
}

public class BlueColor : IColor
{
    public void ApplyColor()
    {
        Console.WriteLine("Applying Blue Color");
    }
}

// Abstraction
public abstract class Shape
{
    protected IColor color;

    protected Shape(IColor color)
    {
        this.color = color;
    }

    public abstract void Draw();
}

// Refined Abstraction
public class Circle : Shape
{
    public Circle(IColor color) : base(color) {}

    public override void Draw()
    {
        Console.Write("Drawing Circle → ");
        color.ApplyColor();
    }
}

public class Square : Shape
{
    public Square(IColor color) : base(color) {}

    public override void Draw()
    {
        Console.Write("Drawing Square → ");
        color.ApplyColor();
    }
}

// Client
class Program
{
    static void Main(string[] args)
    {
        Shape redCircle = new Circle(new RedColor());
        Shape blueSquare = new Square(new BlueColor());

        redCircle.Draw();
        blueSquare.Draw();
    }
}
```
🧾 Output

```
Drawing Circle → Applying Red Color
Drawing Square → Applying Blue Color
```

<br>

### 💡 Benefits / Solution Explained:

**1. No Class Explosion:**

- শুধু Shape এবং Color এর আলাদা hierarchy আছে।

- নতুন shape বা color যোগ করতে শুধু নতুন class বানাতে হবে, existing class modify করার দরকার নেই।

**2. Flexible & Maintainable:**
- Runtime এ যে কোনো color + shape combination তৈরি করা সম্ভব।

**3. Composition over Inheritance:**

- Shape has-a IColor → Loose coupling।

<br>

### 🔍 Explanation Table

| Layer                 | Class                 | কাজ / Responsibility                          |
|----------------------|----------------------|----------------------------------------------|
| Implementor          | IColor               | Color apply করার interface                   |
| Concrete Implementor  | RedColor, BlueColor  | বাস্তব Color implementation                  |
| Abstraction          | Shape                | Shape এর generic structure                   |
| Refined Abstraction  | Circle, Square       | নির্দিষ্ট shape implementation               |
| Bridge (Composition) | Shape → IColor       | Shape “has a” Color, composition ব্যবহার     |

### 🧠  Explanation

Bridge pattern এ দুইটি আলাদা hierarchy থাকে:

1. **Shape related** (Abstraction)  
2. **Color related** (Implementor)  

Shape class এর মধ্যে Color object রাখা হয় (Composition) → এর ফলে:

- Shape independent  
- Color independent  
- Client সহজে যেকোনো combination ব্যবহার করতে পারে  
- Class explosion এড়িয়ে যাওয়া যায়


<br>

### ⚙️ Class Diagram (Conceptual)

```
Abstraction
   ↑
RefinedAbstraction
   |
Implementation (interface)
   ↑
ConcreteImplementationA
ConcreteImplementationB
```

- **Abstraction:** Defines interface and holds reference to Implementation  
- **RefinedAbstraction:** Extends abstraction, adds more behavior  
- **Implementation:** Interface for low-level operations  
- **ConcreteImplementation:** Actual implementation of the interface  



### 🔌 Bridge Pattern –  Example : 2

```csharp
using System;

// Step 1: Implementation Interface
interface IRenderer
{
    void RenderCircle(float radius);
}

// Step 2: Concrete Implementations
class VectorRenderer : IRenderer
{
    public void RenderCircle(float radius)
    {
        Console.WriteLine($"Drawing a circle of radius {radius} as lines (Vector)");
    }
}

class RasterRenderer : IRenderer
{
    public void RenderCircle(float radius)
    {
        Console.WriteLine($"Drawing a circle of radius {radius} as pixels (Raster)");
    }
}

// Step 3: Abstraction
abstract class Shape
{
    protected IRenderer renderer;
    public Shape(IRenderer renderer)
    {
        this.renderer = renderer;
    }
    public abstract void Draw();
}

// Step 4: Refined Abstraction
class Circle : Shape
{
    public float Radius { get; set; }

    public Circle(IRenderer renderer, float radius) : base(renderer)
    {
        Radius = radius;
    }

    public override void Draw()
    {
        renderer.RenderCircle(Radius);
    }
}

// Step 5: Client Code
class Program
{
    static void Main(string[] args)
    {
        IRenderer vector = new VectorRenderer();
        IRenderer raster = new RasterRenderer();

        Shape circle1 = new Circle(vector, 5);
        Shape circle2 = new Circle(raster, 10);

        circle1.Draw(); // Vector circle
        circle2.Draw(); // Raster circle
    }
}
```
🧾 Output
```
Drawing a circle of radius 5 as lines (Vector)
Drawing a circle of radius 10 as pixels (Raster)
```

<br>

### 🔍 Explanation (Step-by-Step)

| Step | Description |
|------|-------------|
| 1️⃣ | IRenderer defines low-level drawing operations |
| 2️⃣ | VectorRenderer & RasterRenderer implement the rendering |
| 3️⃣ | Shape abstraction holds reference to IRenderer |
| 4️⃣ | Circle (RefinedAbstraction) uses IRenderer to draw shapes |
| 5️⃣ | Client can mix and match any Shape with any Renderer independently |

<br>

### ✅ Advantages (সুবিধা)
- **Reduces Class Explosion** → নতুন color বা shape যোগ করা সহজ  
- **Loose Coupling** → Shape এবং Color independent  
- **Flexible & Maintainable** → Runtime এ নতুন combination তৈরি করা যায়  
- **Composition over Inheritance** → OOP best practice অনুসরণ  
- **Open/Closed Principle** → Existing class modify না করেই extension possible  

### ❌ Disadvantages (অসুবিধা)
- **Complexity** → দুইটি hierarchy manage করতে হয়  
- **Slight overhead** → extra reference (composition) থাকার কারণে সামান্য memory/processing overhead  
- **Learning curve** → concept বোঝা beginners জন্য প্রথমে কঠিন  

<br>

### 💬 Real-Life Examples
- **Remote Control & Device:** Remote abstraction, TV/Radio implementor  
- **Drawing App:** Shape abstraction (Circle/Square), Color implementation (Red/Blue/Green)  
- **GUI Framework:** Window abstraction, Skin/Theme implementation  
- **Media Player:** Player abstraction, Codec implementation  
