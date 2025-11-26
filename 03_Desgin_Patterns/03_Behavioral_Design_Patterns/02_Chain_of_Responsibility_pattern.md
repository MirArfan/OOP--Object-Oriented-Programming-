# 🚀 Chain of Responsibility (CoR) Design Pattern

### 📌 Definition

 
Chain of Responsibility is a behavioral design pattern where a request is passed along a chain of handlers. Each handler either processes the request or passes it to the next handler in the chain.
 
>Chain of Responsibility হলো একটি behavioral design pattern, যেখানে একটি request একাধিক handler এর মধ্যে ধাপে ধাপে পাঠানো হয়। প্রতিটি handler চাইলে request process করে, নচেৎ পরবর্তী handler এ পাঠায়।

<br>

### 🎯 Purpose / Goal

| English |  |
|---------|--------|
| Decouple sender & receiver | Request পাঠানো ও প্রোসেসিং করা ক্লাস আলাদা রাখা |
| Dynamic handling | Run-time এ decide করতে পারা কে handle করবে |
| Avoid tightly coupled logic | বিভিন্ন request handler একই logic এ না বাঁধা |

<br>

### ❗ Why Needed


Without CoR, sender needs to know exactly which handler will process the request → tight coupling.  
With CoR, request moves along the chain → any handler can process → flexible & maintainable.

 
CoR ছাড়া sender কে ঠিক জানতে হবে কোন handler request process করবে → tightly coupled হয়।  
CoR এ request chain দিয়ে যায় → যেকোনো handler process করতে পারে → flexible ও maintainable হয়।

<br>

### 🏢 Real-Life Examples

|  Example | |
|----------------|----------------|
| Tech support ticket routing | টিকিটকে সাপোর্ট level 1 → level 2 → level 3 পাঠানো |
| Email spam filters | Spam check → Virus check → Forwarding |
| Event handling in GUI | Mouse click → parent container → window handler |
| Middleware in web frameworks | HTTP request → authentication → logging → routing |

<br>

### ⚙️ How It Works (Step-by-Step)

1. Sender sends request → Request object তৈরি করে।  
2. First handler checks → যদি handle করতে পারে → process করে।  
3. Otherwise passes to next handler → Chain এ linked handler এ চলে যায়।  
4. Request either handled or reaches end → End handler decide করে।

<br>

### 🏗 Structure

| Component       |  |  |
|-----------------|---------|--------|
| Handler         | Interface with SetNext() & Handle() | Handler interface যা next handler set ও request process করে |
| ConcreteHandler | Implements handle logic | প্রকৃত কাজ করে, next handler কে পাঠায় |
| Client          | Creates handlers & chain | Chain তৈরি করে request পাঠায় |

<br>

### 🔼 Advantages

|  |  |
|---------|--------|
| Loose coupling | Handler chain independent of sender |
| Flexible chain order | Chain order runtime এ change করা যায় |
| Responsibility can be shared | Multiple handlers handle different requests |



### 🔽 Disadvantages

|  |  |
|---------|--------|
| Might pass request unnecessarily | অনেক handler skip হতে পারে before processing |
| Debugging harder | Chain বড় হলে trace করা কঠিন |
| Can become complex | Long chains maintain করা কঠিন |


### 📌 Use Cases of Chain of Responsibility Pattern

| Use Case                  |  |
|-----------------------------------|----------------|
| Tech Support Ticket Routing        | Support ticket প্রথমে level 1 agent → level 2 supervisor → level 3 manager → চেইনের মাধ্যমে process হয়। |
| Event Handling in GUI              | Mouse click, key press ইত্যাদি events parent container → window → application handler পর্যন্ত চলে যায়। |
| Email Filtering System             | Email → Spam check → Virus scan → Forwarding → চেইন অনুযায়ী process হয়। |
| Logging System                     | Logger chain → InfoLogger → DebugLogger → ErrorLogger। Log level অনুযায়ী handle হয়। |
| Middleware in Web Frameworks       | HTTP request → Authentication → Authorization → Logging → Routing। প্রতিটি middleware handler। |
| Approval Workflows                 | Document approval: Team lead → Manager → Director। Request chain অনুযায়ী process হয়। |
| Customer Service Requests          | Customer request → Agent → Supervisor → Specialized department। Chain এ responsibility ভাগ। |
| Pipeline Processing / Data Processing | Data passes through multiple stages: validation → transformation → persistence। Each stage = handler। |


<br>

### 🟢 Example 1 :

Scenario : Customer complaint handling system. A complaint can be handled by FrontDesk → Supervisor → Manager depending on severity level.



Severity Level:

- 1 → simple → FrontDesk

- 2 → medium → Supervisor

- 3 → high → Manager

### 1️⃣ Bad Approach (No CoR)
```c#
using System;

class ComplaintHandler
{
    public void HandleComplaint(int level, string complaint)
    {
        if (level == 1)
            Console.WriteLine($"FrontDesk handled complaint: {complaint}");
        else if (level == 2)
            Console.WriteLine($"Supervisor handled complaint: {complaint}");
        else if (level == 3)
            Console.WriteLine($"Manager handled complaint: {complaint}");
    }
}

class Program
{
    static void Main()
    {
        ComplaintHandler handler = new ComplaintHandler();

        handler.HandleComplaint(1, "Late delivery");
        handler.HandleComplaint(2, "Damaged product");
        handler.HandleComplaint(3, "Billing issue");
    }
}
```

