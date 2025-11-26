# ⭐ Command Design Pattern — Definition (Bangla + English)

### 📌Definition  

The core idea of the Command Design Pattern is that you do not execute a business operation directly.
Instead, you **encapsulate the request into a Command object**, and then the **business logic executes that Command**.

This separates the sender of the request from the actual execution logic.

###  
প্রথমে রিকোয়েস্টকে একটি Command অবজেক্টে রূপান্তর করা হয়, এবং পরে সেই Command-ই ব্যবসায়িক লজিকের দ্বারা execute হয়।

এতে রিকোয়েস্ট পাঠানো এবং রিকোয়েস্ট execute করার অংশ এক-অন্যের থেকে আলাদা থাকে।

<br>

### 🎯 Purpose / Goal

| Purpose                                           |
|---------------------------------------|
| Encapsulate request as an object  (Request কে object বানানো           )     |   |
| Decouple sender & receiver        (Invoker এবং Receiver আলাদা রাখা     )     |  |
| Support Undo/Redo                      |   |
| Extend without modifying main system   | 

<br>

### ❗ Why Needed
  
Without this pattern, the caller directly depends on the receiver, making the system tightly coupled and difficult to modify or extend.

<br>

### 🏢 Real-World Examples

| Real Example            |       |
|----------------------------------|---------------------------------------------|
| TV Remote sends command to TV     | Remote বোতাম চাপলে TV কাজ করে              |
| Restaurant Waiter passes order    | Waiter order নেয় → Chef execute করে        |
| Text Editor Undo/Redo history     | প্রতিটি action আলাদা command হিসেবে store হয় |

<br>


### 📌 Use Cases 

|                      |
|----------------------------------------|
|1. Remote Control Systems — each action = command |
|2.  GUI Button Actions / Menu Click        |
| 3. Undo/Redo Support (Editor, Game)       |
| 4. Task Queue / Job Scheduling            |
| 5. Transactional Operations               |
| 6. Macro Recording (Multiple commands run at once) |
| 7. Smart Home Automation                  |




<br>

### 🏠 Real-Life Examples

| Real Example                 | Explanation                             |
|-----------------------------|--------------------------------------------------|
| TV Remote + TV              | Button → Command → TV executes                   |
| Restaurant Waiter + Chef    | Waiter writes order (command) → Chef executes    |
| Text Editor Undo/Redo       | Each typing action is stored as a command        |
| Smart Home Voice Assistant  | "Turn on fan" → Command → Fan executes           |
| Game Controls (Jump, Fire)  | Each player action mapped to a command object    |

<br>

### ☑️ Advantages and Disadvantages

|        ✔ Advantages                         | ❌ Disadvantages                                   |
|---------------------------------------|---------------------------------------------|
| 1. Loose-coupling achieved               | 1. More classes required    |
| 2. New commands add easily               | 2. Simple projects don't need it    |
| 3. Undo/Redo built-in possible           |  |
| 4. Commands can be queued/logged         |   |

<br>



### 🚨 Example 1 :

You build a Remote Control → It directly calls `Light.TurnOn()` & `Light.TurnOff()` 

Adding Fan, AC, Door requires modifying Remote class again & again.

### ❌ Bad Code (No Command Pattern)
```c#
using System;
class Light
{
    public void TurnOn() => Console.WriteLine("Light ON");
    public void TurnOff() => Console.WriteLine("Light OFF");
}

class RemoteControl
{
    private Light light = new Light();

    public void PressButton(string action)
    {
        if (action == "ON") light.TurnOn();
        else if (action == "OFF") light.TurnOff();
    }
}
class Program 
{
    static void Main(){
        RemoteControl remote = new RemoteControl();
        
        remote.PressButton("ON");
        remote.PressButton("OFF");
    }
}
```
Output :
```
Light ON
Light OFF
```
⚠️ **Problem**: Hard to extend, tightly coupled, no undo support.


### ✔ Solution — Using Command Design Pattern

💡 Convert each action into separate Command objects.  
Remote only executes command, it doesn’t know who or how the work is performed.



### 🏗 Structure

| Component         | Explanation                       |
|------------------|--------------------------------------------|
| Command Interface | Defines Execute(), Undo()                  |
| Concrete Command  | LightOnCommand / LightOffCommand — calls real work |
| Receiver          | Light — does the actual work               |
| Invoker           | RemoteControl executes command             |
| Client            | Creates and assigns commands               |

<br>

### ☑️ Solution Code

