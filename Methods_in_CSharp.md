# Methods in C#

`★ Insight ─────────────────────────────────────`
Methods are like verbs in a sentence - they describe what objects can do. Just as a person can speak, walk, or think, objects have methods that define their behaviors and actions.
`─────────────────────────────────────────────────`

## What is a Method?

A **Method** is a code block that performs a specific task and can be called when needed. It's essentially a named block of code that:

- **Receives input** through parameters
- **Performs an action** or calculation
- **Returns a result** (optional)

## Basic Method Structure:

```csharp
[Access Modifier] [Return Type] [MethodName]([Parameters])
{
    // Method body - code to execute
    return [value]; // If return type is not void
}
```

## Method Examples:

### 1. Method with No Parameters and No Return Value
```csharp
public class Greeting
{
    public void SayHello()
    {
        Console.WriteLine("Hello, World!");
    }

    public void PrintCurrentTime()
    {
        Console.WriteLine($"Current time: {DateTime.Now}");
    }
}

// Usage
Greeting greeting = new Greeting();
greeting.SayHello();
greeting.PrintCurrentTime();
```

### 2. Method with Parameters
```csharp
public class Calculator
{
    // Method that takes two integers and returns their sum
    public int Add(int a, int b)
    {
        return a + b;
    }

    // Method that takes string and integer
    public void PrintMessage(string message, int times)
    {
        for (int i = 0; i < times; i++)
        {
            Console.WriteLine(message);
        }
    }

    // Method with multiple parameters of different types
    public string FormatUserInfo(string name, int age, bool isActive)
    {
        string status = isActive ? "Active" : "Inactive";
        return $"{name} (Age: {age}, Status: {status})";
    }
}

// Usage
Calculator calc = new Calculator();
int sum = calc.Add(5, 3);              // Returns 8
calc.PrintMessage("Hello", 3);         // Prints "Hello" 3 times
string info = calc.FormatUserInfo("Alice", 25, true);
```

### 3. Method with Return Value
```csharp
public class TemperatureConverter
{
    // Method that converts Celsius to Fahrenheit
    public double CelsiusToFahrenheit(double celsius)
    {
        return (celsius * 9/5) + 32;
    }

    // Method that returns boolean
    public bool IsHot(double celsius)
    {
        return celsius > 30;
    }

    // Method that returns object
    public DateTime GetFutureDate(int daysFromNow)
    {
        return DateTime.Now.AddDays(daysFromNow);
    }
}

// Usage
TemperatureConverter converter = new TemperatureConverter();
double fahrenheit = converter.CelsiusToFahrenheit(25);    // Returns 77
bool hot = converter.IsHot(35);                          // Returns true
DateTime futureDate = converter.GetFutureDate(7);        // Returns date 7 days from now
```

## Access Modifiers:

### Public Methods
```csharp
public class BankAccount
{
    public void Deposit(decimal amount)  // Can be called from anywhere
    {
        // Implementation
    }
}
```

### Private Methods
```csharp
public class BankAccount
{
    public void Deposit(decimal amount)
    {
        if (ValidateAmount(amount))  // Call private method
        {
            // Deposit logic
        }
    }

    private bool ValidateAmount(decimal amount)  // Can only be called within this class
    {
        return amount > 0;
    }
}
```

## Static Methods vs Instance Methods:

### Instance Methods (require object creation)
```csharp
public class Student
{
    public string Name { get; set; }
    public double Grade { get; set; }

    // Instance method - works with object's data
    public string GetGradeLetter()
    {
        if (Grade >= 90) return "A";
        if (Grade >= 80) return "B";
        if (Grade >= 70) return "C";
        return "F";
    }
}

// Usage
Student student = new Student { Name = "Alice", Grade = 85 };
string grade = student.GetGradeLetter();  // Returns "B"
```

### Static Methods (called on class, no object needed)
```csharp
public class MathHelper
{
    // Static method - doesn't need object creation
    public static double CalculateAreaOfCircle(double radius)
    {
        return Math.PI * radius * radius;
    }

    public static bool IsEven(int number)
    {
        return number % 2 == 0;
    }
}

// Usage
double area = MathHelper.CalculateAreaOfCircle(5);  // No object creation needed
bool even = MathHelper.IsEven(4);
```

## Method Parameters:

### Value Parameters (default)
```csharp
public void ProcessNumber(int number)
{
    number = number * 2;  // Only changes local copy
    Console.WriteLine($"Inside method: {number}");
}

int original = 10;
ProcessNumber(original);  // Prints "Inside method: 20"
Console.WriteLine($"Outside method: {original}");  // Prints "Outside method: 10"
```

### Reference Parameters (ref keyword)
```csharp
public void DoubleNumber(ref int number)
{
    number = number * 2;  // Changes original variable
}

int original = 10;
DoubleNumber(ref original);  // original is now 20
```

### Output Parameters (out keyword)
```csharp
public bool TryParse(string input, out int result)
{
    return int.TryParse(input, out result);
}

if (TryParse("123", out int number))
{
    Console.WriteLine($"Parsed successfully: {number}");  // Prints 123
}
```

