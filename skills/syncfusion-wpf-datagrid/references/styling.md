# Conditional Styling in WPF DataGrid

## Table of Contents
- [Overview](#overview)
- [Cell Style](#cell-style)
- [Row Style](#row-style)
- [Alternate Row Style](#alternate-row-style)
- [Header Style](#header-style)
- [Group Caption Style](#group-caption-style)
- [Summary Cell Style](#summary-cell-style)
- [Summary Row Style](#summary-row-style)
- [Visual States](#visual-states)
- [SfSkinManager Themes](#sfskinmanager-themes)
- [Troubleshooting](#troubleshooting)

## Overview

SfDataGrid provides three approaches for conditional styling:

1. **Using Converter** - Best performance, recommended for most scenarios
2. **Using Data Triggers** - Good for simple conditions, slower with many columns/rows
3. **Using StyleSelector** - Most flexible, affects scrolling performance

## Cell Style

### Using Converter

Style cells based on cell value or data object:

```xaml
<Window.Resources>
    <local:ColorConverter x:Key="converter"/>
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="TotalPrice">
            <syncfusion:GridTextColumn.CellStyle>
                <Style TargetType="syncfusion:GridCell">
                    <Setter Property="Background" Value="{Binding Path=OrderID, Converter={StaticResource converter}}"/>
                </Style>
            </syncfusion:GridTextColumn.CellStyle>
        </syncfusion:GridTextColumn>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
public class ColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        int input = (int)value;
        
        if (input < 1003)
            return new SolidColorBrush(Colors.LightBlue);
        else if (input < 1007)
            return new SolidColorBrush(Colors.Bisque);
        else
            return DependencyProperty.UnsetValue;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Style Based on Entire Record

```xaml
<Window.Resources>
    <local:ColorConverter x:Key="converter"/>
    <Style TargetType="syncfusion:GridCell">
        <Setter Property="Background" Value="{Binding Converter={StaticResource converter}}"/>
    </Style>
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}" />
```

```csharp
public class ColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var data = value as OrderInfo;
        
        if (data.OrderID < 1003)
            return new SolidColorBrush(Colors.LightBlue);
        else if (data.OrderID < 1007)
            return new SolidColorBrush(Colors.Bisque);
        else
            return DependencyProperty.UnsetValue;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Using Data Triggers

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID">
            <syncfusion:GridTextColumn.CellStyle>
                <Style TargetType="syncfusion:GridCell">
                    <Style.Triggers>
                        <DataTrigger Binding="{Binding Path=OrderID}" Value="1001">
                            <Setter Property="Background" Value="Bisque" />
                        </DataTrigger>
                        <MultiDataTrigger>
                            <MultiDataTrigger.Conditions>
                                <Condition Binding="{Binding Path=OrderID}" Value="1008" />
                                <Condition Binding="{Binding Path=CustomerID}" Value="BOLID" />
                            </MultiDataTrigger.Conditions>
                            <Setter Property="Background" Value="LightBlue" />
                        </MultiDataTrigger>
                    </Style.Triggers>
                </Style>
            </syncfusion:GridTextColumn.CellStyle>
        </syncfusion:GridTextColumn>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

### Using CellStyleSelector

```xaml
<Application.Resources>
    <local:SelectorClass x:Key="styleSelector"/>
    <Style x:Key="redCellStyle" TargetType="syncfusion:GridCell">
        <Setter Property="Foreground" Value="Red" />
    </Style>
    <Style x:Key="blueCellStyle" TargetType="syncfusion:GridCell">
        <Setter Property="Foreground" Value="DarkBlue" />
    </Style>    
</Application.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}" 
                       CellStyleSelector="{StaticResource styleSelector}"/>
```

```csharp
public class SelectorClass : StyleSelector
{
    public override Style SelectStyle(object item, DependencyObject container)
    {
        var data = item as OrderInfo;
        
        if (data != null && ((container as GridCell).ColumnBase.GridColumn.MappingName == "TotalPrice"))
        {
            if (data.TotalPrice < 1005)
                return App.Current.Resources["redCellStyle"] as Style;
            return App.Current.Resources["blueCellStyle"] as Style;
        }
        return base.SelectStyle(item, container);
    }
}
```

### Column-Specific CellStyleSelector

Apply StyleSelector to specific column:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="TotalPrice" 
                                   CellStyleSelector="{StaticResource styleSelector}"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

## Row Style

### Using Converter

```xaml
<Window.Resources>
    <local:ColorConverter x:Key="converter"/>
    <Style TargetType="syncfusion:VirtualizingCellsControl">
        <Setter Property="Background" Value="{Binding Converter={StaticResource converter}}" />
    </Style>
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}"/>
```

```csharp
public class ColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var input = (value as OrderInfo).TotalPrice;
        
        if (input < 1003)
            return new SolidColorBrush(Colors.Bisque);
        else if (input < 1007)
            return new SolidColorBrush(Colors.LightBlue);
        else
            return DependencyProperty.UnsetValue;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Using RowStyleSelector

```xaml
<Application.Resources>
    <Style x:Key="rowStyle1" TargetType="syncfusion:VirtualizingCellsControl">
        <Setter Property="Background" Value="Bisque" />
    </Style>
    <Style x:Key="rowStyle2" TargetType="syncfusion:VirtualizingCellsControl">
        <Setter Property="Background" Value="Aqua" />
    </Style>
</Application.Resources>

<Window.Resources>
    <local:CustomRowStyleSelector x:Key="rowStyleSelector" />
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"
                       RowStyleSelector="{StaticResource rowStyleSelector}"/>
```

```csharp
public class CustomRowStyleSelector : StyleSelector
{
    public override Style SelectStyle(object item, DependencyObject container)
    {
        var row = (item as DataRowBase).RowData;
        var data = row as OrderInfo;
        
        if (data.TotalPrice < 1004)
            return App.Current.Resources["rowStyle1"] as Style;
        return App.Current.Resources["rowStyle2"] as Style;
    }
}
```

## Alternate Row Style

### Using AlternatingRowStyleSelector

```xaml
<Application.Resources>
    <Style x:Key="rowStyle1" TargetType="syncfusion:VirtualizingCellsControl">
        <Setter Property="Background" Value="Bisque" />
    </Style>
    <Style x:Key="rowStyle2" TargetType="syncfusion:VirtualizingCellsControl">
        <Setter Property="Background" Value="Aqua" />
    </Style>
</Application.Resources>

<Window.Resources>
    <local:CustomRowStyleSelector x:Key="alternatingRowStyleSelector" />
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"                             
                       AlternatingRowStyleSelector="{StaticResource alternatingRowStyleSelector}"/>
```

```csharp
public class CustomRowStyleSelector : StyleSelector
{
    public override Style SelectStyle(object item, DependencyObject container)
    {
        var row = (item as DataRowBase).RowData;
        var data = row as OrderInfo;
        
        if (data.OrderID < 1006)
            return App.Current.Resources["rowStyle1"] as Style;
        return App.Current.Resources["rowStyle2"] as Style;
    }
}
```

## Header Style

### Using HeaderStyle

```xaml
<Window.Resources>
    <Style x:Key="headerStyle" TargetType="syncfusion:GridHeaderCellControl">
        <Setter Property="Background" Value="Red" />
        <Setter Property="Foreground" Value="White" />
        <Setter Property="FontWeight" Value="Bold" />
    </Style>
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"
                       HeaderStyle="{StaticResource headerStyle}"/>
```

### Column-Specific Header Style

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID">
            <syncfusion:GridTextColumn.HeaderStyle>
                <Style TargetType="syncfusion:GridHeaderCellControl">
                    <Setter Property="Background" Value="LightBlue" />
                </Style>
            </syncfusion:GridTextColumn.HeaderStyle>
        </syncfusion:GridTextColumn>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

## Group Caption Style

### Using GroupCaptionStyle

```xaml
<Window.Resources>
    <Style x:Key="groupCaptionStyle" TargetType="syncfusion:GroupCaptionControl">
        <Setter Property="Background" Value="LightGray" />
        <Setter Property="Foreground" Value="DarkBlue" />
    </Style>
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"
                       ShowGroupDropArea="True"
                       GroupCaptionStyle="{StaticResource groupCaptionStyle}"/>
```

## Summary Cell Style

### Caption Summary Cell Style

```xaml
<Application.Resources>
    <Style TargetType="syncfusion:GridCaptionSummaryCell" x:Key="captionSummaryStyle">
        <Setter Property="Foreground" Value="Red"/>
    </Style>
</Application.Resources>

<Window.Resources>
    <local:SelectorClass x:Key="selector"/>
</Window.Resources>	

<syncfusion:SfDataGrid x:Name="dataGrid" 
                       ShowGroupDropArea="True"
                       CaptionSummaryCellStyleSelector="{StaticResource selector}"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.CaptionSummaryRow>
        <syncfusion:GridSummaryRow ShowSummaryInRow="False">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="price"
                                              Format="'{Sum:c}'"
                                              MappingName="TotalPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.CaptionSummaryRow>
</syncfusion:SfDataGrid>
```

```csharp
public class SelectorClass : StyleSelector
{
    public override Style SelectStyle(object item, DependencyObject container)
    {
        var summaryValue = (item as Group).SummaryDetails.SummaryValues[0];
        var aggregateValue = summaryValue.AggregateValues.ElementAt(0);
        var calculatedValue = aggregateValue.Value;
        
        if ((double)calculatedValue < 0)
            return App.Current.Resources["captionSummaryStyle"] as Style;
        return base.SelectStyle(item, container);
    }
}
```

### Group Summary Cell Style

```xaml
<Application.Resources>
    <Style TargetType="syncfusion:GridGroupSummaryCell" x:Key="customGroupSummary">
        <Setter Property="Foreground" Value="DarkBlue"/>
    </Style>
    <Style TargetType="syncfusion:GridGroupSummaryCell" x:Key="customGroupSummary1">
        <Setter Property="Background" Value="Red"/>
    </Style>
</Application.Resources>

<Window.Resources>
    <local:SelectorClass x:Key="styleSelector"/>        
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ShowGroupDropArea="True"
                       GroupSummaryCellStyleSelector="{StaticResource styleSelector}"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.GroupSummaryRows>
        <syncfusion:GridSummaryRow ShowSummaryInRow="False">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="price"
                                              Format="'{Sum:c}'"
                                              MappingName="TotalPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.GroupSummaryRows>
</syncfusion:SfDataGrid>
```

### Table Summary Cell Style

```xaml
<Application.Resources>
    <Style TargetType="syncfusion:GridTableSummaryCell" x:Key="customTableSummary">
        <Setter Property="Foreground" Value="Red"/>
    </Style>
</Application.Resources>

<Window.Resources>
    <local:SelectorClass x:Key="styleSelector"/>        
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"    
                       ShowRowHeader="True"            
                       ItemsSource="{Binding Orders}"
                       TableSummaryCellStyleSelector="{StaticResource styleSelector}">
    <syncfusion:SfDataGrid.TableSummaryRows>
        <syncfusion:GridTableSummaryRow Position="Top" ShowSummaryInRow="False">
            <syncfusion:GridTableSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="price"
                                              Format="'{Sum:c}'"
                                              MappingName="TotalPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridTableSummaryRow.SummaryColumns>
        </syncfusion:GridTableSummaryRow>
    </syncfusion:SfDataGrid.TableSummaryRows>
</syncfusion:SfDataGrid>
```

## Summary Row Style

### Caption Summary Row Style

```xaml
<Application.Resources>
    <Style TargetType="syncfusion:CaptionSummaryRowControl" x:Key="captionSummaryStyle">
        <Setter Property="Background" Value="Bisque"/>
    </Style>
</Application.Resources>

<Window.Resources>
    <local:SelectorClass x:Key="styleSelector"/>    
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid" 
                       ShowGroupDropArea="True"
                       CaptionSummaryRowStyleSelector="{StaticResource styleSelector}"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.CaptionSummaryRow>
        <syncfusion:GridSummaryRow Title="Total Price : {price}" ShowSummaryInRow="True">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="price"
                                              Format="'{Sum:c}'"
                                              MappingName="TotalPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.CaptionSummaryRow>
</syncfusion:SfDataGrid>
```

### Group Summary Row Style

```xaml
<Application.Resources>
    <Style TargetType="syncfusion:GroupSummaryRowControl" x:Key="customGroupSummary">
        <Setter Property="Background" Value="LightBlue"/>
    </Style>
</Application.Resources>

<Window.Resources>
    <local:SelectorClass x:Key="styleSelector"/>    
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       GroupSummaryRowStyleSelector="{StaticResource styleSelector}"
                       ShowGroupDropArea="True"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.GroupSummaryRows>
        <syncfusion:GridSummaryRow ShowSummaryInRow="True" Title="Total Price: {price}">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="price"
                                              Format="'{Sum:c}'"
                                              MappingName="TotalPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridSummaryRow>
    </syncfusion:SfDataGrid.GroupSummaryRows>
</syncfusion:SfDataGrid>
```

### Table Summary Row Style

```xaml
<Application.Resources>
    <Style TargetType="syncfusion:TableSummaryRowControl" x:Key="tableSummaryRowStyle">
        <Setter Property="Background" Value="Bisque"/>
    </Style>
</Application.Resources>

<Window.Resources>
    <local:SelectorClass x:Key="styleSelector"/>    
</Window.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid"             
                       ShowRowHeader="True"   
                       ItemsSource="{Binding Orders}" 
                       TableSummaryRowStyleSelector="{StaticResource styleSelector}">
    <syncfusion:SfDataGrid.TableSummaryRows>
        <syncfusion:GridTableSummaryRow Position="Top" ShowSummaryInRow="True" Title="Total: {price}">
            <syncfusion:GridTableSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="price"
                                              Format="'{Sum:c}'"
                                              MappingName="TotalPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridTableSummaryRow.SummaryColumns>
        </syncfusion:GridTableSummaryRow>
    </syncfusion:SfDataGrid.TableSummaryRows>
</syncfusion:SfDataGrid>
```

## Visual States

Define visual states for cells and rows:

```xaml
<Style TargetType="syncfusion:GridCell">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="syncfusion:GridCell">
                <Border Background="{TemplateBinding Background}">
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="IndicationStates">
                            <VisualState x:Name="Normal"/>
                            <VisualState x:Name="Current">
                                <Storyboard>
                                    <ColorAnimation Storyboard.TargetName="PART_GridCellBorder"
                                                   Storyboard.TargetProperty="(Border.BorderBrush).(SolidColorBrush.Color)"
                                                   Duration="0" To="Red"/>
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="Selected">
                                <Storyboard>
                                    <ColorAnimation Storyboard.TargetName="PART_GridCellBorder"
                                                   Storyboard.TargetProperty="(Border.Background).(SolidColorBrush.Color)"
                                                   Duration="0" To="LightBlue"/>
                                </Storyboard>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                    <ContentPresenter />
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

## SfSkinManager Themes

Apply built-in themes using SfSkinManager:

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    <syncfusion:SfDataGrid x:Name="dataGrid" ItemsSource="{Binding Orders}" />
</Window>
```

```csharp
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

### Available Themes

- MaterialLight
- MaterialDark
- MaterialLightBlue
- MaterialDarkBlue
- Office2019Colorful
- Office2019Black
- Office2019White
- Office2019HighContrast
- Office2019HighContrastWhite
- FluentLight
- FluentDark
- Windows11Light
- Windows11Dark

## Troubleshooting

### Issue: Styling not applied
**Solution**: Check TargetType matches element. Verify binding path is correct. Ensure style is in correct scope (Window.Resources or Application.Resources).

### Issue: Performance degradation with StyleSelector
**Solution**: Use Converter instead of StyleSelector when possible. Cache styles in StyleSelector. Minimize complex calculations in SelectStyle method.

### Issue: Data Triggers not working
**Solution**: Verify binding path matches property name. Check that property implements INotifyPropertyChanged. Ensure trigger value type matches property type.

### Issue: Alternate row style not visible
**Solution**: Check that AlternationCount is set. Verify AlternatingRowStyleSelector is assigned. Ensure row styles have different backgrounds.

### Issue: Summary cell style not applied
**Solution**: Verify summary is defined correctly. Check StyleSelector container type. Ensure summary values are calculated.

### Issue: Theme not applied
**Solution**: Install Syncfusion.SfSkinManager.WPF package. Add namespace reference. Apply theme to Window or DataGrid.

### Issue: Cell style overrides row style
**Solution**: Cell styles have higher precedence. Use lower priority setters or apply style to row only.
