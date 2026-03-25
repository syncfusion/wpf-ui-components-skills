---
name: syncfusion-wpf-treegrid
description: Comprehensive guide for implementing Syncfusion WPF TreeGrid (SfTreeGrid) control in Windows Presentation Foundation applications. Use this when displaying hierarchical or self-relational data in a grid format with expandable tree structure. Supports column configuration, sorting and filtering hierarchical data, editing tree nodes, cell merging, exporting, and conditional styling.
metadata:
  author: "Syncfusion Inc"
  version : "1.0.0"
version: "33.1.44"
---

# Implementing WPF TreeGrid (SfTreeGrid)

Comprehensive guide for implementing the Syncfusion® WPF TreeGrid control - a data-oriented control that displays self-relational and hierarchical data in a tree structure with columns. This skill provides complete guidance for setup, data binding, column configuration, interactive features, data operations, styling, and advanced capabilities.

## When to Use This Skill

Use this skill when you need to:
- **Display hierarchical data** in a grid with expandable/collapsible tree structure
- **Bind self-relational data** (parent-child relationships within single collection)
- **Bind nested collections** with parent-child object hierarchies
- **Implement load-on-demand** for large hierarchical datasets
- **Configure columns** with various types (text, numeric, datetime, checkbox, combo box)
- **Enable editing** in tree grid with validation
- **Implement sorting and filtering** on hierarchical data
- **Customize selection** behavior (single, multiple, row, cell)
- **Apply conditional styling** based on data or node level
- **Merge cells** across columns in tree grid rows
- **Export tree grids** to Excel or PDF formats
- **Implement MVVM patterns** with TreeGrid controls
- **Customize appearance** with themes, styles, and templates

This skill is essential for any WPF application that needs to present hierarchical or parent-child data in an interactive, feature-rich grid interface.

## Component Overview

The **SfTreeGrid** control provides:

- **Hierarchical Data Display:** Self-relational and nested collection binding
- **Multiple Column Types:** Text, Numeric, DateTime, CheckBox, ComboBox, Hyperlink, and Template columns
- **Interactive Features:** Editing, selection, sorting, filtering, drag-drop
- **Data Operations:** Sorting, filtering with multiple filter levels (Root, All, Extended)
- **Load-On-Demand:** Efficient loading of child nodes on expansion
- **Cell Merging:** Merge adjacent cells across columns
- **Export Capabilities:** Export to Excel and PDF with customization
- **Styling:** Conditional styling, themes, templates, and visual customization
- **MVVM Support:** Complete MVVM pattern compatibility with command binding
- **Performance:** Virtualization and optimized rendering for large datasets

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for initial setup and basic implementation:
- Assembly deployment (Syncfusion.SfGrid.WPF, Syncfusion.Data.WPF, Syncfusion.Shared.WPF)
- Creating WPF project with TreeGrid
- Adding control via Designer, XAML, or code-behind
- Basic TreeGrid setup examples
- Simple self-relational and nested collection binding

### Data Binding & Operations

**Data Binding Approaches**  
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Self-relational data binding (ParentPropertyName, ChildPropertyName)
- Nested collection binding (ChildPropertyName)
- IEnumerable collection support
- Dynamic data object binding
- INotifyCollectionChanged and ObservableCollection
- Data refresh and updates

**Load-On-Demand**  
📄 **Read:** [references/load-on-demand.md](references/load-on-demand.md)
- RequestTreeItems event for on-demand loading
- LoadOnDemandCommand for MVVM scenarios
- Lazy loading child nodes on expansion
- Performance optimization for large datasets
- Populating nodes indicator display

### Column Configuration

**Column Basics**  
📄 **Read:** [references/columns.md](references/columns.md)
- Adding and removing columns
- AutoGenerateColumns vs manual configuration
- Column properties (HeaderText, MappingName, Width, Alignment)
- Column visibility and frozen columns
- Column reordering and resizing
- Text wrapping and trimming

**Column Types**  
📄 **Read:** [references/column-types.md](references/column-types.md)
- TreeGridTextColumn for string data
- TreeGridNumericColumn with number formatting
- TreeGridDateTimeColumn with date patterns
- TreeGridCheckBoxColumn for boolean values
- TreeGridComboBoxColumn for dropdown selection
- TreeGridHyperlinkColumn for clickable links
- TreeGridTemplateColumn for custom content
- Creating custom column types

