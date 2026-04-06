# Filtering in Essential Grouping

This guide covers how to filter data in Essential Grouping using the RecordFilters collection and expression-based filter conditions.

## Overview

Essential Grouping provides powerful filtering capabilities that allow you to display only records that satisfy specific criteria. Filters use expression syntax with:
- Property names enclosed in brackets `[]`
- Logical and algebraic operators
- Special string operators (LIKE, MATCH, IN)
- Complex boolean expressions

## Filter Expressions

### Expression Components

Filter conditions are built using:

**Property References:**
- Enclose property names in square brackets: `[PropertyName]`
- Case-sensitive property names
- Must match actual property names in your data class

**Operators:**
- **Numeric:** `=`, `<`, `>`, `<=`, `>=`, `<>` (not equal)
- **String:** `LIKE`, `MATCH`
- **Logical:** `OR`, `AND`
- **Set membership:** `IN`

**Constants:**
- **Numbers:** Use directly (e.g., `10`, `3.14`)
- **Strings:** Enclose in apostrophes (e.g., `'value'`)
- **Dates:** Use string format with apostrophes

## RecordFilters Collection

### Adding Filter Conditions

Filters are added to the `TableDescriptor.RecordFilters` collection using `RecordFilterDescriptor` objects:

**C#:**
```csharp
// Create a filter descriptor
RecordFilterDescriptor rfd = new RecordFilterDescriptor("[D] LIKE 'd1'");

// Add to RecordFilters collection
groupingEngine.TableDescriptor.RecordFilters.Add(rfd);
```

**VB.NET:**
```vb
' Create a filter descriptor
Dim rfd As New RecordFilterDescriptor("[D] LIKE 'd1'")

' Add to RecordFilters collection
groupingEngine.TableDescriptor.RecordFilters.Add(rfd)
```

### Accessing Filtered Data

Use the `FilteredRecords` collection instead of `Records` to access filtered data:

**C#:**
```csharp
// Access filtered records only
foreach (Record rec in groupingEngine.Table.FilteredRecords)
{
    MyObject obj = rec.GetData() as MyObject;
    if (obj != null)
    {
        Console.WriteLine(obj);
    }
}
```

**Important:** The `FilteredRecords` collection only includes records that satisfy **all** filters in the `RecordFilters` collection (filters are combined with AND logic).

## String Filtering with LIKE

### LIKE Operator

The `LIKE` operator checks if a property value exactly matches or starts with a specified string.

**Exact Match:**
```csharp
// Filter for records where property D equals 'd1'
RecordFilterDescriptor rfd = new RecordFilterDescriptor("[D] LIKE 'd1'");
groupingEngine.TableDescriptor.RecordFilters.Add(rfd);
```

**Wildcard Matching:**
- Use `*` as a wildcard character
- `'value*'` - Starts with "value"
- `'*value'` - Ends with "value"
- `'*value*'` - Contains "value"

```csharp
// Starts with 'd'
new RecordFilterDescriptor("[D] LIKE 'd*'");

// Ends with '1'
new RecordFilterDescriptor("[D] LIKE '*1'");

// Contains 'd'
new RecordFilterDescriptor("[D] LIKE '*d*'");
```

### Complete LIKE Example

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

            // Create GroupingEngine
            Engine groupingEngine = new Engine();
            groupingEngine.SetSourceList(list);

            // Display data before filtering
            Console.WriteLine("Before Filtering:");
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

            // Filter on [D] = d1
            RecordFilterDescriptor rfd = new RecordFilterDescriptor("[D] LIKE 'd1'");
            groupingEngine.TableDescriptor.RecordFilters.Add(rfd);

            // Display data after filtering
            Console.WriteLine("\nAfter Filtering ([D] LIKE 'd1'):");
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
' Display data before filtering
Dim rec As Record
For Each rec In groupingEngine.Table.FilteredRecords
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec

' Pause
Console.ReadLine()

' Filter on [D] = d1
Dim rfd As New RecordFilterDescriptor("[D] LIKE 'd1'")
groupingEngine.TableDescriptor.RecordFilters.Add(rfd)

' Display data after filtering
For Each rec In groupingEngine.Table.FilteredRecords
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec

' Pause
Console.ReadLine()
```

## Numeric Filtering

### Comparison Operators

For numeric properties, use standard comparison operators:

**C#:**
```csharp
// Equal to
new RecordFilterDescriptor("[B] = 2");

// Greater than
new RecordFilterDescriptor("[B] > 5");

// Less than or equal
new RecordFilterDescriptor("[B] <= 10");

// Not equal
new RecordFilterDescriptor("[B] <> 0");

