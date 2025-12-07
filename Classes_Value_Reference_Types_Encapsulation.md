# Classes, Value/Reference Types, and Encapsulation in C#

`★ Insight ─────────────────────────────────────`
Think of a class as a blueprint for houses. The blueprint defines all houses have rooms, windows, and doors, but each actual house (object) can have different colors, sizes, and layouts. Value types are like carrying cash with you (direct value), while reference types are like having a bank account number that points to your money (reference to value).
`─────────────────────────────────────────────────`

## 1. What is a Class?

A **Class** is a blueprint or template for creating objects. It defines:

- **Properties/Fields**: Data the object holds
- **Methods**: Behaviors the object can perform
- **Constructors**: How to create instances
- **Events**: Actions the object can trigger

### Class Example:
```csharp
public class Car
{
    // Properties (data)
    public string Make { get; set; }
    public string Model { get; set; }
    public int Year { get; set; }
    private int speed; // Private field

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

// Creating objects (instances) from the class
Car myCar = new Car("Toyota", "Camry", 2023);
Car yourCar = new Car("Honda", "Civic", 2024);
```

### Class Components:

#### Fields (Data Members)
```csharp
public class Student
{
    private int studentId;      // Private field
    protected string name;      // Protected field
    public double gpa;          // Public field
    private static int count;   // Static field (shared by all instances)
}
```

#### Properties (Smart Fields)
```csharp
public class Person
{
    private string firstName;   // Backing field

    public string FirstName      // Property
    {
        get { return firstName; }           // Getter
        set { firstName = value?.Trim(); }   // Setter with logic
    }

    public string FullName => $"{FirstName} {LastName}";  // Expression-bodied
}
```

#### Methods
```csharp
public class Calculator
{
    public int Add(int a, int b) => a + b;           // Public method
    private bool ValidateInput(int value)           // Private helper
    {
        return value >= 0;
    }
}
```

---

## 2. Value Types vs Reference Types

### Value Types
- **Store data directly** in the variable
- **Each variable has its own copy**
- **Stored on the stack** (usually)
- **Assignment copies the value**

### Reference Types
- **Store reference (memory address)** to the actual data
- **Multiple variables can reference the same object**
- **Stored on the heap**
- **Assignment copies the reference**

```csharp
// VALUE TYPES
int x = 10;
int y = x;  // y gets a copy of x's value (10)
y = 20;     // x remains 10, y is now 20

Console.WriteLine($"x = {x}, y = {y}"); // x = 10, y = 20

// REFERENCE TYPES
Car car1 = new Car("Ford", "Mustang", 2023);
Car car2 = car1;  // car2 references the SAME object as car1

car2.Year = 2024; // This affects car1 too!

Console.WriteLine($"Car1 year: {car1.Year}"); // 2024 (changed!)
Console.WriteLine($"Car2 year: {car2.Year}"); // 2024
```

### Common Value Types:
- `int`, `double`, `float`, `decimal`
- `bool`
- `char`
- `DateTime`
- `struct` (custom value types)
- `enum`

### Common Reference Types:
- `string`
- `object`
- `arrays`
- `class` instances
- `delegate`
- `interface`

### Visual Representation:
```
VALUE TYPES:
[ Variable ] -> [ Actual Data ]
int x = 5;        [ x ] -> [ 5 ]

REFERENCE TYPES:
[ Variable ] -> [ Memory Address ] -> [ Actual Data ]
Car c = new Car(); [ c ] -> [ 0x7F3A ] -> [ Car Object ]
```

### Practical Examples:

#### Value Type Behavior
```csharp
public struct Point
{
    public int X, Y;

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }
}

Point p1 = new Point(1, 2);
Point p2 = p1;          // Copy of the structure
p2.X = 10;              // p1.X is still 1!

Console.WriteLine($"p1.X = {p1.X}, p2.X = {p2.X}"); // p1.X = 1, p2.X = 10
```

#### Reference Type Behavior
```csharp
public class DataList
{
    public List<int> Numbers { get; set; }

    public DataList()
    {
        Numbers = new List<int>();
    }
}

DataList list1 = new DataList();
list1.Numbers.Add(1);
list1.Numbers.Add(2);

DataList list2 = list1;  // Same reference!
list2.Numbers.Add(3);    // Affects list1 too!

Console.WriteLine($"list1 count: {list1.Numbers.Count}"); // 3
Console.WriteLine($"list2 count: {list2.Numbers.Count}"); // 3
```

#### Special Case: String (Reference Type with Value-like Behavior)
```csharp
string s1 = "Hello";
string s2 = s1;      // Reference copy
s2 = "World";        // Creates new string object

Console.WriteLine($"s1: {s1}"); // "Hello" (unchanged!)
Console.WriteLine($"s2: {s2}"); // "World"
```

### Boxing and Unboxing:
```csharp
// Boxing: Value type -> Reference type
int i = 123;
object obj = i;  // i is boxed

// Unboxing: Reference type -> Value type
int j = (int)obj;  // obj is unboxed
```

---

## 3. Encapsulation

**Encapsulation** is the bundling of data (fields) and methods that work on that data into a single unit (class). It also involves restricting direct access to some of an object's components.