**Column Sizing**  
📄 **Read:** [references/column-sizing.md](references/column-sizing.md)
- ColumnSizer property (Auto, Star, SizeToCells, SizeToHeader)
- Auto-fitting columns to content
- Manual width specification
- User resizing behavior
- Min and Max width constraints
- Fill remaining space strategies

### Interactive Features

**Editing**  
📄 **Read:** [references/editing.md](references/editing.md)
- AllowEditing property for grid and column level
- Edit modes (OnTap, OnDoubleTap)
- Edit triggers and edit template customization
- BeginEdit and EndEdit events
- Programmatic editing (BeginEdit, EndEdit, CurrentCell)
- Validation during editing

**Data Validation**  
📄 **Read:** [references/data-validation.md](references/data-validation.md)
- IDataErrorInfo validation interface
- INotifyDataErrorInfo support
- Cell-level and row-level validation
- Custom validation rules
- Validation error display customization
- Handling validation errors

**Selection**  
📄 **Read:** [references/selection.md](references/selection.md)
- SelectionMode (Single, Multiple, Extended, None)
- SelectionUnit (Row, Cell, Any)
- SelectedItem and SelectedItems binding
- CurrentCell and CurrentItem properties
- Programmatic selection
- SelectionChanged and CurrentCellActivated events

**Cell Merging**  
📄 **Read:** [references/merge-cells.md](references/merge-cells.md)
- QueryCoveredRange event for merging cells
- Column-wise merging by fixed range
- Merging based on cell content
- Navigation and selection in merged cells
- Performance considerations for cell merging

### Data Operations

**Sorting**  
📄 **Read:** [references/sorting.md](references/sorting.md)
- AllowSorting property (grid and column level)
- Single and multi-column sorting
- Tri-state sorting (ascending, descending, unsorted)
- Programmatic sorting with SortColumnDescriptions
- Custom sorting logic with IComparer
- SortColumnsChanging and SortColumnsChanged events

**Filtering**  
📄 **Read:** [references/filtering.md](references/filtering.md)
- FilterLevel (Root, All, Extended) for hierarchical filtering
- Advanced filter UI with filter row
- Programmatic filtering with FilterPredicates
- Custom filter logic
- FilterChanging and FilterChanged events
- Clearing and persisting filters

### Styling & Customization

**Conditional Styling**  
📄 **Read:** [references/conditional-styling.md](references/conditional-styling.md)
- StyleSelector for row and cell styling
- RowStyleSelector for row-based formatting
- CellStyleSelector for cell-based formatting
- Conditional formatting based on data values
- Node level-based styling (parent vs child)
- Alternating row styles

**Styles and Templates**  
📄 **Read:** [references/styles-and-templates.md](references/styles-and-templates.md)
- CellTemplate and EditTemplate customization
- HeaderTemplate for column headers
- ExpanderTemplate for expand/collapse buttons
- RowHeaderTemplate customization
- Cell and row styling
- Visual states and triggers
- Theme integration

### Advanced Features

**Export and Printing**  
📄 **Read:** [references/export-and-printing.md](references/export-and-printing.md)
- Export to Excel (ExportToExcel method)
- Export to PDF (ExportToPdf method)
- TreeGridExcelExportingOptions customization
- TreeGridPdfExportingOptions customization
- Print functionality (PrintSettings)
- Exporting specific nodes and columns

**Advanced Configuration**  
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- MVVM pattern implementation with ICommand
- GridLinesVisibility customization (Both, Horizontal, Vertical, None)
- Serialization and Deserialization (Save/LoadSettings)
- Localization (ResourceManager)
- UI Automation support for accessibility
- Theming (MaterialLight, MaterialDark, Office2019, etc.)
- Helper methods and utilities
- NodeCheckBox for node selection
- ToolTip customization

## Quick Start Example

### Basic Self-Relational TreeGrid

```xml
<Window x:Class="TreeGridSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    
    <syncfusion:SfTreeGrid Name="treeGrid"
                           AutoGenerateColumns="True"
                           ChildPropertyName="Children"
                           ItemsSource="{Binding Employees}"
                           AllowSorting="True"
                           AllowFiltering="True"
                           AllowEditing="True">
    </syncfusion:SfTreeGrid>
</Window>
```

