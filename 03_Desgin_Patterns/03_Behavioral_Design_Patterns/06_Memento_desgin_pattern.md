# ⭐ Memento Design Pattern — Definition

### 📖 Definition



The **Memento Design Pattern** is a behavioral design pattern that allows you to **capture and store an object’s internal state** so that it can be **restored later**, **without exposing the object’s internal implementation details**.  
It is commonly used to implement **Undo / Redo** functionality while preserving **encapsulation**.



>**Memento Pattern** ব্যবহার করা হয় কোনো object-এর আগের অবস্থা (state) **সংরক্ষণ করে রাখার জন্য**, যাতে পরে প্রয়োজনে **undo / restore** করা যায় —  
কিন্তু object-এর ভেতরের data বা internal structure **বাইরে expose না করেই**।

<br>

### 🔹 Real-Life Examples

- **Undo / Redo** (Text Editor)
- **Game Save / Load**
- **Form Data Rollback**
- **Browser Back / Forward**
- **Version History** (Google Docs, Git)
- Configuration Snapshot  
-  Transaction Rollback  

<br>

### 🔹 Use Cases 


✔ When **Undo / Rollback** is required  
✔ When an object’s **state needs to be saved and restored**  
✔ When you want to **protect encapsulation**  
✔ When state history must be managed externally  

<br>

### 🔹 Why Memento Design Pattern is Needed?

- Sometimes it is necessary to store the previous state of an object  
- Needed to implement **Undo / Rollback** functionality  
- State should be saved **without exposing internal data** of the object  
- Direct copying of object state can break **Encapsulation**  

👉 Therefore, the **Memento Design Pattern** is used to safely save and restore an object’s state.

<br>

### 🎯 Goal and Purpose

### 🎯 Goal

- Save the state of an object  
- Restore the state when required  

### 🎯 Purpose

- Implement **Undo / Redo** functionality  
- Preserve **Encapsulation** of the object  
- Keep state management **clean and secure**  
- Decouple client code from the object’s internal structure  

<br>





### 🔹 Main 3 Components

| Role | Name | 
|------|------|
| 1️⃣ | Originator | 
| 2️⃣ | Memento | 
| 3️⃣ | Caretaker | 

<br>

### 🔹 How It Works 

1. Originator তার state change করে  
2. Caretaker বর্তমান state একটি Memento হিসেবে save করে  
3. Undo করলে Caretaker থেকে memento ফেরত আসে  
4. Originator আগের state restore করে  




<br>

### 🔹 Advantages of Memento Design Pattern

- ✅ **Maintains Encapsulation**  
  - The internal state of an object is not exposed to the outside world  

- ✅ **Separation of Concerns**  
  - State management (Caretaker) is separated from business logic (Originator)  

- ✅ **Supports Undo / Redo**  
  - Previous states can be restored easily  

- ✅ **Safe State Management**  
  - The object itself decides what should be saved and restored  

<br>

### 🔹 Disadvantages of Memento Design Pattern

- ❌ **High Memory Usage**  
  - Saving every state can consume a large amount of memory  

- ❌ **Performance Degradation**  
  - Frequent save and restore operations may reduce performance  

- ❌ **Increased Complexity**  
  - Managing large or complex states becomes difficult  

- ❌ **Too Many Mementos**  
  - Maintaining a long history of mementos can make the code harder to manage  


<br>
<br>

### 📝 Example 1 : Text Editor (Undo Feature)

We are building a simple **Text Editor** where:

- The user can write text  
- Pressing **Undo** will restore the previous text  



### ❌ Wrong Approach (Without Memento Pattern)


```csharp
using System;

class TextEditor
{
    public string Text;

    public void Write(string text)
    {
        Text = text;
    }
}

class Program
{
    static void Main()
    {
        TextEditor editor = new TextEditor();

        editor.Write("Hello");
        editor.Write("Hello World");

        // Undo manually
        editor.Text = "Hello"; // manually set ❌

        Console.WriteLine(editor.Text);
    }
}
```
Problems with This Approach

