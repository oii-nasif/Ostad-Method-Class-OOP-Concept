# Recursion in C#

`★ Insight ─────────────────────────────────────`
Recursion is like a Russian nesting doll - each doll contains a smaller version of itself, and you keep opening them until you reach the smallest one. Then you work your way back out, using the results from each level to solve the larger problem.
`─────────────────────────────────────────────────`

**Recursion** is a programming technique where a method calls itself to solve a problem. It breaks down complex problems into smaller, similar subproblems until reaching a base case that can be solved directly.

## Key Components of Recursion:

### 1. Base Case
The condition that stops recursion
### 2. Recursive Case
The part that calls itself with a smaller/different input

## Basic Example: Factorial

```csharp
public int Factorial(int n)
{
    // Base Case: Factorial of 0 or 1 is 1
    if (n <= 1)
        return 1;

    // Recursive Case: n! = n * (n-1)!
    return n * Factorial(n - 1);
}

// Usage
int result = Factorial(5); // Returns 120 (5 × 4 × 3 × 2 × 1)
```

## How it Works (Step-by-step):

```
Factorial(4)
= 4 * Factorial(3)
= 4 * (3 * Factorial(2))
= 4 * (3 * (2 * Factorial(1)))
= 4 * (3 * (2 * 1))    // Base case reached
= 4 * (3 * 2)
= 4 * 6
= 24
```

## Common Recursive Examples:

### Fibonacci Sequence
```csharp
public int Fibonacci(int n)
{
    // Base cases
    if (n <= 0) return 0;
    if (n == 1) return 1;

    // Recursive case
    return Fibonacci(n - 1) + Fibonacci(n - 2);
}
```

### Power Function
```csharp
public double Power(double baseNum, int exponent)
{
    // Base case: anything to the power of 0 is 1
    if (exponent == 0)
        return 1;

    // Recursive case
    return baseNum * Power(baseNum, exponent - 1);
}
```

### Greatest Common Divisor (GCD)
```csharp
public int GCD(int a, int b)
{
    // Base case
    if (b == 0)
        return a;

    // Recursive case
    return GCD(b, a % b);
}
```

## Tree Traversal (Classic Recursion):

```csharp
public class TreeNode
{
    public int Value;
    public TreeNode Left;
    public TreeNode Right;
}

public void InOrderTraversal(TreeNode node)
{
    // Base case: empty node
    if (node == null)
        return;

    // Recursive: left subtree
    InOrderTraversal(node.Left);

    // Process current node
    Console.Write(node.Value + " ");

    // Recursive: right subtree
    InOrderTraversal(node.Right);
}
```

`★ Insight ─────────────────────────────────────`
Recursive solutions often mirror the mathematical definition of a problem, making the code more elegant and easier to understand. However, they can be less efficient due to the overhead of multiple method calls and potential stack overflow issues.
`─────────────────────────────────────────────────`

## Types of Recursion:

### 1. Direct Recursion
Method calls itself directly (all examples above)

### 2. Indirect Recursion
Method A calls Method B, which calls Method A
```csharp
public void MethodA(int n)
{
    if (n <= 0) return;
    Console.WriteLine("A: " + n);
    MethodB(n - 1);
}

public void MethodB(int n)
{
    if (n <= 0) return;
    Console.WriteLine("B: " + n);
    MethodA(n - 1);
}
```

### 3. Tail Recursion
Recursive call is the last operation
```csharp
// Tail recursive (more efficient)
public int FactorialTail(int n, int accumulator = 1)
{
    if (n <= 1)
        return accumulator;

    // Recursive call is the last operation
    return FactorialTail(n - 1, n * accumulator);
}
```

## Practical Example: Directory Traversal

```csharp
using System.IO;

public void ListAllFiles(string path, int indent = 0)
{
    // Base case: path doesn't exist
    if (!Directory.Exists(path) && !File.Exists(path))
        return;

    // Print current directory/file with indentation
    string indentation = new string(' ', indent * 2);
    Console.WriteLine($"{indentation}{Path.GetFileName(path)}");

    // If it's a directory, recurse through its contents
    if (Directory.Exists(path))
    {
        foreach (string subPath in Directory.GetFileSystemEntries(path))
        {
            ListAllFiles(subPath, indent + 1); // Recursive call
        }
    }
}
```

## Recursion vs. Iteration:

### Recursive Approach:
```csharp
public int SumRecursive(int n)
{
    if (n <= 0) return 0;
    return n + SumRecursive(n - 1);
}
```

### Iterative Approach:
```csharp
public int SumIterative(int n)
{
    int sum = 0;
    for (int i = 1; i <= n; i++)
    {
        sum += i;
    }
    return sum;
}
```

## Common Pitfalls:

