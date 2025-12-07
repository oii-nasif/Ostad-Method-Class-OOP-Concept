# Optimized 90-Minute OOP in .NET Class Plan

## Learning Objectives
By the end of this session, students will:
- Master the four core OOP principles (Encapsulation, Inheritance, Polymorphism, Abstraction)
- Understand value vs. reference types and their impact on object behavior
- Create and properly initialize classes using constructors and method overloading
- Apply inheritance and polymorphism to create flexible, maintainable code
- Build a practical OOP application demonstrating all learned concepts

## Session Timeline (90 minutes)

---

### **PHASE 1: OOP FOUNDATIONS (30 minutes)**
**(Start: 10:00 - 10:30)**

#### **Module 1: Classes & Objects (10 minutes)**
**10:00-10:10**

**What is OOP? (3 mins)**
- Real-world analogy: LEGO blocks vs. clay sculpting
- Key benefits: Reusability, Scalability, Maintainability
- Quick demo: Procedural vs. OOP approach for a `Student`

**Classes vs. Objects (7 mins)**
- **Class**: Blueprint/template defining structure and behavior
- **Object**: Instance with actual data and state
- **Live Coding**: Create a comprehensive `Car` class
```csharp
public class Car
{
    // Properties (data)
    public string Make { get; set; }
    public string Model { get; set; }
    public int Year { get; set; }
    private int speed; // Private field for encapsulation

    // Constructor
    public Car(string make, string model, int year)
    {
        Make = make;
        Model = model;
        Year = year;
        speed = 0;
    }

    // Methods (behavior)
    public void Accelerate(int amount)
    {
        speed += amount;
        Console.WriteLine($"{Make} {Model} is now going {speed} mph");
    }

    public void Brake()
    {
        speed = 0;
        Console.WriteLine($"{Make} {Model} has stopped");
    }

    // Property with validation
    public int Speed
    {
        get { return speed; }
        private set { speed = Math.Max(0, value); }
    }
}

// Creating objects
Car myCar = new Car("Toyota", "Camry", 2023);
Car yourCar = new Car("Honda", "Civic", 2024);
```

`★ Insight ─────────────────────────────────────`
Think of classes as cookie cutters and objects as the actual cookies. One cutter (class) can create many cookies (objects), each with the same shape but potentially different decorations (property values).
`─────────────────────────────────────────────────`

#### **Module 2: Value vs. Reference Types (10 minutes)**
**10:10-10:20**

**Understanding Memory Model (5 mins)**
- **Value Types**: Store data directly, independent copies
- **Reference Types**: Store memory address, shared references
- Visual demonstration with diagrams

**Live Coding Demonstration (5 mins)**
```csharp
// VALUE TYPES
int x = 10;
int y = x;  // y gets a copy
y = 20;     // x remains 10
Console.WriteLine($"x = {x}, y = {y}"); // x = 10, y = 20

// REFERENCE TYPES
Car car1 = new Car("Ford", "Mustang", 2023);
Car car2 = car1;  // car2 references SAME object
car2.Year = 2024; // This affects car1 too!
Console.WriteLine($"Car1 year: {car1.Year}"); // 2024 (changed!)

// Special case: String (immutable reference type)
string s1 = "Hello";
string s2 = s1;
s2 = "World"; // Creates new string object
Console.WriteLine($"s1: {s1}"); // "Hello" (unchanged!)
```

**Key Takeaway**: Value types copy the value, reference types copy the reference!

#### **Module 3: Encapsulation (10 minutes)**
**10:20-10:30**

**What is Encapsulation? (3 mins)**
- Bundling data and methods together
- Hiding internal implementation details
- Controlling access through public interface

