### 1️⃣ is-a Relationship (Inheritance)

**Definition**:

যদি একটি class অন্য class-এর বৈশিষ্ট্য বা behavior inherit করে → তাকে is-a relationship বলা হয়।

সাধারণত inheritance এর মাধ্যমে তৈরি হয়।

### Example :
```c#
// Parent class
public class Vehicle {
    public void Start() {
        Console.WriteLine("Vehicle started");
    }
}

// Child class inherits Vehicle
public class Car : Vehicle {  // Car is-a Vehicle
    public void OpenTrunk() {
        Console.WriteLine("Trunk opened");
    }
}

// Usage
Car myCar = new Car();
myCar.Start();    // inherited method
myCar.OpenTrunk();
```


### Key Points:

- Child class = Subclass, Parent class = Superclass

- "is-a" = Child is a type of Parent

- Use inheritance

<br>

### 2️⃣ has-a Relationship (Aggregation / Association)

**Definition**:

যখন একটি class অন্য class-এর object contain করে, তখন তাকে has-a relationship বলা হয়।

এটাকে aggregation বা association বলা হয়।

### Example :
```c#
public class Engine {
    public void Start() {
        Console.WriteLine("Engine started");
    }
}

public class Car {
    public Engine engine;   // Car has-a Engine

    public Car() {
        engine = new Engine();
    }

    public void StartCar() {
        engine.Start();
        Console.WriteLine("Car started");
    }
}

// Usage
Car myCar = new Car();
myCar.StartCar();
```


### Key Points:

- Object containment

- Car has-an Engine

- Loose coupling (object lifetime can be independent in aggregation)


<br>

### 3️⃣ Association, Aggregation, Composition
### ✅ 3.1 Association

### Definition:

- দুটি class এর মধ্যে relationship আছে কিন্তু কোনো ownership নেই।

- Object lifetime independent।

- সাধারণত weak coupling।
- objects can exist independently.

<br>

### Example: Teacher ↔ Department
```c#
public class Teacher 
{
    public string Name { get; set; }
}

public class Department 
{
    public string DeptName { get; set; }

    public void AssignTeacher(Teacher teacher) 
    {
        Console.WriteLine($"{teacher.Name} assigned to {DeptName} Department");
    }
}

// Usage
Teacher t1 = new Teacher() { Name = "Mr. Rahman" };
Department d1 = new Department() { DeptName = "CSE" };
d1.AssignTeacher(t1);
```

### Key Points:

- Teacher and Department are associated

- Teacher can exist without Department

<br>

### ✅ 3.2 Aggregation

### Definition:
- Aggregation হলো “has-a” relationship।

- Container object owned object কে hold করে কিন্তু object lifetime independent।

- Weak ownership, can exist independently।

#### Example: Car has-a Engine (Engine can exist outside Car)
```c#
public class Engine 
{
    public void Start()
    {
        Console.WriteLine("Engine started");
    }
}


public class Car 
{
    public Engine Engine { get; set; }  // Car has-a Engine (aggregation)

    public Car(Engine engine) 
    {
        Engine = engine;  // Engine can exist independently
    }

    public void StartCar() 
    {
        Engine.Start();
        Console.WriteLine("Car started");
    }
}

// Usage
Engine e = new Engine();
Car car1 = new Car(e);
Car car2 = new Car(e);  // Same Engine used by multiple Cars
car1.StartCar();
```

### Key Points:

- Engine exists outside Car

- Engine can be shared by multiple Cars

- Weak ownership

<br>

### ✅ 3.3 Composition

 

**Definition**:

- Composition হলো strong ownership

- Container destroys contained objects when it is destroyed (contained object lifetime depends on container)

- Tight coupling

### Example: Car contains Engine (Engine cannot exist without Car)
```c#
public class Engine {
    public void Start() {
        Console.WriteLine("Engine started");
    }
}

public class Car {
    private Engine engine;  // Car contains Engine (composition)

    public Car() {
        engine = new Engine();  // Engine is created inside Car
    }

    public void StartCar() {
        engine.Start();
        Console.WriteLine("Car started");
    }
}

// Usage
Car car = new Car();
car.StartCar();
```
### Key Points:

- Engine **cannot exist without Car**

- Strong ownership

- Lifetime dependent


<br>

### ✅ Quick Summary Table: Object-Oriented Relationships

| Relationship   | Ownership       | Lifetime Dependency | Example                |
|----------------|----------------|-------------------|-----------------------|
| **is-a**       | Inheritance    | Independent       | Car **is-a** Vehicle  |
| **has-a**      | Containment    | Weak/Independent  | Car **has-a** Engine  |
| **Association**| None           | Independent       | Teacher ↔ Department  |
| **Aggregation**| Weak           | Independent       | Car **has-a** Engine  |
| **Composition**| Strong         | Dependent         | Car contains Engine    |
