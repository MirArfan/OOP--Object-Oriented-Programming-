# 🧩 Strategy Design Pattern


### 1️⃣ Definition


Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable at runtime.


>Strategy Pattern এমন একটি pattern যেখানে একই কাজের একাধিক algorithm / behavior থাকে,  
এবং runtime-এ প্রয়োজন অনুযায়ী যেকোনো একটাকে বেছে নেওয়া যায়।

<br>

### 2️⃣ Why Strategy Pattern is Needed?

- The same task can be performed using **multiple algorithms or behaviors**
- Often, `if-else` or `switch-case` is used to select behavior
- Adding a new algorithm usually requires modifying existing code  
  → ❌ Violates **Open/Closed Principle (OCP)**

👉 With the **Strategy Pattern**:
- Each behavior is placed in a separate class
- Behavior can be changed **at runtime**


<br>

### 3️⃣ Goal & Purpose


### 🎯 Goal

- Change algorithms or behavior **at runtime**

### 🎯 Purpose

- **Eliminate conditional logic** (`if-else`, `switch`)
- Keep code **clean, flexible, and extensible**
- Follow the **Open/Closed Principle** 

<br>

### 4️⃣ Use Cases

- **Payment System** (Bkash / Nagad / Card)  
- **Sorting Algorithm**(Quick / Merge / Bubble)  
- **Compression** (ZIP / RAR)  
- **Authentication** (Google / Facebook / Email)  

<br>

### 5️⃣ Real World Example

- **Google Maps Route** → Driving / Walking / Cycling  
- **Online Payment** → Bkash / Card / Cash  
- **Food Delivery Charge** → Inside Dhaka / Outside Dhaka  

<br>

### 6️⃣ How Strategy Pattern Works

### 🔹 Main Components

1️⃣ **Strategy Interface**  
→ Common behavior define করে  

2️⃣ **Concrete Strategies**  
→ আলাদা আলাদা algorithm implement করে  

3️⃣ **Context Class**  
→ Strategy ব্যবহার করে  
```css
Context → has a → Strategy → ConcreteStrategy
```
<br>

### 7️⃣ Advantages

- ✅ Eliminates `if-else` / `switch` statements
- ✅ **Easy to add** new strategies
- ✅ Behavior can be changed at runtime
- ✅ Follows the **Open/Closed Principle**



### 8️⃣ Disadvantages

- ❌ Number of strategy **classes can increase**
- ❌ Over-engineering for simple logic
- ❌ Client must choose the appropriate strategy 
- ❌ Client must **know the different type of strategy** pattern

<br>

### 💳 Example 1 : Payment System (Strategy Design Pattern)

An application allows users to make payments using different methods:

- Bkash  
- Nagad  
- Credit Card  

The payment logic changes based on the selected payment method.


### ❌ Wrong Approach (Without Strategy Pattern)

```csharp
using System;

class PaymentService
{
    public void Pay(string method, double amount)
    {
        if (method == "bkash")
        {
            Console.WriteLine("Paid " + amount + " using Bkash");
        }
        else if (method == "nagad")
        {
            Console.WriteLine("Paid " + amount + " using Nagad");
        }
        else if (method == "card")
        {
            Console.WriteLine("Paid " + amount + " using Credit Card");
        }
        else
        {
            Console.WriteLine("Invalid payment method");
        }
    }
}

class Program
{
    static void Main()
    {
        PaymentService service = new PaymentService();
        service.Pay("bkash", 500);
        service.Pay("card", 1000);
    }
}
```
### ❌ Problems with This Approach
- ❌ Too many if-else statements

- ❌ Adding a new payment method requires modifying existing code

- ❌ Violates Open/Closed Principle (OCP)

- ❌ Hard to maintain and extend

>👉 This is where the Strategy Pattern is needed.

### ✅ Solution: Using Strategy Design Pattern


```c# 
using System;

// 🧱 Step 1: Strategy Interface
interface IPaymentStrategy
{
    void Pay(double amount);
}

// 🧱 Step 2: Concrete Strategies
class BkashPayment : IPaymentStrategy
{
    public void Pay(double amount)
    {
        Console.WriteLine("Paid " + amount + " using Bkash");
    }
}

class NagadPayment : IPaymentStrategy
{
    public void Pay(double amount)
    {
        Console.WriteLine("Paid " + amount + " using Nagad");
    }
}

class CardPayment : IPaymentStrategy
{
    public void Pay(double amount)
    {
        Console.WriteLine("Paid " + amount + " using Credit Card");
    }
}

// 🧱 Step 3: Context Class
class PaymentContext
{
    private IPaymentStrategy _paymentStrategy;

    public PaymentContext(IPaymentStrategy paymentStrategy)
    {
        _paymentStrategy = paymentStrategy;
    }

    public void SetStrategy(IPaymentStrategy paymentStrategy)
    {
        _paymentStrategy = paymentStrategy;
    }

    public void Pay(double amount)
    {
        _paymentStrategy.Pay(amount);
    }
}

class Program
{
    static void Main()
    {
        PaymentContext payment = new PaymentContext(new BkashPayment());
        payment.Pay(500);

        payment.SetStrategy(new CardPayment());
        payment.Pay(1000);

        payment.SetStrategy(new NagadPayment());
        payment.Pay(700);
    }
}
```
### 🖥 Sample Output
```powershell
Paid 500 using Bkash
Paid 1000 using Credit Card
Paid 700 using Nagad
```


<br>
<br>


### 🛒 Example 2 : Discount Calculation System (Strategy Design Pattern)

An e-commerce system applies different discount rates based on user type:

- **Regular Customer** → 5%  
- **Premium Customer** → 10%  
- **VIP Customer** → 20%  

👉 The discount logic must be **changeable** and **extensible** without modifying existing code.


### ✅ Solution: Using Strategy Design Pattern


```csharp
using System;

// 🧱 Step 1: Strategy Interface
interface IDiscountStrategy
{
    double Calculate(double price);
}


// 🧱 Step 2: Concrete Strategies
class RegularDiscount : IDiscountStrategy
{
    public double Calculate(double price)
    {
        return price * 0.05;
    }
}

class PremiumDiscount : IDiscountStrategy
{
    public double Calculate(double price)
    {
        return price * 0.10;
    }
}

class VIPDiscount : IDiscountStrategy
{
    public double Calculate(double price)
    {
        return price * 0.20;
    }
}

// 🧱 Step 3: Context Class
class DiscountContext
{
    private IDiscountStrategy _discountStrategy;

    public DiscountContext(IDiscountStrategy discountStrategy)
    {
        _discountStrategy = discountStrategy;
    }

    public void SetStrategy(IDiscountStrategy discountStrategy)
    {
        _discountStrategy = discountStrategy;
    }

    public double GetDiscount(double price)
    {
        return _discountStrategy.Calculate(price);
    }
}

class Program
{
    static void Main()
    {
        DiscountContext discount = new DiscountContext(new RegularDiscount());
        Console.WriteLine("Regular Discount: " + discount.GetDiscount(1000));

        discount.SetStrategy(new PremiumDiscount());
        Console.WriteLine("Premium Discount: " + discount.GetDiscount(1000));

        discount.SetStrategy(new VIPDiscount());
        Console.WriteLine("VIP Discount: " + discount.GetDiscount(1000));
    }
}
```
### 🖥 Sample Output
```yaml
Regular Discount: 50
Premium Discount: 100
VIP Discount: 200
```