**Live Coding: Bank Account Example (7 mins)**
```csharp
public class BankAccount
{
    // Private fields - hidden from outside
    private decimal balance;
    private string accountNumber;
    private int failedAttempts = 0;

    // Public properties - controlled access
    public decimal Balance
    {
        get { return balance; }  // Read-only access
    }

    public string AccountNumber
    {
        get { return accountNumber; }
    }

    // Constructor with validation
    public BankAccount(string accountNumber, decimal initialBalance)
    {
        this.accountNumber = accountNumber;
        this.balance = initialBalance >= 0 ? initialBalance : 0;
    }

    // Public methods with business logic
    public bool Deposit(decimal amount)
    {
        if (amount <= 0)
        {
            Console.WriteLine("Deposit amount must be positive");
            return false;
        }
        balance += amount;
        return true;
    }

    public bool Withdraw(decimal amount)
    {
        if (amount <= 0)
        {
            Console.WriteLine("Withdrawal amount must be positive");
            failedAttempts++;
            return false;
        }

        if (amount > balance)
        {
            Console.WriteLine("Insufficient funds");
            failedAttempts++;
            return false;
        }

        if (failedAttempts >= 3)
        {
            Console.WriteLine("Account locked - too many failed attempts");
            return false;
        }

        balance -= amount;
        failedAttempts = 0; // Reset on success
        return true;
    }
}
```

**Quick Exercise**: Students add a `GetAccountInfo()` method that returns a formatted string.

---

### **PHASE 2: OBJECT CREATION (20 minutes)**
**(10:30 - 10:50)**

#### **Module 4: Constructors (10 minutes)**
**10:30-10:40**

**Constructor Types (3 mins)**
- Default (parameterless)
- Parameterized
- Constructor chaining
- Static constructors
- Private constructors (Singleton pattern)

**Live Coding: Employee Class with Constructor Overloading (7 mins)**
```csharp
public class Employee
{
    public string Name { get; set; }
    public string Department { get; set; }
    public double Salary { get; set; }
    public static int TotalEmployees { get; private set; }

    // Static constructor - runs once
    static Employee()
    {
        TotalEmployees = 0;
        Console.WriteLine("Employee class initialized");
    }

    // Full constructor
    public Employee(string name, string department, double salary)
    {
        Name = name;
        Department = department;
        Salary = salary >= 0 ? salary : 0;
        TotalEmployees++;
    }

    // Partial constructor - chains to full constructor
    public Employee(string name, string department)
        : this(name, department, 50000) // Default salary
    {
    }

    // Minimal constructor
    public Employee(string name)
        : this(name, "General") // Default department
    {
    }
}

// Usage examples
Employee emp1 = new Employee("Alice", "IT", 75000);
Employee emp2 = new Employee("Bob", "HR");
Employee emp3 = new Employee("Charlie");
Console.WriteLine($"Total employees: {Employee.TotalEmployees}");
```

#### **Module 5: Method Overloading & Object Initializers (10 minutes)**
**10:40-10:50**

**Method Overloading (6 mins)**
- Same method name, different parameters
- Compile-time polymorphism
- Practical examples

```csharp
public class Calculator
{
    public int Add(int a, int b)
    {
        Console.WriteLine("Adding two integers");
        return a + b;
    }

    public int Add(int a, int b, int c)
    {
        Console.WriteLine("Adding three integers");
        return a + b + c;
    }

    public double Add(double a, double b)
    {
        Console.WriteLine("Adding two doubles");
        return a + b;
    }

    public string Add(string a, string b)
    {
        Console.WriteLine("Concatenating strings");
        return a + b;
    }
}
```

**Object Initializers (4 mins)**
```csharp
// Traditional way
Car car1 = new Car("Toyota", "Camry", 2023);

// With object initializer
Car car2 = new Car
{
    Make = "Honda",
    Model = "Civic",
    Year = 2024
};

// Combining constructor + initializer
Car car3 = new Car("Ford", "Mustang", 2023)
{
    // Additional properties can be set here
};
```

---

### **PHASE 3: OOP PRINCIPLES (25 minutes)**
**(10:50 - 11:15)**

#### **Module 6: Inheritance (12 minutes)**
**10:50-11:02**

**Understanding Inheritance (3 mins)**
- "IS-A" relationship
- Code reuse and specialization
- Base classes and derived classes
- `base` keyword for constructor calls

