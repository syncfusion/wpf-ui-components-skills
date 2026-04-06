# Calculated Members in WPF OLAP Client

## Overview

Calculated members allow users to define custom measures and members using the Calculated Member Editor. This feature enables dynamic creation of business metrics and custom calculations without modifying the underlying cube structure.

## Enabling Calculated Members

The Calculated Member Editor is accessed through the OLAP Client toolbar. The calculated member icon is visible only when the `IsCalculatedMembersEnabled` property is set to `true`.

### Configuration

**Through XAML:**

```xaml
<CheckBox Name="chk_CalcMember" 
          ToolTip="Enable/Disable Calculated Members" 
          Content="Enable Calculated Members" 
          IsChecked="{Binding ElementName=olapClient1, Path=IsCalculatedMembersEnabled}"/>
```

**Through C# Code:**

```csharp
// Enable calculated members feature
this.olapClient1.IsCalculatedMembersEnabled = true;
```

**Through VB.NET Code:**

```vbnet
' Enable calculated members feature
Me.olapClient1.IsCalculatedMembersEnabled = True
```

## Calculated Member Editor

When enabled, the Calculated Member icon appears in the OLAP Client toolbar. Click the icon to open the Calculated Member Editor window.

## Creating Calculated Members

### Editor Components

The Calculated Member Editor provides:

1. **Name Field**: Specify the name for your calculated member
2. **Expression Field**: Enter the MDX expression for the calculation
3. **Member Type**: Choose between Measure or Dimension Member
4. **Format String**: Define display formatting (optional)
5. **Parent Hierarchy**: Select where the member appears in the cube structure

### Basic Workflow

1. **Open Editor**: Click calculated member icon in toolbar
2. **Enter Name**: Provide descriptive name (e.g., "Profit Margin %")
3. **Write Expression**: Define MDX calculation
4. **Select Type**: Choose Measure or Dimension Member
5. **Apply**: Add the calculated member to the report
6. **Use**: Drag to axes like any other measure/member

## MDX Expressions for Calculated Members

Calculated members use MDX (Multidimensional Expressions) to define calculations.

### Simple Calculations

**Example 1: Profit**

```mdx
[Measures].[Sales Amount] - [Measures].[Total Cost]
```

**Result:** Creates a "Profit" measure by subtracting cost from sales

**Example 2: Average Order Value**

```mdx
[Measures].[Sales Amount] / [Measures].[Order Count]
```

**Result:** Calculates average revenue per order

### Percentage Calculations

**Example 3: Profit Margin Percentage**

```mdx
([Measures].[Sales Amount] - [Measures].[Total Cost]) / [Measures].[Sales Amount] * 100
```

**Result:** Profit margin as a percentage of sales

**Example 4: Growth Rate**

```mdx
([Measures].[Sales Amount] - [Measures].[Sales Amount Previous Year]) / 
[Measures].[Sales Amount Previous Year] * 100
```

**Result:** Year-over-year growth percentage

### Conditional Calculations

**Example 5: High Value Indicator**

```mdx
IIF([Measures].[Sales Amount] > 100000, "High", "Standard")
```

**Result:** Labels transactions as "High" or "Standard" based on value

**Example 6: Tiered Commission**

```mdx
IIF([Measures].[Sales Amount] > 50000, 
    [Measures].[Sales Amount] * 0.10,
    [Measures].[Sales Amount] * 0.05)
```

**Result:** 10% commission above $50K, 5% below

### Aggregation Calculations

**Example 7: Running Total**

```mdx
Sum(YTD(), [Measures].[Sales Amount])
```

**Result:** Year-to-date cumulative sales

**Example 8: Moving Average**

```mdx
Avg(LastPeriods(3, [Date].[Month].CurrentMember), [Measures].[Sales Amount])
```

**Result:** 3-month moving average of sales

## Common Calculation Patterns

### Variance Analysis

**Actual vs Budget Variance:**

```mdx
[Measures].[Actual Sales] - [Measures].[Budgeted Sales]
```

**Variance Percentage:**

```mdx
([Measures].[Actual Sales] - [Measures].[Budgeted Sales]) / 
[Measures].[Budgeted Sales] * 100
```

### Ratio Analysis

**Conversion Rate:**

```mdx
[Measures].[Orders] / [Measures].[Visitors] * 100
```

**Inventory Turnover:**

```mdx
[Measures].[Cost of Goods Sold] / [Measures].[Average Inventory]
```

### Performance Metrics

**Return on Investment (ROI):**

```mdx
([Measures].[Revenue] - [Measures].[Investment]) / [Measures].[Investment] * 100
```

**Customer Lifetime Value:**

```mdx
[Measures].[Average Order Value] * [Measures].[Purchase Frequency] * 
[Measures].[Customer Lifespan]
```

## Use Cases

### Financial Analysis

**Scenario:** CFO needs profit margin analysis not in the cube

**Solution:**
1. Create calculated member: "Gross Profit Margin %"
2. Expression: `([Measures].[Revenue] - [Measures].[COGS]) / [Measures].[Revenue] * 100`
3. Drag to report for instant profit margin analysis

### Sales Performance

**Scenario:** Sales manager wants to track quota attainment

**Solution:**
1. Create calculated member: "Quota Attainment %"
2. Expression: `[Measures].[Actual Sales] / [Measures].[Sales Quota] * 100`
3. Identify underperforming regions/reps

### Operational KPIs

**Scenario:** Operations team needs fill rate metric

**Solution:**
1. Create calculated member: "Order Fill Rate"
2. Expression: `[Measures].[Filled Orders] / [Measures].[Total Orders] * 100`
3. Monitor inventory and fulfillment performance

### Marketing Analytics

**Scenario:** Marketing needs cost per acquisition

