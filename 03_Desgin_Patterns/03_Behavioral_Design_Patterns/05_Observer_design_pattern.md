# ⭐ Observer Design Pattern

### 1. Definition 


The Observer Design Pattern defines a one-to-many dependency between objects so that when one object (called Subject) changes its state, all its dependent objects (Observers) are notified and updated automatically.

 
>Observer Design Pattern হল এমন একটি pattern যেখানে এক object (Subject) এর state পরিবর্তিত হলে তার সব dependent object (Observers) স্বয়ংক্রিয়ভাবে update হয়।

<br>

### 2. Why Needed 


- To allow multiple objects to stay updated with another object’s state.  
- Reduces tight coupling between objects.  
- Useful for real-time updates.

<br>

### 3. Real World Examples 

- **Social Media Notifications**: Followers get notified when someone posts a new update.  
- **Stock Market Apps**: Traders get notified when stock prices change.  
- **Weather Apps**: Multiple displays update when weather data changes.
- **Chat System**

 

<br>

### 4. Goal and Purpose 
- Establish one-to-many relationships between objects.  
- Ensure automatic updates without tight coupling.  
- Promote loose coupling and modular design.

<br>

### 5. Use Cases 


- Event-driven systems (UI event handling)  
- Messaging or notification systems  
- Stock trading platforms  
- Logging and monitoring systems  
- Real-time dashboards  


<br>

### 6. How it Works 

- Subject maintains a list of observers.  
- Observers register themselves with the subject.  
- When subject’s state changes, it calls Notify() to update all observers.  
- Observers react to the update independently.  

<br>

### 7. Advantages


- Loose coupling between subject and observers  
- Supports broadcast communication  
- Observers can be added/removed dynamically  
- Easy to maintain and extend  



### 8. Disadvantages 
- Too many observers → performance overhead  
- Hard to predict update order  
- Can lead to cascading updates if observers update other subjects  


<br>
<br>

### 📌 Example 1 : Stock Market Updates

- **Stock (Subject):** Tracks the price of a stock.  
- **Traders (Observers):** Want to get notified when the stock price changes.  

**Without Observer Pattern:**  
- Traders directly check the stock → tight coupling.  

**With Observer Pattern:**  
- Traders subscribe to stock → automatically notified when price changes.  

### ❌ Bad Example (Without Observer — Tight Coupling)
```c#
using System;

class Stock
{
    public string Symbol { get; set; }
    public double Price { get; set; }

    public Stock(string symbol, double price)
    {
        Symbol = symbol;
        Price = price;
    }

    // Directly notify trader inside stock (BAD)
    public void UpdatePrice(double newPrice, Trader trader)
    {
        Price = newPrice;
        trader.Notify(this);
    }
}

class Trader
{
    public string Name { get; set; }
    public Trader(string name)
    {
        Name = name;
    }

    public void Notify(Stock stock)
    {
        Console.WriteLine($"{Name} notified: {stock.Symbol} price is now {stock.Price}");
    }
}

class Program
{
    static void Main()
    {
        Stock apple = new Stock("AAPL", 150);
        Trader trader1 = new Trader("Alice");
        Trader trader2 = new Trader("Bob");

        // Stock directly notifies trader → tight coupling
        apple.UpdatePrice(155, trader1);
        apple.UpdatePrice(160, trader2);
    }
}
```

❌ Problems:

- Stock must know about all traders → tight coupling  
- Adding/removing traders requires modifying Stock  
- Not scalable  

✅ Correct Example (Using Observer Pattern)

**Solution Approach:**

- `IObserver` interface → all traders implement  
- `ISubject` interface → stock implements  
- Stock maintains a list of observers (traders)  
- Traders register themselves → automatically notified  


