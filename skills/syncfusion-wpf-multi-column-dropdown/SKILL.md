---
name: syncfusion-wpf-multi-column-dropdown
description: Implements Syncfusion WPF Multi Column Dropdown (SfMultiColumnDropDownControl) for displaying searchable dropdown lists with grid view. Use this when creating dropdowns with multiple columns, autocomplete functionality, or filterable data grids in combo boxes. Supports data binding, column configuration, multi-row selection, and popup customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Multi Column Dropdown

The **SfMultiColumnDropDownControl** combines a TextBox editor with a powerful SfDataGrid to provide a searchable, filterable dropdown control that displays multiple columns. It supports autocomplete, incremental filtering, single and multi-selection modes, and extensive customization options.

## Component Overview

SfMultiColumnDropDownControl is a composite control that embeds a SfDataGrid within a dropdown popup, enabling users to:
- **Search and filter** items by typing in the editor
- **View multiple columns** of data in the dropdown
- **Select single or multiple items** from the grid
- **Autocomplete** text based on DisplayMember
- **Customize** popup appearance, columns, and behavior

**Key capabilities:**
- Data binding to any IEnumerable source
- Automatic or manual column generation
- Case-sensitive and diacritic-sensitive filtering
- Complex and indexer property binding
- Keyboard navigation and accessibility support
- Resizable popup with customizable appearance

## When to Use This Skill

Use this skill when you need to:
- Create a WPF dropdown that displays multiple columns of data
- Implement searchable/filterable dropdown lists with grid view
- Build dropdown controls with autocomplete and incremental filtering
- Allow users to select items from a multi-column data grid
- Create editable dropdowns with complex data binding
- Implement dropdowns with single or multi-selection support
- Customize dropdown popup appearance and behavior
- Add keyboard navigation to dropdown controls

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Use this when:
- Setting up the control for the first time
- Understanding assembly requirements
- Adding control via Designer, XAML, or C#
- Creating basic data models
- Binding initial ItemsSource
- Defining DisplayMember and ValueMember

**Topics covered:**
- Assembly deployment (Syncfusion.Data.WPF, Syncfusion.SfGrid.WPF)
- Creating WPF application with SfMultiColumnDropDownControl
- Adding control via Designer, XAML, and code-behind
- Basic ItemsSource binding
- DisplayMember and ValueMember configuration
- Theme support

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)

Use this when:
- Binding data to the dropdown control
- Understanding DisplayMember vs ValueMember
- Working with complex properties
- Using indexer properties
- Accessing SelectedItem and SelectedValue

**Topics covered:**
- ItemsSource property usage
- DisplayMember (visual presentation in editor)
- ValueMember (value for SelectedValue property)
- Binding collections in XAML and code-behind
- Complex property binding (e.g., `customerDetails.Name`)
- Indexer property binding (e.g., `Country[0]`)

### Columns Configuration
📄 **Read:** [references/columns.md](references/columns.md)

Use this when:
- Configuring dropdown grid columns
- Auto-generating or manually defining columns
- Customizing column types and templates
- Handling AutoGeneratingColumn event
- Setting column sizing and width

**Topics covered:**
- AutoGenerateColumns property
- AutoGenerateColumnMode (Reset, RetainOld, ResetAll, None)
- Manually defining columns collection
- Column types (GridTextColumn, GridCurrencyColumn, etc.)
- AutoGeneratingColumn event for customization
- Column templates (HeaderTemplate, CellTemplate)
- GridColumnSizer for width management

### Editing and Filtering
📄 **Read:** [references/editing-autocomplete.md](references/editing-autocomplete.md)

Use this when:
- Enabling autocomplete functionality
- Implementing incremental filtering
- Configuring search conditions
- Filtering based on multiple columns
- Customizing editor behavior

**Topics covered:**
- AllowAutoComplete for text suggestions
- AllowIncrementalFiltering for live filtering
- SearchCondition (StartsWith, Contains, Equals)
- AllowCaseSensitiveFiltering option
- AllowDiacriticSensitiveFiltering option
- Editor properties (ReadOnly, AllowNullInput, AllowSpinOnMouseWheel)
- FilterRecord method override for custom filtering
- SearchText property for multi-column filtering

### Selection
📄 **Read:** [references/selection.md](references/selection.md)

Use this when:
- Implementing single or multi-selection
- Accessing selected items programmatically
- Handling selection changes
- Customizing multi-select separator
- Working with selection events

**Topics covered:**
- SelectionMode (Single, Multiple)
- SelectedItem, SelectedIndex, SelectedValue properties
- SelectedItems collection for multi-select
- SelectionChanged event
- SeparatorString for multi-select display
- TextSelectionOnFocus property
- Programmatic selection
- Selector column with checkboxes

