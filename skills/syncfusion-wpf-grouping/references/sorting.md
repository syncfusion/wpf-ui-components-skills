# Sorting in Essential Grouping

This guide covers how to sort data in Essential Grouping, including basic sorting, sort direction control, and implementing custom sorting logic with IComparer.

## Overview

Essential Grouping provides powerful sorting capabilities through the `TableDescriptor.SortedColumns` collection. You can:
- Sort by one or multiple properties
- Control sort direction (ascending/descending)
- Implement custom sorting logic for specialized requirements
- Combine sorting with grouping and filtering

## Basic Sorting

### Adding Sorted Columns

To sort your data by a property, add the property name to the `TableDescriptor.SortedColumns` collection:

**C#:**
```csharp
// Sort column A
groupingEngine.TableDescriptor.SortedColumns.Add("A");

// Display the data after sorting
foreach (Record rec in groupingEngine.Table.FilteredRecords)
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
' Sort column A
groupingEngine.TableDescriptor.SortedColumns.Add("A")

' Display the data after sorting
For Each rec As Record In groupingEngine.Table.FilteredRecords
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec

' Pause
Console.ReadLine()
```

### Complete Basic Sorting Example

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

            // Display the data before sorting
            Console.WriteLine("Before Sorting:");
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

            // Sort column A
            groupingEngine.TableDescriptor.SortedColumns.Add("A");

            // Display the data after sorting
            Console.WriteLine("\nAfter Sorting by A:");
            foreach (Record rec in groupingEngine.Table.FilteredRecords)
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
        Next i

        ' Create a Grouping.Engine object
        Dim groupingEngine As New Engine()

        ' Set its data source
        groupingEngine.SetSourceList(list)

        ' Display the data before sorting
        Console.WriteLine("Before Sorting:")
        Dim rec As Record
        For Each rec In groupingEngine.Table.Records
            Dim obj As MyObject = CType(rec.GetData(), MyObject)
            If Not (obj Is Nothing) Then
                Console.WriteLine(obj)
            End If
        Next rec

        ' Pause
        Console.ReadLine()

        ' Sort column A
        groupingEngine.TableDescriptor.SortedColumns.Add("A")

        ' Display the data after sorting
        Console.WriteLine(vbLf + "After Sorting by A:")
        For Each rec In groupingEngine.Table.FilteredRecords
            Dim obj As MyObject = CType(rec.GetData(), MyObject)
            If Not (obj Is Nothing) Then
                Console.WriteLine(obj)
            End If
        Next rec

        ' Pause
        Console.ReadLine()
    End Sub
End Module
```

## Sort Direction Control

### SortedColumns.Add Method Overloads

The `TableDescriptor.SortedColumns.Add` method has three overloads:

**C#:**
```csharp
public int Add(string propertyName);
public int Add(string propertyName, ListSortDirection direction);
public int Add(SortColumnDescriptor scd);
```

**VB.NET:**
```vb
Overloads Public Function Add(propertyName As String) As Integer
Overloads Public Function Add(propertyName As String, dir As ListSortDirection) As Integer
Overloads Public Function Add(scd As SortColumnDescriptor) As Integer
```

### Specifying Sort Direction

Use the second overload to control ascending or descending order:

**C#:**
```csharp
using System.ComponentModel;

// Sort ascending (default)
groupingEngine.TableDescriptor.SortedColumns.Add("A", ListSortDirection.Ascending);

// Sort descending
groupingEngine.TableDescriptor.SortedColumns.Add("B", ListSortDirection.Descending);
```

**VB.NET:**
```vb
Imports System.ComponentModel

' Sort ascending (default)
groupingEngine.TableDescriptor.SortedColumns.Add("A", ListSortDirection.Ascending)

' Sort descending
groupingEngine.TableDescriptor.SortedColumns.Add("B", ListSortDirection.Descending)
```

### Multi-Column Sorting

Add multiple properties to sort by multiple columns:

**C#:**
```csharp
// Sort by C first (ascending), then by B (descending)
groupingEngine.TableDescriptor.SortedColumns.Add("C", ListSortDirection.Ascending);
groupingEngine.TableDescriptor.SortedColumns.Add("B", ListSortDirection.Descending);
```

The sort order follows the sequence in which columns are added.

## Custom Sorting with IComparer

### When to Use Custom Sorting

Custom sorting is useful when:
- Default sorting doesn't match your business logic
- You need case-insensitive string sorting
- Properties contain formatted data requiring special comparison
- Numeric values are stored as strings
- You need locale-specific sorting rules

### Implementing IComparer

Create a class that implements the `IComparer` interface:

**C#:**
```csharp
using System.Collections;

