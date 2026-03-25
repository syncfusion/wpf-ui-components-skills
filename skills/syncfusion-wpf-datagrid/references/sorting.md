# Sorting in WPF DataGrid (SfDataGrid)

Comprehensive reference for sorting operations, configuration, and custom sorting logic in the Syncfusion WPF DataGrid control.

## Table of Contents
- [Overview](#overview)
- [Basic Sorting Configuration](#basic-sorting-configuration)
- [Sorting Modes](#sorting-modes)
- [Multi-Column Sorting](#multi-column-sorting)
- [Programmatic Sorting](#programmatic-sorting)
- [Custom Sorting](#custom-sorting)
- [Sorting Events](#sorting-events)
- [Advanced Scenarios](#advanced-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

WPF DataGrid supports sorting data in ascending or descending order by clicking column headers or programmatically. Sorting rearranges rows based on column values without modifying the underlying data source.

**Key Features:**
- Single and multi-column sorting
- Tri-state sorting (ascending, descending, unsorted)
- Custom sort logic via comparers
- Sorting events for customization
- Programmatic sort control
- Sort order indicators

## Basic Sorting Configuration

### Enable Sorting

Enable sorting for all columns:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowSorting = true;
```

### Column-Level Sorting Control

Enable/disable sorting for specific columns:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="False"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <!-- Sorting enabled -->
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   AllowSorting="True" />
        
        <!-- Sorting disabled -->
        <syncfusion:GridTextColumn MappingName="CustomerID" 
                                   AllowSorting="False" />
        
        <!-- Sorting enabled -->
        <syncfusion:GridTextColumn MappingName="Country" 
                                   AllowSorting="True" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.Columns["OrderID"].AllowSorting = true;
dataGrid.Columns["CustomerID"].AllowSorting = false;
```

**Priority**: `GridColumn.AllowSorting` takes higher priority than `SfDataGrid.AllowSorting`.

## Sorting Modes

### Sort on Single Click (Default)

By default, columns sort when header is clicked once:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="True"
                       ItemsSource="{Binding Orders}" />
```

**Behavior**: 
- First click: Ascending order
- Second click: Descending order

### Sort on Double Click

Change sorting trigger to double-click:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="True"
                       SortClickAction="DoubleClick"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.SortClickAction = SortClickAction.DoubleClick;
```

**Use Cases:**
- Prevent accidental sorting
- Allow single-click for selection
- Custom header interactions

### Tri-State Sorting

Enable three sorting states: ascending → descending → unsorted:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="True"
                       AllowTriStateSorting="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowTriStateSorting = true;
```

**Behavior**:
- First click: Ascending order
- Second click: Descending order
- Third click: Clear sorting (original order)

## Multi-Column Sorting

### Enable Multi-Column Sorting

Multi-column sorting is enabled by default when `AllowSorting` is true.

**User Interaction**: Hold <kbd>Ctrl</kbd> key while clicking column headers.

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="True"
                       ItemsSource="{Binding Orders}" />
```

**Example**: Sort by OrderID first, then by CustomerName while holding <kbd>Ctrl</kbd>.

### Display Sort Numbers

Show sort sequence numbers in column headers:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="True"
                       ShowSortNumbers="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ShowSortNumbers = true;
```

Sort numbers appear as small numbers (1, 2, 3...) indicating sort order.

### Sort Multiple Columns Without Ctrl Key

Custom implementation to sort without <kbd>Ctrl</kbd> key:

```csharp
// Override GridSelectionController
public class CustomSelectionController : GridSelectionController
{
    public CustomSelectionController(SfDataGrid dataGrid) : base(dataGrid)
    {
    }
    
    protected override void ProcessKeyDown(KeyEventArgs args)
    {
        // Treat all header clicks as if Ctrl is pressed
        if (args.OriginalSource is GridHeaderCellControl)
        {
            // Add sort descriptor instead of replacing
            base.ProcessKeyDown(args);
        }
        else
        {
            base.ProcessKeyDown(args);
        }
    }
}

// Apply custom controller
dataGrid.SelectionController = new CustomSelectionController(dataGrid);
```

## Programmatic Sorting

### Adding Sort Columns

Add sort descriptors programmatically:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.SortColumnDescriptions>
        <syncfusion:SortColumnDescription ColumnName="OrderID" 
                                          SortDirection="Ascending" />
        <syncfusion:SortColumnDescription ColumnName="CustomerName" 
                                          SortDirection="Descending" />
    </syncfusion:SfDataGrid.SortColumnDescriptions>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.SortColumnDescriptions.Add(new SortColumnDescription()
{
    ColumnName = "OrderID",
    SortDirection = ListSortDirection.Ascending
});

dataGrid.SortColumnDescriptions.Add(new SortColumnDescription()
{
    ColumnName = "CustomerName",
    SortDirection = ListSortDirection.Descending
});
```

### Removing Sort Columns

Remove specific sort descriptor:

```csharp
var sortDescriptor = dataGrid.SortColumnDescriptions
    .FirstOrDefault(col => col.ColumnName == "OrderID");

if (sortDescriptor != null)
{
    dataGrid.SortColumnDescriptions.Remove(sortDescriptor);
}
```

### Clear All Sorting

Remove all sort descriptors:

```csharp
dataGrid.SortColumnDescriptions.Clear();
```

### Changing Sort Direction

```csharp
var sortDescriptor = dataGrid.SortColumnDescriptions
    .FirstOrDefault(col => col.ColumnName == "OrderID");

if (sortDescriptor != null)
{
    sortDescriptor.SortDirection = sortDescriptor.SortDirection == ListSortDirection.Ascending
        ? ListSortDirection.Descending
        : ListSortDirection.Ascending;
}
```

**Note**: `SortColumnsChanging` and `SortColumnsChanged` events are NOT raised when sorting programmatically.

## Custom Sorting

### Implementing Custom Sort Comparer

Create custom comparer for specialized sorting logic:

```csharp
public class CustomComparer : IComparer<object>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }
    
    public int Compare(object x, object y)
    {
        int lengthX, lengthY;
        
        // Handle data objects
        if (x is OrderInfo)
        {
            lengthX = ((OrderInfo)x).CustomerName.Length;
            lengthY = ((OrderInfo)y).CustomerName.Length;
        }
        // Handle grouped data
        else if (x is Group)
        {
            lengthX = ((Group)x).Key.ToString().Length;
            lengthY = ((Group)y).Key.ToString().Length;
        }
        else
        {
            lengthX = x.ToString().Length;
            lengthY = y.ToString().Length;
        }
        
        // Apply sort direction
        if (lengthX.CompareTo(lengthY) > 0)
            return SortDirection == ListSortDirection.Ascending ? 1 : -1;
        else if (lengthX.CompareTo(lengthY) < 0)
            return SortDirection == ListSortDirection.Ascending ? -1 : 1;
        else
            return 0;
    }
}
```

### Registering Custom Comparer

Register comparer for specific column:

```xaml
<Window.Resources>
    <local:CustomComparer x:Key="customComparer" />
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.SortComparers>
        <syncfusion:SortComparer Comparer="{StaticResource customComparer}" 
                                 PropertyName="CustomerName" />
    </syncfusion:SfDataGrid.SortComparers>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.SortComparers.Add(new SortComparer()
{
    Comparer = new CustomComparer(),
    PropertyName = "CustomerName"
});
```

### Custom Sorting Examples

**Example 1: Case-Insensitive Sorting**

```csharp
public class CaseInsensitiveComparer : IComparer<object>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }
    
    public int Compare(object x, object y)
    {
        string strX = GetStringValue(x);
        string strY = GetStringValue(y);
        
        int result = string.Compare(strX, strY, StringComparison.OrdinalIgnoreCase);
        
        return SortDirection == ListSortDirection.Ascending ? result : -result;
    }
    
    private string GetStringValue(object obj)
    {
        if (obj is OrderInfo)
            return ((OrderInfo)obj).Country;
        else if (obj is Group)
            return ((Group)obj).Key.ToString();
        else
            return obj?.ToString() ?? string.Empty;
    }
}
```

**Example 2: Numeric String Sorting**

```csharp
public class NumericStringComparer : IComparer<object>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }
    
    public int Compare(object x, object y)
    {
        string strX = GetStringValue(x);
        string strY = GetStringValue(y);
        
        // Try parse as numbers
        bool isNumX = int.TryParse(strX, out int numX);
        bool isNumY = int.TryParse(strY, out int numY);
        
        int result;
        
        if (isNumX && isNumY)
        {
            result = numX.CompareTo(numY);
        }
        else
        {
            result = string.Compare(strX, strY);
        }
        
        return SortDirection == ListSortDirection.Ascending ? result : -result;
    }
    
    private string GetStringValue(object obj)
    {
        if (obj is OrderInfo)
            return ((OrderInfo)obj).OrderID.ToString();
        else if (obj is Group)
            return ((Group)obj).Key.ToString();
        else
            return obj?.ToString() ?? string.Empty;
    }
}
```

**Example 3: Date Sorting with Null Handling**

```csharp
public class DateComparer : IComparer<object>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }
    
    public int Compare(object x, object y)
    {
        DateTime? dateX = GetDateValue(x);
        DateTime? dateY = GetDateValue(y);
        
        // Handle nulls (nulls last)
        if (!dateX.HasValue && !dateY.HasValue)
            return 0;
        if (!dateX.HasValue)
            return SortDirection == ListSortDirection.Ascending ? 1 : -1;
        if (!dateY.HasValue)
            return SortDirection == ListSortDirection.Ascending ? -1 : 1;
        
        int result = dateX.Value.CompareTo(dateY.Value);
        
        return SortDirection == ListSortDirection.Ascending ? result : -result;
    }
    
    private DateTime? GetDateValue(object obj)
    {
        if (obj is OrderInfo)
            return ((OrderInfo)obj).OrderDate;
        else if (obj is Group)
            return ((Group)obj).Key as DateTime?;
        else
            return obj as DateTime?;
    }
}
```

## Sorting Events

### SortColumnsChanging Event

Raised before sorting is applied:

```csharp
dataGrid.SortColumnsChanging += DataGrid_SortColumnsChanging;

void DataGrid_SortColumnsChanging(object sender, GridSortColumnsChangingEventArgs e)
{
    // Cancel sorting for specific column
    if (e.AddedItems.Count > 0 && e.AddedItems[0].ColumnName == "OrderID")
    {
        e.Cancel = true;
        return;
    }
    
    // Modify sort direction
    if (e.AddedItems.Count > 0 && e.AddedItems[0].ColumnName == "CustomerName")
    {
        e.AddedItems[0].SortDirection = ListSortDirection.Descending;
    }
    
    // Cancel scrolling to selected item after sorting
    e.CancelScroll = true;
}
```

**Event Arguments**:
- **Action**: Type of action triggering the event
- **AddedItems**: New sort descriptors being added
- **RemovedItems**: Sort descriptors being removed
- **Cancel**: Set to true to cancel sorting
- **CancelScroll**: Set to true to prevent scrolling

### SortColumnsChanged Event

Raised after sorting is applied:

```csharp
dataGrid.SortColumnsChanged += DataGrid_SortColumnsChanged;

void DataGrid_SortColumnsChanged(object sender, GridSortColumnsChangedEventArgs e)
{
    // Log sort changes
    foreach (var descriptor in dataGrid.SortColumnDescriptions)
    {
        Debug.WriteLine($"Sorted: {descriptor.ColumnName} - {descriptor.SortDirection}");
    }
    
    // Update UI
    UpdateSortIndicator();
}
```

## Advanced Scenarios

### Sorting Underlying Collection

Sort the actual data source instead of just the view:

```csharp
dataGrid.SortColumnsChanged += DataGrid_SortColumnsChanged;

void DataGrid_SortColumnsChanged(object sender, GridSortColumnsChangedEventArgs e)
{
    var viewModel = dataGrid.DataContext as ViewModel;
    IEnumerable<OrderInfo> orderedSource = viewModel.Orders;
    
    foreach (var sortColumn in dataGrid.View.SortDescriptions)
    {
        var columnName = sortColumn.PropertyName;
        
        if (sortColumn.Direction == ListSortDirection.Ascending)
            orderedSource = orderedSource.OrderBy(item => GetPropertyValue(item, columnName));
        else
            orderedSource = orderedSource.OrderByDescending(item => GetPropertyValue(item, columnName));
    }
    
    // Update collection
    viewModel.Orders = new ObservableCollection<OrderInfo>(orderedSource);
}

private object GetPropertyValue(object obj, string propertyName)
{
    var propInfo = obj.GetType().GetProperty(propertyName);
    return propInfo?.GetValue(obj);
}
```

### Maintain Sort After Data Changes

```csharp
// Save current sort
private List<SortColumnDescription> SavedSort;

private void SaveSortState()
{
    SavedSort = dataGrid.SortColumnDescriptions.ToList();
}

private void RestoreSortState()
{
    dataGrid.SortColumnDescriptions.Clear();
    
    foreach (var descriptor in SavedSort)
    {
        dataGrid.SortColumnDescriptions.Add(descriptor);
    }
}

// Use before and after data operations
private void RefreshData()
{
    SaveSortState();
    
    // Data operations
    viewModel.RefreshOrders();
    
    RestoreSortState();
}
```

### Custom Sort Indicators

Replace default sort indicators:

```xaml
<Style TargetType="syncfusion:GridHeaderCellControl">
    <Setter Property="SortDirection" Value="{Binding SortDirection}"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="syncfusion:GridHeaderCellControl">
                <Grid>
                    <ContentPresenter />
                    
                    <!-- Custom sort indicator -->
                    <Path x:Name="SortIndicator"
                          Width="10"
                          Height="6"
                          HorizontalAlignment="Right"
                          VerticalAlignment="Center"
                          Margin="0,0,5,0"
                          Fill="Blue"
                          Data="M0,0 L5,6 L10,0 Z"
                          Visibility="Collapsed" />
                </Grid>
                
                <ControlTemplate.Triggers>
                    <DataTrigger Binding="{Binding SortDirection}" Value="Ascending">
                        <Setter TargetName="SortIndicator" Property="Visibility" Value="Visible" />
                        <Setter TargetName="SortIndicator" Property="RenderTransform">
                            <Setter.Value>
                                <RotateTransform Angle="180" CenterX="5" CenterY="3" />
                            </Setter.Value>
                        </Setter>
                    </DataTrigger>
                    
                    <DataTrigger Binding="{Binding SortDirection}" Value="Descending">
                        <Setter TargetName="SortIndicator" Property="Visibility" Value="Visible" />
                    </DataTrigger>
                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### Sorting with Grouping

When grouping is applied, custom sorting affects group order:

```csharp
// Custom comparer that sorts groups
public class GroupAwareComparer : IComparer<object>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }
    
    public int Compare(object x, object y)
    {
        // Handle both data rows and groups
        var valueX = GetCompareValue(x);
        var valueY = GetCompareValue(y);
        
        int result = ((IComparable)valueX).CompareTo(valueY);
        
        return SortDirection == ListSortDirection.Ascending ? result : -result;
    }
    
    private object GetCompareValue(object obj)
    {
        if (obj is OrderInfo)
            return ((OrderInfo)obj).UnitPrice;
        else if (obj is Group)
            return ((Group)obj).Key;
        else
            return obj;
    }
}
```

## Troubleshooting

### Sorting Not Working

**Issue**: Column doesn't sort when header is clicked

**Solutions**:
1. Verify `AllowSorting` is true on grid or column
2. Check if `SortClickAction` is set correctly
3. Ensure column has valid `MappingName`
4. Verify property has getter

```csharp
// Diagnostic check
Debug.WriteLine($"AllowSorting: {dataGrid.AllowSorting}");
Debug.WriteLine($"Column AllowSorting: {dataGrid.Columns["OrderID"].AllowSorting}");
Debug.WriteLine($"SortClickAction: {dataGrid.SortClickAction}");
```

### Custom Comparer Not Applied

**Issue**: Custom sorting logic not executing

**Solutions**:
1. Verify comparer is added to `SortComparers` collection
2. Check `PropertyName` matches column `MappingName` exactly
3. Ensure comparer implements `ISortDirection`
4. Test comparer logic independently

```csharp
// Verify comparer registration
var comparer = dataGrid.SortComparers
    .FirstOrDefault(c => c.PropertyName == "CustomerName");

if (comparer == null)
{
    Debug.WriteLine("Comparer not registered!");
}
```

### Multi-Column Sorting Issues

**Issue**: Cannot sort multiple columns

**Solutions**:
1. Ensure user is holding <kbd>Ctrl</kbd> key
2. Verify `AllowSorting` is enabled
3. Check for custom code canceling sort in events
4. Test with `ShowSortNumbers` enabled

### Sort Order Incorrect

**Issue**: Data sorts in unexpected order

**Solutions**:
1. Check custom comparer logic
2. Verify data types match expected format
3. Test with default sorting first
4. Handle null values properly in comparer

```csharp
// Add null handling
public int Compare(object x, object y)
{
    if (x == null && y == null) return 0;
    if (x == null) return -1;
    if (y == null) return 1;
    
    // Compare logic
}
```

### Performance Issues with Large Datasets

**Issue**: Sorting is slow with many records

**Solutions**:
1. Optimize custom comparer logic
2. Avoid complex calculations in Compare method
3. Cache computed values if possible
4. Consider using indexed properties

```csharp
// Bad: Complex calculation in comparer
public int Compare(object x, object y)
{
    var valueX = CalculateComplexValue((OrderInfo)x);  // Slow!
    var valueY = CalculateComplexValue((OrderInfo)y);
    return valueX.CompareTo(valueY);
}

// Good: Pre-calculated property
public class OrderInfo
{
    public double ComplexValue { get; set; }  // Calculated once
}

public int Compare(object x, object y)
{
    var valueX = ((OrderInfo)x).ComplexValue;
    var valueY = ((OrderInfo)y).ComplexValue;
    return valueX.CompareTo(valueY);
}
```

### Sort Not Persisting After Refresh

**Issue**: Sort order lost after data refresh

**Solutions**:
1. Save sort descriptors before refresh
2. Reapply sort after data update
3. Use View filtering instead of replacing ItemsSource
4. Implement sort persistence

```csharp
private void RefreshData()
{
    // Save sort
    var savedSort = dataGrid.SortColumnDescriptions.ToList();
    
    // Refresh data
    dataGrid.ItemsSource = null;
    dataGrid.ItemsSource = viewModel.Orders;
    
    // Restore sort
    foreach (var descriptor in savedSort)
    {
        dataGrid.SortColumnDescriptions.Add(descriptor);
    }
}
```

### Sorting Events Not Firing

**Issue**: `SortColumnsChanging`/`SortColumnsChanged` not raised

**Solutions**:
1. Events only fire for UI sorting, not programmatic
2. Verify event handlers are registered
3. Check if events are being canceled upstream
4. Use programmatic sort completion detection

```csharp
// For programmatic sorting detection
dataGrid.SortColumnDescriptions.CollectionChanged += SortDescriptions_CollectionChanged;

void SortDescriptions_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
{
    Debug.WriteLine("Sort descriptors changed programmatically");
}
```