**Live Coding: Vehicle Hierarchy (9 mins)**
```csharp
// Base class
public class Vehicle
{
    public string Brand { get; protected set; }
    public int Year { get; protected set; }
    public double Speed { get; protected set; }

    public Vehicle(string brand, int year)
    {
        Brand = brand;
        Year = year;
        Speed = 0;
    }

    public virtual void Accelerate(double amount)
    {
        Speed += amount;
        Console.WriteLine($"{Brand} accelerating to {Speed} mph");
    }

    public virtual void Brake()
    {
        Speed = 0;
        Console.WriteLine($"{Brand} has stopped");
    }

    public virtual string GetInfo()
    {
        return $"{Year} {Brand}";
    }
}

// Derived class
public class Car : Vehicle
{
    public int NumberOfDoors { get; private set; }
    public string FuelType { get; private set; }

    public Car(string brand, int year, int doors, string fuelType)
        : base(brand, year) // Call base constructor
    {
        NumberOfDoors = doors;
        FuelType = fuelType;
    }

    // Override base method
    public override void Accelerate(double amount)
    {
        base.Accelerate(amount); // Call base implementation
        Console.WriteLine($"Car with {NumberOfDoors} doors is moving");
    }

    // Override base method
    public override string GetInfo()
    {
        return $"{base.GetInfo()} Car ({NumberOfDoors} doors, {FuelType})";
    }
}

// Another derived class
public class ElectricCar : Car
{
    public double BatteryCapacity { get; private set; }

    public ElectricCar(string brand, int year, int doors, double batteryCapacity)
        : base(brand, year, doors, "Electric")
    {
        BatteryCapacity = batteryCapacity;
    }

    public override string GetInfo()
    {
        return $"{base.GetInfo()} - Battery: {BatteryCapacity} kWh";
    }
}

// Usage
Vehicle vehicle1 = new Car("Toyota", 2023, 4, "Gasoline");
Vehicle vehicle2 = new ElectricCar("Tesla", 2024, 4, 75);
```

#### **Module 7: Polymorphism (8 minutes)**
**11:02-11:10**

**Runtime Polymorphism (3 mins)**
- Method overriding with `virtual` and `override`
- Treating derived objects as base type
- Dynamic method dispatch

**Live Coding: Shape Drawing Example (5 mins)**
```csharp
public abstract class Shape
{
    public string Name { get; protected set; }

    public Shape(string name)
    {
        Name = name;
    }

    // Abstract method - must be implemented by derived classes
    public abstract double CalculateArea();
    public abstract double CalculatePerimeter();

    // Virtual method - can be overridden
    public virtual void Draw()
    {
        Console.WriteLine($"Drawing a generic {Name}");
    }
}

public class Circle : Shape
{
    public double Radius { get; private set; }

    public Circle(double radius) : base("Circle")
    {
        Radius = radius;
    }

    public override double CalculateArea() => Math.PI * Radius * Radius;
    public override double CalculatePerimeter() => 2 * Math.PI * Radius;

    public override void Draw()
    {
        Console.WriteLine($"Drawing a circle with radius {Radius}");
    }
}

public class Rectangle : Shape
{
    public double Width { get; private set; }
    public double Height { get; private set; }

    public Rectangle(double width, double height) : base("Rectangle")
    {
        Width = width;
        Height = height;
    }

    public override double CalculateArea() => Width * Height;
    public override double CalculatePerimeter() => 2 * (Width + Height);

    public override void Draw()
    {
        Console.WriteLine($"Drawing a rectangle {Width} x {Height}");
    }
}

// Polymorphism in action
List<Shape> shapes = new List<Shape>
{
    new Circle(5),
    new Rectangle(4, 6),
    new Circle(3)
};

foreach (Shape shape in shapes)
{
    shape.Draw(); // Calls the appropriate override method
    Console.WriteLine($"Area: {shape.CalculateArea():F2}");
    Console.WriteLine($"Perimeter: {shape.CalculatePerimeter():F2}");
    Console.WriteLine();
}
```

#### **Module 8: Abstraction (5 minutes)**
**11:10-11:15**

