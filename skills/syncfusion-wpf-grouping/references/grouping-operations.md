# Grouping Operations

## Table of Contents
- [Overview](#overview)
- [Grouping a Table](#grouping-a-table)
- [The TableDescriptor Class](#the-tabledescriptor-class)
- [Accessing Particular Groups](#accessing-particular-groups)
- [Recursive Group Navigation](#recursive-group-navigation)
- [Working with Nested Groups](#working-with-nested-groups)
- [Complete Examples](#complete-examples)

## Overview

Grouping data is a fundamental data analysis technique that organizes information into logical categories. For example, you might want to group sales details by month to analyze total sales on a month-by-month basis, or group employees by department to understand organizational structure.

Essential Grouping provides a powerful, recursive grouping mechanism that allows you to:
- Group data by one or multiple properties
- Create nested group hierarchies
- Access specific groups by their values
- Navigate through group structures programmatically
- Combine grouping with sorting, filtering, and summaries

## Grouping a Table

### Understanding the Engine Structure

The `Engine` object has two key properties for working with grouped data:

**Engine.Table:**
- Holds the actual data needed by Essential Grouping
- Provides access to `Records`, `FilteredRecords`, and `TopLevelGroup`
- Represents the organized data structure

**Engine.TableDescriptor:**
- Holds schema information associated with the data
- Contains collections that define how data should be organized
- Defines grouping, sorting, filtering, and summary rules

### The Columns and Properties Relationship

The `TableDescriptor.Columns` property holds a collection of `ColumnDescriptor` objects that define schema information on the columns in your data.

**Important:** Columns correspond to the public properties in your data class. For example, if you have a class with properties A, B, C, and D, these become the columns that you can reference in grouping operations.

### Applying Basic Grouping

To group data by a property, simply add the property name to the `TableDescriptor.GroupedColumns` collection:

**C#:**
```csharp
// Group on property C
groupingEngine.TableDescriptor.GroupedColumns.Add("C");

// Display the records in the engine after grouping
foreach (Record rec in groupingEngine.Table.Records)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}
```

**VB.NET:**
```vb
' Group on property C
groupingEngine.TableDescriptor.GroupedColumns.Add("C")

' Display the records in the engine after grouping
For Each rec As Record In groupingEngine.Table.Records
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec
```

**Side Effect:** When you group by a property, the data is automatically **sorted** by that property as well. This is a natural consequence of the grouping process.

## The TableDescriptor Class

The `TableDescriptor` is the central location for schema information that controls how your data is organized and processed. Understanding its collections is crucial for effective use of Essential Grouping.

### Key Collections

| Collection | Description | Purpose |
|------------|-------------|---------|
| **SortedColumns** | Holds sorted properties | Controls the order in which records appear |
| **GroupedColumns** | Holds grouped properties | Defines hierarchical grouping structure |
| **Summaries** | Holds summary definitions | Specifies aggregate calculations (Sum, Average, etc.) |
| **RecordFilters** | Holds filter conditions | Determines which records are included |
| **ExpressionFields** | Holds calculated columns | Adds computed properties based on expressions |

### How Collections Work Together

1. **Data Binding:** Data is loaded via `SetSourceList`
2. **Filtering:** `RecordFilters` determines which records are included
3. **Expression Fields:** `ExpressionFields` adds calculated properties
4. **Sorting:** `SortedColumns` orders the data
5. **Grouping:** `GroupedColumns` organizes data hierarchically
6. **Summaries:** `Summaries` calculates aggregates for each group

## Accessing Particular Groups

### Understanding Group Hierarchy

Grouping is a **recursive process** where data sources can be grouped multiple times, leading to groups having sub-groups and so on.

**Key Concept:** There is always a primary starting point called the **TopLevelGroup**.

```csharp
Group topGroup = groupingEngine.Table.TopLevelGroup;
```

The `TopLevelGroup` is the root of the entire group hierarchy and serves as your entry point for navigating grouped data.

### Group Structure: Groups vs Records

The `Group` class has two important properties:

**Group.Groups:**
- A collection of child `Group` objects
- Contains sub-groups when grouping is nested
- Populated when the group has further divisions

**Group.Records:**
- A collection of `Record` objects
- Contains actual data records
- Populated when the group is terminal (leaf node)

**Critical Rule:** At most ONE of these collections will be populated:
- If `Groups` is populated → The group has sub-groups (no records directly)
- If `Records` is populated → Terminal group with data (no sub-groups)

### Accessing a Specific Group

You can access a specific group by its value using dictionary-style indexing:

**C#:**
```csharp
// Get the Group associated with the value "c1"
Group g = groupingEngine.Table.TopLevelGroup.Groups["c1"];
```

**VB.NET:**
```vb
' Get the Group associated with the value "c1"
Dim g As Group = groupingEngine.Table.TopLevelGroup.Groups("c1")
```

This retrieves the group where property C has the value "c1".

## Recursive Group Navigation

### Creating a Recursive Display Method

To display all records in a group structure, use a recursive method that handles both terminal groups (with records) and parent groups (with sub-groups):

**C#:**
```csharp
private static void ShowRecordsUnderGroup(Group g)
{
    if (g.Records != null && g.Records.Count > 0)
    {
        // Displaying the data for all the records in each group
        foreach (Record rec in g.Records)
        {
            MyObject obj = rec.GetData() as MyObject;
            if (obj != null)
            {
                Console.WriteLine(obj);
            }
        }
        Console.WriteLine("--");
    }
    else if (g.Groups != null && g.Groups.Count > 0)
    {
        // Iterating through the groups
        foreach (Group g1 in g.Groups)
        {
            // Recursive call
            ShowRecordsUnderGroup(g1);
        }
    }
}
```

**VB.NET:**
```vb
Private Sub ShowRecordsUnderGroup(ByVal g As Group)
    If Not (g.Records Is Nothing) And g.Records.Count > 0 Then
        Dim rec As Record
        
        ' Displaying the data for all the records in each group
        For Each rec In g.Records
            Dim obj As MyObject = CType(rec.GetData(), MyObject)
            If Not (obj Is Nothing) Then
                Console.WriteLine(obj)
            End If
        Next rec
        Console.WriteLine("--")
    Else
        If Not (g.Groups Is Nothing) And g.Groups.Count > 0 Then
            Dim g1 As Group
            
            ' Iterating through the groups
            For Each g1 In g.Groups
                ' Recursive call
                ShowRecordsUnderGroup(g1)
            Next g1
        End If
    End If
End Sub
```

### Using the Recursive Method

**Display a Specific Group:**
```csharp
// Get the Group associated with the value "c1"
Group g = groupingEngine.Table.TopLevelGroup.Groups["c1"];
ShowRecordsUnderGroup(g);

// Pause
Console.ReadLine();
```

**Display All Groups:**
```csharp
// Show all records under the TopLevelGroup
ShowRecordsUnderGroup(groupingEngine.Table.TopLevelGroup);

// Pause
Console.ReadLine();
```

## Working with Nested Groups

### Creating Multi-Level Grouping

You can group by multiple properties to create nested hierarchies:

**C#:**
```csharp
// Group by C, then by D
groupingEngine.TableDescriptor.GroupedColumns.Add("C");
groupingEngine.TableDescriptor.GroupedColumns.Add("D");
```

### Navigating Nested Groups

With nested grouping, the structure becomes:
```
TopLevelGroup
├── Group "c0" (value of property C)
│   ├── Group "d0" (value of property D)
│   │   └── Records
│   └── Group "d1"
│       └── Records
├── Group "c1"
│   ├── Group "d0"
│   │   └── Records
│   └── Group "d1"
│       └── Records
└── Group "c2"
    └── ...
```

**Accessing Nested Groups:**
```csharp
// Access the "d1" sub-group within the "c1" group
Group c1Group = groupingEngine.Table.TopLevelGroup.Groups["c1"];
Group d1Group = c1Group.Groups["d1"];

// Display records in this specific nested group
ShowRecordsUnderGroup(d1Group);
```

### Enhanced Recursive Method with Indentation

For better visualization of nested groups:

**C#:**
```csharp
private static void ShowRecordsWithIndent(Group g, int level)
{
    string indent = new string(' ', level * 2);
    
    if (g.Records != null && g.Records.Count > 0)
    {
        foreach (Record rec in g.Records)
        {
            MyObject obj = rec.GetData() as MyObject;
            if (obj != null)
            {
                Console.WriteLine(indent + obj);
            }
        }
    }
    else if (g.Groups != null && g.Groups.Count > 0)
    {
        foreach (Group g1 in g.Groups)
        {
            Console.WriteLine(indent + "Group: " + g1.Name);
            ShowRecordsWithIndent(g1, level + 1);
        }
    }
}

// Call with initial level 0
ShowRecordsWithIndent(groupingEngine.Table.TopLevelGroup, 0);
```

## Complete Examples

### Example 1: Single-Level Grouping

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
            // Create data
            ArrayList list = new ArrayList();
            Random r = new Random();
            for (int i = 0; i < 10; i++)
            {
                list.Add(new MyObject(r.Next(5)));
            }

            // Create and configure GroupingEngine
            Engine groupingEngine = new Engine();
            groupingEngine.SetSourceList(list);

            // Group on property C
            groupingEngine.TableDescriptor.GroupedColumns.Add("C");

            // Display grouped data
            Console.WriteLine("Grouped Data:");
            ShowRecordsUnderGroup(groupingEngine.Table.TopLevelGroup);

            Console.ReadLine();
        }

        private static void ShowRecordsUnderGroup(Group g)
        {
            if (g.Records != null && g.Records.Count > 0)
            {
                foreach (Record rec in g.Records)
                {
                    MyObject obj = rec.GetData() as MyObject;
                    if (obj != null)
                    {
                        Console.WriteLine(obj);
                    }
                }
                Console.WriteLine("--");
            }
            else if (g.Groups != null && g.Groups.Count > 0)
            {
                foreach (Group g1 in g.Groups)
                {
                    Console.WriteLine("Group: " + g1.Name);
                    ShowRecordsUnderGroup(g1);
                }
            }
        }
    }
}
```

### Example 2: Accessing Specific Group

**C#:**
```csharp
// Group by property C
groupingEngine.TableDescriptor.GroupedColumns.Add("C");

// Access specific group
Group c1Group = groupingEngine.Table.TopLevelGroup.Groups["c1"];

if (c1Group != null)
{
    Console.WriteLine("Records in 'c1' group:");
    ShowRecordsUnderGroup(c1Group);
}
else
{
    Console.WriteLine("Group 'c1' not found");
}
```

### Example 3: Multi-Level Grouping

**C#:**
```csharp
// Create nested grouping
groupingEngine.TableDescriptor.GroupedColumns.Add("C");
groupingEngine.TableDescriptor.GroupedColumns.Add("D");

Console.WriteLine("Nested Grouping Structure:");
ShowNestedGroups(groupingEngine.Table.TopLevelGroup, 0);

private static void ShowNestedGroups(Group g, int level)
{
    string indent = new string(' ', level * 2);
    
    if (g.Groups != null && g.Groups.Count > 0)
    {
        foreach (Group childGroup in g.Groups)
        {
            Console.WriteLine($"{indent}Group [{childGroup.Name}]");
            ShowNestedGroups(childGroup, level + 1);
        }
    }
    else if (g.Records != null)
    {
        Console.WriteLine($"{indent}Records: {g.Records.Count}");
    }
}
```

## Common Patterns

### Pattern 1: Check Group Existence

```csharp
string groupValue = "c1";
if (groupingEngine.Table.TopLevelGroup.Groups.Contains(groupValue))
{
    Group g = groupingEngine.Table.TopLevelGroup.Groups[groupValue];
    ProcessGroup(g);
}
```

### Pattern 2: Count Records Per Group

```csharp
foreach (Group g in groupingEngine.Table.TopLevelGroup.Groups)
{
    int recordCount = CountRecords(g);
    Console.WriteLine($"Group {g.Name}: {recordCount} records");
}

private static int CountRecords(Group g)
{
    if (g.Records != null)
        return g.Records.Count;
    
    int total = 0;
    if (g.Groups != null)
    {
        foreach (Group subGroup in g.Groups)
        {
            total += CountRecords(subGroup);
        }
    }
    return total;
}
```

### Pattern 3: Flatten Grouped Data

```csharp
List<MyObject> FlattenGroup(Group g)
{
    List<MyObject> results = new List<MyObject>();
    
    if (g.Records != null)
    {
        foreach (Record rec in g.Records)
        {
            MyObject obj = rec.GetData() as MyObject;
            if (obj != null)
                results.Add(obj);
        }
    }
    else if (g.Groups != null)
    {
        foreach (Group subGroup in g.Groups)
        {
            results.AddRange(FlattenGroup(subGroup));
        }
    }
    
    return results;
}
```

## Troubleshooting

### Group Not Found

**Issue:** `Groups["value"]` returns null

**Solutions:**
- Verify the group value exists in your data
- Check that grouping was applied before accessing
- Ensure property values match exactly (case-sensitive)

### Records Not Appearing in Groups

**Issue:** Group exists but has no records

**Solutions:**
- Check if filters are applied that exclude records
- Verify data was bound before grouping
- Ensure the grouped property has valid values

### Stack Overflow with Recursion

**Issue:** Recursive method causes stack overflow

**Solutions:**
- Check for circular references in data
- Verify group structure is properly formed
- Add depth limits to recursive methods for safety
