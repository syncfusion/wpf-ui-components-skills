---
name: syncfusion-wpf-grouping
description: Implements Syncfusion Essential Grouping for WPF, a data organization engine that works without UI controls. Use this when working with GroupingEngine, TableDescriptor, or organizing tabular data with hierarchical grouping, custom sorting, and filtering. Supports binary tree data structures, expression fields, summary calculations, and aggregate operations for WPF data analysis applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Essential Grouping for WPF

Syncfusion Essential Grouping is a 100% Native .NET data technology library that provides support for managing and manipulating tabular information **without dependencies on any particular UI component**. This framework can be used in any .NET environment including C#, VB.NET, and managed C++, making it ideal for scenarios where you need powerful data organization capabilities independent of visual controls.

## When to Use This Skill

Use Essential Grouping when you need to:

- **Organize and analyze tabular data** without binding to specific UI controls
- **Group data hierarchically** by one or multiple properties (e.g., sales by month, employees by department)
- **Apply complex sorting** including custom sorting logic with IComparer implementations
- **Filter records** based on expressions using properties, logical operators, and special operators (LIKE, MATCH, IN, BETWEEN)
- **Calculate summaries** at group levels (Sum, Average, Min, Max, Count)
- **Add calculated columns** using expression fields with algebraic formulas
- **Work with IList data sources** where items have public properties
- **Handle large datasets efficiently** using binary tree data structures (Log2(n) operations for insert/remove/move)
- **Create nested groupings** with sub-groups and recursive data structures

The GroupingEngine is particularly powerful when you need data manipulation capabilities that can work across different UI frameworks or in non-UI scenarios like data processing, reporting, or server-side operations.

## Core Concepts

### Architecture Overview

Essential Grouping uses **balanced binary trees** as its core data structure instead of arrays. Binary trees allow parent branches to cache information about their children, enabling:

- **Quick position lookups** with cached information
- **Fast summary calculations** cached in parent branches
- **Efficient inserts/removes/moves** taking only Log2(n) operations (vs O(n) for ArrayList)

### Key Components

**Engine**: The main GroupingEngine object that manages the entire data manipulation pipeline

**TableDescriptor**: Schema information holder with key collections:
- `GroupedColumns` - Properties to group by
- `SortedColumns` - Properties to sort by
- `Summaries` - Summary calculations to perform
- `RecordFilters` - Filter conditions to apply
- `ExpressionFields` - Calculated columns to add

**Table**: Holds the actual data with collections:
- `Records` - All records from data source
- `FilteredRecords` - Records after applying filters
- `TopLevelGroup` - Root group for accessing hierarchical data

**Groups and Records**: Recursive structure where:
- Groups contain either `Groups` (sub-groups) or `Records` (terminal data)
- Navigate hierarchically through TopLevelGroup

## Documentation and Navigation Guide

### Getting Started and Setup
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Prerequisites and compatibility (.NET 2.0 - 4.6, VS 2015+)
- Installation and required assemblies
- Deployment configuration (Copy Local, licensing)
- Creating your first GroupingEngine application
- Basic project setup and namespace imports

### Data Binding and Iteration
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- IList data source requirements
- Creating custom object classes with public properties
- Building ArrayList data sources
- Setting data source with SetSourceList
- Iterating through records using Table.Records
- GetData() method and type casting

### Grouping Operations
📄 **Read:** [references/grouping-operations.md](references/grouping-operations.md)
- Adding properties to GroupedColumns collection
- Understanding TableDescriptor collections
- Accessing the TopLevelGroup
- Navigating recursive group structures
- Retrieving specific groups by value
- Group.Groups vs Group.Records collections

### Sorting (Standard and Custom)
📄 **Read:** [references/sorting.md](references/sorting.md)
- Adding properties to SortedColumns collection
- Specifying sort direction (ListSortDirection)
- SortedColumns.Add method overloads
- Implementing custom sorting with IComparer
- Creating custom comparer classes
- Assigning comparer to SortColumnDescriptor

