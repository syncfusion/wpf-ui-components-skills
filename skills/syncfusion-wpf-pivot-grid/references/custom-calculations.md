# Custom Calculations in WPF PivotGrid

This guide covers custom calculations, runtime calculated fields, custom summaries, expression fields, and advanced calculation patterns for the Syncfusion WPF PivotGrid control.

## Table of Contents

- [Custom Calculations Overview](#custom-calculations-overview)
- [Calculation Types](#calculation-types)
- [Using Custom Calculations](#using-custom-calculations)
- [Runtime Calculated Fields](#runtime-calculated-fields)
- [Runtime Custom Summary](#runtime-custom-summary)
- [Expression Fields](#expression-fields)
- [Formula-Based Calculations](#formula-based-calculations)
- [Advanced Calculation Patterns](#advanced-calculation-patterns)
- [Best Practices](#best-practices)

## Custom Calculations Overview

Custom calculations in the WPF PivotGrid allow you to define how pivot values are computed and displayed. The `CalculationType` property in the `PivotComputationInfo` class determines the type of calculation applied to your data.

## Calculation Types

The WPF PivotGrid supports the following calculation types through the `CalculationType` enumerator:

### Basic Calculations

- **NoCalculation**: Removes custom calculations and displays original values (default)
- **Index**: Displays value cells as an index value based on PivotEngine generation

### Percentage Calculations

- **PercentageOfGrandTotal**: Displays a value cell as a percentage of the grand total
- **PercentageOfColumnTotal**: Displays values as a percentage of their column total
- **PercentageOfRowTotal**: Displays values as a percentage of their row total
- **PercentageOfParentColumnTotal**: Displays a value as a percentage of parent column item
- **PercentageOfParentRowTotal**: Displays a value as a percentage of parent row item
- **PercentageOfParentTotal**: Displays a value as a percentage of base field parent total

### Comparative Calculations

- **PercentageOf**: Displays values as a percentage of the base item value
- **DifferenceFrom**: Displays values as the difference from the base item value
- **PercentageOfDifferenceFrom**: Displays values as the percentage difference from the base item

### Running Calculations

- **RunningTotalIn**: Displays successive items as a running total
- **PercentageOfRunningTotalIn**: Displays running totals as percentages

### Ranking Calculations

- **RankSmallestToLargest**: Ranks values with smallest as 1
- **RankLargestToSmallest**: Ranks values with largest as 1

### Special Calculations

- **Formula**: Applies algebraic expression-based calculations
- **Distinct**: Displays subtotals based on distinct values of BaseItem

## Using Custom Calculations

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
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
                SummaryType="DoubleTotalSum" 
                CalculationType="PercentageOfColumnTotal" />
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
        
        // Configure PivotRows
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
        
        pivotGrid.PivotRows.Add(m_PivotItem);
        pivotGrid.PivotRows.Add(m_PivotItem1);
        
        // Configure PivotColumns
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
        
        pivotGrid.PivotColumns.Add(n_PivotItem);
        pivotGrid.PivotColumns.Add(n_PivotItem1);
        
        // Configure Calculations with CalculationType
        PivotComputationInfo m_PivotComputationInfo = new PivotComputationInfo() 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum, 
            CalculationType = CalculationType.PercentageOfColumnTotal 
        };
        
        PivotComputationInfo m_PivotComputationInfo1 = new PivotComputationInfo() 
        { 
            CalculationName = "Quantity", 
            FieldName = "Quantity", 
            SummaryType = SummaryType.Count 
        };
        
        pivotGrid.PivotCalculations.Add(m_PivotComputationInfo);
        pivotGrid.PivotCalculations.Add(m_PivotComputationInfo1);
    }
}
```

## Runtime Calculated Fields

Runtime calculated fields allow users to create new fields based on existing calculation items using the calculated field window available through the grouping bar context menu.

### Creating Runtime Calculated Fields

1. **Open Calculated Field Window**: Access through the grouping bar context menu
2. **Define Field Name**: Provide a unique name for the calculated field
3. **Enter Formula**: Use the Fields section to insert calculation fields and operators
4. **Add and Apply**: Click "Add" to create the field and "OK" to populate the grid

### Formula Entry

The formula can be entered by:
- Inserting calculation fields from the **Fields** section
- Using the formula pop-up for numerical operators (+, -, *, /)
- Combining multiple fields with operators

**Example Formula**: `[Amount] / [Quantity]`

## Runtime Custom Summary

The WPF PivotGrid enables custom summaries for PivotItem values at both load time and runtime using the pivot computation info dialog.

### Creating Custom Summary Class

Create a custom summary by inheriting from the `SummaryBase` abstract class:

```csharp
public class MyCustomSummaryBase1 : SummaryBase
{
    private double mTotalValue;
    
    public MyCustomSummaryBase1() {}

    public override void Combine(object other)
    {
        mTotalValue += (double)other;
    }

    public override void CombineSummary(SummaryBase other)
    {
        MyCustomSummaryBase1 dpsb = other as MyCustomSummaryBase1;
        if (null != dpsb)
        {
            mTotalValue += dpsb.mTotalValue;
        }
    }

    public override SummaryBase GetInstance()
    {
        return new MyCustomSummaryBase1();
    }

    public override object GetResult()
    {
        return mTotalValue / 3.33333;
    }

    public override void Reset()
    {
        mTotalValue = 0;
    }
}
```

### Define Custom Summary at Load Time

**XAML Configuration**:

```xml
<Window 
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
    xmlns:local="clr-namespace:WpfApplication1">
    
    <Window.Resources>
        <ResourceDictionary>
            <ObjectDataProvider 
                x:Key="data" 
                ObjectType="{x:Type local:ProductSales}" 
                MethodName="GetSalesData" />
            <local:MyCustomSummaryBase1 x:Key="summary1" />
        </ResourceDictionary>
    </Window.Resources>
    
    <Grid Name="grid1">
        <syncfusion:PivotGridControl 
            HorizontalAlignment="Left" 
            Name="pivotGrid" 
            VerticalAlignment="Top" 
            ItemSource="{Binding Source={StaticResource data}}">
            
            <syncfusion:PivotGridControl.PivotCalculations>
                <syncfusion:PivotComputationInfo 
                    CalculationName="Total" 
                    FieldName="Amount" 
                    Format="C" 
                    SummaryType="Custom" 
                    Summary="{StaticResource summary1}" />
            </syncfusion:PivotGridControl.PivotCalculations>
        </syncfusion:PivotGridControl>
    </Grid>
</Window>
```

**Code-Behind**:

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Add PivotRows and PivotColumns...
        
        PivotComputationInfo m_PivotComputationInfo = new PivotComputationInfo() 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.Custom, 
            Summary = new MyCustomSummaryBase1() 
        };
        
        pivotGrid.PivotCalculations.Add(m_PivotComputationInfo);
    }
}
```

### Define Custom Summary at Runtime

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.pivotGrid.Loaded += pivotGrid_Loaded;
    }

    void pivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Add Custom SummaryBases to CustomSummaryBaseCollection
        this.pivotSchemaDesigner.CustomSummaryBaseCollection = 
            new ObservableCollection<SummaryBase>
            {
                new MyCustomSummaryBase1(),
                new MyCustomSummaryBase2()
            };
    }
}
```

Users can select the custom summaries from the "Summarize value by" combo box in the pivot computation information dialog.

## Expression Fields

Expression fields allow you to add new fields using expressions and handle them like normal fields in the PivotGrid.

### Enabling Expression Fields

Set the `LoadWithDefaultPropertyFields` property to enable expression field support:

**XAML**:

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        LoadWithDefaultPropertyFields="True"  
        ItemSource="{Binding Source={StaticResource data}}">
        
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
pivotGrid.LoadWithDefaultPropertyFields = true;
```

### Creating Expression Fields

Use the `FieldInfo` class to create expression fields:

**XAML**:

```xml
<syncfusion:PivotGridControl 
    Name="pivotGrid" 
    LoadWithDefaultPropertyFields="True">
    
    <syncfusion:PivotGridControl.AllowedFields>
        <syncfusion:FieldInfo 
            Name="UnitPrice" 
            Expression="[Amount] + [Quantity]" 
            FieldType="Expression" />
    </syncfusion:PivotGridControl.AllowedFields>
    
    <syncfusion:PivotGridControl.PivotCalculations>
        <syncfusion:PivotComputationInfo 
            CalculationName="Total" 
            Description="Summation of values" 
            FieldName="UnitPrice"      
            FieldHeader="Unit Price" 
            Format="C" 
            SummaryType="DoubleTotalSum"/>
    </syncfusion:PivotGridControl.PivotCalculations>
</syncfusion:PivotGridControl>
```

**Code-Behind**:

```csharp
FieldInfo field1 = new FieldInfo 
{ 
    Name = "UnitPrice", 
    Expression = "[Amount] + [Quantity]", 
    FieldType = FieldTypes.Expression 
};

pivotGrid.AllowedFields.Add(field1);

PivotComputationInfo m_PivotComputationInfo2 = new PivotComputationInfo() 
{ 
    CalculationName = "UnitPrice", 
    FieldName = "Unit Price", 
    Format = "C", 
    SummaryType = SummaryType.DoubleTotalSum 
};

pivotGrid.PivotCalculations.Add(m_PivotComputationInfo2);
```

## Formula-Based Calculations

Formula-based calculations allow you to create computed fields using algebraic expressions.

### Using Formula Calculation Type

**XAML**:

```xml
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
    <syncfusion:PivotComputationInfo 
        CalculationName="Total" 
        FieldName="UnitPrice" 
        CalculationType="Formula" 
        Formula="[Amount] / [Quantity]" />
</syncfusion:PivotGridControl.PivotCalculations>
```

**Code-Behind**:

```csharp
PivotComputationInfo m_PivotComputationInfo2 = new PivotComputationInfo() 
{ 
    CalculationName = "Total", 
    FieldName = "UnitPrice", 
    CalculationType = CalculationType.Formula, 
    Formula = "[Amount] / [Quantity]" 
};

pivotGrid.PivotCalculations.Add(m_PivotComputationInfo2);
```

### Supported Formula Operators

- Addition: `+`
- Subtraction: `-`
- Multiplication: `*`
- Division: `/`
- Parentheses: `()` for grouping

**Example Formulas**:
- Simple division: `[Amount] / [Quantity]`
- Complex calculation: `([Amount] * 1.1) - [Discount]`
- Multiple operations: `[Price] * [Quantity] + [Tax]`

## Advanced Calculation Patterns

### Combining Multiple Calculation Types

You can combine different calculation types in a single PivotGrid:

```csharp
// Percentage of grand total
PivotComputationInfo percentageCalc = new PivotComputationInfo() 
{ 
    CalculationName = "Percentage", 
    FieldName = "Amount", 
    CalculationType = CalculationType.PercentageOfGrandTotal 
};

// Running total
PivotComputationInfo runningCalc = new PivotComputationInfo() 
{ 
    CalculationName = "Running", 
    FieldName = "Amount", 
    CalculationType = CalculationType.RunningTotalIn 
};

// Formula-based
PivotComputationInfo formulaCalc = new PivotComputationInfo() 
{ 
    CalculationName = "Average", 
    FieldName = "AvgPrice", 
    CalculationType = CalculationType.Formula, 
    Formula = "[Amount] / [Quantity]" 
};
```

### Custom Summary with Multiple Instances

```csharp
public class WeightedAverageSummary : SummaryBase
{
    private double totalValue;
    private double totalWeight;

    public override void Combine(object other)
    {
        if (other is KeyValuePair<double, double> pair)
        {
            totalValue += pair.Key * pair.Value;
            totalWeight += pair.Value;
        }
    }

    public override void CombineSummary(SummaryBase other)
    {
        if (other is WeightedAverageSummary was)
        {
            totalValue += was.totalValue;
            totalWeight += was.totalWeight;
        }
    }

    public override object GetResult()
    {
        return totalWeight > 0 ? totalValue / totalWeight : 0;
    }

    public override SummaryBase GetInstance()
    {
        return new WeightedAverageSummary();
    }

    public override void Reset()
    {
        totalValue = 0;
        totalWeight = 0;
    }
}
```

### Conditional Calculations

```csharp
public class ConditionalSummary : SummaryBase
{
    private double total;
    private int count;
    private double threshold;

    public ConditionalSummary(double threshold = 1000)
    {
        this.threshold = threshold;
    }

    public override void Combine(object other)
    {
        double value = Convert.ToDouble(other);
        if (value >= threshold)
        {
            total += value;
            count++;
        }
    }

    public override void CombineSummary(SummaryBase other)
    {
        if (other is ConditionalSummary cs)
        {
            total += cs.total;
            count += cs.count;
        }
    }

    public override object GetResult()
    {
        return count > 0 ? total / count : 0;
    }

    public override SummaryBase GetInstance()
    {
        return new ConditionalSummary(threshold);
    }

    public override void Reset()
    {
        total = 0;
        count = 0;
    }
}
```

## Best Practices

### Calculation Type Selection

1. **Use PercentageOfGrandTotal** for overall distribution analysis
2. **Use RunningTotalIn** for cumulative trends over time
3. **Use Formula** for custom business logic calculations
4. **Use Custom summaries** for complex aggregations not supported by built-in types

### Performance Considerations

1. **Limit Formula Complexity**: Keep formulas simple for better performance
2. **Cache Expensive Calculations**: Store intermediate results when possible
3. **Optimize Custom Summaries**: Minimize operations in `Combine()` method
4. **Use Built-in Types**: Prefer built-in calculation types when possible

### Expression Field Guidelines

1. **Name Clearly**: Use descriptive names for expression fields
2. **Document Formulas**: Add comments explaining complex expressions
3. **Validate Expressions**: Test expressions with sample data before deployment
4. **Handle Null Values**: Ensure expressions handle null or missing data gracefully

### Custom Summary Design

1. **Override All Methods**: Implement all required abstract methods properly
2. **Reset State**: Always reset state in `Reset()` method
3. **Thread Safety**: Consider thread safety if using in multi-threaded scenarios
4. **Type Safety**: Validate type conversions in `Combine()` method

### Testing Strategies

```csharp
// Test custom calculations with unit tests
[TestMethod]
public void TestCustomSummary()
{
    var summary = new MyCustomSummaryBase1();
    summary.Combine(100.0);
    summary.Combine(200.0);
    
    double result = (double)summary.GetResult();
    Assert.AreEqual(90.0, result, 0.01); // 300 / 3.33333
}
```

### Error Handling

```csharp
public override void Combine(object other)
{
    try
    {
        if (other == null)
            return;
            
        double value = Convert.ToDouble(other);
        mTotalValue += value;
    }
    catch (InvalidCastException)
    {
        // Log error or handle appropriately
    }
}
```

### Documentation

Document custom calculations:

```csharp
/// <summary>
/// Custom summary that calculates weighted average.
/// </summary>
/// <remarks>
/// Formula: Sum(Value * Weight) / Sum(Weight)
/// Use Case: Product pricing with volume discounts
/// </remarks>
public class WeightedAverageSummary : SummaryBase
{
    // Implementation...
}
```

## Summary

Custom calculations in the WPF PivotGrid provide powerful tools for data analysis:

- **Built-in Calculation Types** offer common analysis patterns
- **Runtime Calculated Fields** enable user-driven field creation
- **Custom Summaries** support complex aggregation logic
- **Expression Fields** allow formula-based field definitions
- **Formula Calculations** provide flexible computation options
- **Advanced Patterns** combine multiple techniques for sophisticated analysis

Choose the appropriate calculation approach based on your requirements, performance needs, and user interaction patterns.
