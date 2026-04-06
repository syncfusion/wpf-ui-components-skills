# Named Sets in WPF OLAP Client

## Overview

The OLAP Client supports binding to named set records that are pre-defined in the cube. A named set is a collection of tuples and members that can be defined and saved as part of the cube definition, providing a convenient way to work with commonly used or complex member groupings.

## What is a Named Set?

A **named set** is a pre-defined collection of members or tuples in an OLAP cube. Named sets are created using Multidimensional Expressions (MDX) and saved as part of the cube definition, making them reusable across multiple reports and queries.

### Key Characteristics

- **Pre-defined**: Created during cube design, not at query time
- **Reusable**: Available across all reports and analyses
- **MDX-based**: Defined using MDX expressions
- **Persistent**: Stored as part of the cube schema

### Named Set vs Dimension

| Feature | Named Set | Dimension |
|---------|-----------|-----------|
| **Definition** | Custom collection of members | Organizational category in cube |
| **Creation** | Defined via MDX in cube | Part of cube structure |
| **Flexibility** | Can combine members from multiple dimensions | Single dimensional structure |
| **Usage** | Drag to axes like dimensions | Drag to axes for categorization |

## Accessing Named Sets

Named sets are displayed in the Cube Dimension Browser alongside dimensions, measures, and KPIs.

### Location in UI

Named sets reside inside the **Sets** folder under a dimension element in the tree structure.

**Tree Structure:**
```
Cube Dimension Browser
├── Measures
├── Dimensions
│   ├── Customer
│   ├── Geography
│   └── Product
├── KPIs
└── Sets (Named Sets folder)
    ├── Top 10 Customers
    ├── High Value Products
    └── Key Markets
```

## Using Named Sets in Reports

Named sets can be used just like dimensions in the OLAP Client. Drag them to any axis in the Axis Element Builder.

### Drag-and-Drop

1. **Locate the Named Set** in the Cube Dimension Browser
2. **Click and Hold** on the named set element
3. **Drag** to the desired axis (Categorical, Series, or Slicer)
4. **Drop** at the desired position

### Supported Axes

Named sets can be placed on:
- **Categorical (Column) Axis**: Display named set members as columns
- **Series (Row) Axis**: Display named set members as rows
- **Slicer Axis**: Filter by named set members

## Common Named Set Examples

### Example 1: Top N Customers

A named set defining the top 10 customers by revenue:

**MDX Definition (in cube):**
```mdx
CREATE SET [Top 10 Customers] AS
TopCount([Customer].[Customer].Members, 10, [Measures].[Sales Amount])
```

**Usage in OLAP Client:**
- Drag "Top 10 Customers" to Row axis
- View sales data for only top 10 customers
- No need to manually filter or sort

### Example 2: Key Markets

A named set grouping important geographic markets:

**MDX Definition (in cube):**
```mdx
CREATE SET [Key Markets] AS
{
    [Geography].[Country].&[United States],
    [Geography].[Country].&[United Kingdom],
    [Geography].[Country].&[Germany],
    [Geography].[Country].&[France],
    [Geography].[Country].&[Canada]
}
```

**Usage in OLAP Client:**
- Drag "Key Markets" to Column axis
- Focus analysis on strategic markets
- Consistent definition across all reports

### Example 3: Seasonal Products

A named set for products sold primarily in specific seasons:

**MDX Definition (in cube):**
```mdx
CREATE SET [Summer Products] AS
Filter([Product].[Product].Members, 
       [Product].[Season] = "Summer")
```

**Usage in OLAP Client:**
- Drag "Summer Products" to Row axis
- Analyze seasonal trends
- Compare with other seasonal sets

## Benefits of Named Sets

### Consistency Across Reports

**Without Named Sets:**
- Each report manually filters for "top customers"
- Different analysts may use different criteria
- Inconsistent results across organization

**With Named Sets:**
- "Top 10 Customers" defined once in cube
- All reports use same definition
- Consistent metrics across organization

### Simplified Complex Expressions

**Without Named Sets:**
- Users must understand MDX to create complex filters
- Risk of errors in complex expressions
- Difficult to maintain

**With Named Sets:**
- Complex MDX written once by cube designer
- Users simply drag and drop the named set
- Easy to use, reliable results

### Performance Optimization

**Cube-Level Optimization:**
- Named sets can be pre-calculated during cube processing
- Faster query performance compared to runtime calculations
- Reduced load on analysis services

### Business Logic Encapsulation

**Named sets encapsulate business rules:**
- "VIP Customers" defined by business criteria
- "Strategic Products" based on company strategy
- "Problem Accounts" flagged by finance rules
- Changes to business logic update centrally in cube

## Working with Named Sets

### Combining Named Sets with Dimensions

You can use named sets alongside regular dimensions:

**Example Report Structure:**
```
Columns (Categorical Axis):
- [Date].[Calendar Year]
- [Date].[Calendar Quarter]

Rows (Series Axis):
- [Top 10 Customers] (Named Set)
- [Measures].[Sales Amount]
```

**Result:** Sales by top 10 customers across years and quarters

### Named Sets as Slicers

Use named sets in the slicer axis to filter data:

