# 🎭 Facade Design Pattern (Structural)

### 📘 Definition


The **Facade Pattern** is a structural design pattern that provides a **simple and unified interface** to a complex subsystem.

### 
**Facade Pattern** হলো এমন একটি pattern যা জটিল সিস্টেমের বিভিন্ন class এবং subsystem-এর কাজকে **একটি সহজ interface** এর মাধ্যমে ব্যবহার করতে সাহায্য করে।

<br>


### 🟩 2. Why Needed

- To hide complexity from the client  
- To reduce the number of dependent classes  
- To simplify large codebases  
- To make subsystem usage easy and readable  

<br>

### 🟩 3. Real-Life Example





### 1️⃣ Restaurant & Waiter

- **Client:** Customer  
- **Facade:** Waiter  
- **Subsystems:** Chef, Kitchen, Cashier  

Customer doesn’t interact with each subsystem.  
Just tells Waiter → food is served.



### 2️⃣ Home Theater System

- **Client:** User  
- **Facade:** HomeTheaterFacade  
- **Subsystems:** Projector, Sound System, DVD Player  

User calls `WatchMovie()` → internally all subsystems start.



### 3️⃣ Travel Booking System

- **Client:** Traveler  
- **Facade:** TravelFacade  
- **Subsystems:** FlightBooking, HotelBooking, CarRental, Payment  

Traveler calls `BookTrip()` → internally all bookings done.
 

### 4️⃣ Banking / ATM System
 
- **Client:** Bank Customer  
- **Facade:** ATM  
- **Subsystems:** BankAccount, CashDispenser, TransactionProcessor  

Customer inserts card & enters pin → ATM handles all internally.
 


### 5️⃣ Computer Startup Process

- **Client:** User  
- **Facade:** ComputerFacade  
- **Subsystems:** CPU, Memory, HardDrive, OS  

<br>

### 🟩 4. Goal / Purpose
 
- Provide a simple interface for a complex system  
- Reduce coupling between client and subsystem  ( Client ও subsystem-এর মাঝে dependency কমানো ) 
- Hide unnecessary implementation details  
- Improve readability and maintainability  

<br>

### 🟩 5. Use Cases

- Large systems with many classes  
- Home automation systems  
- Payment gateways  
- Video/audio players  
- Library management systems  
- When you need a single entry point  

<br>

### 🟩 6. How It Works
  
1. System has multiple subsystems  
2. Facade references those subsystems  
3. Facade exposes simple methods  ( Facade একটি বা একাধিক simple method দেয় )
4. Client calls only these simple methods  
5. Facade internally manages all subsystem operations  

➡️ Client → Facade → Subsystems

<br>

### 🟩 7. Advantages
  
- Simplifies complex systems  
- Reduces coupling  ( Client dependency কমায় )
- Improves readability  
- Easy to use for clients  
- Hides complexity  
- Makes code clean and organized  



### 🟩 8. Disadvantages

- Facade may become a “**God Object**”  ( Facade বেশি কাজ করলে “God Object” হয়ে যেতে পারে )
- Too much hiding reduces flexibility  
- Overuse can lead to poor design  


<br>


### 🟩 Travel Booking System 



ধরো তুমি একটি Travel Booking System বানাচ্ছো।  
Traveler wants to book a complete trip, which includes:

- Flight Booking  
- Hotel Booking  
- Car Rental  
- Payment  

### Problem:
- Client code প্রত্যেক subsystem call করতে হবে  
- অনেক repetitive steps, confusing and error-prone  
- Optional parts বা sequence ঠিক না থাকলে bug হয়  

### Goal:
- Facade দিয়ে একটিমাত্র method call করেই সব subsystem handle হবে

### ❌ Bad Approach – Without Facade
```c#
using System;

public class FlightBooking
{
    public void BookFlight(string from, string to) => Console.WriteLine($"Flight booked from {from} to {to}");
}

public class HotelBooking
{
    public void BookHotel(string hotelName) => Console.WriteLine($"Hotel booked: {hotelName}");
}

public class CarRental
{
    public void RentCar(string carType) => Console.WriteLine($"Car rented: {carType}");
}

public class Payment
{
    public void MakePayment(double amount) => Console.WriteLine($"Payment of ${amount} completed");
}

// Client Code
class Program
{
    static void Main()
    {
        var flight = new FlightBooking();
        flight.BookFlight("Dhaka", "New York");

        var hotel = new HotelBooking();
        hotel.BookHotel("Hilton");

        var car = new CarRental();
        car.RentCar("SUV");

        var payment = new Payment();
        payment.MakePayment(1200);

        Console.WriteLine("Trip Booked Successfully!");
    }
}
```

### ✅ Output
```
Flight booked from Dhaka to New York
Hotel booked: Hilton
Car rented: SUV
Payment of $1200 completed
Trip Booked Successfully!
```


### ❗ Problems

