# Grouping Bar in WPF PivotGrid

## Overview

The Grouping Bar is a powerful interactive interface that allows users to slice and dice data by rearranging fields between different areas at runtime. It provides a visual, drag-and-drop approach to reorganizing pivot data without requiring code changes.

**Key Features:**
- Drag-and-drop field reorganization
- Add, rearrange, or remove fields dynamically
- Visual indicators for sorting and filtering
- Built-in context menu for advanced operations
- Customizable appearance and behavior

**Grouping Bar Areas:**
- **Filter Header Area:** Holds filter items that apply to the entire pivot grid
- **Data Header Area:** Contains calculation/computation items
- **Column Header Area:** Displays pivot column items
- **Row Header Area:** Contains pivot row items

## Enabling and Disabling Grouping Bar

By default, the grouping bar is enabled in PivotGridControl. You can control its visibility using the `ShowGroupingBar` property.

### XAML Configuration

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="PivotGrid with Grouping Bar" 
        Height="350" Width="525">
    
    <Window.Resources>
        <ResourceDictionary>
            <ObjectDataProvider x:Key="data" 
                              ObjectType="{x:Type local:ProductSales}" 
                              MethodName="GetSalesData" />
        </ResourceDictionary>
    </Window.Resources>
    
    <Grid Name="grid1">
        <syncfusion:PivotGridControl Name="pivotGrid" 
                                    HorizontalAlignment="Left" 
                                    VerticalAlignment="Top" 
                                    ShowGroupingBar="True"
                                    ItemSource="{Binding Source={StaticResource data}}">
        </syncfusion:PivotGridControl>
    </Grid>
</Window>
```

### Code-Behind Configuration

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using System.Windows;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.Loaded += MainWindow_Loaded;
    }

    void MainWindow_Loaded(object sender, RoutedEventArgs e)
    {
        PivotGridControl pivotGrid = new PivotGridControl();
        grid1.Children.Add(pivotGrid);
        
        // Bind data source
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Show the grouping bar
        pivotGrid.ShowGroupingBar = true;
    }
}
```

## Grouping Bar Components

Each item in the grouping bar consists of:

| Component | Description |
|-----------|-------------|
| **Caption** | Text string that identifies the field's content |
| **Filter Button** | Icon button used to filter field values |
| **Sort Indicator** | Visual indicator showing the sort order (ascending/descending) |
| **Remove Button** | Button to remove the item from the grouping bar |

**Limitation:** Sorting and filtering operations for pivot calculation items are restricted. Similarly, sorting operations for filter items are not supported.

## Drag-and-Drop Pivoting

The grouping bar supports intuitive drag-and-drop operations for reorganizing pivot fields.

### Supported Operations

1. **Move Fields Between Areas**
   - Drag row items to column area or vice versa
   - Move calculation items to data area
   - Relocate items to filter area

2. **Reorder Fields Within Area**
   - Change the order of items within the same area
   - Affects nesting hierarchy in the pivot grid

3. **Remove Fields**
   - Click the remove button on any item
   - Drag items outside the grouping bar area

### Example: Moving Fields Programmatically

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigurePivotGrid();
    }
    
    private void ConfigurePivotGrid()
    {
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Define initial rows
        PivotItem productItem = new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        };
        
        PivotItem dateItem = new PivotItem 
        { 
            FieldHeader = "Date", 
            FieldMappingName = "Date", 
            TotalHeader = "Total" 
        };
        
        // Define columns
        PivotItem countryItem = new PivotItem 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        };
        
        PivotItem stateItem = new PivotItem 
        { 
            FieldHeader = "State", 
            FieldMappingName = "State", 
            TotalHeader = "Total" 
        };
        
        // Add to rows
        pivotGrid.PivotRows.Add(productItem);
        pivotGrid.PivotRows.Add(dateItem);
        
        // Add to columns
        pivotGrid.PivotColumns.Add(countryItem);
        pivotGrid.PivotColumns.Add(stateItem);
        
        // Define calculations
        PivotComputationInfo amountCalc = new PivotComputationInfo 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum 
        };
        
        PivotComputationInfo quantityCalc = new PivotComputationInfo 
        { 
            CalculationName = "Quantity", 
            FieldName = "Quantity", 
            SummaryType = SummaryType.Count 
        };
        
        pivotGrid.PivotCalculations.Add(amountCalc);
        pivotGrid.PivotCalculations.Add(quantityCalc);
    }
}
```

## Grouping Bar Properties

### Background Customization

Customize the background color of the entire grouping bar:

```csharp
using System.Windows.Media;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigurePivotGrid();
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        pivotGrid.GroupingBar.Loaded += GroupingBar_Loaded;
    }
    
    void GroupingBar_Loaded(object sender, RoutedEventArgs e)
    {
        // Set background color for entire grouping bar
        pivotGrid.GroupingBar.Background = Brushes.BurlyWood;
    }
}
```

### Items Background Customization

Customize the background color of individual grouping bar items:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    // Set background color for grouping bar items
    pivotGrid.GroupingBar.ItemsBackground = Brushes.BurlyWood;
}
```

### AllowFiltering Property

Control the visibility of filter buttons in the grouping bar:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    // Hide filter buttons on all grouping bar items
    pivotGrid.GroupingBar.AllowFiltering = false;
}
```

**Use Cases:**
- Restrict user filtering capabilities
- Simplify the UI for specific scenarios
- Enforce predefined filters only

### AllowSorting Property

Control the visibility of sorting indicators:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    // Hide sorting indicators on all grouping bar items
    pivotGrid.GroupingBar.AllowSorting = false;
}
```

