# 🟦 Flyweight Design Pattern

### 🟩 1. Definition

Flyweight Pattern is a structural design pattern that allows **sharing common parts of objects** to **reduce memory usage**.  
It is useful when you need to create a large number of similar objects.

 
Flyweight Pattern হলো একটি structural design pattern যা object-এর common অংশগুলো share করে memory কমায়।  
যখন অনেক সংখ্যক একই ধরনের object তৈরি করতে হয় তখন এটি খুব কার্যকর।

<br>

### 🟩 2. Why Needed 
 
- Creating many similar objects consumes a lot of memory  
( অনেক একই ধরনের object তৈরি করলে memory বেশি লাগে  )
- Sharing intrinsic state reduces memory usage  
- Only extrinsic state is supplied by client  
( Extrinsic state client provide করে )

<br> 

### 🟩 3. Real-Life Example 


#### 1️⃣ Text Editor Characters
 
Large documents have repeated characters (A, B, C).  
Instead of creating a new object for each character, share common character data.  
Only store position, font size, or color as **extrinsic state**.

 

#### 2️⃣ Tree Objects in a Game

Forest in a game has thousands of trees.  
Each tree type (Oak, Pine) shares same graphical data (intrinsic state).  
Position, size are extrinsic state.
  
Position, size extrinsic state হিসেবে client provide করে।  

#### 3️⃣ Icons in a GUI Application
 
Desktop or mobile GUI has many icons.  
Each icon type shares same image data.  
Position on screen is extrinsic state.



#### 4️⃣ Chess Game Pieces

Chessboard has many pieces (pawn, rook, knight).  
Each type shares common data (shape, color).  
Board position, state (moved or not) are extrinsic state.

 

#### 5️⃣ Particle Effects in Games / Animations
 
Particle effects like smoke, fire, spark.  
Many particles share same texture or shape (intrinsic state).  
Position, speed, life time are extrinsic state.

<br>

### 🟩 4. Goal / Purpose
 
- Minimize memory usage  
- Share as much data as possible  
- Separate intrinsic (shared) and extrinsic (unique) state  

<br>

### 🟩 5. Use Cases 

- Text processing (characters, fonts)  
- Graphics (icons, trees in a game)  
- Large number of objects with common data  
- When object creation cost is high  

<br>

### 🟩 6. How It Works 
- Intrinsic state (shared data) store করা হয় Flyweight object এ  
- Extrinsic state (context-specific data) client provide করে  
- Flyweight Factory নিশ্চিত করে যে একই intrinsic state multiple object share করবে  
- Memory efficiency বৃদ্ধি পায়  

<br>

### 🟩 7. Advantages 
 
- Reduces memory usage  
- Improves performance  
- Allows creation of large number of objects efficiently  



### ❌ 8. Disadvantages 

- Increases complexity (need to manage intrinsic/extrinsic state)  
- Overhead for object sharing logic  
- Not suitable for objects that are mostly unique  

<br>

### 🟩 Example 1 : – Text Editor Character Rendering System 

### 📌 Scenario

ধরো তুমি একটি text editor বানাচ্ছো (যেমন VSCode, Notepad, Word)।  
একটি document-এ হয়তো **10,000+ characters** থাকতে পারে।

যদি প্রতিটি character-এর জন্য আলাদা object তৈরি করো → **massive memory waste**।

### ❗ কেন?
- একই character বারবার আসে (A, A, A…)
- একই font style অনেক character share করে
- Extrinsic state (position, color, size) character অনুযায়ী আলাদা  
- তাই প্রতিটি object আলাদা করে বানানো → memory heavy  

➡️ এটি solve করার জন্য **Flyweight Pattern** ব্যবহার করা হয়।


### ❌ Bad Approach (No Flyweight – Direct Object Creation)
```c#
using System;
using System.Collections.Generic;

public class Character
{
    public char Symbol { get; set; }
    public string FontFamily { get; set; }
    public int FontSize { get; set; }
    public int PositionX { get; set; }
    public int PositionY { get; set; }

    public Character(char symbol, string fontFamily, int fontSize, int x, int y)
    {
        Symbol = symbol;
        FontFamily = fontFamily;
        FontSize = fontSize;
        PositionX = x;
        PositionY = y;
    }

    public void Display()
    {
        Console.WriteLine($"Character '{Symbol}' at ({PositionX}, {PositionY}) using {FontFamily} {FontSize}");
    }
}

class Program
{
    static void Main()
    {
        List<Character> doc = new List<Character>();

        // Creating 10,000 characters manually (BAD)
        for (int i = 0; i < 5; i++)
        {
            doc.Add(new Character('A', "Arial", 16, i, i * 10));
        }

        foreach (var c in doc)
            c.Display();
    }
}
```