```csharp
using System;

// ================== COMMAND INTERFACE ==================
public interface ICommand
{
    void Execute();
    void Undo();
}

// ================== RECEIVER ==================
public class Light
{
    public void TurnOn()
    {
        Console.WriteLine("🔆 Light is ON");
    }

    public void TurnOff()
    {
        Console.WriteLine("🌑 Light is OFF");
    }
}

// ================== CONCRETE COMMANDS ==================
public class LightOnCommand : ICommand
{
    private Light _light;
    public LightOnCommand(Light light) => _light = light;

    public void Execute()
    {
        _light.TurnOn();
    }

    public void Undo()
    {
        _light.TurnOff();
    }
}

public class LightOffCommand : ICommand
{
    private Light _light;
    public LightOffCommand(Light light) => _light = light;

    public void Execute()
    {
        _light.TurnOff();
    }

    public void Undo()
    {
        _light.TurnOn();
    }
}

// ================== INVOKER ==================
public class RemoteControl
{
    private ICommand _command;

    public void SetCommand(ICommand command)
    {
        _command = command;
    }

    public void PressButton()
    {
        Console.WriteLine("▶ Executing Command...");
        _command.Execute();
    }

    public void PressUndo()
    {
        Console.WriteLine("⤺ Undoing Command...");
        _command.Undo();
    }
}

// ================== CLIENT TEST (MAIN PROGRAM) ==================
class Program
{
    static void Main()
    {
        Light roomLight = new Light();

        ICommand lightOn = new LightOnCommand(roomLight);
        ICommand lightOff = new LightOffCommand(roomLight);

        RemoteControl remote = new RemoteControl();

        // TURN ON
        remote.SetCommand(lightOn);
        remote.PressButton();

        // TURN OFF
        remote.SetCommand(lightOff);
        remote.PressButton();

        // UNDO LAST ACTION
        remote.PressUndo();

        Console.ReadLine();
    }
}
```

Output :
```
▶ Executing Command...
🔆 Light is ON
▶ Executing Command...
🌑 Light is OFF
⤺ Undoing Command...
🔆 Light is ON
```

### 🟢 Example 2 :

You have a Fan in your smart home. You want to Turn ON/OFF the fan and also set speed.
Without Command Pattern, Remote directly talks to Fan → tightly coupled.

### 1️⃣ Bad Approach (No Command Pattern)
```c#
using System;

class Fan
{
    public void TurnOn() => Console.WriteLine("Fan is ON");
    public void TurnOff() => Console.WriteLine("Fan is OFF");
    public void SetSpeed(int speed) => Console.WriteLine($"Fan speed set to {speed}");
}

class RemoteControl
{
    private Fan fan = new Fan();

    public void PressButton(string action)
    {
        if (action == "ON") fan.TurnOn();
        else if (action == "OFF") fan.TurnOff();
        else if (action.StartsWith("SPEED"))
        {
            int speed = int.Parse(action.Split()[1]);
            fan.SetSpeed(speed);
        }
    }
}

class Program
{
    static void Main()
    {
        RemoteControl remote = new RemoteControl();
        remote.PressButton("ON");
        remote.PressButton("SPEED 3");
        remote.PressButton("OFF");
    }
}
```

### ⚠ Problems:

RemoteControl depends directly on Fan

Adding new devices (AC, Light) requires modifying RemoteControl

Undo/Redo impossible

### 2️⃣ Solution Using Command Pattern
```c#
/// Step 1: Command Interface
public interface ICommand
{
    void Execute();
    void Undo();
}

//// Step 2: Receiver (Fan)
public class Fan
{
    public void TurnOn() => Console.WriteLine("Fan is ON");
    public void TurnOff() => Console.WriteLine("Fan is OFF");
    public void SetSpeed(int speed) => Console.WriteLine($"Fan speed set to {speed}");
}

/// /Step 3: Concrete Commands
public class FanOnCommand : ICommand
{
    private Fan _fan;
    public FanOnCommand(Fan fan) => _fan = fan;
    public void Execute() => _fan.TurnOn();
    public void Undo() => _fan.TurnOff();
}

public class FanOffCommand : ICommand
{
    private Fan _fan;
    public FanOffCommand(Fan fan) => _fan = fan;
    public void Execute() => _fan.TurnOff();
    public void Undo() => _fan.TurnOn();
}

public class FanSpeedCommand : ICommand
{
    private Fan _fan;
    private int _speed;
    private int _previousSpeed;

    public FanSpeedCommand(Fan fan, int speed)
    {
        _fan = fan;
        _speed = speed;
        _previousSpeed = 0; // initial previous speed
    }

    public void Execute()
    {
        _previousSpeed = _speed; // save last speed
        _fan.SetSpeed(_speed);
    }

    public void Undo()
    {
        _fan.SetSpeed(_previousSpeed); // simple undo (reset to last)
    }
}

////Step 4: Invoker (RemoteControl)
public class RemoteControl
{
    private ICommand _command;

    public void SetCommand(ICommand command) => _command = command;

    public void PressButton() => _command.Execute();
    public void PressUndo() => _command.Undo();
}

/// Step 5: Client (Main Program)
class Program
{
    static void Main()
    {
        Fan fan = new Fan();

        ICommand fanOn = new FanOnCommand(fan);
        ICommand fanOff = new FanOffCommand(fan);
        ICommand fanSpeed3 = new FanSpeedCommand(fan, 3);

        RemoteControl remote = new RemoteControl();

        remote.SetCommand(fanOn);
        remote.PressButton();

        remote.SetCommand(fanSpeed3);
        remote.PressButton();

        remote.SetCommand(fanOff);
        remote.PressButton();

        remote.PressUndo(); // undo last action (Fan ON again)
    }
}
```

✅ Console Output
```
Fan is ON
Fan speed set to 3
Fan is OFF
Fan is ON
```