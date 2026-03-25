# WPF DataGrid Data Manipulation

## Overview

The Syncfusion WPF DataGrid (SfDataGrid) provides comprehensive data manipulation capabilities including adding, updating, and deleting records. This guide covers CRUD operations, LiveDataUpdateMode, AddNewRow functionality, and programmatic data operations.

## LiveDataUpdateMode

Control how the DataGrid responds to data changes using the `LiveDataUpdateMode` property.

### Available Modes

```csharp
public enum LiveDataUpdateMode
{
    Default,                    // Updates UI only when collection changes
    AllowSummaryUpdate,         // Updates summaries on property changes
    AllowDataShaping           // Updates grouping, filtering, sorting on property changes
}
```

### Default Mode

Updates UI only when items are added or removed from the collection:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       LiveDataUpdateMode="Default"
                       ItemsSource="{Binding Orders}" />
```

### AllowSummaryUpdate Mode

Updates summary values when underlying data properties change:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       LiveDataUpdateMode="AllowSummaryUpdate"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.TableSummaryRows>
        <syncfusion:GridTableSummaryRow ShowSummaryInRow="False">
            <syncfusion:GridSummaryRow.SummaryColumns>
                <syncfusion:GridSummaryColumn Name="TotalPrice"
                                              Format="'{Sum:c}'"
                                              MappingName="TotalPrice"
                                              SummaryType="DoubleAggregate" />
            </syncfusion:GridSummaryRow.SummaryColumns>
        </syncfusion:GridTableSummaryRow>
    </syncfusion:SfDataGrid.TableSummaryRows>
</syncfusion:SfDataGrid>
```

### AllowDataShaping Mode

Updates grouping, filtering, and sorting when data changes:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       LiveDataUpdateMode="AllowDataShaping"
                       ItemsSource="{Binding Orders}" />
```

```csharp
// Combine multiple modes
dataGrid.LiveDataUpdateMode = LiveDataUpdateMode.AllowSummaryUpdate | 
                               LiveDataUpdateMode.AllowDataShaping;
```

## AddNewRow Feature

### Enabling AddNewRow

Add a row for entering new records at top or bottom:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AddNewRowPosition="Top"
                       ItemsSource="{Binding Orders}" />
```

```csharp
// Add at bottom
dataGrid.AddNewRowPosition = AddNewRowPosition.Bottom;

// Add at top
dataGrid.AddNewRowPosition = AddNewRowPosition.Top;

// Disable AddNewRow
dataGrid.AddNewRowPosition = AddNewRowPosition.None;
```

### Customizing AddNewRow Text

Change the watermark text displayed in AddNewRow:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AddNewRowPosition="Top"
                       AddNewRowText="Click here to add new order..."
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AddNewRowText = "Enter new record here...";
```

### AddNewRowInitiating Event

Set default values or validate before adding:

```csharp
dataGrid.AddNewRowInitiating += DataGrid_AddNewRowInitiating;

void DataGrid_AddNewRowInitiating(object sender, AddNewRowInitiatingEventArgs e)
{
    var newOrder = e.NewObject as OrderInfo;
    
    // Set default values
    newOrder.OrderDate = DateTime.Now;
    newOrder.Status = "Pending";
    newOrder.OrderID = GenerateOrderID();
}
```

### NewItemPlaceholderPosition

Control the position of new item placeholder in grouped view:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       NewItemPlaceholderPosition="AtGroup"
                       ItemsSource="{Binding Orders}" />
```

```csharp
// Place at group
dataGrid.NewItemPlaceholderPosition = NewItemPlaceholderPosition.AtGroup;

// Place at bottom
dataGrid.NewItemPlaceholderPosition = NewItemPlaceholderPosition.AtBottom;

// Hide placeholder
dataGrid.NewItemPlaceholderPosition = NewItemPlaceholderPosition.None;
```

## Adding Rows Programmatically

### Using View.AddNew

Add new records programmatically:

```csharp
// Add new row using View
var newOrder = new OrderInfo
{
    OrderID = 1001,
    CustomerName = "John Doe",
    TotalPrice = 500.00,
    OrderDate = DateTime.Now
};

dataGrid.View.AddNew(newOrder);
```

### CommitAddNew Method

Commit the new row addition:

```csharp
// Start adding new row
dataGrid.View.AddNew();

// ... User enters data ...

// Commit the new row
dataGrid.View.CommitAddNew();
```

### CancelAddNew Method

Cancel adding new row:

```csharp
// Cancel the add operation
dataGrid.View.CancelAddNew();
```

### Adding with Complex Properties

Handle complex property initialization:

