---
name: syncfusion-wpf-pivot-grid
description: Comprehensive guide for implementing Syncfusion WPF PivotGrid control, a powerful pivot table component for cross-tabulated data analysis. Use this when working with WPF pivot tables, PivotGridControl, or cross-tabulated data scenarios. Covers pivot rows/columns configuration, PivotSchemaDesigner, grouping bar, pivot field list, pivot calculations, filtering, sorting, and data binding for business intelligence applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF PivotGrid

The Syncfusion WPF PivotGrid is a powerful cell-oriented, extensible grid control that simulates Microsoft Excel's pivot table feature. It pivots, summarizes, and groups relational data to organize it in a cross-tabulated form, ideal for financial domains and business data analysis.

## When to Use This Skill

Use this skill when you need to:
- **Implement pivot tables** in WPF applications for data analysis
- **Organize and summarize** large datasets in cross-tabulated format
- **Configure pivot rows, columns, and calculations** for multi-dimensional data views
- **Enable interactive data exploration** with grouping bar and field list
- **Create business intelligence dashboards** with pivot analysis
- **Add filtering and sorting** to pivot data
- **Implement custom calculations** and expression fields
- **Export or print** pivot grid data
- **Build financial reports** with dynamic data summarization
- **Enable drag-and-drop pivoting** for end-user flexibility

## Component Overview

The PivotGrid control provides comprehensive pivot table functionality:

- **Data Pivoting:** Organize data by rows, columns, and values
- **Interactive UI:** Grouping bar, field list, schema designer, value chooser
- **Data Operations:** Filtering, sorting (including multi-column and by values)
- **Calculations:** Built-in summary types, custom calculations, expression fields
- **Editing:** Cell-level editing and updating
- **Customization:** Context menus, layout options, freeze headers
- **Performance:** Virtualization and asynchronous data loading
- **Export/Import:** Data export, print support, state serialization

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly references
- Adding PivotGrid through Designer, XAML, or code-behind
- License configuration
- Basic PivotGrid setup and first render
- Minimal working example

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding to List collections
- Binding to DataTable and DataView
- Asynchronous data loading for large datasets
- ItemSource configuration patterns
- Data source best practices

### Core Concepts: PivotItems and Calculations
📄 **Read:** [references/pivot-items-and-calculations.md](references/pivot-items-and-calculations.md)
- Understanding PivotItem (rows and columns)
- PivotComputationInfo for value fields
- Summary types (Count, Sum, Average, Min, Max, etc.)
- FieldHeader, FieldMappingName, TotalHeader properties
- Format strings for calculations
- Basic calculation examples

### Filtering
📄 **Read:** [references/filtering.md](references/filtering.md)
- FilterExpression class overview
- Defining filters in XAML and code-behind
- Column filtering
- Excel-like filtering UI
- Multiple filter conditions
- Filter expression syntax

### Sorting
📄 **Read:** [references/sorting.md](references/sorting.md)
- Default sorting behavior
- Custom comparers for sort logic
- Column sorting
- Multi-column sorting
- Sorting by values
- Sorting events and customization

### Grouping Bar
📄 **Read:** [references/grouping-bar.md](references/grouping-bar.md)
- Enabling and configuring grouping bar
- Defining grouping bar properties
- Drag-and-drop pivoting functionality
- Grouping bar context menu
- Customization and styling

### Field List and Designer
📄 **Read:** [references/field-list-and-designer.md](references/field-list-and-designer.md)
- PivotGrid Field List
- PivotSchemaDesigner for advanced configuration
- Binding PivotSchemaDesigner to PivotGrid
- Pivot Value Chooser
- Setting captions in field list
- Field management and customization

### Custom Calculations
📄 **Read:** [references/custom-calculations.md](references/custom-calculations.md)
- Custom calculation overview
- Runtime calculated fields
- Runtime custom summaries
- Expression fields for formulas
- Advanced calculation patterns
- Custom summary implementations

### Editing and Updating
📄 **Read:** [references/editing-and-updating.md](references/editing-and-updating.md)
- Enabling cell editing
- Updating cell values
- Edit events and validation
- Data modification patterns