### ⚠ Problems:

- Single class handles everything → tightly coupled

- Hard to extend → new handler requires modifying this class

- No flexibility → cannot dynamically change chain

### 2️⃣ Correct Approach Using Chain of Responsibility
```c#
/// Step 1: Handler Interface / Abstract Class
public abstract class Handler
{
    protected Handler nextHandler;
    public void SetNext(Handler next) => nextHandler = next;

    public abstract void HandleRequest(int level, string complaint);
}

/// Step 2: Concrete Handlers
public class FrontDesk : Handler
{
    public override void HandleRequest(int level, string complaint)
    {
        if (level == 1)
            Console.WriteLine($"FrontDesk handled complaint: {complaint}");
        else if (nextHandler != null)
            nextHandler.HandleRequest(level, complaint);
    }
}

public class Supervisor : Handler
{
    public override void HandleRequest(int level, string complaint)
    {
        if (level == 2)
            Console.WriteLine($"Supervisor handled complaint: {complaint}");
        else if (nextHandler != null)
            nextHandler.HandleRequest(level, complaint);
    }
}

public class Manager : Handler
{
    public override void HandleRequest(int level, string complaint)
    {
        if (level >= 3)
            Console.WriteLine($"Manager handled complaint: {complaint}");
        else if (nextHandler != null)
            nextHandler.HandleRequest(level, complaint);
    }
}

/// Step 3: Client (Main Program)
class Program
{
    static void Main()
    {
        Handler frontDesk = new FrontDesk();
        Handler supervisor = new Supervisor();
        Handler manager = new Manager();

        // Create chain
        frontDesk.SetNext(supervisor);
        supervisor.SetNext(manager);

        // Test complaints
        frontDesk.HandleRequest(1, "Late delivery");      // handled by FrontDesk
        frontDesk.HandleRequest(2, "Damaged product");    // handled by Supervisor
        frontDesk.HandleRequest(3, "Billing issue");      // handled by Manager
    }
}
```

✅ Console Output
```yaml
FrontDesk handled complaint: Late delivery
Supervisor handled complaint: Damaged product
Manager handled complaint: Billing issue
```
<br>

### 🟢 Example 2 :

Document approval system. A document needs approval depending on level:

- Level 1 → Team Lead

- Level 2 → Manager

- Level 3 → Director



### 1️⃣ Bad Approach (No CoR)
```c#
using System;

class DocumentApproval
{
    public void Approve(int level, string document)
    {
        if (level == 1)
            Console.WriteLine($"Team Lead approved document: {document}");
        else if (level == 2)
            Console.WriteLine($"Manager approved document: {document}");
        else if (level == 3)
            Console.WriteLine($"Director approved document: {document}");
    }
}

class Program
{
    static void Main()
    {
        DocumentApproval approval = new DocumentApproval();

        approval.Approve(1, "Project Plan");
        approval.Approve(2, "Budget Report");
        approval.Approve(3, "Company Policy");
    }
}
```

### ⚠ Problems:

- Single class handles all approval → tightly coupled

- Hard to extend → adding VP or CEO requires modifying this class

- No flexibility → cannot dynamically reorder approval chain

### 2️⃣ Correct Approach Using Chain of Responsibility
```c#
/// Step 1: Handler Interface / Abstract Class
public abstract class Approver
{
    protected Approver nextApprover;
    public void SetNext(Approver next) => nextApprover = next;

    public abstract void ProcessRequest(int level, string document);
}

/// Step 2: Concrete Handlers
public class TeamLead : Approver
{
    public override void ProcessRequest(int level, string document)
    {
        if (level == 1)
            Console.WriteLine($"Team Lead approved document: {document}");
        else if (nextApprover != null)
            nextApprover.ProcessRequest(level, document);
    }
}

public class Manager : Approver
{
    public override void ProcessRequest(int level, string document)
    {
        if (level == 2)
            Console.WriteLine($"Manager approved document: {document}");
        else if (nextApprover != null)
            nextApprover.ProcessRequest(level, document);
    }
}

public class Director : Approver
{
    public override void ProcessRequest(int level, string document)
    {
        if (level >= 3)
            Console.WriteLine($"Director approved document: {document}");
        else if (nextApprover != null)
            nextApprover.ProcessRequest(level, document);
    }
}

/// Step 3: Client (Main Program)
class Program
{
    static void Main()
    {
        Approver teamLead = new TeamLead();
        Approver manager = new Manager();
        Approver director = new Director();

        // Create chain
        teamLead.SetNext(manager);
        manager.SetNext(director);

        // Test approval requests
        teamLead.ProcessRequest(1, "Project Plan");    // handled by Team Lead
        teamLead.ProcessRequest(2, "Budget Report");   // handled by Manager
        teamLead.ProcessRequest(3, "Company Policy");  // handled by Director
    }
}
```

### ✅ Console Output
```yaml
Team Lead approved document: Project Plan
Manager approved document: Budget Report
Director approved document: Company Policy
```
