---
name: syncfusion-wpf-datagrid
description: Implements Syncfusion WPF DataGrid (SfDataGrid) component for displaying and manipulating tabular data in Windows Presentation Foundation applications. Use this when working with data grids, table views, or data binding scenarios. Supports sorting, filtering, grouping, editing, Excel/PDF export, drag and drop, master-detail views, summaries, and extensive styling options.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion WPF DataGrid

The Syncfusion WPF DataGrid (SfDataGrid) is a high-performance, feature-rich component for displaying and manipulating tabular data in Windows Presentation Foundation applications. It provides extensive functionality for data binding, column configuration, sorting, filtering, grouping, editing, and export capabilities.

## When to Use This Skill

Use the Syncfusion WPF DataGrid when you need to:

- **Display tabular data** in Windows Presentation Foundation applications
- **Handle large datasets** efficiently with virtualization and performance optimization
- **Enable data operations** like sorting, filtering, and grouping
- **Implement data editing** with validation and custom editors
- **Export data** to Excel or PDF formats
- **Create hierarchical views** with master-detail relationships
- **Calculate summaries** for groups, tables, or captions
- **Customize appearance** with conditional styling and themes
- **Support MVVM pattern** with comprehensive data binding
- **Configure columns** with various types and auto-sizing strategies

## Component Overview

The SfDataGrid component offers:

- **Data Binding**: Support for any IEnumerable source, DataTable, and MVVM patterns
- **Column Types**: 15+ built-in column types (Text, Numeric, DateTime, ComboBox, CheckBox, etc.)
- **Data Operations**: Advanced sorting, filtering, and grouping with custom logic support
- **Editing**: In-line editing with validation, custom editors, and add/delete operations
- **Export**: Export to Excel and PDF with customization options
- **Summaries**: Table, group, and caption summaries with built-in aggregates
- **Master-Detail**: Nested grid views for hierarchical data display
- **Styling**: Conditional styling, themes, and complete visual customization
- **Performance**: UI virtualization, on-demand loading, and optimized rendering
- **Selection**: Multiple selection modes (row, cell, extended) with programmatic control

## Documentation and Navigation Guide

### Getting Started and Setup

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:**
- Setting up SfDataGrid for the first time in a WPF project
- Installing NuGet packages and adding assembly references
- Configuring basic grid with data binding
- Understanding namespace imports and XAML setup
- Implementing license registration

**Covers:**
- Installation via NuGet (Syncfusion.SfGrid.WPF)
- Assembly references and dependencies
- Basic XAML markup for SfDataGrid
- Data binding with ItemsSource
- Auto-generating columns
- License configuration
- Essential namespace imports

### Columns and Configuration

#### Column Types and Definition

📄 **Read:** [references/columns.md](references/columns.md)

**When to read:**
- Defining columns manually instead of auto-generation
- Using specific column types (GridTextColumn, GridNumericColumn, etc.)
- Configuring column properties (HeaderText, MappingName, Width)
- Managing column operations (add, remove, reorder)
- Enabling column resizing and drag-drop
- Freezing columns for horizontal scrolling
- Binding column properties to ViewModel

**Covers:**
- 15+ built-in column types with usage scenarios
- Manual column definition in XAML and code-behind
- Column properties and configuration options
- Column manipulation methods
- AllowResizing and column width management
- AllowDraggingColumns for reordering
- FrozenColumnsCount for pinned columns
- ViewModel binding for dynamic column configuration

#### Auto-Sizing Columns

📄 **Read:** [references/autosize-columns.md](references/autosize-columns.md)

**When to read:**
- Implementing automatic column width calculations
- Using ColumnSizer modes (Auto, Star, SizeToCells)
- Filling remaining grid width with specific columns
- Refreshing column sizes at runtime
- Customizing auto-sizing logic
- Implementing star column ratios
- Handling font-based width calculations

**Covers:**
- ColumnSizer enumeration and modes
- AutoFitColumns() method usage
- ColumnWidthMode for individual columns
- AutoLastColumnFill and AutoWithLastColumnFill
- Refreshing auto-size calculations dynamically
- Custom GridColumnSizer implementation
- Star column sizer ratios and weights
- Font-based width calculation strategies

### Data Operations

#### Sorting

📄 **Read:** [references/sorting.md](references/sorting.md)

**When to read:**
- Enabling sorting in the DataGrid
- Implementing single or multi-column sorting
- Adding programmatic sorting
- Creating custom sort logic
- Handling tri-state sorting
- Responding to sorting events

