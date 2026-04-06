# Field List and Schema Designer in WPF PivotGrid

## Table of Contents
- [Overview](#overview)
- [PivotGrid Field List](#pivotgrid-field-list)
- [PivotSchemaDesigner](#pivotschemadesigner)
- [Binding PivotSchemaDesigner to PivotGrid](#binding-pivotschemadesigner-to-pivotgrid)
- [Pivot Value Chooser](#pivot-value-chooser)
- [Setting Field Captions](#setting-field-captions)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The Field List and Schema Designer components provide advanced user interfaces for managing PivotGrid structure and layout. These tools enable dynamic field management, Excel-like pivoting experiences, and runtime customization without code changes.

**Key Components:**

| Component | Purpose |
|-----------|---------|
| **PivotGrid Field List** | Window for adding/removing fields deleted from grouping bar |
| **PivotSchemaDesigner** | Excel-style interface with field list and layout sections |
| **Pivot Value Chooser** | Specialized dialog for managing calculations in RowPivotsOnly mode |

## PivotGrid Field List

The PivotTable Field List solves a critical limitation: once fields are removed from the grouping bar, they cannot be added back. The field list maintains a collection of available fields and allows users to restore deleted items.

### Key Features

- Display fields not currently in the pivot grid
- Add fields by checking checkboxes
- Remove fields by unchecking checkboxes
- Filter individual fields through expander icons
- Accessible via grouping bar context menu

### Enabling Field List in XAML

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="PivotGrid with Field List" 
        Height="600" Width="800">
    
    <Window.Resources>
        <ResourceDictionary>
            <ObjectDataProvider x:Key="data" 
                              ObjectType="{x:Type local:ProductSales}" 
                              MethodName="GetSalesData" />
        </ResourceDictionary>
    </Window.Resources>
    
    <Grid>
        <syncfusion:PivotGridControl Name="pivotGrid" 
                                    HorizontalAlignment="Left" 
                                    VerticalAlignment="Top" 
                                    ShowFieldList="True"
                                    ItemSource="{Binding Source={StaticResource data}}">
            
            <!-- Define Rows -->
            <syncfusion:PivotGridControl.PivotRows>
                <syncfusion:PivotItem FieldHeader="Product" 
                                     FieldMappingName="Product" 
                                     TotalHeader="Total" />
                <syncfusion:PivotItem FieldHeader="Date" 
                                     FieldMappingName="Date" 
                                     TotalHeader="Total" />
            </syncfusion:PivotGridControl.PivotRows>
            
            <!-- Define Columns -->
            <syncfusion:PivotGridControl.PivotColumns>
                <syncfusion:PivotItem FieldHeader="Country" 
                                     FieldMappingName="Country" 
                                     TotalHeader="Total" />
                <syncfusion:PivotItem FieldHeader="State" 
                                     FieldMappingName="State" 
                                     TotalHeader="Total" />
            </syncfusion:PivotGridControl.PivotColumns>
            
            <!-- Define Calculations -->
            <syncfusion:PivotGridControl.PivotCalculations>
                <syncfusion:PivotComputationInfo CalculationName="Total" 
                                                FieldName="Amount" 
                                                Format="C" 
                                                SummaryType="DoubleTotalSum" />
                <syncfusion:PivotComputationInfo CalculationName="Total" 
                                                FieldName="Quantity" 
                                                SummaryType="Count" />
            </syncfusion:PivotGridControl.PivotCalculations>
            
        </syncfusion:PivotGridControl>
    </Grid>
</Window>
```

### Enabling Field List in Code-Behind

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
        ConfigureFieldList();
    }
    
    private void ConfigureFieldList()
    {
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure rows
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
        
        pivotGrid.PivotRows.Add(productItem);
        pivotGrid.PivotRows.Add(dateItem);
        
        // Configure columns
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
        
        pivotGrid.PivotColumns.Add(countryItem);
        pivotGrid.PivotColumns.Add(stateItem);
        
        // Configure calculations
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
        
        // Enable field list
        pivotGrid.ShowFieldList = true;
    }
}
```

### Accessing Field List

**Method 1: Property Setting**
```csharp
pivotGrid.ShowFieldList = true;
```

**Method 2: Grouping Bar Context Menu**
1. Right-click on the grouping bar
2. Select "Show Field List" from the context menu
3. The field list window will launch

### Field List Behavior

**Adding Fields:**
- Check the checkbox next to a field name
- By default, fields are added to the row label area
- Drag fields to column/data areas as needed

**Removing Fields:**
- Uncheck the checkbox next to a field name
- Field is removed from the pivot grid
- Field remains in the field list for later use

**Filtering Fields:**
- Click the expander icon on any field item
- Filter pop-up window opens
- Select/deselect values to filter

## PivotSchemaDesigner

The PivotSchemaDesigner provides an Excel-like interface for managing pivot structure. It combines field management with visual layout control.

### Architecture

The PivotSchemaDesigner consists of two main sections:

**1. Fields Section (Top)**
- Lists all available fields from the data source
- Includes row, column, and calculation items
- Called "PivotTable Field List"
- Supports filtering on individual fields

**2. Layout Section (Bottom)**
- Visual areas for organizing fields
- Four drop zones: Report Filter, Column Label, Row Label, Values
- Drag-and-drop field reorganization
- Real-time pivot grid updates

### Layout Section Areas

| Area | Purpose | Typical Use |
|------|---------|-------------|
| **Report Filter** | Filter entire report based on selected values | Apply global filters (e.g., Year, Region) |
| **Column Label** | Display fields as columns at top of report | Horizontal categorization |
| **Row Label** | Display fields as rows on left side | Vertical categorization and grouping |
| **Values** | Display summary/calculation fields | Aggregated data (sum, count, average) |

### Field Nesting

Fields are nested based on their position:
- In columns: Lower position fields nest within fields above them
- In rows: Lower position fields nest within fields above them

## Binding PivotSchemaDesigner to PivotGrid

The PivotSchemaDesigner is bound to PivotGridControl using the `PivotControl` property.

### XAML Binding with Grid Layout

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="PivotGrid with Schema Designer" 
        Height="600" Width="1000">
    
    <Window.Resources>
        <ResourceDictionary>
            <ObjectDataProvider x:Key="data" 
                              ObjectType="{x:Type local:ProductSales}" 
                              MethodName="GetSalesData" />
        </ResourceDictionary>
    </Window.Resources>
    
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width=".35*"/>
        </Grid.ColumnDefinitions>
        
        <!-- PivotGrid -->
        <syncfusion:PivotGridControl Grid.Column="0"
                                     Name="pivotGrid" 
                                     HorizontalAlignment="Left" 
                                     VerticalAlignment="Top" 
                                     EnableValueEditing="True"
                                     ItemSource="{Binding Source={StaticResource data}}">
            
            <syncfusion:PivotGridControl.PivotRows>
                <syncfusion:PivotItem FieldHeader="Product" 
                                     FieldMappingName="Product" 
                                     TotalHeader="Total" />
                <syncfusion:PivotItem FieldHeader="Date" 
                                     FieldMappingName="Date" 
                                     TotalHeader="Total"/>
            </syncfusion:PivotGridControl.PivotRows>
            
            <syncfusion:PivotGridControl.PivotColumns>
                <syncfusion:PivotItem FieldHeader="Country" 
                                     FieldMappingName="Country" 
                                     TotalHeader="Total" />
                <syncfusion:PivotItem FieldHeader="State" 
                                     FieldMappingName="State" 
                                     TotalHeader="Total"/>
            </syncfusion:PivotGridControl.PivotColumns>
            
            <syncfusion:PivotGridControl.PivotCalculations>
                <syncfusion:PivotComputationInfo CalculationName="Total" 
                                                FieldName="Amount" 
                                                Format="C" 
                                                SummaryType="DoubleTotalSum" />
                <syncfusion:PivotComputationInfo CalculationName="Total" 
                                                FieldName="Quantity" 
                                                SummaryType="Count" />
            </syncfusion:PivotGridControl.PivotCalculations>
            
        </syncfusion:PivotGridControl>
        
        <!-- PivotSchemaDesigner -->
        <syncfusion:PivotSchemaDesigner Grid.Column="1" 
                                       Name="schemaDesigner" 
                                       PivotControl="{Binding ElementName=pivotGrid}">
        </syncfusion:PivotSchemaDesigner>
    </Grid>
</Window>
```

### Code-Behind Binding

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Windows;
using System.Windows.Controls;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    PivotSchemaDesigner schemaDesigner = new PivotSchemaDesigner();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigurePivotWithDesigner();
    }
    
    private void ConfigurePivotWithDesigner()
    {
        // Add PivotGrid to first column
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure pivot items
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
        
        // Add to rows and columns
        pivotGrid.PivotRows.Add(productItem);
        pivotGrid.PivotRows.Add(dateItem);
        pivotGrid.PivotColumns.Add(countryItem);
        pivotGrid.PivotColumns.Add(stateItem);
        
        // Configure calculations
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
        
        // Bind schema designer to pivot grid
        schemaDesigner.PivotControl = pivotGrid;
        
        // Add schema designer to second column
        grid1.Children.Add(schemaDesigner);
        Grid.SetColumn(schemaDesigner, 1);
    }
}
```

### Report Filter Usage

The Report Filter area allows filtering the entire pivot report:

```csharp
// Add item to report filter area (programmatically)
PivotItem yearFilter = new PivotItem
{
    FieldHeader = "Year",
    FieldMappingName = "Year",
    TotalHeader = "Total"
};

// Add to filters collection
pivotGrid.Filters.Add(new FilterExpression
{
    DimensionName = "Year",
    DimensionHeader = "Year",
    Expression = "Year = '2023'"
});
```

**Using Report Filter in UI:**
1. Drag a field to the Report Filter area
2. Click the expander icon on the filter item
3. Select values to filter the entire report
4. Only data matching the selection displays in the grid

## Pivot Value Chooser

The Pivot Value Chooser is specialized for RowPivotsOnly mode, where all dimensions appear as rows and calculations appear as columns.

### Purpose

- List all available PivotFields from the data source
- Enable users to select and add PivotCalculations
- Support drag-and-drop field operations
- Reorder calculation columns at runtime

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **ShowPivotValueChooser** | bool | Shows/hides the value chooser dialog |
| **PossiblePivotCalculations** | ObservableCollection | Collection of possible calculations to display |

### Enabling with Default Values

Display all fields from the ItemSource automatically:

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
        // Enable RowPivotsOnly mode
        pivotGrid.RowPivotsOnly = true;
        
        // Show value chooser with all default fields
        pivotGrid.ShowPivotValueChooser = true;
    }
}
```

### Enabling with Custom Calculations

Define specific calculations to appear in the value chooser:

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Collections.ObjectModel;
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
        // Create collection of possible calculations
        ObservableCollection<PivotComputationInfo> possibleComputations = 
            new ObservableCollection<PivotComputationInfo>
            {
                new PivotComputationInfo
                {
                    CalculationName = "Total Amount",
                    FieldName = "Amount",
                    Format = "C",
                    SummaryType = SummaryType.DoubleTotalSum
                },
                new PivotComputationInfo
                {
                    CalculationName = "Average Amount",
                    FieldName = "Amount",
                    Format = "C",
                    SummaryType = SummaryType.DoubleAverage
                },
                new PivotComputationInfo
                {
                    CalculationName = "Total Quantity",
                    FieldName = "Quantity",
                    Format = "#,##0",
                    SummaryType = SummaryType.IntTotalSum
                },
                new PivotComputationInfo
                {
                    CalculationName = "Item Count",
                    FieldName = "Product",
                    SummaryType = SummaryType.Count
                }
            };
        
        // Set possible calculations
        pivotGrid.PossiblePivotCalculations = possibleComputations;
        
        // Enable RowPivotsOnly mode
        pivotGrid.RowPivotsOnly = true;
        
        // Show value chooser
        pivotGrid.ShowPivotValueChooser = true;
    }
}
```

### Using the Value Chooser

**Adding Calculations:**
1. Open the Value Chooser window
2. Check the calculation you want to add
3. The calculation column appears in the pivot grid

**Removing Calculations:**
1. Uncheck the calculation in the Value Chooser
2. The column is removed from the pivot grid

**Reordering Calculations:**
1. Drag calculation items within the Value Chooser
2. The column order updates in the pivot grid

## Setting Field Captions

Field captions enable duplicate fields with different display names, allowing multiple views of the same data field.

### Use Cases

- Show same field with different aggregations (e.g., Sum vs. Average)
- Display same dimension with different filters
- Create multiple calculation columns from one field

### FieldCaption Property

Available on both `PivotItem` and `PivotComputationInfo`:

```csharp
// For PivotItem
PivotItem item = new PivotItem
{
    FieldMappingName = "Product",  // Underlying field
    FieldCaption = "Product_1",     // Display name
    TotalHeader = "Total"
};

// For PivotComputationInfo
PivotComputationInfo calc = new PivotComputationInfo
{
    FieldName = "Amount",           // Underlying field
    FieldCaption = "Amount_1",      // Display name
    CalculationName = "Total",
    SummaryType = SummaryType.DoubleTotalSum
};
```

### XAML with Field Captions

```xml
<syncfusion:PivotGridControl Name="pivotGrid" 
                            FreezeHeaders="False" 
                            ItemSource="{Binding Source={StaticResource data}}">
    
    <!-- Rows with duplicate Product field -->
    <syncfusion:PivotGridControl.PivotRows>
        <syncfusion:PivotItem FieldMappingName="Product" 
                             FieldCaption="Product_1" 
                             TotalHeader="Total"/>
        <syncfusion:PivotItem FieldMappingName="Product" 
                             FieldCaption="Product_2" 
                             TotalHeader="Total"/>
        <syncfusion:PivotItem FieldMappingName="Date" 
                             FieldHeader="Date" 
                             FieldCaption="Date" 
                             TotalHeader="Total"/>
    </syncfusion:PivotGridControl.PivotRows>
    
    <!-- Columns -->
    <syncfusion:PivotGridControl.PivotColumns>
        <syncfusion:PivotItem FieldMappingName="Country" 
                             FieldHeader="Country" 
                             FieldCaption="Country" 
                             TotalHeader="Total"/>
        <syncfusion:PivotItem FieldMappingName="State" 
                             FieldHeader="State" 
                             FieldCaption="State" 
                             TotalHeader="Total"/>
    </syncfusion:PivotGridControl.PivotColumns>
    
    <!-- Calculations with duplicate Amount field -->
    <syncfusion:PivotGridControl.PivotCalculations>
        <syncfusion:PivotComputationInfo CalculationName="Total" 
                                        Description="Sum of values" 
                                        FieldName="Amount"  
                                        FieldCaption="Amount_1" 
                                        Format="C" 
                                        SummaryType="DoubleTotalSum"/>
        <syncfusion:PivotComputationInfo CalculationName="Total" 
                                        Description="Sum as integer" 
                                        FieldName="Amount"  
                                        FieldCaption="Amount_2" 
                                        Format="#,##" 
                                        SummaryType="IntTotalSum"/>
        <syncfusion:PivotComputationInfo CalculationName="Total" 
                                        Description="Item count" 
                                        FieldName="Quantity" 
                                        FieldCaption="Quantity" 
                                        Format="#,##0"/>
    </syncfusion:PivotGridControl.PivotCalculations>
    
</syncfusion:PivotGridControl>
```

### Code-Behind with Field Captions

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
        ConfigureDuplicateFields();
    }
    
    private void ConfigureDuplicateFields()
    {
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Add duplicate Product fields with different captions
        PivotItem product1 = new PivotItem 
        { 
            FieldCaption = "Product_1", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        };
        
        PivotItem product2 = new PivotItem 
        { 
            FieldCaption = "Product_2", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        };
        
        PivotItem dateItem = new PivotItem 
        { 
            FieldHeader = "Date", 
            FieldMappingName = "Date", 
            TotalHeader = "Total" 
        };
        
        pivotGrid.PivotRows.Add(product1);
        pivotGrid.PivotRows.Add(product2);
        pivotGrid.PivotRows.Add(dateItem);
        
        // Add columns
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
        
        pivotGrid.PivotColumns.Add(countryItem);
        pivotGrid.PivotColumns.Add(stateItem);
        
        // Add duplicate Amount calculations with different captions
        PivotComputationInfo amount1 = new PivotComputationInfo 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            FieldCaption = "Amount_1", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum 
        };
        
        PivotComputationInfo amount2 = new PivotComputationInfo 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            FieldCaption = "Amount_2", 
            Format = "#,##", 
            SummaryType = SummaryType.IntTotalSum 
        };
        
        PivotComputationInfo quantity = new PivotComputationInfo 
        { 
            CalculationName = "Quantity", 
            FieldName = "Quantity", 
            Format = "#,##0", 
            SummaryType = SummaryType.Count 
        };
        
        pivotGrid.PivotCalculations.Add(amount1);
        pivotGrid.PivotCalculations.Add(amount2);
        pivotGrid.PivotCalculations.Add(quantity);
    }
}
```

## Complete Examples

### Example 1: Field List with Schema Designer

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Windows;
using System.Windows.Controls;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    PivotSchemaDesigner schemaDesigner = new PivotSchemaDesigner();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigureCompleteSetup();
    }
    
    private void ConfigureCompleteSetup()
    {
        // Configure grid layout
        Grid mainGrid = new Grid();
        mainGrid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(2, GridUnitType.Star) });
        mainGrid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star) });
        
        // Configure PivotGrid
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotCalculations.Add(new PivotComputationInfo 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum 
        });
        
        // Enable field list
        pivotGrid.ShowFieldList = true;
        
        // Bind schema designer
        schemaDesigner.PivotControl = pivotGrid;
        
        // Add to grid
        mainGrid.Children.Add(pivotGrid);
        mainGrid.Children.Add(schemaDesigner);
        Grid.SetColumn(schemaDesigner, 1);
        
        this.Content = mainGrid;
    }
}
```

