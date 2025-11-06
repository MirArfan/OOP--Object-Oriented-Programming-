# 🧅 Onion Architecture

### Definition (সংজ্ঞা):
Onion Architecture is a software architectural pattern that emphasizes **separation of concerns** and **dependency inversion**, keeping the **core domain logic independent** from infrastructure and UI.

পেঁয়াজ আর্কিটেকচার হলো একটি সফটওয়্যার আর্কিটেকচার প্যাটার্ন যা মূলত উদ্দেশ্য অনুযায়ী ভাগ করা (separation of concerns) এবং dependency inversion-এর উপর ভিত্তি করে তৈরি। এতে core domain logic সবসময় infrastructure এবং UI থেকে স্বাধীন থাকে।

### ১. Layers / লেয়ারসমূহ
#### 1. Domain Layer (Core) / ডোমেইন লেয়ার

- Core layer, contains business logic, entities, value objects, domain services।
কোর লেয়ার, যেখানে ব্যবসায়িক লজিক, entity, value object, domain service থাকে।

- No dependency on outer layers / বাইরের লেয়ারের উপর কোন নির্ভরতা নেই।

- Example: User, Order, Product entities

#### 2. Application Layer / অ্যাপ্লিকেশন লেয়ার

- Implements specific use cases / services, using domain layer.
Domain layer ব্যবহার করে specific use cases / services তৈরি করে।

- No dependency on UI or infrastructure.
UI বা infrastructure এর উপর নির্ভরশীল নয়।

- Example: OrderService, UserService

#### 3. Infrastructure Layer / ইনফ্রাস্ট্রাকচার লেয়ার

- Handles database, network, file system, external APIs.
ডেটাবেস, নেটওয়ার্ক, ফাইল সিস্টেম, এক্সটার্নাল API পরিচালনা করে।

- Provides dependencies for application layer.
Application layer এর জন্য dependency হিসেবে কাজ করে।

- Example: EFCoreOrderRepository, FileStorageService

#### 4. Presentation Layer / প্রেজেন্টেশন লেয়ার

- Outer layer, interacts with the user.
বাইরের লেয়ার, ব্যবহারকারীর সাথে ইন্টারঅ্যাক্ট করে।

- Calls application layer to execute business logic.
Business logic execute করার জন্য Application layer কে কল করে।

- Example: ASP.NET Core MVC Controllers, API Controllers, Blazor Pages

### 2. Main Principles / মূল নীতি

1. **Dependency Rule** :

   - Outer layer never depends on inner layers.
বাইরের লেয়ার কখনো ভিতরের লেয়ারের উপর নির্ভর করবে না।

2. **Domain Layer Independence :**

    - Core domain logic is independent of infrastructure or UI.
কোর ডোমেইন লজিক সবসময় infrastructure এবং UI থেকে আলাদা থাকে।

3. **Testable :**

     - Layers are loosely coupled → easy to write unit tests.
লেয়ারগুলো loosely coupled → সহজে unit test লেখা যায়।



### Core Layer (Domain + Interfaces)

- Models: Student, Trainer, Course

- Interfaces: IStudentRepository, ITrainerRepository, ICourseRepository, IStudentService, ...

### Infrastructure Layer (Data Access)

- StudentRepository, TrainerRepository, CourseRepository

- Fake DB: Database (এইটি একটি in-memory fake DB)

### Service Layer (Business Logic / Application)

- StudentService, TrainerService, CourseService

### Presentation Layer (UI / Controllers)

- StudentController, TrainerController, CourseController

### Composition Root

- Program.Main যেখানে new Database(), repositories, services, controllers বানানো হচ্ছে এবং তাদের মধ্যে injection করা হচ্ছে।


---
### Onion Architecture এর লেয়ারগুলি:
#### Core Layer (সবচেয়ে ভিতরের লেয়ার)
- E**ntities/Models**: Domain model classes (Student, Trainer, Course)

- **Interfaces**: Repository এবং Service interfaces

- **Domain Logic:** Business rules এবং validation

#### Infrastructure Layer
- **Repositories**: Data access implementation (Database interaction)

- **External Services**: Third-party services implementation

#### Service Layer
- **Business Logic**: Application-specific business rules

- **Services**: Use cases implementation

#### Presentation Layer (সবচেয়ে বাইরের লেয়ার)
- **Controllers**: HTTP endpoints, UI interaction

- **Views**: User interface components

