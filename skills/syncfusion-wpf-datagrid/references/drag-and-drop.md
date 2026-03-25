# WPF DataGrid Drag and Drop

## Overview

The Syncfusion WPF DataGrid (SfDataGrid) provides built-in row drag-and-drop functionality that allows users to reorder rows within the same grid or drag rows between different grids. This guide covers drag-drop configuration, events, customization, and advanced scenarios.

## Enabling Row Drag and Drop

### Basic Configuration

Enable row dragging within the same DataGrid:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDraggingRows="True"
                       AllowDrop="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowDraggingRows = true;
dataGrid.AllowDrop = true;
```

## Row Drop Indicators

### RowDropIndicatorMode Property

Control how drop indicators are displayed:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDraggingRows="True"
                       AllowDrop="True"
                       RowDropIndicatorMode="DropAboveAndBelow"
                       ItemsSource="{Binding Orders}" />
```

```csharp
// Available modes
dataGrid.RowDropIndicatorMode = RowDropIndicatorMode.DropAboveAndBelow;  // Default
dataGrid.RowDropIndicatorMode = RowDropIndicatorMode.DropAsChild;
dataGrid.RowDropIndicatorMode = RowDropIndicatorMode.None;
```

## Dragging Multiple Rows

Select and drag multiple rows simultaneously:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDraggingRows="True"
                       AllowDrop="True"
                       SelectionMode="Extended"
                       ItemsSource="{Binding Orders}" />
```

```csharp
// Enable multiple selection
dataGrid.SelectionMode = GridSelectionMode.Extended;

// Select multiple rows programmatically
dataGrid.SelectedItems.Add(orders[0]);
dataGrid.SelectedItems.Add(orders[2]);
dataGrid.SelectedItems.Add(orders[5]);
```

## Drag-Drop Events

### DragStart Event

Raised when drag operation starts:

```csharp
dataGrid.RowDragDropController.DragStart += RowDragDropController_DragStart;

void RowDragDropController_DragStart(object sender, GridRowDragStartEventArgs e)
{
    var draggingRecords = e.DraggingRecords;
    var dataObject = e.Data;
    
    // Cancel drag for specific records
    var order = draggingRecords.FirstOrDefault() as OrderInfo;
    if (order != null && order.IsShipped)
    {
        e.Handled = true;  // Cancel drag
        MessageBox.Show("Cannot drag shipped orders");
    }
}
```

### DragOver Event

Raised continuously while dragging over the grid:

```csharp
dataGrid.RowDragDropController.DragOver += RowDragDropController_DragOver;

void RowDragDropController_DragOver(object sender, GridRowDragOverEventArgs e)
{
    var targetRecord = e.TargetRecord;
    var dropPosition = e.DropPosition;
    var isFromOutsideSource = e.IsFromOutsideSource;
    
    // Prevent dropping on specific rows
    if (targetRecord is OrderInfo order && order.IsLocked)
    {
        e.ShowDragUI = false;  // Hide drop indicator
    }
}
```

### DragLeave Event

Raised when dragging leaves the grid:

```csharp
dataGrid.RowDragDropController.DragLeave += RowDragDropController_DragLeave;

void RowDragDropController_DragLeave(object sender, GridRowDragLeaveEventArgs e)
{
    // Perform cleanup or show message
    Debug.WriteLine("Drag left the grid");
}
```

### Drop Event

Raised when drop operation occurs (before processing):

```csharp
dataGrid.RowDragDropController.Drop += RowDragDropController_Drop;

void RowDragDropController_Drop(object sender, GridRowDropEventArgs e)
{
    var draggingRecords = e.DraggingRecords;
    var targetRecord = e.TargetRecord;
    var dropPosition = e.DropPosition;
    
    // Cancel drop based on conditions
    if (targetRecord is OrderInfo targetOrder && targetOrder.Status == "Completed")
    {
        e.Handled = true;  // Cancel drop
        MessageBox.Show("Cannot drop on completed orders");
    }
}
```

### Dropped Event

Raised after drop operation completes:

```csharp
dataGrid.RowDragDropController.Dropped += RowDragDropController_Dropped;

void RowDragDropController_Dropped(object sender, GridRowDroppedEventArgs e)
{
    var draggingRecords = e.DraggingRecords;
    var targetRecord = e.TargetRecord;
    var dropPosition = e.DropPosition;
    
    // Log drop operation
    foreach (var record in draggingRecords)
    {
        LogRowMove(record, targetRecord, dropPosition);
    }
}
```

## Customizing Drag-Drop Behavior

### Disabling Drag for Specific Rows

Prevent dragging specific rows based on conditions:

```csharp
dataGrid.RowDragDropController.DragStart += (s, e) =>
{
    var order = e.DraggingRecords.FirstOrDefault() as OrderInfo;
    
    // Disable drag for shipped orders
    if (order != null && order.IsShipped)
    {
        e.Handled = true;
        e.ShowDragUI = false;
    }
};
```

### Disabling Drop Over Specific Rows

Prevent dropping on specific rows:

```csharp
dataGrid.RowDragDropController.DragOver += (s, e) =>
{
    var targetOrder = e.TargetRecord as OrderInfo;
    
    // Disable drop on locked rows
    if (targetOrder != null && targetOrder.IsLocked)
    {
        e.ShowDragUI = false;
    }
};
```

### Disabling Default Drag UI

Hide the default drag indicator:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDraggingRows="True"
                       AllowDrop="True"
                       ShowDragUI="False"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.ShowDragUI = false;
```

