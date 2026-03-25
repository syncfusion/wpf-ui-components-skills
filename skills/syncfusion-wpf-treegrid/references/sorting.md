# Sorting for WPF TreeGrid

## Overview

WPF TreeGrid supports single and multi-column sorting with custom comparers and tri-state sorting.

## Basic Sorting

```xaml
<syncfusion:SfTreeGrid AllowSorting="True" 
                       AllowTriStateSorting="True"
                       SortClickAction="DoubleClick"
                       ShowSortNumbers="True"/>
```

## Sort Click Actions

```csharp
// Single click to sort
treeGrid.SortClickAction = SortClickAction.SingleClick;

// Double click to sort
treeGrid.SortClickAction = SortClickAction.DoubleClick;
```

## Programmatic Sorting

```csharp
// Add sort descriptor
treeGrid.SortColumnDescriptions.Add(new SortColumnDescription()
{
    ColumnName = "Salary",
    SortDirection = ListSortDirection.Descending
});

// Multi-column sort
treeGrid.SortColumnDescriptions.Add(new SortColumnDescription() 
{ 
    ColumnName = "Department", 
    SortDirection = ListSortDirection.Ascending 
});
treeGrid.SortColumnDescriptions.Add(new SortColumnDescription() 
{ 
    ColumnName = "Salary", 
    SortDirection = ListSortDirection.Descending 
});

// Clear sorting
treeGrid.SortColumnDescriptions.Clear();
```

## Column-Level Sorting

```xaml
<syncfusion:TreeGridTextColumn MappingName="FirstName" 
                               AllowSorting="True"/>
<syncfusion:TreeGridTextColumn MappingName="ID" 
                               AllowSorting="False"/>
```

## Custom Sort Comparer

```csharp
public class CustomSortComparer : IComparer<object>, ISortDirection
{
    private ListSortDirection _sortDirection;
    
    public ListSortDirection SortDirection
    {
        get => _sortDirection;
        set => _sortDirection = value;
    }

    public int Compare(object x, object y)
    {
        var xValue = x?.ToString();
        var yValue = y?.ToString();

        // Custom comparison logic
        int result = string.Compare(xValue, yValue, StringComparison.OrdinalIgnoreCase);
        
        return SortDirection == ListSortDirection.Ascending ? result : -result;
    }
}

// Apply custom comparer
var column = treeGrid.Columns["FirstName"];
column.SortComparer = new CustomSortComparer();
```

## Numeric String Sorting

```csharp
public class NumericStringSortComparer : IComparer<object>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }

    public int Compare(object x, object y)
    {
        int xNum, yNum;
        
        if (int.TryParse(x?.ToString(), out xNum) && int.TryParse(y?.ToString(), out yNum))
        {
            return SortDirection == ListSortDirection.Ascending 
                ? xNum.CompareTo(yNum) 
                : yNum.CompareTo(xNum);
        }
        
        return 0;
    }
}
```

## Sorting Events

```csharp
// Before sorting
treeGrid.SortColumnsChanging += (s, e) =>
{
    if (e.AddedItems.Any(item => item.ColumnName == "ID"))
    {
        // Cancel sorting on ID column
        e.Cancel = true;
    }
};

// After sorting
treeGrid.SortColumnsChanged += (s, e) =>
{
    Debug.WriteLine($"Sorted by: {string.Join(", ", e.AddedItems.Select(i => i.ColumnName))}");
};
```

## Tri-State Sorting

```csharp
// Enable tri-state sorting (Ascending -> Descending -> Unsorted)
treeGrid.AllowTriStateSorting = true;
```

## Best Practices

### 1. Performance Optimization

```csharp
// Use custom comparers for complex sorting
// Implement IComparable in your data model for better performance
public class Employee : IComparable<Employee>
{
    public int CompareTo(Employee other)
    {
        return this.Salary.CompareTo(other.Salary);
    }
}
```

### 2. Multi-Column Sorting

```csharp
// Show sort numbers for clarity
treeGrid.ShowSortNumbers = true;
```

## Troubleshooting

### Issue: Sorting Not Working

```csharp
// Verify AllowSorting is enabled
treeGrid.AllowSorting = true;

// Check column-level setting
column.AllowSorting = true;
```

### Issue: Custom Comparer Not Applied

```csharp
// Ensure ISortDirection is implemented
public class CustomComparer : IComparer<object>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }
    // ...
}
```
