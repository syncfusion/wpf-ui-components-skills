# Summaries and Expression Fields

## Table of Contents
- [Overview](#overview)
- [Adding Summaries](#adding-summaries)
- [Retrieving Summary Values](#retrieving-summary-values)
- [Expression Fields](#expression-fields)
- [Algebraic Expression Support](#algebraic-expression-support)
- [Special Logical Operators](#special-logical-operators)
- [Custom Functions](#custom-functions)
- [Complete Examples](#complete-examples)

## Overview

Essential Grouping provides two powerful features for data manipulation beyond basic grouping and filtering:

**Summaries:**
- Calculate aggregates (Sum, Average, Min, Max, Count) for groups
- Maintained automatically as data changes
- Available at any group level in the hierarchy

**Expression Fields:**
- Add calculated columns based on formulas
- Use algebraic expressions with properties
- Support complex computations without modifying data classes

## Adding Summaries

### SummaryDescriptor Class

Summaries are defined using `SummaryDescriptor` objects and added to the `TableDescriptor.Summaries` collection.

**Constructor Signature:**
```csharp
SummaryDescriptor(string name, string propertyName, SummaryType summaryType)
```

**Parameters:**
- `name` - Unique identifier for the summary
- `propertyName` - Property to summarize
- `summaryType` - Type of aggregate calculation

### SummaryType Enum

Common summary types include:

- `SummaryType.Int32Aggregate` - For integer properties (Sum, Average, Min, Max, Count)
- `SummaryType.DoubleAggregate` - For double/decimal properties
- `SummaryType.BooleanAggregate` - For boolean properties
- `SummaryType.StringAggregate` - For string properties
- `SummaryType.DateTimeAggregate` - For DateTime properties

### Creating a Summary

**C#:**
```csharp
// Create a summary that computes Int32Aggregate calculations on property B
SummaryDescriptor sdBInt32Agg = new SummaryDescriptor(
    "BInt32Agg",    // Summary name
    "B",            // Property to summarize
    SummaryType.Int32Aggregate
);

// Add this summary to the Summaries collection
groupingEngine.TableDescriptor.Summaries.Add(sdBInt32Agg);
```

**VB.NET:**
```vb
' Create a summary that computes Int32Aggregate calculations on property B
Dim sdBInt32Agg As New SummaryDescriptor( _
    "BInt32Agg", _
    "B", _
    SummaryType.Int32Aggregate)

' Add this summary to the Summaries collection
groupingEngine.TableDescriptor.Summaries.Add(sdBInt32Agg)
```

### Multiple Summaries

You can add multiple summaries for different properties:

**C#:**
```csharp
// Summary for integer property
SummaryDescriptor salarySum = new SummaryDescriptor(
    "SalarySummary",
    "Salary",
    SummaryType.Int32Aggregate
);

// Summary for another property
SummaryDescriptor ageSum = new SummaryDescriptor(
    "AgeSummary",
    "Age",
    SummaryType.Int32Aggregate
);

groupingEngine.TableDescriptor.Summaries.Add(salarySum);
groupingEngine.TableDescriptor.Summaries.Add(ageSum);
```

## Retrieving Summary Values

### GetSummary Method

Once a summary is added, the GroupingEngine maintains summary values for each group. Access these values using the `GetSummary` method:

**C#:**
```csharp
// Add this using statement at the top of the file
using ISummary = Syncfusion.Collections.BinaryTree.ITreeTableSummary;

// Get summary for TopLevelGroup (entire dataset)
ISummary groupSummary = groupingEngine.Table.TopLevelGroup.GetSummary("BInt32Agg");
Int32AggregateSummary int32Summary = (Int32AggregateSummary)groupSummary;

Console.WriteLine("Whole table - Sum: {0}, Average: {1}, Maximum: {2}",
    int32Summary.Sum,
    int32Summary.Average,
    int32Summary.Maximum);
```

**VB.NET:**
```vb
' Get summary for TopLevelGroup
Dim groupSummary As Syncfusion.Collections.BinaryTree.ITreeTableSummary = _
    groupingEngine.Table.TopLevelGroup.GetSummary("BInt32Agg")
    
Dim int32Summary As Int32AggregateSummary = _
    CType(groupSummary, Int32AggregateSummary)

Console.WriteLine("Whole table - Sum: {0}, Average: {1}, Maximum: {2}", _
    int32Summary.Sum, _
    int32Summary.Average, _
    int32Summary.Maximum)
```

### Int32AggregateSummary Properties

After casting to the appropriate aggregate type, access summary values:

**Available Properties:**
- `Sum` - Total of all values
- `Average` - Mean of all values
- `Maximum` - Largest value
- `Minimum` - Smallest value
- `Count` - Number of values

### Getting Summary for Specific Groups

**C#:**
```csharp
// First, group the data
groupingEngine.TableDescriptor.GroupedColumns.Add("C");

// Get summary for a specific group
Group c1Group = groupingEngine.Table.TopLevelGroup.Groups["c1"];
ISummary c1Summary = c1Group.GetSummary("BInt32Agg");
Int32AggregateSummary c1Agg = (Int32AggregateSummary)c1Summary;

Console.WriteLine("c1-group - Sum: {0}, Average: {1}, Maximum: {2}",
    c1Agg.Sum,
    c1Agg.Average,
    c1Agg.Maximum);
```

**VB.NET:**
```vb
' Get summary for specific group
Dim c1Group As Group = groupingEngine.Table.TopLevelGroup.Groups("c1")
Dim c1Summary As Syncfusion.Collections.BinaryTree.ITreeTableSummary = _
    c1Group.GetSummary("BInt32Agg")
Dim c1Agg As Int32AggregateSummary = CType(c1Summary, Int32AggregateSummary)

Console.WriteLine("c1-group - Sum: {0}, Average: {1}, Maximum: {2}", _
    c1Agg.Sum, _
    c1Agg.Average, _
    c1Agg.Maximum)
```

### Complete Summary Example

**C#:**
```csharp
using System;
using System.Collections;
using Syncfusion.Grouping;
using ISummary = Syncfusion.Collections.BinaryTree.ITreeTableSummary;

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

            // Setup GroupingEngine
            Engine groupingEngine = new Engine();
            groupingEngine.SetSourceList(list);

            // Group by property C
            groupingEngine.TableDescriptor.GroupedColumns.Add("C");

            // Create and add summary
            SummaryDescriptor sdBInt32Agg = new SummaryDescriptor(
                "BInt32Agg",
                "B",
                SummaryType.Int32Aggregate
            );
            groupingEngine.TableDescriptor.Summaries.Add(sdBInt32Agg);

            // Get summary for entire table
            ISummary tableSummary = groupingEngine.Table.TopLevelGroup.GetSummary("BInt32Agg");
            Int32AggregateSummary tableAgg = (Int32AggregateSummary)tableSummary;
            
            Console.WriteLine("Whole table - Sum: {0}, Avg: {1}, Max: {2}",
                tableAgg.Sum,
                tableAgg.Average,
                tableAgg.Maximum);

            // Get summary for "c1" group
            Group c1Group = groupingEngine.Table.TopLevelGroup.Groups["c1"];
            if (c1Group != null)
            {
                ISummary c1Summary = c1Group.GetSummary("BInt32Agg");
                Int32AggregateSummary c1Agg = (Int32AggregateSummary)c1Summary;
                
                Console.WriteLine("c1-group - Sum: {0}, Avg: {1}, Max: {2}",
                    c1Agg.Sum,
                    c1Agg.Average,
                    c1Agg.Maximum);
            }

            Console.ReadLine();
        }
    }
}
```

## Expression Fields

### ExpressionFieldDescriptor Class

Expression fields allow you to add calculated columns to your data without modifying the original data class. These fields use algebraic expressions that reference other properties.

**Constructor Signature:**
```csharp
ExpressionFieldDescriptor(string name, string expression)
```

**Parameters:**
- `name` - Name of the calculated field
- `expression` - Algebraic formula using property references

### Creating Expression Fields

**C#:**
```csharp
// Add an expression property: MultipleOfB = 2.1 * B + 3.2
ExpressionFieldDescriptor efd = new ExpressionFieldDescriptor(
    "MultipleOfB",          // Field name
    "2.1 * [B] + 3.2"      // Expression
);

groupingEngine.TableDescriptor.ExpressionFields.Add(efd);
```

**VB.NET:**
```vb
' Add an expression property
Dim efd As New ExpressionFieldDescriptor( _
    "MultipleOfB", _
    "2.1 * [B] + 3.2")

groupingEngine.TableDescriptor.ExpressionFields.Add(efd)
```

### Accessing Expression Field Values

Use the `Record.GetValue` method to retrieve calculated values:

**C#:**
```csharp
// Display the data with expression field values
foreach (Record rec in groupingEngine.Table.FilteredRecords)
{
    object expressionValue = rec.GetValue("MultipleOfB");
    Console.WriteLine(expressionValue);
}
```

**VB.NET:**
```vb
' Display the data with expression field values
For Each rec As Record In groupingEngine.Table.FilteredRecords
    Dim expressionValue As Object = rec.GetValue("MultipleOfB")
    Console.WriteLine(expressionValue)
Next rec
```

### Complete Expression Field Example

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

            // Setup GroupingEngine
            Engine groupingEngine = new Engine();
            groupingEngine.SetSourceList(list);

            // Display original data
            Console.WriteLine("Original Data:");
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

            // Add an expression field
            ExpressionFieldDescriptor efd = new ExpressionFieldDescriptor(
                "MultipleOfB",
                "2.1 * [B] + 3.2"
            );
            groupingEngine.TableDescriptor.ExpressionFields.Add(efd);

            // Display data with expression values
            Console.WriteLine("\nExpression Field Values (2.1 * B + 3.2):");
            foreach (Record rec in groupingEngine.Table.FilteredRecords)
            {
                Console.WriteLine(rec.GetValue("MultipleOfB"));
            }

            // Pause
            Console.ReadLine();
        }
    }
}
```

## Algebraic Expression Support

### Expression Components

Expressions can be any well-formed algebraic combination of:

**Property References:**
- Property (column) names enclosed with square brackets `[]`
- Example: `[Salary]`, `[Age]`, `[Price]`

**Constants:**
- Numerical constants: `10`, `3.14`, `0.5`
- String literals enclosed in apostrophes: `'text'`

**Operators:**
The computations are performed in the following order of precedence:

1. `*`, `/` - Multiplication, division
2. `+`, `-` - Addition, subtraction
3. `<`, `>`, `=`, `<=`, `>=`, `<>` - Comparison operators
4. `match`, `like`, `in`, `between` - Special operators
5. `or`, `and` - Logical operators

### Expression Examples

**Arithmetic Calculations:**
```csharp
// Percentage calculation
new ExpressionFieldDescriptor("Percentage", "[Value] * 100");

// Complex formula
new ExpressionFieldDescriptor("Total", "[Price] * [Quantity] * (1 + [TaxRate])");

// Conditional calculation
new ExpressionFieldDescriptor("Bonus", "[Salary] * 0.10");
```

**Logical Expressions:**
```csharp
// Comparison (returns 1 for true, 0 for false)
new ExpressionFieldDescriptor("IsHighValue", "[Amount] > 1000");

// Complex logical
new ExpressionFieldDescriptor("Eligible", "([Age] >= 18) AND ([Score] > 50)");
```

### Logical Operator Behavior

Logical operators return:
- `1` if the logical expression is True
- `0` if the logical expression is False

**Example:**
```csharp
ExpressionFieldDescriptor efd = new ExpressionFieldDescriptor(
    "IsHighSalary",
    "[Salary] > 50000"
);
// Returns 1 for records where Salary > 50000, otherwise 0
```

## Special Logical Operators

### MATCH Operator

Returns 1 if there is any occurrence of the right-hand argument in the left-hand argument.

**Syntax:**
```
[PropertyName] MATCH 'searchTerm'
```

**Example:**
```csharp
ExpressionFieldDescriptor efd = new ExpressionFieldDescriptor(
    "ContainsRTR",
    "[CompanyName] MATCH 'RTR'"
);
// Returns 1 if CompanyName contains "RTR" anywhere, otherwise 0
```

### LIKE Operator

Checks if the field starts exactly as specified in the right-hand argument. Supports wildcard `*`.

**Syntax:**
```
[PropertyName] LIKE 'pattern'
```

**Patterns:**
- `'value'` - Exact match
- `'value*'` - Starts with "value"
- `'*value'` - Ends with "value"
- `'*value*'` - Contains "value"

**Examples:**
```csharp
// Exact match
new ExpressionFieldDescriptor("IsRTR", "[CompanyName] LIKE 'RTR'");

// Starts with RTR
new ExpressionFieldDescriptor("StartsRTR", "[CompanyName] LIKE 'RTR*'");

// Ends with RTR
new ExpressionFieldDescriptor("EndsRTR", "[CompanyName] LIKE '*RTR'");
```

### IN Operator

Checks if the field value is any of the values listed in the right-hand operand.

**Syntax:**
```
[PropertyName] IN {value1, value2, value3}
```

**Examples:**
```csharp
// Numeric values
new ExpressionFieldDescriptor("IsSpecialCode", "[code] IN {1, 10, 21}");
// Returns 1 if code is 1, 10, or 21

// String values
new ExpressionFieldDescriptor("IsRTRorMAS", "[CompanyName] IN {RTR, MAS}");
// Returns 1 if CompanyName is "RTR" or "MAS"
```

**Important:** Spaces are significant in the list. `{RTR,MAS}` is not the same as `{RTR, MAS}`.

### BETWEEN Operator

Checks if a date field value is between two values.

**Syntax:**
```
[PropertyName] BETWEEN {startDate, endDate}
```

**Special Tokens:**
- `TODAY` - Represents the current date
- Empty first argument - DateTime.MinValue
- Empty second argument - DateTime.MaxValue

**Examples:**
```csharp
// Specific date range
new ExpressionFieldDescriptor(
    "InDateRange",
    "[date] BETWEEN {2/25/2004, 3/2/2004}"
);

// From specific date to today
new ExpressionFieldDescriptor(
    "SinceDate",
    "[date] BETWEEN {1/1/2020, TODAY}"
);

// Up to today
new ExpressionFieldDescriptor(
    "UpToToday",
    "[date] BETWEEN {, TODAY}"
);

// From today onwards
new ExpressionFieldDescriptor(
    "FutureDate",
    "[date] BETWEEN {TODAY, }"
);
```

**Range Behavior:**
- Greater than or equal to start date (>=)
- Less than end date (<)

## Custom Functions

Essential Grouping allows you to add custom functions that can be used in expressions.

### Limitations

**Important Constraints:**
- Cannot be used in algebraic expressions
- Must be used stand-alone in a simple expression
- Expression consists only of the function name and argument list
- Arguments can be any number separated by a delimiter

### Custom Function Example

While the documentation mentions custom functions are supported, the specific implementation details for registering and using custom functions would typically involve:

1. Defining a function with a signature
2. Registering it with the GroupingEngine
3. Using it in expression field definitions

**Note:** For detailed custom function implementation, refer to Syncfusion's API documentation for the specific methods to register custom functions.

## Complete Examples

### Example 1: Department Salary Summary

**C#:**
```csharp
// Group by department
groupingEngine.TableDescriptor.GroupedColumns.Add("Department");

// Add salary summary
SummaryDescriptor salarySum = new SummaryDescriptor(
    "DeptSalary",
    "Salary",
    SummaryType.Int32Aggregate
);
groupingEngine.TableDescriptor.Summaries.Add(salarySum);

// Display summary for each department
foreach (Group dept in groupingEngine.Table.TopLevelGroup.Groups)
{
    ISummary summary = dept.GetSummary("DeptSalary");
    Int32AggregateSummary agg = (Int32AggregateSummary)summary;
    
    Console.WriteLine($"Department: {dept.Name}");
    Console.WriteLine($"  Total: {agg.Sum}");
    Console.WriteLine($"  Average: {agg.Average}");
    Console.WriteLine($"  Count: {agg.Count}");
}
```

### Example 2: Calculated Bonus Field

**C#:**
```csharp
// Add 10% bonus calculation
ExpressionFieldDescriptor bonus = new ExpressionFieldDescriptor(
    "Bonus",
    "[Salary] * 0.10"
);
groupingEngine.TableDescriptor.ExpressionFields.Add(bonus);

// Add total compensation calculation
ExpressionFieldDescriptor total = new ExpressionFieldDescriptor(
    "TotalComp",
    "[Salary] + ([Salary] * 0.10)"
);
groupingEngine.TableDescriptor.ExpressionFields.Add(total);

// Display results
foreach (Record rec in groupingEngine.Table.Records)
{
    Console.WriteLine($"Salary: {rec.GetValue("Salary")}");
    Console.WriteLine($"Bonus: {rec.GetValue("Bonus")}");
    Console.WriteLine($"Total: {rec.GetValue("TotalComp")}");
    Console.WriteLine("---");
}
```

### Example 3: Conditional Expression Fields

**C#:**
```csharp
// Flag high-value records
ExpressionFieldDescriptor highValue = new ExpressionFieldDescriptor(
    "IsHighValue",
    "[Amount] > 1000"
);
groupingEngine.TableDescriptor.ExpressionFields.Add(highValue);

// Categorize by value range
ExpressionFieldDescriptor category = new ExpressionFieldDescriptor(
    "Category",
    "([Amount] > 1000) OR ([Priority] LIKE 'High')"
);
groupingEngine.TableDescriptor.ExpressionFields.Add(category);

// Display with flags
foreach (Record rec in groupingEngine.Table.Records)
{
    Console.WriteLine($"Amount: {rec.GetValue("Amount")}, " +
                     $"High Value: {rec.GetValue("IsHighValue")}, " +
                     $"Category: {rec.GetValue("Category")}");
}
```

## Common Patterns

### Pattern 1: Running Total with Summary

```csharp
// Group by date
groupingEngine.TableDescriptor.GroupedColumns.Add("Date");

// Add daily total
SummaryDescriptor dailyTotal = new SummaryDescriptor(
    "DailyTotal",
    "Amount",
    SummaryType.DoubleAggregate
);
groupingEngine.TableDescriptor.Summaries.Add(dailyTotal);

// Calculate running total across groups
double runningTotal = 0;
foreach (Group dateGroup in groupingEngine.Table.TopLevelGroup.Groups)
{
    ISummary summary = dateGroup.GetSummary("DailyTotal");
    DoubleAggregateSummary agg = (DoubleAggregateSummary)summary;
    runningTotal += agg.Sum;
    
    Console.WriteLine($"Date: {dateGroup.Name}, Daily: {agg.Sum}, Running: {runningTotal}");
}
```

### Pattern 2: Percentage Calculation

```csharp
// Get total for percentage calculation
ISummary totalSummary = groupingEngine.Table.TopLevelGroup.GetSummary("AmountSum");
Int32AggregateSummary totalAgg = (Int32AggregateSummary)totalSummary;
double grandTotal = totalAgg.Sum;

// Calculate percentage for each group
foreach (Group g in groupingEngine.Table.TopLevelGroup.Groups)
{
    ISummary groupSummary = g.GetSummary("AmountSum");
    Int32AggregateSummary groupAgg = (Int32AggregateSummary)groupSummary;
    double percentage = (groupAgg.Sum / grandTotal) * 100;
    
    Console.WriteLine($"{g.Name}: {percentage:F2}%");
}
```

## Troubleshooting

### Summary Returns Null

**Issue:** `GetSummary` returns null

**Solutions:**
- Verify summary name matches exactly
- Check that summary was added to Summaries collection
- Ensure property name exists in data

### Expression Field Not Working

**Issue:** Expression field returns incorrect values

**Solutions:**
- Check property names in brackets `[]`
- Verify operators and precedence
- Test expression components separately
- Ensure numeric properties aren't quoted

### Wrong Aggregate Type Cast

**Issue:** InvalidCastException when casting summary

**Solutions:**
- Use correct aggregate type for data (Int32, Double, DateTime, etc.)
- Match SummaryType to property data type
- Check property actual type in data class