```
Columns: [Product].[Category]
Rows: [Geography].[Country]
Slicer: [Key Time Periods] (Named Set)
```

**Result:** Product sales by country, filtered to key time periods

### Multiple Named Sets

Combine multiple named sets in a single report:

```
Columns: [Top Products] (Named Set)
Rows: [VIP Customers] (Named Set)
Measures: [Sales Amount]
```

**Result:** Cross-analysis of top products vs VIP customers

## MDX Expression Benefits

Named sets leverage the power of MDX (Multidimensional Expressions) without requiring end users to understand MDX syntax.

### Complex Member Selection

**Scenario:** Select customers meeting multiple criteria

**MDX in Named Set:**
```mdx
CREATE SET [High Value Active Customers] AS
Filter([Customer].[Customer].Members,
       [Measures].[Sales Amount] > 100000 AND
       [Measures].[Last Purchase Date] > "2025-01-01")
```

**User Experience:**
- Simply drag "High Value Active Customers" to axis
- No MDX knowledge required
- Consistent definition

### Time Intelligence

**Scenario:** Compare current period vs prior period

**MDX in Named Set:**
```mdx
CREATE SET [Current and Prior Year] AS
{
    PeriodsToDate([Date].[Calendar].[Year]),
    ParallelPeriod([Date].[Calendar].[Year], 1)
}
```

**User Experience:**
- Drag "Current and Prior Year" to axis
- Automatic time comparison
- No date calculation needed

### Hierarchical Selections

**Scenario:** Select specific levels across hierarchy

**MDX in Named Set:**
```mdx
CREATE SET [Key Geographies] AS
{
    [Geography].[Country].&[USA].[State].&[CA],
    [Geography].[Country].&[USA].[State].&[NY],
    [Geography].[Country].&[UK],
    [Geography].[Country].&[Germany]
}
```

**User Experience:**
- Mixed hierarchy levels in one set
- Simplified regional analysis
- Consistent geography definition

## Best Practices

### Work with Cube Designers

**Identify Common Analysis Needs:**
- Meet with business users to understand frequent queries
- Document commonly used member groupings
- Request named sets for these scenarios

**Example Requests:**
- "Top N" sets (customers, products, regions)
- Strategic segment groupings
- Time period definitions
- Problem/opportunity identification sets

### Use Descriptive Names

**Good Named Set Names:**
- "Top 10 Customers by Revenue"
- "Strategic Product Lines"
- "Problem Accounts Requiring Attention"
- "Key Markets for Q4 Promotion"

**Poor Named Set Names:**
- "Set1"
- "Test"
- "MySet"
- "Customers"

### Document Named Sets

For organizational knowledge:
- Maintain a list of available named sets
- Document the business logic for each
- Include usage examples
- Specify when to use each set

### Combine with Measures

Named sets work best when paired with appropriate measures:

```
Named Set: [Top Performing Stores]
Measure: [Sales Amount], [Customer Count], [Transaction Count]
```

Provides comprehensive view of performance.

## Use Cases

### Executive Dashboards

**Scenario:** CEO wants to see top performers at a glance

**Solution:**
- Use named sets: "Top 10 Products", "Top 5 Regions", "VIP Customers"
- Drag to OLAP Client axes
- Quick, consistent view of key metrics

### Targeted Analysis

**Scenario:** Marketing team analyzing promotional campaign

**Solution:**
- Create "Campaign Target Products" named set in cube
- Use in multiple reports: sales, inventory, customer demographics
- Consistent product grouping across all analyses

### Exception Reporting

**Scenario:** Operations team monitoring problem areas

**Solution:**
- Named sets: "Underperforming Stores", "Low Inventory Products", "Delayed Shipments"
- Focus immediately on exceptions
- Drill-through for detailed investigation

### Comparative Analysis

**Scenario:** Compare different customer segments

**Solution:**
- Named sets: "New Customers", "Loyal Customers", "At-Risk Customers"
- Place all three on same axis
- Side-by-side comparison of behavior

## Troubleshooting

### Named Sets Not Visible

**Possible Causes:**
- Cube doesn't contain named sets
- Sets folder is collapsed in tree view
- Permissions restrict access to named sets

**Solutions:**
- Verify cube contains named sets (check in SSMS)
- Expand Sets folder in Cube Dimension Browser
- Check user permissions on cube

### Named Set Returns Unexpected Members

**Possible Causes:**
- MDX definition doesn't match expectation
- Cube data has changed since named set was created
- Named set relies on outdated calculations

**Solutions:**
- Review MDX definition in cube
- Reprocess cube with updated data
- Coordinate with cube designer to update definition

### Performance Issues with Named Sets

**Possible Causes:**
- Named set contains very complex MDX
- Named set not pre-calculated in cube
- Large number of members in set

**Solutions:**
- Optimize MDX in named set definition
- Configure cube to pre-calculate named sets
- Consider breaking into smaller, more focused sets

## Next Steps

After understanding named sets:

1. Learn about calculated members - see [calculated-members.md](calculated-members.md)
2. Explore filtering capabilities - see [filtering.md](filtering.md)
3. Understand report management - see [report-management.md](report-management.md)