### Custom Drag UI Template

Define a custom template for the drag indicator:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDraggingRows="True"
                       AllowDrop="True">
    <syncfusion:SfDataGrid.RowDragDropTemplate>
        <DataTemplate>
            <Border Background="LightBlue" 
                    BorderBrush="Blue" 
                    BorderThickness="1" 
                    Padding="5">
                <StackPanel Orientation="Horizontal">
                    <Image Source="/Images/drag-icon.png" 
                           Width="16" Height="16" 
                           Margin="0,0,5,0"/>
                    <TextBlock Text="{Binding Count}" 
                               FontWeight="Bold"/>
                    <TextBlock Text=" items selected" 
                               Margin="5,0,0,0"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfDataGrid.RowDragDropTemplate>
</syncfusion:SfDataGrid>
```

## Drag-Drop Between Controls

### Drag-Drop Between Two DataGrids

Enable dragging rows between different DataGrids:

```xaml
<!-- Source DataGrid -->
<syncfusion:SfDataGrid x:Name="sourceGrid"
                       AllowDraggingRows="True"
                       AllowDrop="True"
                       ItemsSource="{Binding PendingOrders}" />

<!-- Target DataGrid -->
<syncfusion:SfDataGrid x:Name="targetGrid"
                       AllowDraggingRows="True"
                       AllowDrop="True"
                       ItemsSource="{Binding CompletedOrders}" />
```

Handle cross-grid drag-drop:

```csharp
sourceGrid.RowDragDropController.Dropped += (s, e) =>
{
    if (e.IsFromOutsideSource)
    {
        // Add to target collection
        var targetCollection = targetGrid.ItemsSource as ObservableCollection<OrderInfo>;
        foreach (var record in e.DraggingRecords.Cast<OrderInfo>())
        {
            targetCollection.Add(record);
        }
        
        // Remove from source collection
        var sourceCollection = sourceGrid.ItemsSource as ObservableCollection<OrderInfo>;
        foreach (var record in e.DraggingRecords.Cast<OrderInfo>())
        {
            sourceCollection.Remove(record);
        }
    }
};
```

### Drag-Drop Between DataGrid and ListView

Drag rows from DataGrid to ListView:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDraggingRows="True"
                       ItemsSource="{Binding Orders}" />

<ListView x:Name="listView"
          AllowDrop="True"
          ItemsSource="{Binding SelectedOrders}" />
```

```csharp
// Handle ListView drop
listView.Drop += (s, e) =>
{
    if (e.Data.GetDataPresent("Records"))
    {
        var records = e.Data.GetData("Records") as ObservableCollection<object>;
        var selectedCollection = listView.ItemsSource as ObservableCollection<OrderInfo>;
        
        foreach (var record in records.Cast<OrderInfo>())
        {
            selectedCollection.Add(record);
        }
    }
};

// Configure DataGrid to provide data
dataGrid.RowDragDropController.DragStart += (s, e) =>
{
    e.Data.SetData("Records", e.DraggingRecords);
};
```

### Drag-Drop Between DataGrid and TreeGrid

Enable drag-drop between DataGrid and TreeGrid:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDraggingRows="True"
                       ItemsSource="{Binding Orders}" />

<syncfusion:SfTreeGrid x:Name="treeGrid"
                       AllowDrop="True"
                       ItemsSource="{Binding Categories}" />
```

```csharp
treeGrid.AllowDrop = true;