## আমাদের Implementation এ Onion Architecture:
Core Layer:
```c#
// Models
public class Student {
    public int StudentId { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

// Interfaces
public interface IStudentRepository {
    void AddStudent(Student student);
    void RemoveStudent(Student student);
    Student UpdateStudent(Student student);
    IList<Student> GetAllStudents();
}
```
### Infrastructure Layer:
```c#
public class StudentRepository : IStudentRepository {
    Database _db;
    
    public StudentRepository(Database db) {
        _db = db; // Dependency Injection
    }
    
    public void AddStudent(Student student) {
        _db.students.Add(student);
    }
    // অন্যান্য methods...
}
```
### Service Layer:
```c#
public class StudentService : IStudentService {
    IStudentRepository _studentRepository;
    
    public StudentService(IStudentRepository studentRepository) {
        _studentRepository = studentRepository; // Dependency Injection
    }
    
    public void AddStudent(Student student) {
        // Business Logic এখানে যোগ করা যায়
        _studentRepository.AddStudent(student);
    }
    // অন্যান্য methods...
}
```
### Presentation Layer:
```c#
public class StudentController {
    IStudentService _studentService;
    
    public StudentController(IStudentService studentService) {
        _studentService = studentService; // Dependency Injection
    }
    
    public void AddStudent(Student student) {
        _studentService.AddStudent(student);
    }
    // অন্যান্য methods...
}
```

###  Dependency Injection এর ভূমিকা:
Onion Architecture এর সফল implementation এর জন্য Dependency Injection অত্যন্ত গুরুত্বপূর্ণ। এটি আমাদেরকে:

1. লেয়ারগুলির মধ্যে loose coupling maintain করতে সাহায্য করে

2. Testing এর সময় mock objects ব্যবহার করতে দেয়

3. Code কে more flexible এবং maintainable করে

###  কিভাবে Data Flow হয়:
```
Presentation Layer (Controller) 
    → Service Layer (Business Logic) 
    → Infrastructure Layer (Repository) 
    → Database
```

#### উদাহরণ: যখন একটি Student add করা হয়:

- StudentController.AddStudent() method call হয়

- Controller StudentService.AddStudent() call করে

- Service business logic apply করে এবং StudentRepository.AddStudent() call করে

- Repository ultimately database এ data save করে

### 🏗️ Layer-wise Detailed Breakdown:
#### Layer 1: Domain Layer (Core/Entities)
```c#
// Domain Layer: Core business entities
public class Student
{
    public int StudentId { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }    
}

public class Trainer
{
    public int TrainerId { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

public class Course
{
    public int CourseId { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }    
}
```
### ✅ Key Points:

- SRP (Single Responsibility Principle) follow করে - প্রতিটি class শুধু entity data রাখে

- কোনো external dependency নেই (Database, Services, etc.)

- এটি Onion Architecture এর core/center


### Layer 2: Repository Interfaces (Abstractions)
```c#
// Repository Interfaces - Abstraction layer
public interface IStudentRepository
{
    void AddStudent(Student student);
    void RemoveStudent(Student student);
    Student UpdateStudent(Student student);
    Student GetStudentById(int id);
    List<Student> GetAllStudents();
}

public interface ITrainerRepository
{
    void AddTrainer(Trainer trainer);
    void RemoveTrainer(Trainer trainer);
    Trainer UpdateTrainer(Trainer trainer);
}

public interface ICourseRepository
{
    void AddCourse(Course course);
    void RemoveCourse(Course course);
    Course UpdateCourse(Course course);
}
```
### ✅ Key Points:

- DIP (Dependency Inversion Principle) follow করে

- High-level modules low-level modules এর উপর নির্ভর করে না

- Interfaces ব্যবহার করে loose coupling achieve করা হয়েছে

### Layer 3: Infrastructure Layer (Fake Database)
```c#
// Infrastructure Layer - Data storage
public class FakeDatabase
{
    public List<Student> Students { get; private set; }
    public List<Course> Courses { get; private set; }
    public List<Trainer> Trainers { get; private set; }

    public FakeDatabase()
    {
        Students = new List<Student>();
        Courses = new List<Course>();
        Trainers = new List<Trainer>();
    }
}
```
### ✅ Key Points:

- Data storage handling করে

- Outer layers এটির উপর নির্ভর করতে পারে

- Domain layer এর উপর কোনো dependency নেই