- Client has to call each subsystem manually

- Easy to forget a step

- Sequence of booking may go wrong

- Hard to maintain / extend

### ✅ Facade Pattern Solution
```c#
// Subsystems
public class FlightBooking
{
    public void BookFlight(string from, string to) 
    {
        Console.WriteLine($"Flight booked from {from} to {to}");
    }
}

public class HotelBooking
{
    public void BookHotel(string hotelName)
    {
        Console.WriteLine($"Hotel booked: {hotelName}");
    }
}

public class CarRental
{
    public void RentCar(string carType) => Console.WriteLine($"Car rented: {carType}");
}

public class Payment
{
    public void MakePayment(double amount) => Console.WriteLine($"Payment of ${amount} completed");
}

// Facade
public class TravelFacade
{
    private FlightBooking flight = new FlightBooking();
    private HotelBooking hotel = new HotelBooking();
    private CarRental car = new CarRental();
    private Payment payment = new Payment();

    public void BookCompleteTrip(string from, string to, string hotelName, string carType, double amount)
    {
        flight.BookFlight(from, to);
        hotel.BookHotel(hotelName);
        car.RentCar(carType);
        payment.MakePayment(amount);
        Console.WriteLine("Trip Booked Successfully via Facade!");
    }
}

// Client Code
class Program
{
    static void Main()
    {
        var travel = new TravelFacade();
        travel.BookCompleteTrip("Dhaka", "New York", "Hilton", "SUV", 1200);
    }
}
```

### ✅ Output
```
Flight booked from Dhaka to New York
Hotel booked: Hilton
Car rented: SUV
Payment of $1200 completed
Trip Booked Successfully via Facade!
```

<br>

### 🔑 Explanation

1. Client এখন শুধু **TravelFacade** call করে

2. Facade internally সব subsystem handle করে

3. No need to worry about sequence or missing steps

4. Code becomes **clean, readable, and maintainable**

<br>

### 🟩 Home Theater System – Facade Example 2



ধরো তুমি একটি Home Theater System চালাতে চাও।  
System-এর ভিতরে অনেক subsystem আছে:

- Projector  
- Sound System  
- DVD Player  

### Problem:
- User-কে প্রতিটা subsystem manually চালাতে হবে  
- অনেক repetitive steps, error-prone  
- Sequence ঠিক না থাকলে Movie শুরু হবে না  

### Goal:
- Facade দিয়ে একটিমাত্র method call করেই সব subsystem চালু হবে 


```c#
using System;

public class Projector
{
    public void On() => Console.WriteLine("Projector On");
}

public class SoundSystem
{
    public void TurnOn() => Console.WriteLine("Sound System On");
    public void SetVolume(int level) => Console.WriteLine($"Volume set to {level}");
}

public class DVDPlayer
{
    public void Play(string movie) => Console.WriteLine($"Playing {movie}");
}

// Client Code
class Program
{
    static void Main()
    {
        var projector = new Projector();
        projector.On();

        var sound = new SoundSystem();
        sound.TurnOn();
        sound.SetVolume(50);

        var dvd = new DVDPlayer();
        dvd.Play("Avengers.mp4");

        Console.WriteLine("Movie Started!");
    }
}
```

### ✅ Output
```
Projector On
Sound System On
Volume set to 50
Playing Avengers.mp4
Movie Started!
```

### ❗ Problems
- User has to manually call each subsystem  
- Hard to remember the correct sequence  
- Error-prone and messy  
- Hard to maintain / extend

### ✅ Facade Pattern Solution
```c#
// Subsystems
public class Projector
{
    public void On() => Console.WriteLine("Projector On");
}

public class SoundSystem
{
    public void TurnOn() => Console.WriteLine("Sound System On");
    public void SetVolume(int level) => Console.WriteLine($"Volume set to {level}");
}

public class DVDPlayer
{
    public void Play(string movie) => Console.WriteLine($"Playing {movie}");
}

// Facade
public class HomeTheaterFacade
{
    private Projector projector = new Projector();
    private SoundSystem sound = new SoundSystem();
    private DVDPlayer dvd = new DVDPlayer();

    public void WatchMovie(string movie)
    {
        projector.On();
        sound.TurnOn();
        sound.SetVolume(50);
        dvd.Play(movie);
        Console.WriteLine("Movie Started via Facade!");
    }
}

// Client Code
class Program
{
    static void Main()
    {
        var theater = new HomeTheaterFacade();
        theater.WatchMovie("Avengers.mp4");
    }
}
```

### ✅ Output
```
Projector On
Sound System On
Volume set to 50
Playing Avengers.mp4
Movie Started via Facade!
```

### 🔑 Explanation

- Client শুধুমাত্র `HomeTheaterFacade` call করে  
- Facade internally সব subsystem manage করে  
- Sequence, volume, startup order — সব handled  
- Code becomes clean, readable, and maintainable
