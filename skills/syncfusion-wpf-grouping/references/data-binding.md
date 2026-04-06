# Data Binding in Essential Grouping

Essential Grouping lets you sort, group, and summarize data from any IList data source. This guide covers how to create custom data objects, build data sources, and bind them to the GroupingEngine.

## Overview

Essential Grouping works with any data source that:
- Implements the `IList` interface
- Contains items with public properties
- Properties can be of any type (string, int, DateTime, custom objects, etc.)

The most common approach is to use an `ArrayList` of custom objects, which this guide demonstrates.

## Creating Custom Data Classes

### Defining a Custom Object Class

To use Essential Grouping, first create a class that represents your data items. The class must have public properties that Essential Grouping can access.

**Example: MyObject Class**

**C#:**
```csharp
using System;

namespace GroupingSample
{
    public class MyObject
    {
        private string aValue;
        private string bValue;
        private string cValue;
        private string dValue;

        public MyObject(int i)
        {
            aValue = string.Format("a{0}", i);
            
            // Use digit only for numeric operations
            bValue = string.Format("{0}", i);
            cValue = string.Format("c{0}", i % 3);
            dValue = string.Format("d{0}", i % 2);
        }

        public string A
        {
            get { return aValue; }
            set { aValue = value; }
        }

        public string B
        {
            get { return bValue; }
            set { bValue = value; }
        }

        public string C
        {
            get { return cValue; }
            set { cValue = value; }
        }

        public string D
        {
            get { return dValue; }
            set { dValue = value; }
        }

        public override string ToString()
        {
            return A + "\t" + B + "\t" + C + "\t" + D;
        }
    }
}
```

**VB.NET:**
```vb
Imports System

Public Class MyObject
    Private aValue As String
    Private bValue As String
    Private cValue As String
    Private dValue As String

    Public Sub New(ByVal i As Integer)
        aValue = String.Format("a{0}", i)
        
        ' Use digit only for numeric operations
        bValue = String.Format("{0}", i)
        cValue = String.Format("c{0}", i Mod 3)
        dValue = String.Format("d{0}", i Mod 2)
    End Sub

    Public Property A() As String
        Get
            Return aValue
        End Get
        Set(ByVal Value As String)
            aValue = Value
        End Set
    End Property

    Public Property B() As String
        Get
            Return bValue
        End Get
        Set(ByVal Value As String)
            bValue = Value
        End Set
    End Property

    Public Property C() As String
        Get
            Return cValue
        End Get
        Set(ByVal Value As String)
            cValue = Value
        End Set
    End Property

    Public Property D() As String
        Get
            Return dValue
        End Get
        Set(ByVal Value As String)
            dValue = Value
        End Set
    End Property

    Public Overrides Function ToString() As String
        Return A + ControlChars.Tab + B + ControlChars.Tab + C + ControlChars.Tab + D
    End Function
End Class
```

### Key Design Considerations

**Public Properties Required:**
- Essential Grouping accesses data through public properties
- Properties must have both `get` and `set` accessors
- Property names become column identifiers in expressions

**ToString Override:**
- Override `ToString()` for convenient display and debugging
- Useful when printing records to console or logs

**Data Types:**
- Properties can be any type: string, int, double, DateTime, bool, custom objects
- Numeric properties (like property B above) should store actual numbers for proper sorting and summaries
- String properties work for text-based grouping and filtering

## Creating ArrayList Data Sources

### Basic ArrayList Creation

**C#:**
```csharp
using System;
using System.Collections;
using Syncfusion.Grouping;

namespace GroupingSample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create an ArrayList of random MyObjects
            ArrayList list = new ArrayList();
            Random r = new Random();

            for (int i = 0; i < 10; i++)
            {
                list.Add(new MyObject(r.Next(5)));
                Console.WriteLine(list[i]);
            }

            // Pause
            Console.ReadLine();
        }
    }
}
```

**VB.NET:**
```vb
Imports System
Imports System.Collections
Imports Syncfusion.Grouping

Module Module1
    Sub Main()
        ' Create an ArrayList of random MyObjects
        Dim list As New ArrayList()
        Dim r As New Random()

        Dim i As Integer
        For i = 0 To 9
            list.Add(New MyObject(r.Next(5)))
            Console.WriteLine(list(i))
        Next

        ' Pause
        Console.ReadLine()
    End Sub
End Module
```

### Alternative Data Sources

While ArrayList is common, you can use any IList implementation:

**List<T> (Generic List):**
```csharp
List<MyObject> list = new List<MyObject>();
list.Add(new MyObject(1));
list.Add(new MyObject(2));
```

**BindingList<T>:**
```csharp
BindingList<MyObject> list = new BindingList<MyObject>();
list.Add(new MyObject(1));
```

**ObservableCollection<T>:**
```csharp
ObservableCollection<MyObject> list = new ObservableCollection<MyObject>();
list.Add(new MyObject(1));
```

## Setting Data Source in GroupingEngine

### Adding Required Namespace

First, add the Grouping namespace:

**C#:**
```csharp
using Syncfusion.Grouping;
```

**VB.NET:**
```vb
Imports Syncfusion.Grouping
```

### Binding Data with SetSourceList

**C#:**
```csharp
// Create a Grouping.Engine object
Engine groupingEngine = new Engine();

// Set its data source
groupingEngine.SetSourceList(list);
```

**VB.NET:**
```vb
' Create a Grouping.Engine object
Dim groupingEngine As New Engine()

' Set its data source
groupingEngine.SetSourceList(list)
```

### Complete Binding Example

