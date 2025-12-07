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

## Compiler Resolution Rules:
1. **Exact match** found
2. **Implicit conversion** possible
3. **Params array** matches
4. **No match** - compile error