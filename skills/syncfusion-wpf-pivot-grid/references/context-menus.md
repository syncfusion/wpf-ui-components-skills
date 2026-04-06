# Context Menus in WPF PivotGrid

## Overview

Context menus in PivotGrid provide quick access to common operations through right-click interactions. The PivotGrid supports multiple types of context menus in different areas, each with specific functionality tailored to its location.

**Available Context Menus:**
- **RowPivotsOnly Mode Context Menu:** Column-specific operations for filtering and sorting
- **Grouping Bar Context Menu:** Field management and pivot structure operations
- **Header Cell Context Menu:** Expand/collapse operations for row and column headers

## RowPivotsOnly Mode Context Menu

In RowPivotsOnly mode, the PivotGrid displays a specialized context menu for column operations. This mode shows all dimensions as rows and calculations as columns.

### Available Operations

| Menu Item | Description |
|-----------|-------------|
| **Allow Filtering** | Enables or disables filtering for the selected column |
| **Allow Sorting** | Enables or disables sorting for the selected column |
| **HideValueColumn** | Hides the selected calculation column |
| **ClearValueFilters** | Clears all filters applied to calculation columns |
| **ClearValueSorts** | Clears all sorting applied to calculation columns |
| **ShowPivotValueChooser** | Opens the value chooser window to manage columns |

### Enabling Context Menu

Use the `EnableContextMenu` property for row and column header styles:

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using System.Windows;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Enable context menu for row headers
        pivotGrid.RowHeaderCellStyle.EnableContextMenu = true;
        
        // Enable context menu for column headers
        pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = true;
    }
}
```

### Complete RowPivotsOnly Example

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Windows;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigureRowPivotsOnly();
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    private void ConfigureRowPivotsOnly()
    {
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Add rows only (no columns for RowPivotsOnly mode)
        PivotItem productItem = new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        };
        
        PivotItem countryItem = new PivotItem 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        };
        
        pivotGrid.PivotRows.Add(productItem);
        pivotGrid.PivotRows.Add(countryItem);
        
        // Add calculations
        PivotComputationInfo amountCalc = new PivotComputationInfo 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum,
            AllowFilter = true  // Enable filtering
        };
        
        PivotComputationInfo quantityCalc = new PivotComputationInfo 
        { 
            CalculationName = "Quantity", 
            FieldName = "Quantity", 
            SummaryType = SummaryType.Count,
            AllowFilter = true  // Enable filtering
        };
        
        pivotGrid.PivotCalculations.Add(amountCalc);
        pivotGrid.PivotCalculations.Add(quantityCalc);
        
        // Enable RowPivotsOnly mode
        pivotGrid.RowPivotsOnly = true;
    }
    
    void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Enable context menus
        pivotGrid.RowHeaderCellStyle.EnableContextMenu = true;
        pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = true;
    }
}
```

### Context Menu Operations

**Filtering Columns:**
1. Right-click on a column header
2. Select "Allow Filtering" (toggle on/off)
3. When enabled, filter icon appears in the column
4. Click filter icon to apply filters

**Sorting Columns:**
1. Right-click on a column header
2. Select "Allow Sorting" (toggle on/off)
3. Click column header to sort

**Hiding Columns:**
1. Right-click on a column header
2. Select "HideValueColumn"
3. The column is hidden from view
4. Use "ShowPivotValueChooser" to restore it

**Clearing Filters:**
1. Right-click on any column header
2. Select "ClearValueFilters"
3. All column filters are removed

**Clearing Sorts:**
1. Right-click on any column header
2. Select "ClearValueSorts"
3. All column sorting is removed

**Managing Columns:**
1. Right-click on any column header
2. Select "ShowPivotValueChooser"
3. Value chooser window opens
4. Add, remove, or reorder columns

## Grouping Bar Context Menu

The grouping bar context menu provides operations for field management and pivot structure manipulation.

### Menu Structure

**Main Menu Items:**
- **Reload Data:** Refreshes the grid with current data
- **Show Field List:** Opens the PivotTable Field List window
- **Order:** Submenu for repositioning fields
- **Calculated Field:** Adds new calculated fields at runtime

**Order Submenu:**
- Move to Beginning
- Move to Left
- Move to Right
- Move to End
- Smallest to Largest (A-Z sort)
- Largest to Smallest (Z-A sort)

### Enabling Grouping Bar Context Menu

