# Getting Started with Essential Grouping

This guide covers the prerequisites, installation, deployment, and initial setup for Syncfusion Essential Grouping in WPF applications.

## What is Essential Grouping?

Essential Grouping is a 100% Native .NET library that provides comprehensive support for managing and manipulating tabular information **without dependencies on any particular UI component**. The Grouping Framework can be used in any .NET environment, including C#, VB.NET, and managed C++.

### Key Capabilities

Syncfusion Essential Grouping is a data technology that allows you to:

- **Access and manipulate data** from any IList object whose items have public properties
- **Sort items** on one or several public properties
- **Display and retrieve items** based on grouping produced through sorts
- **Include caption and summary information** on groups
- **Impose filters** on items, retrieving only items that meet your filter conditions
- **Add expression properties** to display calculated values depending on other properties

### Performance Architecture

Essential Grouping uses **balanced binary trees** as the core data structure instead of arrays. This provides significant advantages:

**Binary Tree Benefits:**
- Parent branches cache information about their children
- Position information and summary information are cached in parent branches
- Quick inserts of new records honoring any sort criteria

**Performance Characteristics:**
- **Inserting records:** Log2(n) operations
- **Removing records:** Log2(n) operations
- **Moving records:** Log2(n) operations

Compare this to linear structures like ArrayList where each operation takes O(n) operations, making Essential Grouping significantly faster for large datasets.

### Recursive Grouping

Grouping is a recursive process whereby a data source may be grouped several times. This leads to the recursive situation of groups having sub-groups and so on, enabling complex hierarchical data organization.

### Expression Support

Expressions can be any well-formed algebraic combination of:
- Property (column) names enclosed with brackets `[]`
- Numerical constants and literals
- Algebraic and logical operators (*, /, +, -, <, >, =, <=, >=, <>)
- Special operators (match, like, in, between, or, and)

## Creating a WPF Application

### Step 1: Create New Project

1. Open Microsoft Visual Studio
2. Go to **File** menu and click **New Project**
3. In the New Project dialog, select **WPF Application** template
4. Name your project (e.g., "GroupingSample")
5. Click **OK**

A WPF application will be created with the basic structure.

### Step 2: Add Essential Grouping References

Now you need to deploy Essential Grouping into your WPF application by adding the required assembly references.

## Deploying Essential Grouping

### Required Assemblies

To use Essential Grouping in your application, you need to reference the following Syncfusion assemblies:

**Core Assemblies:**
- `Syncfusion.Grouping.Base.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Wpf.dll`

### Deployment Configuration

To deploy an application that uses Syncfusion assemblies:

**Step 1: Configure Copy Local**

1. In Solution Explorer, go to the **References** folder
2. Select all Syncfusion assemblies
3. Right-click and go to **Properties**
4. Change the **Copy Local** property to `true`
5. Compile the project

This ensures that the Syncfusion assemblies are copied to the output directory (bin/debug/) along with the application executable.


**Step 2: Deploy to Target Machine**

Deploy the application executable along with the Syncfusion assemblies to the target machine. **Important:** The Syncfusion assemblies must reside in the same location as the application executable on the target machine.

## Basic GroupingEngine Setup

### Adding Required Namespaces

Add the following namespace to your code file:

**C#:**
```csharp
using Syncfusion.Grouping;
```

**VB.NET:**
```vb
Imports Syncfusion.Grouping
```

### Creating Your First Grouping Application

Here's a minimal example to get started with Essential Grouping:

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
            // Create an ArrayList data source
            ArrayList list = new ArrayList();
            list.Add(new { Name = "Item1", Value = 100 });
            list.Add(new { Name = "Item2", Value = 200 });
            list.Add(new { Name = "Item3", Value = 150 });

            // Create a Grouping.Engine object
            Engine groupingEngine = new Engine();

            // Set its data source
            groupingEngine.SetSourceList(list);

            // Access the data through Table.Records
            Console.WriteLine("Records in GroupingEngine:");
            foreach (Record rec in groupingEngine.Table.Records)
            {
                var item = rec.GetData();
                Console.WriteLine(item);
            }

            // Pause to see output
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
        ' Create an ArrayList data source
        Dim list As New ArrayList()
        list.Add(New With {.Name = "Item1", .Value = 100})
        list.Add(New With {.Name = "Item2", .Value = 200})
        list.Add(New With {.Name = "Item3", .Value = 150})

        ' Create a Grouping.Engine object
        Dim groupingEngine As New Engine()

        ' Set its data source
        groupingEngine.SetSourceList(list)

        ' Access the data through Table.Records
        Console.WriteLine("Records in GroupingEngine:")
        For Each rec As Record In groupingEngine.Table.Records
            Dim item = rec.GetData()
            Console.WriteLine(item)
        Next

        ' Pause to see output
        Console.ReadLine()
    End Sub
End Module
```

## Essential Components

### The Engine Object

The `Engine` class is the main entry point for Essential Grouping functionality:

```csharp
Engine groupingEngine = new Engine();
```

### Setting a Data Source

Use the `SetSourceList` method to bind your data:

```csharp
groupingEngine.SetSourceList(list); // list must be IList
```

**Requirements:**
- Data source must implement `IList` interface
- Items in the list must have public properties
- Properties can be of any type (string, int, DateTime, custom objects, etc.)

### Accessing Data

Access records through the `Table.Records` collection:

```csharp
foreach (Record rec in groupingEngine.Table.Records)
{
    object dataItem = rec.GetData();
    // Cast to your specific type if needed
}
```

## Next Steps

Now that you have Essential Grouping set up, you can explore its powerful features:

1. **Data Binding** - Learn how to create custom data classes and bind them to the GroupingEngine
2. **Grouping** - Organize data hierarchically by one or more properties
3. **Sorting** - Sort data with standard or custom sorting logic
4. **Filtering** - Apply filter conditions using expression syntax
5. **Summaries** - Calculate aggregates like Sum, Average, Min, Max at group levels
6. **Expression Fields** - Add calculated columns using algebraic formulas

## Troubleshooting

### Assembly Not Found Errors

**Issue:** Application throws FileNotFoundException for Syncfusion assemblies

**Solution:**
- Verify Copy Local is set to `true` for all Syncfusion references
- Ensure assemblies are in the same folder as the executable
- Check that the assembly versions match across all Syncfusion DLLs

### Data Source Not Working

**Issue:** SetSourceList doesn't populate the Engine

**Solution:**
- Verify your data source implements IList
- Check that items in the list have public properties
- Ensure properties have both get and set accessors

### Namespace Not Found

**Issue:** Cannot find Syncfusion.Grouping namespace

**Solution:**
- Add references to required Syncfusion assemblies
- Verify assemblies are for the correct .NET Framework version
- Clean and rebuild the solution