- ❌ Previous state is not automatically saved

- ❌ Multiple Undo is not possible

- ❌ Internal state is directly modified

- ❌ Encapsulation is broken

- ❌ Difficult to maintain in large applications

👉 This is where the Memento Pattern comes in.

### ✅ Solution: Using Memento Design Pattern


```c#
using System;
using System.Collections.Generic;

// Step 1: Memento (State Holder)
class TextMemento
{
    public string Text { get; }

    public TextMemento(string text)
    {
        Text = text;
    }
}

// Step 2: Originator (Text Editor)
class TextEditor
{
    private string _text;

    public void Write(string text)
    {
        _text = text;
    }

    public string Read()
    {
        return _text;
    }

    public TextMemento Save()
    {
        return new TextMemento(_text);
    }

    public void Restore(TextMemento memento)
    {
        _text = memento.Text;
    }
}

// Step 3: Caretaker (History Manager)
class History
{
    private Stack<TextMemento> _history = new Stack<TextMemento>();

    public void Save(TextMemento memento)
    {
        _history.Push(memento);
    }

    public TextMemento Undo()
    {
        return _history.Pop();
    }
}

class Program
{
    static void Main()
    {
        TextEditor editor = new TextEditor();
        History history = new History();

        editor.Write("Hello");
        history.Save(editor.Save());

        editor.Write("Hello World");
        history.Save(editor.Save());

        editor.Write("Hello World !!!");

        Console.WriteLine("Current Text: " + editor.Read());

        editor.Restore(history.Undo());
        Console.WriteLine("After 1st Undo: " + editor.Read());

        editor.Restore(history.Undo());
        Console.WriteLine("After 2nd Undo: " + editor.Read());
    }
}
```

### 🖥 Sample Output
```yaml
Current Text: Hello World !!!
After 1st Undo: Hello World
After 2nd Undo: Hello
```

<br>

### 🏦 Example 2 : Bank Account (Undo Balance Update)

We have a **Bank Account** with a balance.  

- We update the balance (deposit/withdraw)  
- We want to **undo** and restore the previous balance if needed  

### ✅ Solution: Using Memento Pattern

```c#
using System;
using System.Collections.Generic;

// 🧱 Step 1: Memento (State Holder)
class AccountMemento
{
    public decimal Balance { get; }

    public AccountMemento(decimal balance)
    {
        Balance = balance;
    }
}

// 🧱 Step 2: Originator (Bank Account)
class BankAccount
{
    private decimal _balance;

    public void Deposit(decimal amount)
    {
        _balance += amount;
    }

    public decimal GetBalance()
    {
        return _balance;
    }

    public AccountMemento Save()
    {
        return new AccountMemento(_balance);
    }

    public void Restore(AccountMemento memento)
    {
        _balance = memento.Balance;
    }
}

// 🧱 Step 3: Caretaker (History)

class History
{
    private Stack<AccountMemento> _history = new Stack<AccountMemento>();

    public void Save(AccountMemento memento)
    {
        _history.Push(memento);
    }

    public AccountMemento Undo()
    {
        return _history.Pop();
    }
}

class Program
{
    static void Main()
    {
        BankAccount account = new BankAccount();
        History history = new History();

        account.Deposit(100);
        history.Save(account.Save());

        account.Deposit(50);
        history.Save(account.Save());

        account.Deposit(200);
        Console.WriteLine("Current Balance: " + account.GetBalance());

        account.Restore(history.Undo());
        Console.WriteLine("After 1st Undo: " + account.GetBalance());

        account.Restore(history.Undo());
        Console.WriteLine("After 2nd Undo: " + account.GetBalance());
    }
}
```

### 🖥 Sample Output

```yaml
Current Balance: 350
After 1st Undo: 150
After 2nd Undo: 100
```
