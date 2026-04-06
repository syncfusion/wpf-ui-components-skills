# Sorting in WPF OLAP Client

## Overview

The OLAP Client provides sorting capabilities to order your data by rows or columns in ascending or descending order. Sorting is accessed through the Filtering and Sorting dialog box and helps organize data for better analysis and presentation.

## Sorting Types

### Column Sorting

Sorts the **columns** in the result set based on the **column total** of each column.

**How It Works:**
- Calculates the total value for each column
- Orders columns based on these totals
- Applies sort order (ascending or descending)

**Use Cases:**
- Identify highest/lowest revenue regions
- Rank product categories by performance
- Order time periods by total sales

### Row Sorting

Sorts the **rows** in the result set based on the **row total** of each row.

**How It Works:**
- Calculates the total value for each row
- Orders rows based on these totals
- Applies sort order (ascending or descending)

**Use Cases:**
- Rank customers by purchase amount
- Order products by sales volume
- Sort sales representatives by performance

## Opening the Sorting Dialog

The Filtering and Sorting dialog can be opened by clicking the corresponding icon in the toolbar.

### For Row Sorting

Click the **Filter/sort row** icon in the OLAP Client toolbar.

### For Column Sorting

Click the **Filter/sort column** icon in the OLAP Client toolbar.

## Sorting Options

Once the dialog opens, navigate to the **Sorting** tab. The sorting tab provides these options:

The sorting tab provides these options:
### 1. Sorting On

**Purpose:** Select the measure element to use as the sort key field

**How to Use:**
- Click the drop-down list
- Select the measure you want to sort by (e.g., Sales Amount, Quantity, Profit)
- The selected measure determines the sort order

**Example:**
- Sort by "Sales Amount" to order by revenue
- Sort by "Order Quantity" to order by volume
- Sort by "Profit Margin" to order by profitability

### 2. Ascending or Descending

**Purpose:** Specify the sort direction

**Options:**
- **Ascending**: Smallest to largest (1, 2, 3... or A, B, C...)
- **Descending**: Largest to smallest (3, 2, 1... or Z, Y, X...)

**Common Patterns:**
- Descending: Show top performers first
- Ascending: Identify underperformers or lowest values

### 3. Preserve Hierarchy

**Purpose:** Sort records without changing the hierarchical order

**When Checked:**
- Maintains the parent-child relationships in hierarchies
- Sorts within each hierarchy level independently
- Preserves the logical structure of dimensional data

**When Unchecked:**
- Sorts all records as a flat list
- Ignores hierarchical relationships
- May break visual hierarchy grouping

**Example:**

**With Preserve Hierarchy (Checked):**
```
USA
  ├─ California (sorted within USA)
  ├─ Texas
  └─ New York
Canada
  ├─ Ontario (sorted within Canada)
  └─ Quebec
```

**Without Preserve Hierarchy (Unchecked):**
```
California
Ontario
Texas
New York
Quebec
(Countries and states mixed, hierarchy lost)
```

## Code Example: Controlling Visibility

You can toggle the visibility of filter and sort buttons programmatically:

```csharp
// Hide filter and sort buttons in toolbar
this.olapClient1.ShowFilterSortButtons = false;
```

**VB.NET:**

```vbnet
' Hide filter and sort buttons in toolbar
Me.olapClient1.ShowFilterSortButtons = False
```

**Use Cases for Hiding:**
- Simplified UI for end users
- Locked reports with fixed sorting
- Custom sorting implementation
- Read-only dashboard mode

## Sorting Workflow

### Step-by-Step Sorting

1. **Open the Dialog**
   - Click row or column filter/sort icon in toolbar

2. **Navigate to Sorting Tab**
   - Click the "Sorting" tab in the dialog

3. **Configure Sorting**
   - Select measure in "Sorting on" drop-down
   - Choose Ascending or Descending
   - Check/uncheck "Preserve hierarchy" as needed

4. **Apply**
   - Click OK to apply sorting
   - Click Cancel to abort

5. **Verify Results**
   - OLAP Grid and Chart update with sorted data
   - Verify sort order matches expectations

## Combining Sorting with Filtering

Sorting and filtering work together to create focused, organized reports:

### Recommended Order

1. **Apply Filters First**
   - Member filters to select relevant dimensions
   - Value filters to narrow by measure criteria
   - Subset filters to limit record count

2. **Apply Sorting Second**
   - Sort the filtered result set
   - Order by most relevant measure
   - Choose appropriate ascending/descending

