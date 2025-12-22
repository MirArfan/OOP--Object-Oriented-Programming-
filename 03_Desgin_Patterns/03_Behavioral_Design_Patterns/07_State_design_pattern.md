# 🟢 State Design Pattern


### 1️⃣ Definition

State Pattern allows **an object to alter its behavior when its internal state change**s. The object will appear to change its class.

 
>State Pattern ব্যবহার করা হয় যখন কোনো object-এর অভ্যন্তরীণ state পরিবর্তনের সঙ্গে সঙ্গে তার behavior পরিবর্তন করতে হয়।  
→ মানে, state change হলে object-এর behaviorও automatic change হয়।

<br>

### 2️⃣ Why Needed

- Object behavior **depends on its state**  
- Many `if-else` or `switch-case` statements exist to check state and decide behavior  
- Keeping state logic in separate classes makes code **clean & maintainable**

<br>

### 3️⃣ Use Cases

- GUI Button (Enable / Disable / Hover)  
- TCP Connection (Closed / Listening / Established)  
- Document (Draft / Moderation / Published / Archived)  
- Vending Machine (NoCoin / HasCoin / SoldOut)  

<br>

### 4️⃣ Real World Example

- Traffic Light System – Red, Yellow, Green → behavior changes automatically  
- Music Player – Play, Pause, Stop → button actions differ based on state  
- Order Processing System – Pending, Shipped, Delivered → different allowed actions  

<br>

### 5️⃣ Goal & Purpose

### 🎯 Goal

- Behavior changes automatically according to the **current state**  

### 🎯 Purpose

- Reduce conditional logic (minimize `if-else`)  
- Keep object behavior **state-driven**  
- Maintain **Encapsulation & Separation of Concerns**  

<br>

### 6️⃣ How It Works

- **Context Class** → Object whose behavior depends on state  
- **State Interface** → Defines common behavior for all states  
- **Concrete States** → Implement specific behavior  
- **Context** holds a **State reference** → Behavior changes by switching state object  



<br>

### 🔹 Advantages of State Design Pattern

- ✅ **Avoids If-Else / Switch**  
  - Complex conditional logic can be cleaned up  

- ✅ **Easily Add State**  
  - Adding a new state does not require modifying existing code  

- ✅ **Maintains Encapsulation**  
  - Each state is a separate class, behavior is localized  

- ✅ **Runtime Behavior Change**  
  - Object behavior changes automatically according to its current state  
- ✅ Follows **Open/Closed Principle**

<br>

### 🔹 Disadvantages of State Design Pattern

- ❌ **Multiple States → More Classes**  
  - Each state requires a separate class  

- ❌ **Problem Handling Complexity**  
  - State switching and logic handling can get complex in large projects  

- ❌ **Overengineering in Simple Logic**  
  - For small/simple logic, creating multiple classes adds unnecessary complexity  

- ❌ **Maintenance Overhead**  
  - As the number of state classes increases, maintaining them becomes harder  


<br>

### 📝 Example 1 : Music Player

We have a **Music Player** with 3 states: **Stopped**, **Playing**, **Paused**.  

- Button presses change behavior according to the current state  
- In the previous approach, all logic is handled using **if-else statements**  

---

## ❌ Wrong Approach (Without State Pattern)

```csharp
using System;

class MusicPlayer
{
    public string State = "Stopped";

    public void PressPlay()
    {
        if (State == "Stopped" || State == "Paused")
        {
            Console.WriteLine("Music Started Playing");
            State = "Playing";
        }
        else if (State == "Playing")
        {
            Console.WriteLine("Music is already playing");
        }
    }

    public void PressPause()
    {
        if (State == "Playing")
        {
            Console.WriteLine("Music Paused");
            State = "Paused";
        }
        else
        {
            Console.WriteLine("Cannot pause. Music is not playing");
        }
    }

    public void PressStop()
    {
        if (State != "Stopped")
        {
            Console.WriteLine("Music Stopped");
            State = "Stopped";
        }
        else
        {
            Console.WriteLine("Music is already stopped");
        }
    }
}

class Program
{
    static void Main()
    {
        MusicPlayer player = new MusicPlayer();

        player.PressPlay();
        player.PressPause();
        player.PressStop();
        player.PressStop();
    }
}
```
### ❌ Problems (Without State Pattern)

- All behavior handled using **if-else / switch** → messy  
- Adding a new state requires **modifying existing code** → violates OCP  
- Behavior tightly coupled with state string → maintenance is hard  



### ✅ Solution: Using State Design Pattern


```csharp
using System;

//  Step 1: State Interface
interface IMusicState
{
    void PressPlay(MusicPlayer player);
    void PressPause(MusicPlayer player);
    void PressStop(MusicPlayer player);
}

// Step 2: Concrete States
// Stopped State
class StoppedState : IMusicState
{
    public void PressPlay(MusicPlayer player)
    {
        Console.WriteLine("Music Started Playing");
        player.SetState(new PlayingState());
    }

    public void PressPause(MusicPlayer player)
    {
        Console.WriteLine("Cannot pause. Music is stopped");
    }

    public void PressStop(MusicPlayer player)
    {
        Console.WriteLine("Music is already stopped");
    }
}

// Playing State
class PlayingState : IMusicState
{
    public void PressPlay(MusicPlayer player)
    {
        Console.WriteLine("Music is already playing");
    }

    public void PressPause(MusicPlayer player)
    {
        Console.WriteLine("Music Paused");
        player.SetState(new PausedState());
    }

    public void PressStop(MusicPlayer player)
    {
        Console.WriteLine("Music Stopped");
        player.SetState(new StoppedState());
    }
}

// Paused State
class PausedState : IMusicState
{
    public void PressPlay(MusicPlayer player)
    {
        Console.WriteLine("Music Resumed");
        player.SetState(new PlayingState());
    }

    public void PressPause(MusicPlayer player)
    {
        Console.WriteLine("Music is already paused");
    }

    public void PressStop(MusicPlayer player)
    {
        Console.WriteLine("Music Stopped");
        player.SetState(new StoppedState());
    }
}

// Step 3: Context Class

class MusicPlayer
{
    private IMusicState _state;

    public MusicPlayer()
    {
        _state = new StoppedState();
    }

    public void SetState(IMusicState state)
    {
        _state = state;
    }

    public void PressPlay() => _state.PressPlay(this);
    public void PressPause() => _state.PressPause(this);
    public void PressStop() => _state.PressStop(this);
}

// Step 4: Usage (Main Program)

class Program
{
    static void Main()
    {
        MusicPlayer player = new MusicPlayer();

        player.PressPlay();
        player.PressPause();
        player.PressPlay();
        player.PressStop();
        player.PressPause();
    }
}
```

🖥 Sample Output
```yaml
Music Started Playing
Music Paused
Music Resumed
Music Stopped
Cannot pause. Music is stopped
```