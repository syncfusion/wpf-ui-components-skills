# WPF DataGrid Editing

## Table of Contents

- [Overview](#overview)
- [Enabling Editing](#enabling-editing)
- [Edit Triggers](#edit-triggers)
- [Edit Events](#edit-events)
- [Programmatic Editing](#programmatic-editing)
- [IEditableObject Interface](#ieditableobject-interface)
- [Cell Click Events](#cell-click-events)
- [Focus and Keyboard Control](#focus-and-keyboard-control)
- [Advanced Scenarios](#advanced-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion WPF DataGrid (SfDataGrid) provides comprehensive editing capabilities that allow users to modify data in cells. The control supports various edit triggers, validation, and programmatic editing operations.

## Enabling Editing

Enable editing for the entire grid by setting the `AllowEditing` property:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowEditing="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowEditing = true;
```

Disable editing for specific columns:

```xaml
<syncfusion:GridTextColumn MappingName="OrderID" 
                           AllowEditing="False" />
```

## Edit Triggers

Control when cells enter edit mode using the `EditTrigger` property:

### OnTap Trigger

Cells enter edit mode with a single tap/click:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowEditing="True"
                       EditTrigger="OnTap"
                       ItemsSource="{Binding Orders}" />
```

### OnDoubleTap Trigger

Cells enter edit mode with a double tap/click (default):

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowEditing="True"
                       EditTrigger="OnDoubleTap"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.EditTrigger = EditTrigger.OnDoubleTap;
```

## Edit Events

### CurrentCellBeginEdit Event

Raised when a cell enters edit mode. Cancel editing based on conditions:

```csharp
dataGrid.CurrentCellBeginEdit += DataGrid_CurrentCellBeginEdit;

void DataGrid_CurrentCellBeginEdit(object sender, CurrentCellBeginEditEventArgs e)
{
    // Cancel editing for specific columns
    if (e.Column.MappingName == "OrderID")
        e.Cancel = true;
    
    // Cancel editing based on data
    var record = e.RowData as OrderInfo;
    if (record != null && record.IsShipped)
        e.Cancel = true;
}
```

### CurrentCellEndEdit Event

Raised when a cell exits edit mode:

```csharp
dataGrid.CurrentCellEndEdit += DataGrid_CurrentCellEndEdit;

void DataGrid_CurrentCellEndEdit(object sender, CurrentCellEndEditEventArgs e)
{
    // Perform actions after editing completes
    var newValue = e.NewValue;
    var oldValue = e.OldValue;
    
    // Log changes
    Debug.WriteLine($"Cell edited: {newValue}");
}
```

### CurrentCellValueChanged Event

Raised when cell value changes:

```csharp
dataGrid.CurrentCellValueChanged += DataGrid_CurrentCellValueChanged;

void DataGrid_CurrentCellValueChanged(object sender, CurrentCellValueChangedEventArgs e)
{
    var column = e.Column.MappingName;
    var newValue = e.Record.GetType().GetProperty(column).GetValue(e.Record);
    
    // Update related fields
    if (column == "Quantity")
    {
        UpdateTotalPrice(e.Record);
    }
}
```

### CurrentCellDropDownSelectionChanged Event

Raised when ComboBox selection changes:

```csharp
dataGrid.CurrentCellDropDownSelectionChanged += DataGrid_DropDownSelectionChanged;

void DataGrid_DropDownSelectionChanged(object sender, CurrentCellDropDownSelectionChangedEventArgs e)
{
    var selectedItem = e.SelectedItem;
    var selectedIndex = e.SelectedIndex;
}
```

## Programmatic Editing

### BeginEdit Method

Programmatically enter edit mode:

```csharp
// Edit cell at specific row and column index
dataGrid.SelectionController.CurrentCellManager.BeginEdit();

// Edit specific cell
int rowIndex = 3;
int columnIndex = 2;
dataGrid.MoveToCurrentCell(new RowColumnIndex(rowIndex, columnIndex));
dataGrid.SelectionController.CurrentCellManager.BeginEdit();
```

### EndEdit Method

Complete editing and commit changes:

```csharp
dataGrid.SelectionController.CurrentCellManager.EndEdit();
```

### CancelEdit Method

Cancel editing and restore original value:

```csharp
dataGrid.SelectionController.CurrentCellManager.CancelEdit();
```

## IEditableObject Interface

Implement IEditableObject for transaction-based editing:

```csharp
public class OrderInfo : INotifyPropertyChanged, IEditableObject
{
    private OrderInfo backupData;
    private bool inEdit;
    
    public void BeginEdit()
    {
        if (inEdit) return;
        
        inEdit = true;
        backupData = this.MemberwiseClone() as OrderInfo;
    }
    
    public void EndEdit()
    {
        if (!inEdit) return;
        
        inEdit = false;
        backupData = null;
    }
    
    public void CancelEdit()
    {
        if (!inEdit) return;
        
        inEdit = false;
        
        // Restore values
        this.OrderID = backupData.OrderID;
        this.CustomerName = backupData.CustomerName;
        this.TotalPrice = backupData.TotalPrice;
    }
    
    // Properties and INotifyPropertyChanged implementation
}
```

## Cell Click Events

### CellTapped Event

Handle single tap/click on cells:

```csharp
dataGrid.CellTapped += DataGrid_CellTapped;

void DataGrid_CellTapped(object sender, GridCellTappedEventArgs e)
{
    var rowIndex = e.RowColumnIndex.RowIndex;
    var columnIndex = e.RowColumnIndex.ColumnIndex;
    var record = e.Record;
    var column = e.Column;
    
    // Perform custom action
    if (column.MappingName == "Actions")
    {
        ShowDetailsDialog(record);
    }
}
```

### CellDoubleTapped Event

Handle double tap/click on cells:

```csharp
dataGrid.CellDoubleTapped += DataGrid_CellDoubleTapped;

void DataGrid_CellDoubleTapped(object sender, GridCellDoubleTappedEventArgs e)
{
    // Open detailed editor
    OpenCustomEditor(e.Record, e.Column.MappingName);
}
```

## Focus and Keyboard Control

### FocusManagerHelper Properties

Control focus behavior for UIElements in templates:

```xaml
<syncfusion:GridTemplateColumn MappingName="Actions">
    <syncfusion:GridTemplateColumn.CellTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Button Content="Edit" 
                        syncfusion:FocusManagerHelper.FocusedElement="True"
                        syncfusion:FocusManagerHelper.WantsKeyInput="True" />
                <Button Content="Delete" 
                        syncfusion:FocusManagerHelper.WantsKeyInput="True" />
            </StackPanel>
        </DataTemplate>
    </syncfusion:GridTemplateColumn.CellTemplate>
</syncfusion:GridTemplateColumn>
```

### WantsKeyInput Property

Allow keyboard input for custom controls:

```xaml
<syncfusion:GridTemplateColumn MappingName="Rating">
    <syncfusion:GridTemplateColumn.CellTemplate>
        <DataTemplate>
            <ComboBox syncfusion:FocusManagerHelper.WantsKeyInput="True"
                      ItemsSource="{Binding RatingOptions}" />
        </DataTemplate>
    </syncfusion:GridTemplateColumn.CellTemplate>
</syncfusion:GridTemplateColumn>
```

### WantsMouseInput Property

Handle mouse input for UIElements:

```xaml
<Button Content="Action"
        syncfusion:VisualContainer.WantsMouseInput="True"
        Click="ActionButton_Click" />
```

## Advanced Scenarios

### Tracking Edited Cells

Track which cells have been edited using CellStyleSelector:

```csharp
public class EditedCellStyleSelector : StyleSelector
{
    private HashSet<Tuple<object, string>> editedCells = new HashSet<Tuple<object, string>>();
    
    public override Style SelectStyle(object item, DependencyObject container)
    {
        var gridCell = container as GridCell;
        if (gridCell == null) return null;
        
        var record = item;
        var columnName = gridCell.ColumnBase.GridColumn.MappingName;
        var key = Tuple.Create(record, columnName);
        
        if (editedCells.Contains(key))
            return App.Current.Resources["EditedCellStyle"] as Style;
        
        return null;
    }
    
    public void MarkCellAsEdited(object record, string columnName)
    {
        editedCells.Add(Tuple.Create(record, columnName));
    }
}
```

Apply the selector:

```xaml
<Style x:Key="EditedCellStyle" TargetType="syncfusion:GridCell">
    <Setter Property="Background" Value="LightYellow"/>
</Style>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       CellStyleSelector="{StaticResource editedCellStyleSelector}">
```

### Allowing Editing on Minus Key

Enable editing when minus key is pressed:

```csharp
dataGrid.SelectionController.CurrentCellManager.CurrentCell.IsEditing = true;
```

Custom key press handling:

```csharp
protected override void OnKeyDown(KeyEventArgs e)
{
    if (e.Key == Key.OemMinus || e.Key == Key.Subtract)
    {
        var currentCell = dataGrid.SelectionController.CurrentCellManager.CurrentCell;
        if (!currentCell.IsEditing)
        {
            dataGrid.SelectionController.CurrentCellManager.BeginEdit();
        }
    }
    base.OnKeyDown(e);
}
```

## Troubleshooting

### Issue: Editing Doesn't Start

**Solution:**
- Check that `AllowEditing` is set to `true`
- Verify column's `AllowEditing` is not `false`
- Check if `CurrentCellBeginEdit` event is canceling edit

### Issue: Changes Not Saved

**Solution:**
- Ensure proper INotifyPropertyChanged implementation
- Call `EndEdit()` to commit changes
- Check data source supports updates

### Issue: Validation Errors Not Displayed

**Solution:**
- Implement IDataErrorInfo or INotifyDataErrorInfo
- Set appropriate ValidationMode
- Style validation error template

### Issue: Cannot Edit Template Columns

**Solution:**
- Use `EditTemplate` for template columns
- Set `FocusManagerHelper.FocusedElement` on editable control
- Implement proper two-way binding

### Issue: Navigation Keys Not Working During Edit

**Solution:**
- Set `NavigationMode` property appropriately
- Use `WantsKeyInput` for custom controls
- Handle key events in custom editors

## Edge Cases

### Multi-line Text Editing

For GridTextColumn with multi-line text:

```xaml
<syncfusion:GridTextColumn MappingName="Description">
    <syncfusion:GridTextColumn.EditTemplate>
        <DataTemplate>
            <TextBox Text="{Binding Description, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                     TextWrapping="Wrap"
                     AcceptsReturn="True" />
        </DataTemplate>
    </syncfusion:GridTextColumn.EditTemplate>
</syncfusion:GridTextColumn>
```

### Conditional Editing Based on Other Cell Values

```csharp
dataGrid.CurrentCellBeginEdit += (s, e) =>
{
    var record = e.RowData as OrderInfo;
    
    // Allow editing UnitPrice only if Quantity > 0
    if (e.Column.MappingName == "UnitPrice" && record.Quantity == 0)
        e.Cancel = true;
};
```

### Immediate Update on Value Change

```xaml
<syncfusion:GridTextColumn MappingName="Quantity">
    <syncfusion:GridTextColumn.EditTemplate>
        <DataTemplate>
            <TextBox Text="{Binding Quantity, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
        </DataTemplate>
    </syncfusion:GridTextColumn.EditTemplate>
</syncfusion:GridTextColumn>
```
