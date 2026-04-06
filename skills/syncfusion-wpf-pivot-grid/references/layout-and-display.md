# Layout and Display in WPF PivotGrid

This comprehensive guide covers grid layout configuration, header freezing, resizing, display modes, and layout customization options for the Syncfusion WPF PivotGrid control.

## Table of Contents

- [Grid Layout Configuration](#grid-layout-configuration)
- [Freeze Headers](#freeze-headers)
- [Grid Resizing](#grid-resizing)
- [Row Header Area Configuration](#row-header-area-configuration)
- [RowPivotsOnly Mode](#rowpivotsonly-mode)
- [Calculation Display](#calculation-display)
- [Subtotal Configuration](#subtotal-configuration)
- [Grand Total Visibility](#grand-total-visibility)
- [Layout Customization Patterns](#layout-customization-patterns)
- [Best Practices](#best-practices)

## Grid Layout Configuration

The PivotGrid supports different layout options for displaying summary data relative to value cells.

### Layout Types

- **Normal**: Displays summary data at the end of value cells
- **TopSummary**: Displays summary data at the beginning of value cells
- **ExcelLike**: Displays summaries at bottom with indented child members

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        GridLayout="TopSummary" 
        ItemSource="{Binding Source={StaticResource data}}">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                FieldHeader="Product" 
                FieldMappingName="Product" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="Date" 
                FieldMappingName="Date" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotColumns>
            <syncfusion:PivotItem 
                FieldHeader="Country" 
                FieldMappingName="Country" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="State" 
                FieldMappingName="State" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotColumns>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Amount" 
                Format="C" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Quantity" 
                SummaryType="Count" />
        </syncfusion:PivotGridControl.PivotCalculations>

    </syncfusion:PivotGridControl>
</Grid>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        PivotItem m_PivotItem = new PivotItem() 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        };
        PivotItem m_PivotItem1 = new PivotItem() 
        { 
            FieldHeader = "Date", 
            FieldMappingName = "Date", 
            TotalHeader = "Total" 
        };
        PivotItem n_PivotItem = new PivotItem() 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        };
        PivotItem n_PivotItem1 = new PivotItem() 
        { 
            FieldHeader = "State", 
            FieldMappingName = "State", 
            TotalHeader = "Total" 
        };
        
        // Adding PivotItem to PivotRows
        pivotGrid.PivotRows.Add(m_PivotItem);
        pivotGrid.PivotRows.Add(m_PivotItem1);
        
        // Adding PivotItem to PivotColumns
        pivotGrid.PivotColumns.Add(n_PivotItem);
        pivotGrid.PivotColumns.Add(n_PivotItem1);
        
        PivotComputationInfo m_PivotComputationInfo = new PivotComputationInfo() 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum 
        };
        PivotComputationInfo m_PivotComputationInfo1 = new PivotComputationInfo() 
        { 
            CalculationName = "Quantity", 
            FieldName = "Quantity", 
            SummaryType = SummaryType.Count 
        };
        
        pivotGrid.PivotCalculations.Add(m_PivotComputationInfo);
        pivotGrid.PivotCalculations.Add(m_PivotComputationInfo1);

        // Set grid layout
        pivotGrid.GridLayout = GridLayout.TopSummary;
    }
}
```

### Layout Options

**Normal Layout**: Traditional pivot table layout with totals at the end

```csharp
pivotGrid.GridLayout = GridLayout.Normal;
```

**Top Summary Layout**: Totals displayed at the beginning

```csharp
pivotGrid.GridLayout = GridLayout.TopSummary;
```

**Excel-Like Layout**: Microsoft Excel-style layout

```csharp
pivotGrid.GridLayout = GridLayout.ExcelLikeLayout;
```

## Freeze Headers

The PivotGrid provides built-in support for freezing column and row headers for better viewing of value cells.

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        FreezeHeaders="True" 
        ItemSource="{Binding Source={StaticResource data}}">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                FieldHeader="Product" 
                FieldMappingName="Product" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="Date" 
                FieldMappingName="Date" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotColumns>
            <syncfusion:PivotItem 
                FieldHeader="Country" 
                FieldMappingName="Country" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="State" 
                FieldMappingName="State" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotColumns>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Amount" 
                Format="C" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Quantity" 
                SummaryType="Count" />
        </syncfusion:PivotGridControl.PivotCalculations>
    </syncfusion:PivotGridControl>
</Grid>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure PivotRows, PivotColumns, and PivotCalculations...
        
        // Enable header freezing
        pivotGrid.FreezeHeaders = true;
    }
}
```

### Benefits of Frozen Headers

- **Improved Navigation**: Headers remain visible when scrolling
- **Better Context**: Users always see row and column identifiers
- **Enhanced Usability**: Easier data analysis with large datasets

## Grid Resizing

The PivotGrid supports automatic resizing to fit content when expanding and collapsing groups.

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        ResizePivotGridToFit="True" 
        ItemSource="{Binding Source={StaticResource data}}">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                FieldHeader="Product" 
                FieldMappingName="Product" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="Date" 
                FieldMappingName="Date" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotColumns>
            <syncfusion:PivotItem 
                FieldHeader="Country" 
                FieldMappingName="Country" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="State" 
                FieldMappingName="State" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotColumns>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Amount" 
                Format="C" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Quantity" 
                SummaryType="Count" />
        </syncfusion:PivotGridControl.PivotCalculations>

    </syncfusion:PivotGridControl>
</Grid>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure PivotRows, PivotColumns, and PivotCalculations...
        
        // Enable automatic resizing
        pivotGrid.ResizePivotGridToFit = true;
    }
}
```

## Row Header Area Configuration

Control row header area auto-sizing behavior when multiple PivotCalculation items are present.

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        AllowRowHeaderAreaAutoSizing="False" 
        ItemSource="{Binding Source={StaticResource data}}">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                FieldHeader="Product" 
                FieldMappingName="Product" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="Date" 
                FieldMappingName="Date" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotColumns>
            <syncfusion:PivotItem 
                FieldHeader="Country" 
                FieldMappingName="Country" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="State" 
                FieldMappingName="State" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotColumns>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Amount" 
                Format="C" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Quantity" 
                SummaryType="Count" />
        </syncfusion:PivotGridControl.PivotCalculations>
    </syncfusion:PivotGridControl>
</Grid>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure PivotRows, PivotColumns, and PivotCalculations...
        
        // Restrict row header area auto-sizing
        pivotGrid.AllowRowHeaderAreaAutoSizing = false;
    }
}
```

### Behavior

- **True** (default): Row headers stretch to accommodate multiple calculations
- **False**: Shows computation button (ShowFields) in data header area
- Displays pivot computation list window when clicking ShowFields button

## RowPivotsOnly Mode

RowPivotsOnly mode displays only column values, showing row and column headers as value cells.

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        Name="pivotGrid" 
        RowPivotsOnly="True">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                AllowFilter="False" 
                FieldHeader="PID" 
                FieldMappingName="PID" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                AllowFilter="False" 
                FieldHeader="Location" 
                FieldMappingName="Location" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                FieldHeader="Color" 
                FieldName="Color" 
                Format="0.0" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                FieldHeader="Class" 
                FieldName="Class" 
                Format="0.0" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                FieldHeader="Units" 
                FieldName="Units" 
                Format="0.0" 
                SummaryType="DoubleTotalSum" />
        </syncfusion:PivotGridControl.PivotCalculations>
    </syncfusion:PivotGridControl>
</Grid>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        pivotGrid.RowPivotsOnly = true;
    }
}
```

### Features in RowPivotsOnly Mode

- Displays only column values
- Row headers become value cells
- Column headers become value cells
- Supports column filtering
- Supports column sorting
- Compact data representation

## Calculation Display

Control how calculation values are displayed in the PivotGrid.

### Show Single Calculation Header

Display calculation headers even with only one PivotCalculation.

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure PivotRows, PivotColumns, and PivotCalculations...
        
        // Show single calculation header
        pivotGrid.PivotEngine.ShowSingleCalculationHeader = true;
    }
}
```

### Show Calculations as Columns

Display calculations in columns or rows using `ShowCalculationsAsColumns` property.

**XAML Configuration**:

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        FreezeHeaders="False" 
        VerticalAlignment="Top" 
        ShowCalculationsAsColumns="False" 
        ItemSource="{Binding Source={StaticResource data}}">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                FieldHeader="Product" 
                FieldMappingName="Product" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="Date" 
                FieldMappingName="Date" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotColumns>
            <syncfusion:PivotItem 
                FieldHeader="Country" 
                FieldMappingName="Country" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="State" 
                FieldMappingName="State" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotColumns>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Amount" 
                Format="C" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Quantity" 
                SummaryType="Count" />
        </syncfusion:PivotGridControl.PivotCalculations>

    </syncfusion:PivotGridControl>
</Grid>
```

**Code-Behind**:

```csharp
pivotGrid.ShowCalculationsAsColumns = false;
```

**Via Pivot Schema Designer**:
- Check/uncheck "Show Calculations as column" checkbox in Layout section

## Subtotal Configuration

Control subtotal display at various levels in the PivotGrid.

### Hide All Subtotals

**XAML**:

```xml
<Grid>
    <syncfusion:PivotGridControl 
        Name="pivotGrid" 
        ShowSubTotals="False" 
        ItemSource="{Binding Source={StaticResource data}}">
        
        <!-- PivotRows, PivotColumns, PivotCalculations -->
        
    </syncfusion:PivotGridControl>
</Grid>
```

**Code-Behind**:

```csharp
pivotGrid.ShowSubTotals = false;
```

### Hide Row Subtotals Only

**XAML**:

```xml
<syncfusion:PivotGridControl 
    Name="pivotGrid" 
    ShowRowSubTotals="False">
    <!-- Configuration -->
</syncfusion:PivotGridControl>
```

**Code-Behind**:

```csharp
pivotGrid.ShowRowSubTotals = false;
```

### Hide Column Subtotals Only

**XAML**:

```xml
<syncfusion:PivotGridControl 
    Name="pivotGrid" 
    ShowColumnSubTotals="False">
    <!-- Configuration -->
</syncfusion:PivotGridControl>
```

**Code-Behind**:

```csharp
pivotGrid.ShowColumnSubTotals = false;
```

### Hide Subtotals for Specific PivotItem

**XAML**:

```xml
<syncfusion:PivotGridControl.PivotRows>
    <syncfusion:PivotItem 
        FieldHeader="Product" 
        FieldMappingName="Product" 
        TotalHeader="Total" 
        ShowSubTotal="False" />
    <syncfusion:PivotItem 
        FieldHeader="Country" 
        FieldMappingName="Country" 
        TotalHeader="Total" />
</syncfusion:PivotGridControl.PivotRows>
```

**Code-Behind**:

```csharp
PivotItem m_PivotItem = new PivotItem() 
{ 
    FieldHeader = "Product", 
    FieldMappingName = "Product", 
    TotalHeader = "Total", 
    ShowSubTotal = false 
};
```

### Show Subtotals for Child Elements

Display subtotals based on child elements instead of parent nodes.

**XAML**:

```xml
<syncfusion:PivotGridControl 
    Name="pivotGrid" 
    ShowSubTotalsForChildren="True">
    
    <syncfusion:PivotGridControl.PivotRows>
        <syncfusion:PivotItem 
            FieldHeader="Product" 
            FieldMappingName="Product" 
            TotalHeader="Total" />
        <syncfusion:PivotItem 
            FieldHeader="Date" 
            FieldMappingName="Date" 
            TotalHeader="Total" />
        <syncfusion:PivotItem 
            FieldHeader="Country" 
            FieldMappingName="Country" 
            TotalHeader="Total" />
    </syncfusion:PivotGridControl.PivotRows>
</syncfusion:PivotGridControl>
```

**Code-Behind**:

```csharp
pivotGrid.PivotEngine.ShowSubTotalsForChildren = true;
```

## Grand Total Visibility

Control grand total row visibility, especially useful in RowPivotsOnly mode.

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        Name="pivotGrid" 
        GrandTotalRowAlwaysVisible="True" 
        RowPivotsOnly="True">
        
        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                AllowFilter="False" 
                FieldHeader="PID" 
                FieldMappingName="PID" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                AllowFilter="False" 
                FieldHeader="Location" 
                FieldMappingName="Location" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                FieldHeader="Color" 
                FieldName="Color" 
                Format="0.0" 
                SummaryType="DoubleTotalSum" />
        </syncfusion:PivotGridControl.PivotCalculations>
    </syncfusion:PivotGridControl>
</Grid>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        pivotGrid.GrandTotalRowAlwaysVisible = true;
    }
}
```

## Layout Customization Patterns

### Combining Layout Options

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        
        // Configure data source
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure PivotRows, PivotColumns, and PivotCalculations...
        
        // Apply multiple layout options
        pivotGrid.GridLayout = GridLayout.TopSummary;
        pivotGrid.FreezeHeaders = true;
        pivotGrid.ResizePivotGridToFit = true;
        pivotGrid.ShowSubTotals = true;
        pivotGrid.ShowCalculationsAsColumns = true;
    }
}
```

### Dynamic Layout Switching

```csharp
public void SwitchLayout(GridLayout layout)
{
    pivotGrid.GridLayout = layout;
    pivotGrid.RefreshGrid();
}

