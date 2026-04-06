# Filtering in WPF OLAP Client

## Table of Contents
- [Overview](#overview)
- [Filtering by Member](#filtering-by-member)
- [Filtering by Value](#filtering-by-value)
  - [Column Filter vs Row Filter](#column-filter-vs-row-filter)
  - [Opening the Filtering Dialog](#opening-the-filtering-dialog)
  - [Filter Options](#filter-options)
- [Subset Filtering](#subset-filtering)
- [Controlling Filter Button Visibility](#controlling-filter-button-visibility)
- [Best Practices](#best-practices)

## Overview

The OLAP Client provides three types of filtering mechanisms to refine and focus your data analysis:

1. **Member Filtering**: Select specific members from a hierarchy
2. **Value Filtering**: Apply conditional filters based on measure values
3. **Subset Filtering**: Limit the number of records displayed

## Filtering by Member

Member filtering allows you to include or exclude specific members from a dimension hierarchy, providing precise control over which data appears in your analysis.

### How to Filter by Member

1. In the Axis Element Builder, locate the dimension element you want to filter
2. Click the **split button** at the right corner of the dimension node
3. The **Member Editor** dialog opens, displaying all members in a tree view structure
4. Check or uncheck the checkboxes corresponding to members you want to include/exclude
5. Click **OK** to apply the filter (or **Cancel** to abort)

### Member Editor Features

- **Tree View Structure**: Displays members hierarchically
- **Check/Uncheck**: Toggle individual member selection
- **Check All**: Select all members at once
- **Uncheck All**: Deselect all members at once
- **Real-time Update**: OLAP Grid and Chart refresh based on selected members

### Use Cases

- **Focus on Specific Regions**: Include only relevant countries/states
- **Exclude Outliers**: Remove abnormal or test data members
- **Comparative Analysis**: Select specific members for side-by-side comparison
- **Compliance Reporting**: Include only required organizational units

## Filtering by Value

Value filtering applies conditional logic to filter rows and columns based on measure values. This allows you to show only data that meets specific numerical criteria.

### Column Filter vs Row Filter

**Column Filter:**
- Checks **each row of a column** against the filter condition
- The column is included **only if ALL rows** satisfy the condition
- If any row fails the condition, the entire column is filtered out

**Row Filter:**
- Checks **each column of a row** against the filter condition
- The row is included **only if ALL columns** satisfy the condition
- If any column fails the condition, the entire row is filtered out

### Opening the Filtering Dialog

Value filtering is accessed through the Filtering and Sorting dialog. Click the corresponding Row Filter or Column Filter icon in the OLAP Client toolbar to open the dialog.

### Filter Options

The Filter tab provides these options:

#### 1. Filter Empty Rows/Columns

**Purpose:** Removes empty rows or columns from the result set

**Usage:** Check this option to exclude cells with no data, making the report cleaner and more focused

**Example:**
```csharp
// Enable filtering of empty rows in code (if applicable through properties)
// This is typically configured through the UI dialog
```

#### 2. Filter Expressions (Filter 1 and Filter 2)

You can apply **two filter expressions** simultaneously to a report. Each filter expression contains:

**Condition:**
- Choose a comparison operator:
  - Equals
  - Not Equals
  - Less Than
  - Less Than or Equal To
  - Greater Than
  - Greater Than or Equal To
  - Between
  - Not Between

**Filter On:**
- Select the **measure element** to filter against
- Available measures appear in a drop-down list

**Value:**
- Enter the **conditional value** for the expression
- Numeric value that the measure is compared against

#### Filter Expression Examples

**Example 1: Show only high sales**
- Condition: Greater Than
- Filter On: Sales Amount
- Value: 100000
- Result: Shows only columns/rows where Sales Amount > 100,000

**Example 2: Range filtering**
- Filter 1:
  - Condition: Greater Than or Equal To
  - Filter On: Sales Amount
  - Value: 50000
- Filter 2:
  - Condition: Less Than or Equal To
  - Filter On: Sales Amount
  - Value: 200000
- Result: Shows data where Sales Amount is between 50,000 and 200,000

**Example 3: Exclude zero sales**
- Condition: Not Equals
- Filter On: Sales Amount
- Value: 0
- Result: Filters out all zero-sale records

## Subset Filtering

Subset filtering restricts the **number of records** displayed in the result set to a specific count. This is useful for top-N or bottom-N analysis and managing large datasets.

### Configuration

Subset filtering can be applied to both rows and columns independently.

### How to Apply Subset Filter

1. Open the Filtering and Sorting dialog (for row or column)
2. Navigate to the subset filter section
3. Enter the maximum number of records to display
4. Apply the filter

### Use Cases

**Top-N Analysis:**
- Show top 10 customers by revenue
- Display top 5 products by sales volume
- List top 20 sales representatives

**Performance Optimization:**
- Limit initial data load for large cubes
- Reduce rendering time for complex reports
- Paginate through large result sets

**Executive Summaries:**
- Focus on most important items
- Highlight key performers
- Simplify decision-making with focused data

### Code Example

```csharp
// Toggle visibility of subset filters
this.olapClient1.ShowSubsetFilters = true;
```

**VB.NET:**

```vbnet
' Toggle visibility of subset filters
Me.olapClient1.ShowSubsetFilters = True
```

## Controlling Filter Button Visibility

You can programmatically control the visibility of filter and sort buttons in the OLAP Client toolbar.

### Hide Filter/Sort Buttons

```csharp
// Hide filter and sort buttons in toolbar
this.olapClient1.ShowFilterSortButtons = false;
```

**VB.NET:**

```vbnet
' Hide filter and sort buttons in toolbar
Me.olapClient1.ShowFilterSortButtons = False
```

### Hide Subset Filter Options

```csharp
// Hide subset filter options
this.olapClient1.ShowSubsetFilters = false;
```

**VB.NET:**

```vbnet
' Hide subset filter options
Me.olapClient1.ShowSubsetFilters = False
```

### Use Cases for Hiding Filters

- **Simplified UI**: For end users who don't need filtering capabilities
- **Locked Reports**: Prevent modification of predefined filter criteria
- **Restricted Mode**: When presenting fixed analytical views
- **Custom Filtering**: When implementing custom filtering UI

## Best Practices

### Combine Filter Types

Use multiple filter types together for powerful data analysis:

1. **Member Filter** → Select relevant dimensions
2. **Value Filter** → Apply measure-based conditions
3. **Subset Filter** → Limit to top/bottom N results

**Example Workflow:**
- Filter members to include only "North America" region
- Apply value filter for Sales > $100,000
- Use subset filter to show top 10 results

### Filter Early, Display Less

Apply filters before rendering to improve performance:
- Member filtering reduces data volume at the source
- Value filtering narrows the result set
- Subset filtering limits display complexity

### Document Filter Criteria

For saved reports, document your filter settings:
- Which members are included/excluded
- Value filter expressions applied
- Subset limits configured
- Business rationale for filters

### Test Filter Combinations

Not all filter combinations produce intuitive results:
- Column filters check ALL rows in a column
- Row filters check ALL columns in a row
- Empty result sets may indicate overly restrictive filters

### Performance Considerations

- **Member filtering**: Fastest, reduces data at query level
- **Value filtering**: Moderate, applied during data processing
- **Subset filtering**: Fastest, limits display only
- **Combined filters**: Test with large datasets to ensure acceptable performance

## Common Filtering Scenarios

### Scenario 1: Regional Focus

**Goal:** Analyze only European markets

**Solution:**
1. Open Member Editor for Geography dimension
2. Check only European countries
3. Uncheck other regions
4. Apply filter

### Scenario 2: High-Value Transactions

**Goal:** Show only sales above $50,000

**Solution:**
1. Open Column/Row Filter dialog
2. Filter 1: Condition = Greater Than, Filter On = Sales Amount, Value = 50000
3. Apply filter

### Scenario 3: Top Performers

**Goal:** Display top 5 products by revenue

**Solution:**
1. Open Filtering dialog
2. Apply subset filter with limit = 5
3. Sort by revenue (descending) - see sorting.md
4. Apply filters

### Scenario 4: Remove Empty Data

**Goal:** Clean report by removing empty rows/columns

**Solution:**
1. Open Filtering dialog
2. Check "Filter empty rows/columns"
3. Apply filter

## Troubleshooting

### No Data Displayed After Filtering

**Cause:** Filters are too restrictive  
**Solution:**
- Check member selections - ensure some members are checked
- Review value filter criteria - may be excluding all data
- Verify subset filter limit is not zero

### Filter Not Working as Expected

**Cause:** Misunderstanding of column vs row filter logic  
**Solution:**
- Remember: Column filter checks ALL rows in column
- Remember: Row filter checks ALL columns in row
- Try the opposite axis if results are unexpected

### Performance Degradation

**Cause:** Complex filter expressions on large datasets  
**Solution:**
- Apply member filters first to reduce data volume
- Simplify value filter expressions
- Use subset filtering to limit result size
- Consider cube optimization or aggregations

## Next Steps

After mastering filtering:

1. Learn about sorting - see [sorting.md](sorting.md)
2. Explore paging for large datasets - see [paging.md](paging.md)
3. Combine with drill-through - see [drill-through.md](drill-through.md)