### Example Workflow

**Goal:** Show top 10 products by revenue in European markets

1. **Filter:** Member filter for Europe region only
2. **Filter:** Subset filter limit = 10
3. **Sort:** Sort by Sales Amount, Descending
4. **Result:** Top 10 European products by revenue, highest first

## Common Sorting Scenarios

### Scenario 1: Top Revenue Regions

**Goal:** Identify highest-revenue geographic regions

**Steps:**
1. Open column sorting dialog
2. Sorting on: Sales Amount
3. Order: Descending
4. Preserve hierarchy: Checked (to maintain geographic structure)
5. Apply

**Result:** Regions ordered by revenue, highest first, hierarchy preserved

### Scenario 2: Alphabetical Product List

**Goal:** Display products in alphabetical order

**Steps:**
1. Open row sorting dialog
2. Sorting on: Product Name (if available as measure) or use default
3. Order: Ascending
4. Preserve hierarchy: Unchecked (flat list)
5. Apply

**Result:** Products listed A to Z

### Scenario 3: Underperforming Analysis

**Goal:** Find lowest-performing sales representatives

**Steps:**
1. Open row sorting dialog
2. Sorting on: Sales Amount
3. Order: Ascending
4. Apply
5. Optional: Apply subset filter to show bottom 5

**Result:** Sales reps ordered by performance, lowest first

### Scenario 4: Time-Based Trending

**Goal:** Order time periods to see progression

**Steps:**
1. Open column sorting dialog
2. Sorting on: Date or Time measure
3. Order: Ascending
4. Preserve hierarchy: Checked (Year > Quarter > Month structure)
5. Apply

**Result:** Time periods in chronological order with hierarchy intact

## Best Practices

### Choose the Right Measure

Sort by the measure most relevant to your analysis:
- **Revenue analysis** → Sort by Sales Amount
- **Volume analysis** → Sort by Quantity
- **Profitability analysis** → Sort by Profit or Margin
- **Efficiency analysis** → Sort by Cost or Ratio measures

### Use Preserve Hierarchy Appropriately

**Check it when:**
- Working with geographic hierarchies (Country → State → City)
- Analyzing organizational structures (Department → Team → Individual)
- Time-based hierarchies (Year → Quarter → Month)
- You need to maintain logical groupings

**Uncheck it when:**
- Creating flat rankings
- Comparing unrelated items
- Building simple top-N lists
- Hierarchy structure is not meaningful

### Combine with Subset Filters

Sorting becomes more powerful with subset filtering:
- Sort descending + subset filter = Top-N
- Sort ascending + subset filter = Bottom-N
- Focus on extremes for actionable insights

### Document Sorting Criteria

For saved reports, document:
- Which measure is used for sorting
- Ascending vs descending rationale
- Hierarchy preservation choice
- Business reason for sort order

## Performance Considerations

### Sorting Performance Factors

- **Data volume**: Large datasets take longer to sort
- **Measure complexity**: Calculated measures may slow sorting
- **Hierarchy preservation**: Slightly more overhead when preserving hierarchy
- **Multiple sorts**: Sorting both rows and columns increases processing time

### Optimization Tips

1. **Filter before sorting**: Reduce dataset size with filters first
2. **Use subset filters**: Limit the number of records to sort
3. **Sort by simple measures**: Avoid complex calculated measures when possible
4. **Enable paging**: For very large result sets, combine sorting with paging

## Troubleshooting

### Sort Not Working

**Possible Causes:**
- No measure selected in "Sorting on"
- Filter criteria exclude all data
- Hierarchy conflicts with sort order

**Solutions:**
- Verify measure is selected
- Check filter settings
- Try unchecking "Preserve hierarchy"

### Unexpected Sort Order

**Possible Causes:**
- Sorting by wrong measure
- Preserve hierarchy affecting results
- Null values in data

**Solutions:**
- Verify correct measure is selected
- Toggle "Preserve hierarchy" to see impact
- Check for data quality issues

### Sort Doesn't Persist

**Possible Causes:**
- Sort not saved with report
- Data source changed
- Report reloaded without saving

**Solutions:**
- Save report after applying sort
- Reapply sort after data refresh
- Check report serialization settings

## Next Steps

After mastering sorting:

1. Learn about filtering - see [filtering.md](filtering.md)
2. Explore paging for large datasets - see [paging.md](paging.md)
3. Understand report management - see [report-management.md](report-management.md)