```csharp
dataGrid.AddNewRowInitiating += (s, e) =>
{
    var newOrder = e.NewObject as OrderInfo;
    
    // Initialize complex property
    newOrder.Customer = new CustomerInfo
    {
        CustomerID = 0,
        Name = string.Empty
    };
    
    // Initialize collection
    newOrder.OrderDetails = new ObservableCollection<OrderDetail>();
};
```

## Deleting Rows

### Enabling Row Deletion

Allow users to delete rows using Delete key:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDeleting="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowDeleting = true;
```

### RecordDeleting Event

Validate or cancel deletion before it occurs:

```csharp
dataGrid.RecordDeleting += DataGrid_RecordDeleting;

void DataGrid_RecordDeleting(object sender, RecordDeletingEventArgs e)
{
    var orders = e.Items.Cast<OrderInfo>();
    
    // Cancel deletion for shipped orders
    foreach (var order in orders)
    {
        if (order.IsShipped)
        {
            e.Cancel = true;
            MessageBox.Show("Cannot delete shipped orders");
            return;
        }
    }
}
```

### RecordDeleted Event

Perform actions after records are deleted:

```csharp
dataGrid.RecordDeleted += DataGrid_RecordDeleted;

void DataGrid_RecordDeleted(object sender, RecordDeletedEventArgs e)
{
    var deletedOrders = e.Items.Cast<OrderInfo>();
    
    // Log deletion
    foreach (var order in deletedOrders)
    {
        LogDeletion(order.OrderID);
    }
}
```

### Programmatic Deletion

Delete records programmatically:

```csharp
// Delete specific record
var orderToDelete = dataGrid.SelectedItem as OrderInfo;
if (orderToDelete != null)
{
    var collection = dataGrid.ItemsSource as ObservableCollection<OrderInfo>;
    collection.Remove(orderToDelete);
}

// Delete by index
if (dataGrid.ItemsSource is ObservableCollection<OrderInfo> orders)
{
    orders.RemoveAt(0);
}

// Delete multiple records
var selectedItems = dataGrid.SelectedItems.Cast<OrderInfo>().ToList();
foreach (var item in selectedItems)
{
    (dataGrid.ItemsSource as ObservableCollection<OrderInfo>).Remove(item);
}
```

### Deleting Cell Values in Display Mode

Clear cell values without entering edit mode:

```csharp
protected override void ProcessKeyDown(KeyEventArgs e)
{
    if (e.Key == Key.Delete && dataGrid.SelectionController.CurrentCellManager.CurrentCell != null)
    {
        var currentCell = dataGrid.SelectionController.CurrentCellManager.CurrentCell;
        var record = dataGrid.GetRecordAtRowIndex(currentCell.RowIndex);
        
        if (record != null)
        {
            var property = record.GetType().GetProperty(currentCell.GridColumn.MappingName);
            if (property != null && property.CanWrite)
            {
                property.SetValue(record, null);
            }
        }
        
        e.Handled = true;
        return;
    }
    
    base.ProcessKeyDown(e);
}
```

## Updating Records

### Direct Property Update

Update properties directly when INotifyPropertyChanged is implemented:

```csharp
var order = dataGrid.SelectedItem as OrderInfo;
if (order != null)
{
    order.TotalPrice = 750.00;  // Automatically updates UI
    order.Status = "Completed";
}
```

### Programmatic Cell Value Update

Update specific cell values:

```csharp
// Get record at row index
int rowIndex = 3;
var record = dataGrid.GetRecordAtRowIndex(rowIndex);

if (record != null)
{
    var property = record.GetType().GetProperty("TotalPrice");
    property?.SetValue(record, 1000.00);
}
```

### Batch Updates

Update multiple records efficiently:

```csharp
// Suspend notifications
dataGrid.View.BeginInit();

try
{
    var orders = dataGrid.ItemsSource as ObservableCollection<OrderInfo>;
    foreach (var order in orders.Where(o => o.Status == "Pending"))
    {
        order.Status = "Processing";
        order.LastModified = DateTime.Now;
    }
}
finally
{
    // Resume notifications
    dataGrid.View.EndInit();
}
```

## RefreshView Methods

### Refresh Entire View

Refresh all data in the grid:

```csharp
// Refresh complete view
dataGrid.View.Refresh();
```

### Refresh Specific Row

Refresh a single data row:

```csharp
// Refresh row at index
int rowIndex = 5;
dataGrid.UpdateDataRow(rowIndex);