### Example 2: Value Chooser in RowPivotsOnly Mode

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
using System.Collections.ObjectModel;
using System.Windows;

public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigureValueChooser();
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    private void ConfigureValueChooser()
    {
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Add rows only (RowPivotsOnly mode)
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        });
        
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        });
        
        // Add initial calculations
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
        // Define possible calculations
        ObservableCollection<PivotComputationInfo> possibleCalcs = 
            new ObservableCollection<PivotComputationInfo>
            {
                new PivotComputationInfo
                {
                    CalculationName = "Total Amount",
                    FieldName = "Amount",
                    Format = "C",
                    SummaryType = SummaryType.DoubleTotalSum
                },
                new PivotComputationInfo
                {
                    CalculationName = "Avg Amount",
                    FieldName = "Amount",
                    Format = "C2",
                    SummaryType = SummaryType.DoubleAverage
                },
                new PivotComputationInfo
                {
                    CalculationName = "Total Quantity",
                    FieldName = "Quantity",
                    SummaryType = SummaryType.IntTotalSum
                },
                new PivotComputationInfo
                {
                    CalculationName = "Count",
                    FieldName = "Product",
                    SummaryType = SummaryType.Count
                }
            };
        
        pivotGrid.PossiblePivotCalculations = possibleCalcs;
        pivotGrid.RowPivotsOnly = true;
        pivotGrid.ShowPivotValueChooser = true;
    }
}
```

## Best Practices

### 1. Choose the Right Tool

**Use Field List when:**
- Users need to restore deleted fields
- Simple add/remove operations are sufficient
- Minimal UI complexity is desired

**Use Schema Designer when:**
- Excel-like experience is required
- Visual layout management is important
- Users need to see field organization clearly

**Use Value Chooser when:**
- Using RowPivotsOnly mode
- Managing multiple calculations
- Dynamic calculation selection is needed

### 2. Performance Considerations

```csharp
// For large datasets, limit possible calculations
void PivotGrid_Loaded(object sender, RoutedEventArgs e)
{
    // Only include commonly used calculations
    ObservableCollection<PivotComputationInfo> possibleCalcs = 
        new ObservableCollection<PivotComputationInfo>
        {
            // Add only 5-10 most important calculations
            // Too many options can overwhelm users and impact performance
        };
    
    pivotGrid.PossiblePivotCalculations = possibleCalcs;
}
```

### 3. User Experience

```csharp
// Provide clear field captions for duplicate fields
PivotComputationInfo totalAmount = new PivotComputationInfo
{
    FieldName = "Amount",
    FieldCaption = "Total Amount (USD)",  // Clear, descriptive caption
    Format = "C",
    SummaryType = SummaryType.DoubleTotalSum
};

