# Filtering in WPF PivotGrid

## Table of Contents
- [Overview](#overview)
- [FilterExpression Class](#filterexpression-class)
- [Defining Filters in XAML](#defining-filters-in-xaml)
- [Defining Filters in Code-Behind](#defining-filters-in-code-behind)
- [Filter Pop-Up UI](#filter-pop-up-ui)
- [Column Filtering (RowPivotsOnly Mode)](#column-filtering-rowpivotsonly-mode)
- [Excel-Like Filtering](#excel-like-filtering)
- [Filter Operators and Expressions](#filter-operators-and-expressions)
- [Multiple Filter Conditions](#multiple-filter-conditions)

## Overview

Filtering in PivotGrid allows you to display only a subset of data that meets specified criteria. Filters are:
- **Additive:** Each additional filter builds on the current filter
- **Persistent:** Filters are automatically reapplied when the grid refreshes
- **Multiple:** Apply 'n' number of filtering conditions simultaneously

## FilterExpression Class

The `FilterExpression` class encapsulates filter definitions with these properties:

| Property | Type | Description |
|----------|------|-------------|
| **Expression** | string | Logical expression defining the filter |
| **Name** | string | Name of the FilterExpression |
| **DimensionName** | string | Dimension name for the filter |
| **DimensionHeader** | string | Display header for the dimension |
| **Format** | string | Format string for the FilterExpression |
| **Evaluator** | Func | Custom evaluator for the filter value |

## Defining Filters in XAML

Add `FilterExpression` objects to the `PivotGridControl.Filters` collection:

```xml
<syncfusion:PivotGridControl Name="pivotGrid" 
                            ShowFieldList="True"
                            ItemSource="{Binding Source={StaticResource data}}">
    
    <!-- Define PivotRows and PivotColumns -->
    <syncfusion:PivotGridControl.PivotRows>
        <syncfusion:PivotItem FieldHeader="Product" 
                             FieldMappingName="Product" 
                             TotalHeader="Total" />
        <syncfusion:PivotItem FieldHeader="Date" 
                             FieldMappingName="Date" 
                             TotalHeader="Total" />
    </syncfusion:PivotGridControl.PivotRows>
    
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
    
    <!-- Define Filters -->
    <syncfusion:PivotGridControl.Filters>
        <syncfusion:FilterExpression DimensionHeader="Product" 
                                    DimensionName="Product" 
                                    Name="ProductFilter" 
                                    Expression="Product = 'Bike'" />
    </syncfusion:PivotGridControl.Filters>
</syncfusion:PivotGridControl>
```

## Defining Filters in Code-Behind

Create and add `FilterExpression` objects programmatically:

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;

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
        
        // Configure PivotItems
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product" 
        });
        
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Date", 
            FieldMappingName = "Date" 
        });
        
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country" 
        });
        
        pivotGrid.PivotColumns.Add(new PivotItem 
        { 
            FieldHeader = "State", 
            FieldMappingName = "State" 
        });
        
        // Configure Calculations
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
        
        // Create and Add Filter
        FilterExpression productFilter = new FilterExpression
        {
            DimensionHeader = "Product",
            DimensionName = "Product",
            Name = "ProductFilter",
            Expression = "Product = 'Bike'"
        };
        
        pivotGrid.Filters.Add(productFilter);
        
        // Bind Data
        pivotGrid.ItemSource = ProductSales.GetSalesData();
    }
}
```

## Filter Pop-Up UI

Enable interactive filtering through the grouping bar:

### Enabling Filter Pop-Up

```csharp
pivotGrid.ShowGroupingBar = true;
```

### Using Filter Pop-Up

1. Click the filter button on any grouping bar item
2. The filter pop-up displays all available values
3. Uncheck items you want to filter out
4. Click OK to apply the filter

The PivotGrid automatically creates a `FilterExpression` from the unchecked items and applies the filter.

## Column Filtering (RowPivotsOnly Mode)

When `RowPivotsOnly` is enabled, you can filter calculation columns.

### Enabling Column Filtering

```csharp
// Define PivotCalculation with AllowFilter
PivotComputationInfo costCalc = new PivotComputationInfo
{
    FieldName = "Cost",
    FieldHeader = "Cost",
    AllowFilter = true  // Enable filtering for this calculation
};

pivotGrid.PivotCalculations.Add(costCalc);
```

### Applying Saved Value Filter

```csharp
void pivotGrid_Loaded(object sender, RoutedEventArgs e)
{
    // Create dictionary with filter values
    Dictionary<string, HashSet<string>> filterDictionary = 
        new Dictionary<string, HashSet<string>>();
    
    // Add values to filter (only these values will be shown)
    filterDictionary.Add("Cost", new HashSet<string>() { "701", "230" });
    
    // Apply the filter
    pivotGrid.InternalGrid.ApplySavedValueFilter(filterDictionary);
}
```

## Excel-Like Filtering

Enable advanced Excel-style filtering with sorting capabilities.

### Enabling Excel-Like Filtering

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        pivotGrid.GroupingBar.Loaded += GroupingBar_Loaded;
    }
    
    void GroupingBar_Loaded(object sender, RoutedEventArgs e)
    {
        // Enable Excel-like filtering
        pivotGrid.GroupingBar.AllowMultiFunctionalSortFilter = true;
    }
}
```

### Excel-Like Filter Features

**Sorting Options:**
- **Sort A to Z:** Ascending order
- **Sort Z to A:** Descending order

**Filter Management:**
- **Clear Filters:** Remove all filters from the pivot item

**Label Filters:**
- Equals / Does not equal
- Greater than / Greater than or equal to
- Less than / Less than or equal to
- Begins with / Does not begin with
- Ends with / Does not end with
- Contains / Does not contain
- Between / Not between

**Value Filters:**
- Equals / Does not equal
- Greater than / Greater than or equal to
- Less than / Less than or equal to
- Between / Not between
- Top 10

### Label Filter Example

```
Label Filter for "State" field:
- Select "Begins with"
- Enter "A"
- Result: Shows only states beginning with "A" (Alberta, etc.)
```

### Value Filter Example

```
Value Filter for "State" field:
- Select "Greater than"
- Field: Quantity
- Value: 500
- Result: Shows only states where Quantity > 500
```

## Filter Operators and Expressions

### Basic Comparison Operators

```csharp
// Equality
Expression = "Product = 'Bike'"
Expression = "Country = 'Canada'"

// Inequality
Expression = "Product != 'Car'"

// Numeric Comparisons
Expression = "Amount > 1000"
Expression = "Quantity >= 5"
Expression = "Price < 500"
Expression = "Discount <= 0.1"
```

### Logical Operators

```csharp
// AND condition
Expression = "Product = 'Bike' AND Country = 'Canada'"

// OR condition
Expression = "Product = 'Bike' OR Product = 'Car'"

// Complex conditions
Expression = "(Product = 'Bike' OR Product = 'Car') AND Country = 'Canada'"
```

### String Operators

```csharp
// Contains
Expression = "State LIKE '%Alberta%'"

// Starts with
Expression = "Product LIKE 'Bi%'"

// Ends with
Expression = "State LIKE '%ia'"
```

### IN Operator

```csharp
// Multiple values
Expression = "Product IN ('Bike', 'Car', 'Truck')"
Expression = "State IN ('Alberta', 'Ontario')"
```

## Multiple Filter Conditions

Apply multiple filters simultaneously:

### XAML Multiple Filters

```xml
<syncfusion:PivotGridControl.Filters>
    <!-- Filter by Product -->
    <syncfusion:FilterExpression DimensionName="Product" 
                                Expression="Product = 'Bike'" />
    
    <!-- Filter by Country -->
    <syncfusion:FilterExpression DimensionName="Country" 
                                Expression="Country = 'Canada'" />
    
    <!-- Filter by Amount -->
    <syncfusion:FilterExpression DimensionName="Amount" 
                                Expression="Amount > 50000" />
</syncfusion:PivotGridControl.Filters>
```

### Code-Behind Multiple Filters

```csharp
// Filter 1: Product
pivotGrid.Filters.Add(new FilterExpression
{
    DimensionName = "Product",
    Name = "ProductFilter",
    Expression = "Product = 'Bike'"
});

// Filter 2: Country
pivotGrid.Filters.Add(new FilterExpression
{
    DimensionName = "Country",
    Name = "CountryFilter",
    Expression = "Country = 'Canada'"
});

// Filter 3: Date Range
pivotGrid.Filters.Add(new FilterExpression
{
    DimensionName = "Date",
    Name = "DateFilter",
    Expression = "Date IN ('FY 2006', 'FY 2007')"
});
```

## Dynamic Filtering

### Adding Filters at Runtime

```csharp
private void ApplyFilter_Click(object sender, RoutedEventArgs e)
{
    string selectedProduct = productComboBox.SelectedItem.ToString();
    
    // Remove existing product filter
    var existingFilter = pivotGrid.Filters
        .FirstOrDefault(f => f.DimensionName == "Product");
    if (existingFilter != null)
    {
        pivotGrid.Filters.Remove(existingFilter);
    }
    
    // Add new filter
    FilterExpression newFilter = new FilterExpression
    {
        DimensionName = "Product",
        Name = "ProductFilter",
        Expression = $"Product = '{selectedProduct}'"
    };
    
    pivotGrid.Filters.Add(newFilter);
}
```

### Removing Filters

```csharp
// Remove specific filter
private void RemoveFilter(string dimensionName)
{
    var filter = pivotGrid.Filters
        .FirstOrDefault(f => f.DimensionName == dimensionName);
    
    if (filter != null)
    {
        pivotGrid.Filters.Remove(filter);
    }
}

// Clear all filters
private void ClearAllFilters_Click(object sender, RoutedEventArgs e)
{
    pivotGrid.Filters.Clear();
}
```

## Best Practices

### 1. Filter at Data Source

For large datasets, filter at the database level before binding:

```csharp
// Filter in SQL query
string query = "SELECT * FROM Sales WHERE Product = 'Bike'";
// Then bind filtered data to PivotGrid
```

### 2. Use Appropriate Expressions

Match filter expressions to data types:
```csharp
// String (use quotes)
Expression = "Product = 'Bike'"

// Numeric (no quotes)
Expression = "Amount > 1000"

// Date (format appropriately)
Expression = "OrderDate >= '2023-01-01'"
```

### 3. Test Complex Filters

Complex filters can impact performance:
```csharp
// Simple (fast)
Expression = "Product = 'Bike'"

// Complex (slower)
Expression = "(Product = 'Bike' OR Product = 'Car') AND (Amount > 1000 OR Quantity > 5)"
```

### 4. Validate Filter Expressions

```csharp
try
{
    FilterExpression filter = new FilterExpression
    {
        DimensionName = "Product",
        Expression = userInputExpression
    };
    pivotGrid.Filters.Add(filter);
}
catch (Exception ex)
{
    MessageBox.Show($"Invalid filter expression: {ex.Message}");
}
```

## Common Issues

### Issue: Filter Not Applied

**Solutions:**
1. Verify `DimensionName` matches `FieldMappingName` exactly
2. Check expression syntax (quotes for strings, no quotes for numbers)
3. Ensure data source contains the filtered values

### Issue: Filter Clears on Refresh

**Solution:** Filters should persist automatically. If not:
```csharp
// Store filters before refresh
var savedFilters = pivotGrid.Filters.ToList();

// Refresh data
pivotGrid.ItemSource = GetNewData();

// Reapply filters if needed
foreach (var filter in savedFilters)
{
    if (!pivotGrid.Filters.Contains(filter))
    {
        pivotGrid.Filters.Add(filter);
    }
}
```

### Issue: Excel-Like Filtering Not Showing

**Solution:**
1. Ensure `ShowGroupingBar = true`
2. Set `AllowMultiFunctionalSortFilter = true` in GroupingBar_Loaded event
3. Verify grouping bar is loaded before setting property

## Related Topics

- **Sorting:** Combine filtering with sorting for better data organization
- **Grouping Bar:** Interactive UI for filtering
- **Data Binding:** Optimize filtering with proper data sources
