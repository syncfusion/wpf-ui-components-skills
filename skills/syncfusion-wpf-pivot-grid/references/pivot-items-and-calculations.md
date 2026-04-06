# PivotItems and Calculations

## Table of Contents
- [PivotItem Overview](#pivotitem-overview)
- [PivotItem Properties](#pivotitem-properties)
- [Defining PivotItems](#defining-pivotitems)
- [PivotComputationInfo Overview](#pivotcomputationinfo-overview)
- [PivotComputationInfo Properties](#pivotcomputationinfo-properties)
- [Defining PivotCalculations](#defining-pivotcalculations)
- [Summary Types](#summary-types)
- [Common Patterns](#common-patterns)

## PivotItem Overview

A **PivotItem** is a container that defines dimensional fields in the PivotGrid. Each PivotItem represents a field category (like Product, Date, Country) that can be placed in either rows or columns to organize data hierarchically.

**Key Concepts:**
- **PivotRows:** Items that define row dimensions (vertical grouping)
- **PivotColumns:** Items that define column dimensions (horizontal grouping)
- Items can be nested to create multi-level hierarchies
- Each item provides grouping and aggregation boundaries

## PivotItem Properties

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| **FieldHeader** | string | Display header text for the pivot item |
| **FieldMappingName** | string | Property name from data source to bind |
| **TotalHeader** | string | Header text for summary/total cells |

### Configuration Properties

| Property | Type | Description |
|----------|------|-------------|
| **Comparer** | IComparer | Custom comparer for sorting (null uses default) |
| **Format** | string | Format string for display (e.g., "C", "N2") |
| **ShowSubTotal** | bool | Show or hide subtotals for this item |
| **AllowRunTimeGroupByField** | bool | Enable/disable grouping in Field List |
| **AllowFilter** | bool | Enable/disable filtering for this item |
| **AllowSort** | bool | Enable/disable sorting for this item |

## Defining PivotItems

### Method 1: XAML Definition

```xml
<syncfusion:PivotGridControl Name="pivotGrid" 
                            ItemSource="{Binding Source={StaticResource data}}">
    
    <!-- Define Row Dimensions -->
    <syncfusion:PivotGridControl.PivotRows>
        <syncfusion:PivotItem FieldHeader="Product" 
                             FieldMappingName="Product" 
                             TotalHeader="Total" />
        <syncfusion:PivotItem FieldHeader="Date" 
                             FieldMappingName="Date" 
                             TotalHeader="Total" />
    </syncfusion:PivotGridControl.PivotRows>
    
    <!-- Define Column Dimensions -->
    <syncfusion:PivotGridControl.PivotColumns>
        <syncfusion:PivotItem FieldHeader="Country" 
                             FieldMappingName="Country" 
                             TotalHeader="Total" />
        <syncfusion:PivotItem FieldHeader="State" 
                             FieldMappingName="State" 
                             TotalHeader="Total" />
    </syncfusion:PivotGridControl.PivotColumns>
</syncfusion:PivotGridControl>
```

### Method 2: Code-Behind Definition

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;

public void ConfigurePivotItems()
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    // Create Row PivotItems
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
    
    // Create Column PivotItems
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
    
    // Add to PivotRows
    pivotGrid.PivotRows.Add(productItem);
    pivotGrid.PivotRows.Add(dateItem);
    
    // Add to PivotColumns
    pivotGrid.PivotColumns.Add(countryItem);
    pivotGrid.PivotColumns.Add(stateItem);
}
```

### Advanced PivotItem Configuration

```csharp
// PivotItem with all properties
PivotItem advancedItem = new PivotItem
{
    FieldHeader = "Region",
    FieldMappingName = "RegionName",
    TotalHeader = "Regional Total",
    ShowSubTotal = true,
    AllowFilter = true,
    AllowSort = true,
    AllowRunTimeGroupByField = true,
    Format = "G"  // General format
};
```

## PivotComputationInfo Overview

**PivotComputationInfo** defines the value fields and calculations that appear in PivotGrid cells. Each computation specifies:
- Which field to aggregate
- How to aggregate it (Sum, Count, Average, etc.)
- How to display the result (format string)

## PivotComputationInfo Properties

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| **CalculationName** | string | Display name for the calculation |
| **FieldName** | string | Property name from data source |
| **SummaryType** | SummaryType | Aggregation method (Sum, Count, etc.) |
| **Format** | string | Display format (e.g., "C", "N2", "P") |

### Advanced Properties

| Property | Type | Description |
|----------|------|-------------|
| **Description** | string | Description of the calculation |
| **FieldHeader** | string | Alternative header text |
| **Summary** | SummaryBase | Custom summary object for SummaryType.Custom |
| **AllowRunTimeGroupByField** | bool | Enable/disable grouping |
| **DisplayOption** | enum | How to display calculation values |
| **CalculationType** | enum | How to make computations visible |
| **AllowFilter** | bool | Enable filtering (RowPivotsOnly mode) |
| **AllowSort** | bool | Enable sorting (RowPivotsOnly mode) |
| **BaseItem** | string | Base item for calculations |
| **BaseField** | string | Base field for calculations |
| **EnableHyperlinks** | bool | Enable hyperlinks (RowPivotsOnly mode) |
| **InnerMostComputationsOnly** | bool | Show results only at innermost level |
| **PadString** | string | Pad string for DisplayIfDiscreteValuesEqual |

## Defining PivotCalculations

### Method 1: XAML Definition

```xml
<syncfusion:PivotGridControl Name="pivotGrid">
    <syncfusion:PivotGridControl.PivotCalculations>
        
        <!-- Sum of Amount with Currency Format -->
        <syncfusion:PivotComputationInfo 
            CalculationName="Total Amount" 
            FieldName="Amount" 
            SummaryType="DoubleTotalSum"
            Format="C" />
        
        <!-- Count of Quantity -->
        <syncfusion:PivotComputationInfo 
            CalculationName="Item Count" 
            FieldName="Quantity" 
            SummaryType="Count" />
        
        <!-- Average Price -->
        <syncfusion:PivotComputationInfo 
            CalculationName="Average Price" 
            FieldName="Price" 
            SummaryType="DoubleAverage"
            Format="C2" />
            
    </syncfusion:PivotGridControl.PivotCalculations>
</syncfusion:PivotGridControl>
```

### Method 2: Code-Behind Definition

```csharp
using Syncfusion.PivotAnalysis.Base;

public void ConfigureCalculations()
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    // Sum calculation
    PivotComputationInfo sumAmount = new PivotComputationInfo
    {
        CalculationName = "Total Amount",
        FieldName = "Amount",
        SummaryType = SummaryType.DoubleTotalSum,
        Format = "C"
    };
    
    // Count calculation
    PivotComputationInfo countQuantity = new PivotComputationInfo
    {
        CalculationName = "Item Count",
        FieldName = "Quantity",
        SummaryType = SummaryType.Count
    };
    
    // Average calculation
    PivotComputationInfo avgPrice = new PivotComputationInfo
    {
        CalculationName = "Average Price",
        FieldName = "Price",
        SummaryType = SummaryType.DoubleAverage,
        Format = "C2"
    };
    
    // Add to PivotCalculations
    pivotGrid.PivotCalculations.Add(sumAmount);
    pivotGrid.PivotCalculations.Add(countQuantity);
    pivotGrid.PivotCalculations.Add(avgPrice);
}
```

## Summary Types

The `SummaryType` enumeration defines how data is aggregated:

### Numeric Aggregations

| Summary Type | Description | Use Case |
|--------------|-------------|----------|
| **DoubleTotalSum** | Sum of double/integer values | Total revenue, total sales |
| **DoubleAverage** | Average of double/integer values | Average price, average quantity |
| **DoubleMaximum** | Maximum double/integer value | Highest sale, peak value |
| **DoubleMinimum** | Minimum double/integer value | Lowest price, minimum stock |
| **DoubleStandardDeviation** | Standard deviation | Data variability analysis |
| **DoubleVariance** | Variance of values | Statistical analysis |

### Type-Specific Aggregations

| Summary Type | Description |
|--------------|-------------|
| **DecimalTotalSum** | Sum of decimal values |
| **IntTotalSum** | Sum of integer values |
| **Count** | Count of items |

### Special Summary Types

| Summary Type | Description | Use Case |
|--------------|-------------|----------|
| **Custom** | Use custom SummaryBase object | Complex calculations |
| **DisplayIfDiscreteValuesEqual** | Display if all values are equal | Consistency checking |

### XAML Examples

```xml
<!-- Sum -->
<syncfusion:PivotComputationInfo CalculationName="Total Sales" 
                                FieldName="Amount" 
                                SummaryType="DoubleTotalSum" 
                                Format="C" />

<!-- Average -->
<syncfusion:PivotComputationInfo CalculationName="Avg Quantity" 
                                FieldName="Quantity" 
                                SummaryType="DoubleAverage" 
                                Format="N2" />

<!-- Count -->
<syncfusion:PivotComputationInfo CalculationName="Transaction Count" 
                                FieldName="TransactionId" 
                                SummaryType="Count" />

<!-- Min/Max -->
<syncfusion:PivotComputationInfo CalculationName="Max Price" 
                                FieldName="Price" 
                                SummaryType="DoubleMaximum" 
                                Format="C" />
```

### Code-Behind Examples

```csharp
// Sum
pivotGrid.PivotCalculations.Add(new PivotComputationInfo
{
    CalculationName = "Total Sales",
    FieldName = "Amount",
    SummaryType = SummaryType.DoubleTotalSum,
    Format = "C"
});

// Average
pivotGrid.PivotCalculations.Add(new PivotComputationInfo
{
    CalculationName = "Avg Quantity",
    FieldName = "Quantity",
    SummaryType = SummaryType.DoubleAverage,
    Format = "N2"
});

// Standard Deviation
pivotGrid.PivotCalculations.Add(new PivotComputationInfo
{
    CalculationName = "Price Variation",
    FieldName = "Price",
    SummaryType = SummaryType.DoubleStandardDeviation,
    Format = "N4"
});
```

## DisplayIfDiscreteValuesEqual Summary

This special summary type displays the aggregated value only if all values in the group are identical. Otherwise, it displays a pad string.

### XAML Configuration

```xml
<syncfusion:PivotComputationInfo 
    CalculationName="Product Code" 
    FieldName="Code" 
    SummaryType="DisplayIfDiscreteValuesEqual" 
    PadString="***" />
```

### Code-Behind Configuration

```csharp
PivotComputationInfo codeCalc = new PivotComputationInfo
{
    CalculationName = "Product Code",
    FieldName = "Code",
    SummaryType = SummaryType.DisplayIfDiscreteValuesEqual,
    PadString = "***"  // Displayed when values differ
};
pivotGrid.PivotCalculations.Add(codeCalc);
```

**Result:**
- If all items in a group have Code = "ABC123" → displays "ABC123"
- If items have different codes → displays "***"

## Common Patterns

### Pattern 1: Complete PivotGrid Configuration

```csharp
public void SetupPivotGrid()
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    // Configure Rows
    pivotGrid.PivotRows.Add(new PivotItem 
    { 
        FieldHeader = "Product", 
        FieldMappingName = "Product" 
    });
    
    pivotGrid.PivotRows.Add(new PivotItem 
    { 
        FieldHeader = "Year", 
        FieldMappingName = "Year" 
    });
    
    // Configure Columns
    pivotGrid.PivotColumns.Add(new PivotItem 
    { 
        FieldHeader = "Region", 
        FieldMappingName = "Region" 
    });
    
    // Configure Multiple Calculations
    pivotGrid.PivotCalculations.Add(new PivotComputationInfo
    {
        CalculationName = "Revenue",
        FieldName = "Amount",
        SummaryType = SummaryType.DoubleTotalSum,
        Format = "C0"
    });
    
    pivotGrid.PivotCalculations.Add(new PivotComputationInfo
    {
        CalculationName = "Units Sold",
        FieldName = "Quantity",
        SummaryType = SummaryType.IntTotalSum,
        Format = "N0"
    });
    
    pivotGrid.PivotCalculations.Add(new PivotComputationInfo
    {
        CalculationName = "Avg Unit Price",
        FieldName = "UnitPrice",
        SummaryType = SummaryType.DoubleAverage,
        Format = "C2"
    });
    
    // Bind data
    pivotGrid.ItemSource = GetSalesData();
}
```

### Pattern 2: Financial Dashboard

```csharp
// Revenue and cost analysis
pivotGrid.PivotCalculations.Add(new PivotComputationInfo
{
    CalculationName = "Total Revenue",
    FieldName = "Revenue",
    SummaryType = SummaryType.DecimalTotalSum,
    Format = "C0"
});

pivotGrid.PivotCalculations.Add(new PivotComputationInfo
{
    CalculationName = "Total Cost",
    FieldName = "Cost",
    SummaryType = SummaryType.DecimalTotalSum,
    Format = "C0"
});

pivotGrid.PivotCalculations.Add(new PivotComputationInfo
{
    CalculationName = "Margin %",
    FieldName = "MarginPercent",
    SummaryType = SummaryType.DoubleAverage,
    Format = "P2"
});
```

### Pattern 3: Conditional Subtotal Display

```csharp
// Show subtotals only for specific dimensions
PivotItem productItem = new PivotItem
{
    FieldHeader = "Product",
    FieldMappingName = "Product",
    ShowSubTotal = true  // Show product subtotals
};

PivotItem categoryItem = new PivotItem
{
    FieldHeader = "Category",
    FieldMappingName = "Category",
    ShowSubTotal = false  // Hide category subtotals
};
```

## Format Strings

Common format strings for calculations:

| Format | Description | Example Output |
|--------|-------------|----------------|
| **"C"** | Currency | $1,234.56 |
| **"C0"** | Currency, no decimals | $1,235 |
| **"N2"** | Number, 2 decimals | 1,234.56 |
| **"P"** | Percentage | 12.35% |
| **"P0"** | Percentage, no decimals | 12% |
| **"#,##0"** | Thousand separator | 1,235 |
| **"0.00"** | Fixed decimals | 1234.56 |

## Field Mapping Rules

**Critical:** `FieldMappingName` must match the property name in your data source **exactly** (case-sensitive).

```csharp
// Data Model
public class Sales
{
    public string ProductName { get; set; }  // Property name
    public double TotalAmount { get; set; }
}

// Correct Mapping
new PivotItem 
{ 
    FieldMappingName = "ProductName"  // Exact match ✓
}

// Incorrect Mappings
new PivotItem 
{ 
    FieldMappingName = "productname"  // Wrong case ✗
}
new PivotItem 
{ 
    FieldMappingName = "Product Name"  // Wrong format ✗
}
```

## Related Topics

- **Data Binding:** Learn how to connect data sources
- **Filtering:** Filter data by PivotItem values
- **Sorting:** Sort PivotItems with custom comparers
- **Custom Calculations:** Create advanced calculation formulas