### Layer 4: Repository Implementations
```c#
// Concrete Repository Implementations
public class StudentRepository : IStudentRepository
{
    private readonly FakeDatabase _db;

    public StudentRepository(FakeDatabase db)
    {
        _db = db; // Dependency Injection
    }

    public void AddStudent(Student student) => _db.Students.Add(student);
    public void RemoveStudent(Student student) => _db.Students.Remove(student);
    public Student UpdateStudent(Student student) => student;
    public Student GetStudentById(int id) => _db.Students.FirstOrDefault(s => s.StudentId == id);
    public List<Student> GetAllStudents() => _db.Students.ToList();
}

// Similarly for TrainerRepository and CourseRepository
public class TrainerRepository : ITrainerRepository
{
    private readonly FakeDatabase _db;

    public TrainerRepository(FakeDatabase db) => _db = db;

    public void AddTrainer(Trainer trainer) => _db.Trainers.Add(trainer);
    public void RemoveTrainer(Trainer trainer) => _db.Trainers.Remove(trainer);
    public Trainer UpdateTrainer(Trainer trainer) => trainer;
}

public class CourseRepository : ICourseRepository
{
    private readonly FakeDatabase _db;

    public CourseRepository(FakeDatabase db) => _db = db;

    public void AddCourse(Course course) => _db.Courses.Add(course);
    public void RemoveCourse(Course course) => _db.Courses.Remove(course);
    public Course UpdateCourse(Course course) => course;
}
```
### ✅ Key Points:

- SRP follow করে - শুধু data access handling করে

- DIP follow করে - FakeDatabase inject করা হয়

- Domain logic/entities modify হয় না
### Full Code

