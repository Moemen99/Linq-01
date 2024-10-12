# C# Advanced Concepts: Anonymous Types, var, and dynamic

## Table of Contents
1. [Implicit Type Local Variable (var and dynamic)](#implicit-type-local-variable)
2. [Extension Methods](#extension-methods)
3. [Anonymous Types](#anonymous-types)
4. [Using object vs var for Anonymous Types](#using-object-vs-var-for-anonymous-types)

## Implicit Type Local Variable


## Extension Methods


## Anonymous Types

Anonymous types in C# provide a convenient way to encapsulate a set of read-only properties into a single object without having to explicitly define a type first.

### Creating an Anonymous Type

Instead of creating a class for one-time use:

```csharp
internal class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Salary { get; set; }
}

Employee Emp01 = new Employee() { Id = 10, Name = "Aya", Salary = 10_000 };
```

We can use an anonymous type:

```csharp
var Emp01 = new { Id = 10, Name = "Aya", Salary = 10_000 };
```

### Characteristics of Anonymous Types

1. **Immutable**: Objects created from anonymous types are read-only.
   ```csharp
   // Emp01.Salary = 20_000; // Invalid
   ```

2. **Type Name**: The compiler generates a unique name for each anonymous type.
   ```csharp
   Console.WriteLine(Emp01.GetType().Name); // Output: <>f__AnonymousType0`3
   ```

3. **ToString() Override**: Anonymous types have a custom ToString() implementation.
   ```csharp
   Console.WriteLine(Emp01); // Output: { Id = 10, Name = Aya, Salary = 10000 }
   ```

### Creating New Instances

Before C# 10:
```csharp
var Emp02 = new { Emp01.Id, Emp01.Name, Salary = 20_000 };
```

C# 10 and later:
```csharp
var Emp03 = Emp01 with { Salary = 20_000 };
```

### Type Equality and Differences

Anonymous types are considered the same type if they have:
1. Same property names (case-sensitive)
2. Same property order
3. Same property data types

```csharp
var Hamda = new { Id = 20, Name = "Hamda", Salary = 200 };
Console.WriteLine(Hamda.GetType().Name); // Same as Emp01's type name

var Emp05 = new { Id = 20, Name = "Mona" };
Console.WriteLine(Emp05.GetType().Name); // Different from Emp01's type name
```

In the case of `Emp05`, a new anonymous type is created because it has a different set of properties compared to `Emp01` and `Hamda`. The compiler generates a new type name for this structure.

## Using object vs var for Anonymous Types

While `var` is commonly used for anonymous types, you can also use `object`. However, there are important differences to consider:

```csharp
object objEmp = new { Id = 10, Name = "Aya", Salary = 10_000 };
var varEmp = new { Id = 10, Name = "Aya", Salary = 10_000 };

// Using var
Console.WriteLine(varEmp.Salary); // Works fine

// Using object
// Console.WriteLine(objEmp.Salary); // Compile-time error
Console.WriteLine((objEmp as dynamic).Salary); // Works, but loses compile-time type checking
```

Using `var`:
- Provides compile-time type checking
- Allows direct access to properties
- Infers the correct anonymous type

Using `object`:
- Loses compile-time type checking
- Requires casting or use of `dynamic` to access properties
- More flexible but less type-safe

In general, `var` is preferred for anonymous types as it provides better type safety and easier property access.

## GetType() Method

The `GetType()` method, inherited from `object`, returns the type of an object:

```csharp
int x = 5;
Console.WriteLine(x.GetType().Name); // Output: Int32
```

## Mermaid Diagram: Anonymous Type Creation Process

```mermaid
graph TD
    A[Start] --> B{Create object?}
    B -->|One-time use| C[Use Anonymous Type]
    B -->|Reusable| D[Create Class]
    C --> E[var obj = new { prop1, prop2, ... }]
    D --> F[class MyClass { ... }]
    F --> G[MyClass obj = new MyClass { ... }]
    E --> H{Same properties as existing?}
    H -->|Yes| I[Reuse existing type]
    H -->|No| J[Create new anonymous type]
    I --> K[End]
    J --> K
    G --> K
```