// Refresh specific record
var order = dataGrid.SelectedItem as OrderInfo;
var recordIndex = dataGrid.View.Records.IndexOfRecord(order);
if (recordIndex >= 0)
{
    dataGrid.UpdateDataRow(dataGrid.ResolveToRowIndex(recordIndex));
}
```

## Data Validation

### IDataErrorInfo Implementation

Implement validation in your data model:

```csharp
public class OrderInfo : INotifyPropertyChanged, IDataErrorInfo
{
    private double totalPrice;
    
    public double TotalPrice
    {
        get { return totalPrice; }
        set
        {
            totalPrice = value;
            OnPropertyChanged("TotalPrice");
        }
    }
    
    public string Error
    {
        get { return null; }
    }
    
    public string this[string columnName]
    {
        get
        {
            if (columnName == "TotalPrice")
            {
                if (TotalPrice < 0)
                    return "Total price cannot be negative";
                if (TotalPrice > 10000)
                    return "Total price exceeds maximum limit";
            }
            return null;
        }
    }
}
```

### INotifyDataErrorInfo Implementation

For asynchronous validation:

```csharp
public class OrderInfo : INotifyPropertyChanged, INotifyDataErrorInfo
{
    private Dictionary<string, List<string>> errors = new Dictionary<string, List<string>>();
    
    public event EventHandler<DataErrorsChangedEventArgs> ErrorsChanged;
    
    public bool HasErrors => errors.Count > 0;
    
    public IEnumerable GetErrors(string propertyName)
    {
        if (string.IsNullOrEmpty(propertyName))
            return errors.Values.SelectMany(x => x);
            
        if (errors.ContainsKey(propertyName))
            return errors[propertyName];
            
        return null;
    }
    
    private void ValidateProperty(string propertyName, object value)
    {
        ClearErrors(propertyName);
        
        if (propertyName == "TotalPrice")
        {
            double price = (double)value;
            if (price < 0)
                AddError(propertyName, "Total price cannot be negative");
        }
    }
    
    private void AddError(string propertyName, string error)
    {
        if (!errors.ContainsKey(propertyName))
            errors[propertyName] = new List<string>();
            
        errors[propertyName].Add(error);
        OnErrorsChanged(propertyName);
    }
    
    private void ClearErrors(string propertyName)
    {
        if (errors.ContainsKey(propertyName))
        {
            errors.Remove(propertyName);
            OnErrorsChanged(propertyName);
        }
    }
    
    private void OnErrorsChanged(string propertyName)
    {
        ErrorsChanged?.Invoke(this, new DataErrorsChangedEventArgs(propertyName));
    }
}
```

## Master-Details View Support

### AddNewRow in Master-Details

Enable AddNewRow for nested grids:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="OrderDetails">
            <syncfusion:GridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid x:Name="detailsGrid"
                                       AddNewRowPosition="Top"
                                       AllowDeleting="True" />
            </syncfusion:GridViewDefinition.DataGrid>
        </syncfusion:GridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

## Troubleshooting

### Issue: AddNewRow Not Visible

**Solution:**
- Check `AddNewRowPosition` is not `None`
- Verify ItemsSource is not null
- Ensure collection supports adding (not read-only)

### Issue: New Records Not Added to Collection

**Solution:**
- Verify ItemsSource is ObservableCollection or implements INotifyCollectionChanged
- Check if AddNewRowInitiating event is canceling
- Ensure CommitAddNew is called

### Issue: Deleted Records Reappear

**Solution:**
- Use ObservableCollection for ItemsSource
- Ensure proper two-way binding
- Check if data source is being reset

### Issue: Updates Not Reflected in UI

**Solution:**
- Implement INotifyPropertyChanged in data model
- Call View.Refresh() if needed
- Check LiveDataUpdateMode setting

### Issue: Validation Errors Not Showing

**Solution:**
- Implement IDataErrorInfo or INotifyDataErrorInfo
- Check ValidationMode property
- Verify error template is defined

## Edge Cases

### Handling Null Values

```csharp
dataGrid.AddNewRowInitiating += (s, e) =>
{
    var order = e.NewObject as OrderInfo;
    
    // Handle nullable types
    order.ShippedDate = null;  // Nullable DateTime
    order.Discount = 0.0;      // Default for non-nullable
};
```

### Complex Property Editing

```csharp
// Ensure complex properties are initialized
dataGrid.AddNewRowInitiating += (s, e) =>
{
    var order = e.NewObject as OrderInfo;
    order.Address = new Address { City = "", State = "" };
};
```

### Cascading Updates

```csharp
dataGrid.CurrentCellValueChanged += (s, e) =>
{
    var order = e.Record as OrderInfo;
    
    // Update dependent fields
    if (e.Column.MappingName == "Quantity" || e.Column.MappingName == "UnitPrice")
    {
        order.TotalPrice = order.Quantity * order.UnitPrice;
    }
};
```
