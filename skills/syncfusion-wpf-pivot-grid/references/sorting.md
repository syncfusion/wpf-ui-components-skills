# Sorting in WPF PivotGrid

## Table of Contents
- [Overview](#overview)
- [Default Sorting Behavior](#default-sorting-behavior)
- [Custom Comparers](#custom-comparers)
- [Sort Options](#sort-options)
- [Column Sorting (RowPivotsOnly Mode)](#column-sorting-rowpivotsonly-mode)
- [Multi-Column Sorting](#multi-column-sorting)
- [Sorting by Values](#sorting-by-values)
- [Sorting Events](#sorting-events)
- [Programmatic Sorting](#programmatic-sorting)

## Overview

The PivotGrid provides comprehensive sorting capabilities to organize and analyze data effectively:
- **Built-in comparers** for all data types (ascending/descending by default)
- **Custom comparers** for specialized sorting logic
- **Multi-column sorting** with Ctrl+Click
- **Sort by values** (not just labels)
- **Column sorting** in RowPivotsOnly mode
- **Sorting events** for tracking sort operations

## Default Sorting Behavior

By default, PivotGrid automatically sorts data using built-in comparers based on data types:
- **Strings:** Alphabetical order (A-Z)
- **Numbers:** Numeric order (smallest to largest)
- **Dates:** Chronological order (earliest to latest)

Click any column header to toggle between ascending and descending order.

## Custom Comparers

Implement custom sorting logic using the `IComparer` interface.

### ReverseOrderComparer Example

```csharp
using System.Collections;

public class ReverseOrderComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (x == null && y == null)
            return 0;
        else if (y == null)
            return 1;
        else if (x == null)
            return -1;
        else
            return -x.ToString().CompareTo(y.ToString());
    }
}
```

### Applying Custom Comparer

**In Code-Behind:**

```csharp
public MainWindow()
{
    InitializeComponent();
    this.Loaded += MainWindow_Loaded;
}

void MainWindow_Loaded(object sender, RoutedEventArgs e)
{
    PivotGridControl pivotGrid = new PivotGridControl();
    grid1.Children.Add(pivotGrid);
    
    // Create PivotItem with custom comparer
    PivotItem productItem = new PivotItem
    {
        FieldHeader = "Product",
        FieldMappingName = "Product",
        TotalHeader = "Total",
        Comparer = new ReverseOrderComparer()  // Apply custom comparer
    };
    
    PivotItem dateItem = new PivotItem
    {
        FieldHeader = "Date",
        FieldMappingName = "Date",
        TotalHeader = "Total"
        // Uses default comparer
    };
    
    // Add to PivotRows
    pivotGrid.PivotRows.Add(productItem);
    pivotGrid.PivotRows.Add(dateItem);
    
    // Configure columns and calculations
    // ... (PivotColumns, PivotCalculations)
    
    pivotGrid.ItemSource = ProductSales.GetSalesData();
}
```

### Custom Comparer for Numeric Strings

```csharp
public class NumericStringComparer : IComparer
{
    public int Compare(object x, object y)
    {
        // Try to parse as numbers
        if (double.TryParse(x?.ToString(), out double numX) && 
            double.TryParse(y?.ToString(), out double numY))
        {
            return numX.CompareTo(numY);
        }
        
        // Fall back to string comparison
        return string.Compare(x?.ToString(), y?.ToString());
    }
}
```

### Custom Comparer for Dates

```csharp
public class CustomDateComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (DateTime.TryParse(x?.ToString(), out DateTime dateX) && 
            DateTime.TryParse(y?.ToString(), out DateTime dateY))
        {
            return dateX.CompareTo(dateY);
        }
        
        return 0;
    }
}
```

## Sort Options

Control which columns can be sorted using the `SortOption` property.

### SortOption Enumeration

| Option | Description | Use Case |
|--------|-------------|----------|
| **All** | Enable sorting for all columns | General use, full sorting capability |
| **ColumnSorting** | Sort only value columns (exclude totals) | Focus on data values only |
| **TotalSorting** | Sort only subtotal columns | Analyze subtotal trends |
| **GrandTotalSorting** | Sort only grand total columns | Compare overall totals |
| **None** | Disable all sorting | Read-only displays |

### XAML Configuration

```xml
<syncfusion:PivotGridControl Name="pivotGrid" 
                            SortOption="All"
                            ItemSource="{Binding Source={StaticResource data}}">
    <!-- PivotRows, PivotColumns, PivotCalculations -->
</syncfusion:PivotGridControl>
```

### Code-Behind Configuration

```csharp
// Enable sorting for all columns
pivotGrid.SortOption = PivotSortOption.All;

// Sort only value columns
pivotGrid.SortOption = PivotSortOption.ColumnSorting;

// Sort only totals
pivotGrid.SortOption = PivotSortOption.TotalSorting;

// Sort only grand totals
pivotGrid.SortOption = PivotSortOption.GrandTotalSorting;

// Disable sorting
pivotGrid.SortOption = PivotSortOption.None;
```

## Column Sorting (RowPivotsOnly Mode)

In RowPivotsOnly mode, enable sorting for calculation columns using the `AllowSort` property.

### Enabling Column Sorting in XAML

```xml
<syncfusion:PivotGridControl Name="pivotGrid" 
                            SortOption="All" 
                            RowPivotsOnly="True">
    
    <syncfusion:PivotGridControl.PivotRows>
        <syncfusion:PivotItem FieldHeader="PID" 
                             FieldMappingName="PID" 
                             AllowFilter="False" />
        <syncfusion:PivotItem FieldHeader="Location" 
                             FieldMappingName="Location" 
                             AllowFilter="False" />
    </syncfusion:PivotGridControl.PivotRows>
    
    <syncfusion:PivotGridControl.PivotCalculations>
        <syncfusion:PivotComputationInfo FieldHeader="Color" 
                                        FieldName="Color" 
                                        SummaryType="DoubleTotalSum" 
                                        AllowSort="True" />
        <syncfusion:PivotComputationInfo FieldHeader="Class" 
                                        FieldName="Class" 
                                        SummaryType="DoubleTotalSum" 
                                        AllowSort="True" />
        <syncfusion:PivotComputationInfo FieldHeader="Cost" 
                                        FieldName="Cost" 
                                        SummaryType="DoubleTotalSum" 
                                        AllowSort="True" />
    </syncfusion:PivotGridControl.PivotCalculations>
</syncfusion:PivotGridControl>
```

### Enabling Column Sorting in Code-Behind

```csharp
public MainWindow()
{
    InitializeComponent();
    pivotGrid.Loaded += PivotGrid_Loaded;
}

void PivotGrid_Loaded(object sender, RoutedEventArgs e)
{
    // Enable sorting for all calculations
    for (int i = 0; i < pivotGrid.PivotCalculations.Count; i++)
    {
        pivotGrid.PivotCalculations[i].AllowSort = true;
    }
}
```

### Programmatic Column Sorting

```csharp
void PivotGrid_Loaded(object sender, RoutedEventArgs e)
{
    // Define sorted columns and directions
    List<string> sortedColumns = new List<string> { "Cost", "Units" };
    
    List<System.ComponentModel.ListSortDirection> sortDirections = 
        new List<System.ComponentModel.ListSortDirection>
        {
            System.ComponentModel.ListSortDirection.Descending,
            System.ComponentModel.ListSortDirection.Ascending
        };
    
    // Apply sorting
    pivotGrid.InternalGrid.ApplySavedValueSorts(sortedColumns, sortDirections);
}
```

## Multi-Column Sorting

Sort by multiple columns simultaneously using Ctrl+Click.

### Enabling Multi-Column Sorting

**XAML:**
```xml
<syncfusion:PivotGridControl Name="pivotGrid" 
                            SortOption="All"
                            ItemSource="{Binding Source={StaticResource data}}">
    <!-- PivotRows, PivotColumns, PivotCalculations -->
</syncfusion:PivotGridControl>
```

**Code-Behind:**
```csharp
pivotGrid.SortOption = PivotSortOption.All;
```

### Using Multi-Column Sorting

1. **First Sort:** Click a column header (e.g., "Quantity")
   - Column shows sort indicator with index "1"
   
2. **Second Sort:** Hold Ctrl and click another column (e.g., "Amount")
   - Columns show sort indicators: "1" for Quantity, "2" for Amount
   - Data is sorted: OrderBy(Quantity).ThenBy(Amount)

3. **Additional Sorts:** Continue Ctrl+Click for more columns
   - Each column shows its sort order index

4. **Clear Sort:** Click column without Ctrl to reset to single-column sort

### Multi-Column Sorting in RowPivotsOnly Mode

```xml
<syncfusion:PivotGridControl Name="pivotGrid" 
                            SortOption="All" 
                            RowPivotsOnly="True">
    <syncfusion:PivotGridControl.PivotCalculations>
        <syncfusion:PivotComputationInfo FieldHeader="Color" 
                                        FieldName="Color" 
                                        AllowSort="True" />
        <syncfusion:PivotComputationInfo FieldHeader="Cost" 
                                        FieldName="Cost" 
                                        AllowSort="True" />
        <syncfusion:PivotComputationInfo FieldHeader="Units" 
                                        FieldName="Units" 
                                        AllowSort="True" />
    </syncfusion:PivotGridControl.PivotCalculations>
</syncfusion:PivotGridControl>
```

## Sorting by Values

Sort pivot items based on aggregated values instead of labels.

### Sort Direction Property

```csharp
// Set sort direction globally
pivotGrid.SortDirection = ListSortDirection.Ascending;  // A to Z, 1-9
pivotGrid.SortDirection = ListSortDirection.Descending; // Z to A, 9-1
```

### Sorting All Columns

```csharp
pivotGrid.SortOption = PivotSortOption.All;
```
Enables sorting for all value columns including data, subtotals, and grand totals.

### Sorting Only Value Columns

```csharp
pivotGrid.SortOption = PivotSortOption.ColumnSorting;
```
Sorts only value columns, excluding subtotals and grand totals.

### Sorting Only Subtotals

```csharp
pivotGrid.SortOption = PivotSortOption.TotalSorting;
```
Sorts only subtotal columns.

### Sorting Only Grand Totals

```csharp
pivotGrid.SortOption = PivotSortOption.GrandTotalSorting;
```
Sorts only grand total columns.

## Sorting Events

Track sort operations with before/after events.

### SortBegin Event

Fired before sorting begins. Returns indexes and values of fields being sorted.

```csharp
pivotGrid.SortBegin += PivotGrid_SortBegin;

void PivotGrid_SortBegin(object sender, OnSortActionStarted e)
{
    // Get indexes before sorting
    Dictionary<int, object> indexesBeforeSort = e.List;
    
    // Log or process sort initiation
    foreach (var item in indexesBeforeSort)
    {
        Console.WriteLine($"Index: {item.Key}, Value: {item.Value}");
    }
}
```

### SortCompleted Event

Fired after sorting completes. Returns indexes and values after sort.

```csharp
pivotGrid.SortCompleted += PivotGrid_SortCompleted;

void PivotGrid_SortCompleted(object sender, OnSortActionCompleted e)
{
    // Get indexes after sorting
    Dictionary<int, object> indexesAfterSort = e.List;
    
    // Log or process sort completion
    foreach (var item in indexesAfterSort)
    {
        Console.WriteLine($"Index: {item.Key}, Value: {item.Value}");
    }
}
```

### Complete Event Example

```csharp
public MainWindow()
{
    InitializeComponent();
    this.Loaded += MainWindow_Loaded;
}

void MainWindow_Loaded(object sender, RoutedEventArgs e)
{
    PivotGridControl pivotGrid = new PivotGridControl();
    grid1.Children.Add(pivotGrid);
    
    // Configure PivotGrid
    // ... (PivotRows, PivotColumns, PivotCalculations)
    
    // Subscribe to sorting events
    pivotGrid.SortBegin += PivotGrid_SortBegin;
    pivotGrid.SortCompleted += PivotGrid_SortCompleted;
    
    pivotGrid.ItemSource = ProductSales.GetSalesData();
}

void PivotGrid_SortBegin(object sender, OnSortActionStarted e)
{
    Dictionary<int, object> indexesBeforeSort = e.List;
    // Process before sort
}

void PivotGrid_SortCompleted(object sender, OnSortActionCompleted e)
{
    Dictionary<int, object> indexesAfterSort = e.List;
    // Process after sort
}
```

## Programmatic Sorting

### Sort by Specific Column

```csharp
// In RowPivotsOnly mode
List<string> columnsToSort = new List<string> { "Amount" };
List<ListSortDirection> directions = new List<ListSortDirection>
{
    ListSortDirection.Descending
};

pivotGrid.InternalGrid.ApplySavedValueSorts(columnsToSort, directions);
```

### Sort by Multiple Columns

```csharp
List<string> columnsToSort = new List<string> 
{ 
    "Amount", 
    "Quantity", 
    "Cost" 
};

List<ListSortDirection> directions = new List<ListSortDirection>
{
    ListSortDirection.Descending,  // Amount descending
    ListSortDirection.Ascending,   // Quantity ascending
    ListSortDirection.Descending   // Cost descending
};

pivotGrid.InternalGrid.ApplySavedValueSorts(columnsToSort, directions);
```

### Clear Sorting

```csharp
// Reset to no sorting
pivotGrid.SortOption = PivotSortOption.None;
```

## Common Patterns

### Pattern 1: Conditional Sorting

```csharp
// Enable/disable sorting based on user role
if (userRole == "Admin")
{
    pivotGrid.SortOption = PivotSortOption.All;
}
else
{
    pivotGrid.SortOption = PivotSortOption.None;
}
```

### Pattern 2: Save and Restore Sort State

```csharp
// Save sort state
private List<string> _sortedColumns;
private List<ListSortDirection> _sortDirections;

private void SaveSortState()
{
    // Store current sort configuration
    _sortedColumns = new List<string> { "Amount", "Quantity" };
    _sortDirections = new List<ListSortDirection>
    {
        ListSortDirection.Descending,
        ListSortDirection.Ascending
    };
}

private void RestoreSortState()
{
    if (_sortedColumns != null && _sortDirections != null)
    {
        pivotGrid.InternalGrid.ApplySavedValueSorts(_sortedColumns, _sortDirections);
    }
}
```

### Pattern 3: Dynamic Sort Based on User Selection

```csharp
private void SortButton_Click(object sender, RoutedEventArgs e)
{
    string selectedColumn = columnComboBox.SelectedItem.ToString();
    ListSortDirection direction = ascendingRadio.IsChecked == true
        ? ListSortDirection.Ascending
        : ListSortDirection.Descending;
    
    List<string> columns = new List<string> { selectedColumn };
    List<ListSortDirection> directions = new List<ListSortDirection> { direction };
    
    pivotGrid.InternalGrid.ApplySavedValueSorts(columns, directions);
}
```

## Best Practices

### 1. Choose Appropriate Sort Option

Match sort option to use case:
```csharp
// For full flexibility
pivotGrid.SortOption = PivotSortOption.All;

// For read-only reports
pivotGrid.SortOption = PivotSortOption.None;

// For total analysis
pivotGrid.SortOption = PivotSortOption.TotalSorting;
```

### 2. Use Custom Comparers for Complex Data

For non-standard sorting logic:
```csharp
// Example: Sort fiscal years (FY 2023, FY 2024)
public class FiscalYearComparer : IComparer
{
    public int Compare(object x, object y)
    {
        string xYear = ExtractYear(x?.ToString());
        string yYear = ExtractYear(y?.ToString());
        return string.Compare(xYear, yYear);
    }
    
    private string ExtractYear(string fy)
    {
        return fy?.Replace("FY ", "");
    }
}
```

### 3. Enable Sorting Selectively

```csharp
// Enable sorting only for specific calculations
pivotGrid.PivotCalculations[0].AllowSort = true;  // Amount
pivotGrid.PivotCalculations[1].AllowSort = false; // Quantity (no sort)
```

## Common Issues

### Issue: Sorting Not Working

**Solutions:**
1. Verify `SortOption` is not set to `None`
2. In RowPivotsOnly mode, ensure `AllowSort = true` for calculations
3. Check that custom comparer implements IComparer correctly

### Issue: Multi-Column Sort Not Working

**Solution:** Ensure `SortOption = PivotSortOption.All`:
```csharp
pivotGrid.SortOption = PivotSortOption.All;
```

### Issue: Custom Comparer Not Applied

**Solution:** Verify comparer is assigned before data binding:
```csharp
PivotItem item = new PivotItem
{
    FieldMappingName = "Product",
    Comparer = new ReverseOrderComparer()
};
pivotGrid.PivotRows.Add(item);
// Then bind data
pivotGrid.ItemSource = data;
```

## Related Topics

- **Filtering:** Combine sorting with filtering for targeted analysis
- **Grouping Bar:** Interactive sorting through UI
- **PivotItems:** Configure sorting per dimension