### Popup Customization
📄 **Read:** [references/popup-customization.md](references/popup-customization.md)

Use this when:
- Customizing popup appearance
- Configuring popup size and resizing
- Setting popup background and borders
- Handling popup events
- Adding custom headers to dropdown

**Topics covered:**
- Popup appearance (Background, BorderBrush, BorderThickness)
- Popup sizing (PopupHeight, PopupWidth, Min/Max values)
- IsAutoPopupSize for automatic sizing
- Resizing with resize thumb
- PopupContentTemplate and HeaderTemplate
- Popup events (Opening, Opened, Closing, Closed)
- StaysOpen property
- Custom control in dropdown header

### Keyboard and Accessibility
📄 **Read:** [references/keyboard-accessibility.md](references/keyboard-accessibility.md)

Use this when:
- Implementing keyboard navigation
- Ensuring accessibility compliance
- Setting up UI automation
- Configuring coded UI tests
- Understanding keyboard shortcuts

**Topics covered:**
- Keyboard shortcuts (Enter, Esc, F4, Alt+Arrows, Up/Down)
- Keyboard navigation patterns
- UI Automation support
- Coded UI Test properties and methods
- QTP (Quick Test Professional) support
- Accessibility features

## Quick Start

### Basic Implementation

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourNamespace">
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:SfMultiColumnDropDownControl 
            x:Name="sfMultiColumn"
            Width="200"
            Height="30"
            ItemsSource="{Binding Orders}"
            DisplayMember="CustomerName"
            ValueMember="OrderID"
            SelectedIndex="0"/>
    </Grid>
</Window>
```

### Code-Behind Setup

```csharp
using Syncfusion.UI.Xaml.Grid;
using System.Collections.ObjectModel;

namespace YourNamespace
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create control programmatically
            SfMultiColumnDropDownControl sfMultiColumn = new SfMultiColumnDropDownControl();
            sfMultiColumn.Width = 200;
            sfMultiColumn.Height = 30;
            sfMultiColumn.ItemsSource = viewModel.Orders;
            sfMultiColumn.DisplayMember = "CustomerName";
            sfMultiColumn.ValueMember = "OrderID";
            
            // Add to layout
            Root_Grid.Children.Add(sfMultiColumn);
        }
    }
    
    // Data Model
    public class OrderInfo
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public string CustomerID { get; set; }
        public string Country { get; set; }
        
        public OrderInfo(int orderId, string customerName, string customerId, string country)
        {
            OrderID = orderId;
            CustomerName = customerName;
            CustomerID = customerId;
            Country = country;
        }
    }
    
    // ViewModel
    public class ViewModel
    {
        public ObservableCollection<OrderInfo> Orders { get; set; }
        
        public ViewModel()
        {
            Orders = new ObservableCollection<OrderInfo>
            {
                new OrderInfo(1001, "Maria Anders", "ALFKI", "Germany"),
                new OrderInfo(1002, "Ana Trujilo", "ANATR", "Mexico"),
                new OrderInfo(1003, "Thomas Hardy", "AROUT", "UK")
            };
        }
    }
}
```

## Common Patterns

### Pattern 1: Filterable Dropdown with Autocomplete

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    ValueMember="OrderID"
    AllowAutoComplete="True"
    AllowIncrementalFiltering="True"
    SearchCondition="Contains"
    SelectedIndex="0"/>
```

### Pattern 2: Multi-Selection Dropdown

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    ValueMember="OrderID"
    SelectionMode="Multiple"
    SeparatorString=", "/>
```

Access selected items:
```csharp
// Get all selected items
var selectedItems = sfMultiColumn.SelectedItems;

// Programmatically add selections
sfMultiColumn.SelectedItems.Add(orderItem);
```

### Pattern 3: Custom Columns with Manual Definition

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="OrderID"
    ValueMember="OrderID"
    AutoGenerateColumns="False">
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" HeaderText="Order ID" Width="100"/>
        <syncfusion:GridTextColumn MappingName="CustomerName" HeaderText="Customer" Width="150"/>
        <syncfusion:GridCurrencyColumn MappingName="Amount" HeaderText="Amount" Width="120"/>
        <syncfusion:GridTextColumn MappingName="Country" Width="100"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

### Pattern 4: Customized Popup Appearance

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    ValueMember="OrderID"
    PopupHeight="300"
    PopupWidth="500"
    PopupBackground="WhiteSmoke"
    PopupBorderBrush="SteelBlue"
    PopupBorderThickness="2"
    PopupDropDownGridBackground="White"
    IsAutoPopupSize="False"/>
```

## Key Properties