### Context Menus
📄 **Read:** [references/context-menus.md](references/context-menus.md)
- Main PivotGrid context menu
- Grouping bar context menu
- Header cell context menu
- Customizing menu items
- Menu actions and events

### Layout and Display Options
📄 **Read:** [references/layout-and-display.md](references/layout-and-display.md)
- Grid layout configuration
- Freeze headers for fixed row/column headers
- Auto-resizing to fit content
- Restricting row header area resizing
- RowPivotsOnly mode
- Showing calculations as columns
- Show/hide sub-totals and grand totals
- Layout customization patterns

### Selection
📄 **Read:** [references/selection.md](references/selection.md)
- Selection modes
- Cell and range selection
- Selection events
- Programmatic selection

### Exporting and Printing
📄 **Read:** [references/exporting-and-printing.md](references/exporting-and-printing.md)
- Exporting PivotGrid data
- Supported export formats
- Print functionality and print preview
- Export configuration options

### Serialization and State Persistence
📄 **Read:** [references/serialization.md](references/serialization.md)
- Serializing PivotGrid configuration
- Deserialization
- State persistence across sessions
- Saving and loading PivotGrid state

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Hyperlink cells
- Inner-most computations
- GetRawItem method for raw data access
- Performance improvements with virtualization
- Advanced scenarios and edge cases

## Quick Start Example

### Basic PivotGrid with Sales Data

**XAML:**
```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourNamespace"
        Title="PivotGrid Example" Height="600" Width="800">
    <Window.Resources>
        <ObjectDataProvider x:Key="salesData" 
                          ObjectType="{x:Type local:ProductSales}" 
                          MethodName="GetSalesData" />
    </Window.Resources>
    
    <Grid>
        <syncfusion:PivotGridControl Name="pivotGrid" 
                                    ItemSource="{Binding Source={StaticResource salesData}}">
            <!-- Define Pivot Rows -->
            <syncfusion:PivotGridControl.PivotRows>
                <syncfusion:PivotItem FieldHeader="Product" 
                                     FieldMappingName="Product" 
                                     TotalHeader="Total" />
                <syncfusion:PivotItem FieldHeader="Date" 
                                     FieldMappingName="Date" 
                                     TotalHeader="Total" />
            </syncfusion:PivotGridControl.PivotRows>
            
            <!-- Define Pivot Columns -->
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
                <syncfusion:PivotComputationInfo CalculationName="Total Amount" 
                                                FieldName="Amount" 
                                                Format="C" 
                                                SummaryType="DoubleTotalSum" />
                <syncfusion:PivotComputationInfo CalculationName="Quantity" 
                                                FieldName="Quantity" 
                                                SummaryType="Count" />
            </syncfusion:PivotGridControl.PivotCalculations>
        </syncfusion:PivotGridControl>
    </Grid>
</Window>
```

**Code-behind (ProductSales.cs):**
```csharp
using System;
using System.Collections.Generic;

public class ProductSales
{
    public string Product { get; set; }
    public string Date { get; set; }
    public string Country { get; set; }
    public string State { get; set; }
    public int Quantity { get; set; }
    public double Amount { get; set; }

    public static List<ProductSales> GetSalesData()
    {
        return new List<ProductSales>
        {
            new ProductSales { Product = "Bike", Date = "FY 2005", Country = "Canada", 
                             State = "Alberta", Quantity = 5, Amount = 150000 },
            new ProductSales { Product = "Car", Date = "FY 2006", Country = "Canada", 
                             State = "Ontario", Quantity = 3, Amount = 90000 },
            // Add more data...
        };
    }
}
```

## Common Patterns

### Pattern 1: PivotGrid with Grouping Bar
Enable interactive drag-and-drop pivoting:

```xml
<syncfusion:PivotGridControl ShowGroupingBar="True">
    <!-- PivotItems and Calculations -->
</syncfusion:PivotGridControl>
```

### Pattern 2: Code-Behind Configuration
Define PivotGrid programmatically:

