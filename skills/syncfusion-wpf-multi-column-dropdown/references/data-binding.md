# Data Binding in SfMultiColumnDropDownControl

## Table of Contents
- [Overview](#overview)
- [ItemsSource Property](#itemssource-property)
- [DisplayMember and ValueMember](#displaymember-and-valuemember)
- [Binding in XAML](#binding-in-xaml)
- [Binding in Code-Behind](#binding-in-code-behind)
- [Complex Property Binding](#complex-property-binding)
- [Indexer Property Binding](#indexer-property-binding)
- [Selection Properties](#selection-properties)
- [Data Model Best Practices](#data-model-best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Data binding in SfMultiColumnDropDownControl enables you to populate the dropdown list with data from various sources. The control uses three key properties to manage data display and selection:

- **ItemsSource:** The data collection to display
- **DisplayMember:** Property path shown in the TextBox editor
- **ValueMember:** Property path for the SelectedValue

## ItemsSource Property

The `ItemsSource` property accepts any `IEnumerable` collection and populates the dropdown grid with data.

### Supported Data Sources

- `ObservableCollection<T>` (recommended for dynamic data)
- `List<T>`
- `IList<T>`
- `BindingList<T>`
- `IEnumerable<T>`
- DataTable
- DataView

### Example Data Model

```csharp
public class OrderInfo
{
    public int OrderID { get; set; }
    public string CustomerName { get; set; }
    public string CustomerID { get; set; }
    public string Country { get; set; }
    public string ShipCity { get; set; }
}
```

## DisplayMember and ValueMember

### DisplayMember

The `DisplayMember` property specifies which property from the data object is displayed in the TextBox editor after selection.

**Usage:**
```xml
<syncfusion:SfMultiColumnDropDownControl 
    DisplayMember="CustomerName"
    ItemsSource="{Binding Orders}"/>
```

When a user selects an item, the CustomerName value appears in the editor.

### ValueMember

The `ValueMember` property specifies which property value is returned through the `SelectedValue` property.

**Usage:**
```xml
<syncfusion:SfMultiColumnDropDownControl 
    DisplayMember="CustomerName"
    ValueMember="OrderID"
    ItemsSource="{Binding Orders}"/>
```

### Display vs Value Example

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <syncfusion:SfMultiColumnDropDownControl 
        x:Name="sfMultiColumn"
        Grid.Row="0"
        Width="175"
        Height="30"
        DisplayMember="CustomerName"
        ValueMember="OrderID"
        ItemsSource="{Binding Orders}"
        SelectedIndex="0"/>
    
    <StackPanel Grid.Row="1" Margin="20">
        <TextBlock Text="Selected Item (Display Member):" FontSize="14"/>
        <TextBlock 
            Text="{Binding ElementName=sfMultiColumn, Path=SelectedItem.CustomerName}" 
            FontSize="18" 
            FontWeight="Bold"/>
        
        <TextBlock Text="Selected Value (Value Member):" FontSize="14" Margin="0,10,0,0"/>
        <TextBlock 
            Text="{Binding ElementName=sfMultiColumn, Path=SelectedValue}" 
            FontSize="18" 
            FontWeight="Bold"/>
    </StackPanel>
</Grid>
```

**Result:**
- **Editor displays:** "Maria Anders" (from DisplayMember)
- **SelectedValue returns:** 1001 (from ValueMember)

## Binding in XAML

### Basic XAML Binding

```xml
<Window xmlns:local="clr-namespace:YourNamespace">
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <syncfusion:SfMultiColumnDropDownControl 
        ItemsSource="{Binding Orders}"
        DisplayMember="CustomerName"
        ValueMember="OrderID"
        SelectedIndex="0"/>
</Window>
```

### ViewModel Setup

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<OrderInfo> _orders;
    
    public ObservableCollection<OrderInfo> Orders
    {
        get { return _orders; }
        set 
        { 
            _orders = value;
            OnPropertyChanged(nameof(Orders));
        }
    }
    
    public ViewModel()
    {
        Orders = new ObservableCollection<OrderInfo>
        {
            new OrderInfo { OrderID = 1001, CustomerName = "Maria Anders", CustomerID = "ALFKI", Country = "Germany" },
            new OrderInfo { OrderID = 1002, CustomerName = "Ana Trujilo", CustomerID = "ANATR", Country = "Mexico" },
            new OrderInfo { OrderID = 1003, CustomerName = "Thomas Hardy", CustomerID = "AROUT", Country = "UK" }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Binding in Code-Behind

### Direct Binding

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Create data collection
        var orders = new ObservableCollection<OrderInfo>
        {
            new OrderInfo { OrderID = 1001, CustomerName = "Maria Anders", CustomerID = "ALFKI", Country = "Germany" },
            new OrderInfo { OrderID = 1002, CustomerName = "Ana Trujilo", CustomerID = "ANATR", Country = "Mexico" }
        };
        
        // Bind to control
        sfMultiColumn.ItemsSource = orders;
        sfMultiColumn.DisplayMember = "CustomerName";
        sfMultiColumn.ValueMember = "OrderID";
        sfMultiColumn.SelectedIndex = 0;
    }
}
```

### Binding with ViewModel

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Set DataContext
    ViewModel viewModel = new ViewModel();
    this.DataContext = viewModel;
    
    // Control binds automatically if XAML has {Binding Orders}
    // Or bind in code:
    sfMultiColumn.ItemsSource = viewModel.Orders;
}
```

## Complex Property Binding

You can bind to nested properties using dot notation.

### Complex Data Model

```csharp
public class OrderInfo
{
    public int OrderID { get; set; }
    public string City { get; set; }
    public string Country { get; set; }
    public CustomerDetails CustomerDetails { get; set; }
}

public class CustomerDetails
{
    public string Name { get; set; }
    public string Designation { get; set; }
    public string Email { get; set; }
}
```

### Binding to Complex Properties

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerDetails.Name"
    ValueMember="OrderID"
    AutoGenerateColumns="False">
    
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" HeaderText="Order ID"/>
        <syncfusion:GridTextColumn MappingName="CustomerDetails.Name" HeaderText="Customer"/>
        <syncfusion:GridTextColumn MappingName="CustomerDetails.Designation" HeaderText="Designation"/>
        <syncfusion:GridTextColumn MappingName="City"/>
        <syncfusion:GridTextColumn MappingName="Country"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

### Code-Behind Complex Binding

```csharp
sfMultiColumn.DisplayMember = "CustomerDetails.Name";
sfMultiColumn.ValueMember = "CustomerDetails.Email";
```

### Populating Complex Data

```csharp
var orders = new ObservableCollection<OrderInfo>
{
    new OrderInfo 
    { 
        OrderID = 1001,
        City = "Berlin",
        Country = "Germany",
        CustomerDetails = new CustomerDetails 
        { 
            Name = "Maria Anders",
            Designation = "Sales Representative",
            Email = "maria@example.com"
        }
    }
};
```

## Indexer Property Binding

SfMultiColumnDropDownControl supports binding to indexer properties using bracket notation.

### Indexer Data Model

```csharp
public class OrderInfo
{
    public int OrderID { get; set; }
    public int ProductID { get; set; }
    public string CustomerID { get; set; }
    
    // Indexer property
    private string[] countryData = new string[3];
    
    public string this[int index]
    {
        get { return countryData[index]; }
        set { countryData[index] = value; }
    }
    
    public OrderInfo()
    {
        countryData = new string[3];
    }
}
```

### Binding to Indexer

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="Country[0]"
    ValueMember="Country[0]"
    AutoGenerateColumns="False"
    SelectedIndex="0">
    
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID"/>
        <syncfusion:GridTextColumn MappingName="ProductID"/>
        <syncfusion:GridTextColumn MappingName="CustomerID"/>
        <syncfusion:GridTextColumn MappingName="Country[0]" HeaderText="Country"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

### Populating Indexer Data

```csharp
var orders = new ObservableCollection<OrderInfo>();

var order1 = new OrderInfo 
{ 
    OrderID = 1001, 
    ProductID = 501,
    CustomerID = "ALFKI"
};
order1[0] = "Germany";

var order2 = new OrderInfo 
{ 
    OrderID = 1002, 
    ProductID = 502,
    CustomerID = "ANATR"
};
order2[0] = "Mexico";

orders.Add(order1);
orders.Add(order2);
```

## Selection Properties

Access selected data through multiple properties.

### SelectedItem

Returns the entire selected data object.

```csharp
// Get selected object
OrderInfo selectedOrder = sfMultiColumn.SelectedItem as OrderInfo;

if (selectedOrder != null)
{
    Console.WriteLine($"Selected: {selectedOrder.CustomerName}");
    Console.WriteLine($"Order ID: {selectedOrder.OrderID}");
}
```

### SelectedValue

Returns the value based on ValueMember property.

```csharp
// If ValueMember = "OrderID"
int orderId = (int)sfMultiColumn.SelectedValue;

// If ValueMember = "CustomerName"
string customerName = sfMultiColumn.SelectedValue as string;
```

### SelectedIndex

Returns the zero-based index of selected item.

```csharp
int index = sfMultiColumn.SelectedIndex; // -1 if nothing selected

// Set selection programmatically
sfMultiColumn.SelectedIndex = 2; // Select third item
```

### Text Property

Returns the displayed text in the editor (based on DisplayMember).

```csharp
string displayedText = sfMultiColumn.Text;
```

## Data Model Best Practices

### Use ObservableCollection for Dynamic Data

```csharp
// Recommended: Automatically notifies UI of changes
public ObservableCollection<OrderInfo> Orders { get; set; }

// Not recommended for dynamic scenarios
public List<OrderInfo> Orders { get; set; }
```

### Implement INotifyPropertyChanged

```csharp
public class OrderInfo : INotifyPropertyChanged
{
    private string _customerName;
    
    public string CustomerName
    {
        get { return _customerName; }
        set
        {
            if (_customerName != value)
            {
                _customerName = value;
                OnPropertyChanged(nameof(CustomerName));
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Use Appropriate Data Types

```csharp
public class OrderInfo
{
    public int OrderID { get; set; }              // Numbers as int/decimal
    public DateTime OrderDate { get; set; }        // Dates as DateTime
    public decimal Amount { get; set; }            // Currency as decimal
    public bool IsProcessed { get; set; }          // Flags as bool
    public string CustomerName { get; set; }       // Text as string
}
```

## Troubleshooting

### DisplayMember Not Showing Correct Value

**Problem:** Wrong property appears in editor

**Solution:**
```csharp
// Verify property name (case-sensitive)
sfMultiColumn.DisplayMember = "CustomerName"; // ✓ Correct
// sfMultiColumn.DisplayMember = "customername"; // ✗ Wrong case

// Check property exists and is public
public string CustomerName { get; set; } // ✓ Accessible
// private string CustomerName { get; set; } // ✗ Not accessible
```

### ValueMember Returns Null

**Problem:** SelectedValue is null when item is selected

**Solutions:**
1. Ensure ValueMember property name is correct
2. Verify property is not actually null in data
3. Check that SelectedItem is not null first

```csharp
if (sfMultiColumn.SelectedItem != null)
{
    var value = sfMultiColumn.SelectedValue;
}
```

### Complex Binding Not Working

**Problem:** Nested property not displaying

**Solution:**
```csharp
// Ensure nested objects are instantiated
public class OrderInfo
{
    public CustomerDetails CustomerDetails { get; set; } = new CustomerDetails();
}

// Or in constructor
public OrderInfo()
{
    CustomerDetails = new CustomerDetails();
}
```

### Data Not Updating in UI

**Problem:** Changes to data don't appear in dropdown

**Solution:**
```csharp
// Use ObservableCollection
public ObservableCollection<OrderInfo> Orders { get; set; }

// Implement INotifyPropertyChanged on data objects
public class OrderInfo : INotifyPropertyChanged
{
    // Property change notifications
}

// Refresh if needed
sfMultiColumn.ItemsSource = null;
sfMultiColumn.ItemsSource = orders;
```

### Binding to DataTable

**Example:**
```csharp
DataTable dataTable = new DataTable();
dataTable.Columns.Add("OrderID", typeof(int));
dataTable.Columns.Add("CustomerName", typeof(string));
dataTable.Columns.Add("Country", typeof(string));

dataTable.Rows.Add(1001, "Maria Anders", "Germany");
dataTable.Rows.Add(1002, "Ana Trujilo", "Mexico");

sfMultiColumn.ItemsSource = dataTable.DefaultView;
sfMultiColumn.DisplayMember = "CustomerName";
sfMultiColumn.ValueMember = "OrderID";
```
