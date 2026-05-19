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
    var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(e.GridColumn));
    
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
        var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(e.GridColumn));
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
    var node = treeGrid.View.Nodes[e.RowColumnIndex.RowIndex];
                
    if (node != null && node.HasChildNodes)
    {
        int left = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(treeGrid.Columns[0]));

        int right = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(treeGrid.Columns[treeGrid.Columns.Count - 1]));

        if (e.RowColumnIndex.ColumnIndex == left)
        {
            e.Range = new TreeGridCoveredCellInfo(
                            left,
                            right,
                            e.RowColumnIndex.RowIndex
                        );

            e.Handled = true;
        }
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
        var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(e.GridColumn));
        var node = treeGrid.View.Nodes[e.RowColumnIndex.RowIndex];
        var record = node?.Item;
        var currentValue = record?.GetType().GetProperty(e.GridColumn.MappingName)?.GetValue(record);
        
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
        var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(e.GridColumn));
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
    var columnIndex = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(e.GridColumn));
    var startRowIndex = e.RowColumnIndex.RowIndex;
    var endRowIndex = startRowIndex;

    var startNode = treeGrid.View.Nodes[startRowIndex];
    var startRecord = startNode?.Item;

    var startValue = startRecord?.GetType().GetProperty(e.GridColumn.MappingName)?.GetValue(startRecord);

    for (int i = startRowIndex + 1; i < treeGrid.View.Nodes.Count; i++)
    {
        var node = treeGrid.View.Nodes[i];
        var record = node?.Item;

        var currentValue = record?.GetType().GetProperty(e.GridColumn.MappingName)?.GetValue(record);
        if (AreValuesEqual(startValue, currentValue))
            endRowIndex = i;
        else
            break;
    }

    if (endRowIndex > startRowIndex)
    {
        e.Range = new TreeGridCoveredCellInfo(
        columnIndex, columnIndex,startRowIndex );
        e.Handled = true;
    }
};
```

### Merge Across Columns

```csharp
treeGrid.QueryCoveredRange += (s, e) =>
{
    var node = treeGrid.View.Nodes[e.RowColumnIndex.RowIndex];
    var employee = node?.Item as Employee;

    if (employee == null)
        return;
    if (string.IsNullOrEmpty(employee.LastName))
    {

        var firstNameColumn = treeGrid.Columns["FirstName"];
        var lastNameColumn = treeGrid.Columns["LastName"];

        var firstNameIndex = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(firstNameColumn));

        var lastNameIndex = treeGrid.ResolveToGridVisibleColumnIndex(treeGrid.Columns.IndexOf(lastNameColumn));

        if (e.RowColumnIndex.ColumnIndex == firstNameIndex)
        {

            e.Range = new TreeGridCoveredCellInfo(firstNameIndex, lastNameIndex, e.RowColumnIndex.RowIndex);

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
    treeGrid.InvalidateMeasure();
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
treeGrid.SortColumnsChanged += (s, e) => treeGrid.InvalidateMeasure();
treeGrid.View.Filter = item => /* filter logic */;
treeGrid.View.RefreshFilter();
treeGrid.InvalidateMeasure();
```

### 3. Consider Edit Mode

```csharp
// Disable editing for merged cells if needed
treeGrid.CurrentCellBeginEdit += (s, e) =>
{
    var colIndex = treeGrid.Columns.IndexOf(e.Column);
    var visibleColumnIndex = treeGrid.ResolveToGridVisibleColumnIndex(colIndex);
    var node = treeGrid.View.Nodes[e.RowColumnIndex.RowIndex];
    var record = node?.Item;
    var cellValue = record?.GetType().GetProperty(e.Column.MappingName)?.GetValue(record);
    
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
Debug.WriteLine($"Merge range: Left={e.Range.Left}, Right={e.Range.Right}");

// Check if values are actually equal
var value1 = treeGrid.View.Nodes[e.Range.RowIndex].Item?.GetType().GetProperty(column.MappingName)?.GetValue(record);
var nextIndex = e.Range.RowIndex + 1;
var value2 = treeGrid.View.Nodes[nextIndex].Item?.GetType().GetProperty(column.MappingName)?.GetValue(nextRecord);
Debug.WriteLine($"Value1: {value1}, Value2: {value2}, Equal: {Equals(value1, value2)}");
```

### Issue: Merged Cells Not Updating

```csharp
// Call InvalidateMeasure after data changes
treeGrid.InvalidateMeasure();

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
