# ⭐ Mediator Design Pattern


### ✅ 1. Definition 


The Mediator Design Pattern defines an object that centralizes communication between multiple objects so they don’t communicate with each other directly. Instead, they communicate through a mediator.  
It reduces the dependency and complexity among interacting objects.


>Mediator Design Pattern এমন একটি pattern যেখানে একাধিক object একে অপরের সাথে সরাসরি যোগাযোগ না করে, বরং একটি mediator (মধ্যস্থ) object-এর মাধ্যমে যোগাযোগ করে।  
এর ফলে object গুলোর মধ্যে dependency এবং জটিলতা কমে।

<br>

### ✅ 2. Why Needed 


- When many objects talk to each other directly, the system becomes complex and tightly coupled.  
- Mediator helps reduce direct dependencies.  
- It centralizes communication logic, making systems easier to modify and maintain.

<br>

### ✅ 3. Real World Examples 


- **Air Traffic Control (ATC):**  
  Airplanes don’t talk to each other → they communicate via ATC (mediator).
- **Chat Room:**  
  Users don’t message each other directly → messages go through the chat server.
- **Office Communication via HR:**  
  Employees communicate through HR for specific tasks.

<br>

### ✅ 4. Goal and Purpose 


- Reduce direct communication between objects  
- Simplify communication logic  
- Make the system loosely coupled and easier to modify  

<br>

### ✅ 5. Use Cases 


- Chat applications  
- Air traffic communication systems  
- UI dialog box interactions  
- Event handling systems  
- Component-based architecture (React, Angular internals)

 

<br>

### ✅ 6. How It Works

1. Objects (Colleagues) don’t talk to each other directly  
2. They send messages/events to a **Mediator**  
3. Mediator decides what action to take  
4. Mediator forwards message to the appropriate colleague  

<br>

### ✅ 7. Advantages 


- Reduces communication complexity  
- Eliminates tight coupling  
- Easy to modify or extend behavior  
- Centralized interaction control  
- Cleaner, more organized code  
- SRP maintain



### ✅ 8. Disadvantages 


- Mediator can grow too large (God Object problem)  
- Too much logic in mediator reduces clarity  
- Hard to maintain if poorly designed 
- Mediator e problem hole full system e porblem hoi

<br>
<br>

### 📌 Example 1 : Simple Chat Room

We have multiple users who want to communicate with each other.




### ❌ Bad Example (Without Mediator — Wrong Approach)

### **Problems**

- **Every user needs a reference to all other users**  
  Each user must store a list of all other users to send messages.

- **Too many direct dependencies**  
  If 10 users exist, each user depends on 9 others → tight coupling.

- **Adding a new user means updating all users**  
  You must update every user's reference list — not scalable.

- **Communication logic scattered everywhere**  
  Hard to maintain, hard to debug, code becomes messy and complex.


```c#
using System;
using System.Collections.Generic;

class User
{
    public string Name { get; set; }
    public List<User> Contacts = new List<User>();

    public User(string name)
    {
        Name = name;
    }

    // Direct messaging (BAD)
    public void SendMessage(string message, User toUser)
    {
        Console.WriteLine($"{Name} to {toUser.Name}: {message}");
        toUser.ReceiveMessage(message, this);
    }

    public void ReceiveMessage(string message, User fromUser)
    {
        Console.WriteLine($"{Name} received from {fromUser.Name}: {message}");
    }
}

class Program
{
    static void Main()
    {
        User a = new User("Alice");
        User b = new User("Bob");
        User c = new User("Charlie");

        // Direct connections
        a.Contacts.Add(b);
        a.Contacts.Add(c);

        b.Contacts.Add(a);
        b.Contacts.Add(c);

        a.SendMessage("Hello Bob!", b);
        b.SendMessage("Hi Alice!", a);
    }
}
```
### ❌ Problems With This Bad Example


- Users depend on each other  
- More users = more connections = more complexity  
- Hard to manage the message flow  
- Code becomes tightly coupled  
- Difficult to extend or test  



### ✅ Correct Example (Using Mediator Pattern)

### **Solution Approach**
- Create an **`IChatMediator`** interface  
- Create a **`ChatMediator`** class to control message passing  
- Users communicate **only through the mediator**  
- Mediator decides **who receives the message**  
- No direct dependency among users  



