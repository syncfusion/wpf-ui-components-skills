# Summaries in WPF DataGrid

## Overview

SfDataGrid provides support to display concise information about data objects using three types of summaries:

- **Table Summary** – Display summary at top or bottom of DataGrid
- **Group Summary** – Display summary in each group
- **Caption Summary** – Display summary in group caption

## Table Summary

Table summary calculates values over all records and displays at the top or bottom of DataGrid.

### Defining Summary for Column

Display summary in specific columns:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"                               
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.TableSummaryRows>
        <syncfusion:GridTableSummaryRow ShowSummaryInRow="False">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'Total UnitPrice : {Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
                <syncfusion:GridSummaryColumn Name="ProductCount"
                                              Format="'Total Product Count : {Count:d}'"
                                              MappingName="ProductName"
                                              SummaryType="CountAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridTableSummaryRow>
    </syncfusion:SfDataGrid.TableSummaryRows>
</syncfusion:SfDataGrid>
```

```csharp
this.dataGrid.TableSummaryRows.Add(new GridTableSummaryRow()
{
    ShowSummaryInRow = false,
    SummaryColumns = new ObservableCollection<ISummaryColumn>()
    {
        new GridSummaryColumn()
        { 
            Name = "PriceAmount", 
            MappingName="UnitPrice", 
            SummaryType= SummaryType.DoubleAggregate, 
            Format="Total UnitPrice : {Sum:c}"
        }, 
        new GridSummaryColumn()
        {
            Name="ProductCount",
            MappingName="ProductName",
            SummaryType=SummaryType.CountAggregate,
            Format="Total Product Count : {Count:d}"
        },
    }
});
```

### Displaying Summary for Row

Display summary information in a single row with title:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.TableSummaryRows>
        <syncfusion:GridTableSummaryRow Title=" Total Price : {PriceAmount} for {ProductCount} products " ShowSummaryInRow="True">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'{Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
                <syncfusion:GridSummaryColumn Name="ProductCount"
                                              Format="'{Count:d}'"
                                              MappingName="ProductName"
                                              SummaryType="CountAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridTableSummaryRow>
    </syncfusion:SfDataGrid.TableSummaryRows>
</syncfusion:SfDataGrid>
```

### Positioning Table Summary

Position summary at top or bottom:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.TableSummaryRows>
        <syncfusion:GridTableSummaryRow Position="Top" ShowSummaryInRow="False">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'{Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridTableSummaryRow>
        <syncfusion:GridTableSummaryRow Position="Bottom" ShowSummaryInRow="True"
                                        Title="Total Product count: {ProductCount}">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="ProductCount"
                                              Format="'{Count:d}'"
                                              MappingName="ProductName"
                                              SummaryType="CountAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridTableSummaryRow>
    </syncfusion:SfDataGrid.TableSummaryRows>
</syncfusion:SfDataGrid>
```

## Group Summary

Group summary calculates values based on records in each group.

### Defining Summary for Column

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"                             
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}"
                       ShowGroupDropArea="True">
    <syncfusion:SfDataGrid.GroupSummaryRows>
        <syncfusion:GridSummaryRow ShowSummaryInRow="False">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'Amount - {Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
                <syncfusion:GridSummaryColumn Name="ProductCount"
                                              Format="'Count - {Count:d}'"
                                              MappingName="ProductName"
                                              SummaryType="CountAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.GroupSummaryRows>
</syncfusion:SfDataGrid>
```

### Displaying Summary for Row

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}"
                       ShowGroupDropArea="True">
    <syncfusion:SfDataGrid.GroupSummaryRows>
        <syncfusion:GridSummaryRow Title="Total Price : {PriceAmount} for {ProductCount} products" ShowSummaryInRow="True">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'{Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
                <syncfusion:GridSummaryColumn Name="ProductCount"
                                              Format="'{Count:d}'"
                                              MappingName="ProductName"
                                              SummaryType="CountAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.GroupSummaryRows>
</syncfusion:SfDataGrid>
```

## Caption Summary

Caption summary displays in the caption of groups. Built-in format: `{ColumnName}: {Key} - {ItemsCount} Items`

### Formatting Built-in Caption Summary

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowGrouping="True"
                       AutoGenerateColumns="True"
                       GroupCaptionTextFormat="{ColumnName}: {Key}"
                       ItemsSource="{Binding Orders}"
                       ShowGroupDropArea="True"/>
```

