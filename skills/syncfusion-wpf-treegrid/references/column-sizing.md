# Column Sizing for WPF TreeGrid

## Overview

WPF TreeGrid provides flexible column sizing options through the `ColumnSizer` property and individual column width settings.

## ColumnSizer Modes

### Star Sizing
Distributes available width proportionally:

```xaml
<syncfusion:SfTreeGrid ColumnSizer="Star">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn MappingName="FirstName" Width="2*"/>
        <syncfusion:TreeGridTextColumn MappingName="LastName" Width="*"/>
        <syncfusion:TreeGridTextColumn MappingName="Email" Width="3*"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

### Auto Sizing
Fits columns to content:

```csharp
// Auto-size all columns
treeGrid.ColumnSizer = GridLengthUnitType.Auto;

// Auto-size specific column
treeGrid.Columns["FirstName"].Width = double.NaN;
```

### SizeToCells
Sizes based on cell content:

```csharp
treeGrid.ColumnSizer = GridLengthUnitType.SizeToCells;
```

### SizeToHeader
Sizes based on header text:

```csharp
treeGrid.ColumnSizer = GridLengthUnitType.SizeToHeader;
```

### FillColumn
Fills remaining space:

```csharp
treeGrid.ColumnSizer = GridLengthUnitType.FillColumn;
```

## TreeGridColumnSizer

### Refresh Column Sizes

```csharp
// Refresh all columns
treeGrid.TreeGridColumnSizer.Refresh();

// Refresh specific column
treeGrid.TreeGridColumnSizer.Refresh("FirstName");

// Refresh with options
treeGrid.TreeGridColumnSizer.Refresh(true); // Reset cached sizes
```

### Custom Column Sizer

```csharp
public class CustomColumnSizer : TreeGridColumnSizer
{
    public CustomColumnSizer(SfTreeGrid treeGrid) : base(treeGrid)
    {
    }

    protected override double CalculateColumnWidth(TreeGridColumn column)
    {
        if (column.MappingName == "FirstName")
            return 200; // Custom width

        return base.CalculateColumnWidth(column);
    }
}

// Apply custom sizer
treeGrid.TreeGridColumnSizer = new CustomColumnSizer(treeGrid);
```

## Individual Column Width

```xaml
<syncfusion:TreeGridTextColumn MappingName="FirstName"
                              Width="150"
                              MinimumWidth="100"
                              MaximumWidth="300"/>
```

```csharp
// Set fixed width
treeGrid.Columns["FirstName"].Width = 150;

// Set minimum/maximum
treeGrid.Columns["FirstName"].MinimumWidth = 100;
treeGrid.Columns["FirstName"].MaximumWidth = 300;

// Auto width
treeGrid.Columns["FirstName"].Width = double.NaN;
```

## Best Practices

### 1. Use Appropriate Sizing Mode

```csharp
// For responsive layouts
treeGrid.ColumnSizer = GridLengthUnitType.Star;

// For content-based sizing
treeGrid.ColumnSizer = GridLengthUnitType.Auto;

// For manual control
treeGrid.ColumnSizer = GridLengthUnitType.None;
```

### 2. Set Min/Max Widths

```csharp
foreach (var column in treeGrid.Columns)
{
    column.MinimumWidth = 50;
    column.MaximumWidth = 400;
}
```

### 3. Performance Optimization

```csharp
// Suspend layout updates during bulk changes
treeGrid.BeginInit();

foreach (var column in treeGrid.Columns)
{
    column.Width = 150;
}

treeGrid.EndInit(); // Triggers single layout pass
```

## Troubleshooting

### Issue: Columns Not Resizing

```csharp
// Verify AllowResizingColumns
treeGrid.AllowResizingColumns = true;

// Check individual column
treeGrid.Columns["FirstName"].AllowResizing = true;

// Refresh sizes
treeGrid.TreeGridColumnSizer.Refresh();
```

### Issue: Auto-Sizing Performance

```csharp
// Limit auto-sizing to visible rows
treeGrid.ColumnSizer = GridLengthUnitType.SizeToCells;

// Or use fixed widths for large datasets
treeGrid.ColumnSizer = GridLengthUnitType.None;
foreach (var column in treeGrid.Columns)
{
    column.Width = 150;
}
```