### Key Concepts:
1. **Data Hiding**: Keep internal data private
2. **Controlled Access**: Use public properties/methods
3. **Validation**: Ensure data remains valid

### Encapsulation Example:

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

    // Constructor
    public BankAccount(string accountNumber, decimal initialBalance)
    {
        this.accountNumber = accountNumber;
        this.balance = initialBalance >= 0 ? initialBalance : 0;
    }

    // Public methods - controlled operations
    public bool Deposit(decimal amount)
    {
        if (amount <= 0)
        {
            Console.WriteLine("Deposit amount must be positive");
            return false;
        }

        balance += amount;
        Console.WriteLine($"Deposited ${amount}. New balance: ${balance}");
        return true;
    }

    public bool Withdraw(decimal amount)
    {
        // Validation
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
            Console.WriteLine("Account locked due to too many failed attempts");
            return false;
        }

        balance -= amount;
        failedAttempts = 0; // Reset on success
        Console.WriteLine($"Withdrew ${amount}. New balance: ${balance}");
        return true;
    }

    // Private helper method - internal use only
    private bool IsAccountLocked()
    {
        return failedAttempts >= 3;
    }
}
```

### Why Encapsulation Matters:

#### 1. **Data Protection**
```csharp
// BAD: No encapsulation
public class BadStudent
{
    public int Age; // Anyone can set invalid values
}

BadStudent s = new BadStudent();
s.Age = -5; // Invalid! Negative age?

// GOOD: With encapsulation
public class GoodStudent
{
    private int age;

    public int Age
    {
        get { return age; }
        set
        {
            if (value >= 0 && value <= 120)
                age = value;
            else
                throw new ArgumentException("Invalid age");
        }
    }
}
```

#### 2. **Implementation Flexibility**
```csharp
public class Temperature
{
    private double celsius; // Internal storage

    // Can change internal implementation without affecting users
    public double Celsius
    {
        get { return celsius; }
        set { celsius = value; }
    }

    public double Fahrenheit
    {
        get { return (celsius * 9/5) + 32; } // Calculated
        set { celsius = (value - 32) * 5/9; } // Calculated
    }

    public double Kelvin
    {
        get { return celsius + 273.15; } // Calculated
        set { celsius = value - 273.15; } // Calculated
    }
}
```

#### 3. **Validation and Business Rules**
```csharp
public class Product
{
    private decimal price;

    public decimal Price
    {
        get { return price; }
        set
        {
            if (value < 0)
                throw new ArgumentException("Price cannot be negative");
            if (value > 10000)
                throw new ArgumentException("Price exceeds maximum limit");
            price = value;
        }
    }

    // Complex business logic
    public bool ApplyDiscount(decimal percentage)
    {
        if (percentage < 0 || percentage > 50)
            return false;

        price = price * (1 - percentage / 100);
        return true;
    }
}
```

### Benefits of Encapsulation:

1. **Protection**: Prevents invalid data states
2. **Flexibility**: Can change internal implementation
3. **Security**: Controls what can be accessed/modified
4. **Maintainability**: Easier to debug and modify
5. **Abstraction**: Hides complexity from users

`★ Insight ─────────────────────────────────────`
Encapsulation is like a TV remote control. You don't need to know about the circuits inside (implementation details), you just use the buttons (public interface). The TV manufacturer can change the internal electronics as long as the buttons still work the same way.
`─────────────────────────────────────────────────`

### Access Modifiers in C#:
- **`public`**: Accessible from anywhere
- **`private`**: Accessible only within the same class
- **`protected`**: Accessible within class and derived classes
- **`internal`**: Accessible within the same assembly
- **`protected internal`**: Union of protected and internal

### Advanced Encapsulation Techniques:

#### Auto-Properties
```csharp
public class Person
{
    public string FirstName { get; set; }                    // Read/write
    public string LastName { get; private set; }            // Write only in class
    public int Age { get; }                                 // Read-only
    public DateTime CreatedAt { get; } = DateTime.Now;      // Auto-initialized
}
```

#### Property with Backing Field and Logic
```csharp
public class EmailAccount
{
    private string email;

    public string Email
    {
        get { return email; }
        set
        {
            if (IsValidEmail(value))
                email = value;
            else
                throw new ArgumentException("Invalid email format");
        }
    }

    private bool IsValidEmail(string email)
    {
        return email.Contains("@") && email.Contains(".");
    }
}
```

#### Computed Properties
```csharp
public class ShoppingCart
{
    private List<CartItem> items = new List<CartItem>();

    public IReadOnlyList<CartItem> Items => items.AsReadOnly(); // Read-only view

    public decimal Total
    {
        get
        {
            return items.Sum(item => item.Price * item.Quantity);
        }
    }

    public int ItemCount => items.Sum(item => item.Quantity);
}
```

### Summary:
These three fundamental concepts - **Classes** (blueprints), **Value/Reference Types** (how data is stored), and **Encapsulation** (data protection) - form the foundation of Object-Oriented Programming in C#. Together, they enable developers to create robust, maintainable, and secure applications.