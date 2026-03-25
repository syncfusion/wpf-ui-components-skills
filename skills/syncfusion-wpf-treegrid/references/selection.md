# Selection for WPF TreeGrid

## Table of Contents

- [Overview](#overview)
- [Selection Units](#selection-units)
- [Selection Modes](#selection-modes)
- [Navigation Mode](#navigation-mode)
- [Programmatic Selection](#programmatic-selection)
- [Selection Events](#selection-events)
- [Keyboard Navigation](#keyboard-navigation)
- [Mouse Selection](#mouse-selection)
- [Scrolling](#scrolling)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid supports row, cell, and mixed selection with various modes and navigation behaviors.

## Selection Units

```csharp
// Select entire rows
treeGrid.SelectionUnit = TreeGridSelectionUnit.Row;

// Select individual cells
treeGrid.SelectionUnit = TreeGridSelectionUnit.Cell;

// Select both rows and cells
treeGrid.SelectionUnit = TreeGridSelectionUnit.Any;
```

## Selection Modes

```csharp
// No selection
treeGrid.SelectionMode = GridSelectionMode.None;

// Single selection
treeGrid.SelectionMode = GridSelectionMode.Single;

// Single selection with deselect on click
treeGrid.SelectionMode = GridSelectionMode.SingleDeselect;

// Multiple selection
treeGrid.SelectionMode = GridSelectionMode.Multiple;

// Extended selection (Ctrl+Click, Shift+Click)
treeGrid.SelectionMode = GridSelectionMode.Extended;
```

## Navigation Mode

```csharp
// Cell navigation
treeGrid.NavigationMode = NavigationMode.Cell;

// Row navigation
treeGrid.NavigationMode = NavigationMode.Row;
```

## Programmatic Selection

### Row Selection

```csharp
// Select by index
treeGrid.SelectedIndex = 5;

// Select single row
var node = treeGrid.View.Nodes[0];
treeGrid.SelectedItem = node.Item;

// Select multiple rows
treeGrid.SelectedItems.Add(treeGrid.View.Nodes[0].Item);
treeGrid.SelectedItems.Add(treeGrid.View.Nodes[1].Item);

// Select all rows
treeGrid.SelectAll();

// Clear selection
treeGrid.ClearSelection();
```

### Cell Selection

```csharp
// Select single cell
var rowColumnIndex = new RowColumnIndex(5, 2);
treeGrid.SelectCell(rowColumnIndex);

// Select multiple cells
var cellInfos = new List<TreeGridCellInfo>()
{
    new TreeGridCellInfo(5, "FirstName"),
    new TreeGridCellInfo(5, "LastName"),
    new TreeGridCellInfo(6, "FirstName")
};
treeGrid.SelectCells(cellInfos);

// Get selected cells
var selectedCells = treeGrid.SelectedCells;
```

## Selection Events

```csharp
// Before current cell changes
treeGrid.CurrentCellActivating += (s, e) =>
{
    Debug.WriteLine($"Activating cell: Row={e.RowColumnIndex.RowIndex}, Col={e.RowColumnIndex.ColumnIndex}");
    
    // Cancel navigation
    if (e.RowColumnIndex.RowIndex == 0)
        e.Cancel = true;
};

// After current cell changed
treeGrid.CurrentCellActivated += (s, e) =>
{
    var rowData = treeGrid.GetRecordAtRowIndex(e.RowColumnIndex.RowIndex);
    Debug.WriteLine($"Cell activated: {rowData}");
};

// Before selection changes
treeGrid.SelectionChanging += (s, e) =>
{
    // Cancel selection
    if (e.AddedItems.Count > 10)
    {
        e.Cancel = true;
        MessageBox.Show("Cannot select more than 10 items");
    }
};

// After selection changed
treeGrid.SelectionChanged += (s, e) =>
{
    Debug.WriteLine($"Selected items count: {treeGrid.SelectedItems.Count}");
    
    foreach (var item in e.AddedItems)
    {
        Debug.WriteLine($"Added: {item}");
    }
    
    foreach (var item in e.RemovedItems)
    {
        Debug.WriteLine($"Removed: {item}");
    }
};
```

## Keyboard Navigation

### Default Keyboard Shortcuts

```
Arrow Keys: Navigate cells/rows
Tab: Move to next cell
Shift+Tab: Move to previous cell
Ctrl+Home: First cell
Ctrl+End: Last cell
Page Up/Down: Scroll page
Ctrl+A: Select all
Escape: Clear selection (in SingleDeselect mode)
```

### Custom Keyboard Handling

```csharp
treeGrid.CurrentCellActivating += (s, e) =>
{
    // Custom keyboard logic
    if (Keyboard.IsKeyDown(Key.LeftCtrl))
    {
        // Handle Ctrl key behavior
    }
};
```

## Mouse Selection

```csharp
// Extended mode: Ctrl+Click for multi-select
treeGrid.SelectionMode = GridSelectionMode.Extended;

// Multiple mode: Click to toggle selection
treeGrid.SelectionMode = GridSelectionMode.Multiple;

// Shift+Click for range selection (Extended mode)
// Automatically handled by the grid
```

## Scrolling

```csharp
// Scroll to specific row
var rowIndex = 100;
treeGrid.ScrollInView(new RowColumnIndex(rowIndex, 0));

// Scroll to selected item
if (treeGrid.SelectedIndex >= 0)
{
    treeGrid.ScrollInView(new RowColumnIndex(treeGrid.SelectedIndex, 0));
}

// Auto-scroll settings
treeGrid.AutoScroller.AutoScrolling = AutoScrollOrientation.Both;
```

## Get Current Selection

```csharp
// Get selected item
var selectedItem = treeGrid.SelectedItem as Employee;

// Get all selected items
foreach (var item in treeGrid.SelectedItems)
{
    var employee = item as Employee;
    Debug.WriteLine(employee.FirstName);
}

// Get selected cells
foreach (var cellInfo in treeGrid.SelectedCells)
{
    Debug.WriteLine($"Row: {cellInfo.RowIndex}, Column: {cellInfo.Column.MappingName}");
}

// Get current cell
var currentCell = treeGrid.SelectionController.CurrentCellManager.CurrentCell;
```

## Select Rows by Condition

```csharp
public void SelectHighSalaryEmployees()
{
    treeGrid.ClearSelection();
    
    foreach (var node in treeGrid.View.Nodes)
    {
        var employee = node.Item as Employee;
        if (employee != null && employee.Salary > 80000)
        {
            treeGrid.SelectedItems.Add(employee);
        }
    }
}
```

## Best Practices

### 1. Choose Appropriate Selection Mode

```csharp
// For read-only grids, use Extended for better UX
treeGrid.SelectionMode = GridSelectionMode.Extended;

// For action-based selection (checkboxes), use Multiple
treeGrid.SelectionMode = GridSelectionMode.Multiple;
```

### 2. Handle Selection Events Efficiently

```csharp
// Use SelectionChanging to prevent invalid selections
treeGrid.SelectionChanging += (s, e) =>
{
    // Validation logic here
    if (!IsValidSelection(e.AddedItems))
    {
        e.Cancel = true;
    }
};
```

### 3. Provide Visual Feedback

```xaml
<!-- Customize selection colors -->
<syncfusion:SfTreeGrid.SelectionStyle>
    <Style TargetType="syncfusion:TreeGridCell">
        <Setter Property="Background" Value="LightBlue"/>
    </Style>
</syncfusion:SfTreeGrid.SelectionStyle>
```

## Troubleshooting

### Issue: Cannot Select Rows

```csharp
// Verify selection mode
treeGrid.SelectionMode = GridSelectionMode.Single; // or Multiple/Extended

// Check if selection is not disabled
treeGrid.SelectionMode != GridSelectionMode.None
```

### Issue: Ctrl+Click Not Working

```csharp
// Use Extended mode for Ctrl+Click
treeGrid.SelectionMode = GridSelectionMode.Extended;
```

### Issue: Selected Item Not Visible

```csharp
// Scroll selected item into view
treeGrid.SelectionChanged += (s, e) =>
{
    if (treeGrid.SelectedIndex >= 0)
    {
        treeGrid.ScrollInView(new RowColumnIndex(treeGrid.SelectedIndex, 0));
    }
};
```
