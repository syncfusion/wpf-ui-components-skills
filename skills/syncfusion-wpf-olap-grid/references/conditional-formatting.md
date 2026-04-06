# Conditional Formatting

## Table of Contents
- [Conditional Formatting Overview](#conditional-formatting-overview)
- [OlapGridDataConditionalFormat Class](#olapgriddataconditionalformat-class)
- [Condition Specification](#condition-specification)
- [Cell Style Customization](#cell-style-customization)
- [Multiple Conditions](#multiple-conditions)
- [XAML Configuration](#xaml-configuration)
- [Code-Behind Configuration](#code-behind-configuration)
- [Examples and Patterns](#examples-and-patterns)

## Conditional Formatting Overview

Conditional formatting allows you to format grid cells based on data conditions. Cells meeting specified criteria are automatically styled with custom backgrounds, fonts, colors, and borders.

**Use Cases:**
- Highlight exceptional values (sales above/below targets)
- Color-code performance metrics (green for good, red for poor)
- Draw attention to outliers or anomalies
- Create heat maps based on value ranges
- Emphasize key business indicators

**How it Works:**
1. Define condition(s) to filter cells
2. Specify cell style to apply
3. Grid automatically applies formatting to matching cells
4. Formatting updates when data changes

## OlapGridDataConditionalFormat Class

The main class for defining conditional formats.

### Basic Structure

```csharp
OlapGridDataConditionalFormat format = new OlapGridDataConditionalFormat();

// Add conditions
format.Conditions.Add(...);

// Set style for matching cells
format.CellStyle = new OlapGridCellStyle() { ... };

// Apply to grid
olapGrid1.ConditionalFormats.Add(format);
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | string | Optional identifier for the format |
| `Conditions` | Collection | OlapGridDataCondition objects defining filter criteria |
| `CellStyle` | OlapGridCellStyle | Style applied to cells meeting conditions |

## Condition Specification

### OlapGridDataCondition Class

Defines filter criteria for which cells to format.

**Key Properties:**
- **ConditionType** - Comparison operator (GreaterThan, LessThan, Equals, etc.)
- **MeasureElement** - Which measure to evaluate
- **Value** - Threshold value for comparison
- **PredicateType** - How to combine with other conditions (And, Or)

### ConditionType Enumeration

```csharp
public enum OlapGridDataConditionType
{
    GreaterThan,      // > value
    LessThan,         // < value
    Equals,           // == value
    NotEquals,        // != value
    GreaterThanOrEqual,  // >= value
    LessThanOrEqual,  // <= value
    Between           // min <= x <= max
}
```

### PredicateType Enumeration

```csharp
public enum PredicateType
{
    And,  // All conditions must be true
    Or    // Any condition can be true
}
```

### Simple Condition Example

```csharp
OlapGridDataCondition condition = new OlapGridDataCondition()
{
    ConditionType = OlapGridDataConditionType.GreaterThan,
    MeasureElement = "Internet Sales Amount",
    Value = "2000000",
    PredicateType = PredicateType.Or
};
```

**Effect:** Cells with Internet Sales Amount > 2,000,000 will match.

## Cell Style Customization

### OlapGridCellStyle Class

Defines visual appearance for matching cells.

**Available Properties:**
- **Background** - Cell background brush/color
- **Foreground** - Text color
- **FontFamily** - Font family name
- **FontSize** - Font size in points
- **FontWeight** - Bold, normal, etc.
- **FontStyle** - Italic, normal
- **BorderBrush** - Border color
- **BorderThickness** - Border width

### Style Example

```csharp
OlapGridCellStyle style = new OlapGridCellStyle()
{
    Background = Brushes.Yellow,
    Foreground = Brushes.DarkRed,
    FontFamily = new FontFamily("Calibri"),
    FontSize = 12,
    FontWeight = FontWeights.Bold
};
```

### Common Style Patterns

**High Values (Good Performance):**
```csharp
new OlapGridCellStyle()
{
    Background = Brushes.LightGreen,
    Foreground = Brushes.DarkGreen,
    FontWeight = FontWeights.Bold
}
```

**Low Values (Poor Performance):**
```csharp
new OlapGridCellStyle()
{
    Background = Brushes.LightCoral,
    Foreground = Brushes.DarkRed,
    FontWeight = FontWeights.Bold
}
```

**Warning/Attention:**
```csharp
new OlapGridCellStyle()
{
    Background = Brushes.LightYellow,
    Foreground = Brushes.DarkOrange,
    BorderBrush = Brushes.Orange,
    BorderThickness = new Thickness(2)
}
```

## Multiple Conditions

Combine multiple conditions to create complex criteria.

### AND Logic (All Must Be True)

```csharp
OlapGridDataConditionalFormat format = new OlapGridDataConditionalFormat();

// Condition 1: Greater than 2M
format.Conditions.Add(new OlapGridDataCondition()
{
    ConditionType = OlapGridDataConditionType.GreaterThan,
    MeasureElement = "Internet Sales Amount",
    Value = "2000000",
    PredicateType = PredicateType.And  // AND with next condition
});

// Condition 2: Less than 5M
format.Conditions.Add(new OlapGridDataCondition()
{
    ConditionType = OlapGridDataConditionType.LessThan,
    MeasureElement = "Internet Sales Amount",
    Value = "5000000",
    PredicateType = PredicateType.And
});

// Result: Cells with 2M < value < 5M will match
```

### OR Logic (Any Can Be True)

```csharp
OlapGridDataConditionalFormat format = new OlapGridDataConditionalFormat();

// Condition 1: Less than 1M (poor sales)
format.Conditions.Add(new OlapGridDataCondition()
{
    ConditionType = OlapGridDataConditionType.LessThan,
    MeasureElement = "Sales Amount",
    Value = "1000000",
    PredicateType = PredicateType.Or  // OR with next condition
});

// Condition 2: Greater than 10M (excellent sales)
format.Conditions.Add(new OlapGridDataCondition()
{
    ConditionType = OlapGridDataConditionType.GreaterThan,
    MeasureElement = "Sales Amount",
    Value = "10000000",
    PredicateType = PredicateType.Or
});

// Result: Cells with value < 1M OR value > 10M will match
```

## XAML Configuration

Define conditional formats declaratively in XAML.

### Basic XAML Example

```xaml
<syncfusion:OlapGrid Name="olapGrid1">
    <syncfusion:OlapGrid.ConditionalFormats>
        
        <!-- Format 1: High Sales -->
        <syncfusion:OlapGridDataConditionalFormat Name="HighSales">
            
            <!-- Cell Style -->
            <syncfusion:OlapGridDataConditionalFormat.CellStyle>
                <syncfusion:OlapGridCellStyle 
                    Background="Yellow" 
                    FontFamily="Calibri" 
                    FontSize="12"
                    FontWeight="Bold"/>
            </syncfusion:OlapGridDataConditionalFormat.CellStyle>
            
            <!-- Conditions -->
            <syncfusion:OlapGridDataConditionalFormat.Conditions>
                <syncfusion:OlapGridDataCondition 
                    ConditionType="GreaterThan" 
                    Value="2000000" 
                    MeasureElement="Internet Sales Amount" 
                    PredicateType="And"/>
                <syncfusion:OlapGridDataCondition 
                    ConditionType="LessThan" 
                    Value="5000000" 
                    MeasureElement="Internet Sales Amount" 
                    PredicateType="And"/>
            </syncfusion:OlapGridDataConditionalFormat.Conditions>
            
        </syncfusion:OlapGridDataConditionalFormat>
        
    </syncfusion:OlapGrid.ConditionalFormats>
</syncfusion:OlapGrid>
```

### Multiple Formats in XAML

```xaml
<syncfusion:OlapGrid.ConditionalFormats>
    
    <!-- High Values (Green) -->
    <syncfusion:OlapGridDataConditionalFormat Name="HighValues">
        <syncfusion:OlapGridDataConditionalFormat.CellStyle>
            <syncfusion:OlapGridCellStyle Background="LightGreen" Foreground="DarkGreen"/>
        </syncfusion:OlapGridDataConditionalFormat.CellStyle>
        <syncfusion:OlapGridDataConditionalFormat.Conditions>
            <syncfusion:OlapGridDataCondition 
                ConditionType="GreaterThan" 
                Value="5000000" 
                MeasureElement="Sales Amount" 
                PredicateType="Or"/>
        </syncfusion:OlapGridDataConditionalFormat.Conditions>
    </syncfusion:OlapGridDataConditionalFormat>
    
    <!-- Low Values (Red) -->
    <syncfusion:OlapGridDataConditionalFormat Name="LowValues">
        <syncfusion:OlapGridDataConditionalFormat.CellStyle>
            <syncfusion:OlapGridCellStyle Background="LightCoral" Foreground="DarkRed"/>
        </syncfusion:OlapGridDataConditionalFormat.CellStyle>
        <syncfusion:OlapGridDataConditionalFormat.Conditions>
            <syncfusion:OlapGridDataCondition 
                ConditionType="LessThan" 
                Value="1000000" 
                MeasureElement="Sales Amount" 
                PredicateType="Or"/>
        </syncfusion:OlapGridDataConditionalFormat.Conditions>
    </syncfusion:OlapGridDataConditionalFormat>
    
</syncfusion:OlapGrid.ConditionalFormats>
```

## Code-Behind Configuration

Configure conditional formatting programmatically for dynamic scenarios.

### Complete C# Example

```csharp
public void ApplyConditionalFormatting()
{
    // Create format
    OlapGridDataConditionalFormat format = new OlapGridDataConditionalFormat();
    format.Name = "HighSales";
    
    // Add conditions
    format.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.GreaterThan,
        MeasureElement = "Internet Sales Amount",
        Value = "2000000",
        PredicateType = PredicateType.Or
    });
    
    format.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.LessThan,
        MeasureElement = "Internet Sales Amount",
        Value = "5000000",
        PredicateType = PredicateType.And
    });
    
    // Set cell style
    format.CellStyle = new OlapGridCellStyle()
    {
        Background = Brushes.Yellow,
        FontFamily = new FontFamily("Calibri"),
        FontSize = 12,
        FontWeight = FontWeights.Bold
    };
    
    // Apply to grid
    olapGrid1.ConditionalFormats.Add(format);
    
    // Rebind to apply formatting
    olapGrid1.DataBind();
}
```

### VB.NET Example

```vbnet
Public Sub ApplyConditionalFormatting()
    ' Create format
    Dim format As OlapGridDataConditionalFormat = New OlapGridDataConditionalFormat()
    format.Name = "HighSales"
    
    ' Add conditions
    format.Conditions.Add(New OlapGridDataCondition() With {
        .ConditionType = OlapGridDataConditionType.GreaterThan,
        .MeasureElement = "Internet Sales Amount",
        .Value = "2000000",
        .PredicateType = PredicateType.Or
    })
    
    format.Conditions.Add(New OlapGridDataCondition() With {
        .ConditionType = OlapGridDataConditionType.LessThan,
        .MeasureElement = "Internet Sales Amount",
        .Value = "5000000",
        .PredicateType = PredicateType.And
    })
    
    ' Set cell style
    format.CellStyle = New OlapGridCellStyle() With {
        .Background = Brushes.Yellow,
        .FontFamily = New FontFamily("Calibri"),
        .FontSize = 12,
        .FontWeight = FontWeights.Bold
    }
    
    ' Apply to grid
    olapGrid1.ConditionalFormats.Add(format)
    olapGrid1.DataBind()
End Sub
```

## Examples and Patterns

### Pattern 1: Traffic Light Formatting

Color-code cells as Red (poor), Yellow (average), Green (good).

```csharp
public void ApplyTrafficLightFormatting()
{
    // Red: Poor performance (< 1M)
    OlapGridDataConditionalFormat redFormat = new OlapGridDataConditionalFormat();
    redFormat.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.LessThan,
        MeasureElement = "Sales Amount",
        Value = "1000000",
        PredicateType = PredicateType.Or
    });
    redFormat.CellStyle = new OlapGridCellStyle()
    {
        Background = Brushes.LightCoral,
        Foreground = Brushes.DarkRed,
        FontWeight = FontWeights.Bold
    };
    olapGrid1.ConditionalFormats.Add(redFormat);
    
    // Yellow: Average performance (1M - 3M)
    OlapGridDataConditionalFormat yellowFormat = new OlapGridDataConditionalFormat();
    yellowFormat.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.GreaterThanOrEqual,
        MeasureElement = "Sales Amount",
        Value = "1000000",
        PredicateType = PredicateType.And
    });
    yellowFormat.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.LessThan,
        MeasureElement = "Sales Amount",
        Value = "3000000",
        PredicateType = PredicateType.And
    });
    yellowFormat.CellStyle = new OlapGridCellStyle()
    {
        Background = Brushes.LightYellow,
        Foreground = Brushes.DarkGoldenrod
    };
    olapGrid1.ConditionalFormats.Add(yellowFormat);
    
    // Green: Good performance (>= 3M)
    OlapGridDataConditionalFormat greenFormat = new OlapGridDataConditionalFormat();
    greenFormat.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.GreaterThanOrEqual,
        MeasureElement = "Sales Amount",
        Value = "3000000",
        PredicateType = PredicateType.Or
    });
    greenFormat.CellStyle = new OlapGridCellStyle()
    {
        Background = Brushes.LightGreen,
        Foreground = Brushes.DarkGreen,
        FontWeight = FontWeights.Bold
    };
    olapGrid1.ConditionalFormats.Add(greenFormat);
    
    olapGrid1.DataBind();
}
```

### Pattern 2: Heat Map

Create a gradual heat map based on value ranges.

```csharp
public void ApplyHeatMap()
{
    // Very Low
    AddRangeFormat(0, 500000, Brushes.AliceBlue, Brushes.Blue);
    
    // Low
    AddRangeFormat(500000, 1000000, Brushes.LightCyan, Brushes.DarkCyan);
    
    // Medium
    AddRangeFormat(1000000, 2000000, Brushes.LightYellow, Brushes.DarkOrange);
    
    // High
    AddRangeFormat(2000000, 5000000, Brushes.LightSalmon, Brushes.DarkRed);
    
    // Very High
    AddConditionalFormat(OlapGridDataConditionType.GreaterThanOrEqual, "5000000", 
        Brushes.Red, Brushes.White);
    
    olapGrid1.DataBind();
}