### 🟢  MEDIATOR EXAMPLE
```c#
using System;
using System.Collections.Generic;

// ======================================
// MEDIATOR INTERFACE
// ======================================
public interface IChatMediator
{
    void SendMessage(string message, User user);
    void AddUser(User user);
}

// ======================================
// CONCRETE MEDIATOR
// ======================================
public class ChatMediator : IChatMediator
{
    private List<User> _users = new List<User>();

    public void AddUser(User user)
    {
        _users.Add(user);
    }

    public void SendMessage(string message, User sender)
    {
        foreach (var user in _users)
        {
            if (user != sender)
            {
                user.Receive(message, sender);
            }
        }
    }
}

// ======================================
// USER (Colleague)
// ======================================
public abstract class User
{
    protected IChatMediator mediator;
    public string Name { get; set; }

    public User(IChatMediator mediator, string name)
    {
        this.mediator = mediator;
        Name = name;
    }

    public abstract void Send(string message);
    public abstract void Receive(string message, User sender);
}

// ======================================
// CONCRETE USER
// ======================================
public class ChatUser : User
{
    public ChatUser(IChatMediator med, string name) : base(med, name) { }

    public override void Send(string message)
    {
        Console.WriteLine($"{Name} sends: {message}");
        mediator.SendMessage(message, this);
    }

    public override void Receive(string message, User sender)
    {
        Console.WriteLine($"{Name} receives from {sender.Name}: {message}");
    }
}

// ======================================
// CLIENT
// ======================================
class Program
{
    static void Main()
    {
        IChatMediator chatRoom = new ChatMediator();

        User alice = new ChatUser(chatRoom, "Alice");
        User bob = new ChatUser(chatRoom, "Bob");
        User charlie = new ChatUser(chatRoom, "Charlie");

        chatRoom.AddUser(alice);
        chatRoom.AddUser(bob);
        chatRoom.AddUser(charlie);

        alice.Send("Hello everyone!");
        bob.Send("Hi Alice!");
    }
}
```

### 🎯 OUTPUT
```yaml
Alice sends: Hello everyone!
Bob receives from Alice: Hello everyone!
Charlie receives from Alice: Hello everyone!

Bob sends: Hi Alice!
Alice receives from Bob: Hi Alice!
Charlie receives from Bob: Hi Alice!
```




<br>

### ✅ Example 2 : Smart Home Device Communication

Imagine you have several smart home devices:

- **Smart Light**  
- **Smart Fan**  
- **Smart Door**  
- **Smart AC**  

#### Problem Without Mediator
- All devices communicate directly with each other  
- System becomes **tightly coupled**  
- Hard to add new devices or modify behavior  
- Communication logic is **scattered** and messy  

#### **Mediator Solution**
- Introduce a **central `SmartHomeHub`** as a mediator  
- Devices communicate **only through the hub**  
- Hub decides **who receives the message and what action to take**  
- Adds **loose coupling**, maintainability, and scalability  

<br>

✅ Correct Example (Using Mediator)

All smart devices communicate through a SmartHomeHub (Mediator).

Each device only sends events → the hub decides what to do.

### ✅ Example — Mediator Pattern
```c#
using System;

interface ISmartHomeMediator
{
    void Notify(object sender, string ev);
}

class SmartHomeHub : ISmartHomeMediator
{
    private SmartLight _light;
    private SmartDoor _door;
    private SmartFan _fan;

    public void RegisterLight(SmartLight light) => _light = light;
    public void RegisterDoor(SmartDoor door) => _door = door;
    public void RegisterFan(SmartFan fan) => _fan = fan;

    public void Notify(object sender, string ev)
    {
        if (ev == "DoorOpened")
        {
            Console.WriteLine("[Hub] Door opened event received. Taking actions...");
            _light.TurnOn();
            _fan.TurnOn();
        }
        else if (ev == "DoorClosed")
        {
            Console.WriteLine("[Hub] Door closed event received. Turning fan off...");
            _fan.TurnOff();
        }
    }
}

class SmartLight
{
    public void TurnOn() => Console.WriteLine("Light is ON");
}

class SmartFan
{
    public void TurnOn() => Console.WriteLine("Fan is ON");
    public void TurnOff() => Console.WriteLine("Fan is OFF");
}

class SmartDoor
{
    private ISmartHomeMediator _mediator;

    public SmartDoor(ISmartHomeMediator mediator)
    {
        _mediator = mediator;
    }

    public void Open()
    {
        Console.WriteLine("Door is OPEN");
        _mediator.Notify(this, "DoorOpened");
    }

    public void Close()
    {
        Console.WriteLine("Door is CLOSED");
        _mediator.Notify(this, "DoorClosed");
    }
}

class Program
{
    static void Main()
    {
        SmartHomeHub hub = new SmartHomeHub();

        SmartLight light = new SmartLight();
        SmartFan fan = new SmartFan();
        SmartDoor door = new SmartDoor(hub);

        hub.RegisterLight(light);
        hub.RegisterDoor(door);
        hub.RegisterFan(fan);

        door.Open();
        Console.WriteLine();
        door.Close();
    }
}
```

### ✅ Output
```yaml
Door is OPEN
[Hub] Door opened event received. Taking actions...
Light is ON
Fan is ON

Door is CLOSED
[Hub] Door closed event received. Turning fan off...
Fan is OFF
```

