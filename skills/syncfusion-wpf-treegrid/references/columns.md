# Columns Management for WPF TreeGrid

## Table of Contents
- [Overview](#overview)
- [Column Generation](#column-generation)
  - [Auto Column Generation](#auto-column-generation)
  - [Manual Column Definition](#manual-column-definition)
- [Column Manipulation](#column-manipulation)
  - [Adding Columns](#adding-columns)
  - [Removing Columns](#removing-columns)
  - [Accessing Columns](#accessing-columns)
- [Column Resizing](#column-resizing)
  - [Resize Modes](#resize-modes)
  - [Programmatic Resizing](#programmatic-resizing)
  - [Disable Resizing](#disable-resizing)
- [Column Reordering](#column-reordering)
  - [Enable Drag-Drop](#enable-drag-drop)
  - [Programmatic Reordering](#programmatic-reordering)
- [Column Freezing](#column-freezing)
  - [Freeze Columns](#freeze-columns)
  - [Freeze Panes](#freeze-panes)
- [Stacked Headers](#stacked-headers)
  - [Defining Stacked Headers](#defining-stacked-headers)
  - [Multi-Level Stacking](#multi-level-stacking)
- [Column Visibility](#column-visibility)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid provides comprehensive column management capabilities including automatic generation, manual definition, resizing, reordering, freezing, and stacked headers.

## Column Generation

### Auto Column Generation

Enable automatic column generation from data source properties:

```xaml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding EmployeeDetails}"
                       AutoGenerateColumns="True"
                       ChildPropertyName="Children"
                       AutoExpandMode="RootNodesExpanded" />
```

**Control column generation with AutoGenerateColumnsMode:**

```csharp
// Generate only new columns
treeGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None; // Default
treeGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.Reset; // Reset and regenerate
treeGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.ResetAll; // Reset including manually added
treeGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.SmartReset; // Keep manual columns
```

**Customize auto-generation with AutoGeneratingColumn event:**

```csharp
this.treeGrid.AutoGeneratingColumn += TreeGrid_AutoGeneratingColumn;

private void TreeGrid_AutoGeneratingColumn(object sender, TreeGridAutoGeneratingColumnEventArgs e)
{
    if (e.Column.MappingName == "Salary")
    {
        // Change to currency column
        e.Column = new TreeGridCurrencyColumn
        {
            MappingName = "Salary",
            HeaderText = "Annual Salary",
            CurrencySymbol = "$",
            CurrencyDecimalDigits = 2
        };
    }
    
    if (e.Column.MappingName == "Password")
    {
        // Cancel generation for sensitive data
        e.Cancel = true;
    }
}
```

### Manual Column Definition

Define columns explicitly in XAML:

```xaml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding EmployeeDetails}"
                       AutoGenerateColumns="False"
                       ChildPropertyName="Children">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="First Name" 
                                      MappingName="FirstName" 
                                      Width="150"/>
        
        <syncfusion:TreeGridTextColumn HeaderText="Last Name" 
                                      MappingName="LastName" 
                                      Width="150"/>
        
        <syncfusion:TreeGridNumericColumn HeaderText="Employee ID" 
                                         MappingName="EmployeeID"
                                         Width="120"
                                         NumberDecimalDigits="0"/>
        
        <syncfusion:TreeGridCurrencyColumn HeaderText="Salary" 
                                          MappingName="Salary"
                                          Width="150"
                                          CurrencySymbol="$"/>
        
        <syncfusion:TreeGridDateTimeColumn HeaderText="DOB" 
                                          MappingName="DOB"
                                          Width="150"
                                          Pattern="ShortDate"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

**Define columns in code:**

```csharp
// Clear auto-generated columns
treeGrid.Columns.Clear();

// Add columns programmatically
treeGrid.Columns.Add(new TreeGridTextColumn
{
    MappingName = "FirstName",
    HeaderText = "First Name",
    Width = 150
});

treeGrid.Columns.Add(new TreeGridTextColumn
{
    MappingName = "LastName",
    HeaderText = "Last Name",
    Width = 150
});

treeGrid.Columns.Add(new TreeGridNumericColumn
{
    MappingName = "EmployeeID",
    HeaderText = "Employee ID",
    Width = 120,
    NumberDecimalDigits = 0
});
```

## Column Manipulation

### Adding Columns

```csharp
// Add at end
treeGrid.Columns.Add(new TreeGridTextColumn
{
    MappingName = "Department",
    HeaderText = "Department"
});

// Insert at specific position
treeGrid.Columns.Insert(0, new TreeGridTextColumn
{
    MappingName = "EmployeeID",
    HeaderText = "ID"
});

// Add multiple columns
var columns = new List<TreeGridColumn>
{
    new TreeGridTextColumn { MappingName = "FirstName", HeaderText = "First Name" },
    new TreeGridTextColumn { MappingName = "LastName", HeaderText = "Last Name" }
};

foreach (var column in columns)
{
    treeGrid.Columns.Add(column);
}
```

### Removing Columns

```csharp
// Remove by index
treeGrid.Columns.RemoveAt(0);

// Remove by column object
var column = treeGrid.Columns[0];
treeGrid.Columns.Remove(column);

// Remove by mapping name
var columnToRemove = treeGrid.Columns.FirstOrDefault(c => c.MappingName == "Department");
if (columnToRemove != null)
{
    treeGrid.Columns.Remove(columnToRemove);
}

// Clear all columns
treeGrid.Columns.Clear();
```

### Accessing Columns

```csharp
// By index
var firstColumn = treeGrid.Columns[0];

// By mapping name
var nameColumn = treeGrid.Columns["FirstName"];

// Iterate through columns
foreach (var column in treeGrid.Columns)
{
    Debug.WriteLine($"Column: {column.HeaderText}, Mapping: {column.MappingName}");
}

// Get column count
int columnCount = treeGrid.Columns.Count;

// Check if column exists
bool hasNameColumn = treeGrid.Columns.Any(c => c.MappingName == "FirstName");
```

## Column Resizing

### Resize Modes

Set how columns are sized:

```xaml
<!-- Individual column sizing -->
<syncfusion:TreeGridTextColumn MappingName="FirstName" 
                              Width="150"
                              MinimumWidth="100"
                              MaximumWidth="300"/>

<!-- Auto-size all columns -->
<syncfusion:SfTreeGrid ColumnSizer="Star">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="FirstName"/>
        <syncfusion:TreeGridTextColumn MappingName="LastName"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

**ColumnSizer options:**

```csharp
// Star sizing - distribute available width
treeGrid.ColumnSizer = GridLengthUnitType.Star;

// Auto sizing based on content
treeGrid.ColumnSizer = GridLengthUnitType.Auto;

// No auto-sizing
treeGrid.ColumnSizer = GridLengthUnitType.None;

// SizeToCells - fit to cell content
treeGrid.ColumnSizer = GridLengthUnitType.SizeToCells;

// SizeToHeader - fit to header text
treeGrid.ColumnSizer = GridLengthUnitType.SizeToHeader;
```

### Programmatic Resizing

```csharp
// Resize specific column
treeGrid.Columns["FirstName"].Width = 200;

// Auto-fit column width
treeGrid.Columns["FirstName"].Width = double.NaN; // Auto

// Resize all columns
foreach (var column in treeGrid.Columns)
{
    column.Width = 150;
}

// Auto-fit all columns to content
treeGrid.TreeGridColumnSizer.Refresh();

// Auto-fit specific column
treeGrid.TreeGridColumnSizer.Refresh("FirstName");
```

### Disable Resizing

```xaml
<!-- Disable resizing for specific column -->
<syncfusion:TreeGridTextColumn MappingName="FirstName" 
                              AllowResizing="False"/>

<!-- Disable resizing for all columns -->
<syncfusion:SfTreeGrid AllowResizingColumns="False"/>
```

## Column Reordering

### Enable Drag-Drop

```xaml
<syncfusion:SfTreeGrid AllowDraggingColumns="True"
                       ItemsSource="{Binding EmployeeDetails}">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="FirstName" 
                                      AllowDragging="True"/>
        <syncfusion:TreeGridTextColumn MappingName="LastName" 
                                      AllowDragging="True"/>
        <syncfusion:TreeGridTextColumn MappingName="EmployeeID" 
                                      AllowDragging="False"/> <!-- Cannot be reordered -->
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

### Programmatic Reordering

```csharp
// Move column to new position
var column = treeGrid.Columns["FirstName"];
treeGrid.Columns.Move(column, 2);

// Swap columns
var oldIndex = treeGrid.Columns.IndexOf(treeGrid.Columns["FirstName"]);
var newIndex = treeGrid.Columns.IndexOf(treeGrid.Columns["LastName"]);
treeGrid.Columns.Move(oldIndex, newIndex);

// Reorder using events
treeGrid.ColumnDragging += TreeGrid_ColumnDragging;

private void TreeGrid_ColumnDragging(object sender, TreeGridColumnDraggingEventArgs e)
{
    if (e.Reason == QueryColumnDraggingReason.DragStarted)
    {
        // Prevent dragging specific columns
        if (e.From == "EmployeeID")
        {
            e.Cancel = true;
        }
    }
}
```

## Column Freezing

### Freeze Columns

```csharp
// Freeze first N columns
treeGrid.FrozenColumnCount = 2; // Freeze first 2 columns

// Freeze from end
treeGrid.FooterColumnCount = 1; // Freeze last column

// XAML
```

```xaml
<syncfusion:SfTreeGrid FrozenColumnCount="2"
                       FooterColumnCount="1"/>
```

### Freeze Panes

Freeze both rows and columns:

```csharp
// Freeze rows and columns
treeGrid.FrozenColumnCount = 2;
treeGrid.FrozenRowsCount = 1;

// Get frozen column count
int frozenCount = treeGrid.FrozenColumnCount;

// Unfreeze
treeGrid.FrozenColumnCount = 0;
```

## Stacked Headers

### Defining Stacked Headers

Create multi-level column headers:

```xaml
<syncfusion:SfTreeGrid ItemsSource="{Binding EmployeeDetails}">
    <syncfusion:SfTreeGrid.StackedHeaderRows>
        <syncfusion:StackedHeaderRow>
            <syncfusion:StackedHeaderRow.StackedColumns>
                <syncfusion:StackedColumn 
                    ChildColumns="FirstName,LastName"
                    HeaderText="Employee Name"
                    MappingName="EmployeeName"/>
                
                <syncfusion:StackedColumn 
                    ChildColumns="EmployeeID,DOB"
                    HeaderText="Employee Info"
                    MappingName="EmployeeInfo"/>
            </syncfusion:StackedHeaderRow.StackedColumns>
        </syncfusion:StackedHeaderRow>
    </syncfusion:SfTreeGrid.StackedHeaderRows>
    
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="FirstName" HeaderText="First Name"/>
        <syncfusion:TreeGridTextColumn MappingName="LastName" HeaderText="Last Name"/>
        <syncfusion:TreeGridNumericColumn MappingName="EmployeeID" HeaderText="ID"/>
        <syncfusion:TreeGridDateTimeColumn MappingName="DOB" HeaderText="Date of Birth"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

### Multi-Level Stacking

```xaml
<syncfusion:SfTreeGrid.StackedHeaderRows>
    <!-- Level 1 -->
    <syncfusion:StackedHeaderRow>
        <syncfusion:StackedHeaderRow.StackedColumns>
            <syncfusion:StackedColumn 
                ChildColumns="EmployeeName,EmployeeInfo"
                HeaderText="Employee Details"
                MappingName="EmployeeDetails"/>
        </syncfusion:StackedHeaderRow.StackedColumns>
    </syncfusion:StackedHeaderRow>
    
    <!-- Level 2 -->
    <syncfusion:StackedHeaderRow>
        <syncfusion:StackedHeaderRow.StackedColumns>
            <syncfusion:StackedColumn 
                ChildColumns="FirstName,LastName"
                HeaderText="Name"
                MappingName="EmployeeName"/>
            
            <syncfusion:StackedColumn 
                ChildColumns="EmployeeID,DOB"
                HeaderText="Info"
                MappingName="EmployeeInfo"/>
        </syncfusion:StackedHeaderRow.StackedColumns>
    </syncfusion:StackedHeaderRow>
</syncfusion:SfTreeGrid.StackedHeaderRows>
```

**Programmatic stacked headers:**

```csharp
var stackedHeaderRow = new StackedHeaderRow();

stackedHeaderRow.StackedColumns.Add(new StackedColumn
{
    ChildColumns = "FirstName,LastName",
    HeaderText = "Employee Name",
    MappingName = "EmployeeName"
});

stackedHeaderRow.StackedColumns.Add(new StackedColumn
{
    ChildColumns = "EmployeeID,DOB",
    HeaderText = "Employee Info",
    MappingName = "EmployeeInfo"
});

treeGrid.StackedHeaderRows.Add(stackedHeaderRow);
```

## Column Visibility

```csharp
// Hide column
treeGrid.Columns["FirstName"].IsHidden = true;

// Show column
treeGrid.Columns["FirstName"].IsHidden = false;

// Toggle visibility
treeGrid.Columns["FirstName"].IsHidden = !treeGrid.Columns["FirstName"].IsHidden;

// Hide multiple columns
var columnsToHide = new[] { "FirstName", "LastName" };
foreach (var columnName in columnsToHide)
{
    treeGrid.Columns[columnName].IsHidden = true;
}

// Get visible columns
var visibleColumns = treeGrid.Columns.Where(c => !c.IsHidden);
```

## Best Practices

### 1. Use Appropriate Column Types

```csharp
// Use specific column types for better performance
treeGrid.Columns.Add(new TreeGridNumericColumn
{
    MappingName = "EmployeeID",
    NumberDecimalDigits = 0  // Integer values
});

treeGrid.Columns.Add(new TreeGridCurrencyColumn
{
    MappingName = "Salary",
    CurrencySymbol = "$",
    CurrencyDecimalDigits = 2
});
```

### 2. Optimize Column Width

```csharp
// Set explicit widths for better performance
treeGrid.ColumnSizer = GridLengthUnitType.None;

foreach (var column in treeGrid.Columns)
{
    column.Width = 150; // Fixed width
}

// Use Star sizing for responsive layouts
treeGrid.ColumnSizer = GridLengthUnitType.Star;
```

### 3. Handle Column Events

```csharp
treeGrid.ColumnDragging += (s, e) =>
{
    if (e.Reason == QueryColumnDraggingReason.DragEnded)
    {
        // Save new column order
        SaveColumnLayout();
    }
};

treeGrid.Resizing += (s, e) =>
{
    if (e.Reason == QueryResizingReason.ResizeEnded)
    {
        // Save new column widths
        SaveColumnWidths();
    }
};
```

### 4. Persist Column Configuration

```csharp
public void SaveColumnLayout()
{
    var layout = new List<ColumnConfig>();
    
    foreach (var column in treeGrid.Columns)
    {
        layout.Add(new ColumnConfig
        {
            MappingName = column.MappingName,
            Width = column.Width,
            IsHidden = column.IsHidden,
            DisplayIndex = treeGrid.Columns.IndexOf(column)
        });
    }
    
    // Serialize to file or settings
    var json = JsonConvert.SerializeObject(layout);
    File.WriteAllText("column-layout.json", json);
}

public void LoadColumnLayout()
{
    if (!File.Exists("column-layout.json")) return;
    
    var json = File.ReadAllText("column-layout.json");
    var layout = JsonConvert.DeserializeObject<List<ColumnConfig>>(json);
    
    foreach (var config in layout.OrderBy(c => c.DisplayIndex))
    {
        var column = treeGrid.Columns[config.MappingName];
        if (column != null)
        {
            column.Width = config.Width;
            column.IsHidden = config.IsHidden;
            
            var currentIndex = treeGrid.Columns.IndexOf(column);
            if (currentIndex != config.DisplayIndex)
            {
                treeGrid.Columns.Move(currentIndex, config.DisplayIndex);
            }
        }
    }
}

public class ColumnConfig
{
    public string MappingName { get; set; }
    public double Width { get; set; }
    public bool IsHidden { get; set; }
    public int DisplayIndex { get; set; }
}
```

## Troubleshooting

### Issue: Columns Not Appearing

**Problem**: Columns defined but not visible

**Solutions**:
1. Check `AutoGenerateColumns` is set correctly
2. Verify `MappingName` matches property names
3. Ensure ItemsSource is set

```csharp
// Debug column setup
Debug.WriteLine($"AutoGenerateColumns: {treeGrid.AutoGenerateColumns}");
Debug.WriteLine($"Column Count: {treeGrid.Columns.Count}");
Debug.WriteLine($"ItemsSource: {treeGrid.ItemsSource != null}");

foreach (var column in treeGrid.Columns)
{
    Debug.WriteLine($"Column: {column.MappingName}, Hidden: {column.IsHidden}");
}
```

### Issue: Column Resizing Not Working

**Problem**: Cannot resize columns

**Solutions**:
1. Check `AllowResizingColumns` property
2. Verify individual column `AllowResizing`
3. Check if column has fixed width

```csharp
// Enable resizing
treeGrid.AllowResizingColumns = true;
treeGrid.Columns["FirstName"].AllowResizing = true;

// Remove fixed width constraint
treeGrid.Columns["FirstName"].Width = double.NaN; // Auto
```

### Issue: Frozen Columns Not Freezing

**Problem**: Columns not staying frozen during scroll

**Solutions**:
1. Verify `FrozenColumnCount` is set
2. Check if columns are in correct position
3. Ensure scrolling is enabled

```csharp
// Correct frozen column setup
treeGrid.FrozenColumnCount = 2;

// Verify setup
Debug.WriteLine($"Frozen Columns: {treeGrid.FrozenColumnCount}");
Debug.WriteLine($"Total Columns: {treeGrid.Columns.Count}");
```

### Issue: Stacked Headers Misaligned

**Problem**: Stacked headers don't align with columns

**Solutions**:
1. Verify `ChildColumns` property matches `MappingName`
2. Check column order
3. Ensure all referenced columns exist

```csharp
// Validate stacked header configuration
var stackedColumn = new StackedColumn
{
    ChildColumns = "FirstName,LastName",
    HeaderText = "Name"
};

// Verify columns exist
var childColumns = stackedColumn.ChildColumns.Split(',');
foreach (var childColumn in childColumns)
{
    var column = treeGrid.Columns[childColumn.Trim()];
    if (column == null)
    {
        Debug.WriteLine($"Warning: Column '{childColumn}' not found");
    }
}
```

### Issue: Column Width Not Persisting

**Problem**: Column widths reset on grid reload

**Solutions**:
1. Save width on resize event
2. Restore widths after data binding
3. Delay width setting until after load

```csharp
private void TreeGrid_Loaded(object sender, RoutedEventArgs e)
{
    LoadColumnLayout(); // Restore saved layout
}

private void TreeGrid_Resizing(object sender, ResizingEventArgs e)
{
    if (e.Reason == QueryResizingReason.ResizeEnded)
    {
        SaveColumnLayout(); // Save immediately
    }
}
```
