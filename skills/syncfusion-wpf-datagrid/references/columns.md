# Columns in WPF DataGrid (SfDataGrid)

Comprehensive reference for defining, manipulating, and customizing columns in the Syncfusion WPF DataGrid control.

## Table of Contents
- [Column Types](#column-types)
- [Defining Columns](#defining-columns)
- [Auto-Generated Columns](#auto-generated-columns)
- [Manually Defining Columns](#manually-defining-columns)
- [Column Manipulation](#column-manipulation)
- [Column Properties](#column-properties)
- [Column Resizing](#column-resizing)
- [Column Drag and Drop](#column-drag-and-drop)
- [Freezing Columns](#freezing-columns)
- [MVVM Support](#mvvm-support)
- [Troubleshooting](#troubleshooting)

## Column Types

SfDataGrid provides built-in column types for different data scenarios:

### GridTextColumn
Displays string data. Most commonly used column type.

```xaml
<syncfusion:GridTextColumn HeaderText="Order ID" MappingName="OrderID" />
```

```csharp
dataGrid.Columns.Add(new GridTextColumn() 
{ 
    HeaderText = "Order ID", 
    MappingName = "OrderID" 
});
```

### GridNumericColumn
Displays numeric data (int, float, double, decimal).

```xaml
<syncfusion:GridNumericColumn HeaderText="Unit Price" MappingName="UnitPrice" />
```

### GridDateTimeColumn
Displays DateTime values with customizable formatting.

```xaml
<syncfusion:GridDateTimeColumn HeaderText="Order Date" 
                               MappingName="OrderDate"
                               FormatString="d" />
```

### GridCheckBoxColumn
Displays boolean values as checkboxes.

```xaml
<syncfusion:GridCheckBoxColumn HeaderText="Is Shipped" MappingName="IsShipped" />
```

### GridComboBoxColumn
Displays dropdown list for selecting from predefined values.

```xaml
<syncfusion:GridComboBoxColumn HeaderText="Country" 
                               MappingName="Country"
                               ItemsSource="{Binding Countries}"
                               DisplayMemberPath="Name"
                               SelectedValuePath="Code" />
```

### GridHyperlinkColumn
Displays clickable URI links.

```xaml
<syncfusion:GridHyperlinkColumn HeaderText="Website" MappingName="WebsiteUrl" />
```

### GridTemplateColumn
Allows custom content using DataTemplates.

```xaml
<syncfusion:GridTemplateColumn HeaderText="Custom" MappingName="CustomData">
    <syncfusion:GridTemplateColumn.CellTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="{Binding ImagePath}" Width="20" Height="20" />
                <TextBlock Text="{Binding Name}" Margin="5,0,0,0" />
            </StackPanel>
        </DataTemplate>
    </syncfusion:GridTemplateColumn.CellTemplate>
</syncfusion:GridTemplateColumn>
```

### Additional Column Types
- **GridCurrencyColumn**: For currency values
- **GridPercentColumn**: For percentage values
- **GridMaskColumn**: For masked input
- **GridTimeSpanColumn**: For time span values
- **GridImageColumn**: For displaying images
- **GridUnBoundColumn**: For calculated or virtual columns
- **GridMultiColumnDropDownList**: For multi-column dropdown selection

## Defining Columns

### Auto-Generated Columns

Enable automatic column generation based on data object properties:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AutoGenerateColumns = true;
```

**Auto-Generation Modes:**

```csharp
// AutoGenerateColumnsMode options:
dataGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.Reset;       // Default
// dataGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.RetainOld;  // Retain on ItemsSource change
// dataGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.ResetAll;   // Clear all columns
// dataGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.None;       // No auto-generation
```

**Column Type Mapping:**

| Data Type | Generated Column Type |
|-----------|----------------------|
| string, object, dynamic | GridTextColumn |
| int, float, double, decimal (nullable) | GridNumericColumn |
| DateTime, DateTimeOffset (nullable) | GridDateTimeColumn |
| Uri, Uri? | GridHyperlinkColumn |
| bool, bool? | GridCheckBoxColumn |
| TimeSpan, TimeSpan? | GridTimeSpanColumn |

### Customizing Auto-Generated Columns

Use the `AutoGeneratingColumn` event to customize or cancel column generation:

```csharp
dataGrid.AutoGeneratingColumn += DataGrid_AutoGeneratingColumn;

void DataGrid_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    // Cancel column generation for specific property
    if (e.Column.MappingName == "InternalID")
    {
        e.Cancel = true;
        return;
    }
    
    // Change column type
    if (e.Column.MappingName == "UnitPrice" && e.Column is GridNumericColumn)
    {
        e.Column = new GridCurrencyColumn() 
        { 
            MappingName = "UnitPrice", 
            HeaderText = "Unit Price" 
        };
    }
    
    // Customize column properties
    if (e.Column.MappingName == "OrderID")
    {
        e.Column.AllowEditing = false;
        e.Column.AllowSorting = true;
        e.Column.AllowFiltering = true;
        e.Column.Width = 120;
    }
    
    // Set custom header template
    if (e.Column.MappingName == "OrderID")
    {
        e.Column.HeaderTemplate = this.Resources["CustomHeaderTemplate"] as DataTemplate;
    }
}
```

### Auto-Generating Complex Type Columns

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumnsForCustomType="True"
                       AutoGenerateColumnsModeForCustomType="Parent"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AutoGenerateColumnsForCustomType = true;
dataGrid.AutoGenerateColumnsModeForCustomType = AutoGenerateColumnsModeForCustomType.Parent;
```

**Modes:**
- **Both**: Auto-generate columns for custom type and inner properties
- **Child**: Auto-generate columns for inner properties only
- **Parent**: Auto-generate column for custom type only

### Data Annotations Support

```csharp
public class OrderInfo
{
    [Display(Order = 0)]
    public int OrderID { get; set; }
    
    [Display(AutoGenerateField = false)]
    public int InternalID { get; set; }
    
    [Display(Name = "Customer Name", AutoGenerateFilter = true)]
    public string CustomerName { get; set; }
    
    [ReadOnly(true)]
    public string Status { get; set; }
    
    [DataType(DataType.Currency)]
    public double UnitPrice { get; set; }
    
    [DisplayFormat(DataFormatString = "yyyy")]
    public DateTime OrderDate { get; set; }
    
    [Editable(true)]
    public string Country { get; set; }
}
```

## Manually Defining Columns

Disable auto-generation and define columns explicitly:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="False"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn HeaderText="Order ID" MappingName="OrderID" Width="100" />
        <syncfusion:GridTextColumn HeaderText="Customer ID" MappingName="CustomerID" Width="120" />
        <syncfusion:GridTextColumn HeaderText="Customer Name" MappingName="CustomerName" Width="150" />
        <syncfusion:GridDateTimeColumn HeaderText="Order Date" MappingName="OrderDate" Width="130" />
        <syncfusion:GridNumericColumn HeaderText="Unit Price" MappingName="UnitPrice" Width="110" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AutoGenerateColumns = false;
dataGrid.Columns.Add(new GridTextColumn() { HeaderText = "Order ID", MappingName = "OrderID", Width = 100 });
dataGrid.Columns.Add(new GridTextColumn() { HeaderText = "Customer ID", MappingName = "CustomerID", Width = 120 });
dataGrid.Columns.Add(new GridTextColumn() { HeaderText = "Customer Name", MappingName = "CustomerName", Width = 150 });
dataGrid.Columns.Add(new GridDateTimeColumn() { HeaderText = "Order Date", MappingName = "OrderDate", Width = 130 });
dataGrid.Columns.Add(new GridNumericColumn() { HeaderText = "Unit Price", MappingName = "UnitPrice", Width = 110 });
```

## Column Manipulation

### Adding Columns

```csharp
// Add at runtime
dataGrid.Columns.Add(new GridTextColumn() 
{ 
    HeaderText = "New Column", 
    MappingName = "NewProperty" 
});
```

### Accessing Columns

```csharp
// By index
GridColumn column = dataGrid.Columns[1];

// By MappingName
GridColumn column = dataGrid.Columns["OrderID"];
```

### Removing Columns

```csharp
// Remove specific column
var column = dataGrid.Columns["OrderID"];
dataGrid.Columns.Remove(column);

// Remove by index
dataGrid.Columns.RemoveAt(1);

// Clear all columns
dataGrid.Columns.Clear();
```

### Removing Columns from Stacked Headers

```csharp
var childColumns = dataGrid.StackedHeaderRows[0].StackedColumns[0].ChildColumns.Split(',');

foreach (var name in childColumns)
{
    var column = dataGrid.Columns[name];
    if (column != null)
        dataGrid.Columns.Remove(column);
}
```

## Column Properties

### Common Properties

```xaml
<syncfusion:GridTextColumn HeaderText="Order ID"
                          MappingName="OrderID"
                          Width="120"
                          MinimumWidth="80"
                          MaximumWidth="200"
                          AllowEditing="False"
                          AllowSorting="True"
                          AllowFiltering="True"
                          AllowGrouping="True"
                          AllowFocus="True"
                          AllowResizing="True"
                          AllowDragging="True"
                          IsHidden="False"
                          TextAlignment="Left"
                          HeaderTextAlignment="Center"
                          VerticalAlignment="Center" />
```

### Width Management

```csharp
// Fixed width
column.Width = 120;

// Auto-size
column.ColumnSizer = GridLengthUnitType.Auto;

// Star sizing
column.ColumnSizer = GridLengthUnitType.Star;

// Min and Max width
column.MinimumWidth = 80;
column.MaximumWidth = 200;
```

### Display Formatting

```csharp
// For GridNumericColumn
var numericColumn = new GridNumericColumn()
{
    MappingName = "UnitPrice",
    HeaderText = "Unit Price",
    FormatString = "C",  // Currency format
    NumberDecimalDigits = 2
};

// For GridDateTimeColumn
var dateColumn = new GridDateTimeColumn()
{
    MappingName = "OrderDate",
    HeaderText = "Order Date",
    FormatString = "d"  // Short date format
};
```

## Column Resizing

### Enable Resizing

```xaml
<!-- Enable for all columns -->
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowResizingColumns="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowResizingColumns = true;

// For specific column
dataGrid.Columns["OrderID"].AllowResizing = true;
```

### Hidden Column Resizing

Enable resizing for hidden columns:

```csharp
dataGrid.AllowResizingHiddenColumns = true;
```

### Disable Resizing for Specific Column

```csharp
dataGrid.Columns["OrderID"].AllowResizing = false;
```

### Resizing Events

```csharp
dataGrid.ResizingColumns += DataGrid_ResizingColumns;

void DataGrid_ResizingColumns(object sender, ResizingColumnsEventArgs e)
{
    // Cancel resizing for specific column
    if (e.ColumnIndex == 1)
    {
        e.Cancel = true;
        return;
    }
    
    // Get final width after resizing
    if (e.Reason == ColumnResizingReason.Resized)
    {
        var resizedWidth = e.Width;
        // Perform action with resized width
    }
}
```

## Column Drag and Drop

### Enable Drag and Drop

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AllowDraggingColumns="True"
                       ItemsSource="{Binding Orders}" />
```

```csharp
dataGrid.AllowDraggingColumns = true;

// Disable for specific column
dataGrid.Columns["OrderID"].AllowDragging = false;
```

### Disable Column Reordering

```csharp
dataGrid.QueryColumnDragging += DataGrid_QueryColumnDragging;

void DataGrid_QueryColumnDragging(object sender, QueryColumnDraggingEventArgs e)
{
    var column = dataGrid.Columns[e.From];
    
    // Prevent specific column from being dragged
    if (column.MappingName == "CustomerName" && e.Reason == QueryColumnDraggingReason.Dropping)
    {
        e.Cancel = true;
    }
}
```

### Disable Drag Between Frozen and Non-Frozen Columns

```csharp
dataGrid.QueryColumnDragging += DataGrid_QueryColumnDragging;

void DataGrid_QueryColumnDragging(object sender, QueryColumnDraggingEventArgs e)
{
    if (e.Reason == QueryColumnDraggingReason.Dropping)
    {
        var frozenColIndex = dataGrid.FrozenColumnCount + dataGrid.ResolveToStartColumnIndex();
        
        // Prevent drag from frozen to non-frozen
        if (e.From < frozenColIndex && e.To > frozenColIndex - 1)
            e.Cancel = true;
        
        // Prevent drag from non-frozen to frozen
        if (e.From > frozenColIndex && e.To < frozenColIndex || 
           (e.From == frozenColIndex && e.To < frozenColIndex))
            e.Cancel = true;
    }
}
```

### Custom Drag and Drop Controller

```csharp
dataGrid.GridColumnDragDropController = new CustomDragDropController(dataGrid);

public class CustomDragDropController : GridColumnDragDropController
{
    public CustomDragDropController(SfDataGrid dataGrid) : base(dataGrid)
    {
    }
    
    public override bool CanShowPopup(GridColumn column)
    {
        // Custom logic for showing popup
        return base.CanShowPopup(column);
    }
    
    protected override void PopupContentDroppedOnHeaderRow(int oldIndex, int newColumnIndex)
    {
        // Custom logic for drop on header
        base.PopupContentDroppedOnHeaderRow(oldIndex, newColumnIndex);
    }
}
```

## Freezing Columns

Freeze columns at the left or right side:

```xaml
<!-- Freeze left columns -->
<syncfusion:SfDataGrid x:Name="dataGrid"
                       FrozenColumnCount="2"
                       ItemsSource="{Binding Orders}" />

<!-- Freeze right columns -->
<syncfusion:SfDataGrid x:Name="dataGrid"
                       FooterColumnCount="2"
                       ItemsSource="{Binding Orders}" />
```

```csharp
// Freeze first 2 columns from left
dataGrid.FrozenColumnCount = 2;

// Freeze last 2 columns from right
dataGrid.FooterColumnCount = 2;
```

**Limitations:**
- FrozenColumnCount and FooterColumnCount must be less than the number of visible columns
- Cannot freeze specific columns, only from left or right
- Frozen and footer column counts are maintained when scrolling

## MVVM Support

Bind column properties to ViewModel:

```csharp
// ViewModel
public class ViewModel : INotifyPropertyChanged
{
    private bool _allowFiltering = true;
    
    public bool AllowFiltering
    {
        get { return _allowFiltering; }
        set
        {
            _allowFiltering = value;
            OnPropertyChanged(nameof(AllowFiltering));
        }
    }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```xaml
<Window.DataContext>
    <local:ViewModel />
</Window.DataContext>

<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Orders}"
                       AutoGenerateColumns="False">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" 
                                   AllowFiltering="{Binding AllowFiltering}"
                                   AllowSorting="{Binding AllowSorting}"
                                   AllowEditing="{Binding AllowEditing}" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

## Troubleshooting

### Column Not Displaying Data

**Issue**: Column shows but no data appears

**Solutions**:
1. Verify `MappingName` matches property name exactly (case-sensitive)
2. Ensure property has public getter
3. Check if data type matches column type
4. Verify ItemsSource is properly bound

```csharp
// Correct
<syncfusion:GridTextColumn MappingName="OrderID" />  // Property: public int OrderID { get; set; }

// Incorrect
<syncfusion:GridTextColumn MappingName="orderId" />  // Case mismatch
```

### Column Width Not Working

**Issue**: Setting Width doesn't take effect

**Solutions**:
1. Check if ColumnSizer is set (it overrides Width)
2. Verify MinimumWidth and MaximumWidth constraints
3. Ensure Width is not being set programmatically elsewhere

```csharp
// This won't work if ColumnSizer is set
column.Width = 120;
column.ColumnSizer = GridLengthUnitType.Auto;  // Auto overrides Width

// Solution: Remove ColumnSizer or set to None
column.ColumnSizer = GridLengthUnitType.None;
column.Width = 120;
```

### Column Resizing Not Working

**Issue**: Cannot resize column by dragging

**Solutions**:
1. Ensure `AllowResizingColumns` is true on SfDataGrid
2. Check if column's `AllowResizing` is not set to false
3. Verify Width is not set with ColumnSizer (may limit resizing)

```csharp
// Enable resizing
dataGrid.AllowResizingColumns = true;
column.AllowResizing = true;  // Should not be false
```

### Column Dragging Not Working

**Issue**: Cannot reorder columns

**Solutions**:
1. Set `AllowDraggingColumns` to true on SfDataGrid
2. Check column's `AllowDragging` property
3. Verify `QueryColumnDragging` event isn't canceling the operation

### Auto-Generated Columns Missing

**Issue**: Columns not auto-generating

**Solutions**:
1. Ensure `AutoGenerateColumns` is true
2. Verify ItemsSource is bound and contains data
3. Check if properties have public getters
4. Ensure `AutoGenerateColumnsMode` is not set to None

```csharp
// Verify settings
dataGrid.AutoGenerateColumns = true;
dataGrid.AutoGenerateColumnsMode = AutoGenerateColumnsMode.Reset;
```

### Column Filtering/Sorting Not Working

**Issue**: Column doesn't respond to filter/sort operations

**Solutions**:
1. Enable `AllowFiltering`/`AllowSorting` on SfDataGrid
2. Check column's `AllowFiltering`/`AllowSorting` properties
3. Verify column has valid `MappingName`

### Memory Issues with Many Columns

**Issue**: Performance degradation with large number of columns

**Solutions**:
1. Use virtualization (enabled by default)
2. Disable columns not currently needed
3. Use `IsHidden` instead of removing/adding columns
4. Consider using column groups or master-detail view

### Column Template Not Updating

**Issue**: Custom templates don't reflect data changes

**Solutions**:
1. Ensure ViewModel implements INotifyPropertyChanged
2. Verify binding mode is TwoWay when needed
3. Check if UpdateSourceTrigger is appropriate
4. Use ObservableCollection for dynamic data

```xaml
<syncfusion:GridTemplateColumn.CellTemplate>
    <DataTemplate>
        <TextBlock Text="{Binding PropertyName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
    </DataTemplate>
</syncfusion:GridTemplateColumn.CellTemplate>
```