**Covers:**
- AllowSorting property for enabling sort functionality
- SortColumnDescriptions collection for programmatic sorting
- AllowSorting at column level
- Custom IComparer implementation for sort logic
- SortClickAction (SingleClick, DoubleClick)
- Tri-state sorting configuration
- Sorting events (SortColumnsChanging, SortColumnsChanged)
- Removing sort indicators

#### Filtering

📄 **Read:** [references/filtering.md](references/filtering.md)

**When to read:**
- Implementing data filtering functionality
- Adding programmatic filters with predicates
- Configuring filter row or advanced filter UI
- Creating custom filter logic
- Using Excel-style filtering
- Handling filter events

**Covers:**
- View filtering with View.Filter predicate
- Column filtering with FilterPredicates
- Filter row implementation (AllowFiltering)
- Advanced filter UI configuration
- Excel-style filter popup
- Custom filter logic and conditions
- FilterChanged and FilterChanging events
- RefreshFilter() method usage
- Filter row customization

#### Grouping

📄 **Read:** [references/grouping.md](references/grouping.md)

**When to read:**
- Enabling data grouping functionality
- Adding programmatic grouping by columns
- Customizing group display and behavior
- Implementing custom grouping logic
- Managing group expand/collapse state
- Adding group summaries

**Covers:**
- AllowGrouping property configuration
- GroupColumnDescriptions for programmatic grouping
- Drag-drop column headers to group panel
- Custom IGroupComparer implementation
- Group expand/collapse methods
- ShowGroupDropArea property
- Group summaries integration
- Grouping events (GroupExpanding, GroupExpanded, GroupCollapsing, GroupCollapsed)

#### Selection

📄 **Read:** [references/selection.md](references/selection.md)

**When to read:**
- Configuring selection modes in the grid
- Implementing row or cell selection
- Adding programmatic selection
- Handling selection events
- Using checkbox selection column

**Covers:**
- SelectionMode (Single, Multiple, Extended, None)
- SelectionUnit (Row, Cell, Any)
- SelectedItem and SelectedItems properties
- SelectedIndex for single selection
- Programmatic selection methods (SelectRow, SelectCell)
- Selection events (SelectionChanging, SelectionChanged)
- CheckBox selection column
- NavigationMode and keyboard selection

### Editing and Manipulation

#### Grid Editing

📄 **Read:** [references/editing.md](references/editing.md)

**When to read:**
- Enabling in-line editing in the grid
- Configuring edit modes and triggers
- Implementing column-level editing control
- Adding validation to edited cells
- Creating custom editors for columns
- Managing add/delete row operations
- Handling editing events

**Covers:**
- AllowEditing property for grid-level control
- AllowEditing at column level
- EditTrigger (OnTap, OnDoubleTap)
- BeginEdit() and EndEdit() methods
- Validation with IDataErrorInfo and INotifyDataErrorInfo
- Custom editor templates
- AddNewRow configuration
- Deleting rows programmatically
- Editing events (CurrentCellBeginEdit, CurrentCellEndEdit, etc.)

#### Data Manipulation

📄 **Read:** [references/data-manipulation.md](references/data-manipulation.md)

**When to read:**
- Adding rows programmatically to the grid
- Deleting rows or clearing data
- Updating cell values in code
- Performing CRUD operations
- Implementing data validation
- Refreshing grid data after changes

**Covers:**
- View.AddNew() for adding rows
- View.Remove() and View.RemoveAt() for deletion
- Updating data source for cell changes
- CRUD operation patterns
- Data validation strategies
- RefreshView() and Refresh() methods
- CommitEdit() and CancelEdit() operations
- UpdateSourceTrigger configuration

#### Drag and Drop

📄 **Read:** [references/drag-and-drop.md](references/drag-and-drop.md)

**When to read:**
- Enabling row drag-drop within grid
- Implementing drag-drop between multiple grids
- Configuring column drag-drop
- Customizing drag-drop behavior
- Handling drag-drop events

**Covers:**
- AllowDraggingRows property
- Row drag-drop within same grid
- Drag-drop between different grids
- AllowDraggingColumns for column reordering
- Drag-drop events (RowDragStarting, RowDragOver, RowDropping, RowDropped)
- Custom drag-drop logic
- Drop position indicators

### Export and Serialization

#### Export to Excel and PDF

📄 **Read:** [references/export.md](references/export.md)

**When to read:**
- Exporting grid data to Excel format
- Exporting grid to PDF documents
- Customizing export appearance and formatting
- Exporting selected rows only
- Handling export events
- Adding required assemblies for export functionality