dataGrid.RowDragDropController.Dropped += (s, e) =>
{
    if (e.IsFromOutsideSource)
    {
        // Add dragged items as children in tree
        var parentNode = treeGrid.GetNodeAtRowIndex(e.DropIndex);
        foreach (var record in e.DraggingRecords)
        {
            // Add logic to insert into tree hierarchy
        }
    }
};
```

## Reordering Source Collection

Update the underlying collection when rows are reordered:

```csharp
dataGrid.RowDragDropController.Dropped += (s, e) =>
{
    if (!e.IsFromOutsideSource)
    {
        var collection = dataGrid.ItemsSource as ObservableCollection<OrderInfo>;
        
        // Get target index
        int targetIndex = collection.IndexOf(e.TargetRecord as OrderInfo);
        
        // Remove and reinsert records
        foreach (var record in e.DraggingRecords.Cast<OrderInfo>().ToList())
        {
            int oldIndex = collection.IndexOf(record);
            collection.RemoveAt(oldIndex);
            
            if (oldIndex < targetIndex)
                targetIndex--;
                
            if (e.DropPosition == DropPosition.DropAbove)
                collection.Insert(targetIndex, record);
            else
                collection.Insert(targetIndex + 1, record);
                
            targetIndex++;
        }
    }
};
```

## Advanced Scenarios

### Drag-Drop with Grouping

Handle drag-drop when grouping is enabled:

```csharp
dataGrid.RowDragDropController.Dropped += (s, e) =>
{
    if (dataGrid.View.GroupDescriptions.Count > 0)
    {
        // Handle group-aware reordering
        var targetGroup = e.TargetRecord as Group;
        if (targetGroup != null)
        {
            // Add records to target group
            foreach (var record in e.DraggingRecords)
            {
                UpdateGroupProperty(record, targetGroup.Key);
            }
        }
    }
};
```

### Drag-Drop with Master-Details

Enable drag-drop in nested grids:

```xaml
<syncfusion:SfDataGrid.DetailsViewDefinition>
    <syncfusion:GridViewDefinition RelationalColumn="OrderDetails">
        <syncfusion:GridViewDefinition.DataGrid>
            <syncfusion:SfDataGrid x:Name="detailsGrid"
                                   AllowDraggingRows="True"
                                   AllowDrop="True" />
        </syncfusion:GridViewDefinition.DataGrid>
    </syncfusion:GridViewDefinition>
</syncfusion:SfDataGrid.DetailsViewDefinition>
```

### Custom Drop Logic

Implement custom drop behavior:

```csharp
dataGrid.RowDragDropController.Drop += (s, e) =>
{
    e.Handled = true;  // Prevent default behavior
    
    var draggingOrders = e.DraggingRecords.Cast<OrderInfo>().ToList();
    var targetOrder = e.TargetRecord as OrderInfo;
    
    // Implement custom logic
    if (e.DropPosition == DropPosition.DropAbove)
    {
        InsertOrdersAbove(draggingOrders, targetOrder);
    }
    else if (e.DropPosition == DropPosition.DropBelow)
    {
        InsertOrdersBelow(draggingOrders, targetOrder);
    }
    else if (e.DropPosition == DropPosition.DropAsChild)
    {
        ConvertToChildOrders(draggingOrders, targetOrder);
    }
};
```

## Troubleshooting

### Issue: Drag Operation Not Working

**Solution:**
- Verify `AllowDraggingRows` is set to `true`
- Check `AllowDrop` is `true`
- Ensure ItemsSource is not null
- Verify collection supports reordering

### Issue: Drop Indicator Not Showing

**Solution:**
- Check `ShowDragUI` property is not `false`
- Verify `RowDropIndicatorMode` is set appropriately
- Check if DragOver event is setting `ShowDragUI` to `false`

### Issue: Records Not Reordered in Collection

**Solution:**
- Manually reorder collection in Dropped event
- Use ObservableCollection for ItemsSource
- Implement proper indexing logic

### Issue: Cannot Drag Between Controls

**Solution:**
- Set `AllowDrop="True"` on target control
- Handle Drop event on target control
- Pass data correctly via DataObject

### Issue: Drag-Drop Conflicts with Selection

**Solution:**
- Set appropriate `SelectionMode`
- Handle DragStart to manage selection
- Clear selection after drop if needed

## Edge Cases

### Dragging Single Item from Multi-Selection

```csharp
dataGrid.RowDragDropController.DragStart += (s, e) =>
{
    // Check if dragging only one item from multiple selection
    if (dataGrid.SelectedItems.Count > 1 && e.DraggingRecords.Count == 1)
    {
        // Optionally clear other selections or drag all selected
        dataGrid.SelectedItems.Clear();
        dataGrid.SelectedItem = e.DraggingRecords[0];
    }
};
```

### Preventing Self-Drop

```csharp
dataGrid.RowDragDropController.Drop += (s, e) =>
{
    // Prevent dropping on itself
    if (e.DraggingRecords.Contains(e.TargetRecord))
    {
        e.Handled = true;
    }
};
```

### Drag-Drop with Frozen Columns

Drag-drop works normally with frozen columns, but visual feedback may be affected. Handle positioning in custom drag template if needed.

### Handling Undo/Redo for Drag-Drop

```csharp
private Stack<DragDropOperation> undoStack = new Stack<DragDropOperation>();

dataGrid.RowDragDropController.Dropped += (s, e) =>
{
    // Store operation for undo
    var operation = new DragDropOperation
    {
        DraggedRecords = e.DraggingRecords.ToList(),
        TargetRecord = e.TargetRecord,
        DropPosition = e.DropPosition,
        SourceIndex = GetRecordIndex(e.DraggingRecords.First())
    };
    
    undoStack.Push(operation);
};

void UndoDragDrop()
{
    if (undoStack.Count > 0)
    {
        var operation = undoStack.Pop();
        // Restore original positions
    }
}
```
