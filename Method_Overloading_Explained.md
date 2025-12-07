# Method Overloading in C#

`★ Insight ─────────────────────────────────────`
Method overloading is like having multiple entrances to the same building - each entrance leads to the same destination but accepts different types of visitors. This creates a more flexible and intuitive API for your classes.
`─────────────────────────────────────────────────`

**Method Overloading** is a C# feature that allows you to define multiple methods with the **same name** but **different parameters** within the same class. The compiler determines which method to call based on the arguments you provide.

## Key Characteristics:
- **Same method name**
- **Different parameter lists** (different types, different number, or different order)
- **Same return type** or different - doesn't matter for overloading
- **Compile-time polymorphism** (resolved at compile time)

## Example:

```csharp
public class Calculator
{
    // Overload 1: Two integers
    public int Add(int a, int b)
    {
        Console.WriteLine("Adding two integers");
        return a + b;
    }

    // Overload 2: Three integers
    public int Add(int a, int b, int c)
    {
        Console.WriteLine("Adding three integers");
        return a + b + c;
    }

    // Overload 3: Two doubles
    public double Add(double a, double b)
    {
        Console.WriteLine("Adding two doubles");
        return a + b;
    }

    // Overload 4: String concatenation
    public string Add(string a, string b)
    {
        Console.WriteLine("Concatenating strings");
        return a + b;
    }
}
```

## Usage:
```csharp
Calculator calc = new Calculator();

calc.Add(5, 3);        // Calls overload 1
calc.Add(1, 2, 3);     // Calls overload 2
calc.Add(3.14, 2.71);  // Calls overload 3
calc.Add("Hello", " World"); // Calls overload 4
```

## Common Use Cases:
- **Constructors** with different initialization options
- **Utility methods** that work with different data types
- **Optional parameters** before C# 4.0
- **Convenient APIs** that accept multiple input formats

`★ Insight ─────────────────────────────────────`
Method overloading improves code readability and usability by providing a consistent interface name while handling different scenarios. It's better than having methods like `AddIntegers()`, `AddDoubles()`, `AddThreeIntegers()` - the single `Add()` name makes the API cleaner and more intuitive.
`─────────────────────────────────────────────────`

## Difference from Method Overriding:
- **Overloading**: Same class, same name, different parameters (compile-time)
- **Overriding**: Different classes (inheritance), same name, same parameters (runtime)

## More Advanced Examples:

### Constructor Overloading
```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    // Default constructor
    public Person()
    {
        Name = "Unknown";
        Age = 0;
    }

    // Constructor with name only
    public Person(string name)
    {
        Name = name;
        Age = 0;
    }

    // Constructor with name and age
    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

### Parameter Order Matters
```csharp
public class Logger
{
    public void Log(string message, int priority)
    {
        Console.WriteLine($"Priority {priority}: {message}");
    }

    public void Log(int priority, string message)
    {
        Console.WriteLine($"[{priority}] {message}");
    }
}
```

### Optional Parameters vs Overloading
```csharp
// Before C# 4.0 - Used overloading
public void Process(string data)
{
    Process(data, false);
}

public void Process(string data, bool force)
{
    // Implementation
}

// After C# 4.0 - Can use optional parameters
public void Process(string data, bool force = false)
{
    // Implementation
}
```

## Best Practices:
1. **Use meaningful overloads** - don't confuse users
2. **Keep behavior consistent** - all overloads should do similar things
3. **Consider optional parameters** for C# 4.0+
4. **Document your overloads** clearly with XML comments
5. **Avoid too many overloads** - can make API confusing

## Method Overriding

**Method Overriding** is completely different from overloading. Overriding allows a **derived class** to provide a specific implementation of a method that is already defined in its **base class**.

### Key Characteristics of Overriding:
- **Same method signature** (name, parameters, return type)
- **Different classes** (base and derived)
- **Runtime polymorphism** (determined at runtime)
- **Uses `virtual` and `override` keywords**

### Example: Overriding in Action

```csharp
// Base class
public class Animal
{
    public string Name { get; set; }

    public Animal(string name)
    {
        Name = name;
    }

    // Virtual method - can be overridden
    public virtual void MakeSound()
    {
        Console.WriteLine("Some generic animal sound");
    }

    // Virtual method with behavior
    public virtual void Move()
    {
        Console.WriteLine("Animal is moving");
    }
}

// Derived class
public class Dog : Animal
{
    public Dog(string name) : base(name) { }

    // Override base method
    public override void MakeSound()
    {
        Console.WriteLine("Woof! Woof!");
    }

    // Override base method and extend it
    public override void Move()
    {
        base.Move(); // Call base implementation
        Console.WriteLine($"{Name} is running on four legs");
    }
}

// Another derived class
public class Cat : Animal
{
    public Cat(string name) : base(name) { }

    public override void MakeSound()
    {
        Console.WriteLine("Meow!");
    }

    public override void Move()
    {
        base.Move();
        Console.WriteLine($"{Name} is sneaking quietly");
    }
}
```

### Using Overridden Methods:

```csharp
// Create different animal objects
Animal animal1 = new Animal("Generic");
Animal dog = new Dog("Buddy");
Animal cat = new Cat("Whiskers");