public void ToggleFreezeHeaders()
{
    pivotGrid.FreezeHeaders = !pivotGrid.FreezeHeaders;
    pivotGrid.RefreshGrid();
}

public void ToggleSubtotals()
{
    pivotGrid.ShowSubTotals = !pivotGrid.ShowSubTotals;
    pivotGrid.RefreshGrid();
}
```

### Conditional Layout Configuration

```csharp
public void ConfigureLayoutForDataSize(int rowCount, int columnCount)
{
    if (rowCount > 1000 || columnCount > 50)
    {
        // Large dataset configuration
        pivotGrid.FreezeHeaders = true;
        pivotGrid.ResizePivotGridToFit = false;
        pivotGrid.ShowSubTotals = false; // Reduce visual complexity
    }
    else
    {
        // Standard configuration
        pivotGrid.FreezeHeaders = false;
        pivotGrid.ResizePivotGridToFit = true;
        pivotGrid.ShowSubTotals = true;
    }
}
```

### User Preference Persistence

```csharp
public class LayoutPreferences
{
    public GridLayout Layout { get; set; }
    public bool FreezeHeaders { get; set; }
    public bool ShowSubTotals { get; set; }
    public bool ShowCalculationsAsColumns { get; set; }
    
    public void ApplyTo(PivotGridControl grid)
    {
        grid.GridLayout = Layout;
        grid.FreezeHeaders = FreezeHeaders;
        grid.ShowSubTotals = ShowSubTotals;
        grid.ShowCalculationsAsColumns = ShowCalculationsAsColumns;
    }
    