### Filtering with Expressions
📄 **Read:** [references/filtering.md](references/filtering.md)
- Creating RecordFilterDescriptor objects
- Filter expression syntax with brackets []
- String operators: LIKE, MATCH
- Numeric comparisons: =, <, >, <=, >=, <>
- Complex expressions with OR, AND
- Using FilteredRecords collection

### Summaries and Expression Fields
📄 **Read:** [references/summaries-and-expressions.md](references/summaries-and-expressions.md)
- Creating SummaryDescriptor objects
- SummaryType enum (Int32Aggregate, DoubleAggregate, etc.)
- Retrieving summary values with GetSummary
- Adding expression fields with ExpressionFieldDescriptor
- Algebraic expression syntax
- Special operators: MATCH, LIKE, IN, BETWEEN
- Custom functions in expressions

## Quick Start

### Basic GroupingEngine Setup

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
            // 1. Create your data source (IList with objects having public properties)
            ArrayList list = new ArrayList();
            list.Add(new Employee { Name = "John", Department = "Sales", Salary = 50000 });
            list.Add(new Employee { Name = "Jane", Department = "IT", Salary = 65000 });
            list.Add(new Employee { Name = "Bob", Department = "Sales", Salary = 55000 });
            list.Add(new Employee { Name = "Alice", Department = "IT", Salary = 70000 });

            // 2. Create GroupingEngine and set data source
            Engine groupingEngine = new Engine();
            groupingEngine.SetSourceList(list);

            // 3. Access data through Table.Records
            Console.WriteLine("All Records:");
            foreach (Record rec in groupingEngine.Table.Records)
            {
                Employee emp = rec.GetData() as Employee;
                if (emp != null)
                {
                    Console.WriteLine($"{emp.Name}, {emp.Department}, {emp.Salary}");
                }
            }

            // 4. Apply grouping
            groupingEngine.TableDescriptor.GroupedColumns.Add("Department");

            // 5. Navigate groups
            Console.WriteLine("\nGrouped by Department:");
            ShowRecordsUnderGroup(groupingEngine.Table.TopLevelGroup);

            Console.ReadLine();
        }

        static void ShowRecordsUnderGroup(Group g)
        {
            if (g.Records != null && g.Records.Count > 0)
            {
                // Terminal group - display records
                foreach (Record rec in g.Records)
                {
                    Employee emp = rec.GetData() as Employee;
                    if (emp != null)
                    {
                        Console.WriteLine($"  {emp.Name}, {emp.Department}, {emp.Salary}");
                    }
                }
            }
            else if (g.Groups != null && g.Groups.Count > 0)
            {
                // Has sub-groups - recurse
                foreach (Group subGroup in g.Groups)
                {
                    Console.WriteLine($"Group: {subGroup.Name}");
                    ShowRecordsUnderGroup(subGroup);
                }
            }
        }
    }

    public class Employee
    {
        public string Name { get; set; }
        public string Department { get; set; }
        public int Salary { get; set; }
    }
}
```

## Common Patterns

### Pattern 1: Group and Calculate Summaries

```csharp
// Group by department
groupingEngine.TableDescriptor.GroupedColumns.Add("Department");

// Add summary for salary
SummaryDescriptor salarySum = new SummaryDescriptor(
    "DeptSalaryTotal", 
    "Salary", 
    SummaryType.Int32Aggregate
);
groupingEngine.TableDescriptor.Summaries.Add(salarySum);

// Get summary for a specific group
Group itGroup = groupingEngine.Table.TopLevelGroup.Groups["IT"];
ISummary summary = itGroup.GetSummary("DeptSalaryTotal");
Int32AggregateSummary aggSummary = (Int32AggregateSummary)summary;

Console.WriteLine($"IT Department Total: {aggSummary.Sum}");
Console.WriteLine($"IT Department Average: {aggSummary.Average}");
```

### Pattern 2: Filter and Sort Data

```csharp
// Sort by salary descending
SortColumnDescriptor sortDesc = new SortColumnDescriptor(
    "Salary", 
    ListSortDirection.Descending
);
groupingEngine.TableDescriptor.SortedColumns.Add(sortDesc);