By default, context menus are enabled in all grouping bar areas. Control visibility using `DisableContextMenu` property:

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Windows;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigurePivotGrid();
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    private void ConfigurePivotGrid()
    {
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure pivot structure
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Date", 
            FieldMappingName = "Date", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "State", 
            FieldMappingName = "State", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotCalculations.Add(new PivotComputationInfo 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum 
        });
        
        pivotGrid.PivotCalculations.Add(new PivotComputationInfo 
        { 
            CalculationName = "Quantity", 
            FieldName = "Quantity", 
            SummaryType = SummaryType.Count 
        });
    }
    
    void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        pivotGrid.GroupingBar.Loaded += GroupingBar_Loaded;
    }
    
    void GroupingBar_Loaded(object sender, RoutedEventArgs e)
    {
        // Enable context menu for column header area
        pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = false;
        
        // Disable context menu for row header area
        pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = true;
        
        // Enable context menu for data header area
        pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = false;
    }
}
```

### Selective Context Menu Configuration

Configure context menus for specific scenarios:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    // Scenario 1: Admin users - full access
    if (CurrentUser.IsAdmin)
    {
        pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = false;
        pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = false;
        pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = false;
    }
    // Scenario 2: Regular users - limited access
    else if (CurrentUser.IsRegular)
    {
        pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = true;
        pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = true;
        pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = false;
    }
    // Scenario 3: Read-only users - no access
    else
    {
        pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = true;
        pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = true;
        pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = true;
    }
}
```

### Using Grouping Bar Context Menu

**Reloading Data:**
1. Right-click on any grouping bar item
2. Select "Reload Data"
3. Grid refreshes with current ItemSource data

**Opening Field List:**
1. Right-click on any grouping bar item
2. Select "Show Field List"
3. PivotTable Field List window opens

**Reordering Fields:**
1. Right-click on a grouping bar item
2. Hover over "Order" to see submenu
3. Select desired position:
   - "Move to Beginning" - First position in area
   - "Move to Left" - One position left
   - "Move to Right" - One position right
   - "Move to End" - Last position in area

**Sorting Fields:**
1. Right-click on any grouping bar item
2. Hover over "Order" to see submenu
3. Select sorting option:
   - "Smallest to Largest" - Alphabetical A-Z
   - "Largest to Smallest" - Reverse alphabetical Z-A

**Adding Calculated Fields:**
1. Right-click on any grouping bar item
2. Select "Calculated Field"
3. Dialog opens for defining new calculation
4. Enter name, formula, and format
5. New field added to pivot grid

## Header Cell Context Menu

Header cell context menus provide expand/collapse operations for grouped data in rows and columns.

### Available Operations

**Expand Operations:**
- Expand current group
- Expand all groups in row/column
- Expand to specific level

**Collapse Operations:**
- Collapse current group
- Collapse all groups in row/column

### Enabling Header Cell Context Menu

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Windows;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigurePivotGrid();
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    private void ConfigurePivotGrid()
    {
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure rows with multiple levels for grouping
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Date", 
            FieldMappingName = "Date", 
            TotalHeader = "Total" 
        });
        
        // Configure columns with multiple levels
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "State", 
            FieldMappingName = "State", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotCalculations.Add(new PivotComputationInfo 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum 
        });
    }
    
    void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Enable context menu for column headers
        pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = true;
        
        // Enable context menu for row headers
        pivotGrid.RowHeaderCellStyle.EnableContextMenu = true;
    }
}
```

### Programmatic Expand/Collapse

In addition to context menus, you can control expansion programmatically:

```csharp
void PivotGrid_Loaded(object sender, RoutedEventArgs e)
{
    // Expand specific row by unique text
    pivotGrid.ExpandRow("Bike");
    
    // Collapse specific row
    pivotGrid.CollapseRow("Bike");
    
    // Expand specific column
    pivotGrid.ExpandColumn("Canada");
    
    // Collapse specific column
    pivotGrid.CollapseColumn("Canada");
    
    // Expand multiple rows
    pivotGrid.ExpandRow(new List<string> { "Bike", "Car" });
    
    // Collapse multiple rows
    pivotGrid.CollapseRow(new List<string> { "Bike", "Car" });
    
    // Expand multiple columns
    pivotGrid.ExpandColumn(new List<string> { "Canada", "France" });
    
    // Collapse multiple columns
    pivotGrid.CollapseColumn(new List<string> { "Canada", "France" });
    
    // Expand all groups (rows and columns)
    pivotGrid.ExpandAllGroup();
    
    // Collapse all groups (rows and columns)
    pivotGrid.CollapseAllGroup();
}
```

### Context Menu with Programmatic Control

Combine context menus with programmatic control for advanced scenarios:

```csharp
using System.Collections.Generic;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigurePivotGrid();
        pivotGrid.Loaded += PivotGrid_Loaded;
        
        // Add buttons for programmatic control
        AddControlButtons();
    }
    
    void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Enable context menus
        pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = true;
        pivotGrid.RowHeaderCellStyle.EnableContextMenu = true;
    }
    
    private void AddControlButtons()
    {
        StackPanel buttonPanel = new StackPanel 
        { 
            Orientation = Orientation.Horizontal, 
            Margin = new Thickness(5) 
        };
        
        Button expandAllButton = new Button 
        { 
            Content = "Expand All", 
            Margin = new Thickness(5) 
        };
        expandAllButton.Click += (s, e) => pivotGrid.ExpandAllGroup();
        
        Button collapseAllButton = new Button 
        { 
            Content = "Collapse All", 
            Margin = new Thickness(5) 
        };
        collapseAllButton.Click += (s, e) => pivotGrid.CollapseAllGroup();
        
        Button expandProductsButton = new Button 
        { 
            Content = "Expand Products", 
            Margin = new Thickness(5) 
        };
        expandProductsButton.Click += (s, e) => 
        {
            pivotGrid.ExpandRow(new List<string> { "Bike", "Car", "Van" });
        };
        
        buttonPanel.Children.Add(expandAllButton);
        buttonPanel.Children.Add(collapseAllButton);
        buttonPanel.Children.Add(expandProductsButton);
        
        // Add to main layout
        // (assumes DockPanel or similar container)
    }
}
```

## Customizing Context Menus

### Disabling Specific Menu Items

While you cannot remove individual menu items, you can control entire context menu areas:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    // Disable all context menus in grouping bar
    pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = true;
    pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = true;
    pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = true;
}

void PivotGrid_Loaded(object sender, RoutedEventArgs e)
{
    // Disable header cell context menus
    pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = false;
    pivotGrid.RowHeaderCellStyle.EnableContextMenu = false;
}
```