    public static LayoutPreferences FromGrid(PivotGridControl grid)
    {
        return new LayoutPreferences
        {
            Layout = grid.GridLayout,
            FreezeHeaders = grid.FreezeHeaders,
            ShowSubTotals = grid.ShowSubTotals,
            ShowCalculationsAsColumns = grid.ShowCalculationsAsColumns
        };
    }
}
```

## Best Practices

### Layout Selection

1. **Normal Layout**: Use for traditional pivot table users
2. **TopSummary Layout**: Better for top-down data analysis
3. **ExcelLike Layout**: Familiar to Microsoft Excel users

### Header Freezing

1. **Enable for Large Datasets**: Always freeze headers with >20 rows/columns
2. **Disable for Simple Grids**: Small grids don't need frozen headers
3. **Performance**: Frozen headers have minimal performance impact

### Resizing Strategy

1. **Dynamic Content**: Enable `ResizePivotGridToFit` for frequently changing data
2. **Fixed Layout**: Disable for stable reports
3. **User Experience**: Consider animation and transition effects

### Row Header Configuration

1. **Multiple Calculations**: Set `AllowRowHeaderAreaAutoSizing` to false
2. **Single Calculation**: Keep default (true) for better space utilization
3. **Custom Sizing**: Implement custom sizing logic if needed

### RowPivotsOnly Usage

1. **Tabular Reports**: Ideal for flat, table-like displays
2. **Column Analysis**: Focus on column-based comparisons
3. **Simplified Views**: Reduce complexity for end users

### Calculation Display

1. **Multiple Calculations**: Show as columns for comparison
2. **Single Calculation**: Show header for clarity
3. **Space Optimization**: Toggle based on available screen space

### Subtotal Management

1. **Detailed Analysis**: Show all subtotals
2. **Summary Views**: Hide lower-level subtotals
3. **Performance**: Hiding subtotals improves rendering speed
4. **User Control**: Allow users to toggle subtotal visibility

### Grand Total Visibility

1. **RowPivotsOnly Mode**: Always make grand totals visible
2. **Normal Mode**: Use default behavior
3. **Conditional Display**: Show/hide based on user role or report type

### Performance Considerations

```csharp
// Optimize for large datasets
public void OptimizeForLargeDataset()
{
    pivotGrid.FreezeHeaders = true;
    pivotGrid.ResizePivotGridToFit = false;
    pivotGrid.ShowSubTotals = false;
    pivotGrid.AllowRowHeaderAreaAutoSizing = false;
}