// Filter for salaries over 60000
RecordFilterDescriptor filter = new RecordFilterDescriptor("[Salary] > 60000");
groupingEngine.TableDescriptor.RecordFilters.Add(filter);

// Access filtered records
foreach (Record rec in groupingEngine.Table.FilteredRecords)
{
    Employee emp = rec.GetData() as Employee;
    Console.WriteLine($"{emp.Name}: {emp.Salary}");
}
```

### Pattern 3: Add Expression Field (Calculated Column)

```csharp
// Add a calculated bonus field (10% of salary)
ExpressionFieldDescriptor bonusField = new ExpressionFieldDescriptor(
    "Bonus", 
    "[Salary] * 0.10"
);
groupingEngine.TableDescriptor.ExpressionFields.Add(bonusField);

// Access the calculated value
foreach (Record rec in groupingEngine.Table.Records)
{
    Employee emp = rec.GetData() as Employee;
    object bonusValue = rec.GetValue("Bonus");
    Console.WriteLine($"{emp.Name}: Salary={emp.Salary}, Bonus={bonusValue}");
}
```

### Pattern 4: Custom Sorting with IComparer

```csharp
// Custom comparer for case-insensitive name sorting
public class NameComparer : IComparer
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

// Apply custom comparer
NameComparer comparer = new NameComparer();
groupingEngine.TableDescriptor.SortedColumns.Add("Name");
groupingEngine.TableDescriptor.SortedColumns["Name"].Comparer = comparer;
```

### Pattern 5: Nested Grouping (Multiple Levels)

```csharp
// Group by Department, then by Salary range
groupingEngine.TableDescriptor.GroupedColumns.Add("Department");
groupingEngine.TableDescriptor.GroupedColumns.Add("SalaryRange");

// Navigate nested groups
foreach (Group deptGroup in groupingEngine.Table.TopLevelGroup.Groups)
{
    Console.WriteLine($"Department: {deptGroup.Name}");
    
    foreach (Group salaryGroup in deptGroup.Groups)
    {
        Console.WriteLine($"  Salary Range: {salaryGroup.Name}");
        
        foreach (Record rec in salaryGroup.Records)
        {
            Employee emp = rec.GetData() as Employee;
            Console.WriteLine($"    {emp.Name}: {emp.Salary}");
        }
    }
}
```

## Key Properties and Collections

### Engine Properties
- `Table` - Holds the actual data and provides access to Records, FilteredRecords, and TopLevelGroup
- `TableDescriptor` - Schema information with collections for grouping, sorting, filtering, summaries

### TableDescriptor Collections
- `GroupedColumns` - Properties to group by (hierarchical grouping when multiple added)
- `SortedColumns` - Properties to sort by with optional custom comparers
- `RecordFilters` - Filter conditions using expression syntax
- `Summaries` - Summary calculations (Sum, Average, Min, Max, Count, etc.)
- `ExpressionFields` - Calculated columns using algebraic expressions

### Group Properties
- `Groups` - Collection of child groups (if not terminal)
- `Records` - Collection of records (if terminal group)
- `Name` - The grouping value for this group
- `GetSummary(string name)` - Retrieves summary value for this group

### Record Methods
- `GetData()` - Returns the underlying data object
- `GetValue(string propertyName)` - Gets property or expression field value

## Common Use Cases

1. **Sales Reporting**: Group sales by region/month, calculate totals and averages, filter for high-value orders
2. **Employee Management**: Group by department/role, sort by salary, calculate payroll summaries
3. **Inventory Analysis**: Group products by category, filter stock levels, calculate value summaries
4. **Data Export Preparation**: Organize data hierarchically before exporting to reports or other formats
5. **Server-Side Data Processing**: Manipulate data on server without UI dependencies
6. **Multi-Framework Applications**: Share data logic between WPF, WinForms, ASP.NET applications
7. **Custom Report Generators**: Build report data structures with grouping and summaries
8. **Data Validation Pipelines**: Filter and group data for validation and quality checks

## Required Assemblies

When deploying applications using Essential Grouping, ensure these assemblies are copied to the application folder:


- `Syncfusion.Grouping.Base.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Wpf.dll`