**Use Cases:**
- Maintain fixed sort order
- Simplify the user interface
- Prevent accidental sort changes

### AllowRemove Property

Control whether users can remove items from the grouping bar:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    // Hide remove buttons on all grouping bar items
    pivotGrid.GroupingBar.AllowRemove = false;
}
```

**Use Cases:**
- Lock the pivot structure
- Prevent users from removing critical fields
- Maintain consistent report layouts

### AllowMultiFunctionalSortFilter Property

Enable Excel-like filtering with advanced options:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    // Enable Excel-like filtering
    pivotGrid.GroupingBar.AllowMultiFunctionalSortFilter = true;
}
```

**Features enabled:**
- Advanced label filters (begins with, ends with, contains)
- Value filters (top 10, greater than, etc.)
- Integrated sorting options
- Clear filter options

## Grouping Bar Context Menu

The grouping bar provides a context menu with powerful operations.

### Context Menu Items

| Menu Item | Description |
|-----------|-------------|
| **Reload Data** | Refreshes the grid with the current item source |
| **Show Field List** | Launches the PivotTable Field List window |
| **Order** | Changes the position of items with submenu options |
| **Calculated Field** | Adds a new calculated field at runtime |

### Order Submenu Options

- **Move to Beginning:** Moves item to first position
- **Move to Left:** Moves item one step left
- **Move to Right:** Moves item one step right
- **Move to End:** Moves item to last position
- **Smallest to Largest:** Arranges fields alphabetically A-Z
- **Largest to Smallest:** Arranges fields alphabetically Z-A

### Enabling/Disabling Context Menu

Control context menu visibility for different grouping bar areas:

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigurePivotGrid();
        pivotGrid.Loaded += PivotGrid_Loaded;
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

## Complete Configuration Example

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Windows;
using System.Windows.Media;

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
        // Customize appearance
        pivotGrid.GroupingBar.Background = Brushes.LightGray;
        pivotGrid.GroupingBar.ItemsBackground = Brushes.White;
        
        // Configure functionality
        pivotGrid.GroupingBar.AllowFiltering = true;
        pivotGrid.GroupingBar.AllowSorting = true;
        pivotGrid.GroupingBar.AllowRemove = true;
        pivotGrid.GroupingBar.AllowMultiFunctionalSortFilter = true;
        
        // Configure context menu
        pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = false;
        pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = false;
        pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = false;
    }
}
```

## Best Practices

### 1. Load Event Timing

Always configure grouping bar properties in the appropriate loaded events:

```csharp
// Correct approach
pivotGrid.Loaded += PivotGrid_Loaded;

void PivotGrid_Loaded(object sender, RoutedEventArgs e)
{
    pivotGrid.GroupingBar.Loaded += GroupingBar_Loaded;
}

void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    // Configure grouping bar properties here
    pivotGrid.GroupingBar.AllowFiltering = true;
}
```

### 2. User Experience

Consider your users when configuring the grouping bar:

```csharp
// For power users - enable all features
pivotGrid.GroupingBar.AllowFiltering = true;
pivotGrid.GroupingBar.AllowSorting = true;
pivotGrid.GroupingBar.AllowRemove = true;
pivotGrid.GroupingBar.AllowMultiFunctionalSortFilter = true;

// For restricted scenarios - limit functionality
pivotGrid.GroupingBar.AllowFiltering = true;
pivotGrid.GroupingBar.AllowSorting = false;
pivotGrid.GroupingBar.AllowRemove = false;
```

### 3. Visual Consistency

Maintain consistent styling across your application:

```csharp
// Define standard colors
var groupingBarBackground = new SolidColorBrush(Color.FromRgb(240, 240, 240));
var itemsBackground = Brushes.White;

// Apply consistently
pivotGrid.GroupingBar.Background = groupingBarBackground;
pivotGrid.GroupingBar.ItemsBackground = itemsBackground;
```

### 4. Context Menu Strategy

Enable context menus strategically based on user roles:

```csharp
// Admin users - full access
if (currentUser.IsAdmin)
{
    pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = false;
    pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = false;
    pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = false;
}
else
{
    // Regular users - limited access
    pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = true;
    pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = true;
    pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = false;
}
```

## Common Use Cases

### Use Case 1: Read-Only Pivot Grid

Lock down the pivot structure for reports:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    pivotGrid.GroupingBar.AllowRemove = false;
    pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = true;
    pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = true;
    pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = true;
}
```

### Use Case 2: Simplified Grouping Bar

Remove clutter for basic scenarios:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    pivotGrid.GroupingBar.AllowFiltering = false;
    pivotGrid.GroupingBar.AllowSorting = false;
    pivotGrid.GroupingBar.AllowRemove = false;
}
```

### Use Case 3: Power User Interface

Enable all features for advanced users:

```csharp
void GroupingBar_Loaded(object sender, RoutedEventArgs e)
{
    pivotGrid.GroupingBar.AllowFiltering = true;
    pivotGrid.GroupingBar.AllowSorting = true;
    pivotGrid.GroupingBar.AllowRemove = true;
    pivotGrid.GroupingBar.AllowMultiFunctionalSortFilter = true;
    
    pivotGrid.GroupingBar.ColumnHeaderArea.DisableContextMenu = false;
    pivotGrid.GroupingBar.RowHeaderArea.DisableContextMenu = false;
    pivotGrid.GroupingBar.DataHeaderArea.DisableContextMenu = false;
}
```

## Related Topics

- **PivotGrid Field List:** Add back removed fields
- **Filtering:** Apply filters through grouping bar
- **Sorting:** Sort data using grouping bar indicators
- **Context Menus:** Advanced operations through right-click menus