PivotComputationInfo avgAmount = new PivotComputationInfo
{
    FieldName = "Amount",
    FieldCaption = "Average Amount (USD)",  // Clearly distinguish from total
    Format = "C2",
    SummaryType = SummaryType.DoubleAverage
};
```

### 4. Layout Management

```csharp
// Use appropriate grid proportions for schema designer
Grid mainGrid = new Grid();

// PivotGrid gets more space (65%)
mainGrid.ColumnDefinitions.Add(new ColumnDefinition 
{ 
    Width = new GridLength(65, GridUnitType.Star) 
});

// Schema designer gets less space (35%)
mainGrid.ColumnDefinitions.Add(new ColumnDefinition 
{ 
    Width = new GridLength(35, GridUnitType.Star) 
});
```

### 5. Initialization Order

```csharp
// Always bind data before enabling field list or schema designer
pivotGrid.ItemSource = ProductSales.GetSalesData();  // First

// Configure pivot structure
pivotGrid.PivotRows.Add(/*...*/);  // Second
pivotGrid.PivotColumns.Add(/*...*/);
pivotGrid.PivotCalculations.Add(/*...*/);

// Enable UI components
pivotGrid.ShowFieldList = true;  // Last
schemaDesigner.PivotControl = pivotGrid;
```

## Related Topics

- **Grouping Bar:** Interactive field management at the top of the pivot grid
- **Filtering:** Apply filters through field list and schema designer
- **RowPivotsOnly Mode:** Specialized layout for tabular pivot views
- **Custom Calculations:** Create calculated fields at runtime