```csharp
using Syncfusion.UI.Xaml.TreeGrid;
using System.Collections.ObjectModel;

namespace TreeGridSample
{
    public class Employee
    {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Title { get; set; }
        public int ReportsTo { get; set; }
        public ObservableCollection<Employee> Children { get; set; }
        
        public Employee()
        {
            Children = new ObservableCollection<Employee>();
        }
    }

    public class EmployeeViewModel
    {
        public ObservableCollection<Employee> Employees { get; set; }
        
        public EmployeeViewModel()
        {
            Employees = new ObservableCollection<Employee>();
            PopulateData();
        }
        
        private void PopulateData()
        {
            var ceo = new Employee
            {
                EmployeeID = 1,
                FirstName = "Robert",
                LastName = "King",
                Title = "CEO"
            };
            
            ceo.Children.Add(new Employee
            {
                EmployeeID = 2,
                FirstName = "David",
                LastName = "Smith",
                Title = "VP of Sales",
                ReportsTo = 1
            });
            
            ceo.Children.Add(new Employee
            {
                EmployeeID = 3,
                FirstName = "Nancy",
                LastName = "Davolio",
                Title = "VP of Marketing",
                ReportsTo = 1
            });
            
            Employees.Add(ceo);
        }
    }
}
```

## Common Patterns

### Pattern 1: Self-Relational Data Binding

For data where parent-child relationship exists within the same collection:

```csharp
public class Task
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public int? ParentID { get; set; }  // Nullable for root items
}

// ViewModel
public ObservableCollection<Task> Tasks { get; set; }
```

```xml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding Tasks}"
                       ParentPropertyName="ParentID"
                       SelfRelationRootValue="-1"
                       ChildPropertyName="TaskID">
</syncfusion:SfTreeGrid>
```

### Pattern 2: Nested Collection Binding

For data with explicit child collections:

```csharp
public class Folder
{
    public string FolderName { get; set; }
    public ObservableCollection<Folder> SubFolders { get; set; }
}
```

```xml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders">
</syncfusion:SfTreeGrid>
```

### Pattern 3: Load-On-Demand with RequestTreeItems

For large datasets where child nodes load when parent expands:

```csharp
treeGrid.RequestTreeItems += TreeGrid_RequestTreeItems;

private void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    if (e.ParentItem == null)
    {
        // Load root nodes
        e.ChildItems = GetRootNodes();
    }
    else
    {
        // Load child nodes for expanded parent
        var parent = e.ParentItem as BusinessObject;
        e.ChildItems = GetChildNodes(parent.ID);
    }
}
```

### Pattern 4: Custom Column Definition

```xml
<syncfusion:SfTreeGrid ItemsSource="{Binding Employees}"
                       AutoGenerateColumns="False">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="Name" 
                                       MappingName="FirstName"
                                       Width="150"/>
        <syncfusion:TreeGridNumericColumn HeaderText="Age" 
                                          MappingName="Age"
                                          NumberDecimalDigits="0"/>
        <syncfusion:TreeGridDateTimeColumn HeaderText="DOB" 
                                           MappingName="DateOfBirth"
                                           FormatString="MM/dd/yyyy"/>
        <syncfusion:TreeGridCheckBoxColumn HeaderText="Active" 
                                           MappingName="IsActive"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

### Pattern 5: MVVM Command Binding

```xml
<syncfusion:SfTreeGrid ItemsSource="{Binding Employees}"
                       ChildPropertyName="Children">
    <syncfusion:SfTreeGrid.RecordContextMenu>
        <ContextMenu>
            <MenuItem Header="Add Child" 
                      Command="{Binding AddChildCommand}"
                      CommandParameter="{Binding}"/>
            <MenuItem Header="Delete" 
                      Command="{Binding DeleteCommand}"
                      CommandParameter="{Binding}"/>
        </ContextMenu>
    </syncfusion:SfTreeGrid.RecordContextMenu>
</syncfusion:SfTreeGrid>
```

```csharp
public class EmployeeViewModel : INotifyPropertyChanged
{
    public ICommand AddChildCommand { get; set; }
    public ICommand DeleteCommand { get; set; }
    