```c#

using System;
using System.Collections.Generic;

// Onion Architecture

/*
Student:
StudentId, Name, Email
1, shahriar, shahriar@gmail.com
2, shahriar2, shahriar2@gmail.com

Trainer:
TrainerId, Name, Email

Course:
CourseId, Name, Description

Enrollment:
*/


// Fake Database
public class Database {
    public IList<Student> students;
    public IList<Course> courses;
    public IList<Trainer> trainers;

    public Database() {
        students = new List<Student>();
        courses = new List<Course>();
        trainers = new List<Trainer>();
    }
}

// Core Layer

// Models
public class Student {
    public int StudentId { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

public class Trainer {
    public int TrainerId { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

public class Course {
    public int CourseId { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
}

// Interfaces

// Repositories
public interface IStudentRepository {
    void AddStudent (Student student);
    void RemoveStudent (Student student);
    Student UpdateStudent (Student student);
    IList<Student> GetAllStudents();
}

public interface ITrainerRepository {
    void Add (Trainer trainer); 
    void Remove (Trainer trainer);
    Trainer Update (Trainer trainer);
}

public interface ICourseRepository {
    void Add (Course course);
    void Remove (Course course);
    Course Update (Course course);
}

// Services
public interface IStudentService {
    void AddStudent(Student student);
    void RemoveStudent(Student student);
    Student UpdateStudent(Student student);
    IList<Student> GetAllStudents();
}

public interface ITrainerService {
    void Add(Trainer trainer);
    void Remove(Trainer trainer);
    Trainer Update(Trainer trainer);
}

public interface ICourseService {
    void Add(Course course);
    void Remove(Course course);
    Course Update(Course course);
}

// Infrastructure Layer - Data Access Layer - Data layer
// Repositories
public class StudentRepository : IStudentRepository {
    Database _db;

    public StudentRepository (Database db) {
        _db = db;
    }

    public void AddStudent (Student student) {
        Console.WriteLine("StudentRepository.AddStudent()");
        _db.students.Add(student);
    }

    public void RemoveStudent (Student student) {
        _db.students.Remove(student);
    }

    public Student UpdateStudent (Student student) {
        // Update logic here
        return student;
    }

    public IList<Student> GetAllStudents() {
        return _db.students;
    }
}

public class TrainerRepository : ITrainerRepository {
    Database _db;

    public TrainerRepository(Database db) {
        _db = db;
    }

    public void Add (Trainer trainer) {
        _db.trainers.Add(trainer);
    }

    public void Remove (Trainer trainer) {
        _db.trainers.Remove(trainer);
    }

    public Trainer Update (Trainer trainer) {
        // Update logic here
        return trainer;
    }
}

public class CourseRepository : ICourseRepository {
    Database _db;

    public CourseRepository(Database db) {
        _db = db;
    }

    public void Add (Course course) {
        _db.courses.Add(course);
    }

    public void Remove (Course course) {
        _db.courses.Remove(course);
    }

    public Course Update (Course course) {
        // Update logic here
        return course;
    }
}

// Service Layer - Business Logic Layer
// Services - Business Logic

public class StudentService : IStudentService {
    IStudentRepository _studentRepository;

    public StudentService (IStudentRepository studentRepository) {
        _studentRepository = studentRepository;
    }

    public void AddStudent(Student student) {
        // Business Logic
        Console.WriteLine("StudentService.AddStudent()");
        _studentRepository.AddStudent(student);
    }

    public void RemoveStudent(Student student) {
        // Business Logic
        _studentRepository.RemoveStudent(student);
    }

    public Student UpdateStudent(Student student) {
        // Business Logic
        return _studentRepository.UpdateStudent(student);
    }

    public IList<Student> GetAllStudents() {
        return _studentRepository.GetAllStudents();
    }
}

public class TrainerService : ITrainerService {
    ITrainerRepository _trainerRepository;

    public TrainerService(ITrainerRepository trainerRepository) {
        _trainerRepository = trainerRepository;
    }

    public void Add(Trainer trainer) {
        // Business Logic
        _trainerRepository.Add(trainer);
    }

    public void Remove(Trainer trainer) {
        // Business Logic
        _trainerRepository.Remove(trainer);
    }

    public Trainer Update(Trainer trainer) {
        // Business Logic
        return _trainerRepository.Update(trainer);
    }
}

public class CourseService : ICourseService {
    ICourseRepository _courseRepository; 

    public CourseService(ICourseRepository courseRepository) {
        _courseRepository = courseRepository;
    }

    public void Add(Course course) {
        // Business Logic
        _courseRepository.Add(course);
    }

    public void Remove(Course course) {
        // Business Logic
        _courseRepository.Remove(course);
    }

    public Course Update(Course course) {
        // Business Logic
        return _courseRepository.Update(course);
    }
}

// Presentation Layer - UI Layer - Frontend Layer
// Controllers

public class StudentController {
    IStudentService _studentService;

    public StudentController(IStudentService studentService) {
        _studentService = studentService;
    }

    public void AddStudent(Student student) {
        Console.WriteLine("StudentController.AddStudent()");
        _studentService.AddStudent(student);
    }

    public void RemoveStudent(Student student) {
        _studentService.RemoveStudent(student);
    }

    public Student UpdateStudent(Student student) {
        return _studentService.UpdateStudent(student);
    }

    public IList<Student> GetAllStudents() {
        return _studentService.GetAllStudents();
    }
}

public class TrainerController {
    ITrainerService _trainerService;

    public TrainerController(ITrainerService trainerService) {
        _trainerService = trainerService;
    }

    public void Add(Trainer trainer) {
        _trainerService.Add(trainer);
    }

    public void Remove(Trainer trainer) {
        _trainerService.Remove(trainer);
    }

    public Trainer Update(Trainer trainer) {
        return _trainerService.Update(trainer);
    }
}

public class CourseController {
    ICourseService _courseService;

    public CourseController(ICourseService courseService) {
        _courseService = courseService;
    }

    public void Add(Course course) {
        _courseService.Add(course);
    }

    public void Remove(Course course) {
        _courseService.Remove(course);
    }

    public Course Update(Course course) {
        return _courseService.Update(course);
    }
}

// Core Layer -> Models, Interfaces
// Infrastructure Layer -> Repositories
// Service Layer -> Services
// Presentation Layer -> Controllers

// Frontend -> HTML -> JS -> Controllers -> Services -> Controller(Backend)(API) -> Services -> Repositories -> Database

class Program {
    public static void Main () {
        Database db = new Database();
        IStudentRepository studentRepository = new StudentRepository(db);
        IStudentService studentService = new StudentService(studentRepository);
        StudentController studentController = new StudentController(studentService);

        Student student = new Student {
            StudentId = 1,
            Name = "Shahriar",
            Email = "shahriar@gmail.com"
        };

        Student student2 = new Student {
            StudentId = 2,
            Name = "Shahriar2",
            Email = "shahriar2@gmail.com"
        };

        studentController.AddStudent(student);
        studentController.AddStudent(student2);
        Console.WriteLine("------------------------------");
        Console.WriteLine("Student Added");
        Console.WriteLine("------------------------------");
        
        IList<Student> students = studentController.GetAllStudents();
        foreach (var s in students) {
            Console.WriteLine("Student Details:");
            Console.WriteLine(s.Name);
            Console.WriteLine(s.Email);
            Console.WriteLine(s.StudentId);
            Console.WriteLine("------------------------------");
        }
    }
}


```