// Range check
new RecordFilterDescriptor("[B] >= 5 AND [B] <= 15");
```

**Important:** Use comparison operators (`=`, `<`, `>`) for numeric values, not LIKE. The LIKE operator is for string matching.

## Complex Filter Expressions

### Combining Conditions with OR

**C#:**
```csharp
// Filter for D='d1' OR B=2
RecordFilterDescriptor rfd = new RecordFilterDescriptor("[D] LIKE 'd1' OR [B] = 2");
groupingEngine.TableDescriptor.RecordFilters.Add(rfd);
```

### Combining Conditions with AND

**C#:**
```csharp
// Filter for C='c1' AND B>3
RecordFilterDescriptor rfd = new RecordFilterDescriptor("[C] LIKE 'c1' AND [B] > 3");
groupingEngine.TableDescriptor.RecordFilters.Add(rfd);
```

### Complex Expression Example

**C#:**
```csharp
// Clear existing filters
groupingEngine.TableDescriptor.RecordFilters.Clear();

// Complex filter: (D='d1' OR B=2) AND C!='c0'
RecordFilterDescriptor rfd = new RecordFilterDescriptor(
    "([D] LIKE 'd1' OR [B] = 2) AND [C] <> 'c0'"
);
groupingEngine.TableDescriptor.RecordFilters.Add(rfd);

// Display filtered records
foreach (Record rec in groupingEngine.Table.FilteredRecords)
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
' Clear existing filters
groupingEngine.TableDescriptor.RecordFilters.Clear()

' Complex filter
Dim rfd As New RecordFilterDescriptor("[D] LIKE 'd1' OR [B] = 2")
groupingEngine.TableDescriptor.RecordFilters.Add(rfd)

' Display filtered records
For Each rec In groupingEngine.Table.FilteredRecords
    Dim obj As MyObject = CType(rec.GetData(), MyObject)
    If Not (obj Is Nothing) Then
        Console.WriteLine(obj)
    End If
Next rec
```

## Managing Filters

### Clearing Filters

Remove all filters to show all records:

**C#:**
```csharp
groupingEngine.TableDescriptor.RecordFilters.Clear();
```

### Removing Specific Filters

**C#:**
```csharp
// Remove filter at index
groupingEngine.TableDescriptor.RecordFilters.RemoveAt(0);

// Remove all and add new filter
groupingEngine.TableDescriptor.RecordFilters.Clear();
groupingEngine.TableDescriptor.RecordFilters.Add(newFilter);
```

### Multiple Independent Filters

When you add multiple filters, they are combined with AND logic:

**C#:**
```csharp
// These two filters are ANDed together
groupingEngine.TableDescriptor.RecordFilters.Add(
    new RecordFilterDescriptor("[D] LIKE 'd1'")
);
groupingEngine.TableDescriptor.RecordFilters.Add(
    new RecordFilterDescriptor("[B] > 2")
);

// Equivalent to: [D] LIKE 'd1' AND [B] > 2
```

## Special Operators

### MATCH Operator

Returns 1 if there is any occurrence of the right-hand argument in the left-hand argument.

**Syntax:**
```csharp
new RecordFilterDescriptor("[PropertyName] MATCH 'searchTerm'");
```

**Example:**
```csharp
// Find records where CompanyName contains 'RTR' anywhere
new RecordFilterDescriptor("[CompanyName] MATCH 'RTR'");
```

**Result:** Returns records where CompanyName contains "RTR" as a substring.

### IN Operator

Checks if the field value is any of the values listed in the right-hand operand.

**Syntax:**
```csharp
new RecordFilterDescriptor("[PropertyName] IN {value1, value2, value3}");
```

**Example:**
```csharp
// Numeric values
new RecordFilterDescriptor("[code] IN {1, 10, 21}");

// String values (no quotes needed inside braces)
new RecordFilterDescriptor("[CompanyName] IN {RTR, MAS}");
```

**Important:** 
- Values are separated by commas
- Enclosed in braces `{}`
- Spaces are significant: `{RTR,MAS}` ≠ `{RTR, MAS}`
- String values don't need quotes inside braces

### BETWEEN Operator

Checks if a value is within a range (typically used for dates).

**Syntax:**
```csharp
new RecordFilterDescriptor("[PropertyName] BETWEEN {startValue, endValue}");
```

**Example:**
```csharp
// Date range
new RecordFilterDescriptor("[date] BETWEEN {2/25/2004, 3/2/2004}");

// Current date token
new RecordFilterDescriptor("[date] BETWEEN {TODAY, 12/31/2026}");

