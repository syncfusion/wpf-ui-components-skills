# XAML Configuration

## Table of Contents
- [XAML Configuration Overview](#xaml-configuration-overview)
- [Declarative Grid Setup](#declarative-grid-setup)
- [Property Configuration](#property-configuration)
- [XAML Namespaces](#xaml-namespaces)
- [Styles and Templates](#styles-and-templates)
- [Best Practices](#best-practices)

## XAML Configuration Overview

While the OLAP Grid often requires code-behind for data binding (OlapDataManager, OlapReport), many visual and behavioral properties can be configured declaratively in XAML.

**Benefits of XAML Configuration:**
- Visual designer support
- Separation of UI and logic
- Easier maintenance
- Resource reusability
- Binding support

## Declarative Grid Setup

### Basic XAML Structure

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="MyApp.MainWindow">
    
    <Grid>
        <syncfusion:OlapGrid Name="olapGrid1" 
                            Layout="ExcelLikeLayout"
                            ShowToolTip="True"
                            AllowSelection="True" />
    </Grid>
    
</Window>
```

## Property Configuration

### Layout Properties

```xaml
<syncfusion:OlapGrid Name="olapGrid1" 
                    Layout="ExcelLikeLayout"
                    ShowExpanders="True" />
```

**Layout Options:**
- `Normal`
- `ExcelLikeLayout`
- `ExcelLikeLayoutWithMemberProperties`
- `NormalTopSummary`
- `NoSummaries`

### Visual Properties

```xaml
<syncfusion:OlapGrid Name="olapGrid1"
                    Background="White"
                    Foreground="Black"
                    FontFamily="Segoe UI"
                    FontSize="12"
                    BorderBrush="Gray"
                    BorderThickness="1" />
```

### Behavioral Properties

```xaml
<syncfusion:OlapGrid Name="olapGrid1"
                    AllowSelection="True"
                    ShowToolTip="True"
                    IsEnabled="True" />
```

## XAML Namespaces

### Required Namespace

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Complete Window Declaration

```xaml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        mc:Ignorable="d"
        Title="OLAP Grid" Height="600" Width="900">
    
    <syncfusion:OlapGrid Name="olapGrid1" />
    
</Window>
```

## Conditional Formats in XAML

```xaml
<syncfusion:OlapGrid Name="olapGrid1">
    <syncfusion:OlapGrid.ConditionalFormats>
        
        <syncfusion:OlapGridDataConditionalFormat Name="HighSales">
            <syncfusion:OlapGridDataConditionalFormat.CellStyle>
                <syncfusion:OlapGridCellStyle 
                    Background="LightGreen" 
                    Foreground="DarkGreen"
                    FontWeight="Bold"
                    FontSize="12" />
            </syncfusion:OlapGridDataConditionalFormat.CellStyle>
            
            <syncfusion:OlapGridDataConditionalFormat.Conditions>
                <syncfusion:OlapGridDataCondition 
                    ConditionType="GreaterThan" 
                    Value="5000000" 
                    MeasureElement="Sales Amount" 
                    PredicateType="Or" />
            </syncfusion:OlapGridDataConditionalFormat.Conditions>
        </syncfusion:OlapGridDataConditionalFormat>
        
    </syncfusion:OlapGrid.ConditionalFormats>
</syncfusion:OlapGrid>
```

## Styles and Templates

### Grid Style Resource

```xaml
<Window.Resources>
    <Style x:Key="OlapGridStyle" TargetType="syncfusion:OlapGrid">
        <Setter Property="Background" Value="White"/>
        <Setter Property="FontFamily" Value="Segoe UI"/>
        <Setter Property="FontSize" Value="12"/>
        <Setter Property="BorderBrush" Value="LightGray"/>
        <Setter Property="BorderThickness" Value="1"/>
    </Style>
</Window.Resources>

<syncfusion:OlapGrid Name="olapGrid1" Style="{StaticResource OlapGridStyle}" />
```

### Cell Style Template

```xaml
<Window.Resources>
    <syncfusion:OlapGridCellStyle x:Key="HighlightStyle"
                                 Background="Yellow"
                                 Foreground="Black"
                                 FontWeight="Bold" />
</Window.Resources>
```

## Resource Management

### Shared Resources

```xaml
<Application.Resources>
    <!-- Shared across application -->
    <syncfusion:OlapGridCellStyle x:Key="HeaderStyle"
                                 Background="LightBlue"
                                 Foreground="DarkBlue"
                                 FontWeight="Bold"
                                 FontSize="14" />
    
    <syncfusion:OlapGridCellStyle x:Key="DataStyle"
                                 Background="White"
                                 Foreground="Black"
                                 FontSize="12" />
</Application.Resources>
```

## Complete XAML Example

```xaml
<Window x:Class="OlapGridDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="OLAP Grid Demo" Height="700" Width="1000">
    
    <Window.Resources>
        <Style x:Key="GridStyle" TargetType="syncfusion:OlapGrid">
            <Setter Property="FontFamily" Value="Segoe UI"/>
            <Setter Property="FontSize" Value="12"/>
            <Setter Property="Background" Value="White"/>
        </Style>
    </Window.Resources>
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        
        <!-- Toolbar -->
        <ToolBar Grid.Row="0">
            <Button Content="Refresh" Click="Refresh_Click"/>
            <Separator/>
            <ComboBox x:Name="LayoutComboBox" Width="150" 
                     SelectionChanged="Layout_Changed">
                <ComboBoxItem Content="Normal" IsSelected="True"/>
                <ComboBoxItem Content="Excel-Like"/>
                <ComboBoxItem Content="Top Summary"/>
            </ComboBox>
            <Separator/>
            <Button Content="Export to Excel" Click="ExportExcel_Click"/>
        </ToolBar>
        
        <!-- OLAP Grid -->
        <syncfusion:OlapGrid Grid.Row="1"
                            Name="olapGrid1"
                            Style="{StaticResource GridStyle}"
                            Layout="ExcelLikeLayout"
                            ShowToolTip="True">
            
            <syncfusion:OlapGrid.ConditionalFormats>
                <syncfusion:OlapGridDataConditionalFormat Name="HighValues">
                    <syncfusion:OlapGridDataConditionalFormat.CellStyle>
                        <syncfusion:OlapGridCellStyle Background="LightGreen" 
                                                     Foreground="DarkGreen"
                                                     FontWeight="Bold"/>
                    </syncfusion:OlapGridDataConditionalFormat.CellStyle>
                    <syncfusion:OlapGridDataConditionalFormat.Conditions>
                        <syncfusion:OlapGridDataCondition 
                            ConditionType="GreaterThan" 
                            Value="3000000" 
                            MeasureElement="Sales Amount" 
                            PredicateType="Or"/>
                    </syncfusion:OlapGridDataConditionalFormat.Conditions>
                </syncfusion:OlapGridDataConditionalFormat>
            </syncfusion:OlapGrid.ConditionalFormats>
            
        </syncfusion:OlapGrid>
        
        <!-- Status Bar -->
        <StatusBar Grid.Row="2">
            <StatusBarItem>
                <TextBlock x:Name="StatusText" Text="Ready"/>
            </StatusBarItem>
        </StatusBar>
        
    </Grid>
</Window>
```

## Best Practices

**Separation of Concerns:**
- Use XAML for visual properties and layout
- Use code-behind for data binding and business logic
- Keep XAML clean and readable

**Resource Organization:**
- Define common styles in Application.Resources
- Group related styles together
- Use meaningful resource keys

**Property Settings:**
- Set visual properties in XAML
- Configure data properties in code
- Use data binding where appropriate

**Maintainability:**
- Comment complex XAML sections
- Use consistent naming conventions
- Keep XAML files reasonably sized

**Performance:**
- Avoid excessive nesting in XAML
- Use StaticResource over DynamicResource when possible
- Minimize use of complex templates

## Limitations

Certain configurations must be done in code:

- **OlapDataManager** creation and setup
- **OlapReport** configuration
- **Connection string** specification
- **Data binding** (`DataBind()` method)
- **Dynamic conditional formats** based on runtime data
- **Complex event handling**

## Troubleshooting

**Grid not displaying:**
- Verify namespace is correct
- Check Name property is set
- Ensure Window/UserControl loads properly

**Properties not taking effect:**
- Check property spelling
- Verify property is settable in XAML
- Some properties require code-behind

**Styles not applying:**
- Confirm TargetType matches
- Check resource key spelling
- Verify resource is in scope

**Conditional formats not working:**
- Ensure MeasureElement name matches cube measure
- Verify condition values are strings
- Check PredicateType is appropriate