**Covers:**
- ExcelExportingOptions for Excel export
- PdfExportingOptions for PDF export
- ExportToExcel() and ExportToPdf() methods
- Required NuGet packages (Syncfusion.SfGridConverter.WPF)
- Customizing export styles and appearance
- ExportMode (All, SelectedRows)
- Export events (ExcelExportingOptions.CellExporting)
- Excluding columns from export
- Custom cell formatting in exports

### Advanced Features

#### Summaries

📄 **Read:** [references/summaries.md](references/summaries.md)

**When to read:**
- Adding table summaries for entire grid
- Implementing group summaries within groups
- Creating caption summaries in group headers
- Using built-in aggregate functions
- Creating custom summary calculations
- Formatting summary display

**Covers:**
- GridSummaryRow for table summaries
- TableSummaryRows collection
- GroupSummaryRows for group-level aggregates
- CaptionSummaryRow for group captions
- Built-in aggregates (Count, Sum, Average, Min, Max, etc.)
- Custom aggregate implementation
- Summary display format strings
- SummaryColumns configuration
- ShowSummaryInRow for custom templates

#### Master-Detail View

📄 **Read:** [references/master-detail-view.md](references/master-detail-view.md)

**When to read:**
- Creating hierarchical grid views
- Implementing master-detail relationships
- Configuring nested grids
- Setting up auto-expanding details
- Supporting multiple detail views
- Customizing detail view templates

**Covers:**
- DetailsViewDefinition setup
- AutoGenerateRelations property
- Nested SfDataGrid in DetailsViewTemplate
- RelationalColumn for specifying child collection
- ExpandAllDetailsView() method
- Multiple detail view levels
- Custom detail view templates
- Detail view events

#### Advanced Configuration

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

**When to read:**
- Implementing localization and globalization
- Serializing and deserializing grid state
- Saving and restoring grid configuration
- Supporting multiple languages and cultures
- Persisting user preferences

**Covers:**
- Localization with ResourceManager and Resx files
- Setting CurrentUICulture for language support
- Creating culture-specific resource files
- Serialization options (SerializationOptions)
- Serialize() and Deserialize() methods
- Saving grid state to XML file or stream
- Restoring grid configuration from file
- Custom column serialization with SerializationController
- Performance tips (UI virtualization, NotifyListChangedAction)
- AllowFrozenGroupHeaders option

### Styling and Customization

📄 **Read:** [references/styling.md](references/styling.md)

**When to read:**
- Applying cell styling and conditional formatting
- Customizing row appearance
- Implementing alternate row styles
- Creating conditional styling based on data
- Styling headers, summaries, and group rows
- Applying themes with SfSkinManager

**Covers:**
- CellStyle for uniform cell styling
- CellStyleSelector for conditional cell formatting
- RowStyle and RowStyleSelector for row appearance
- AlternatingRowStyle for zebra striping
- HeaderStyle for column headers
- GroupCaptionStyle for group rows
- SummaryCellStyle for summary rows
- SfSkinManager for theme application
- Style triggers and data binding in styles
- Visual states (Normal, Selected, Current)

## Quick Start Example

Here's a minimal example to get started with SfDataGrid in WPF:

### XAML

```xml
<Window x:Class="DataGridDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    
    <Grid>
        <syncfusion:SfDataGrid x:Name="dataGrid"
                               AutoGenerateColumns="True"
                               AllowSorting="True"
                               AllowFiltering="True"
                               AllowGrouping="True"
                               AllowEditing="True"
                               ItemsSource="{Binding Orders}">
        </syncfusion:SfDataGrid>
    </Grid>
</Window>
```

### Code-Behind with ViewModel

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using Syncfusion.UI.Xaml.Grid;

public class OrderInfo : INotifyPropertyChanged
{
    private string orderId;
    private string customerName;
    private string country;
    private decimal orderTotal;

    public string OrderID 
    { 
        get => orderId; 
        set { orderId = value; OnPropertyChanged(nameof(OrderID)); } 
    }
    
    public string CustomerName 
    { 
        get => customerName; 
        set { customerName = value; OnPropertyChanged(nameof(CustomerName)); } 
    }
    
    public string Country 
    { 
        get => country; 
        set { country = value; OnPropertyChanged(nameof(Country)); } 
    }
    
