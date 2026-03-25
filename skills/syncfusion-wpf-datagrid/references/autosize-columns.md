# AutoSize Columns in WPF DataGrid (SfDataGrid)

Comprehensive reference for column auto-sizing strategies, configuration, and customization in the Syncfusion WPF DataGrid control.

## Table of Contents
- [Overview](#overview)
- [ColumnSizer Types](#columnsizer-types)
- [Basic Configuration](#basic-configuration)
- [Auto-Sizing Strategies](#auto-sizing-strategies)
- [Refreshing Auto-Size Calculations](#refreshing-auto-size-calculations)
- [Font and Size Customization](#font-and-size-customization)
- [Star Column Sizing](#star-column-sizing)
- [Advanced Customization](#advanced-customization)
- [Performance Optimization](#performance-optimization)
- [Troubleshooting](#troubleshooting)

## Overview

The WPF DataGrid provides intelligent column width calculation using the `ColumnSizer` property. This allows columns to automatically adjust their widths based on content, headers, or custom logic.

**Key Points:**
- ColumnSizer is applied only when column width is not explicitly defined
- Calculations respect `MinWidth` and `MaxWidth` properties
- Column-level ColumnSizer takes priority over grid-level settings
- Can be refreshed at runtime to recalculate widths

## ColumnSizer Types

### GridLengthUnitType.Star
Divides available width equally among all Star columns.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="Star"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.Star;
```

**Use Cases:**
- Responsive layouts
- Equal column distribution
- Filling available space

### GridLengthUnitType.Auto
Calculates width based on header and cell contents to prevent truncation.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="Auto"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.Auto;
```

**Use Cases:**
- Dynamic content
- Variable-length text
- Optimal content display

### GridLengthUnitType.SizeToCells
Calculates width based only on cell contents (excludes header).

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="SizeToCells"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.SizeToCells;
```

**Use Cases:**
- Content-focused sizing
- When headers are shorter than content
- Minimizing wasted space

### GridLengthUnitType.SizeToHeader
Calculates width based only on header text.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="SizeToHeader"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.SizeToHeader;
```

**Use Cases:**
- Header-driven layout
- Consistent column widths
- Limited cell content

### GridLengthUnitType.AutoWithLastColumnFill
Auto-sizes all columns except the last visible column, which fills remaining space.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="AutoWithLastColumnFill"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.AutoWithLastColumnFill;
```

**Use Cases:**
- Flexible layouts
- Preventing horizontal scrollbar
- Last column with variable content

### GridLengthUnitType.AutoLastColumnFill
Auto-sizes all columns; last column gets maximum of auto-width and remaining space.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="AutoLastColumnFill"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.AutoLastColumnFill;
```

**Use Cases:**
- Content-aware last column
- Optimal space utilization
- Preventing content truncation

### GridLengthUnitType.None
Uses default or explicitly defined column width.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="None"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.None;
column.Width = 120;
```

**Use Cases:**
- Fixed layouts
- Manual width control
- Consistent column sizes

## Basic Configuration

### Grid-Level ColumnSizer

Apply auto-sizing to all columns:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="Auto"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.Auto;
```

### Column-Level ColumnSizer

Override grid-level setting for specific columns:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="Star"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   ColumnSizer="Auto" />
        <syncfusion:GridTextColumn MappingName="CustomerName" 
                                   ColumnSizer="SizeToCells" />
        <syncfusion:GridTextColumn MappingName="Country" />  <!-- Uses Star from grid -->
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.Star;
dataGrid.Columns["OrderID"].ColumnSizer = GridLengthUnitType.Auto;
dataGrid.Columns["CustomerName"].ColumnSizer = GridLengthUnitType.SizeToCells;
```

**Priority**: Column-level > Grid-level

### Fill Remaining Width for Specific Column

When using `AutoWithLastColumnFill` or `AutoLastColumnFill`, you can change which column fills remaining space:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="AutoWithLastColumnFill"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   ColumnSizer="AutoLastColumnFill" />  <!-- This fills instead -->
        <syncfusion:GridTextColumn MappingName="CustomerID" />
        <syncfusion:GridTextColumn MappingName="CustomerName" />
        <syncfusion:GridTextColumn MappingName="Country" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.AutoWithLastColumnFill;
dataGrid.Columns["OrderID"].ColumnSizer = GridLengthUnitType.AutoLastColumnFill;
```

## Auto-Sizing Strategies

### Combining MinWidth and MaxWidth

Control auto-size bounds with constraints:

```xaml
<syncfusion:GridTextColumn MappingName="CustomerName"
                          ColumnSizer="Auto"
                          MinimumWidth="80"
                          MaximumWidth="200" />
```

```csharp
var column = dataGrid.Columns["CustomerName"];
column.ColumnSizer = GridLengthUnitType.Auto;
column.MinimumWidth = 80;
column.MaximumWidth = 200;
```

### Mixed Sizing Strategy

Combine different sizing approaches:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <!-- Fixed width -->
        <syncfusion:GridTextColumn MappingName="OrderID" Width="100" />
        
        <!-- Auto-size to content -->
        <syncfusion:GridTextColumn MappingName="CustomerID" ColumnSizer="Auto" />
        
        <!-- Size to cells only -->
        <syncfusion:GridTextColumn MappingName="CustomerName" ColumnSizer="SizeToCells" />
        
        <!-- Star sizing (fills remaining) -->
        <syncfusion:GridTextColumn MappingName="Country" ColumnSizer="Star" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

## Refreshing Auto-Size Calculations

### Refresh All Columns

Recalculate widths for all columns:

```csharp
// After data changes
var viewModel = dataGrid.DataContext as ViewModel;
viewModel.Orders.Add(new OrderInfo(11, "BLFKI", "Very Long Customer Name That Extends Beyond"));

// Refresh calculations
dataGrid.GridColumnSizer.Refresh();
```

### Reset and Refresh Calculations

Reset auto-calculation cache before refreshing:

```csharp
// Reset all columns
dataGrid.GridColumnSizer.ResetAutoCalculationforAllColumns();
dataGrid.GridColumnSizer.Refresh();

// Reset specific column
dataGrid.GridColumnSizer.ResetAutoCalculation(dataGrid.Columns["CustomerName"]);
dataGrid.GridColumnSizer.Refresh();
```

**When to Reset:**
- After adding/removing records
- When cell content significantly changes
- After applying formatting changes
- When switching between templates

### Reset Explicit Width

Clear explicit width to enable auto-sizing:

```csharp
foreach (var column in dataGrid.Columns)
{
    if (!double.IsNaN(column.Width))
        column.Width = double.NaN;
}
dataGrid.GridColumnSizer.Refresh();
```

## Font and Size Customization

### Global Font Settings

Configure font settings used for all auto-size calculations:

```csharp
dataGrid.GridColumnSizer.FontSize = 12.0;
dataGrid.GridColumnSizer.FontFamily = new FontFamily("Arial");
dataGrid.GridColumnSizer.Margin = new Thickness(10, 5, 10, 5);
```

### Sort and Filter Icon Widths

Adjust icon widths considered in calculations:

```csharp
dataGrid.GridColumnSizer.SortIconWidth = 20;
dataGrid.GridColumnSizer.FilterIconWidth = 20;
```

### Column-Specific Font Settings

Override font settings for individual columns using attached properties:

```csharp
var gridColumn = dataGrid.Columns["OrderID"];
GridColumnSizer.SetFontFamily(gridColumn, new FontFamily("Segoe UI"));
GridColumnSizer.SetFontSize(gridColumn, 14.0);
GridColumnSizer.SetMargin(gridColumn, new Thickness(12, 6, 12, 6));
```

### Custom Font Settings via Overrides

```csharp
public class ColumnSizerExt : GridColumnSizer
{
    public ColumnSizerExt(SfDataGrid grid) : base(grid)
    {
    }
    
    protected override FormattedText GetFormattedText(GridColumn column, object record, string displayText)
    {
        var formattedText = base.GetFormattedText(column, record, displayText);
        
        // Custom font for specific column
        if (column.MappingName.Equals("OrderID"))
        {
            formattedText.SetFontFamily(new FontFamily("Courier New"));
            formattedText.SetFontSize(12);
        }
        
        return formattedText;
    }
}

// Apply custom sizer
dataGrid.GridColumnSizer = new ColumnSizerExt(dataGrid);
```

## Star Column Sizing

### Basic Star Sizing

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.Star;
```

All columns with Star sizing share available width equally.

### Star Column with Ratios

Implement custom width ratios using attached property:

**Step 1: Define Attached Property**

```csharp
public static class StarRatio
{
    public static readonly DependencyProperty ColumnRatioProperty = 
        DependencyProperty.RegisterAttached(
            "ColumnRatio", 
            typeof(int), 
            typeof(StarRatio), 
            new PropertyMetadata(1));
    
    public static int GetColumnRatio(DependencyObject obj)
    {
        return (int)obj.GetValue(ColumnRatioProperty);
    }
    
    public static void SetColumnRatio(DependencyObject obj, int value)
    {
        obj.SetValue(ColumnRatioProperty, value);
    }
}
```

**Step 2: Implement Custom GridColumnSizer**

```csharp
public class GridColumnSizerExt : GridColumnSizer
{
    public GridColumnSizerExt(SfDataGrid grid) : base(grid)
    {
    }
    
    protected override void SetStarWidth(double remainingColumnWidth, IEnumerable<GridColumn> remainingColumns)
    {
        var removedColumn = new List<GridColumn>();
        var columns = remainingColumns.ToList();
        var totalRemainingStarValue = remainingColumnWidth;
        double removedWidth = 0;
        bool isRemoved;
        
        while (columns.Count > 0)
        {
            isRemoved = false;
            removedWidth = 0;
            var columnsCount = 0;
            
            // Calculate total ratio
            columns.ForEach((col) =>
            {
                columnsCount += StarRatio.GetColumnRatio(col);
            });
            
            double starWidth = Math.Floor((totalRemainingStarValue / columnsCount));
            var column = columns.First();
            starWidth *= StarRatio.GetColumnRatio(column);
            double computedWidth = SetColumnWidth(column, starWidth);
            
            if (starWidth != computedWidth && starWidth > 0)
            {
                isRemoved = true;
                columns.Remove(column);
                
                foreach (var remColumn in removedColumn)
                {
                    if (!columns.Contains(remColumn))
                    {
                        removedWidth += remColumn.ActualWidth;
                        columns.Add(remColumn);
                    }
                }
                removedColumn.Clear();
                totalRemainingStarValue += removedWidth;
            }
            
            totalRemainingStarValue = totalRemainingStarValue - computedWidth;
            
            if (!isRemoved)
            {
                columns.Remove(column);
                if (!removedColumn.Contains(column))
                    removedColumn.Add(column);
            }
        }
    }
}
```

**Step 3: Apply Custom Sizer and Ratios**

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ColumnSizer="Star"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <!-- Ratio 1:2:3 -->
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   local:StarRatio.ColumnRatio="1" />
        <syncfusion:GridTextColumn MappingName="CustomerID" 
                                   local:StarRatio.ColumnRatio="2" />
        <syncfusion:GridTextColumn MappingName="CustomerName" 
                                   local:StarRatio.ColumnRatio="3" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.GridColumnSizer = new GridColumnSizerExt(dataGrid);
```

Result: OrderID gets 1/6, CustomerID gets 2/6, CustomerName gets 3/6 of available width.

## Advanced Customization

### Custom GridColumnSizer

Override calculation logic by creating custom GridColumnSizer:

```csharp
public class CustomColumnSizer : GridColumnSizer
{
    public CustomColumnSizer(SfDataGrid grid) : base(grid)
    {
    }
    
    // Customize cell width calculation
    protected override double CalculateCellWidth(GridColumn column, bool setWidth = true)
    {
        // Custom logic for GridComboBoxColumn
        if (column is GridComboBoxColumn)
        {
            double colWidth = double.MaxValue;
            var source = (column as GridComboBoxColumn).ItemsSource;
            string maximumComboItemsText = string.Empty;
            var clientSize = new Size(colWidth, DataGrid.RowHeight);
            
            // Find longest item text
            foreach (var comboItems in source)
            {
                string comboItemText = (string)comboItems;
                if (maximumComboItemsText.Length < comboItemText.Length)
                    maximumComboItemsText = comboItemText;
            }
            
            // Measure and add scrollbar width
            var measureSize = MeasureText(clientSize, maximumComboItemsText, column, null, GridQueryBounds.Width);
            return measureSize.Width + SystemParameters.ScrollWidth;
        }
        
        return base.CalculateCellWidth(column, setWidth);
    }
    
    // Customize header width calculation
    protected override double CalculateHeaderWidth(GridColumn column, bool setWidth = true)
    {
        // Add custom padding for specific columns
        if (column.MappingName == "OrderID")
        {
            return base.CalculateHeaderWidth(column, setWidth) + 20;
        }
        
        return base.CalculateHeaderWidth(column, setWidth);
    }
}

// Apply custom sizer
dataGrid.GridColumnSizer = new CustomColumnSizer(dataGrid);
```

### ComboBox Column Auto-Sizing

Auto-size GridComboBoxColumn based on ItemsSource:

```csharp
public class CustomColumnSizer : GridColumnSizer
{
    public CustomColumnSizer(SfDataGrid grid) : base(grid)
    {
    }
    
    protected override double CalculateCellWidth(GridColumn column, bool setWidth = true)
    {
        if (column is GridComboBoxColumn)
        {
            double colWidth = double.MaxValue;
            var source = (column as GridComboBoxColumn).ItemsSource;
            string maximumComboItemsText = string.Empty;
            var clientSize = new Size(colWidth, DataGrid.RowHeight);
            
            foreach (var comboItems in source)
            {
                string comboItemText = (string)comboItems;
                if (maximumComboItemsText.Length < comboItemText.Length)
                    maximumComboItemsText = comboItemText;
            }
            
            var measureSize = MeasureText(clientSize, maximumComboItemsText, column, null, GridQueryBounds.Width);
            return measureSize.Width + SystemParameters.ScrollWidth;
        }
        
        return base.CalculateCellWidth(column, setWidth);
    }
}
```

## Performance Optimization

### Minimize Auto-Size Operations

```csharp
// Bad: Multiple refreshes
foreach (var order in newOrders)
{
    viewModel.Orders.Add(order);
    dataGrid.GridColumnSizer.Refresh();  // Don't do this!
}

// Good: Single refresh
foreach (var order in newOrders)
{
    viewModel.Orders.Add(order);
}
dataGrid.GridColumnSizer.Refresh();  // Refresh once
```

### Use BeginInit/EndInit for Bulk Operations

```csharp
dataGrid.View.BeginInit();
try
{
    // Bulk operations
    dataGrid.GridColumnSizer.ResetAutoCalculationforAllColumns();
    
    foreach (var order in largeDataSet)
    {
        viewModel.Orders.Add(order);
    }
}
finally
{
    dataGrid.View.EndInit();
    dataGrid.GridColumnSizer.Refresh();
}
```

### Selective Column Auto-Sizing

Auto-size only specific columns:

```csharp
// Reset and refresh only specific columns
var columnsToResize = new[] { "CustomerName", "ProductName" };

foreach (var columnName in columnsToResize)
{
    var column = dataGrid.Columns[columnName];
    dataGrid.GridColumnSizer.ResetAutoCalculation(column);
}

dataGrid.GridColumnSizer.Refresh();
```

### Disable Auto-Sizing When Not Needed

```csharp
// Temporarily disable auto-sizing
var originalSizer = dataGrid.ColumnSizer;
dataGrid.ColumnSizer = GridLengthUnitType.None;

// Perform operations

// Re-enable
dataGrid.ColumnSizer = originalSizer;
dataGrid.GridColumnSizer.Refresh();
```

## Troubleshooting

### Auto-Sizing Not Working

**Issue**: ColumnSizer not affecting column width

**Solutions**:
1. Ensure column Width is not explicitly set
2. Verify ColumnSizer is not set to None
3. Check MinWidth/MaxWidth constraints
4. Call Refresh() after data changes

```csharp
// Reset explicit width
column.Width = double.NaN;
column.ColumnSizer = GridLengthUnitType.Auto;
dataGrid.GridColumnSizer.Refresh();
```

### Columns Too Wide/Narrow

**Issue**: Auto-sized columns are not optimal

**Solutions**:
1. Adjust font size and margin settings
2. Set MinWidth/MaxWidth constraints
3. Use different ColumnSizer type
4. Customize calculation logic

```csharp
// Set constraints
column.MinimumWidth = 80;
column.MaximumWidth = 300;

// Or adjust font settings
dataGrid.GridColumnSizer.FontSize = 12.0;
dataGrid.GridColumnSizer.Margin = new Thickness(8, 4, 8, 4);
```

### Star Columns Not Distributing Evenly

**Issue**: Star columns have unequal widths

**Solutions**:
1. Ensure all columns have ColumnSizer set to Star
2. Check for MinWidth/MaxWidth constraints
3. Verify no explicit widths are set
4. Use custom star ratio if needed

```csharp
// Verify all star columns
foreach (var column in dataGrid.Columns)
{
    if (column.ColumnSizer == GridLengthUnitType.Star)
    {
        column.Width = double.NaN;  // Remove explicit width
        column.MinimumWidth = 0;    // Remove constraints if not needed
        column.MaximumWidth = double.PositiveInfinity;
    }
}
```

### Last Column Not Filling Space

**Issue**: Using AutoLastColumnFill but last column doesn't fill

**Solutions**:
1. Verify ColumnSizer is set correctly
2. Check if last column is visible
3. Ensure total column widths don't exceed grid width
4. Use AutoWithLastColumnFill instead

```csharp
dataGrid.ColumnSizer = GridLengthUnitType.AutoLastColumnFill;

// Or specify which column should fill
dataGrid.Columns["Description"].ColumnSizer = GridLengthUnitType.AutoLastColumnFill;
```

### Performance Issues with Auto-Sizing

**Issue**: Grid is slow when auto-sizing many columns

**Solutions**:
1. Use BeginInit/EndInit for bulk operations
2. Limit auto-sizing to visible columns only
3. Use SizeToCells instead of Auto (faster)
4. Implement custom caching in GridColumnSizer

```csharp
// Faster alternative
dataGrid.ColumnSizer = GridLengthUnitType.SizeToCells;  // Excludes header calculation

// Or cache measurements
public class CachedColumnSizer : GridColumnSizer
{
    private Dictionary<string, double> _cache = new Dictionary<string, double>();
    
    protected override double CalculateCellWidth(GridColumn column, bool setWidth = true)
    {
        if (_cache.ContainsKey(column.MappingName))
            return _cache[column.MappingName];
        
        var width = base.CalculateCellWidth(column, setWidth);
        _cache[column.MappingName] = width;
        return width;
    }
}
```

### Auto-Size Not Updating After Data Changes

**Issue**: Column widths don't adjust after adding/updating data

**Solutions**:
1. Call Refresh() after data changes
2. Reset calculations if needed
3. Ensure INotifyPropertyChanged is implemented
4. Verify ObservableCollection is used

```csharp
// After data operation
viewModel.Orders.Add(newOrder);

// Reset and refresh
dataGrid.GridColumnSizer.ResetAutoCalculationforAllColumns();
dataGrid.GridColumnSizer.Refresh();
```

### Mixed Sizing Not Working As Expected

**Issue**: Combination of sizing types produces unexpected results

**Solutions**:
1. Understand precedence: Fixed Width > Column ColumnSizer > Grid ColumnSizer
2. Verify each column's ColumnSizer setting
3. Check for conflicting width constraints
4. Test each sizing type separately

```csharp
// Debug column sizing
foreach (var column in dataGrid.Columns)
{
    Debug.WriteLine($"{column.MappingName}: " +
                   $"Width={column.Width}, " +
                   $"ColumnSizer={column.ColumnSizer}, " +
                   $"MinWidth={column.MinimumWidth}, " +
                   $"MaxWidth={column.MaximumWidth}");
}
```
