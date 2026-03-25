# Conditional Styling for WPF TreeGrid

## Table of Contents

- [Overview](#overview)
- [Converter-Based Styling](#converter-based-styling)
- [DataTrigger Styling](#datatrigger-styling)
- [StyleSelector](#styleselector)
- [Row Styling](#row-styling)
- [Cell Styling](#cell-styling)
- [Header Styling](#header-styling)
- [Performance Considerations](#performance-considerations)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid supports conditional styling through converters, data triggers, and style selectors.

## Converter-Based Styling

### Value Converter

```csharp
public class SalaryToColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is decimal salary)
        {
            if (salary > 80000)
                return Brushes.Green;
            else if (salary > 50000)
                return Brushes.Orange;
            else
                return Brushes.Red;
        }
        return Brushes.Black;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Apply Converter in Column

```xaml
<Window.Resources>
    <local:SalaryToColorConverter x:Key="SalaryConverter"/>
</Window.Resources>

<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="Salary">
            <syncfusion:TreeGridTextColumn.CellStyle>
                <Style TargetType="syncfusion:TreeGridCell">
                    <Setter Property="Foreground" 
                           Value="{Binding Salary, Converter={StaticResource SalaryConverter}}"/>
                </Style>
            </syncfusion:TreeGridTextColumn.CellStyle>
        </syncfusion:TreeGridTextColumn>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

## DataTrigger Styling

### Single Trigger

```xaml
<syncfusion:TreeGridTextColumn MappingName="Status">
    <syncfusion:TreeGridTextColumn.CellStyle>
        <Style TargetType="syncfusion:TreeGridCell">
            <Style.Triggers>
                <DataTrigger Binding="{Binding Status}" Value="Active">
                    <Setter Property="Background" Value="LightGreen"/>
                    <Setter Property="Foreground" Value="DarkGreen"/>
                </DataTrigger>
                <DataTrigger Binding="{Binding Status}" Value="Inactive">
                    <Setter Property="Background" Value="LightCoral"/>
                    <Setter Property="Foreground" Value="DarkRed"/>
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </syncfusion:TreeGridTextColumn.CellStyle>
</syncfusion:TreeGridTextColumn>
```

### MultiDataTrigger

```xaml
<Style TargetType="syncfusion:TreeGridCell">
    <Style.Triggers>
        <MultiDataTrigger>
            <MultiDataTrigger.Conditions>
                <Condition Binding="{Binding Department}" Value="Sales"/>
                <Condition Binding="{Binding Salary, Converter={StaticResource SalaryRangeConverter}}" Value="True"/>
            </MultiDataTrigger.Conditions>
            <Setter Property="Background" Value="Gold"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </MultiDataTrigger>
    </Style.Triggers>
</Style>
```

## StyleSelector

### Cell StyleSelector

```csharp
public class EmployeeCellStyleSelector : StyleSelector
{
    public Style HighSalaryStyle { get; set; }
    public Style MediumSalaryStyle { get; set; }
    public Style DefaultStyle { get; set; }

    public override Style SelectStyle(object item, DependencyObject container)
    {
        var employee = item as Employee;
        if (employee == null)
            return DefaultStyle;

        if (employee.Salary > 80000)
            return HighSalaryStyle;
        else if (employee.Salary > 50000)
            return MediumSalaryStyle;
        else
            return DefaultStyle;
    }
}
```

### Apply StyleSelector

```xaml
<Window.Resources>
    <local:EmployeeCellStyleSelector x:Key="CellStyleSelector">
        <local:EmployeeCellStyleSelector.HighSalaryStyle>
            <Style TargetType="syncfusion:TreeGridCell">
                <Setter Property="Background" Value="LightGreen"/>
                <Setter Property="Foreground" Value="DarkGreen"/>
            </Style>
        </local:EmployeeCellStyleSelector.HighSalaryStyle>
        <local:EmployeeCellStyleSelector.MediumSalaryStyle>
            <Style TargetType="syncfusion:TreeGridCell">
                <Setter Property="Background" Value="LightYellow"/>
            </Style>
        </local:EmployeeCellStyleSelector.MediumSalaryStyle>
        <local:EmployeeCellStyleSelector.DefaultStyle>
            <Style TargetType="syncfusion:TreeGridCell"/>
        </local:EmployeeCellStyleSelector.DefaultStyle>
    </local:EmployeeCellStyleSelector>
</Window.Resources>

<syncfusion:TreeGridTextColumn MappingName="Salary" 
                               CellStyleSelector="{StaticResource CellStyleSelector}"/>
```

## Row Styling

### Row StyleSelector

```csharp
public class EmployeeRowStyleSelector : StyleSelector
{
    public Style ManagerStyle { get; set; }
    public Style DeveloperStyle { get; set; }
    public Style DefaultStyle { get; set; }

    public override Style SelectStyle(object item, DependencyObject container)
    {
        var node = item as TreeNode;
        if (node == null)
            return DefaultStyle;

        var employee = node.Item as Employee;
        if (employee == null)
            return DefaultStyle;

        if (employee.Title == "Manager")
            return ManagerStyle;
        else if (employee.Title == "Developer")
            return DeveloperStyle;
        else
            return DefaultStyle;
    }
}
```

```xaml
<syncfusion:SfTreeGrid RowStyleSelector="{StaticResource RowStyleSelector}"/>
```

## Cell Styling

### Alternating Row Colors

```xaml
<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.AlternatingRowStyle>
        <Style TargetType="syncfusion:TreeGridRowControl">
            <Setter Property="Background" Value="AliceBlue"/>
        </Style>
    </syncfusion:SfTreeGrid.AlternatingRowStyle>
</syncfusion:SfTreeGrid>
```

### Conditional Cell Background

```xaml
<syncfusion:TreeGridNumericColumn MappingName="Salary">
    <syncfusion:TreeGridNumericColumn.CellStyle>
        <Style TargetType="syncfusion:TreeGridCell">
            <Style.Triggers>
                <DataTrigger Binding="{Binding Path=Salary, 
                             Converter={StaticResource SalaryRangeConverter}, 
                             ConverterParameter=High}" 
                             Value="True">
                    <Setter Property="Background" Value="#FFE5F7E5"/>
                    <Setter Property="Foreground" Value="#FF2E7D32"/>
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </syncfusion:TreeGridNumericColumn.CellStyle>
</syncfusion:TreeGridNumericColumn>
```

## Header Styling

```xaml
<syncfusion:TreeGridTextColumn MappingName="FirstName">
    <syncfusion:TreeGridTextColumn.HeaderStyle>
        <Style TargetType="syncfusion:TreeGridHeaderCell">
            <Setter Property="Background" Value="Navy"/>
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>
    </syncfusion:TreeGridTextColumn.HeaderStyle>
</syncfusion:TreeGridTextColumn>
```

## Image in Cells

```xaml
<syncfusion:TreeGridTemplateColumn MappingName="Status">
    <syncfusion:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Width="16" Height="16">
                    <Image.Style>
                        <Style TargetType="Image">
                            <Style.Triggers>
                                <DataTrigger Binding="{Binding Status}" Value="Active">
                                    <Setter Property="Source" Value="/Images/check.png"/>
                                </DataTrigger>
                                <DataTrigger Binding="{Binding Status}" Value="Inactive">
                                    <Setter Property="Source" Value="/Images/cross.png"/>
                                </DataTrigger>
                            </Style.Triggers>
                        </Style>
                    </Image.Style>
                </Image>
                <TextBlock Text="{Binding Status}" Margin="5,0,0,0"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.CellTemplate>
</syncfusion:TreeGridTemplateColumn>
```

## Performance Considerations

### Converter vs DataTrigger vs StyleSelector

```
Performance: DataTrigger > Converter > StyleSelector

DataTrigger:
- Best performance
- Limited to simple conditions
- XAML-based

Converter:
- Good performance
- More flexible
- Reusable

StyleSelector:
- Most flexible
- Slight performance overhead
- Complex logic
```

### Best Performance Approach

```csharp
// For simple conditions, use DataTrigger
// For complex logic, use StyleSelector with caching

public class CachedStyleSelector : StyleSelector
{
    private Dictionary<string, Style> _styleCache = new Dictionary<string, Style>();

    public override Style SelectStyle(object item, DependencyObject container)
    {
        var employee = item as Employee;
        var key = GetStyleKey(employee);
        
        if (!_styleCache.ContainsKey(key))
        {
            _styleCache[key] = CreateStyle(employee);
        }
        
        return _styleCache[key];
    }
}
```

## Best Practices

### 1. Use Appropriate Styling Method

```csharp
// Simple value-based: Use DataTrigger
// Value transformation: Use Converter
// Complex business logic: Use StyleSelector
```

### 2. Optimize StyleSelector

```csharp
// Cache styles when possible
// Minimize object allocations
// Use simple comparisons
```

### 3. Leverage Freezable Objects

```csharp
public Style GetStyle()
{
    var style = new Style(typeof(TreeGridCell));
    style.Setters.Add(new Setter(TreeGridCell.BackgroundProperty, Brushes.Red));
    
    // Freeze for better performance
    if (style.CanFreeze)
        style.Freeze();
    
    return style;
}
```

## Troubleshooting

### Issue: Styles Not Applying

```csharp
// Verify TargetType matches
<Style TargetType="syncfusion:TreeGridCell"> // Correct

// Check binding path
<DataTrigger Binding="{Binding Salary}" Value="50000"> // Verify property name
```

### Issue: StyleSelector Not Called

```csharp
// Ensure StyleSelector is set on column or grid
column.CellStyleSelector = new EmployeeCellStyleSelector();

// Verify SelectStyle returns a valid style
public override Style SelectStyle(object item, DependencyObject container)
{
    return DefaultStyle; // Never return null
}
```

### Issue: Performance Problems

```csharp
// Use DataTrigger for simple conditions
// Cache styles in StyleSelector
// Avoid complex converters in large grids
```