// Polymorphism in action
animal1.MakeSound();  // "Some generic animal sound"
dog.MakeSound();      // "Woof! Woof!"
cat.MakeSound();      // "Meow!"

dog.Move();  // Animal is moving + "Buddy is running on four legs"
cat.Move();  // Animal is moving + "Whiskers is sneaking quietly"
```

### Overriding Rules:

#### 1. Must use `virtual` in base class
```csharp
public class Parent
{
    public virtual void Display() { Console.WriteLine("Parent"); }
}

// Cannot override non-virtual methods
public void Method() { }        // ❌ Cannot be overridden
public virtual void Method2() { } // ✅ Can be overridden
```

#### 2. Must use `override` in derived class
```csharp
public class Child : Parent
{
    // Must use override keyword
    public override void Display() { Console.WriteLine("Child"); }
}
```

#### 3. Access modifiers cannot be more restrictive
```csharp
public class Base
{
    protected virtual void Method() { }
}

public class Derived : Base
{
    protected override void Method() { }     // ✅ Same access level
    // private override void Method() { }     // ❌ More restrictive
    // public override void Method() { }      // ✅ Less restrictive
}
```

### Abstract Methods (Must Override)

```csharp
public abstract class Shape
{
    public string Name { get; protected set; }

    protected Shape(string name)
    {
        Name = name;
    }

    // Abstract method - no implementation, must be overridden
    public abstract double CalculateArea();

    // Abstract method - must be overridden
    public abstract double CalculatePerimeter();

    // Regular virtual method - can be overridden
    public virtual void Draw()
    {
        Console.WriteLine($"Drawing a {Name}");
    }
}

public class Circle : Shape
{
    public double Radius { get; private set; }

    public Circle(double radius) : base("Circle")
    {
        Radius = radius;
    }

    // Must implement abstract methods
    public override double CalculateArea() => Math.PI * Radius * Radius;
    public override double CalculatePerimeter() => 2 * Math.PI * Radius;

    // Can override virtual method (optional)
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

    // Using default Draw() method from Shape
}
```

### Sealed Methods (Cannot Override Further)

```csharp
public class Vehicle
{
    public virtual void StartEngine()
    {
        Console.WriteLine("Vehicle engine started");
    }
}

public class Car : Vehicle
{
    // Prevent further overriding
    public sealed override void StartEngine()
    {
        Console.WriteLine("Car engine started with key");
    }
}

public class SportsCar : Car
{
    // ❌ Cannot override sealed method
    // public override void StartEngine() { }
}
```

## Overloading vs. Overriding - Side by Side

| Aspect | Method Overloading | Method Overriding |
|--------|-------------------|------------------|
| **Purpose** | Multiple methods with same name for convenience | Change behavior in derived class |
| **Scope** | Same class | Base and derived classes |
| **Parameters** | Must be different (type, number, or order) | Must be identical |
| **Keywords** | No special keywords needed | `virtual`, `override` |
| **Resolution** | Compile-time | Runtime |
| **Relationship** | Compile-time polymorphism | Runtime polymorphism |

### Complete Example Demonstrating Both:

```csharp
public class Employee
{
    public string Name { get; set; }
    public decimal BaseSalary { get; set; }

    // Method Overloading (same class, different parameters)
    public decimal CalculateBonus()
    {
        return BaseSalary * 0.1m; // Default 10% bonus
    }

    public decimal CalculateBonus(decimal percentage)
    {
        return BaseSalary * percentage;
    }

    public decimal CalculateBonus(decimal percentage, int yearsOfService)
    {
        decimal bonus = BaseSalary * percentage;
        if (yearsOfService > 5)
            bonus += 1000; // Service bonus
        return bonus;
    }

    // Virtual method for overriding
    public virtual string GetJobTitle()
    {
        return "Employee";
    }
}

public class Manager : Employee
{
    public int TeamSize { get; set; }

    // Method Overriding (different class, same signature)
    public override string GetJobTitle()
    {
        return "Manager";
    }

    // Can still do overloading in derived class
    public decimal CalculateBonus(decimal percentage, int yearsOfService, int teamBonus)
    {
        decimal bonus = BaseSalary * percentage;
        if (yearsOfService > 5)
            bonus += 1000;
        bonus += teamBonus * 100; // Team leadership bonus
        return bonus;
    }
}

// Usage
Employee emp = new Employee { Name = "John", BaseSalary = 50000 };
Manager mgr = new Manager { Name = "Sarah", BaseSalary = 80000, TeamSize = 5 };

// Overloading examples
Console.WriteLine(emp.CalculateBonus());                           // 5000
Console.WriteLine(emp.CalculateBonus(0.15m));                      // 7500
Console.WriteLine(emp.CalculateBonus(0.15m, 6));                    // 8500

Console.WriteLine(mgr.CalculateBonus(0.2m, 7, 50));                // Additional overloads

// Overriding examples
Console.WriteLine(emp.GetJobTitle());  // "Employee"
Console.WriteLine(mgr.GetJobTitle());  // "Manager"

// Polymorphism
Employee empAsManager = new Manager { Name = "Mike", BaseSalary = 75000 };
Console.WriteLine(empAsManager.GetJobTitle());  // "Manager" (calls override)
```

## Compiler Resolution Rules:
1. **Exact match** found
2. **Implicit conversion** possible
3. **Params array** matches
4. **No match** - compile error