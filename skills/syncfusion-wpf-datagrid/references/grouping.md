# Grouping in WPF DataGrid (SfDataGrid)

Comprehensive reference for grouping operations, configuration, and customization in the Syncfusion WPF DataGrid control.

## Table of Contents
- [Overview](#overview)
- [UI Grouping](#ui-grouping)
- [Programmatic Grouping](#programmatic-grouping)
- [Group Display Options](#group-display-options)
- [Expanding and Collapsing Groups](#expanding-and-collapsing-groups)
- [Custom Grouping](#custom-grouping)
- [Group Customization](#group-customization)
- [Grouping Events](#grouping-events)
- [Troubleshooting](#troubleshooting)

## Overview

WPF DataGrid allows grouping data by one or more columns, organizing records into a hierarchical structure based on matching column values. Grouped data is automatically sorted in ascending order.

**Key Features:**
- UI and programmatic grouping
- Multi-level grouping
- Custom group logic
- Expand/collapse group rows
- Frozen group headers
- Group caption summaries
- Custom group comparers

## UI Grouping

### Enable UI Grouping

Allow users to drag columns to GroupDropArea:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowGrouping="True"
                       ShowGroupDropArea="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowGrouping = true;
dataGrid.ShowGroupDropArea = true;
```

**User Action**: Drag column header to GroupDropArea to create group.

### Column-Level Grouping Control

```xaml
<syncfusion:SfDataGrid AllowGrouping="True" ShowGroupDropArea="True">
    <syncfusion:SfDataGrid.Columns>
        <!-- Grouping enabled -->
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   AllowGrouping="True" />
        
        <!-- Grouping disabled -->
        <syncfusion:GridTextColumn MappingName="CustomerID" 
                                   AllowGrouping="False" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.Columns["OrderID"].AllowGrouping = true;
dataGrid.Columns["CustomerID"].AllowGrouping = false;
```

**Priority**: `GridColumn.AllowGrouping` > `SfDataGrid.AllowGrouping`

### Multi-Level Grouping

Group by multiple columns by dragging additional columns to GroupDropArea:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowGrouping="True"
                       ShowGroupDropArea="True"
                       ItemsSource="{Binding Orders}" />
```

Drag OrderID first, then CustomerName to create nested groups.

## Programmatic Grouping

### Add Group Column Descriptions

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ShowGroupDropArea="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.GroupColumnDescriptions>
        <syncfusion:GroupColumnDescription ColumnName="OrderID" />
        <syncfusion:GroupColumnDescription ColumnName="CustomerID" />
    </syncfusion:SfDataGrid.GroupColumnDescriptions>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription()
{
    ColumnName = "OrderID"
});

dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription()
{
    ColumnName = "CustomerID"
});
```

### Multiple Groups with BeginInit/EndInit

Improve performance when adding multiple groups:

```csharp
dataGrid.View.BeginInit();
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription() { ColumnName = "OrderID" });
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription() { ColumnName = "CustomerID" });
dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription() { ColumnName = "Country" });
dataGrid.View.EndInit();
```

### Group Based on Display Text

Group by displayed value instead of actual value:

```xaml
<syncfusion:GridNumericColumn MappingName="OrderID" 
                             NumberDecimalDigits="1"
                             GroupMode="Display" />
```

```csharp
dataGrid.Columns["OrderID"].GroupMode = DataReflectionMode.Display;
```

### Group Caption for ComboBox Columns

Display text instead of value in group captions:

```xaml
<syncfusion:GridComboBoxColumn ItemsSource="{Binding ComboItemsSource}"
                              MappingName="ShipId"
                              DisplayMemberPath="ShipCity"
                              SelectedValuePath="ShipId"
                              GroupMode="Display" />
```

### Remove Grouping

```csharp
// Clear all groups
dataGrid.GroupColumnDescriptions.Clear();

// Remove specific group
dataGrid.View.BeginInit();
dataGrid.GroupColumnDescriptions.Remove(dataGrid.GroupColumnDescriptions[0]);
dataGrid.View.EndInit();
```

## Group Display Options

### Hide Grouped Column

Hide column header when grouped:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ShowColumnWhenGrouped="False"
                       ShowGroupDropArea="True" />
```

```csharp
dataGrid.ShowColumnWhenGrouped = false;
```

### Freeze Group Headers

Keep group caption rows visible while scrolling:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowFrozenGroupHeaders="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowFrozenGroupHeaders = true;
```

Group caption row stays at top until its records scroll out of view.

### Customize Indent Column Width

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowGrouping="True"
                       IndentColumnWidth="50"
                       ShowGroupDropArea="True" />
```

```csharp
dataGrid.IndentColumnWidth = 50;
```

## Expanding and Collapsing Groups

### Auto-Expand Groups

Automatically expand all groups when grouping is applied:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoExpandGroups="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AutoExpandGroups = true;
```

### Expand/Collapse All Groups

```csharp
// Expand all
dataGrid.ExpandAllGroup();

// Collapse all
dataGrid.CollapseAllGroup();
```

### Expand/Collapse by Level

```csharp
// Expand groups at specific level
dataGrid.ExpandGroupsAtLevel(2);

// Collapse groups at specific level
dataGrid.CollapseGroupsAtLevel(2);
```

### Expand/Collapse Specific Group

```csharp
var group = dataGrid.View.Groups[0] as Group;

// Expand specific group
dataGrid.ExpandGroup(group);

// Collapse specific group
dataGrid.CollapseGroup(group);
```

## Custom Grouping

### Implement Custom Group Logic

Create custom converter for specialized grouping:

```csharp
public class GroupDateTimeConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var saleInfo = value as SalesByDate;
        var dt = DateTime.Now;
        var days = (int)Math.Floor((dt - saleInfo.Date).TotalDays);
        var dayOfWeek = (int)dt.DayOfWeek;
        var difference = days - dayOfWeek;
        
        if (days <= dayOfWeek)
        {
            if (days == 0) return "TODAY";
            if (days == 1) return "YESTERDAY";
            return saleInfo.Date.DayOfWeek.ToString().ToUpper();
        }
        
        if (difference > 0 && difference <= 7)
            return "LAST WEEK";
        if (difference > 7 && difference <= 14)
            return "TWO WEEKS AGO";
        if (difference > 14 && difference <= 21)
            return "THREE WEEKS AGO";
        if (dt.Year == saleInfo.Date.Year && dt.Month == saleInfo.Date.Month)
            return "EARLIER THIS MONTH";
        if (DateTime.Now.AddMonths(-1).Month == saleInfo.Date.Month)
            return "LAST MONTH";
            
        return "OLDER";
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Apply Custom Grouping

```xaml
<Window.Resources>
    <local:GroupDateTimeConverter x:Key="customGroupConverter" />
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding DailySalesDetails}">
    <syncfusion:SfDataGrid.GroupColumnDescriptions>
        <syncfusion:GroupColumnDescription ColumnName="Date" 
                                          Converter="{StaticResource customGroupConverter}" />
    </syncfusion:SfDataGrid.GroupColumnDescriptions>
</syncfusion:SfDataGrid>
```

### Sort Grouped Records

Sort records within each group:

```xaml
<syncfusion:SfDataGrid.GroupColumnDescriptions>
    <syncfusion:GroupColumnDescription ColumnName="SickLeaveHours"
                                      Converter="{StaticResource customGrouping}"
                                      SortGroupRecords="True" />
</syncfusion:SfDataGrid.GroupColumnDescriptions>
```

```csharp
var groupDescriptor = new GroupColumnDescription()
{
    ColumnName = "SickLeaveHours",
    Converter = new CustomGroupingConverter(),
    SortGroupRecords = true
};
dataGrid.GroupColumnDescriptions.Add(groupDescriptor);
```

## Group Customization

### GroupDropArea Customization

**Customize Text:**

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       GroupDropAreaText="Drag columns here to group"
                       ShowGroupDropArea="True" />
```

**Customize Height and Appearance:**

```xaml
<Style TargetType="syncfusion:GroupDropArea">
    <Setter Property="Height" Value="80" />
    <Setter Property="Foreground" Value="Red" />
    <Setter Property="Background" Value="LightGray" />
</Style>
```

**Always Expand GroupDropArea:**

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       IsGroupDropAreaExpanded="True"
                       ShowGroupDropArea="True" />
```

```csharp
dataGrid.IsGroupDropAreaExpanded = true;
```

### Sort Groups by Summary Values

Sort groups based on summary calculations:

```csharp
public class SortGroupComparers : IComparer<Group>, ISortDirection
{
    public ListSortDirection SortDirection { get; set; }
    
    public int Compare(Group x, Group y)
    {
        int cmp = 0;
        var xGroupSummary = Convert.ToInt32((x as Group).GetSummaryValue(
            x.SummaryDetails.SummaryRow.SummaryColumns[0].MappingName, "Count"));
        var yGroupSummary = Convert.ToInt32((y as Group).GetSummaryValue(
            y.SummaryDetails.SummaryRow.SummaryColumns[0].MappingName, "Count"));
        
        cmp = ((IComparable)xGroupSummary).CompareTo(yGroupSummary);
        
        if (SortDirection == ListSortDirection.Descending)
            cmp = -cmp;
            
        return cmp;
    }
}
```

Apply custom group comparer:

```xaml
<Window.Resources>
    <local:SortGroupComparers x:Key="groupComparer" />
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowSorting="True"
                       SummaryGroupComparer="{StaticResource groupComparer}">
    <syncfusion:SfDataGrid.GroupColumnDescriptions>
        <syncfusion:GroupColumnDescription ColumnName="OrderID" />
    </syncfusion:SfDataGrid.GroupColumnDescriptions>
    
    <syncfusion:SfDataGrid.CaptionSummaryRow>
        <syncfusion:GridSummaryRow Title="Items Count: {IDCount}" ShowSummaryInRow="True">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="IDCount"
                                             Format="'{Count}'"
                                             MappingName="OrderID"
                                             SummaryType="CountAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.CaptionSummaryRow>
</syncfusion:SfDataGrid>
```

## Grouping Events

### GroupExpanding Event

Raised when group is being expanded:

```csharp
dataGrid.GroupExpanding += DataGrid_GroupExpanding;

void DataGrid_GroupExpanding(object sender, GroupChangingEventArgs e)
{
    // Cancel expansion for specific group
    if (e.Group.Key.Equals(1001))
    {
        e.Cancel = true;
    }
}
```

**Event Arguments**:
- **Group**: The group being expanded
- **Cancel**: Set to true to cancel expansion

### GroupExpanded Event

Raised after group is expanded:

```csharp
dataGrid.GroupExpanded += DataGrid_GroupExpanded;

void DataGrid_GroupExpanded(object sender, GroupChangedEventArgs e)
{
    // Perform action after expansion
    var expandedGroup = e.Group;
    Debug.WriteLine($"Group expanded: {expandedGroup.Key}");
}
```

### GroupCollapsing Event

Raised when group is being collapsed:

```csharp
dataGrid.GroupCollapsing += DataGrid_GroupCollapsing;

void DataGrid_GroupCollapsing(object sender, GroupChangingEventArgs e)
{
    // Cancel collapse for specific group
    if (e.Group.Key.Equals(1001))
    {
        e.Cancel = true;
    }
}
```

### GroupCollapsed Event

Raised after group is collapsed:

```csharp
dataGrid.GroupCollapsed += DataGrid_GroupCollapsed;

void DataGrid_GroupCollapsed(object sender, GroupChangedEventArgs e)
{
    // Perform action after collapse
    var collapsedGroup = e.Group;
    Debug.WriteLine($"Group collapsed: {collapsedGroup.Key}");
}
```

## Troubleshooting

### Grouping Not Working

**Issue**: Cannot group columns

**Solutions**:
1. Verify `AllowGrouping` is true
2. Enable `ShowGroupDropArea`
3. Check column `AllowGrouping` property
4. Ensure valid `MappingName`

```csharp
// Diagnostic check
Debug.WriteLine($"AllowGrouping: {dataGrid.AllowGrouping}");
Debug.WriteLine($"ShowGroupDropArea: {dataGrid.ShowGroupDropArea}");
Debug.WriteLine($"Column AllowGrouping: {dataGrid.Columns["OrderID"].AllowGrouping}");
```

### Custom Grouping Not Applied

**Issue**: Custom grouping converter not executing

**Solutions**:
1. Verify converter is set in GroupColumnDescription
2. Check converter logic for errors
3. Ensure converter handles all data types (data objects and groups)
4. Test converter independently

```csharp
// Test converter
var converter = new CustomGroupConverter();
var result = converter.Convert(testData, typeof(object), null, CultureInfo.CurrentCulture);
Debug.WriteLine($"Converter result: {result}");
```

### Groups Not Expanding/Collapsing

**Issue**: Groups don't respond to expand/collapse actions

**Solutions**:
1. Check if events are being canceled
2. Verify group reference is correct
3. Ensure UI is not frozen
4. Test with ExpandAllGroup/CollapseAllGroup

```csharp
// Force expand all
dataGrid.ExpandAllGroup();

// Check group state
foreach (var group in dataGrid.View.Groups)
{
    Debug.WriteLine($"Group: {group.Key}, IsExpanded: {(group as Group).IsExpanded}");
}
```

### GroupDropArea Not Visible

**Issue**: GroupDropArea doesn't show

**Solutions**:
1. Set `ShowGroupDropArea` to true
2. Check if style is hiding it
3. Verify height is not zero
4. Ensure grouping is enabled

```xaml
<syncfusion:SfDataGrid AllowGrouping="True"
                       ShowGroupDropArea="True"
                       IsGroupDropAreaExpanded="True" />
```

### Performance Issues with Grouping

**Issue**: Slow performance with large grouped datasets

**Solutions**:
1. Use `BeginInit/EndInit` for multiple groups
2. Disable `AutoExpandGroups` if not needed
3. Limit number of group levels
4. Optimize custom grouping logic

```csharp
// Efficient multiple grouping
dataGrid.View.BeginInit();
dataGrid.AutoExpandGroups = false;  // Don't auto-expand

foreach (var columnName in groupColumns)
{
    dataGrid.GroupColumnDescriptions.Add(new GroupColumnDescription()
    {
        ColumnName = columnName
    });
}

dataGrid.View.EndInit();
```

### Frozen Headers Not Working

**Issue**: Group headers don't freeze while scrolling

**Solutions**:
1. Verify `AllowFrozenGroupHeaders` is true
2. Check if virtualization is enabled
3. Ensure scrolling is working correctly
4. Test with simple grouping first

```csharp
dataGrid.AllowFrozenGroupHeaders = true;
```

### Group Summary Not Displaying

**Issue**: Caption summary rows not showing or incorrect

**Solutions**:
1. Define CaptionSummaryRow in XAML or code
2. Verify summary column mapping names
3. Check summary format string
4. Ensure data type supports aggregation

```xaml
<syncfusion:SfDataGrid.CaptionSummaryRow>
    <syncfusion:GridSummaryRow ShowSummaryInRow="True" Title="Total: {Sum}">
        <syncfusion:GridSummaryRow.SummaryColumns>
            <syncfusion:GridSummaryColumn Name="Sum"
                                         MappingName="UnitPrice"
                                         SummaryType="DoubleAggregate"
                                         Format="'{Sum:C}'" />
        </syncfusion:GridSummaryRow.SummaryColumns>
    </syncfusion:GridSummaryRow>
</syncfusion:SfDataGrid.CaptionSummaryRow>
```
