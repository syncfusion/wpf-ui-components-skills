# Styles and Templates for WPF TreeGrid

## Table of Contents

- [Overview](#overview)
- [Header Styling](#header-styling)
- [Stacked Header Styling](#stacked-header-styling)
- [Row Styling](#row-styling)
- [Cell Styling](#cell-styling)
- [Cell Templates](#cell-templates)
- [Row Templates](#row-templates)
- [Theme Customization](#theme-customization)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid supports extensive customization through styles and templates for headers, cells, rows, and stacked headers.

## Header Styling

### Basic Header Style

```xaml
<syncfusion:SfTreeGrid HeaderStyle="{StaticResource CustomHeaderStyle}">
```

```xaml
<Style x:Key="CustomHeaderStyle" TargetType="syncfusion:TreeGridHeaderCell">
    <Setter Property="Background" Value="#FF2196F3"/>
    <Setter Property="Foreground" Value="White"/>
    <Setter Property="FontWeight" Value="Bold"/>
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="BorderThickness" Value="0,0,1,1"/>
    <Setter Property="BorderBrush" Value="White"/>
</Style>
```

### Column-Specific Header Style

```xaml
<syncfusion:TreeGridTextColumn MappingName="FirstName">
    <syncfusion:TreeGridTextColumn.HeaderStyle>
        <Style TargetType="syncfusion:TreeGridHeaderCell">
            <Setter Property="Background" Value="Navy"/>
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="HorizontalContentAlignment" Value="Center"/>
        </Style>
    </syncfusion:TreeGridTextColumn.HeaderStyle>
</syncfusion:TreeGridTextColumn>
```

### Header Template

```xaml
<syncfusion:TreeGridTextColumn MappingName="Salary">
    <syncfusion:TreeGridTextColumn.HeaderTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="/Images/dollar.png" Width="16" Height="16" Margin="0,0,5,0"/>
                <TextBlock Text="Salary" VerticalAlignment="Center" FontWeight="Bold"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:TreeGridTextColumn.HeaderTemplate>
</syncfusion:TreeGridTextColumn>
```

## Stacked Header Styling

### Default Stacked Header Style

```xaml
<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.StackedHeaderRows>
        <syncfusion:StackedHeaderRow>
            <syncfusion:StackedHeaderRow.StackedColumns>
                <syncfusion:StackedColumn ChildColumns="FirstName,LastName" HeaderText="Name">
                    <syncfusion:StackedColumn.HeaderStyle>
                        <Style TargetType="syncfusion:TreeGridStackedHeaderCell">
                            <Setter Property="Background" Value="#FF4CAF50"/>
                            <Setter Property="Foreground" Value="White"/>
                            <Setter Property="FontWeight" Value="Bold"/>
                            <Setter Property="FontSize" Value="14"/>
                        </Style>
                    </syncfusion:StackedColumn.HeaderStyle>
                </syncfusion:StackedColumn>
            </syncfusion:StackedHeaderRow.StackedColumns>
        </syncfusion:StackedHeaderRow>
    </syncfusion:SfTreeGrid.StackedHeaderRows>
</syncfusion:SfTreeGrid>
```

### Different Styles for Stacked Headers

```xaml
<Window.Resources>
    <Style x:Key="StackedHeaderStyle1" TargetType="syncfusion:TreeGridStackedHeaderCell">
        <Setter Property="Background" Value="#FF2196F3"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="FontWeight" Value="Bold"/>
    </Style>
    
    <Style x:Key="StackedHeaderStyle2" TargetType="syncfusion:TreeGridStackedHeaderCell">
        <Setter Property="Background" Value="#FF4CAF50"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="FontWeight" Value="Bold"/>
    </Style>
</Window.Resources>

<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.StackedHeaderRows>
        <syncfusion:StackedHeaderRow>
            <syncfusion:StackedHeaderRow.StackedColumns>
                <syncfusion:StackedColumn ChildColumns="FirstName,LastName" 
                                         HeaderText="Name"
                                         HeaderStyle="{StaticResource StackedHeaderStyle1}"/>
                <syncfusion:StackedColumn ChildColumns="Department,Title" 
                                         HeaderText="Employment"
                                         HeaderStyle="{StaticResource StackedHeaderStyle2}"/>
            </syncfusion:StackedHeaderRow.StackedColumns>
        </syncfusion:StackedHeaderRow>
    </syncfusion:SfTreeGrid.StackedHeaderRows>
</syncfusion:SfTreeGrid>
```

## Row Styling

### Row Style

```xaml
<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.RowStyle>
        <Style TargetType="syncfusion:TreeGridRowControl">
            <Setter Property="Background" Value="White"/>
            <Setter Property="BorderBrush" Value="LightGray"/>
            <Setter Property="BorderThickness" Value="0,0,0,1"/>
        </Style>
    </syncfusion:SfTreeGrid.RowStyle>
</syncfusion:SfTreeGrid>
```

### Alternating Row Style

```xaml
<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.AlternatingRowStyle>
        <Style TargetType="syncfusion:TreeGridRowControl">
            <Setter Property="Background" Value="#FFF5F5F5"/>
        </Style>
    </syncfusion:SfTreeGrid.AlternatingRowStyle>
</syncfusion:SfTreeGrid>
```

## Cell Styling

### Cell Style

```xaml
<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.CellStyle>
        <Style TargetType="syncfusion:TreeGridCell">
            <Setter Property="BorderBrush" Value="LightGray"/>
            <Setter Property="BorderThickness" Value="0,0,1,0"/>
            <Setter Property="Padding" Value="5"/>
        </Style>
    </syncfusion:SfTreeGrid.CellStyle>
</syncfusion:SfTreeGrid>
```

### Column Cell Style

```xaml
<syncfusion:TreeGridNumericColumn MappingName="Salary">
    <syncfusion:TreeGridNumericColumn.CellStyle>
        <Style TargetType="syncfusion:TreeGridCell">
            <Setter Property="Background" Value="LightYellow"/>
            <Setter Property="FontWeight" Value="Bold"/>
            <Setter Property="HorizontalContentAlignment" Value="Right"/>
        </Style>
    </syncfusion:TreeGridNumericColumn.CellStyle>
</syncfusion:TreeGridNumericColumn>
```

## Cell Templates

### Template Column

```xaml
<syncfusion:TreeGridTemplateColumn MappingName="Employee">
    <syncfusion:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Ellipse Width="32" Height="32" Margin="5">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="{Binding Photo}"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel VerticalAlignment="Center">
                    <TextBlock Text="{Binding FirstName}" FontWeight="Bold"/>
                    <TextBlock Text="{Binding Title}" FontSize="10" Foreground="Gray"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.CellTemplate>
</syncfusion:TreeGridTemplateColumn>
```

### Edit Template

```xaml
<syncfusion:TreeGridTemplateColumn MappingName="Rating">
    <syncfusion:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Rating}" HorizontalAlignment="Center"/>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.CellTemplate>
    <syncfusion:TreeGridTemplateColumn.EditTemplate>
        <DataTemplate>
            <Slider Minimum="0" Maximum="5" Value="{Binding Rating}" 
                   TickFrequency="1" IsSnapToTickEnabled="True"/>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.EditTemplate>
</syncfusion:TreeGridTemplateColumn>
```

## Row Templates

### Custom Row Template

```xaml
<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.RowTemplate>
        <DataTemplate>
            <Border Background="White" BorderBrush="LightGray" BorderThickness="0,0,0,1" Padding="10">
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition Width="*"/>
                        <ColumnDefinition Width="Auto"/>
                    </Grid.ColumnDefinitions>
                    
                    <Image Grid.Column="0" Source="{Binding Photo}" Width="48" Height="48" Margin="0,0,10,0"/>
                    
                    <StackPanel Grid.Column="1" VerticalAlignment="Center">
                        <TextBlock Text="{Binding FirstName}" FontWeight="Bold" FontSize="16"/>
                        <TextBlock Text="{Binding Title}" Foreground="Gray"/>
                        <TextBlock Text="{Binding Department}" FontSize="12"/>
                    </StackPanel>
                    
                    <StackPanel Grid.Column="2" VerticalAlignment="Center" HorizontalAlignment="Right">
                        <TextBlock Text="{Binding Salary, StringFormat=C}" FontWeight="Bold" FontSize="16"/>
                        <TextBlock Text="Annual Salary" FontSize="10" Foreground="Gray"/>
                    </StackPanel>
                </Grid>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeGrid.RowTemplate>
</syncfusion:SfTreeGrid>
```

## Theme Customization

### Override Theme Colors

```xaml
<Window.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Syncfusion.Themes.MaterialLight.WPF;component/SfTreeGrid/SfTreeGrid.xaml"/>
        </ResourceDictionary.MergedDictionaries>
        
        <!-- Override specific colors -->
        <SolidColorBrush x:Key="TreeGridHeaderBackground" Color="#FF1976D2"/>
        <SolidColorBrush x:Key="TreeGridHeaderForeground" Color="White"/>
    </ResourceDictionary>
</Window.Resources>
```

### Custom Theme

```xaml
<syncfusion:SfTreeGrid VisualStyle="Custom">
    <syncfusion:SfTreeGrid.HeaderStyle>
        <Style TargetType="syncfusion:TreeGridHeaderCell">
            <!-- Custom header style -->
        </Style>
    </syncfusion:SfTreeGrid.HeaderStyle>
    <syncfusion:SfTreeGrid.CellStyle>
        <Style TargetType="syncfusion:TreeGridCell">
            <!-- Custom cell style -->
        </Style>
    </syncfusion:SfTreeGrid.CellStyle>
</syncfusion:SfTreeGrid>
```

## Grid Lines Styling

```xaml
<syncfusion:SfTreeGrid GridLinesVisibility="Both">
    <syncfusion:SfTreeGrid.GridLinesBrush>
        <SolidColorBrush Color="#FFE0E0E0"/>
    </syncfusion:SfTreeGrid.GridLinesBrush>
</syncfusion:SfTreeGrid>
```

## Selection Style

```xaml
<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.SelectionStyle>
        <Style TargetType="syncfusion:TreeGridCell">
            <Setter Property="Background" Value="#FF2196F3"/>
            <Setter Property="Foreground" Value="White"/>
        </Style>
    </syncfusion:SfTreeGrid.SelectionStyle>
</syncfusion:SfTreeGrid>
```

## Current Cell Style

```xaml
<syncfusion:SfTreeGrid>
    <syncfusion:SfTreeGrid.CurrentCellStyle>
        <Style TargetType="syncfusion:TreeGridCell">
            <Setter Property="BorderBrush" Value="DarkBlue"/>
            <Setter Property="BorderThickness" Value="2"/>
        </Style>
    </syncfusion:SfTreeGrid.CurrentCellStyle>
</syncfusion:SfTreeGrid>
```

## Best Practices

### 1. Use Resource Dictionaries

```xaml
<Window.Resources>
    <ResourceDictionary>
        <Style x:Key="HeaderStyle" TargetType="syncfusion:TreeGridHeaderCell">
            <!-- Shared header style -->
        </Style>
    </ResourceDictionary>
</Window.Resources>
```

### 2. Optimize Templates

```xaml
<!-- Keep templates simple for better performance -->
<DataTemplate>
    <TextBlock Text="{Binding Value}"/> <!-- Good -->
</DataTemplate>

<!-- Avoid complex nested layouts in large grids -->
<DataTemplate>
    <Grid>
        <Grid.RowDefinitions>...</Grid.RowDefinitions>
        <Border>
            <StackPanel>...</StackPanel>
        </Border>
    </Grid> <!-- Complex - may impact performance -->
</DataTemplate>
```

### 3. Reuse Styles

```csharp
// Define styles in App.xaml or shared resource dictionary
// Reference them across multiple grids
```

## Troubleshooting

### Issue: Styles Not Applying

```xaml
<!-- Verify TargetType matches -->
<Style TargetType="syncfusion:TreeGridCell"> <!-- Correct -->
<Style TargetType="TreeGridCell"> <!-- Incorrect -->

<!-- Ensure style is in scope -->
<Window.Resources>
    <Style x:Key="MyStyle" TargetType="syncfusion:TreeGridCell">
        <!-- Style here -->
    </Style>
</Window.Resources>
```

### Issue: Template Not Rendering

```csharp
// Verify data binding paths
<TextBlock Text="{Binding FirstName}"/> // Check property name

// Ensure DataContext is correct
// For CellTemplate, DataContext is the data item
// For HeaderTemplate, DataContext is the column
```

### Issue: Performance with Row Templates

```csharp
// Use CellTemplate instead of RowTemplate when possible
// RowTemplate disables UI virtualization
// Consider using CellStyleSelector for conditional styling instead
```