public class AComparer : IComparer
{
    // Implementing IComparer to define the object comparisons
    public int Compare(object x, object y)
    {
        if (x == null && y == null)
            return 0;
        else if (x == null)
            return -1;
        else if (y == null)
            return 1;
        else
        {
            int sx = 0;
            int sy = 0;
            try
            {
                // Ignoring the leading character to have numerical sorting
                sx = int.Parse(x.ToString().Substring(1));
                sy = int.Parse(y.ToString().Substring(1));
                return sy - sx;
            }
            catch
            {
                throw new ArgumentException("A must be in the form 'annnn'");
            }
        }
    }
}
```

**VB.NET:**
```vb
Imports System.Collections

Public Class AComparer
    Implements IComparer
    
    ' Implementing IComparer to define the object comparisons
    Public Function Compare(ByVal x As Object, ByVal y As Object) As Integer _
                                            Implements IComparer.Compare
        If x Is Nothing And y Is Nothing Then
            Return 0
        Else
            If x Is Nothing Then
                Return -1
            Else
                If y Is Nothing Then
                    Return 1
                Else
                    Dim sx As Integer = 0
                    Dim sy As Integer = 0
                    Try
                        ' Ignoring the leading character to have numerical sorting
                        sx = Integer.Parse(x.ToString().Substring(1))
                        sy = Integer.Parse(y.ToString().Substring(1))
                        Return sy - sx
                    Catch
                        Throw New ArgumentException("A must be in the form 'annnn'")
                    End Try
                End If
            End If
        End If
    End Function
End Class
```

### Applying Custom Comparer

Use the third `Add` overload with `SortColumnDescriptor`:

**C#:**
```csharp
// Create custom comparer
AComparer comparer = new AComparer();

// Add column to sort
groupingEngine.TableDescriptor.SortedColumns.Add("A");

// Assign custom comparer
groupingEngine.TableDescriptor.SortedColumns["A"].Comparer = comparer;
```

**VB.NET:**
```vb
' Create custom comparer
Dim comparer As New AComparer()

' Add column to sort
groupingEngine.TableDescriptor.SortedColumns.Add("A")

' Assign custom comparer
groupingEngine.TableDescriptor.SortedColumns("A").Comparer = comparer
```

### Complete Custom Sorting Example

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
                list.Add(new MyObject(r.Next(20)));
            }

            // Create a Grouping.Engine object
            Engine groupingEngine = new Engine();

            // Set its data source
            groupingEngine.SetSourceList(list);

            // Display the data before sorting
            Console.WriteLine("Before Custom Sorting:");
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

            // Custom sort column A
            // Get an IComparer object to handle the custom sorting
            AComparer comparer = new AComparer();
            groupingEngine.TableDescriptor.SortedColumns.Add("A");
            groupingEngine.TableDescriptor.SortedColumns["A"].Comparer = comparer;

            // Display the data after custom sorting
            Console.WriteLine("\nAfter Custom Sorting by A:");
            foreach (Record rec in groupingEngine.Table.FilteredRecords)
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

    public class AComparer : IComparer
    {
        public int Compare(object x, object y)
        {
            if (x == null && y == null)
                return 0;
            else if (x == null)
                return -1;
            else if (y == null)
                return 1;
            else
            {
                int sx = 0;
                int sy = 0;
                try
                {
                    // Ignoring the leading character to have numerical sorting
                    sx = int.Parse(x.ToString().Substring(1));
                    sy = int.Parse(y.ToString().Substring(1));
                    return sy - sx;
                }
                catch
                {
                    throw new ArgumentException("A must be in the form 'annnn'");
                }
            }
        }
    }
}
```

## Common Custom Comparers

### Case-Insensitive String Comparer

**C#:**
```csharp
public class CaseInsensitiveComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (x == null && y == null) return 0;
        if (x == null) return -1;
        if (y == null) return 1;

        return string.Compare(
            x.ToString(), 
            y.ToString(), 
            StringComparison.OrdinalIgnoreCase
        );
    }
}
```

### Numeric String Comparer

**C#:**
```csharp
public class NumericStringComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (x == null && y == null) return 0;
        if (x == null) return -1;
        if (y == null) return 1;

        if (int.TryParse(x.ToString(), out int nx) && 
            int.TryParse(y.ToString(), out int ny))
        {
            return nx.CompareTo(ny);
        }

        // Fallback to string comparison
        return string.Compare(x.ToString(), y.ToString());
    }
}
```