### Defining Summary for Column

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}"
                       ShowGroupDropArea="True">
    <syncfusion:SfDataGrid.CaptionSummaryRow>
        <syncfusion:GridSummaryRow ShowSummaryInRow="False">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'{Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
                <syncfusion:GridSummaryColumn Name="ProductCount"
                                              Format="'{Count:d}'"
                                              MappingName="ProductName"
                                              SummaryType="CountAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.CaptionSummaryRow>
</syncfusion:SfDataGrid>
```

### Displaying Summary for Row

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}"
                       ShowGroupDropArea="True">
    <syncfusion:SfDataGrid.CaptionSummaryRow>
        <syncfusion:GridSummaryRow Title="Total Price : {PriceAmount} for {ProductCount} Products" ShowSummaryInRow="True">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'{Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
                <syncfusion:GridSummaryColumn Name="ProductCount"
                                              Format="'{Count:d}'"
                                              MappingName="ProductName"
                                              SummaryType="CountAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.CaptionSummaryRow>
</syncfusion:SfDataGrid>
```

## Built-in Aggregates

### Count Aggregate
Count of values in the column:

```xaml
<syncfusion:GridSummaryColumn Name="ProductCount"
                              Format="'Count: {Count:d}'"
                              MappingName="ProductName"
                              SummaryType="CountAggregate" />
```

### Sum Aggregate
Sum of values:

```xaml
<syncfusion:GridSummaryColumn Name="TotalPrice"
                              Format="'Total: {Sum:c}'"
                              MappingName="UnitPrice"
                              SummaryType="DoubleAggregate" />
```

### Average Aggregate
Average of values:

```xaml
<syncfusion:GridSummaryColumn Name="AveragePrice"
                              Format="'Average: {Average:c}'"
                              MappingName="UnitPrice"
                              SummaryType="DoubleAggregate" />
```

### Min Aggregate
Minimum value:

```xaml
<syncfusion:GridSummaryColumn Name="MinPrice"
                              Format="'Min: {Min:c}'"
                              MappingName="UnitPrice"
                              SummaryType="DoubleAggregate" />
```

### Max Aggregate
Maximum value:

```xaml
<syncfusion:GridSummaryColumn Name="MaxPrice"
                              Format="'Max: {Max:c}'"
                              MappingName="UnitPrice"
                              SummaryType="DoubleAggregate" />
```

## Custom Summary Calculations

Implement custom aggregate functions:

```csharp
public class CustomAggregate : ISummaryAggregate
{
    public CustomAggregate()
    {
    }

    public double StdDev { get; set; }

    public Action<IEnumerable, string, PropertyDescriptor> CalculateAggregateFunc()
    {
        return (items, property, pd) =>
        {
            var enumerableItems = items as IEnumerable<OrderInfo>;
            if (pd.Name == "UnitPrice")
            {
                var values = enumerableItems.Select(x => x.UnitPrice);
                var count = values.Count();
                var avg = values.Average();
                var sumOfSquares = values.Sum(x => Math.Pow(x - avg, 2));
                StdDev = Math.Sqrt(sumOfSquares / count);
            }
        };
    }
}
```

Use custom aggregate:

```csharp
this.dataGrid.TableSummaryRows.Add(new GridTableSummaryRow()
{
    ShowSummaryInRow = false,
    SummaryColumns = new ObservableCollection<ISummaryColumn>()
    {
        new GridSummaryColumn()
        {
            Name = "StdDev",
            CustomAggregate = new CustomAggregate(),
            MappingName = "UnitPrice",
            Format = "Standard Deviation: {StdDev}",
            SummaryType = SummaryType.Custom
        }
    }
});
```

## Summary Templates

### Table Summary Template

```xaml
<Window.Resources>
    <local:TableSummaryConverter x:Key="summaryConverter" />
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.TableSummaryRows>
        <syncfusion:GridTableSummaryRow Title="Total Price : {PriceAmount}" ShowSummaryInRow="True" Position="Bottom">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'{Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
            <syncfusion:GridSummaryRow.TitleTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding Converter={StaticResource summaryConverter}}" 
                               Foreground="Blue" Background="Yellow" FontSize="15"/>
                </DataTemplate>
            </syncfusion:GridSummaryRow.TitleTemplate>
        </syncfusion:GridTableSummaryRow>
    </syncfusion:SfDataGrid.TableSummaryRows>
</syncfusion:SfDataGrid>
```

