# Date Formatting in SfDatePicker

## Table of Contents
- [FormatString Property](#formatstring-property)
- [Standard Format Specifiers](#standard-format-specifiers)
- [SelectorFormatString Property](#selectorformatstring-property)
- [Common Format Examples](#common-format-examples)
- [Custom Format Patterns](#custom-format-patterns)

## FormatString Property

The **FormatString** property controls how the selected date is **displayed** in the text portion of the SfDatePicker control. It also affects how dates are validated when the user types directly into the control.

### Default Value
The default value is **"d"**, which displays the short date format based on the system's regional settings (e.g., "3/19/2026" in en-US).

### Via XAML
```xaml
<syncfusion:SfDatePicker FormatString="M" 
                         x:Name="sfDatePicker"/>
```

### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.FormatString = "M";
```

### How FormatString Affects Display
- Determines what the user sees in the control's text box
- Applies to both display and inline editing
- Influences validation when user types dates
- Affects the .ToString() representation of the date

**Example:**
- Input date: May 30, 2026
- FormatString "d" → "5/30/2026"
- FormatString "M" → "May"
- FormatString "yyyy" → "2026"

## Standard Format Specifiers

The FormatString property accepts standard .NET date/time format specifiers:

| Specifier | Description | Example |
|-----------|-------------|---------|
| **d** | Short date pattern | 3/19/2026 |
| **D** | Long date pattern | Wednesday, March 19, 2026 |
| **f** | Full date/time (short time) | Wednesday, March 19, 2026 12:00 PM |
| **F** | Full date/time (long time) | Wednesday, March 19, 2026 12:00:00 PM |
| **g** | General (short date, short time) | 3/19/2026 12:00 PM |
| **M** | Month day pattern | March 19 |
| **m** | Month day pattern (same as M) | March 19 |
| **y** | Year month pattern | March 2026 |
| **Y** | Year month pattern (same as y) | March 2026 |
| **yyyy** | 4-digit year | 2026 |
| **yy** | 2-digit year | 26 |
| **MM** | 2-digit month | 03 |
| **M** | 1 or 2-digit month | 3 |
| **dd** | 2-digit day | 19 |
| **d** | 1 or 2-digit day | 19 |

### Common Format Combinations
```xaml
<!-- Month-only selection -->
<syncfusion:SfDatePicker FormatString="M"/>

<!-- Year-month display -->
<syncfusion:SfDatePicker FormatString="y"/>

<!-- Long date format -->
<syncfusion:SfDatePicker FormatString="D"/>

<!-- Custom format: MM/dd/yyyy -->
<syncfusion:SfDatePicker FormatString="MM/dd/yyyy"/>
```

## SelectorFormatString Property

The **SelectorFormatString** property controls which **selectors appear in the dropdown date picker** (the visual cell selectors for day, month, and year). This is different from FormatString.

### Default Value
The default value is **"M/d/yyyy"**, which shows date, month, and year selectors.

### Purpose
- Determines which selection cells appear in the SfDateSelector dropdown
- User can only select the components shown
- Does not affect the displayed text format in the textbox

### Via XAML
```xaml
<syncfusion:SfDatePicker SelectorFormatString="M" 
                         x:Name="sfDatePicker"/>
```

### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.SelectorFormatString = "M";
```

### Selector Format Examples

| SelectorFormatString | What User Can Select | Display |
|---------------------|----------------------|---------|
| **"M/d/yyyy"** | Month, Day, Year | 3 columns of selectors |
| **"M"** | Month only | 1 column (months) |
| **"d"** | Day only | 1 column (days) |
| **"yyyy"** | Year only | 1 column (years) |
| **"M/d"** | Month and Day | 2 columns (months, days) |
| **"d/yyyy"** | Day and Year | 2 columns (days, years) |

### Practical Example: Month Selection Only
```xaml
<syncfusion:SfDatePicker x:Name="sfDatePicker"
                         FormatString="M"
                         SelectorFormatString="M">
    <!-- User can only select a month; year and day default to today -->
</syncfusion:SfDatePicker>
```

### Practical Example: Year Selection Only
```xaml
<syncfusion:SfDatePicker x:Name="sfDatePicker"
                         FormatString="yyyy"
                         SelectorFormatString="yyyy">
    <!-- User can only select a year; month and day default to today -->
</syncfusion:SfDatePicker>
```

## Common Format Examples

### Example 1: Short Date Format
```xaml
<syncfusion:SfDatePicker FormatString="d" x:Name="sfDatePicker"/>
<!-- Display: 3/19/2026 -->
```

### Example 2: Long Date Format
```xaml
<syncfusion:SfDatePicker FormatString="D" x:Name="sfDatePicker"/>
<!-- Display: Wednesday, March 19, 2026 -->
```

### Example 3: Month-Year Only
```xaml
<syncfusion:SfDatePicker FormatString="y" x:Name="sfDatePicker"/>
<!-- Display: March 2026 -->
```

### Example 4: Custom Format MM/dd/yyyy
```xaml
<syncfusion:SfDatePicker FormatString="MM/dd/yyyy" x:Name="sfDatePicker"/>
<!-- Display: 03/19/2026 -->
```

### Example 5: ISO Format yyyy-MM-dd
```xaml
<syncfusion:SfDatePicker FormatString="yyyy-MM-dd" x:Name="sfDatePicker"/>
<!-- Display: 2026-03-19 -->
```

### Example 6: Display Full DateTime
```xaml
<syncfusion:SfDatePicker FormatString="F" x:Name="sfDatePicker"/>
<!-- Display: Wednesday, March 19, 2026 12:00:00 PM -->
```

## Custom Format Patterns

In addition to standard specifiers, you can create custom format patterns using format codes:

### Custom Pattern Syntax
```
d  = Day (1-31)
dd = Day (01-31, zero-padded)
M  = Month (1-12)
MM = Month (01-12, zero-padded)
MMM = Month name (Jan, Feb, etc.)
MMMM = Full month name (January, February, etc.)
y  = Year (no leading zeros)
yy = Year (2-digit, 09 for 2009)
yyyy = Year (4-digit, 2009)
```

### Custom Pattern Examples

**Pattern: "dd/MM/yyyy"**
```
Input Date: March 19, 2026
Output: 19/03/2026
```

**Pattern: "MMMM dd, yyyy"**
```
Input Date: March 19, 2026
Output: March 19, 2026
```

**Pattern: "dd-MMM-yy"**
```
Input Date: March 19, 2026
Output: 19-Mar-26
```

**Pattern: "yyyy.MM.dd"**
```
Input Date: March 19, 2026
Output: 2026.03.19
```

### Applying Custom Patterns via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();

// European format: day/month/year
sfDatePicker.FormatString = "dd/MM/yyyy";

// With month name: 19 Mar 2026
sfDatePicker.FormatString = "dd MMM yyyy";

// Long format: March 19, 2026
sfDatePicker.FormatString = "MMMM dd, yyyy";
```

## Key Differences: FormatString vs SelectorFormatString

| Aspect | FormatString | SelectorFormatString |
|--------|--------------|---------------------|
| **Purpose** | Controls text display | Controls dropdown selectors |
| **Affects** | What user sees in textbox | Which cells appear in picker |
| **Default** | "d" (3/19/2026) | "M/d/yyyy" (3 columns) |
| **Example** | FormatString="M" shows "March" | SelectorFormatString="M" shows 1 month selector |

## Regional Settings Impact

Format output is influenced by the system's **Regional Options** (Regional and Language Settings). Different cultures produce different results:

- **en-US:** 3/19/2026
- **en-GB:** 19/03/2026
- **de-DE:** 19.03.2026

To ensure consistent formatting across different regions, use explicit custom patterns (e.g., "yyyy-MM-dd") rather than culture-dependent patterns.

## Tips for Best Practices

1. **Use standard specifiers** (d, D, M, y, yyyy) for most cases—they adapt to regional settings
2. **Use custom patterns** when you need consistent, region-independent formatting
3. **Match FormatString and SelectorFormatString** for consistency (e.g., both show "M" for month-only)
4. **Test with different regional settings** if targeting international users
5. **Document your format choice** in code comments for future maintainers