```csharp
PivotGridControl pivotGrid = new PivotGridControl();

// Add Pivot Rows
pivotGrid.PivotRows.Add(new PivotItem 
{ 
    FieldHeader = "Product", 
    FieldMappingName = "Product", 
    TotalHeader = "Total" 
});

// Add Pivot Columns
pivotGrid.PivotColumns.Add(new PivotItem 
{ 
    FieldHeader = "Country", 
    FieldMappingName = "Country", 
    TotalHeader = "Total" 
});

// Add Calculations
pivotGrid.PivotCalculations.Add(new PivotComputationInfo 
{ 
    CalculationName = "Total", 
    FieldName = "Amount", 
    Format = "C", 
    SummaryType = SummaryType.DoubleTotalSum 
});

// Set ItemSource
pivotGrid.ItemSource = ProductSales.GetSalesData();
```

### Pattern 3: Filtering Data
Apply filters to narrow data display:

```csharp
FilterExpression filter = new FilterExpression
{
    DimensionHeader = "Product",
    DimensionName = "Product",
    Name = "ProductFilter",
    Expression = "Product = 'Bike'"
};
pivotGrid.Filters.Add(filter);
```

### Pattern 4: Custom Sorting
Use custom comparers for specific sort logic:

```csharp
public class ReverseOrderComparer : IComparer
{
    public int Compare(object x, object y)
    {
        return -x.ToString().CompareTo(y.ToString());
    }
}

// Apply to PivotItem
PivotItem item = new PivotItem 
{ 
    FieldHeader = "Product", 
    FieldMappingName = "Product",
    Comparer = new ReverseOrderComparer() 
};
```

## Key Properties

### PivotGridControl Properties
- **ItemSource**: Data source for the pivot grid
- **PivotRows**: Collection of PivotItems for row dimensions
- **PivotColumns**: Collection of PivotItems for column dimensions
- **PivotCalculations**: Collection of PivotComputationInfo for value calculations
- **Filters**: Collection of FilterExpression for filtering data
- **ShowGroupingBar**: Enable/disable grouping bar
- **ShowFieldList**: Enable/disable field list

### PivotItem Properties
- **FieldHeader**: Display header text
- **FieldMappingName**: Property name to bind from data source
- **TotalHeader**: Header text for total row/column
- **Comparer**: Custom comparer for sorting

### PivotComputationInfo Properties
- **CalculationName**: Display name for the calculation
- **FieldName**: Property name to calculate
- **SummaryType**: Aggregation type (Count, Sum, Average, etc.)
- **Format**: Display format string (e.g., "C" for currency)

## Common Use Cases

### Financial Reporting
Create comprehensive financial reports with multi-dimensional analysis of revenue, expenses, and profitability by product, region, and time period.

### Sales Analysis
Analyze sales performance across products, territories, and time periods with dynamic pivoting and drill-down capabilities.

### Business Intelligence Dashboards
Build interactive BI dashboards allowing users to explore data through drag-and-drop pivoting with grouping bar and field list.

### Inventory Management
Track inventory levels, movements, and costs across warehouses, products, and time periods.

### Budget Analysis
Compare budgets vs. actuals across departments, cost centers, and fiscal periods with variance calculations.

## Related Components
- **PivotSchemaDesigner**: Advanced configuration interface
- **PivotGrid Field List**: Interactive field management
- **Grouping Bar**: Drag-and-drop pivoting interface

## Assembly Requirements
 
Required assemblies for WPF PivotGrid:
- `Syncfusion.Grid.Wpf`
- `Syncfusion.GridCommon.Wpf`
- `Syncfusion.Linq.Base`
- `Syncfusion.PivotAnalysis.Base`
- `Syncfusion.PivotAnalysis.Wpf`
- `Syncfusion.Shared.Wpf`

### NuGet Package Installation

- Important: Use version * (latest available).

Install via NuGet Package Manager:

```powershell
Install-Package Syncfusion.PivotTable.wpf
```

Or via .NET CLI:

```bash
dotnet add package Syncfusion.PivotTable.wpf
```

The NuGet package automatically includes all required dependencies.



## Namespace Imports

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
```

---

**Navigate to the appropriate reference file above for detailed implementation guidance on specific features.**
