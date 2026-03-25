# Selection in SfMultiColumnDropDownControl

## Table of Contents
- [Overview](#overview)
- [Single Selection](#single-selection)
- [Multi-Selection](#multi-selection)
- [Selection Properties](#selection-properties)
- [Selection Events](#selection-events)
- [Programmatic Selection](#programmatic-selection)
- [Text Selection on Focus](#text-selection-on-focus)
- [Custom Header Template](#custom-header-template)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

SfMultiColumnDropDownControl supports both single and multiple item selection from the dropdown grid. Selection behavior is controlled through the `SelectionMode` property and accessed through various selection properties.

## Single Selection

**Default Mode:** Users can select only one item at a time.

### Enabling Single Selection

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    ValueMember="OrderID"
    SelectionMode="Single"
    SelectedIndex="0"/>
```

### Single Selection Behavior

- **Click on row:** Selects the row and closes popup
- **Enter key:** Selects highlighted row and closes popup
- **Arrow keys:** Navigate between rows (when popup is open)
- **Escape key:** Closes popup without changing selection

### Code-Behind Single Selection

```csharp
using Syncfusion.UI.Xaml.Grid;

// Set to single selection mode
sfMultiColumn.SelectionMode = DropDownSelectionMode.Single;

// Select by index
sfMultiColumn.SelectedIndex = 2;

// Get selected item
OrderInfo selectedOrder = sfMultiColumn.SelectedItem as OrderInfo;

// Get selected value (based on ValueMember)
int orderId = (int)sfMultiColumn.SelectedValue;
```

## Multi-Selection

Allows users to select multiple items simultaneously.

### Enabling Multi-Selection

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    ValueMember="OrderID"
    SelectionMode="Multiple"/>
```

### Multi-Selection Methods

Users can select multiple items in several ways:

1. **Click individual rows** - Each click toggles selection
2. **Drag mouse** over rows to select range
3. **Space key** - Toggles selection of focused row
4. **Checkbox column** - Check/uncheck selector boxes

### Multi-Selection Behavior

- Selections persist while popup is open
- **OK button:** Confirms selections and closes popup
- **Cancel/Escape:** Discards changes and closes popup
- **Enter key:** Confirms selections and closes popup

### Code-Behind Multi-Selection

```csharp
// Enable multi-selection
sfMultiColumn.SelectionMode = DropDownSelectionMode.Multiple;

// Get all selected items
var selectedItems = sfMultiColumn.SelectedItems;

// Iterate through selections
foreach (OrderInfo order in selectedItems)
{
    Console.WriteLine($"Selected: {order.CustomerName}");
}

// Programmatically select multiple items
sfMultiColumn.SelectedItems.Clear();
sfMultiColumn.SelectedItems.Add(orders[0]);
sfMultiColumn.SelectedItems.Add(orders[2]);
sfMultiColumn.SelectedItems.Add(orders[5]);
```

### Separator String Customization

By default, multiple selected values are separated by `;` in the editor. Customize this separator:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    SelectionMode="Multiple"
    DisplayMember="CustomerName"
    SeparatorString=", "/>
```

**Default Display:** `Maria Anders; Ana Trujilo; Thomas Hardy`  
**Custom Display:** `Maria Anders, Ana Trujilo, Thomas Hardy`

### Code-Behind Separator

```csharp
// Use comma and space
sfMultiColumn.SeparatorString = ", ";

// Use pipe separator
sfMultiColumn.SeparatorString = " | ";

// Use line break (for multi-line display)
sfMultiColumn.SeparatorString = Environment.NewLine;
```

## Selection Properties

### SelectedItem

Gets or sets the currently selected data object.

```csharp
// Get selected item
OrderInfo order = sfMultiColumn.SelectedItem as OrderInfo;

if (order != null)
{
    MessageBox.Show($"Selected: {order.CustomerName} from {order.Country}");
}

// Set selected item
OrderInfo newSelection = orders.First(o => o.OrderID == 1005);
sfMultiColumn.SelectedItem = newSelection;
```

**In XAML:**
```xml
<syncfusion:SfMultiColumnDropDownControl 
    SelectedItem="{Binding CurrentOrder, Mode=TwoWay}"/>
```

### SelectedIndex

Gets or sets the zero-based index of the selected item.

```csharp
// Get current index
int currentIndex = sfMultiColumn.SelectedIndex; // -1 if nothing selected

// Select by index
sfMultiColumn.SelectedIndex = 0; // Select first item
sfMultiColumn.SelectedIndex = 4; // Select fifth item

// Clear selection
sfMultiColumn.SelectedIndex = -1;
```

**In XAML:**
```xml
<syncfusion:SfMultiColumnDropDownControl 
    SelectedIndex="2"/>  <!-- Select third item on load -->
```

### SelectedValue

Gets the value from the selected item based on `ValueMember` property.

```csharp
// If ValueMember = "OrderID"
int orderId = (int)sfMultiColumn.SelectedValue;

// If ValueMember = "CustomerName"
string customerName = sfMultiColumn.SelectedValue as string;

// Check for selection
if (sfMultiColumn.SelectedValue != null)
{
    // Process selected value
}
```

**In XAML (Two-Way Binding):**
```xml
<syncfusion:SfMultiColumnDropDownControl 
    ValueMember="OrderID"
    SelectedValue="{Binding SelectedOrderID, Mode=TwoWay}"/>
```

### SelectedItems (Multi-Selection)

Collection of all selected items in multi-selection mode.

```csharp
// Get selected items collection
var selectedItems = sfMultiColumn.SelectedItems;

// Check count
int selectedCount = selectedItems.Count;

// Add items programmatically
sfMultiColumn.SelectedItems.Add(orderItem1);
sfMultiColumn.SelectedItems.Add(orderItem2);

// Remove specific item
sfMultiColumn.SelectedItems.Remove(orderItem1);

// Clear all selections
sfMultiColumn.SelectedItems.Clear();

// Convert to typed list
List<OrderInfo> selectedOrders = selectedItems.Cast<OrderInfo>().ToList();
```

### Text Property

Gets the display text in the editor (based on DisplayMember).

```csharp
// Get displayed text
string displayedText = sfMultiColumn.Text;

// For multi-selection, includes separator
// Example: "Maria Anders; Ana Trujilo"
```

## Selection Events

### SelectionChanged Event

Fires when the selection changes.

**XAML:**
```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    SelectionChanged="SfMultiColumn_SelectionChanged"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"/>
```

**Event Handler:**
```csharp
private void SfMultiColumn_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var control = sender as SfMultiColumnDropDownControl;
    
    // Get selected index
    int selectedIndex = control.SelectedIndex;
    
    // Get selected item
    OrderInfo selectedOrder = control.SelectedItem as OrderInfo;
    
    if (selectedOrder != null)
    {
        // Update UI based on selection
        StatusTextBlock.Text = $"Selected: {selectedOrder.CustomerName}";
        
        // Trigger dependent operations
        LoadOrderDetails(selectedOrder.OrderID);
    }
}
```

### SelectionChangedEventArgs Properties

```csharp
private void SfMultiColumn_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Access event arguments
    var args = e as SelectionChangedEventArgs;
    
    // Get selected index from event
    int selectedIndex = args.SelectedIndex;
    
    // Get selected item from event
    object selectedItem = args.SelectedItem;
    
    // Process selection
    if (selectedItem is OrderInfo order)
    {
        ProcessOrder(order);
    }
}
```

## Programmatic Selection

### Select by Index

```csharp
// Select first item
sfMultiColumn.SelectedIndex = 0;

// Select last item
sfMultiColumn.SelectedIndex = sfMultiColumn.ItemsSource.Cast<object>().Count() - 1;

// Select middle item
int count = sfMultiColumn.ItemsSource.Cast<object>().Count();
sfMultiColumn.SelectedIndex = count / 2;
```

### Select by Item

```csharp
// Find and select specific item
var orders = sfMultiColumn.ItemsSource as ObservableCollection<OrderInfo>;
var targetOrder = orders.FirstOrDefault(o => o.OrderID == 1003);

if (targetOrder != null)
{
    sfMultiColumn.SelectedItem = targetOrder;
}
```

### Select by Value

```csharp
// If ValueMember = "OrderID"
int targetOrderId = 1003;

var orders = sfMultiColumn.ItemsSource as ObservableCollection<OrderInfo>;
var targetOrder = orders.FirstOrDefault(o => o.OrderID == targetOrderId);

if (targetOrder != null)
{
    sfMultiColumn.SelectedItem = targetOrder;
}
```

### Multi-Selection Programmatically

```csharp
// Clear existing selections
sfMultiColumn.SelectedItems.Clear();

// Add multiple items
var orders = sfMultiColumn.ItemsSource as ObservableCollection<OrderInfo>;

foreach (var order in orders.Where(o => o.Country == "Germany"))
{
    sfMultiColumn.SelectedItems.Add(order);
}

// Or select by indices
sfMultiColumn.SelectedItems.Add(orders[0]);
sfMultiColumn.SelectedItems.Add(orders[3]);
sfMultiColumn.SelectedItems.Add(orders[7]);
```

### Clear Selection

```csharp
// Single selection
sfMultiColumn.SelectedIndex = -1;
sfMultiColumn.SelectedItem = null;

// Multi-selection
sfMultiColumn.SelectedItems.Clear();
```

## Text Selection on Focus

The `TextSelectionOnFocus` property automatically selects all text in the editor when the control receives focus.

### Enabling Text Selection

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    TextSelectionOnFocus="True"
    SelectedIndex="0"/>
```

**Behavior:**
1. Control receives focus (Tab key or mouse click)
2. All text in editor is selected
3. User can immediately start typing to replace

### Use Cases

- Quick data entry scenarios
- Replace selections frequently
- Keyboard-focused workflows

```csharp
// Enable in code
sfMultiColumn.TextSelectionOnFocus = true;
```

## Custom Header Template

Add custom controls (like search textbox) to the dropdown header.

### Adding Search Box in Header

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    SelectionMode="Multiple">
    
    <syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock Text="Select Orders" FontWeight="Bold" Margin="5"/>
                <TextBox x:Name="SearchBox" 
                         Margin="5" 
                         Watermark="Search..."
                         TextChanged="SearchBox_TextChanged"/>
                <Separator Margin="0,5"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
</syncfusion:SfMultiColumnDropDownControl>
```

### Header with Selection Count

```xml
<syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
    <DataTemplate>
        <Border Background="LightGray" Padding="10">
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="Selected: " FontWeight="Bold"/>
                <TextBlock Text="{Binding ElementName=sfMultiColumn, Path=SelectedItems.Count}"/>
                <TextBlock Text=" items"/>
            </StackPanel>
        </Border>
    </DataTemplate>
</syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
```

## Best Practices

### 1. Use SelectedItem Over SelectedIndex

```csharp
// Recommended: Get full object
OrderInfo order = sfMultiColumn.SelectedItem as OrderInfo;
int orderId = order?.OrderID ?? 0;

// Less flexible: Only get index
int index = sfMultiColumn.SelectedIndex;
```

### 2. Handle Null Selections

```csharp
// Always check for null
if (sfMultiColumn.SelectedItem != null)
{
    var order = sfMultiColumn.SelectedItem as OrderInfo;
    ProcessOrder(order);
}

// Or use null-conditional operator
var customerName = (sfMultiColumn.SelectedItem as OrderInfo)?.CustomerName ?? "None";
```

### 3. Use SelectionChanged Event for Dependent UI

```csharp
private void SfMultiColumn_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var order = sfMultiColumn.SelectedItem as OrderInfo;
    
    if (order != null)
    {
        // Update other controls
        DetailsPanel.DataContext = order;
        
        // Load related data
        LoadOrderItems(order.OrderID);
        
        // Enable/disable buttons
        DeleteButton.IsEnabled = true;
    }
}
```

### 4. Provide Clear Visual Feedback for Multi-Selection

```xml
<!-- Show selected count in UI -->
<StackPanel>
    <syncfusion:SfMultiColumnDropDownControl 
        x:Name="sfMultiColumn"
        SelectionMode="Multiple"/>
    
    <TextBlock>
        <Run Text="Selected: "/>
        <Run Text="{Binding ElementName=sfMultiColumn, Path=SelectedItems.Count}"/>
        <Run Text=" orders"/>
    </TextBlock>
</StackPanel>
```

## Troubleshooting

### SelectedItem Returns Null

**Problem:** Selected item is null after selection

**Solutions:**
```csharp
// Check if ItemsSource is properly bound
if (sfMultiColumn.ItemsSource != null)
{
    var item = sfMultiColumn.SelectedItem;
}

// Ensure SelectedIndex is valid
if (sfMultiColumn.SelectedIndex >= 0)
{
    var item = sfMultiColumn.SelectedItem;
}
```

### SelectionChanged Fires Unexpectedly

**Problem:** Event fires during initialization

**Solution:**
```csharp
private bool _isInitializing = true;

public MainWindow()
{
    InitializeComponent();
    
    // Setup control
    sfMultiColumn.ItemsSource = orders;
    sfMultiColumn.SelectedIndex = 0;
    
    _isInitializing = false;
}

private void SfMultiColumn_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (_isInitializing) return;
    
    // Handle selection change
}
```

### Multi-Selection Not Showing in Editor

**Problem:** Multiple selections don't appear in TextBox

**Solutions:**
```xml
<!-- Ensure SelectionMode is Multiple -->
<syncfusion:SfMultiColumnDropDownControl 
    SelectionMode="Multiple"
    DisplayMember="CustomerName"/>  <!-- DisplayMember must be set -->

<!-- Check SeparatorString is visible -->
<syncfusion:SfMultiColumnDropDownControl 
    SeparatorString="; "/>  <!-- Use visible separator -->
```

### Cannot Clear Selection

**Problem:** SelectedIndex = -1 doesn't work

**Solution:**
```csharp
// For single selection
sfMultiColumn.SelectedItem = null;
sfMultiColumn.SelectedIndex = -1;

// For multi-selection
sfMultiColumn.SelectedItems.Clear();

// Ensure AllowNullInput is True if you need to allow empty selection
sfMultiColumn.AllowNullInput = true;
```