### 1. Missing Base Case
```csharp
// WRONG - Infinite recursion!
public int BadRecursion(int n)
{
    return n + BadRecursion(n - 1); // No base case!
}
```

### 2. Stack Overflow
```csharp
// Potentially problematic with large n
public int ProblematicRecursion(int n)
{
    if (n <= 0) return 0;
    return 1 + ProblematicRecursion(n - 1);
}

// Better: Use iteration for large numbers
public int BetterIterative(int n)
{
    int count = 0;
    for (int i = 0; i < n; i++)
        count++;
    return count;
}
```

### 3. Inefficient Recursion
```csharp
// Fibonacci - recalculates same values many times
public int InefficientFibonacci(int n)
{
    if (n <= 1) return n;
    return InefficientFibonacci(n - 1) + InefficientFibonacci(n - 2);
}

// Better: Memoization
public class FibonacciMemo
{
    private Dictionary<int, int> memo = new Dictionary<int, int>();

    public int Calculate(int n)
    {
        if (n <= 1) return n;

        if (memo.ContainsKey(n))
            return memo[n];

        memo[n] = Calculate(n - 1) + Calculate(n - 2);
        return memo[n];
    }
}
```

## Advanced Recursive Examples:

### QuickSort
```csharp
public void QuickSort(int[] arr, int left, int right)
{
    // Base case
    if (left >= right) return;

    // Partition
    int pivot = Partition(arr, left, right);

    // Recursive calls
    QuickSort(arr, left, pivot - 1);
    QuickSort(arr, pivot + 1, right);
}

private int Partition(int[] arr, int left, int right)
{
    int pivot = arr[right];
    int i = left - 1;

    for (int j = left; j < right; j++)
    {
        if (arr[j] < pivot)
        {
            i++;
            Swap(arr, i, j);
        }
    }
    Swap(arr, i + 1, right);
    return i + 1;
}
```

### Binary Search
```csharp
public int BinarySearch(int[] arr, int target, int left, int right)
{
    // Base case: not found
    if (left > right) return -1;

    int mid = left + (right - left) / 2;

    // Base case: found
    if (arr[mid] == target) return mid;

    // Recursive cases
    if (arr[mid] > target)
        return BinarySearch(arr, target, left, mid - 1);
    else
        return BinarySearch(arr, target, mid + 1, right);
}
```

### Tower of Hanoi
```csharp
public void TowerOfHanoi(int n, char fromRod, char toRod, char auxRod)
{
    // Base case
    if (n == 1)
    {
        Console.WriteLine($"Move disk 1 from {fromRod} to {toRod}");
        return;
    }

    // Recursive: Move n-1 disks to auxiliary rod
    TowerOfHanoi(n - 1, fromRod, auxRod, toRod);

    // Move largest disk to destination
    Console.WriteLine($"Move disk {n} from {fromRod} to {toRod}");

    // Recursive: Move n-1 disks from auxiliary to destination
    TowerOfHanoi(n - 1, auxRod, toRod, fromRod);
}
```

## When to Use Recursion:

### Good for:
- **Tree/Graph structures**: Natural fit for hierarchical data
- **Divide and conquer algorithms**: QuickSort, MergeSort
- **Backtracking problems**: Sudoku solver, maze solving
- **Mathematical sequences**: Factorial, Fibonacci
- **Directory/File operations**: Traversing nested structures

### Avoid when:
- **Performance is critical**: Iterative solutions are often faster
- **Deep recursion**: Risk of stack overflow
- **Simple linear problems**: Iteration is more straightforward
- **Large input sizes**: Memory usage grows with recursion depth

## Best Practices:

1. **Always have a clear base case**
2. **Ensure progress toward base case in recursive calls**
3. **Consider tail recursion for optimization**
4. **Use memoization for overlapping subproblems**
5. **Be mindful of stack overflow with deep recursion**
6. **Sometimes iterative solutions are better**

## Debugging Recursive Methods:

```csharp
public int DebugFactorial(int n, int depth = 0)
{
    string indent = new string(' ', depth * 2);
    Console.WriteLine($"{indent}Factorial({n}) called");

    if (n <= 1)
    {
        Console.WriteLine($"{indent}Base case reached, returning 1");
        return 1;
    }

    int result = n * DebugFactorial(n - 1, depth + 1);
    Console.WriteLine($"{indent}Returning {result} for Factorial({n})");
    return result;
}
```

## Performance Considerations:

- **Space Complexity**: O(n) for call stack
- **Time Complexity**: Depends on problem structure
- **Overhead**: Method calls have performance cost
- **Optimization**: Some compilers optimize tail recursion

Recursion is a powerful tool that can lead to elegant, concise solutions, especially for problems that naturally break down into similar subproblems. Understanding when and how to use it effectively is a key skill in programming.