### 🟢 Observer Pattern Code
```c#
using System;
using System.Collections.Generic;

// =====================
// OBSERVER INTERFACE
// =====================
interface ITrader
{
    void Update(Stock stock);
}

// =====================
// SUBJECT INTERFACE
// =====================
interface IStock
{
    void RegisterTrader(ITrader trader);
    void RemoveTrader(ITrader trader);
    void NotifyTraders();
}

// =====================
// CONCRETE SUBJECT
// =====================
class Stock : IStock
{
    private List<ITrader> _traders = new List<ITrader>();
    private double _price;

    public string Symbol { get; private set; }

    public double Price
    {
        get { return _price; }
        set
        {
            _price = value;
            NotifyTraders();
        }
    }

    public Stock(string symbol, double price)
    {
        Symbol = symbol;
        _price = price;
    }

    public void RegisterTrader(ITrader trader)
    {
        _traders.Add(trader);
    }

    public void RemoveTrader(ITrader trader)
    {
        _traders.Remove(trader);
    }

    public void NotifyTraders()
    {
        foreach (var trader in _traders)
        {
            trader.Update(this);
        }
    }
}

// =====================
// CONCRETE OBSERVER
// =====================
class Trader : ITrader
{
    public string Name { get; private set; }
    public Trader(string name)
    {
        Name = name;
    }

    public void Update(Stock stock)
    {
        Console.WriteLine($"{Name} notified: {stock.Symbol} price is now {stock.Price}");
    }
}

// =====================
// CLIENT
// =====================
class Program
{
    static void Main()
    {
        Stock apple = new Stock("AAPL", 150);

        Trader alice = new Trader("Alice");
        Trader bob = new Trader("Bob");

        // Traders subscribe to stock updates
        apple.RegisterTrader(alice);
        apple.RegisterTrader(bob);

        // Stock price changes → all traders automatically notified
        apple.Price = 155;
        apple.Price = 160;
    }
}
```

### ✅ Output
```yaml
Alice notified: AAPL price is now 155
Bob notified: AAPL price is now 155
Alice notified: AAPL price is now 160
Bob notified: AAPL price is now 160
```


<br>

### 📌 Example 2 : Weather Station

We have:

- **WeatherData (Subject)** → tracks temperature, humidity, pressure  
- **Displays (Observers)** → multiple display devices want updates automatically  


### 🟢 Example 2 : Observer Pattern Code
```c#
using System;
using System.Collections.Generic;

// =====================
// OBSERVER INTERFACE
// =====================
interface IObserver
{
    void Update(float temperature, float humidity);
}

// =====================
// SUBJECT INTERFACE
// =====================
interface ISubject
{
    void RegisterObserver(IObserver observer);
    void RemoveObserver(IObserver observer);
    void NotifyObservers();
}

// =====================
// CONCRETE SUBJECT
// =====================
class WeatherData : ISubject
{
    private List<IObserver> _observers = new List<IObserver>();
    private float _temperature;
    private float _humidity;

    public float Temperature { get => _temperature; set { _temperature = value; NotifyObservers(); } }
    public float Humidity { get => _humidity; set { _humidity = value; NotifyObservers(); } }

    public void RegisterObserver(IObserver observer) => _observers.Add(observer);
    public void RemoveObserver(IObserver observer) => _observers.Remove(observer);

    public void NotifyObservers()
    {
        foreach (var observer in _observers)
        {
            observer.Update(_temperature, _humidity);
        }
    }
}

// =====================
// CONCRETE OBSERVER
// =====================
class CurrentConditionsDisplay : IObserver
{
    public void Update(float temperature, float humidity)
    {
        Console.WriteLine($"Current conditions: {temperature}C, {humidity}% humidity");
    }
}

class StatisticsDisplay : IObserver
{
    public void Update(float temperature, float humidity)
    {
        Console.WriteLine($"Statistics Display: Temp={temperature}C, Humidity={humidity}%");
    }
}

// =====================
// CLIENT
// =====================
class Program
{
    static void Main()
    {
        WeatherData weatherData = new WeatherData();

        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay();
        StatisticsDisplay statisticsDisplay = new StatisticsDisplay();

        // Observers subscribe to weather data
        weatherData.RegisterObserver(currentDisplay);
        weatherData.RegisterObserver(statisticsDisplay);

        // Weather updates
        weatherData.Temperature = 25.5f;
        weatherData.Humidity = 65f;

        Console.WriteLine();

        weatherData.Temperature = 27f;
        weatherData.Humidity = 70f;
    }
}
```

### ✅ Output
```yaml
Current conditions: 25.5C, 65% humidity
Statistics Display: Temp=25.5C, Humidity=65%
Current conditions: 27C, 70% humidity
Statistics Display: Temp=27C, Humidity=70%
```