**Abstract Classes vs Interfaces (3 mins)**
- **Abstract Classes**: Partial implementation, can have state
- **Interfaces**: Pure contract, no implementation (historically)
- **When to use which**

**Quick Example (2 mins)**
```csharp
// Abstract class - shared implementation
public abstract class Animal
{
    public string Name { get; protected set; }
    public int Age { get; protected set; }

    protected Animal(string name, int age)
    {
        Name = name;
        Age = age;
    }

    // Concrete method - shared by all animals
    public void Sleep()
    {
        Console.WriteLine($"{Name} is sleeping");
    }

    // Abstract methods - must be implemented
    public abstract void MakeSound();
    public abstract void Move();
}

// Interface - pure capability
public interface ITrainable
{
    bool Train(string command);
    List<string> KnownCommands { get; }
}

// Dog uses both
public class Dog : Animal, ITrainable
{
    private List<string> commands = new List<string>();

    public Dog(string name, int age) : base(name, age) { }

    public override void MakeSound() => Console.WriteLine("Woof!");
    public override void Move() => Console.WriteLine($"{Name} is running");

    public bool Train(string command)
    {
        commands.Add(command);
        return true;
    }

    public List<string> KnownCommands => commands.AsReadOnly();
}
```

`★ Insight ─────────────────────────────────────`
Interfaces are like contracts - they promise WHAT a class can do without specifying HOW. This allows completely unrelated classes to work together if they share the same interface.
`─────────────────────────────────────────────────`

---

### **PHASE 4: PRACTICE & ASSESSMENT (15 minutes)**
**(11:15 - 11:30)**

#### **Module 9: Mini Project - Library Management System (12 minutes)**
**11:15-11:27**

**Project Requirements (2 mins)**
Create a mini OOP system with:
1. Abstract `LibraryItem` base class
2. `Book` and `DVD` derived classes
3. `ILoanable` interface
4. `Member` class

**Guided Implementation (10 mins)**
Students code along with instructor:

```csharp
// Step 1: Interface
public interface ILoanable
{
    bool IsAvailable { get; }
    DateTime? DueDate { get; }
    bool CheckOut();
    bool CheckIn();
}

// Step 2: Abstract base class
public abstract class LibraryItem : ILoanable
{
    public string Title { get; protected set; }
    public string ItemId { get; protected set; }
    public bool IsAvailable { get; private set; } = true;
    public DateTime? DueDate { get; private set; }

    protected LibraryItem(string title, string itemId)
    {
        Title = title;
        ItemId = itemId;
    }

    public bool CheckOut()
    {
        if (!IsAvailable) return false;
        IsAvailable = false;
        DueDate = DateTime.Now.AddDays(14);
        return true;
    }

    public bool CheckIn()
    {
        IsAvailable = true;
        DueDate = null;
        return true;
    }

    public abstract string GetDetails();
}

// Step 3: Derived classes
public class Book : LibraryItem
{
    public string Author { get; private set; }
    public int Pages { get; private set; }

    public Book(string title, string author, int pages, string itemId)
        : base(title, itemId)
    {
        Author = author;
        Pages = pages;
    }

    public override string GetDetails()
    {
        return $"Book: {Title} by {Author} ({Pages} pages)";
    }
}

public class DVD : LibraryItem
{
    public string Director { get; private set; }
    public int RuntimeMinutes { get; private set; }

    public DVD(string title, string director, int runtime, string itemId)
        : base(title, itemId)
    {
        Director = director;
        RuntimeMinutes = runtime;
    }

    public override string GetDetails()
    {
        return $"DVD: {Title} directed by {Director} ({RuntimeMinutes} min)";
    }
}

// Step 4: Member class
public class Member
{
    public string Name { get; private set; }
    public string MemberId { get; private set; }
    private List<LibraryItem> BorrowedItems = new List<LibraryItem>();

    public Member(string name, string memberId)
    {
        Name = name;
        MemberId = memberId;
    }

    public bool BorrowItem(LibraryItem item)
    {
        if (item.CheckOut())
        {
            BorrowedItems.Add(item);
            Console.WriteLine($"{Name} borrowed: {item.Title}");
            return true;
        }
        Console.WriteLine($"Cannot borrow: {item.Title}");
        return false;
    }

    public void ReturnItem(LibraryItem item)
    {
        if (BorrowedItems.Contains(item))
        {
            item.CheckIn();
            BorrowedItems.Remove(item);
            Console.WriteLine($"{Name} returned: {item.Title}");
        }
    }

    public void ListBorrowedItems()
    {
        Console.WriteLine($"\n{Name}'s borrowed items:");
        foreach (var item in BorrowedItems)
        {
            Console.WriteLine($"- {item.GetDetails()} (Due: {item.DueDate:MM/dd})");
        }
    }
}
```