```csharp
public class TableSummaryConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var data = value as SummaryRecordEntry;
        if (data != null)
        {
            var summaryText = SummaryCreator.GetSummaryDisplayText(data, "UnitPrice", null);
            return $"Total Price : {summaryText}";
        }
        return null;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return null;
    }
}
```

### Group Summary Template

```xaml
<Window.Resources>
    <local:GroupSummaryConverter x:Key="groupSummaryConverter"/>
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}"
                       ShowGroupDropArea="True">
    <syncfusion:SfDataGrid.GroupSummaryRows>
        <syncfusion:GridSummaryRow Title="Total Price : {PriceAmount}" ShowSummaryInRow="True">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'{Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
            <syncfusion:GridSummaryRow.TitleTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding Converter={StaticResource groupSummaryConverter}}" 
                               Foreground="Blue" Background="Yellow" FontSize="15"/>
                </DataTemplate>
            </syncfusion:GridSummaryRow.TitleTemplate>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.GroupSummaryRows>
</syncfusion:SfDataGrid>
```

### Caption Summary Template

```xaml
<Window.Resources>
    <local:CaptionSummaryConverter x:Key="captionSummaryConverter"/>
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}"
                       ShowGroupDropArea="True">
    <syncfusion:SfDataGrid.CaptionSummaryRow>
        <syncfusion:GridSummaryRow Title="Total Price : {PriceAmount}" ShowSummaryInRow="True">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="PriceAmount"
                                              Format="'{Sum:c}'"
                                              MappingName="UnitPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
            <syncfusion:GridSummaryRow.TitleTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding Converter={StaticResource captionSummaryConverter}}" 
                               Foreground="Blue" Background="Yellow" FontSize="15"/>
                </DataTemplate>
            </syncfusion:GridSummaryRow.TitleTemplate>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.CaptionSummaryRow>
</syncfusion:SfDataGrid>
```

## ShowSummaryInRow Property

Control whether summary is displayed in row or column:

```xaml
<!-- Display in separate columns -->
<syncfusion:GridSummaryRow ShowSummaryInRow="False">
    <syncfusion:GridSummaryRow.SummaryColumns>
        <syncfusion:GridSummaryColumn Name="Price" Format="'{Sum:c}'" 
                                      MappingName="UnitPrice" SummaryType="DoubleAggregate" />
    </syncfusion:GridSummaryRow.SummaryColumns>
</syncfusion:GridSummaryRow>

<!-- Display in single row with title -->
<syncfusion:GridSummaryRow Title="Total: {Price}" ShowSummaryInRow="True">
    <syncfusion:GridSummaryRow.SummaryColumns>
        <syncfusion:GridSummaryColumn Name="Price" Format="'{Sum:c}'" 
                                      MappingName="UnitPrice" SummaryType="DoubleAggregate" />
    </syncfusion:GridSummaryRow.SummaryColumns>
</syncfusion:GridSummaryRow>
```

## Troubleshooting

### Issue: Summary not displaying
**Solution**: Verify that `ShowSummaryInRow` is set correctly. Check that `MappingName` matches column property name. Ensure `SummaryType` is appropriate for data type.

### Issue: Custom aggregate not working
**Solution**: Ensure `CalculateAggregateFunc` is implemented correctly. Verify that `CustomAggregate` property is set. Check that `SummaryType` is set to `Custom`.

### Issue: Summary format not applied
**Solution**: Use correct format string syntax. For aggregates, use `{FunctionName:format}` like `{Sum:c}` for currency. For custom aggregates, use property name like `{PropertyName}`.

### Issue: Group summary showing incorrect values
**Solution**: Verify grouping is applied correctly. Check that summary is calculated on correct column. Ensure data types match aggregate function.

### Issue: Caption summary not visible
**Solution**: Ensure `AllowGrouping` is enabled. Verify `CaptionSummaryRow` is defined. Check that groups are expanded.

### Issue: Template not applied
**Solution**: Verify converter is implemented correctly. Check DataTemplate bindings. Ensure resource key matches reference.