Output : 
```
Character 'A' at (0, 0) using Arial 16
Character 'A' at (1, 10) using Arial 16
Character 'A' at (2, 20) using Arial 16
Character 'A' at (3, 30) using Arial 16
Character 'A' at (4, 40) using Arial 16
```

### ❗ Problems 



- প্রতিটি character object আলাদা তৈরি হচ্ছে

- FontFamily & FontSize পুনরাবৃত্তি—memory waste

- বড় ডকুমেন্টে হাজার হাজার object → performance slow



✅ Flyweight Pattern Solution
```c#
using System;
using System.Collections.Generic;

/// ✔ Step 1: Flyweight Class (Shared Intrinsic State)
public class CharacterFlyweight
{
    public char Symbol { get; private set; }
    public string FontFamily { get; private set; }
    public int FontSize { get; private set; }

    public CharacterFlyweight(char symbol, string fontFamily, int fontSize)
    {
        Symbol = symbol;
        FontFamily = fontFamily;
        FontSize = fontSize;
    }

    public void Display(int x, int y)
    {
        Console.WriteLine($"Character '{Symbol}' at ({x}, {y}) using {FontFamily} {FontSize}");
    }
}

/// ✔ Step 2: Flyweight Factory
public class FlyweightFactory
{
    private Dictionary<string, CharacterFlyweight> cache = new Dictionary<string, CharacterFlyweight>();

    public CharacterFlyweight GetFlyweight(char symbol)
    {
        string key = $"{symbol}-Arial-16";

        if (!cache.ContainsKey(key))
        {
            cache[key] = new CharacterFlyweight(symbol, "Arial", 16);
        }

        return cache[key];
    }
}

/// ✔ Step 3: Client Code
class Program
{
    static void Main()
    {
        FlyweightFactory factory = new FlyweightFactory();

        // Simulating 5 characters
        for (int i = 0; i < 5; i++)
        {
            var ch = factory.GetFlyweight('A');  // Shared object
            ch.Display(i * 10, i * 5);          // Extrinsic state
        }
    }
}
```

### 🟩 Output
```
Character 'A' at (0, 0) using Arial 16
Character 'A' at (10, 5) using Arial 16
Character 'A' at (20, 10) using Arial 16
Character 'A' at (30, 15) using Arial 16
Character 'A' at (40, 20) using Arial 16
```

### 🧠 Easy Explanation 

#### 🟩 Intrinsic State (Shared)
- Character (`'A'`)
- FontFamily (`"Arial"`)
- FontSize (`16`)

#### 🟦 Extrinsic State (Unique)
- Position (`x, y`)

Flyweight Pattern **একই character-object share করে**, ফলে memory usage অনেক কমে যায়।

<br>

## 🌳 Example 2 : Game Tree

You are making an open-world 3D game (like **GTA**, **Red Dead Redemption**, **PUBG**).  
The world contains **50,000+ trees**.

### All trees:
- Use the **same model (3D mesh)**
- **Same texture file**
- But **different positions, height, rotation, color variations (extrinsic)**

If every tree stores its own model + texture → **huge memory waste** → game lags, crashes.



### ❌ BAD CODE (Without Flyweight – Memory Waste)

### **Problem**

Each tree object stores:

- 3D model (**20 MB**)
- Texture (**5 MB**)
- Shader data
- Position
- Size
- Rotation

If **50,000 trees × 25 MB = 1,250,000 MB ≈ 1.2 TB memory.**  
➡️ **Game instantly crashes.**

### ❌ Bad Code Example 
```C#

using System;
using System.Collections.Generic;

public class Tree
{
    public string Model;     // 20 MB
    public string Texture;   // 5 MB
    public int X;
    public int Y;

    public Tree(string model, string texture, int x, int y)
    {
        Model = model;
        Texture = texture;
        X = x;
        Y = y;
    }

    public void Draw()
    {
        Console.WriteLine("Drawing Tree at " + X + ", " + Y);
    }
}

public class Forest
{
    public List<Tree> Trees = new();

    public void PlantTrees()
    {
        for (int i = 0; i < 5; i++)  // demo এর জন্য 5, না হলে 50000 line output huge হবে
        {
            Trees.Add(new Tree("treeModel.obj", "treeTexture.png", i, i + 10));
        }
    }

    public void Show()
    {
        foreach (var t in Trees)
            t.Draw();
    }
}

class Program
{
    static void Main(string[] args)
    {
        Forest forest = new Forest();
        forest.PlantTrees();
        forest.Show();
    }
}
```

Output:
```
Drawing Tree at 0, 10
Drawing Tree at 1, 11
Drawing Tree at 2, 12
Drawing Tree at 3, 13
Drawing Tree at 4, 14
```