    public EmployeeViewModel()
    {
        AddChildCommand = new RelayCommand(AddChild);
        DeleteCommand = new RelayCommand(Delete);
    }
    
    private void AddChild(object parameter)
    {
        var parent = parameter as Employee;
        if (parent != null)
        {
            parent.Children.Add(new Employee { /* ... */ });
        }
    }
}
```

## Key Properties

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| **ItemsSource** | IEnumerable | Data source for the TreeGrid |
| **ChildPropertyName** | string | Property name for child collection |
| **ParentPropertyName** | string | Property name for parent ID (self-relational) |
| **SelfRelationRootValue** | object | Value indicating root level nodes |
| **AutoGenerateColumns** | bool | Auto-generate columns from data source |
| **Columns** | TreeGridColumns | Collection of column definitions |

### Behavior Properties

| Property | Type | Description |
|----------|------|-------------|
| **AllowEditing** | bool | Enable/disable cell editing |
| **AllowSorting** | bool | Enable/disable sorting |
| **AllowFiltering** | bool | Enable/disable filtering |
| **FilterLevel** | FilterLevel | Filtering scope (Root, All, Extended) |
| **SelectionMode** | GridSelectionMode | Selection mode (Single, Multiple, Extended) |
| **SelectionUnit** | GridSelectionUnit | Selection unit (Row, Cell, Any) |
| **AutoExpandMode** | AutoExpandMode | Auto-expand behavior (RootNodesExpanded, AllNodesExpanded) |
| **ExpanderPosition** | ExpanderPosition | Position of expand/collapse icon (Start, End) |

### Display Properties

| Property | Type | Description |
|----------|------|-------------|
| **GridLinesVisibility** | GridLinesVisibility | Grid lines display (Both, Horizontal, Vertical, None) |
| **ColumnSizer** | GridLengthUnitType | Column auto-sizing strategy |
| **RowHeight** | double | Height of each row |
| **HeaderRowHeight** | double | Height of header row |
| **IndentColumnWidth** | double | Width of indent for each level |

## Common Use Cases

### 1. Employee Hierarchy Management
Display organizational structure with employees, managers, and departments in expandable tree format.

### 2. File System Browser
Show folders and files in hierarchical structure with load-on-demand for large directory trees.

### 3. Project Task Management
Display projects, sub-projects, tasks, and subtasks with status tracking, editing, and conditional styling.

### 4. Bill of Materials (BOM)
Show product assemblies with components, sub-components, quantities, and costs in hierarchical view.

### 5. Category-Product Catalog
Display product categories, subcategories, and products with filtering, sorting, and export capabilities.

### 6. Account Hierarchy
Financial account trees with parent-child relationships, balance calculations, and drill-down capabilities.

### 7. Multi-Level Approval Workflows
Track approval hierarchies with status indicators, conditional styling, and interactive editing.

### 8. Geographic Data Organization
Display countries, states, cities in hierarchical format with data visualization and export features.

## Performance Considerations

- **Use Load-On-Demand** for datasets with more than 1,000 total nodes
- **Enable Virtualization** (enabled by default) for smooth scrolling
- **Avoid complex templates** in frequently updated cells
- **Use ItemsSourceChanged** event instead of frequent rebinding
- **Batch updates** when modifying multiple nodes
- **Disable live updates** (LiveNodeUpdateMode) when not needed
- **Optimize FilterLevel** choice based on data structure

## Related Skills

- **[Implementing Syncfusion WPF Components](../../../)** - Parent library skill
- **Implementing Data Grids** (Future) - For flat grid data display
- **Implementing Charts** - Already available at [../../data-visualization/implementing-charts/](../../data-visualization/implementing-charts/)

## Support Resources

- **Official Documentation:** https://help.syncfusion.com/wpf/treegrid/overview
- **API Reference:** https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.TreeGrid.html
- **Community Forums:** https://www.syncfusion.com/forums/wpf
- **Sample Browser:** Available via Syncfusion Control Panel

---

**Next Steps:** Navigate to the specific reference file above based on your implementation needs. Start with `getting-started.md` for initial setup, then explore data binding, column configuration, and advanced features as needed.