    public decimal OrderTotal 
    { 
        get => orderTotal; 
        set { orderTotal = value; OnPropertyChanged(nameof(OrderTotal)); } 
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class ViewModel
{
    public ObservableCollection<OrderInfo> Orders { get; set; }

    public ViewModel()
    {
        Orders = new ObservableCollection<OrderInfo>
        {
            new OrderInfo { OrderID = "1001", CustomerName = "John Doe", Country = "USA", OrderTotal = 1500.50m },
            new OrderInfo { OrderID = "1002", CustomerName = "Jane Smith", Country = "UK", OrderTotal = 2300.75m },
            new OrderInfo { OrderID = "1003", CustomerName = "Bob Johnson", Country = "Canada", OrderTotal = 890.25m }
        };
    }
}
```

## Common Patterns

### MVVM Data Binding Pattern

```csharp
// ViewModel property
public ObservableCollection<Employee> Employees { get; set; }

// XAML binding
<syncfusion:SfDataGrid ItemsSource="{Binding Employees}" />
```

### Manual Column Definition

```xml
<syncfusion:SfDataGrid AutoGenerateColumns="False" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" HeaderText="Order ID" Width="120"/>
        <syncfusion:GridTextColumn MappingName="CustomerName" HeaderText="Customer" Width="150"/>
        <syncfusion:GridNumericColumn MappingName="OrderTotal" HeaderText="Total" 
                                      NumberDecimalDigits="2" Width="120"/>
        <syncfusion:GridDateTimeColumn MappingName="OrderDate" HeaderText="Date" 
                                       Pattern="ShortDate" Width="120"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

### Programmatic Sorting

```csharp
// Add sort description
dataGrid.SortColumnDescriptions.Add(new SortColumnDescription
{
    ColumnName = "OrderTotal",
    SortDirection = ListSortDirection.Descending
});
```

### Programmatic Filtering

```csharp
// Set filter predicate
dataGrid.View.Filter = (obj) =>
{
    var order = obj as OrderInfo;
    return order != null && order.OrderTotal > 1000;
};
dataGrid.View.RefreshFilter();
```

### Selection Handling

```csharp
// Handle selection changed
dataGrid.SelectionChanged += (sender, e) =>
{
    var selectedItem = dataGrid.SelectedItem as OrderInfo;
    if (selectedItem != null)
    {
        MessageBox.Show($"Selected Order: {selectedItem.OrderID}");
    }
};
```

### Export to Excel

```csharp
using Syncfusion.UI.Xaml.Grid.Converter;

var options = new ExcelExportingOptions
{
    ExcelVersion = ExcelVersion.Excel2013
};
dataGrid.ExportToExcel(dataGrid.View, options);
```

## Key Properties

### Core Properties

- **ItemsSource**: Data source for the grid (IEnumerable)
- **AutoGenerateColumns**: Automatically create columns from data source (bool)
- **Columns**: Collection of manually defined columns

### Data Operation Properties

- **AllowSorting**: Enable column sorting (bool)
- **AllowFiltering**: Enable filtering functionality (bool)
- **AllowGrouping**: Enable grouping by columns (bool)
- **AllowEditing**: Enable in-line editing (bool)

### Column Configuration

- **ColumnSizer**: Auto-sizing strategy for columns
- **FrozenColumnsCount**: Number of columns frozen from left (int)
- **AllowResizingColumns**: Enable column resizing (bool)
- **AllowDraggingColumns**: Enable column reordering (bool)

### Selection Properties

- **SelectionMode**: Selection behavior (Single, Multiple, Extended, None)
- **SelectionUnit**: Unit of selection (Row, Cell, Any)
- **SelectedItem**: Currently selected item (object)
- **SelectedItems**: Collection of selected items

### Visual Properties

- **HeaderRowHeight**: Height of column header row (double)
- **RowHeight**: Default height for data rows (double)
- **GridLinesVisibility**: Visibility of grid lines (Both, Horizontal, Vertical, None)

## Common Use Cases

### Business Applications

Create data entry forms with editable grids, validation, and CRUD operations for managing business records like orders, customers, or inventory.

### Data Analysis and Reporting

Display large datasets with sorting, filtering, grouping, and summaries for analyzing sales data, financial reports, or statistical information.

### Master-Detail Views

Show hierarchical data relationships with expandable detail views for order-line items, customer-orders, or category-products structures.

### Data Export and Printing

Export filtered and grouped data to Excel or PDF for sharing reports, creating backups, or printing formatted documents.

### Dashboard and Monitoring

Build real-time data monitoring grids with auto-refresh, conditional styling for alerts, and summary rows for KPIs.

---

**Next Steps:** Navigate to the specific reference documentation above based on your implementation needs. Start with [getting-started.md](references/getting-started.md) for initial setup, then explore feature-specific guides as required.