### Date String Comparer

**C#:**
```csharp
public class DateStringComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (x == null && y == null) return 0;
        if (x == null) return -1;
        if (y == null) return 1;

        if (DateTime.TryParse(x.ToString(), out DateTime dx) && 
            DateTime.TryParse(y.ToString(), out DateTime dy))
        {
            return DateTime.Compare(dx, dy);
        }

        // Fallback to string comparison
        return string.Compare(x.ToString(), y.ToString());
    }
}
```

## Common Patterns

### Pattern 1: Sort with Multiple Criteria

```csharp
// Primary sort by Department (ascending), secondary by Salary (descending)
groupingEngine.TableDescriptor.SortedColumns.Add("Department", ListSortDirection.Ascending);
groupingEngine.TableDescriptor.SortedColumns.Add("Salary", ListSortDirection.Descending);
```

### Pattern 2: Dynamic Sort Column Selection

```csharp
public void SortByColumn(string columnName, bool descending)
{
    groupingEngine.TableDescriptor.SortedColumns.Clear();
    
    ListSortDirection direction = descending ? 
        ListSortDirection.Descending : 
        ListSortDirection.Ascending;
    
    groupingEngine.TableDescriptor.SortedColumns.Add(columnName, direction);
}
```

### Pattern 3: Toggle Sort Direction

```csharp
public void ToggleSortDirection(string columnName)
{
    if (groupingEngine.TableDescriptor.SortedColumns.Contains(columnName))
    {
        SortColumnDescriptor sortDesc = groupingEngine.TableDescriptor.SortedColumns[columnName];
        
        // Toggle direction
        sortDesc.SortDirection = sortDesc.SortDirection == ListSortDirection.Ascending ?
            ListSortDirection.Descending :
            ListSortDirection.Ascending;
    }
    else
    {
        groupingEngine.TableDescriptor.SortedColumns.Add(columnName);
    }
}
```

### Pattern 4: Remove Sorting

```csharp
// Remove all sorting
groupingEngine.TableDescriptor.SortedColumns.Clear();

// Remove specific column sorting
if (groupingEngine.TableDescriptor.SortedColumns.Contains("A"))
{
    groupingEngine.TableDescriptor.SortedColumns.Remove("A");
}
```

## Sorting with Grouping

When you group data, the groups are automatically sorted by the grouped property. However, you can control the sort order of records within groups:

**C#:**
```csharp
// Group by Department
groupingEngine.TableDescriptor.GroupedColumns.Add("Department");

// Sort records within each group by Salary (descending)
groupingEngine.TableDescriptor.SortedColumns.Add("Salary", ListSortDirection.Descending);
```

**Result:** Data is grouped by Department, and within each department group, records are sorted by Salary in descending order.

## Performance Considerations

### Binary Tree Efficiency

Essential Grouping uses balanced binary trees, making sorting highly efficient:
- Sorting operations: O(n log n)
- Maintaining sort order on inserts: O(log n)

### Best Practices

1. **Apply sorting early:** Sort before accessing records for better performance
2. **Minimize sort column changes:** Changing sort criteria can be expensive for large datasets
3. **Use appropriate comparers:** Custom comparers should be efficient
4. **Combine with filtering:** Filter data before sorting to reduce the dataset size

## Troubleshooting

### Unexpected Sort Order

**Issue:** Data not sorting as expected

**Solutions:**
- Verify property names are correct and case-sensitive
- Check data types (string "10" sorts before "2")
- Use custom comparer for numeric strings
- Ensure data is not null

### Custom Comparer Not Working

**Issue:** Custom comparer not being applied

**Solutions:**
- Verify comparer is assigned to correct column
- Check that column exists in SortedColumns collection
- Ensure Compare method returns correct values (-1, 0, 1)
- Add null checks in Compare method

### Performance Issues

**Issue:** Sorting is slow with large datasets

**Solutions:**
- Profile custom comparer for efficiency
- Avoid expensive operations in Compare method
- Apply filters before sorting
- Consider caching comparison results if applicable

### ArgumentException in Custom Comparer

**Issue:** Exception thrown during comparison

**Solutions:**
- Add try-catch blocks in Compare method
- Validate data format before parsing
- Provide meaningful error messages
- Handle edge cases (null, empty strings, invalid formats)
