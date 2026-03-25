# Selection in WPF DataGrid (SfDataGrid)

Comprehensive reference for selection operations, modes, and customization in the Syncfusion WPF DataGrid control.

## Table of Contents
- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [Selection Units](#selection-units)
- [Navigation Modes](#navigation-modes)
- [Programmatic Selection](#programmatic-selection)
- [Getting Selected Items](#getting-selected-items)
- [Selection in Master-Details View](#selection-in-master-details-view)
- [Selection Events](#selection-events)
- [Selection Appearance](#selection-appearance)
- [Keyboard and Mouse Behavior](#keyboard-and-mouse-behavior)
- [Troubleshooting](#troubleshooting)

## Overview

WPF DataGrid supports selecting one or more rows or cells through UI interaction or programmatically. Selection enables users to highlight and work with specific data in the grid.

**Key Features:**
- Row and cell selection
- Single and multiple selection
- Keyboard and mouse navigation
- Programmatic selection control
- Selection events
- Customizable appearance
- Master-details view selection

## Selection Modes

Control how many items can be selected:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       SelectionMode="Extended"
                       ItemsSource="{Binding Orders}" />
```

### None
Disables selection completely:

```csharp
dataGrid.SelectionMode = GridSelectionMode.None;
```

### Single
Allows selecting only one row or cell:

```csharp
dataGrid.SelectionMode = GridSelectionMode.Single;
```

**Behavior**: Selecting new item clears previous selection.

### SingleDeselect
Single selection with ability to deselect:

```csharp
dataGrid.SelectionMode = GridSelectionMode.SingleDeselect;
```

**Features**:
- Click same item to deselect
- Press Space key to select/deselect
- Previous selection cleared on new selection

### Multiple
Multiple selection without modifier keys:

```csharp
dataGrid.SelectionMode = GridSelectionMode.Multiple;
```

**Features**:
- Click to select/deselect items
- No Ctrl key required
- Navigation keys move current cell only
- Space key to select/deselect

### Extended
Multiple selection with modifier keys:

```csharp
dataGrid.SelectionMode = GridSelectionMode.Extended;
```

**Features**:
- Hold Ctrl for multi-select
- Hold Shift for range selection
- Most common mode for multi-selection

## Selection Units

Define what can be selected:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       SelectionUnit="Row"
                       ItemsSource="{Binding Orders}" />
```

### Row
Select entire rows:

```csharp
dataGrid.SelectionUnit = GridSelectionUnit.Row;
```

### Cell
Select individual cells:

```csharp
dataGrid.SelectionUnit = GridSelectionUnit.Cell;
```

### Any
Select rows by clicking row header, cells by clicking cells:

```csharp
dataGrid.SelectionUnit = GridSelectionUnit.Any;
```

## Navigation Modes

Control keyboard navigation:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       NavigationMode="Cell"
                       ItemsSource="{Binding Orders}" />
```

### Cell
Navigate between cells and rows:

```csharp
dataGrid.NavigationMode = NavigationMode.Cell;
```

### Row
Navigate only between rows:

```csharp
dataGrid.NavigationMode = NavigationMode.Row;
```

**Note**: Cannot use Row navigation with Cell/Any selection unit.

## Programmatic Selection

### Select Single Item

```csharp
// By index
var recordIndex = dataGrid.ResolveToRecordIndex(5);
dataGrid.SelectedIndex = recordIndex;

// By item
var record = dataGrid.GetRecordAtRowIndex(6);
dataGrid.SelectedItem = record;
```

### Select Multiple Items

```csharp
// Add to SelectedItems
var viewModel = dataGrid.DataContext as ViewModel;

foreach (var order in viewModel.Orders)
{
    if (order.Country == "Mexico")
        dataGrid.SelectedItems.Add(order);
}
```

### Select Range of Rows

```csharp
// Select rows 3 through 7
dataGrid.SelectRows(3, 7);
```

### Select Specific Cell

```csharp
var record = dataGrid.GetRecordAtRowIndex(3);
var column = dataGrid.Columns[1];
dataGrid.SelectCell(record, column);
```

### Select Range of Cells

```csharp
dataGrid.Loaded += DataGrid_Loaded;
dataGrid.SelectionController = new GridSelectionControllerExt(dataGrid);

void DataGrid_Loaded(object sender, RoutedEventArgs e)
{
    var firstRecord = dataGrid.GetRecordAtRowIndex(3);
    var lastRecord = dataGrid.GetRecordAtRowIndex(7);
    var firstColumn = dataGrid.Columns[1];
    var lastColumn = dataGrid.Columns[3];
    
    dataGrid.SelectCells(firstRecord, firstColumn, lastRecord, lastColumn);
    (dataGrid.SelectionController as GridSelectionControllerExt).rowColumnIndex = new RowColumnIndex(7, 3);
    (dataGrid.SelectionController as GridSelectionControllerExt).UpdatePressedIndex();
}

public class GridSelectionControllerExt : GridCellSelectionController
{
    public RowColumnIndex rowColumnIndex { get; set; }
    
    public GridSelectionControllerExt(SfDataGrid datagrid) : base(datagrid)
    {
    }
    
    public void UpdatePressedIndex()
    {
        this.isInShiftSelection = true;
        this.PressedRowColumnIndex = rowColumnIndex;
    }
}
```

### Select All

```csharp
dataGrid.SelectAll();
```

### Set Current Item

```csharp
var viewModel = dataGrid.DataContext as ViewModel;
dataGrid.CurrentItem = viewModel.Orders.FirstOrDefault(order => order.Country == "Spain");
```

### Move Current Cell

```csharp
dataGrid.MoveCurrentCell(new RowColumnIndex(3, 2), false);
```

### Clear Selection

```csharp
// Clear all selection
dataGrid.SelectionController.ClearSelections(true);

// Or for row selection
dataGrid.SelectedItem = null;
dataGrid.SelectedItems.Clear();
```

### Unselect Cells

```csharp
// Unselect range
var firstRecord = dataGrid.GetRecordAtRowIndex(3);
var lastRecord = dataGrid.GetRecordAtRowIndex(7);
var firstColumn = dataGrid.Columns[1];
var lastColumn = dataGrid.Columns[3];
dataGrid.UnSelectCells(firstRecord, firstColumn, lastRecord, lastColumn);

// Unselect specific cell
var removeRecord = dataGrid.GetRecordAtRowIndex(5);
var removeColumn = dataGrid.Columns[2];
dataGrid.UnSelectCell(removeRecord, removeColumn);
```

## Getting Selected Items

### Row Selection

```csharp
// First selected item
var selectedItem = dataGrid.SelectedItem;

// Selected index
var selectedIndex = dataGrid.SelectedIndex;

// All selected items
var selectedItems = dataGrid.SelectedItems;

// Selected row information
var selectedRows = dataGrid.SelectionController.SelectedRows;
```

### Cell Selection

```csharp
// Current cell info
var currentCellInfo = dataGrid.CurrentCellInfo;

// Selected cells collection
var selectedCells = dataGrid.SelectionController.SelectedCells;

// Get selected cells as GridCellInfo collection
List<GridCellInfo> selectedCells = dataGrid.GetSelectedCells();
```

### Current Item vs Selected Item

```csharp
// SelectedItem: First selected item
var firstSelected = dataGrid.SelectedItem;

// CurrentItem: Item with current focus
var currentItem = dataGrid.CurrentItem;

// CurrentColumn: Column with current focus
var currentColumn = dataGrid.CurrentColumn;
```

**Difference**: When multiple items selected, SelectedItem is first selected, CurrentItem is currently focused.

## Selection in Master-Details View

### Get Selected DetailsViewDataGrid

```csharp
// First level
var detailsViewDataGrid = dataGrid.SelectedDetailsViewGrid;

// Nested level
var nestedDetailsView = dataGrid.SelectedDetailsViewGrid.SelectedDetailsViewGrid;
```

### Get SelectedItem from DetailsViewDataGrid

```csharp
var detailsViewDataGrid = dataGrid.SelectedDetailsViewGrid;
var selectedItem = detailsViewDataGrid.SelectedItem;
```

Using SelectionChanged event:

```xaml
<syncfusion:GridViewDefinition RelationalColumn="OrderDetails">
    <syncfusion:GridViewDefinition.DataGrid>
        <syncfusion:SfDataGrid x:Name="FirstDetailsViewGrid"
                              SelectionChanged="FirstDetailsViewGrid_SelectionChanged" />
    </syncfusion:GridViewDefinition.DataGrid>
</syncfusion:GridViewDefinition>
```

```csharp
void FirstDetailsViewGrid_SelectionChanged(object sender, GridSelectionChangedEventArgs e)
{
    var selectedItem = (e.OriginalSender as DetailsViewDataGrid).SelectedItem;
    var selectedIndex = (e.OriginalSender as DetailsViewDataGrid).SelectedIndex;
    var currentItem = (e.OriginalSender as DetailsViewDataGrid).CurrentItem;
}
```

### Programmatic Selection in DetailsViewDataGrid

```csharp
dataGrid.DetailsViewLoading += DataGrid_DetailsViewLoading;

void DataGrid_DetailsViewLoading(object sender, DetailsViewLoadingAndUnloadingEventArgs e)
{
    var record = e.DetailsViewDataGrid.GetRecordAtRowIndex(e.DetailsViewDataGrid.GetFirstDataRowIndex());
    e.DetailsViewDataGrid.SelectedItem = record;
}
```

### Get Parent DataGrid

```csharp
using Syncfusion.UI.Xaml.Grid.Helpers;

// Immediate parent
var parentDataGrid = detailsViewDataGrid.GetParentDataGrid();

// Top-level parent
var topLevelDataGrid = detailsViewDataGrid.GetTopLevelParentDataGrid();
```

### Get DetailsViewDataGrid by Index

```csharp
// By row index
var detailsViewDataGrid = dataGrid.GetDetailsViewGrid(2);

// By record index and relation name
var detailsViewDataGrid = dataGrid.GetDetailsViewGrid(0, "ProductDetails");
```

## Selection Events

### CurrentCellActivating Event

Raised before moving current cell:

```csharp
dataGrid.CurrentCellActivating += DataGrid_CurrentCellActivating;

void DataGrid_CurrentCellActivating(object sender, CurrentCellActivatingEventArgs e)
{
    // Cancel current cell movement
    var provider = dataGrid.View.GetPropertyAccessProvider();
    var record = dataGrid.GetRecordAtRowIndex(e.CurrentRowColumnIndex.RowIndex);
    
    if (record != null)
    {
        var column = dataGrid.Columns[dataGrid.ResolveToGridVisibleColumnIndex(e.CurrentRowColumnIndex.ColumnIndex)];
        var cellValue = provider.GetValue(record, column.MappingName);
        
        if (cellValue.ToString() == "1001")
            e.Cancel = true;
    }
}
```

**Event Arguments**:
- **ActivationTrigger**: Reason for moving current cell
- **CurrentRowColumnIndex**: Target cell position
- **PreviousRowColumnIndex**: Source cell position
- **Cancel**: Set true to cancel movement

### CurrentCellActivated Event

Raised after current cell is moved:

```csharp
dataGrid.CurrentCellActivated += DataGrid_CurrentCellActivated;

void DataGrid_CurrentCellActivated(object sender, CurrentCellActivatedEventArgs e)
{
    // Perform action after current cell moved
    Debug.WriteLine($"Current cell moved to: Row {e.CurrentRowColumnIndex.RowIndex}, Column {e.CurrentRowColumnIndex.ColumnIndex}");
}
```

### SelectionChanging Event

Raised before processing selection (keyboard/mouse only):

```csharp
dataGrid.SelectionChanging += DataGrid_SelectionChanging;

void DataGrid_SelectionChanging(object sender, GridSelectionChangingEventArgs e)
{
    // Cancel selection for unbound rows
    var unboundRow = e.AddedItems.Where(rowInfo => (rowInfo as GridRowInfo).IsUnBoundRow).ToList();
    
    if (unboundRow.Count() > 0)
        e.Cancel = true;
}
```

**Event Arguments**:
- **AddedItems**: Items being selected
- **RemovedItems**: Items being unselected
- **Cancel**: Set true to cancel selection

### SelectionChanged Event

Raised after selection is processed:

```csharp
dataGrid.SelectionChanged += DataGrid_SelectionChanged;

void DataGrid_SelectionChanged(object sender, GridSelectionChangedEventArgs e)
{
    // Log selection changes
    foreach (var item in e.AddedItems)
    {
        Debug.WriteLine($"Selected: {item}");
    }
    
    foreach (var item in e.RemovedItems)
    {
        Debug.WriteLine($"Unselected: {item}");
    }
}
```

## Selection Appearance

### Change Selection Colors

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       RowSelectionBrush="#FFDFF3F4"
                       GroupRowSelectionBrush="#FFC8E3E3"
                       SelectionForegroundBrush="DarkBlue"
                       SelectionMode="Extended" />
```

```csharp
dataGrid.RowSelectionBrush = new SolidColorBrush(Color.FromRgb(223, 243, 244));
dataGrid.GroupRowSelectionBrush = new SolidColorBrush(Color.FromRgb(200, 227, 227));
dataGrid.SelectionForegroundBrush = new SolidColorBrush(Colors.DarkBlue);
```

### Customize Current Cell Border

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       CurrentCellBorderBrush="Red"
                       CurrentCellBorderThickness="1.6" />
```

```csharp
dataGrid.CurrentCellBorderBrush = new SolidColorBrush(Colors.Red);
dataGrid.CurrentCellBorderThickness = new Thickness(1.6);
```

### Customize Row Selection Border

Edit VirtualizingCellsControl template:

```xaml
<Style TargetType="syncfusion:VirtualizingCellsControl">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="syncfusion:VirtualizingCellsControl">
                <Grid>
                    <Border x:Name="PART_RowBorder"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}" />
                    
                    <!-- Custom selection border -->
                    <Border x:Name="PART_SelectionBorder"
                            BorderBrush="Red"
                            BorderThickness="1.5,1.5,1.5,2.5"
                            Opacity="0.75"
                            Background="{TemplateBinding RowSelectionBrush}"
                            Clip="{TemplateBinding SelectionBorderClipRect}"
                            Visibility="{TemplateBinding SelectionBorderVisiblity}" />
                    
                    <Border BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}">
                        <ContentPresenter />
                    </Border>
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### Customize Cell Selection

Edit GridCell template:

```xaml
<Style TargetType="syncfusion:GridCell">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="syncfusion:GridCell">
                <Grid>
                    <Border Background="{TemplateBinding CellSelectionBrush}"
                            Visibility="{TemplateBinding SelectionBorderVisibility}" />
                    
                    <Border x:Name="PART_GridCellBorder"
                            Background="{TemplateBinding Background}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}">
                        <Grid>
                            <ContentPresenter Margin="{TemplateBinding Padding}" />
                        </Grid>
                    </Border>
                    
                    <!-- Custom current cell border -->
                    <Border Background="Transparent"
                            BorderBrush="Red"
                            BorderThickness="0.5"
                            Margin="0,0,1,1"
                            Visibility="{TemplateBinding CurrentCellBorderVisibility}" />
                    
                    <!-- Inner border -->
                    <Border Background="Transparent"
                            BorderBrush="Red"
                            BorderThickness="0.5"
                            Margin="2,2,3,3"
                            Visibility="{TemplateBinding CurrentCellBorderVisibility}" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

## Keyboard and Mouse Behavior

### Keyboard Navigation

| Key | Description |
|-----|-------------|
| Down Arrow | Move current cell down |
| Up Arrow | Move current cell up |
| Left Arrow | Move current cell left |
| Right Arrow | Move current cell right |
| Home | Move to first cell in row |
| End | Move to last cell in row |
| Ctrl + Home | Move to first cell in grid |
| Ctrl + End | Move to last cell in grid |
| Page Down | Scroll down one page |
| Page Up | Scroll up one page |
| Tab | Move to next cell |
| Shift + Tab | Move to previous cell |
| Enter | Commit edit and move down |
| Ctrl + Enter | Commit edit and stay |
| F2 | Enter edit mode |
| Esc | Cancel edit |
| Delete | Delete current row |
| Ctrl + A | Select all |

### Shift Key Combinations

With SelectionMode as Extended:

| Key | Description |
|-----|-------------|
| Shift + Down | Select from current to next row |
| Shift + Up | Select from current to previous row |
| Shift + Left | Select from current to previous cell |
| Shift + Right | Select from current to next cell |
| Shift + Home | Select from current to first cell in row |
| Shift + End | Select from current to last cell in row |
| Shift + Page Down | Select from current to next page |
| Shift + Page Up | Select from current to previous page |
| Shift + Ctrl + Down | Select from current to last row |
| Shift + Ctrl + Up | Select from current to first row |
| Shift + Ctrl + Home | Select from current to first cell |
| Shift + Ctrl + End | Select from current to last cell |

### Mouse Behavior

```csharp
// Enable/disable selection on pointer pressed
dataGrid.AllowSelectionOnPointerPressed = true;
```

**Extended Mode**:
- Ctrl + Click: Add/remove from selection
- Shift + Click: Select range
- Drag: Select multiple (if enabled)

## Troubleshooting

### Selection Not Working

**Issue**: Cannot select rows or cells

**Solutions**:
1. Verify SelectionMode is not None
2. Check SelectionUnit setting
3. Ensure AllowSelection not overridden
4. Test with simple data first

```csharp
// Diagnostic check
Debug.WriteLine($"SelectionMode: {dataGrid.SelectionMode}");
Debug.WriteLine($"SelectionUnit: {dataGrid.SelectionUnit}");
```

### Current Cell Not Visible

**Issue**: Current cell border not showing

**Solutions**:
1. Check CurrentCellBorderThickness not zero
2. Verify CurrentCellBorderBrush color visible
3. Ensure cell is in view
4. Test with default style

### Selection Events Not Firing

**Issue**: SelectionChanged or other events not raised

**Solutions**:
1. Verify event handler registered
2. Check if programmatic selection (doesn't fire SelectionChanging)
3. Ensure selection actually changes
4. Test with simple event handler

```csharp
dataGrid.SelectionChanged += (s, e) => 
{
    Debug.WriteLine($"Selection changed: Added={e.AddedItems.Count}, Removed={e.RemovedItems.Count}");
};
```

### Multi-Selection Not Working

**Issue**: Cannot select multiple items

**Solutions**:
1. Set SelectionMode to Extended or Multiple
2. Hold Ctrl/Shift keys (Extended mode)
3. Check for event handlers canceling selection
4. Verify mouse capture not blocked

### Selection Cleared Unexpectedly

**Issue**: Selection lost during operations

**Solutions**:
1. Save selection before data operations
2. Use View operations instead of ItemsSource replacement
3. Check for code clearing selection
4. Implement selection persistence

```csharp
// Save and restore selection
private List<object> _savedSelection;

private void SaveSelection()
{
    _savedSelection = dataGrid.SelectedItems.Cast<object>().ToList();
}

private void RestoreSelection()
{
    dataGrid.SelectedItems.Clear();
    foreach (var item in _savedSelection)
    {
        dataGrid.SelectedItems.Add(item);
    }
}
```

### Performance Issues with Large Selection

**Issue**: Slow when selecting many items

**Solutions**:
1. Use BeginInit/EndInit for bulk selection
2. Disable UI updates during selection
3. Consider paging for very large datasets
4. Optimize SelectionChanged event handlers

```csharp
// Efficient bulk selection
dataGrid.SelectionController.SuspendUpdates();
try
{
    foreach (var item in itemsToSelect)
    {
        dataGrid.SelectedItems.Add(item);
    }
}
finally
{
    dataGrid.SelectionController.ResumeUpdates();
}
```