### Core Data Properties
- **ItemsSource**: Data source for the dropdown (IEnumerable)
- **DisplayMember**: Property path displayed in the editor
- **ValueMember**: Property path for SelectedValue
- **SelectedItem**: Currently selected data object
- **SelectedIndex**: Index of selected item (-1 if none)
- **SelectedValue**: Value from SelectedItem based on ValueMember
- **SelectedItems**: Collection of selected items (multi-select mode)

### Filtering & Editing
- **AllowAutoComplete**: Enable text auto-completion (default: false)
- **AllowIncrementalFiltering**: Filter items based on typed text (default: false)
- **SearchCondition**: Filter condition (StartsWith, Contains, Equals)
- **AllowCaseSensitiveFiltering**: Case-sensitive filtering (default: false)
- **AllowDiacriticSensitiveFiltering**: Diacritic-sensitive filtering (default: true)
- **ReadOnly**: Make editor read-only (default: false)
- **AllowNullInput**: Allow null values (default: false)
- **SearchText**: Current text in editor (read-only)

### Column Configuration
- **AutoGenerateColumns**: Auto-generate columns from data (default: true)
- **AutoGenerateColumnMode**: Column generation mode (Reset, RetainOld, ResetAll, None)
- **Columns**: Collection of manually defined columns
- **GridColumnSizer**: Column width sizing strategy (Auto, Star, None)

### Selection
- **SelectionMode**: Single or Multiple selection
- **SeparatorString**: Separator for multi-select display (default: ";")
- **TextSelectionOnFocus**: Auto-select text on focus (default: false)

### Popup
- **IsDropDownOpen**: Popup open/closed state
- **PopupHeight/PopupWidth**: Popup dimensions
- **PopupMinHeight/PopupMaxHeight**: Size constraints
- **PopupMinWidth/PopupMaxWidth**: Size constraints
- **IsAutoPopupSize**: Auto-size based on grid (default: false)
- **PopupBackground**: Popup background brush
- **PopupBorderBrush**: Popup border brush
- **PopupBorderThickness**: Popup border thickness

### Behavior
- **AllowImmediatePopup**: Open popup when match found during editing
- **AllowSpinOnMouseWheel**: Change selection with mouse wheel (default: true)

## Common Use Cases

### 1. **Product Selection with Search**
Display product catalog with multiple columns (ID, Name, Category, Price), allow users to search by product name with autocomplete.

### 2. **Customer Lookup**
Show customer information in dropdown (Name, Email, Phone, City), filter incrementally as user types, display selected customer name in editor.

### 3. **Multi-Project Assignment**
Allow users to select multiple projects from a list showing project code, name, status, and deadline with checkboxes.

### 4. **Invoice Item Selection**
Dropdown showing items with columns for item code, description, unit price, and stock level, with filtering by item code or description.

### 5. **Employee Directory**
Searchable employee list with columns for name, department, email, and phone number, with autocomplete on employee name.

### 6. **Report Parameter Selection**
Display available reports with name, category, and description columns, allow filtering and selection for report generation.

## Related Skills

- [Implementing WPF Data Grids](../../grids/implementing-data-grids/) - For advanced SfDataGrid features used in dropdown
- [WPF Data Binding](../../../getting-started/) - General WPF data binding patterns
- [WPF Theming](../../../common/theming.md) - Applying themes to components

## Best Practices

1. **Use ValueMember appropriately**: Set ValueMember to a unique identifier property for reliable selection tracking
2. **Enable filtering for large datasets**: Use AllowIncrementalFiltering to improve usability with many items
3. **Define columns manually for control**: Set AutoGenerateColumns="False" and define columns for predictable layout
4. **Handle SelectionChanged event**: Respond to selection changes for dependent UI updates
5. **Set reasonable popup sizes**: Configure PopupHeight/PopupWidth or use IsAutoPopupSize for optimal display
6. **Consider performance**: For very large datasets (>1000 items), implement virtualization or server-side filtering
7. **Test keyboard navigation**: Ensure users can navigate efficiently with keyboard shortcuts
8. **Provide clear DisplayMember**: Choose a property that clearly identifies items to users

## Troubleshooting

- **Columns not showing**: Verify ItemsSource is bound and contains data; check AutoGenerateColumns setting
- **Filtering not working**: Ensure AllowIncrementalFiltering="True" and DisplayMember is set correctly
- **Autocomplete not appearing**: Set AllowAutoComplete="True" and verify DisplayMember property exists in data
- **Multi-select not visible**: Set SelectionMode="Multiple" and check SelectedItems collection
- **Popup not sizing correctly**: Adjust PopupHeight/PopupWidth or enable IsAutoPopupSize
- **Performance issues with large data**: Consider data virtualization or limit displayed columns
