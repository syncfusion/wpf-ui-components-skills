# AI Filtering and Highlighting

## Table of Contents
- [Overview](#overview)
- [AI Filtering](#ai-filtering)
- [Filter Operators](#filter-operators)
- [Complex Filtering](#complex-filtering)
- [AI Highlighting](#ai-highlighting)
- [Highlight Colors](#highlight-colors)
- [Combining Highlights](#combining-highlights)
- [Common Use Cases](#common-use-cases)

## Overview

The Smart DataGrid provides AI-powered filtering and highlighting capabilities through natural language commands. Users can apply complex filter conditions and visual emphasis without building predicates manually or writing conditional styling code.

**Key Capabilities:**
- **Intelligent Filtering** – Apply filters using plain-language conditions
- **Filter Operators** – Equals, contains, greaterThan, lessThan, between
- **Multi-Step Filters** – Combine conditions with AND/OR logic
- **Row Highlighting** – Emphasize entire rows based on conditions
- **Cell Highlighting** – Emphasize specific cells based on conditions
- **Custom Colors** – Specify colors (named, hex, RGB) for highlights
- **Selective Clearing** – Clear all, row-only, or cell-only highlights

## AI Filtering

Filtering uses natural language conditions to build predicates internally and apply them to the grid. Only rows matching the filter conditions are displayed.

### Basic Filtering Syntax

**Pattern:**
```
filter <ColumnName> <Operator> <Value>
```

**Example Prompts:**
```
filter Country equals Germany
filter Freight greaterThan 500
filter PaymentStatus equals Not Paid
filter CustomerName contains Emma
```

### Filter Operators

The Smart DataGrid supports various filter operators for different comparison types:

| Operator | Aliases | Purpose | Example |
|----------|---------|---------|---------|
| **equals** | `=`, `is`, `==` | Exact match | `filter Country equals Germany` |
| **contains** | `includes`, `has` | Substring match | `filter CustomerName contains John` |
| **greaterThan** | `>`, `gt`, `more than` | Greater than | `filter Freight greaterThan 1000` |
| **lessThan** | `<`, `lt`, `less than` | Less than | `filter Quantity lessThan 10` |
| **between** | `in range`, `from...to` | Range filter | `filter OrderID between 20251001 and 20251020` |

### Filter Operator Examples

#### Equals Operator

Match exact values:

```
filter Country equals Germany
filter PaymentStatus equals Paid
filter ProductName equals Keyboard
```

**Use when:** Need exact matches (countries, statuses, categories).

#### Contains Operator

Match partial strings:

```
filter CustomerName contains Emma
filter ProductName contains Monitor
filter ShipCity contains Berlin
```

**Use when:** Searching for text within string fields.

#### GreaterThan Operator

Filter numeric or date values above a threshold:

```
filter Freight greaterThan 1000
filter Quantity greaterThan 10
filter OrderDate greaterThan 2026-01-01
```

**Use when:** Finding high-value items, large quantities, recent dates.

#### LessThan Operator

Filter numeric or date values below a threshold:

```
filter Freight lessThan 100
filter Quantity lessThan 5
filter OrderID lessThan 20251020
```

**Use when:** Finding low-cost items, small quantities, older records.

#### Between Operator

Filter values within a range:

```
filter OrderID between 20251001 and 20251020
filter Freight between 100 and 500
filter Quantity between 5 and 15
```

**Use when:** Analyzing data within specific ranges.

## Complex Filtering

Combine multiple filter conditions with logical operators (AND/OR) to create sophisticated queries.

### AND Logic

Apply multiple conditions that all must be true:

**Example Prompts:**
```
filter Country equals Germany AND Revenue greaterThan 1000
filter PaymentStatus equals Not Paid AND Freight greaterThan 500
filter Country equals Brazil AND ProductName contains Monitor AND Quantity greaterThan 5
```

**Behavior:**
- All conditions must be satisfied
- Only rows matching ALL conditions are shown
- Multiple AND conditions can be chained

**Example Scenario:**

**Prompt:** `"filter Country equals Germany AND Revenue greaterThan 1000"`

**Result:**
- Only shows orders where:
  - Country is "Germany" (exact match)
  - AND Revenue is greater than 1000
- All other rows are hidden

### OR Logic

Apply multiple conditions where at least one must be true:

**Example Prompts:**
```
filter Country equals Germany OR Country equals Brazil
filter PaymentStatus equals Not Paid OR Freight greaterThan 500
```

**Behavior:**
- At least one condition must be satisfied
- Rows matching ANY condition are shown
- Useful for multiple category filtering

### Combining AND/OR

Create complex logical expressions:

**Example Prompt:**
```
filter (Country equals Germany OR Country equals Brazil) AND PaymentStatus equals Not Paid
```

**Note:** The AI interprets complex logic, though explicit parentheses may not be required in all cases.

### Clearing Filters

Remove all applied filters to show all data:

**Example Prompts:**
```
clear filters
remove filters
clear filter
show all data
```

**Behavior:**
- Removes all filter descriptors
- All rows are displayed again
- Sorting and grouping are preserved if applied

## AI Highlighting

Highlighting applies visual styles to rows or cells that meet specified conditions. This emphasizes important data without filtering out other information.

### Row Highlighting

Emphasize entire rows based on conditions:

**Example Prompts:**
```
highlight rows where Total > 1000 color LightPink
highlight rows where PaymentStatus equals Not Paid color Red
highlight rows where Freight > 500 color Yellow
```

**Syntax:**
```
highlight rows where <Condition> color <Color>
```

**Behavior:**
- Entire row is highlighted if condition is met
- Multiple row highlights can be applied simultaneously
- Each highlight can have a different color

### Cell Highlighting

Emphasize specific cells in a column based on conditions:

**Example Prompts:**
```
highlight cells in OrderID lessThan 1005 color Red
highlight cells in Quantity greaterThan 10 color Green
highlight cells in Freight between 100 and 500 color Yellow
```

**Syntax:**
```
highlight cells in <ColumnName> <Condition> color <Color>
```

**Behavior:**
- Only cells in the specified column are highlighted
- Condition is evaluated for each cell individually
- Multiple cell highlights can be applied to different columns

## Highlight Colors

The Smart DataGrid supports multiple ways to specify highlight colors.

### Named Colors

Use standard color names:

```
highlight rows where Total > 1000 color LightPink
highlight rows where Freight > 500 color Yellow
highlight cells in OrderID lessThan 1005 color Red
highlight rows where PaymentStatus equals Paid color LightGreen
```

**Supported Named Colors:**
- Red, Green, Blue
- Yellow, Orange, Purple
- Pink, LightPink
- LightGreen, LightBlue, LightYellow
- Gray, LightGray
- And other standard WPF color names

### Hex Colors

Use hexadecimal color codes:

```
highlight rows where Total > 1000 color #FFB6C1
highlight cells in Freight greaterThan 500 color #FFFF00
```

**Format:**
- `#RRGGBB` (e.g., `#FF0000` for red)
- `#AARRGGBB` (with alpha channel, e.g., `#80FF0000` for semi-transparent red)

### RGB Colors

Use RGB notation:

```
highlight rows where Quantity > 10 color RGB(255, 192, 203)
highlight cells in OrderID lessThan 1005 color RGB(255, 0, 0)
```

**Format:**
- `RGB(red, green, blue)` where each value is 0-255

### Default Highlight Color

If no color is specified in the prompt, the grid uses the `HighlightBrush` property value:

**XAML:**
```xml
<syncfusion:SfSmartDataGrid HighlightBrush="Orange"/>
```

**Code-Behind:**
```csharp
SmartGrid.HighlightBrush = new SolidColorBrush(Colors.Orange);
```

**Prompt without color:**
```
highlight rows where Total > 1000
```

**Result:** Rows are highlighted using the Orange brush.

## Combining Highlights

Multiple highlight rules can be applied simultaneously in a single command or through sequential commands.

### Single Command with Multiple Highlights

Apply both row and cell highlights in one prompt:

**Example Prompt:**
```
highlight rows where Total > 1000 color LightPink AND highlight cells in OrderID lessThan 1005 color Red
```

**Behavior:**
1. Rows with Total > 1000 are highlighted with LightPink
2. Cells in OrderID column with values < 1005 are highlighted with Red
3. Both highlights are active simultaneously

### Sequential Highlight Commands

Apply highlights one at a time:

**Commands:**
```
1. "highlight rows where Freight > 500 color Yellow"
2. "highlight cells in PaymentStatus equals Not Paid color Red"
```

**Behavior:**
- Both highlights accumulate
- Previous highlights remain active
- Each rule is independent

### Overlapping Highlights

When row and cell highlights overlap:

**Prompt:**
```
highlight rows where OrderID lessThan 1005 color LightPink AND highlight cells in OrderID lessThan 1003 color Red
```

**Result:**
- Rows with OrderID < 1005 have LightPink background
- Cells in OrderID column with values < 1003 have Red background (overrides row highlight for that cell)
- Cell highlights take precedence over row highlights

## Clearing Highlights

The Smart DataGrid provides selective clearing of highlights.

### Clear All Highlights

Remove all row and cell highlights:

**Example Prompts:**
```
clear highlight
clear highlights
remove all highlights
```

**Behavior:**
- All row highlights removed
- All cell highlights removed
- Grid returns to default styling

### Clear Row Highlights Only

Remove only row highlights, keeping cell highlights:

**Example Prompts:**
```
clear row highlight
remove row highlights
clear row highlighting
```

**Behavior:**
- All row highlights removed
- Cell highlights remain active

### Clear Cell Highlights Only

Remove only cell highlights, keeping row highlights:

**Example Prompts:**
```
clear cell highlight
remove cell highlights
clear cell highlighting
```

**Behavior:**
- All cell highlights removed
- Row highlights remain active

## Common Use Cases

### Use Case 1: Payment Status Monitoring

**Goal:** Visually identify unpaid orders.

**Command:**
```
highlight rows where PaymentStatus equals Not Paid color Red
```

**Result:** Unpaid orders stand out with red background for immediate attention.

### Use Case 2: High-Value Order Alerts

**Goal:** Emphasize high-freight orders.

**Command:**
```
highlight rows where Freight > 500 color LightPink
```

**Result:** Expensive shipments are visually distinct.

### Use Case 3: Multi-Condition Analysis

**Goal:** Highlight unpaid high-value orders in Germany.

**Command:**
```
filter Country equals Germany AND highlight rows where PaymentStatus equals Not Paid AND Freight > 500 color Red
```

**Result:**
- Shows only German orders (filtered)
- Highlights unpaid orders with freight > 500 (red background)

### Use Case 4: Column-Specific Alerts

**Goal:** Highlight low-quantity items in the Quantity column.

**Command:**
```
highlight cells in Quantity lessThan 5 color Orange
```

**Result:** Low-quantity cells in the Quantity column are orange, making inventory issues visible.

### Use Case 5: Multi-Color Highlighting

**Goal:** Use different colors for different priority levels.

**Commands:**
```
highlight rows where Freight > 1000 color Red AND highlight rows where Freight between 500 and 1000 color Yellow AND highlight rows where Freight < 500 color LightGreen
```

**Result:**
- High freight (>1000): Red
- Medium freight (500-1000): Yellow
- Low freight (<500): Light Green

### Use Case 6: Combined Filter and Highlight

**Goal:** Filter to specific region and highlight critical items.

**Commands:**
```
1. "filter Country equals Brazil"
2. "highlight rows where PaymentStatus equals Not Paid color Red"
```

**Result:**
- Shows only Brazilian orders
- Unpaid orders within Brazil are highlighted in red

## Best Practices

### 1. Use Descriptive Color Choices

**Good:**
```
highlight rows where PaymentStatus equals Not Paid color Red  // Red = warning/danger
highlight rows where PaymentStatus equals Paid color LightGreen  // Green = success
```

### 2. Combine Filtering and Highlighting

Filter first to narrow data, then highlight within filtered results:
```
1. "filter Country equals Germany"
2. "highlight rows where Freight > 500 color Yellow"
```

### 3. Use Cell Highlighting for Column-Specific Emphasis

When only one column needs attention:
```
highlight cells in Quantity lessThan 5 color Orange
```

Better than highlighting entire rows when only one value is critical.

### 4. Clear Before Applying New Highlights

For predictable results:
```
1. "clear highlight"
2. "highlight rows where Total > 1000 color LightPink"
```

### 5. Test Color Contrast

Ensure highlight colors are readable against text:
- Avoid dark colors for text on dark themes
- Test with actual data to verify readability

### 6. Use Selective Clearing for Iterative Exploration

```
1. "highlight rows where Freight > 500 color Yellow"
2. "highlight cells in PaymentStatus equals Not Paid color Red"
3. "clear row highlight"  // Keep cell highlights, remove row highlights
```

## Troubleshooting

### Issue: Filters Not Applied

**Possible Causes:**
- EnableSmartActions is false
- AI service not configured
- Column name doesn't match property name
- Operator not recognized

**Solution:**
- Set `EnableSmartActions="True"`
- Verify AI service setup
- Use exact property names
- Use supported operators (equals, contains, greaterThan, lessThan, between)

### Issue: Highlights Not Visible

**Possible Causes:**
- No rows/cells match the condition
- Color too similar to background
- HighlightBrush not set (when color not specified)

**Solution:**
- Verify condition matches some data
- Use contrasting colors
- Set HighlightBrush property as fallback

### Issue: Between Operator Not Working

**Cause:** Incorrect syntax.

**Correct Syntax:**
```
filter OrderID between 20251001 and 20251020
```

**Incorrect:**
```
filter OrderID between 20251001 to 20251020  // Use "and", not "to"
```

### Issue: Complex AND/OR Not Interpreted Correctly

**Cause:** Ambiguous logic or unsupported syntax.

**Solution:** Break into simpler commands or use explicit parentheses:
```
filter (Country equals Germany OR Country equals Brazil) AND Freight > 500
```

### Issue: Clear Commands Not Working

**Solution:** Use explicit clear keywords:
- `"clear filters"` (not "remove filter")
- `"clear highlight"` (not "unhighlight")
- `"clear row highlight"` (specific to rows)
- `"clear cell highlight"` (specific to cells)

## Advanced Filtering Scenarios

### Scenario 1: Date Range Filtering

**Command:**
```
filter OrderDate between 2026-01-01 and 2026-03-31
```

**Result:** Shows orders from Q1 2026.

### Scenario 2: Multi-Column AND Filter

**Command:**
```
filter Country equals Germany AND PaymentStatus equals Not Paid AND Freight greaterThan 500
```

**Result:** German unpaid orders with freight > 500.

### Scenario 3: Text Search Across Columns

**Commands (applied separately or combined):**
```
filter CustomerName contains Emma OR ProductName contains Monitor
```

**Result:** Shows orders where customer name includes "Emma" or product name includes "Monitor".

### Scenario 4: Negative Filtering

**Command:**
```
filter PaymentStatus not equals Paid
```

Or using AI interpretation:
```
filter PaymentStatus equals Not Paid
```

**Result:** Shows all unpaid orders.

## Advanced Highlighting Scenarios

### Scenario 1: Three-Tier Priority Highlighting

**Command:**
```
highlight rows where Freight > 1000 color Red AND highlight rows where Freight between 500 and 1000 color Yellow AND highlight rows where Freight < 500 color LightGreen
```

**Result:**
- High priority (>1000): Red
- Medium priority (500-1000): Yellow
- Low priority (<500): Light Green

### Scenario 2: Status-Based Multi-Column Highlighting

**Command:**
```
highlight rows where PaymentStatus equals Not Paid color LightPink AND highlight cells in Freight greaterThan 500 color Orange
```

**Result:**
- Unpaid orders have pink rows
- High-freight cells (within any row) are orange

### Scenario 3: Conditional Cell Highlighting

**Command:**
```
highlight cells in Quantity lessThan 5 color Red AND highlight cells in Quantity greaterThan 15 color Green
```

**Result:**
- Low inventory (< 5): Red cells
- High inventory (> 15): Green cells
- Normal inventory (5-15): No highlight

### Scenario 4: Filtered Highlighting

Combine filtering with highlighting for focused analysis:

**Commands:**
```
1. "filter Country equals Germany"
2. "highlight rows where Freight > 500 color Yellow"
```

**Result:**
- Only German orders are shown
- High-freight German orders are highlighted yellow

## Common Use Cases

### Use Case 1: Overdue Payment Tracking

**Goal:** Identify unpaid orders.

**Command:**
```
filter PaymentStatus equals Not Paid
highlight rows where Freight > 500 color Red
```

**Result:** Shows unpaid orders with high-value shipments highlighted.

### Use Case 2: Regional Sales Analysis

**Goal:** Analyze orders from specific regions.

**Command:**
```
filter Country equals Brazil OR Country equals Spain
group by Country
highlight rows where Freight > 500 color Yellow
```

**Result:** Brazilian and Spanish orders, grouped by country, with high-freight items highlighted.

### Use Case 3: Inventory Alerts

**Goal:** Find low-stock items.

**Command:**
```
highlight cells in Quantity lessThan 5 color Red
```

**Result:** Low-quantity cells are red, indicating potential stock issues.

### Use Case 4: Order Volume Analysis

**Goal:** Identify top customers by order frequency.

**Command:**
```
group by CustomerName
highlight rows where Quantity greaterThan 10 color LightBlue
```

**Result:** Orders grouped by customer, large quantity orders highlighted.

### Use Case 5: Date-Based Filtering

**Goal:** Show recent orders only.

**Command:**
```
filter OrderDate greaterThan 2026-03-01
sort by OrderDate descending
```

**Result:** Shows March 2026 orders, most recent first.

### Use Case 6: Multi-Criteria Highlighting

**Goal:** Different colors for different conditions.

**Command:**
```
highlight rows where PaymentStatus equals Not Paid color Red AND highlight rows where Freight > 1000 color Orange AND highlight cells in Quantity lessThan 5 color Yellow
```

**Result:**
- Unpaid orders: Red rows
- High freight: Orange rows
- Low quantity: Yellow cells
- Provides visual dashboard of critical data points

## Tips for Effective Filtering and Highlighting

1. **Start with Filtering, Then Highlight:** Narrow data first, then emphasize within results.
2. **Use Row Highlights for Overall Status:** Payment status, order priority, fulfillment stage.
3. **Use Cell Highlights for Specific Values:** Quantities, prices, dates that need attention.
4. **Combine with Grouping:** Group → Filter → Highlight for multi-layer analysis.
5. **Choose Contrasting Colors:** Ensure highlights are visible against grid theme.
6. **Use Selective Clearing:** Clear row or cell highlights independently for iterative exploration.
7. **Test Conditions First:** Apply filter to verify condition logic before highlighting.