#### **Module 10: Q&A and Wrap-up (3 minutes)**
**11:27-11:30**

**Quick Review (1 min)**
- Four OOP pillars: Encapsulation, Inheritance, Polymorphism, Abstraction
- Classes are blueprints, objects are instances
- Constructors initialize objects
- Inheritance creates "IS-A" relationships
- Polymorphism enables flexible code

**Key Takeaways (1 min)**
- Use encapsulation to protect data integrity
- Leverage inheritance to avoid code duplication
- Apply polymorphism for flexible, maintainable code
- Think in terms of objects and their relationships

**Next Steps (1 min)**
- Practice with real-world scenarios
- Explore SOLID principles
- Learn design patterns
- Study advanced C# features (generics, LINQ, async)

---

## Teaching Materials Reference

- **Classes & Value/Reference Types**: `Classes_Value_Reference_Types_Encapsulation.md`
- **Constructors**: `Constructors_Explained.md`
- **Method Overloading**: `Method_Overloading_Explained.md`
- **Encapsulation**: Integrated in first comprehensive file
- **Recursion**: `Recursion_in_CSharp.md` (Note: General programming topic, not OOP-specific)

---

## Teaching Strategies

### **Active Learning Techniques**
1. **Think-Pair-Share**: After each concept, students discuss with a partner
2. **Live Coding Errors**: Intentionally make bugs for students to spot
3. **Real-world Analogies**: Connect concepts to everyday objects
4. **Progressive Complexity**: Build on previous examples

### **Interactive Elements**
- **Poll Questions**: "What will this code output?"
- **Code Reviews**: Students evaluate each other's solutions
- **Refactoring Challenges**: Convert procedural to OOP code

## Assessment Strategy

### **Formative Assessment (During Class)**
- **Quick Code Checks**: Students share screens for immediate feedback
- **Concept Check Questions**: Multiple choice polls
- **Exit Ticket**: One thing learned + one question

### **Take-Home Practice**
```csharp
// Challenge: Design a Shape hierarchy
public abstract class Shape
{
    public abstract double CalculateArea();
    public abstract double CalculatePerimeter();
}

// Students implement: Rectangle, Triangle, Circle
```

### **Extension Activities**
- **Advanced**: Implement generics with OOP
- **Creative**: Design a game using OOP principles
- **Real-world**: Refactor existing procedural code

## Required Materials

### **Software**
- Visual Studio 2022 or VS Code with .NET SDK
- .NET 6/7/8 framework
- GitHub Copilot (optional, for learning)

### **Handouts**
1. **Cheat Sheet**: OOP concepts and syntax
2. **Class Diagram Templates**
3. **Practice Problems** with solutions

## Success Metrics

- Students can create and instantiate classes
- Understanding of inheritance hierarchies
- Ability to choose between interfaces and abstract classes
- Completion of the mini-library project

## Tips for Delivery

1. **Pace Yourself**: 20 minutes per module max
2. **Code First, Theory Second**: Show before telling
3. **Embrace Mistakes**: Use bugs as teaching moments
4. **Stay Interactive**: No more than 3 minutes of straight lecture
5. **Connect Concepts**: Always show how new concepts build on previous ones

---

This plan balances theory with hands-on practice, ensuring students not only understand OOP concepts but can immediately apply them in .NET. The progressive difficulty helps build confidence while the real-world examples make abstract concepts tangible.