**Solution:**
1. Create calculated member: "Cost Per Acquisition"
2. Expression: `[Measures].[Marketing Spend] / [Measures].[New Customers]`
3. Evaluate campaign efficiency

## Benefits of Calculated Members

### On-the-Fly Analysis

**Without Calculated Members:**
- Wait for cube designer to add new measures
- Cube reprocessing required
- Days or weeks for new metrics

**With Calculated Members:**
- Create metrics immediately
- No cube changes needed
- Instant availability

### Ad-Hoc Business Questions

**Scenario:** Executive asks unexpected question during meeting

**Solution:**
- Open Calculated Member Editor
- Define metric on the spot
- Answer question immediately with data

### User Empowerment

**Enable Business Users:**
- Finance users create their own financial ratios
- Sales managers define performance metrics
- Analysts build custom KPIs
- Reduces dependency on IT/BI team

### Prototyping New Metrics

**Before Permanent Implementation:**
1. Create calculated member to test metric
2. Use in reports, validate with stakeholders
3. If valuable, request permanent cube measure
4. If not needed, simply delete calculated member

## Best Practices

### Use Descriptive Names

**Good Names:**
- "Gross Profit Margin %"
- "Year-over-Year Growth Rate"
- "Average Order Value (USD)"
- "Customer Retention Rate"

**Poor Names:**
- "Calc1"
- "New Measure"
- "Test"
- "Temp"

### Document Complex Expressions

For intricate MDX:
- Add comments explaining calculation logic
- Document business rules applied
- Note any assumptions or limitations

### Test Calculations

**Validation Steps:**
1. Create calculated member
2. Compare results against known values
3. Verify with different dimension slices
4. Check edge cases (nulls, zeros, extremes)

### Consider Performance

**Performance Impact:**
- Simple calculations (addition, subtraction): Minimal
- Complex aggregations: Moderate
- Recursive calculations: Significant

**Optimization:**
- Keep expressions as simple as possible
- Avoid unnecessary nesting
- Test with large datasets

### Format Appropriately

Use format strings for readability:
- Currency: `"$#,##0.00"`
- Percentage: `"0.00%"`
- Whole numbers: `"#,##0"`
- Decimals: `"0.00"`

## Integration with OLAP Client

### Using Calculated Members in Reports

After creating a calculated member:

1. **Appears in Measures**: Calculated member shows in measure list
2. **Drag to Axis**: Use like any other measure
3. **Combine with Dimensions**: Slice by any dimension
4. **Include in Exports**: Available in exported reports
5. **Save with Report**: Calculated members persist in saved reports

### Combining with Other Features

**With Filtering:**
```
Create: "High Value Orders" calculated measure
Filter: Show only where > $10,000
Result: Focused high-value analysis
```

**With Sorting:**
```
Create: "Profit Margin %" calculated measure
Sort: Descending by Profit Margin %
Result: Ranked profitability view
```

**With Drill-Through:**
```
Create: "Order Efficiency" calculated measure
Click: Drill through on low-efficiency cell
Result: View underlying inefficient transactions
```

## Calculated Members vs Cube Measures

| Aspect | Calculated Members | Cube Measures |
|--------|-------------------|---------------|
| **Creation** | Runtime in OLAP Client | Design-time in cube |
| **Persistence** | Saved with report | Permanent in cube |
| **Performance** | Calculated on query | Pre-aggregated (faster) |
| **Availability** | Report-specific | Available to all users |
| **Complexity** | Limited by UI | Full MDX capability |
| **Use Case** | Ad-hoc, temporary | Standard, permanent |


## Troubleshooting

### Calculated Member Icon Not Visible

**Possible Cause:**  
`IsCalculatedMembersEnabled` property not set to `true`

**Solution:**
```csharp
this.olapClient1.IsCalculatedMembersEnabled = true;
```

### Expression Errors

**Possible Causes:**
- Invalid MDX syntax
- Referenced measures don't exist
- Incorrect parentheses or operators

**Solutions:**
- Verify MDX syntax
- Check measure names (case-sensitive)
- Test expressions incrementally
- Consult MDX documentation

### Unexpected Results

**Possible Causes:**
- Calculation logic error
- Null value handling
- Division by zero
- Incorrect operator precedence

**Solutions:**
- Add null checks: `IIF(IsEmpty([Measure]), 0, [Measure])`
- Protect against division by zero:
  ```mdx
  IIF([Measures].[Denominator] = 0, NULL, 
      [Measures].[Numerator] / [Measures].[Denominator])
  ```
- Use parentheses to clarify precedence

### Performance Issues

**Possible Causes:**
- Complex recursive calculations
- Heavy aggregations in expression
- Calculated member used with large datasets

**Solutions:**
- Simplify expression
- Consider requesting permanent cube measure
- Apply filters to reduce data volume
- Test with smaller data slices first

## MDX Resources

For more complex calculated members, understanding MDX is helpful:

**Basic MDX Operations:**
- Arithmetic: `+`, `-`, `*`, `/`
- Comparison: `=`, `<>`, `>`, `<`, `>=`, `<=`
- Logical: `AND`, `OR`, `NOT`
- Conditional: `IIF(condition, true_value, false_value)`

**Common MDX Functions:**
- `Sum()`, `Avg()`, `Min()`, `Max()`, `Count()`
- `YTD()`, `MTD()`, `QTD()`
- `ParallelPeriod()`, `LastPeriods()`
- `IsEmpty()`, `IIF()`, `Format()`

## Next Steps

After mastering calculated members:

1. Explore named sets for member groupings - see [named-sets.md](named-sets.md)
2. Learn about filtering calculated members - see [filtering.md](filtering.md)
3. Combine with drill-through - see [drill-through.md](drill-through.md)