**C#:**
```csharp
using System;
using System.Collections;
using Syncfusion.Grouping;

namespace GroupingSample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create an ArrayList of random MyObjects
            ArrayList list = new ArrayList();
            Random r = new Random();

            for (int i = 0; i < 10; i++)
            {
                list.Add(new MyObject(r.Next(5)));
            }

            // Create a Grouping.Engine object
            Engine groupingEngine = new Engine();

            // Set its data source
            groupingEngine.SetSourceList(list);

            Console.WriteLine("Data bound to GroupingEngine successfully!");
            Console.ReadLine();
        }
    }
}
```

## Iterating Through Data

### Accessing Records Collection

Once data is bound, access it through the `Engine.Table.Records` collection:

**C#:**
```csharp
// Access the data directly from the Engine
foreach (Record rec in groupingEngine.Table.Records)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}

// Pause
Console.ReadLine();
```

**VB.NET:**
```vb
' Access the data directly from the Engine
Dim rec As Record
For Each rec In groupingEngine.Table.Records
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec

' Pause
Console.ReadLine()
```

### Understanding the Record Object

**Record Class:**
- Represents a single row of data in the GroupingEngine
- Provides access to the underlying data object
- Offers methods to retrieve property values

**Key Methods:**
- `GetData()` - Returns the underlying data object (cast to your type)
- `GetValue(string propertyName)` - Gets the value of a specific property

### Complete Data Binding and Iteration Example

**C#:**
```csharp
using System;
using System.Collections;
using Syncfusion.Grouping;

namespace GroupingSample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create an ArrayList of random MyObjects
            ArrayList list = new ArrayList();
            Random r = new Random();

            Console.WriteLine("Original Data:");
            for (int i = 0; i < 10; i++)
            {
                list.Add(new MyObject(r.Next(5)));
                Console.WriteLine(list[i]);
            }

            Console.WriteLine("\nPress Enter to see data from GroupingEngine...");
            Console.ReadLine();

            // Create a Grouping.Engine object
            Engine groupingEngine = new Engine();

            // Set its data source
            groupingEngine.SetSourceList(list);

            // Access the data directly from the Engine
            Console.WriteLine("\nData from GroupingEngine:");
            foreach (Record rec in groupingEngine.Table.Records)
            {
                MyObject obj = rec.GetData() as MyObject;
                if (obj != null)
                {
                    Console.WriteLine(obj);
                }
            }

            // Pause
            Console.ReadLine();
        }
    }

    public class MyObject
    {
        private string aValue;
        private string bValue;
        private string cValue;
        private string dValue;

        public MyObject(int i)
        {
            aValue = string.Format("a{0}", i);
            bValue = string.Format("{0}", i);
            cValue = string.Format("c{0}", i % 3);
            dValue = string.Format("d{0}", i % 2);
        }

        public string A
        {
            get { return aValue; }
            set { aValue = value; }
        }

        public string B
        {
            get { return bValue; }
            set { bValue = value; }
        }

        public string C
        {
            get { return cValue; }
            set { cValue = value; }
        }

        public string D
        {
            get { return dValue; }
            set { dValue = value; }
        }

        public override string ToString()
        {
            return A + "\t" + B + "\t" + C + "\t" + D;
        }
    }
}
```

## Property Access Patterns

### Using GetData() Method

The `GetData()` method returns the underlying data object:

```csharp
MyObject obj = rec.GetData() as MyObject;
```

**Best Practice:** Always check for null after casting:

```csharp
if (obj != null)
{
    // Safe to use obj
    Console.WriteLine(obj.A);
}
```

### Using GetValue() Method

To access a specific property without casting:

```csharp
object valueA = rec.GetValue("A");
object valueB = rec.GetValue("B");

Console.WriteLine($"A: {valueA}, B: {valueB}");
```

**When to use GetValue():**
- Accessing expression fields (calculated columns)
- Dynamic property access by name
- Avoiding casting when you only need specific properties

## Common Patterns

### Pattern 1: Strong-Typed Data Access

```csharp
foreach (Record rec in groupingEngine.Table.Records)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        ProcessObject(obj.A, obj.B, obj.C, obj.D);
    }
}
```

### Pattern 2: Property-Based Access

```csharp
foreach (Record rec in groupingEngine.Table.Records)
{
    string a = rec.GetValue("A")?.ToString();
    string b = rec.GetValue("B")?.ToString();
    
    ProcessData(a, b);
}
```

### Pattern 3: Dynamic Property Iteration

```csharp
string[] properties = { "A", "B", "C", "D" };

foreach (Record rec in groupingEngine.Table.Records)
{
    foreach (string propName in properties)
    {
        object value = rec.GetValue(propName);
        Console.Write($"{propName}={value}\t");
    }
    Console.WriteLine();
}
```

## Troubleshooting

### Data Not Appearing in Records Collection

**Issue:** `groupingEngine.Table.Records` is empty

**Solutions:**
- Verify `SetSourceList` was called with a populated list
- Check that the list implements IList
- Ensure items in the list are not null

### Properties Not Accessible

**Issue:** Cannot access properties by name

**Solutions:**
- Verify properties are `public`
- Ensure properties have both `get` and `set` accessors
- Check property names match exactly (case-sensitive)

### Cast Failures with GetData()

**Issue:** Casting `GetData()` result fails

**Solutions:**
- Always use `as` operator with null check
- Verify the record's actual type matches your cast
- Use `GetValue()` for property-based access without casting

### Performance with Large Datasets

**Issue:** Slow iteration over thousands of records

**Solutions:**
- Consider using `FilteredRecords` if you don't need all data
- Apply filters early to reduce the working set
- Use binary tree benefits by keeping data in the GroupingEngine
