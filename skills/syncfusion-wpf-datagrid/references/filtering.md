# Filtering in WPF DataGrid (SfDataGrid)

Comprehensive reference for filtering operations, UI filtering, programmatic filtering, and custom filter logic in the Syncfusion WPF DataGrid control.

## Table of Contents
- [Overview](#overview)
- [Programmatic Filtering](#programmatic-filtering)
- [UI Filtering](#ui-filtering)
- [Filter UI Modes](#filter-ui-modes)
- [Advanced Filter](#advanced-filter)
- [Checkbox Filter](#checkbox-filter)
- [Filter Configuration](#filter-configuration)
- [Filter Events](#filter-events)
- [Customization](#customization)
- [Troubleshooting](#troubleshooting)

## Overview

WPF DataGrid provides comprehensive filtering capabilities allowing users to filter data through UI interactions or programmatically. Filtering retrieves records that satisfy specified conditions without modifying the underlying data source.

**Key Features:**
- View filtering and column filtering
- Excel-like checkbox filter UI
- Advanced filter with multiple conditions
- Custom filter predicates
- Filter events for customization
- Case-sensitive and case-insensitive filtering
- Filter based on display text or actual values

## Programmatic Filtering

### View Filtering

Apply filter to entire grid view using predicate:

```csharp
// Define filter predicate
public bool FilterRecords(object o)
{
    string filterText = "Germany";
    var item = o as OrderInfo;
    
    if (item != null)
    {
        if (item.Country.Equals(filterText))
            return true;
    }
    return false;
}

// Apply filter
private void ApplyFilter_Click(object sender, RoutedEventArgs e)
{
    dataGrid.View.Filter = FilterRecords;
    dataGrid.View.RefreshFilter();
}
```

**Limitations**: View filter not supported when ItemsSource is DataTable.

### Column Filtering

Apply filters to specific columns using FilterPredicates:

```csharp
// Single filter predicate
dataGrid.Columns["OrderID"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = "1005"
});

// Multiple filter predicates with OR logic
dataGrid.Columns["Country"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = "Germany",
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.Or
});

dataGrid.Columns["Country"].FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = "USA",
    FilterBehavior = FilterBehavior.StronglyTyped,
    PredicateType = PredicateType.Or
});
```

### Filter Behavior

Control how filter values are interpreted:

```csharp
var predicate = new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = "1005",
    FilterBehavior = FilterBehavior.StronglyTyped,  // or StringTyped
    IsCaseSensitive = false
};

dataGrid.Columns["OrderID"].FilterPredicates.Add(predicate);
```

**FilterBehavior Options**:
- **StringTyped**: Treats FilterValue as string (default)
- **StronglyTyped**: Considers FilterValue's underlying type

**Note**: `IsCaseSensitive` not supported when ItemsSource is DataTable.

### Bulk Filter Additions

Improve performance when adding multiple filter predicates:

```csharp
private void ApplyBulkFilter()
{
    var gridColumn = dataGrid.Columns["EmployeeId"];
    var filterValues = new[] { 1001, 1002, 1003, 1004, 1005 };
    
    dataGrid.View.BeginInit();
    
    foreach (var filterValue in filterValues)
    {
        gridColumn.FilterPredicates.Add(new FilterPredicate()
        {
            FilterType = FilterType.Equals,
            FilterValue = filterValue,
            FilterBehavior = FilterBehavior.StronglyTyped,
            FilterMode = ColumnFilter.Value,
            PredicateType = PredicateType.Or,
            IsCaseSensitive = true
        });
    }
    
    dataGrid.View.EndInit();
}
```

### Clearing Filters

```csharp
// Clear all filters
dataGrid.ClearFilters();

// Clear specific column filter by name
dataGrid.ClearFilter("OrderID");

// Clear specific column filter by reference
dataGrid.ClearFilter(dataGrid.Columns[0]);
```

## UI Filtering

### Enable UI Filtering

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowFiltering="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowFiltering = true;
```

### Column-Level Filter Control

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowFiltering="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <!-- Filtering enabled -->
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   AllowFiltering="True" />
        
        <!-- Filtering disabled -->
        <syncfusion:GridTextColumn MappingName="CustomerID" 
                                   AllowFiltering="False" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.Columns["OrderID"].AllowFiltering = true;
dataGrid.Columns["CustomerID"].AllowFiltering = false;
```

**Priority**: `GridColumn.AllowFiltering` > `SfDataGrid.AllowFiltering`

## Filter UI Modes

### Available Modes

DataGrid provides three filter UI modes:

```csharp
public enum FilterMode
{
    CheckboxFilter,  // Shows only checkbox filter
    AdvancedFilter,  // Shows only advanced filter
    Both            // Shows both (default)
}
```

### Set Filter Mode for Grid

Apply filter mode to all columns:

```xaml
<Window.Resources>
    <Style x:Key="filterControlStyle" TargetType="syncfusion:GridFilterControl">
        <Setter Property="FilterMode" Value="AdvancedFilter" />
    </Style>
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowFiltering="True"
                       FilterPopupStyle="{StaticResource filterControlStyle}"
                       ItemsSource="{Binding Orders}" />
```

### Set Filter Mode for Column

Apply filter mode to specific column:

```xaml
<syncfusion:GridTextColumn MappingName="OrderID">
    <syncfusion:GridTextColumn.FilterPopupStyle>
        <Style TargetType="syncfusion:GridFilterControl">
            <Setter Property="FilterMode" Value="CheckboxFilter" />
        </Style>
    </syncfusion:GridTextColumn.FilterPopupStyle>
</syncfusion:GridTextColumn>
```

### Set Filter Mode Programmatically

```csharp
dataGrid.FilterItemsPopulating += DataGrid_FilterItemsPopulating;

void DataGrid_FilterItemsPopulating(object sender, GridFilterItemsPopulatingEventArgs e)
{
    if (e.Column.MappingName == "OrderID")
    {
        e.FilterControl.FilterMode = FilterMode.AdvancedFilter;
    }
}
```

### Skip Filter Style for Column

Apply default style to specific column:

```xaml
<Window.Resources>
    <Style x:Key="filterControlStyle" TargetType="syncfusion:GridFilterControl">
        <Setter Property="FilterMode" Value="AdvancedFilter" />
    </Style>
</Window.Resources>

<syncfusion:SfDataGrid FilterPopupStyle="{StaticResource filterControlStyle}">
    <syncfusion:SfDataGrid.Columns>
        <!-- Uses default style (both filters) -->
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   FilterPopupStyle="{x:Null}" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

## Advanced Filter

### Filter Types

DataGrid automatically loads appropriate filter type based on column data type:

**Text Filters** (string, object, dynamic):
- Equals
- Does Not Equal
- Begins With
- Does Not Begin With
- Ends With
- Does Not End With
- Contains
- Does Not Contain
- Empty
- Not Empty
- Null
- Not Null

**Number Filters** (int, double, decimal, float, long, short, byte):
- Equals
- Does Not Equal
- Less Than
- Less Than or Equal
- Greater Than
- Greater Than or Equal
- Null
- Not Null

**Date Filters** (DateTime, DateTimeOffset):
- Equals
- Does Not Equal
- Before
- Before Or Equal
- After
- After Or Equal
- Null
- Not Null

### Change Advanced Filter Type

Override automatic detection:

```csharp
// Force text filters for numeric column
dataGrid.Columns["OrderID"].FilterBehavior = FilterBehavior.StringTyped;
```

```csharp
// Programmatic override
dataGrid.FilterItemsPopulating += DataGrid_FilterItemsPopulating;

void DataGrid_FilterItemsPopulating(object sender, GridFilterItemsPopulatingEventArgs e)
{
    if (e.Column.MappingName == "OrderID")
    {
        e.FilterControl.AdvancedFilterType = AdvancedFilterType.TextFilter;
        e.FilterControl.SetColumnDataType(typeof(string));
        e.FilterControl.AscendingSortString = GridResourceWrapper.SortStringAscending;
        e.FilterControl.DescendingSortString = GridResourceWrapper.SortStringDescending;
    }
}
```

### Advanced Filter for Dynamic ItemsSource

Set ColumnMemberType for proper filter type:

```csharp
// For dynamic data with numeric column
dataGrid.Columns["OrderID"].ColumnMemberType = typeof(double?);
```

### Case-Sensitive Filtering

Enable case-sensitive button in advanced filter:

```csharp
// Case sensitive button available by default in advanced filter
// User can toggle it in UI
// Not available when ItemsSource is DataTable
```

### Performance Optimization

Disable unique items generation for better performance:

```xaml
<Window.Resources>
    <Style TargetType="syncfusion:AdvancedFilterControl">
        <Setter Property="CanGenerateUniqueItems" Value="False" />
    </Style>
    
    <Style x:Key="filterControlStyle" TargetType="syncfusion:GridFilterControl">
        <Setter Property="FilterMode" Value="AdvancedFilter" />
    </Style>
</Window.Resources>

<syncfusion:SfDataGrid FilterPopupStyle="{StaticResource filterControlStyle}" />
```

When disabled, a textbox appears instead of combo box for manual text entry.

## Checkbox Filter

### Basic Usage

Checkbox filter displays list of unique values:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowFiltering="True"
                       ItemsSource="{Binding Orders}" />
```

### Null Values in Filter

Include/exclude null values:

```xaml
<syncfusion:GridTextColumn MappingName="Country" 
                          AllowBlankFilters="True" />
```

```csharp
dataGrid.Columns["Country"].AllowBlankFilters = true;  // Include nulls
dataGrid.Columns["Country"].AllowBlankFilters = false; // Exclude nulls
```

### Immediate Update

Update filter immediately without OK button:

```xaml
<syncfusion:GridTextColumn MappingName="OrderID" 
                          ImmediateUpdateColumnFilter="True" />
```

```csharp
dataGrid.Columns["OrderID"].ImmediateUpdateColumnFilter = true;
```

When enabled:
- OK/Cancel buttons hidden
- Done button shown
- Filters applied immediately on selection

**Limitation**: SelectAll option not reflected immediately when enabled.

## Filter Configuration

### Filter Based on Display Text

Filter using formatted display text instead of actual values:

```xaml
<syncfusion:GridDateTimeColumn MappingName="OrderDate" 
                              ColumnFilter="DisplayText" />
```

```csharp
dataGrid.Columns["OrderDate"].ColumnFilter = ColumnFilter.DisplayText;
```

**Example**: Two records with different DateTime values (2/10/2010 12:00 AM and 2/10/2010 6:30 PM) will be treated as same if display format shows only date.

### Filtering Null Values

Control null value filtering:

```xaml
<syncfusion:GridTextColumn MappingName="Country" 
                          AllowBlankFilters="False" />
```

```csharp
dataGrid.Columns["Country"].AllowBlankFilters = false;
```

When false, "Null" and "Not Null" options hidden from filter popup.

### Apply ICollectionView.Filter on Initial Load

Enable view filters on initial load:

```csharp
dataGrid.CanUseViewFilter = true;
```

Applies ICollectionView.Filter and DataView.RowFilter immediately when grid loads.

## Filter Events

### FilterItemsPopulating Event

Raised while populating filter items:

```csharp
dataGrid.FilterItemsPopulating += DataGrid_FilterItemsPopulating;

void DataGrid_FilterItemsPopulating(object sender, GridFilterItemsPopulatingEventArgs e)
{
    // Change filter mode
    if (e.Column.MappingName == "OrderID")
    {
        e.FilterControl.FilterMode = FilterMode.AdvancedFilter;
    }
    
    // Customize filter control
    e.FilterControl.CanGenerateUniqueItems = false;
}
```

### FilterItemsPopulated Event

Raised after filter items are populated:

```csharp
dataGrid.FilterItemsPopulated += DataGrid_FilterItemsPopulated;

void DataGrid_FilterItemsPopulated(object sender, GridFilterItemsPopulatedEventArgs e)
{
    if (e.Column.MappingName == "OrderID")
    {
        var itemsSource = e.ItemsSource as List<FilterElement>;
        
        // Remove specific filter element
        var filterElement = itemsSource.FirstOrDefault(item => item.ActualValue.Equals(1005));
        if (filterElement != null)
        {
            itemsSource.Remove(filterElement);
        }
        
        // Modify filter elements
        foreach (var element in itemsSource)
        {
            if (element.ActualValue.Equals(1001))
            {
                element.DisplayText = "High Priority Order";
            }
        }
    }
}
```

### FilterChanging Event

Raised before filter is applied:

```csharp
dataGrid.FilterChanging += DataGrid_FilterChanging;

void DataGrid_FilterChanging(object sender, GridFilterEventArgs e)
{
    // Cancel filtering for specific column
    if (e.Column.MappingName == "OrderID")
    {
        e.Cancel = true;
        return;
    }
    
    // Modify filter predicates
    if (e.FilterPredicates != null && e.Column.MappingName == "CustomerID")
    {
        if (e.FilterPredicates.Count > 0 && e.FilterPredicates[0].FilterValue.Equals("ALFKI"))
        {
            e.FilterPredicates[0].FilterValue = "ANATR";
        }
    }
}
```

### FilterChanged Event

Raised after filter is applied:

```csharp
dataGrid.FilterChanged += DataGrid_FilterChanged;

void DataGrid_FilterChanged(object sender, GridFilterEventArgs e)
{
    // Get filtered records
    ObservableCollection<OrderInfo> filteredOrders = new ObservableCollection<OrderInfo>();
    var records = dataGrid.View.Records;
    
    foreach (RecordEntry record in records)
    {
        filteredOrders.Add(record.Data as OrderInfo);
    }
    
    // Update UI or perform actions with filtered data
    StatusText.Text = $"Filtered Records: {filteredOrders.Count}";
}
```

## Customization

### Customize Filter Popup Size

```xaml
<Window.Resources>
    <Style TargetType="syncfusion:GridFilterControl">
        <Setter Property="FilterPopupHeight" Value="632" />
        <Setter Property="FontSize" Value="14" />
        <Setter Property="FontWeight" Value="Normal" />
    </Style>
</Window.Resources>
```

### Customize Sort Options Text

```csharp
dataGrid.FilterItemsPopulating += DataGrid_FilterItemsPopulating;

void DataGrid_FilterItemsPopulating(object sender, GridFilterItemsPopulatingEventArgs e)
{
    if (e.Column.MappingName == "CustomerName")
    {
        e.FilterControl.AscendingSortString = "Sort A to Z";
        e.FilterControl.DescendingSortString = "Sort Z to A";
    }
}
```

### Collapse Sort Options

```xaml
<Style TargetType="syncfusion:GridFilterControl" x:Key="gridFilterControlStyle">
    <Setter Property="SortOptionVisibility" Value="Collapsed" />
</Style>

<syncfusion:SfDataGrid FilterPopupStyle="{StaticResource gridFilterControlStyle}" />
```

### Custom Filter Icon Style

Customize filter icon when filter is applied:

```xaml
<Style TargetType="syncfusion:FilterToggleButton">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="syncfusion:FilterToggleButton">
                <Grid>
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="FilterStates">
                            <VisualState x:Name="Filtered">
                                <Storyboard>
                                    <ColorAnimation Duration="0:0:0:1"
                                                   Storyboard.TargetName="PathFillColor"
                                                   Storyboard.TargetProperty="Color"
                                                   To="Red" />
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="UnFiltered">
                                <Storyboard>
                                    <ColorAnimation Duration="0:0:0:1"
                                                   Storyboard.TargetName="PathFillColor"
                                                   Storyboard.TargetProperty="Color"
                                                   To="Gray" />
                                </Storyboard>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                    
                    <Path Name="PART_FilterToggleButtonIndicator">
                        <Path.Fill>
                            <SolidColorBrush x:Name="PathFillColor" Color="Gray" />
                        </Path.Fill>
                    </Path>
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### Show Images in CheckBox Filter

Display images instead of image paths:

```xaml
<syncfusion:GridTextColumn MappingName="ImageLink">
    <syncfusion:GridTextColumn.FilterPopupStyle>
        <Style TargetType="syncfusion:GridFilterControl">
            <Setter Property="CheckboxFilterStyle">
                <Setter.Value>
                    <Style TargetType="syncfusion:CheckboxFilterControl">
                        <Setter Property="ItemTemplate">
                            <Setter.Value>
                                <DataTemplate>
                                    <CheckBox Content="{Binding}">
                                        <CheckBox.ContentTemplate>
                                            <DataTemplate>
                                                <Image Source="{Binding Path=ActualValue, 
                                                               Converter={StaticResource stringToImageConverter}}"
                                                       Height="25" />
                                            </DataTemplate>
                                        </CheckBox.ContentTemplate>
                                    </CheckBox>
                                </DataTemplate>
                            </Setter.Value>
                        </Setter>
                    </Style>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:GridTextColumn.FilterPopupStyle>
</syncfusion:GridTextColumn>
```

```csharp
public class StringToImageConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        string imageName = value as string;
        return new BitmapImage(new Uri($@"..\..\Images\{imageName}", UriKind.Relative));
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return null;
    }
}
```

## Troubleshooting

### Filtering Not Working

**Issue**: Filter popup doesn't appear or filtering doesn't work

**Solutions**:
1. Ensure `AllowFiltering` is true
2. Verify column has valid `MappingName`
3. Check if `AllowFiltering` is disabled at column level
4. Confirm ItemsSource is bound correctly

```csharp
// Diagnostic check
Debug.WriteLine($"AllowFiltering: {dataGrid.AllowFiltering}");
Debug.WriteLine($"Column AllowFiltering: {dataGrid.Columns["OrderID"].AllowFiltering}");
```

### Filter Predicates Not Applied

**Issue**: Programmatic filter predicates don't filter data

**Solutions**:
1. Verify FilterBehavior is set correctly
2. Check FilterType matches data type
3. Ensure FilterValue is correct type
4. Call RefreshFilter if using View filter

```csharp
// Correct predicate setup
var predicate = new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = 1005,  // Correct type for numeric column
    FilterBehavior = FilterBehavior.StronglyTyped
};

dataGrid.Columns["OrderID"].FilterPredicates.Add(predicate);
```

### Case-Sensitive Filter Not Working

**Issue**: Case-sensitive toggle button missing or not working

**Solutions**:
1. Not supported when ItemsSource is DataTable
2. Verify advanced filter is enabled
3. Check if custom style is overriding default template

### Wrong Filter Type Loaded

**Issue**: Text filter loads for numeric column or vice versa

**Solutions**:
1. Set `ColumnMemberType` for dynamic data
2. Override in `FilterItemsPopulating` event
3. Check column `FilterBehavior` property

```csharp
// For dynamic data
dataGrid.Columns["OrderID"].ColumnMemberType = typeof(int);

// Or in event
void DataGrid_FilterItemsPopulating(object sender, GridFilterItemsPopulatingEventArgs e)
{
    if (e.Column.MappingName == "OrderID")
    {
        e.FilterControl.AdvancedFilterType = AdvancedFilterType.NumberFilter;
    }
}
```

### Filter Popup Performance Issues

**Issue**: Filter popup opens slowly with large datasets

**Solutions**:
1. Set `CanGenerateUniqueItems` to false
2. Use `AdvancedFilter` mode only
3. Implement custom filter logic
4. Consider using server-side filtering

```xaml
<Style TargetType="syncfusion:AdvancedFilterControl">
    <Setter Property="CanGenerateUniqueItems" Value="False" />
</Style>
```

### Null Values Not Filtering Correctly

**Issue**: Null values not appearing or filtering incorrectly

**Solutions**:
1. Set `AllowBlankFilters` to true
2. Handle null values in custom predicates
3. Check data type supports null

```csharp
dataGrid.Columns["Country"].AllowBlankFilters = true;
```

### Filter State Not Persisting

**Issue**: Filters cleared after data refresh

**Solutions**:
1. Save filter predicates before refresh
2. Use View filtering instead of replacing ItemsSource
3. Implement filter persistence

```csharp
// Save filters
private void SaveFilters()
{
    _savedFilters = new Dictionary<string, List<FilterPredicate>>();
    
    foreach (var column in dataGrid.Columns)
    {
        if (column.FilterPredicates.Count > 0)
        {
            _savedFilters[column.MappingName] = column.FilterPredicates.ToList();
        }
    }
}

// Restore filters
private void RestoreFilters()
{
    foreach (var kvp in _savedFilters)
    {
        var column = dataGrid.Columns[kvp.Key];
        column.FilterPredicates.Clear();
        
        foreach (var predicate in kvp.Value)
        {
            column.FilterPredicates.Add(predicate);
        }
    }
}
```

### Custom Filter ItemsSource Not Working

**Issue**: Modified ItemsSource in FilterItemsPopulated not reflected

**Solutions**:
1. Ensure modifications done before popup displays
2. Verify correct event timing
3. Check if items are being recreated

```csharp
void DataGrid_FilterItemsPopulated(object sender, GridFilterItemsPopulatedEventArgs e)
{
    if (e.Column.MappingName == "OrderID")
    {
        var itemsSource = e.ItemsSource as List<FilterElement>;
        
        // Modifications here
        var removeItem = itemsSource.FirstOrDefault(i => i.ActualValue.Equals(1005));
        if (removeItem != null)
        {
            itemsSource.Remove(removeItem);
        }
    }
}
```