### Optional Parameters
```csharp
public class Greeter
{
    public void Greet(string name = "Guest", string message = "Welcome")
    {
        Console.WriteLine($"{message}, {name}!");
    }
}

// Usage
Greeter greeter = new Greeter();
greeter.Greet();                    // "Welcome, Guest!"
greeter.Greet("Alice");             // "Welcome, Alice!"
greeter.Greet("Bob", "Hello");      // "Hello, Bob!"
```

## Expression-Bodied Methods (C# 6+):

```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }

    // Traditional method
    public string GetFullNameTraditional()
    {
        return $"{FirstName} {LastName}";
    }

    // Expression-bodied method (same result, shorter)
    public string GetFullName() => $"{FirstName} {LastName}";

    // Single-line methods
    public bool IsAdult => DateTime.Now.Year - BirthYear >= 18;
    public int Age => DateTime.Now.Year - BirthYear;
}
```

## Method Best Practices:

### 1. Single Responsibility
```csharp
// GOOD: Each method does one thing
public class EmailService
{
    public bool ValidateEmail(string email) => email.Contains("@");
    public void SendEmail(string email, string message) { /* send logic */ }
}

// BAD: Method doing too many things
public void ProcessAndValidateAndSendEmail(string email, string message)
{
    // Validation, processing, and sending all mixed together
}
```

### 2. Clear Naming
```csharp
// GOOD: Method name describes what it does
public double CalculateTotalPrice(double price, int quantity, double taxRate)
{
    return (price * quantity) * (1 + taxRate);
}

// BAD: Unclear method name
public double Process(double p, int q, double t)
{
    return (p * q) * (1 + t);
}
```

### 3. Proper Parameter Validation
```csharp
public bool Withdraw(decimal amount)
{
    // Validate parameters at the start
    if (amount <= 0)
    {
        throw new ArgumentException("Amount must be positive");
    }

    if (amount > Balance)
    {
        return false;
    }

    Balance -= amount;
    return true;
}
```

## Methods in Classes (Putting It All Together):

```csharp
public class ShoppingCart
{
    private List<string> items = new List<string>();
    private decimal total = 0;

    // Public method to add item
    public void AddItem(string item, decimal price)
    {
        if (ValidateItem(item, price))
        {
            items.Add(item);
            total += price;
            Console.WriteLine($"Added: {item} (${price})");
        }
    }

    // Private helper method
    private bool ValidateItem(string item, decimal price)
    {
        return !string.IsNullOrWhiteSpace(item) && price > 0;
    }

    // Public method to calculate total with tax
    public decimal CalculateTotalWithTax(decimal taxRate = 0.08)
    {
        return total * (1 + taxRate);
    }

    // Public method to display contents
    public void DisplayItems()
    {
        Console.WriteLine("\nShopping Cart Contents:");
        for (int i = 0; i < items.Count; i++)
        {
            Console.WriteLine($"{i + 1}. {items[i]}");
        }
        Console.WriteLine($"Subtotal: ${total:F2}");
    }

    // Static method for tax calculation
    public static decimal CalculateTax(decimal amount, decimal taxRate)
    {
        return amount * taxRate;
    }
}
```

## Common Method Patterns:

### 1. Property Accessors (Getters/Setters)
```csharp
public class Person
{
    private int age;

    public int Age
    {
        get { return age; }                    // Getter method
        set
        {
            if (value >= 0 && value <= 150)    // Setter method with validation
                age = value;
        }
    }
}
```

### 2. Factory Methods
```csharp
public class User
{
    public string Username { get; private set; }
    public string Email { get; private set; }

    private User(string username, string email)
    {
        Username = username;
        Email = email;
    }

    // Factory method
    public static User CreateUser(string username, string email)
    {
        // Validation and creation logic
        if (string.IsNullOrWhiteSpace(username))
            throw new ArgumentException("Username required");

        return new User(username, email);
    }
}
```

### 3. Method Chaining
```csharp
public class QueryBuilder
{
    private string query = "";

    public QueryBuilder Select(string columns)
    {
        query += $"SELECT {columns} ";
        return this;  // Return this for chaining
    }

    public QueryBuilder From(string table)
    {
        query += $"FROM {table} ";
        return this;
    }

    public QueryBuilder Where(string condition)
    {
        query += $"WHERE {condition} ";
        return this;
    }

    public string Build() => query;

    // Usage: builder.Select("*").From("users").Where("active = 1").Build();
}
```

`★ Insight ─────────────────────────────────────`
Good methods are like good employees - they have one clear responsibility, do their job well, and communicate clearly about what they do and what they need to do it.
`─────────────────────────────────────────────────`

## Summary:

Methods are the building blocks of C# programs that:

1. **Define behavior** of objects
2. **Organize code** into reusable blocks
3. **Take input** through parameters
4. **Return results** when needed
5. **Can be public** (accessible everywhere) or **private** (internal use)
6. **Can be static** (class-level) or **instance** (object-level)

Understanding methods is fundamental to OOP because they define what objects can DO, while properties define what objects HAVE.