# Columns Configuration in SfMultiColumnDropDownControl

## Table of Contents
- [Overview](#overview)
- [Auto-Generating Columns](#auto-generating-columns)
  - [AutoGenerateColumns Property](#autogeneratecolumns-property)
  - [AutoGenerateColumnMode](#autogeneratecolumnmode)
- [Manually Defining Columns](#manually-defining-columns)
  - [Column Types](#column-types)
  - [Column Properties](#column-properties)
- [AutoGeneratingColumn Event](#autogeneratingcolumn-event)
  - [Canceling Column Generation](#canceling-column-generation)
  - [Changing Column Type](#changing-column-type)
  - [Customizing Column Properties](#customizing-column-properties)
  - [Setting Column Templates](#setting-column-templates)
- [Column Sizing](#column-sizing)
  - [GridColumnSizer Property](#gridcolumnsizer-property)
  - [Individual Column Sizing](#individual-column-sizing)
- [Column Templates](#column-templates)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

SfMultiColumnDropDownControl provides flexible column configuration similar to SfDataGrid. You can either let the control automatically generate columns from your data source or manually define which columns to display and how they appear.

## Auto-Generating Columns

### AutoGenerateColumns Property

The `AutoGenerateColumns` property determines whether columns are automatically created based on the data source properties.

**Default:** `true`

```xml
<!-- Auto-generate columns (default behavior) -->
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AutoGenerateColumns="True"/>
```

When `true`, all public properties of the data object become columns.

### AutoGenerateColumnMode

Controls how columns are generated when `AutoGenerateColumns` is `true`.

| Mode | Description |
|------|-------------|
| **Reset** | Creates columns for all fields and retains explicitly defined columns |
| **RetainOld** | Creates columns only if no explicit columns exist |
| **ResetAll** | Clears existing columns and creates new ones from data source |
| **None** | Keeps only explicitly defined columns |

**Example:**

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    AutoGenerateColumns="True"
    AutoGenerateColumnMode="Reset"
    DisplayMember="CustomerName">
    
    <!-- These columns are retained even with auto-generation -->
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" HeaderText="ID" Width="80"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

## Manually Defining Columns

For precise control over columns, set `AutoGenerateColumns` to `false` and define columns explicitly.

### Basic Manual Column Definition

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    Width="400"
    Height="30"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    ValueMember="OrderID"
    AutoGenerateColumns="False">
    
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" HeaderText="Order ID" Width="100"/>
        <syncfusion:GridTextColumn MappingName="CustomerName" HeaderText="Customer" Width="150"/>
        <syncfusion:GridTextColumn MappingName="Country" Width="120"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

### Code-Behind Column Definition

```csharp
sfMultiColumn.AutoGenerateColumns = false;

sfMultiColumn.Columns.Add(new GridTextColumn() 
{ 
    MappingName = "OrderID", 
    HeaderText = "Order ID",
    Width = 100
});

sfMultiColumn.Columns.Add(new GridTextColumn() 
{ 
    MappingName = "CustomerName", 
    HeaderText = "Customer",
    Width = 150
});

sfMultiColumn.Columns.Add(new GridCurrencyColumn() 
{ 
    MappingName = "Amount",
    HeaderText = "Total",
    Width = 120
});
```

### Column Types

SfMultiColumnDropDownControl supports various column types:

| Column Type | Use Case | Example |
|-------------|----------|---------|
| **GridTextColumn** | Text data | Names, descriptions, IDs |
| **GridNumericColumn** | Numeric data | Quantities, counts |
| **GridCurrencyColumn** | Monetary values | Prices, totals, amounts |
| **GridDateTimeColumn** | Date/time data | Order dates, deadlines |
| **GridCheckBoxColumn** | Boolean data | Status flags, checkboxes |
| **GridHyperlinkColumn** | Clickable links | URLs, navigation |
| **GridComboBoxColumn** | Dropdown selection | Categories, statuses |
| **GridTemplateColumn** | Custom content | Complex layouts |

**Example with Multiple Types:**

```xml
<syncfusion:SfMultiColumnDropDownControl.Columns>
    <syncfusion:GridTextColumn MappingName="OrderID"/>
    <syncfusion:GridTextColumn MappingName="CustomerName"/>
    <syncfusion:GridCurrencyColumn MappingName="Amount" CurrencySymbol="$"/>
    <syncfusion:GridDateTimeColumn MappingName="OrderDate" Format="MM/dd/yyyy"/>
    <syncfusion:GridCheckBoxColumn MappingName="IsProcessed"/>
</syncfusion:SfMultiColumnDropDownControl.Columns>
```

### Column Properties

Common properties available for all column types:

```xml
<syncfusion:GridTextColumn 
    MappingName="CustomerName"
    HeaderText="Customer Name"
    Width="150"
    MinimumWidth="100"
    MaximumWidth="300"
    AllowEditing="False"
    AllowSorting="True"
    AllowFiltering="True"
    AllowGrouping="False"
    AllowFocus="True"
    AllowResizing="True"
    AllowDragging="True"
    TextAlignment="Left"
    IsHidden="False"/>
```

## AutoGeneratingColumn Event

The `AutoGeneratingColumn` event fires when each column is auto-generated, allowing you to customize or cancel column creation.

### Event Handler Setup

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    AutoGenerateColumns="True"
    AutoGeneratingColumn="SfMultiColumn_AutoGeneratingColumn"/>
```

```csharp
private void SfMultiColumn_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    // Customize columns here
}
```

### Canceling Column Generation

Prevent specific columns from being generated:

```csharp
private void SfMultiColumn_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    // Don't show OrderID column
    if (e.Column.MappingName == "OrderID")
    {
        e.Cancel = true;
    }
    
    // Hide sensitive data
    if (e.Column.MappingName == "Password" || e.Column.MappingName == "SSN")
    {
        e.Cancel = true;
    }
}
```

### Changing Column Type

Replace auto-generated column type with a different one:

```csharp
private void SfMultiColumn_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    // Change UnitPrice from numeric to currency
    if (e.Column.MappingName == "UnitPrice")
    {
        if (e.Column is GridNumericColumn)
        {
            e.Column = new GridCurrencyColumn() 
            { 
                MappingName = "UnitPrice", 
                HeaderText = "Unit Price",
                CurrencySymbol = "$"
            };
        }
    }
    
    // Change date format
    if (e.Column.MappingName == "OrderDate")
    {
        if (e.Column is GridDateTimeColumn dateColumn)
        {
            dateColumn.Format = "MM/dd/yyyy";
            dateColumn.HeaderText = "Order Date";
        }
    }
}
```

### Customizing Column Properties

Modify properties of auto-generated columns:

```csharp
private void SfMultiColumn_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    // Customize OrderID column
    if (e.Column.MappingName == "OrderID")
    {
        e.Column.HeaderText = "Order #";
        e.Column.Width = 80;
        e.Column.AllowEditing = false;
        e.Column.AllowSorting = true;
        e.Column.AllowFiltering = true;
        e.Column.TextAlignment = TextAlignment.Center;
    }
    
    // Make all currency columns read-only
    if (e.Column is GridCurrencyColumn)
    {
        e.Column.AllowEditing = false;
    }
    
    // Set default width for text columns
    if (e.Column is GridTextColumn)
    {
        e.Column.Width = 150;
    }
}
```

### Setting Column Templates

Apply custom templates to auto-generated columns:

**XAML Resources:**

```xml
<Window.Resources>
    <DataTemplate x:Key="HeaderTemplate">
        <TextBlock Text="{Binding}" 
                   TextWrapping="Wrap" 
                   Foreground="DarkBlue"
                   FontWeight="Bold"/>
    </DataTemplate>
    
    <DataTemplate x:Key="CellTemplate">
        <Border Background="LightYellow" Padding="5">
            <TextBlock Text="{Binding CustomerName}" FontStyle="Italic"/>
        </Border>
    </DataTemplate>
</Window.Resources>
```

**Apply in Event:**

```csharp
private void SfMultiColumn_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    if (e.Column.MappingName == "CustomerName")
    {
        e.Column.HeaderTemplate = this.FindResource("HeaderTemplate") as DataTemplate;
    }
    
    if (e.Column.MappingName == "Country")
    {
        e.Column.CellTemplate = this.FindResource("CellTemplate") as DataTemplate;
    }
}
```

## Column Sizing

### GridColumnSizer Property

Controls how column widths are calculated across all columns.

| Value | Description |
|-------|-------------|
| **None** | Columns use their defined Width property |
| **Auto** | Columns auto-size based on content |
| **Star** | Columns share available space equally |
| **SizeToCells** | Width based on cell content only |
| **SizeToHeader** | Width based on header text only |

**Equal Width Distribution (Star):**

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    GridColumnSizer="Star"
    AutoGenerateColumns="False">
    
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID"/>
        <syncfusion:GridTextColumn MappingName="CustomerName"/>
        <syncfusion:GridTextColumn MappingName="Country"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

All three columns will have equal width, sharing the available space.

**Auto-Size Based on Content:**

```xml
<syncfusion:SfMultiColumnDropDownControl 
    GridColumnSizer="Auto"
    ItemsSource="{Binding Orders}"/>
```

### Individual Column Sizing

Override `GridColumnSizer` for specific columns:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    GridColumnSizer="None"
    ItemsSource="{Binding Orders}"
    AutoGenerateColumns="False">
    
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <!-- Fixed width -->
        <syncfusion:GridTextColumn MappingName="OrderID" Width="80"/>
        
        <!-- Auto-size this column -->
        <syncfusion:GridTextColumn MappingName="CustomerName" ColumnSizer="Auto"/>
        
        <!-- Star sizing (takes remaining space) -->
        <syncfusion:GridTextColumn MappingName="Description" Width="*"/>
        
        <!-- Fixed width -->
        <syncfusion:GridTextColumn MappingName="Country" Width="100"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

### Width Constraints

```xml
<syncfusion:GridTextColumn 
    MappingName="CustomerName"
    Width="150"
    MinimumWidth="100"
    MaximumWidth="300"/>
```

## Column Templates

### HeaderTemplate

Customize column header appearance:

```xml
<Window.Resources>
    <DataTemplate x:Key="CustomHeaderTemplate">
        <StackPanel Orientation="Horizontal">
            <Image Source="/Images/icon.png" Width="16" Height="16" Margin="0,0,5,0"/>
            <TextBlock Text="{Binding}" FontWeight="Bold" VerticalAlignment="Center"/>
        </StackPanel>
    </DataTemplate>
</Window.Resources>

<syncfusion:SfMultiColumnDropDownControl.Columns>
    <syncfusion:GridTextColumn 
        MappingName="CustomerName" 
        HeaderText="Customer"
        HeaderTemplate="{StaticResource CustomHeaderTemplate}"/>
</syncfusion:SfMultiColumnDropDownControl.Columns>
```

### CellTemplate

Customize cell content:

```xml
<Window.Resources>
    <DataTemplate x:Key="CustomCellTemplate">
        <Border Background="{Binding Converter={StaticResource StatusColorConverter}}" 
                Padding="5" CornerRadius="3">
            <TextBlock Text="{Binding Status}" Foreground="White" FontWeight="Bold"/>
        </Border>
    </DataTemplate>
</Window.Resources>

<syncfusion:GridTextColumn 
    MappingName="Status"
    CellTemplate="{StaticResource CustomCellTemplate}"/>
```

## Best Practices

### 1. Disable Auto-Generation for Controlled Layout

```xml
<!-- Recommended for production applications -->
<syncfusion:SfMultiColumnDropDownControl 
    AutoGenerateColumns="False"
    ItemsSource="{Binding Orders}">
    <!-- Define only needed columns -->
</syncfusion:SfMultiColumnDropDownControl>
```

### 2. Set Meaningful Header Text

```xml
<syncfusion:GridTextColumn 
    MappingName="CustomerID" 
    HeaderText="Customer ID"/>  <!-- Clear header -->
```

### 3. Use Appropriate Column Types

```xml
<!-- Use specific column types for better formatting -->
<syncfusion:GridCurrencyColumn MappingName="Amount"/>  <!-- Not GridTextColumn -->
<syncfusion:GridDateTimeColumn MappingName="OrderDate"/>
<syncfusion:GridCheckBoxColumn MappingName="IsActive"/>
```

### 4. Consider Column Order

```xml
<!-- Show important columns first -->
<syncfusion:SfMultiColumnDropDownControl.Columns>
    <syncfusion:GridTextColumn MappingName="OrderID"/>      <!-- Primary key first -->
    <syncfusion:GridTextColumn MappingName="CustomerName"/> <!-- Display member second -->
    <syncfusion:GridTextColumn MappingName="Status"/>       <!-- Important info -->
    <syncfusion:GridTextColumn MappingName="Notes"/>        <!-- Details last -->
</syncfusion:SfMultiColumnDropDownControl.Columns>
```

### 5. Set Reasonable Widths

```xml
<!-- Balance between readability and space -->
<syncfusion:GridTextColumn MappingName="ID" Width="60"/>
<syncfusion:GridTextColumn MappingName="Name" Width="150"/>
<syncfusion:GridTextColumn MappingName="Description" Width="*"/>  <!-- Takes remaining -->
```

## Troubleshooting

### Columns Not Appearing

**Problem:** Defined columns don't show in dropdown

**Solutions:**
```xml
<!-- Ensure AutoGenerateColumns is False -->
<syncfusion:SfMultiColumnDropDownControl 
    AutoGenerateColumns="False">  <!-- Must be False for manual columns -->
```

```csharp
// Verify MappingName matches property
public class OrderInfo
{
    public string CustomerName { get; set; }  // Property name
}

// Column MappingName must match exactly (case-sensitive)
<syncfusion:GridTextColumn MappingName="CustomerName"/>  // ✓
<syncfusion:GridTextColumn MappingName="customername"/>  // ✗
```

### Column Width Not Applied

**Problem:** Width settings are ignored

**Solution:**
```xml
<!-- Check GridColumnSizer isn't overriding -->
<syncfusion:SfMultiColumnDropDownControl 
    GridColumnSizer="None">  <!-- Allows individual widths -->
    
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="Name" Width="150"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

### AutoGeneratingColumn Event Not Firing

**Problem:** Event handler never called

**Solutions:**
1. Ensure `AutoGenerateColumns="True"`
2. Check event is wired up correctly
3. Verify ItemsSource has data

```xml
<syncfusion:SfMultiColumnDropDownControl 
    AutoGenerateColumns="True"
    AutoGeneratingColumn="SfMultiColumn_AutoGeneratingColumn"
    ItemsSource="{Binding Orders}"/>
```

### Template Not Displaying

**Problem:** Custom template not showing

**Solutions:**
```xml
<!-- Ensure template is in resources -->
<Window.Resources>
    <DataTemplate x:Key="MyTemplate">
        <!-- Template content -->
    </DataTemplate>
</Window.Resources>

<!-- Reference with StaticResource -->
<syncfusion:GridTextColumn 
    HeaderTemplate="{StaticResource MyTemplate}"/>  <!-- Not DynamicResource -->
```
