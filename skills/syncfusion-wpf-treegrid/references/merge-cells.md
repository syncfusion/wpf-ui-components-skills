# Merge Cells for WPF TreeGrid

## Table of Contents

- [Overview](#overview)
- [QueryCoveredRange Event](#querycoveredrange-event)
- [Column-Wise Merging](#column-wise-merging)
- [Parent Node Merging](#parent-node-merging)
- [Conditional Merging](#conditional-merging)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid supports cell merging through the QueryCoveredRange event, allowing cells with identical values to be merged.

## QueryCoveredRange Event

```csharp
treeGrid.QueryCoveredRange += (s, e) =>
{
    // Merge logic here
};
```

### TreeGridCoveredCellInfo

```csharp
public class TreeGridCoveredCellInfo
{
    public int Left { get; set; }      // Start column index
    public int Right { get; set; }     // End column index
    public int Top { get; set; }       // Start row index
    public int Bottom { get; set; }    // End row index
}
```

## Column-Wise Merging

### Merge Cells in Specific Column

```csharp
treeGrid.QueryCoveredRange += (s, e) =>
{
    var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(e.GridColumn.MappingName);
    
    // Only merge cells in "Department" column
    if (e.GridColumn.MappingName == "Department")
    {
        var range = GetMergeRange(e.RowColumnIndex, columnIndex);
        if (range != null)
        {
            e.Range = range;
            e.Handled = true;
        }
    }
};

private TreeGridCoveredCellInfo GetMergeRange(RowColumnIndex rowColumnIndex, int columnIndex)
{
    var startRowIndex = rowColumnIndex.RowIndex;
    var endRowIndex = startRowIndex;
    
    var startValue = treeGrid.GetCellValue(startRowIndex, columnIndex);
    
    // Find contiguous cells with same value
    for (int i = startRowIndex + 1; i < treeGrid.View.Nodes.Count; i++)
    {
        var currentValue = treeGrid.GetCellValue(i, columnIndex);
        if (Equals(startValue, currentValue))
            endRowIndex = i;
        else
            break;
    }
    
    if (endRowIndex > startRowIndex)
    {
        return new TreeGridCoveredCellInfo()
        {
            Left = columnIndex,
            Right = columnIndex,
            Top = startRowIndex,
            Bottom = endRowIndex
        };
    }
    
    return null;
}
```

### Merge Multiple Columns

```csharp
private List<string> _mergingColumns = new List<string> { "Department", "Location", "Manager" };

treeGrid.QueryCoveredRange += (s, e) =>
{
    if (_mergingColumns.Contains(e.GridColumn.MappingName))
    {
        var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(e.GridColumn.MappingName);
        var range = GetMergeRange(e.RowColumnIndex, columnIndex);
        
        if (range != null)
        {
            e.Range = range;
            e.Handled = true;
        }
    }
};
```

## Parent Node Merging

### Merge Parent Node Cells

```csharp
treeGrid.QueryCoveredRange += (s, e) =>
{
    var node = treeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
    
    // Merge cells for parent nodes only
    if (node != null && node.HasChildNodes)
    {
        var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(e.GridColumn.MappingName);
        
        // Merge across all columns for parent row
        e.Range = new TreeGridCoveredCellInfo()
        {
            Left = 1, // Start from first column after expander
            Right = treeGrid.Columns.Count - 1,
            Top = e.RowColumnIndex.RowIndex,
            Bottom = e.RowColumnIndex.RowIndex
        };
        e.Handled = true;
    }
};
```

## Conditional Merging

### Merge Based on Value

```csharp
treeGrid.QueryCoveredRange += (s, e) =>
{
    if (e.GridColumn.MappingName == "Status")
    {
        var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(e.GridColumn.MappingName);
        var currentValue = treeGrid.GetCellValue(e.RowColumnIndex.RowIndex, columnIndex);
        
        // Only merge if status is "Active"
        if (currentValue?.ToString() == "Active")
        {
            var range = GetMergeRange(e.RowColumnIndex, columnIndex);
            if (range != null)
            {
                e.Range = range;
                e.Handled = true;
            }
        }
    }
};
```

### Merge Based on Level

```csharp
treeGrid.QueryCoveredRange += (s, e) =>
{
    var node = treeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
    
    // Merge cells at specific level
    if (node != null && node.Level == 0)
    {
        var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(e.GridColumn.MappingName);
        var range = GetMergeRange(e.RowColumnIndex, columnIndex);
        
        if (range != null)
        {
            e.Range = range;
            e.Handled = true;
        }
    }
};
```

## Advanced Merging

### Merge with Custom Comparison

```csharp
private bool AreValuesEqual(object value1, object value2)
{
    if (value1 == null && value2 == null)
        return true;
    
    if (value1 == null || value2 == null)
        return false;
    
    // Custom comparison logic
    var str1 = value1.ToString().Trim().ToLower();
    var str2 = value2.ToString().Trim().ToLower();
    
    return str1 == str2;
}

treeGrid.QueryCoveredRange += (s, e) =>
{
    var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(e.GridColumn.MappingName);
    var startRowIndex = e.RowColumnIndex.RowIndex;
    var endRowIndex = startRowIndex;
    
    var startValue = treeGrid.GetCellValue(startRowIndex, columnIndex);
    
    for (int i = startRowIndex + 1; i < treeGrid.View.Nodes.Count; i++)
    {
        var currentValue = treeGrid.GetCellValue(i, columnIndex);
        if (AreValuesEqual(startValue, currentValue))
            endRowIndex = i;
        else
            break;
    }
    
    if (endRowIndex > startRowIndex)
    {
        e.Range = new TreeGridCoveredCellInfo()
        {
            Left = columnIndex,
            Right = columnIndex,
            Top = startRowIndex,
            Bottom = endRowIndex
        };
        e.Handled = true;
    }
};
```

### Merge Across Columns

```csharp
treeGrid.QueryCoveredRange += (s, e) =>
{
    var node = treeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
    var employee = node?.Item as Employee;
    
    // Merge "First Name" and "Last Name" columns for specific condition
    if (employee != null && string.IsNullOrEmpty(employee.LastName))
    {
        var firstNameIndex = treeGrid.ResolveToGridVisibleColumnIndex("FirstName");
        var lastNameIndex = treeGrid.ResolveToGridVisibleColumnIndex("LastName");
        
        if (e.RowColumnIndex.ColumnIndex == firstNameIndex)
        {
            e.Range = new TreeGridCoveredCellInfo()
            {
                Left = firstNameIndex,
                Right = lastNameIndex,
                Top = e.RowColumnIndex.RowIndex,
                Bottom = e.RowColumnIndex.RowIndex
            };
            e.Handled = true;
        }
    }
};
```

## Refresh Merged Cells

```csharp
// Refresh after data changes
public void RefreshMergedCells()
{
    treeGrid.InvalidateCells();
}
```

## Best Practices

### 1. Optimize Performance

```csharp
// Cache column indices
private Dictionary<string, int> _columnIndexCache = new Dictionary<string, int>();

private int GetColumnIndex(string mappingName)
{
    if (!_columnIndexCache.ContainsKey(mappingName))
    {
        _columnIndexCache[mappingName] = treeGrid.ResolveToGridVisibleColumnIndex(mappingName);
    }
    return _columnIndexCache[mappingName];
}
```

### 2. Handle Sorting and Filtering

```csharp
// Refresh merged cells after sorting/filtering
treeGrid.SortColumnsChanged += (s, e) => treeGrid.InvalidateCells();
treeGrid.View.Filter = item => /* filter logic */;
treeGrid.View.RefreshFilter();
treeGrid.InvalidateCells();
```

### 3. Consider Edit Mode

```csharp
// Disable editing for merged cells if needed
treeGrid.CurrentCellBeginEdit += (s, e) =>
{
    var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(e.Column.MappingName);
    var cellValue = treeGrid.GetCellValue(e.RowColumnIndex.RowIndex, columnIndex);
    
    // Check if cell is part of merged range
    // Cancel edit if necessary
};
```

## Troubleshooting

### Issue: Cells Not Merging

```csharp
// Ensure e.Handled is set to true
e.Handled = true;

// Verify range is valid
Debug.WriteLine($"Merge range: Left={e.Range.Left}, Right={e.Range.Right}, Top={e.Range.Top}, Bottom={e.Range.Bottom}");

// Check if values are actually equal
var value1 = treeGrid.GetCellValue(e.Range.Top, e.Range.Left);
var value2 = treeGrid.GetCellValue(e.Range.Bottom, e.Range.Left);
Debug.WriteLine($"Value1: {value1}, Value2: {value2}, Equal: {Equals(value1, value2)}");
```

### Issue: Merged Cells Not Updating

```csharp
// Call InvalidateCells after data changes
treeGrid.InvalidateCells();

// Or refresh the entire view
treeGrid.View.Refresh();
```

### Issue: Performance Issues with Large Grids

```csharp
// Limit merging to specific columns
private HashSet<string> _mergingColumns = new HashSet<string> { "Department" };

treeGrid.QueryCoveredRange += (s, e) =>
{
    if (!_mergingColumns.Contains(e.GridColumn.MappingName))
        return;
    
    // Merge logic here
};

// Consider using virtualization
// Avoid complex comparisons in QueryCoveredRange
```