// Open-ended ranges
new RecordFilterDescriptor("[date] BETWEEN {, TODAY}"); // up to today
new RecordFilterDescriptor("[date] BETWEEN {TODAY, }"); // from today onwards
```

**Special Tokens:**
- `TODAY` - Represents current date
- Empty first argument - DateTime.MinValue
- Empty second argument - DateTime.MaxValue

**Range Behavior:**
- Inclusive of start date (>=)
- Exclusive of end date (<)

## Common Patterns

### Pattern 1: Dynamic Filter Builder

```csharp
public void ApplyFilter(string propertyName, string operator, object value)
{
    string expression = "";
    
    if (value is string)
    {
        expression = $"[{propertyName}] {operator} '{value}'";
    }
    else
    {
        expression = $"[{propertyName}] {operator} {value}";
    }
    
    groupingEngine.TableDescriptor.RecordFilters.Clear();
    groupingEngine.TableDescriptor.RecordFilters.Add(
        new RecordFilterDescriptor(expression)
    );
}

// Usage
ApplyFilter("Department", "LIKE", "Sales");
ApplyFilter("Salary", ">", 50000);
```

### Pattern 2: Multi-Criteria Filter

```csharp
public void ApplyMultiFilter(List<string> conditions)
{
    string combinedExpression = string.Join(" AND ", conditions);
    
    groupingEngine.TableDescriptor.RecordFilters.Clear();
    groupingEngine.TableDescriptor.RecordFilters.Add(
        new RecordFilterDescriptor(combinedExpression)
    );
}

// Usage
var conditions = new List<string>
{
    "[Department] LIKE 'Sales'",
    "[Salary] > 50000",
    "[HireDate] BETWEEN {1/1/2020, TODAY}"
};
ApplyMultiFilter(conditions);
```

### Pattern 3: Toggle Filter

```csharp
public void ToggleFilter(string expression)
{
    var existingFilter = groupingEngine.TableDescriptor.RecordFilters
        .Cast<RecordFilterDescriptor>()
        .FirstOrDefault(f => f.Expression == expression);
    
    if (existingFilter != null)
    {
        // Remove if exists
        groupingEngine.TableDescriptor.RecordFilters.Remove(existingFilter);
    }
    else
    {
        // Add if doesn't exist
        groupingEngine.TableDescriptor.RecordFilters.Add(
            new RecordFilterDescriptor(expression)
        );
    }
}
```

### Pattern 4: Search Filter

```csharp
public void ApplySearchFilter(string searchTerm, params string[] properties)
{
    if (string.IsNullOrEmpty(searchTerm))
    {
        groupingEngine.TableDescriptor.RecordFilters.Clear();
        return;
    }
    
    // Build OR expression for multiple properties
    var conditions = properties.Select(p => $"[{p}] MATCH '{searchTerm}'");
    string expression = string.Join(" OR ", conditions);
    
    groupingEngine.TableDescriptor.RecordFilters.Clear();
    groupingEngine.TableDescriptor.RecordFilters.Add(
        new RecordFilterDescriptor(expression)
    );
}

// Usage: Search in Name or Department
ApplySearchFilter("John", "Name", "Department");
```

## Filtering with Grouping

Filters are applied before grouping, so only filtered records appear in groups:

**C#:**
```csharp
// Apply filter first
groupingEngine.TableDescriptor.RecordFilters.Add(
    new RecordFilterDescriptor("[Salary] > 50000")
);

// Then group
groupingEngine.TableDescriptor.GroupedColumns.Add("Department");

// Result: Only employees with salary > 50000 are grouped by department
```

## Performance Considerations

### Filter Early

Apply filters before other operations to reduce the working dataset:

```csharp
// Efficient order
SetSourceList(data);
RecordFilters.Add(...);  // Filter first
SortedColumns.Add(...);  // Sort filtered data
GroupedColumns.Add(...); // Group filtered, sorted data
```

### Complex Expressions

- Simple filters are faster than complex expressions
- Use indexed properties when possible
- Avoid expensive string operations in filters

## Troubleshooting

### Filter Not Working

**Issue:** FilteredRecords shows all records

**Solutions:**
- Verify expression syntax (brackets around properties)
- Check property names are correct and case-sensitive
- Ensure operator is appropriate for data type (LIKE for strings, = for numbers)
- Use apostrophes around string values

### No Records After Filtering

**Issue:** FilteredRecords is empty but should have results

**Solutions:**
- Check filter expression for typos
- Verify data contains matching values
- Test filter condition in parts (break complex expressions)
- Check for case sensitivity in string comparisons

### Unexpected Filter Results

**Issue:** Wrong records appearing in FilteredRecords

**Solutions:**
- Verify AND/OR logic in complex expressions
- Check operator precedence (use parentheses)
- Ensure numeric values aren't quoted
- Check for whitespace in string values

### Expression Syntax Errors

**Issue:** Exception when adding filter

**Solutions:**
- Enclose property names in brackets `[PropertyName]`
- Enclose string values in apostrophes `'value'`
- Use correct operator for data type
- Check for unmatched brackets or quotes
- Validate property names exist in data source