// Optimize for small datasets
public void OptimizeForSmallDataset()
{
    pivotGrid.FreezeHeaders = false;
    pivotGrid.ResizePivotGridToFit = true;
    pivotGrid.ShowSubTotals = true;
    pivotGrid.AllowRowHeaderAreaAutoSizing = true;
}
```

### Responsive Layout

```csharp
public void AdaptToWindowSize(double width, double height)
{
    if (width < 800)
    {
        // Compact layout for small screens
        pivotGrid.ShowCalculationsAsColumns = false;
        pivotGrid.ShowSubTotals = false;
    }
    else
    {
        // Full layout for larger screens
        pivotGrid.ShowCalculationsAsColumns = true;
        pivotGrid.ShowSubTotals = true;
    }
}
```

## Summary

Layout and display configuration in the WPF PivotGrid provides:

- **Grid Layout Options**: Normal, TopSummary, and ExcelLike layouts
- **Header Freezing**: Keep headers visible during scrolling
- **Automatic Resizing**: Fit grid to content dynamically
- **Row Header Control**: Manage header area auto-sizing
- **RowPivotsOnly Mode**: Simplified column-focused display
- **Calculation Display**: Control header and column/row positioning
- **Subtotal Configuration**: Granular control over subtotal visibility
- **Grand Total Control**: Always-visible grand totals option
- **Customization Patterns**: Combine options for optimal user experience
- **Best Practices**: Performance, usability, and responsive design guidelines

Choose layout options based on your data structure, user requirements, and performance needs to create an optimal PivotGrid experience.