### Role-Based Context Menu Access

Configure context menus based on user permissions:

```csharp
public class UserRoles
{
    public bool CanReloadData { get; set; }
    public bool CanModifyStructure { get; set; }
    public bool CanAddCalculations { get; set; }
    public bool CanExpandCollapse { get; set; }
}

public partial class MainWindow : Window
{
    private UserRoles currentUserRoles;
    
    void ConfigureContextMenusByRole(UserRoles roles)
    {
        currentUserRoles = roles;
        
        // Configure based on permissions
        if (!roles.CanModifyStructure)
        {
            // Disable structure modification
            pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = true;
            pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = true;
        }
        
        if (!roles.CanExpandCollapse)
        {
            // Disable expand/collapse
            pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = false;
            pivotGrid.RowHeaderCellStyle.EnableContextMenu = false;
        }
        
        // Note: Individual menu items cannot be disabled
        // Use grouping bar properties to restrict functionality
        if (!roles.CanAddCalculations)
        {
            // Disable grouping bar entirely to prevent calculated field addition
            pivotGrid.ShowGroupingBar = false;
        }
    }
}
```

## Complete Example: All Context Menus

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Windows;
using System.Collections.Generic;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigureCompletePivotGrid();
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    private void ConfigureCompletePivotGrid()
    {
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure rows
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Date", 
            FieldMappingName = "Date", 
            TotalHeader = "Total" 
        });
        
        // Configure columns
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "State", 
            FieldMappingName = "State", 
            TotalHeader = "Total" 
        });
        
        // Configure calculations
        pivotGrid.PivotCalculations.Add(new PivotComputationInfo 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum,
            AllowFilter = true
        });
        
        pivotGrid.PivotCalculations.Add(new PivotComputationInfo 
        { 
            CalculationName = "Quantity", 
            FieldName = "Quantity", 
            SummaryType = SummaryType.Count,
            AllowFilter = true
        });
        
        // Enable grouping bar
        pivotGrid.ShowGroupingBar = true;
    }
    
    void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Configure grouping bar context menu
        pivotGrid.GroupingBar.Loaded += GroupingBar_Loaded;
        
        // Enable header cell context menus
        pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = true;
        pivotGrid.RowHeaderCellStyle.EnableContextMenu = true;
    }
    
    void GroupingBar_Loaded(object sender, RoutedEventArgs e)
    {
        // Enable all grouping bar context menus
        pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = false;
        pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = false;
        pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = false;
    }
}
```

## Best Practices

### 1. Enable Appropriate Context Menus

Enable context menus based on user needs:

```csharp
// Power users - enable all
pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = true;
pivotGrid.RowHeaderCellStyle.EnableContextMenu = true;
pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = false;
pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = false;

// Report viewers - limit to expand/collapse only
pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = true;
pivotGrid.RowHeaderCellStyle.EnableContextMenu = true;
pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = true;
pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = true;
```

### 2. Provide Programmatic Alternatives

Offer buttons or menu items for common operations:

```csharp
// Instead of relying only on context menus
// Provide toolbar buttons for key operations
Button reloadButton = new Button { Content = "Reload Data" };
reloadButton.Click += (s, e) => 
{
    pivotGrid.ItemSource = ProductSales.GetSalesData();
};
```

### 3. Context Menu Timing

Always configure context menus in the appropriate loaded events:

```csharp
// Correct timing
pivotGrid.Loaded += PivotGrid_Loaded;

void PivotGrid_Loaded(object sender, RoutedEventArgs e)
{
    pivotGrid.GroupingBar.Loaded += GroupingBar_Loaded;
    pivotGrid.ColumnHeaderCellStyle.EnableContextMenu = true;
}

void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = false;
}
```

### 4. User Guidance

Provide tooltips or documentation for context menu usage:

```csharp
// Add tooltips to guide users
pivotGrid.ToolTip = "Right-click on headers for more options";
```

## Related Topics

- **Grouping Bar:** Interactive field management interface
- **RowPivotsOnly Mode:** Specialized layout with column operations
- **Field List:** Add and remove fields dynamically
- **Filtering and Sorting:** Operations accessible through context menus