### ❌ BAD CODE SUMMARY
Problem 1:

- Each tree keeps duplicate model + texture.

- Memory usage goes out of control.


- Game performance becomes terrible.

This is where Flyweight Pattern solves everything.

### ✅ Flyweight Pattern – SOLUTION
#### Core Idea

Store shared (intrinsic) data once:

- Model

- Texture

- Tree type

Store unique (extrinsic) separately:

- X, Y position

- Rotation

- Scale

Thousands of trees reuse **only ONE TreeType** object.


### ✅ Flyweight Solution Code

1️⃣ Flyweight Class (Shared Tree Model + Texture)
```c#
using System;
using System.Collections.Generic;

///////////////////////////////
// BAD CODE (Memory Heavy)
///////////////////////////////

public class BadTree
{
    public string Model; // 20 MB
    public string Texture; // 5 MB
    public int X;
    public int Y;

    public BadTree(string model, string texture, int x, int y)
    {
        Model = model;
        Texture = texture;
        X = x;
        Y = y;
    }

    public void Draw()
    {
        Console.WriteLine($"Drawing Tree at ({X}, {Y})");
    }
}

public class BadForest
{
    public List<BadTree> Trees = new();

    public void PlantTrees()
    {
        for (int i = 0; i < 5; i++)   // 5 korechi demo output dekhar jonno
        {
            Trees.Add(new BadTree("treeModel.obj", "treeTexture.png", i, i + 10));
        }
    }

    public void Show()
    {
        foreach (var t in Trees)
            t.Draw();
    }
}

///////////////////////////////
// FLYWEIGHT SOLUTION
///////////////////////////////

public class TreeType
{
    public string Model;   // Shared
    public string Texture; // Shared

    public TreeType(string model, string texture)
    {
        Model = model;
        Texture = texture;
    }

    public void Draw(int x, int y)
    {
        Console.WriteLine($"Drawing SHARED tree at ({x}, {y})");
    }
}

public class TreeFactory
{
    private static Dictionary<string, TreeType> treeTypes = new();

    public static TreeType GetTreeType(string key, string model, string texture)
    {
        if (!treeTypes.ContainsKey(key))
        {
            treeTypes[key] = new TreeType(model, texture);
        }
        return treeTypes[key];
    }
}

public class Tree
{
    public int X;
    public int Y;
    public TreeType Type;   // Flyweight object

    public Tree(int x, int y, TreeType type)
    {
        X = x;
        Y = y;
        Type = type;
    }

    public void Draw()
    {
        Type.Draw(X, Y);
    }
}

public class Forest
{
    private List<Tree> Trees = new();

    public void PlantTree(int x, int y, string key, string model, string texture)
    {
        TreeType type = TreeFactory.GetTreeType(key, model, texture);
        Trees.Add(new Tree(x, y, type));
    }

    public void PlantManyTrees()
    {
        for (int i = 0; i < 5; i++)  // demo output
        {
            PlantTree(i, i + 10, "oak", "treeModel.obj", "treeTexture.png");
        }
    }

    public void Draw()
    {
        foreach (var t in Trees)
            t.Draw();
    }
}

///////////////////////////////
// MAIN METHOD
///////////////////////////////

class Program
{
    static void Main()
    {
        Console.WriteLine("---- BAD CODE OUTPUT ----");
        var badForest = new BadForest();
        badForest.PlantTrees();
        badForest.Show();

        Console.WriteLine("\n---- FLYWEIGHT OUTPUT ----");
        var forest = new Forest();
        forest.PlantManyTrees();
        forest.Draw();
    }
}
```

Output :
```
---- BAD CODE OUTPUT ----
Drawing Tree at (0, 10)
Drawing Tree at (1, 11)
Drawing Tree at (2, 12)
Drawing Tree at (3, 13)
Drawing Tree at (4, 14)

---- FLYWEIGHT OUTPUT ----
Drawing SHARED tree at (0, 10)
Drawing SHARED tree at (1, 11)
Drawing SHARED tree at (2, 12)
Drawing SHARED tree at (3, 13)
Drawing SHARED tree at (4, 14)
```

### 📉 Memory Usage After Flyweight
Before Flyweight

- 50,000 trees × 25 MB = 1.2 TB

After Flyweight

- Flyweight stored once: ~25 MB
- 50,000 lightweight nodes ~ few MB

➡ >99% memory saved

➡ Game runs smoothly


### explanation: 

🔥 **Bad Code**
- Every tree creates new model + texture  
- Memory wasted  
- 50,000 tree = **1.2 TB**  
- Game crashes  


🌟 **Flyweight**
- Shared Struct (**TreeType**) stores model + texture **only once**  
- Individual Tree stores only **x, y**  
- Memory saved **99%**  