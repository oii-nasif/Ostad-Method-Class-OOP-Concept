# Constructors in C#

`★ Insight ─────────────────────────────────────`
A constructor is like a building's foundation ceremony - it happens once when the object is "built", prepares everything for use, and you can't build (instantiate) without it. It ensures your object starts life in a valid, ready-to-use state.
`─────────────────────────────────────────────────`

A **Constructor** is a special method in a class that is automatically called when an object is created (instantiated). Its primary purpose is to initialize the object's data members and establish a valid initial state.

## Key Characteristics:
- **Same name as the class**
- **No return type** (not even `void`)
- **Automatically called** when using `new` keyword
- **Can be overloaded** (multiple constructors with different parameters)
- **Cannot be called directly** like regular methods

## Basic Constructor Example:

```csharp
public class Person
{
    // Fields
    public string Name;
    public int Age;

    // Constructor
    public Person(string name, int age)
    {
        Name = name;  // Initialize fields
        Age = age;
        Console.WriteLine($"Person {Name} created at age {Age}");
    }
}

// Usage
Person person1 = new Person("Alice", 25); // Constructor called automatically
```

## Types of Constructors:

### 1. Default Constructor
```csharp
public class Car
{
    public string Brand = "Unknown";
    public int Year = 2024;

    // Default constructor (no parameters)
    public Car()
    {
        Console.WriteLine("Default car created");
    }
}

// Usage
Car car1 = new Car(); // Uses default constructor
```

### 2. Parameterized Constructor
```csharp
public class Student
{
    public string Name;
    public int Grade;

    public Student(string name, int grade)
    {
        Name = name;
        Grade = grade;
    }
}

// Usage
Student student = new Student("Bob", 10);
```

### 3. Constructor Overloading
```csharp
public class Book
{
    public string Title;
    public string Author;
    public int Year;

    // Default constructor
    public Book()
    {
        Title = "Untitled";
        Author = "Anonymous";
        Year = 2024;
    }

    // Constructor with title
    public Book(string title)
    {
        Title = title;
        Author = "Anonymous";
        Year = 2024;
    }

    // Full constructor
    public Book(string title, string author, int year)
    {
        Title = title;
        Author = author;
        Year = year;
    }
}

// Usage
Book book1 = new Book();
Book book2 = new Book("1984");
Book book3 = new Book("1984", "George Orwell", 1949);
```

### 4. Constructor Chaining with `this`
```csharp
public class Employee
{
    public string Name;
    public string Department;
    public double Salary;

    // Full constructor
    public Employee(string name, string department, double salary)
    {
        Name = name;
        Department = department;
        Salary = salary;
    }

    // Partial constructor - calls full constructor
    public Employee(string name, string department)
        : this(name, department, 50000) // Calls the constructor above
    {
    }

    // Minimal constructor - calls partial constructor
    public Employee(string name)
        : this(name, "General") // Calls the constructor above
    {
    }
}
```

### 5. Private Constructor
```csharp
public class Singleton
{
    private static Singleton instance;

    // Private constructor - prevents external instantiation
    private Singleton()
    {
        Console.WriteLine("Singleton created");
    }

    public static Singleton GetInstance()
    {
        if (instance == null)
            instance = new Singleton();
        return instance;
    }
}

// Usage - can't use new Singleton()
Singleton singleton = Singleton.GetInstance();
```

### 6. Static Constructor
```csharp
public class DatabaseConnection
{
    public static string ConnectionString;
    public static int ConnectionCount;

    // Static constructor - called once, before any instance
    static DatabaseConnection()
    {
        ConnectionString = "Server=localhost;Database=MyDB;";
        ConnectionCount = 0;
        Console.WriteLine("DatabaseConnection initialized");
    }

    public DatabaseConnection()
    {
        ConnectionCount++;
    }
}
```

`★ Insight ─────────────────────────────────────`
Constructors enforce object invariants - rules that must always be true for your object. By initializing properties correctly in the constructor, you prevent objects from ever existing in an invalid state, which is a key principle of robust OOP design.
`─────────────────────────────────────────────────`

## Constructor in Inheritance:

```csharp
public class Vehicle
{
    public string Brand;

    public Vehicle(string brand)
    {
        Brand = brand;
        Console.WriteLine($"Vehicle {Brand} created");
    }
}

public class Car : Vehicle
{
    public int Doors;

    // Base class constructor called with 'base'
    public Car(string brand, int doors) : base(brand)
    {
        Doors = doors;
        Console.WriteLine($"Car with {Doors} doors created");
    }
}
```

## Constructor vs. Regular Methods:
```csharp
public class Counter
{
    private int count;

    // Constructor - sets initial state
    public Counter(int initialValue = 0)
    {
        count = initialValue;
    }

    // Regular method - changes state during object lifetime
    public void Increment()
    {
        count++;
    }

    public int GetCount()
    {
        return count;
    }
}
```

## Constructor Execution Order in Inheritance:
1. **Static constructors** (base class first, then derived)
2. **Instance constructors** (base class first, then derived)
3. **Field initializers** (before constructor body runs)

```csharp
public class Base
{
    static Base() => Console.WriteLine("Base static constructor");
    public Base() => Console.WriteLine("Base instance constructor");
}

public class Derived : Base
{
    static Derived() => Console.WriteLine("Derived static constructor");
    public Derived() => Console.WriteLine("Derived instance constructor");
}

// Output when creating new Derived():
// Base static constructor
// Derived static constructor
// Base instance constructor
// Derived instance constructor
```

## Constructor vs. Object Initializers:
```csharp
// Using constructor
var person1 = new Person("Alice", 25);

// Using object initializer
var person2 = new Person
{
    Name = "Bob",
    Age = 30
};

// Combined approach
var person3 = new Person("Charlie") { Age = 35 };
```

## Best Practices:
1. **Keep constructors simple** - don't do complex logic
2. **Initialize all fields** - avoid null/default values
3. **Validate parameters** - ensure valid initial state
4. **Use constructor chaining** to avoid code duplication
5. **Consider factory methods** for complex object creation
6. **Document constructor behavior** clearly

## When to Use Different Constructor Types:

- **Default Constructor**: When objects have sensible defaults
- **Parameterized Constructor**: When essential data must be provided
- **Private Constructor**: For singleton patterns or static utility classes
- **Static Constructor**: For one-time initialization of static resources
- **Constructor Chaining**: To eliminate duplicate initialization code

Constructors are fundamental to OOP because they ensure objects always start in a predictable, valid state, which prevents many common bugs and makes your code more reliable.