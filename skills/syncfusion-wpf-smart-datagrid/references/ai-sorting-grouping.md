# AI Sorting and Grouping

## Table of Contents
- [Overview](#overview)
- [AI Sorting](#ai-sorting)
- [AI Grouping](#ai-grouping)
- [Combining Operations](#combining-operations)
- [Best Practices](#best-practices)

## Overview

The Smart DataGrid supports natural language commands for sorting and grouping data. Users can organize data by one or more columns using simple text prompts, without manual configuration of sort descriptors or group descriptors.

**Key Features:**
- **Natural Language Sorting** – Sort by column name with direction (ascending/descending)
- **Multi-Column Sorting** – Apply sorting to multiple columns in a single command
- **Smart Grouping** – Organize data hierarchically using prompts
- **Multi-Level Grouping** – Create nested groups with single or multiple commands
- **Clear Commands** – Remove sorting or grouping with simple prompts

## AI Sorting

Sorting organizes data by one or more columns in ascending or descending order. The Smart DataGrid interprets natural language prompts and applies the appropriate sort configuration.

### Single-Column Sorting

Sort data by a single column using natural language:

**Example Prompts:**
```
sort by city in descending
sort by OrderID ascending
sort by Freight descending
sort by CustomerName
```

**Behavior:**
- If direction is not specified, defaults to ascending
- Column names are case-insensitive
- Previous sorting is replaced by new sort command

**XAML Setup:**
```xml
<syncfusion:SfSmartDataGrid x:Name="SmartGrid" 
                            ItemsSource="{Binding OrderInfoCollection}"
                            EnableSmartActions="True"/>
```

**Result:**
When user enters `"sort by city in descending"`, the grid automatically:
1. Identifies the "City" column (or "ShipCity" if that's the exact property name)
2. Applies descending sort order
3. Updates the grid display

### Multi-Column Sorting

Apply sorting to multiple columns in a single command:

**Example Prompts:**
```
sort by city column in descending and Revenue ascending
sort by Country ascending and Freight descending
sort by PaymentStatus descending and OrderDate ascending
```

**Behavior:**
- Multiple sort criteria are separated by "and"
- Each column can have its own direction (ascending/descending)
- Sort priority follows the order specified in the prompt
- Previous sorting is replaced

**Example Scenario:**

**Prompt:** `"sort by city column in descending and Revenue ascending"`

**Result:**
1. Primary sort: City (descending) – Z to A
2. Secondary sort: Revenue (ascending) – low to high within each city
3. Grid displays data sorted by both criteria

### Clearing Sorting

Remove all applied sorting to return to the original data order:

**Example Prompts:**
```
clear sorting
remove sorting
clear sort
```

**Behavior:**
- Removes all sort descriptors
- Grid returns to unsorted state (original data order)
- Does not affect filtering or grouping

### Sort Direction Keywords

The AI understands various keywords for sort direction:

**Ascending:**
- `ascending`
- `asc`
- `a to z`
- `low to high`

**Descending:**
- `descending`
- `desc`
- `z to a`
- `high to low`

### Column Name Recognition

The AI attempts to match column names intelligently:

**Exact Match:**
```
sort by OrderID descending  // Matches "OrderID" property
```

**Partial Match:**
```
sort by city descending     // Matches "ShipCity" property
sort by customer ascending  // Matches "CustomerName" property
```

**Best Practice:** Use exact property names for reliable results.

## AI Grouping

Grouping organizes data by one or more columns, creating hierarchical views. The Smart DataGrid interprets grouping prompts and creates group descriptors automatically.

### Single-Level Grouping

Group data by a single column:

**Example Prompts:**
```
group by Country
group by PaymentStatus
group by ProductName
```

**Behavior:**
- Creates groups based on unique values in the specified column
- Grid displays expandable/collapsible group headers
- Items within each group maintain their current sort order
- Previous grouping is replaced by new grouping

**Visual Result:**
```
▼ Brazil (10 items)
  - Order 1
  - Order 2
▼ Germany (15 items)
  - Order 3
  - Order 4
▼ USA (20 items)
  - Order 5
  - Order 6
```

### Multi-Level Grouping

Create nested groups by specifying multiple columns:

**Example Prompts:**
```
group by Country and ShipCity
group by PaymentStatus and ProductName
group by Country and PaymentStatus and ProductName
```

**Behavior:**
- Creates hierarchical nested groups
- First column is the top-level group
- Subsequent columns create nested sub-groups
- Can be applied in a single command or multiple sequential commands

**Example Scenario:**

**Prompt:** `"group by Country and ShipCity"`

**Visual Result:**
```
▼ Brazil (10 items)
  ▼ Rio (5 items)
    - Order 1
    - Order 2
  ▼ São Paulo (5 items)
    - Order 3
    - Order 4
▼ Germany (15 items)
  ▼ Berlin (8 items)
    - Order 5
    - Order 6
  ▼ Munich (7 items)
    - Order 7
    - Order 8
```

### Sequential Grouping Commands

You can also create multi-level grouping with multiple commands:

**Sequential Prompts:**
```
1. "group by Country"
2. "group by ShipCity"
```

**Result:**
- Both grouping levels are applied
- Same effect as `"group by Country and ShipCity"`

### Clearing Grouping

Remove all applied grouping to return to flat list view:

**Example Prompts:**
```
clear grouping
remove grouping
clear groups
ungroup
```

**Behavior:**
- Removes all group descriptors
- Grid displays flat list (no groups)
- Sorting is preserved if applied
- Filtering is preserved if applied

### Grouping Keywords

The AI recognizes various grouping keywords:

**Group Commands:**
- `group by`
- `group data by`
- `organize by`
- `arrange by`

**Clear Commands:**
- `clear grouping`
- `remove grouping`
- `clear groups`
- `ungroup`

## Combining Operations

Sorting and grouping can be combined for powerful data organization:

### Sorting Within Groups

Apply sorting after grouping to organize items within each group:

**Commands:**
```
1. "group by Country"
2. "sort by Freight descending"
```

**Result:**
- Data is grouped by Country
- Within each country group, items are sorted by Freight (high to low)

### Grouping After Sorting

Apply grouping after sorting to organize pre-sorted data:

**Commands:**
```
1. "sort by OrderDate descending"
2. "group by PaymentStatus"
```

**Result:**
- Data is grouped by PaymentStatus (Paid, Not Paid)
- Within each group, items maintain descending OrderDate sort

### Complex Multi-Operation Example

**Step-by-step Commands:**
```
1. "sort by Freight descending"
2. "group by Country and ShipCity"
3. "filter Country equals Germany"
```

**Result:**
- Only German orders are shown (filtered)
- Orders are grouped by ShipCity within Germany
- Within each city, orders are sorted by Freight (high to low)

## Best Practices

### 1. Use Exact Property Names

**Recommended:**
```
group by ShipCountry  // Exact property name
sort by OrderID       // Exact property name
```

**Less Reliable:**
```
group by country      // May match "ShipCountry" or "Country"
sort by ID            // May not match "OrderID"
```

### 2. Specify Sort Direction Explicitly

**Recommended:**
```
sort by Freight descending
sort by CustomerName ascending
```

**Less Clear:**
```
sort by Freight  // Defaults to ascending, may not be intended
```

### 3. Group Before Sorting for Clarity

When combining operations:
```
1. "group by Country"
2. "sort by Freight descending"
```

This creates clear visual hierarchy: groups first, then sorted items within groups.

### 4. Use Multi-Column Sorting for Complex Ordering

Instead of multiple commands:
```
sort by Country ascending and OrderDate descending
```

Better than:
```
1. "sort by Country ascending"
2. "sort by OrderDate descending"  // Replaces first sort!
```

### 5. Clear Before Applying New Organization

For predictable results:
```
1. "clear sorting"
2. "clear grouping"
3. "group by Country"
4. "sort by Freight descending"
```

### 6. Test Grouping on Columns with Reasonable Cardinality

**Good Grouping Columns:**
- Country (10-20 unique values)
- PaymentStatus (2-3 unique values)
- ProductName (10-50 unique values)

**Poor Grouping Columns:**
- OrderID (100% unique – creates 1 item per group)
- OrderDate (too many unique values)
- Freight (continuous numeric values)

### 7. Combine Grouping with Filtering for Analysis

**Example:**
```
1. "filter PaymentStatus equals Not Paid"
2. "group by Country"
3. "sort by Freight descending"
```

**Result:** Analyze unpaid orders by country, prioritizing high-value shipments.

## Common Use Cases

### Use Case 1: Regional Sales Analysis

**Goal:** Analyze orders by region and city.

**Commands:**
```
1. "group by Country and ShipCity"
2. "sort by Freight descending"
```

**Result:** Hierarchical view showing countries → cities → orders, with highest freight costs first.

### Use Case 2: Payment Status Tracking

**Goal:** Separate paid and unpaid orders, sorted by date.

**Commands:**
```
1. "group by PaymentStatus"
2. "sort by OrderDate descending"
```

**Result:** Two groups (Paid, Not Paid) with most recent orders first.

### Use Case 3: Product Inventory Overview

**Goal:** View orders by product, sorted by quantity.

**Commands:**
```
1. "group by ProductName"
2. "sort by Quantity descending"
```

**Result:** Products grouped together, showing largest quantity orders first.

### Use Case 4: Multi-Level Regional Analysis

**Goal:** Analyze orders by country, city, and payment status.

**Commands:**
```
group by Country and ShipCity and PaymentStatus
```

**Result:** Three-level hierarchy: Country → City → Payment Status → Orders.

### Use Case 5: Clearing and Resetting

**Goal:** Remove all organization and start fresh.

**Commands:**
```
1. "clear grouping"
2. "clear sorting"
```

**Result:** Grid returns to original flat list view.

## Troubleshooting

### Issue: Sort Not Applied

**Possible Causes:**
- EnableSmartActions is false
- AI service not configured
- Column name doesn't match property name

**Solution:**
- Set `EnableSmartActions="True"`
- Verify AI service setup in App.xaml.cs
- Use exact property names from data model

### Issue: Grouping Creates Too Many Groups

**Cause:** Grouped by column with high cardinality (many unique values).

**Solution:** Group by columns with fewer unique values (e.g., Country, Status, Category).

### Issue: Multi-Column Sort Replaces Previous Sort

**Cause:** Separate sort commands replace each other.

**Solution:** Use single command with "and" to specify all sort criteria:
```
sort by Country ascending and Freight descending
```

### Issue: Clear Command Not Working

**Possible Causes:**
- Wrong keyword
- AI service connection issue

**Solution:**
- Use explicit clear commands: `"clear sorting"` or `"clear grouping"`
- Verify AI service is active and responding