private void AddRangeFormat(double min, double max, Brush background, Brush foreground)
{
    OlapGridDataConditionalFormat format = new OlapGridDataConditionalFormat();
    format.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.GreaterThanOrEqual,
        MeasureElement = "Sales Amount",
        Value = min.ToString(),
        PredicateType = PredicateType.And
    });
    format.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.LessThan,
        MeasureElement = "Sales Amount",
        Value = max.ToString(),
        PredicateType = PredicateType.And
    });
    format.CellStyle = new OlapGridCellStyle()
    {
        Background = background,
        Foreground = foreground
    };
    olapGrid1.ConditionalFormats.Add(format);
}
```

### Pattern 3: Dynamic Thresholds

Calculate thresholds based on data (e.g., average, percentile).

```csharp
public void ApplyDynamicFormatting(double averageSales)
{
    // Above average
    OlapGridDataConditionalFormat aboveAvg = new OlapGridDataConditionalFormat();
    aboveAvg.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.GreaterThan,
        MeasureElement = "Sales Amount",
        Value = averageSales.ToString(),
        PredicateType = PredicateType.Or
    });
    aboveAvg.CellStyle = new OlapGridCellStyle()
    {
        Background = Brushes.LightGreen,
        FontWeight = FontWeights.Bold
    };
    olapGrid1.ConditionalFormats.Add(aboveAvg);
    
    // Below average
    OlapGridDataConditionalFormat belowAvg = new OlapGridDataConditionalFormat();
    belowAvg.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.LessThan,
        MeasureElement = "Sales Amount",
        Value = averageSales.ToString(),
        PredicateType = PredicateType.Or
    });
    belowAvg.CellStyle = new OlapGridCellStyle()
    {
        Background = Brushes.LightCoral
    };
    olapGrid1.ConditionalFormats.Add(belowAvg);
    
    olapGrid1.DataBind();
}
```

### Pattern 4: Multiple Measures

Format different measures with different rules.

```csharp
public void FormatMultipleMeasures()
{
    // High Internet Sales
    OlapGridDataConditionalFormat internetHigh = new OlapGridDataConditionalFormat();
    internetHigh.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.GreaterThan,
        MeasureElement = "Internet Sales Amount",
        Value = "3000000",
        PredicateType = PredicateType.Or
    });
    internetHigh.CellStyle = new OlapGridCellStyle() { Background = Brushes.LightBlue };
    olapGrid1.ConditionalFormats.Add(internetHigh);
    
    // High Reseller Sales
    OlapGridDataConditionalFormat resellerHigh = new OlapGridDataConditionalFormat();
    resellerHigh.Conditions.Add(new OlapGridDataCondition()
    {
        ConditionType = OlapGridDataConditionType.GreaterThan,
        MeasureElement = "Reseller Sales Amount",
        Value = "2000000",
        PredicateType = PredicateType.Or
    });
    resellerHigh.CellStyle = new OlapGridCellStyle() { Background = Brushes.LightPink };
    olapGrid1.ConditionalFormats.Add(resellerHigh);
    
    olapGrid1.DataBind();
}
```

## Best Practices

**Keep It Simple:**
- Limit to 3-5 conditional formats per grid
- Use distinct colors that are easily differentiated
- Avoid overly complex condition logic

**Accessibility:**
- Don't rely solely on color (use font weight, borders)
- Ensure sufficient color contrast
- Test with colorblind simulation tools

**Performance:**
- Conditional formatting is evaluated on every data bind
- Complex conditions on large datasets may impact performance
- Consider limiting formatting to key measures only

**User Experience:**
- Provide legend explaining color meanings
- Use consistent color schemes across reports
- Allow users to toggle formatting on/off if desired

## Troubleshooting

**Formatting not applied:**
- Call `DataBind()` after adding conditional formats
- Verify measure name matches exactly
- Check condition logic (And/Or predicates)
- Ensure Value is string representation of number

**Wrong cells formatted:**
- Review condition types (GreaterThan vs GreaterThanOrEqual)
- Check predicate types (And vs Or)
- Verify measure element name

**Colors not showing:**
- Confirm OlapGridCellStyle properties are set
- Check for conflicting styles
- Verify Brushes are valid

**Sample Location:**

```
{System Drive}:\Users\<User Name>\AppData\Local\Syncfusion\EssentialStudio\<Version>\WPF\OlapGrid.WPF\Samples\Appearance\Conditional Formatting
```
