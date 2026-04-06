# Number Formatting and Culture

## Table of Contents
# Number Formatting and Culture (summary)

This summary highlights the formatting controls available on `PercentTextBox`:

- Use `Culture` for broad, locale-appropriate formats (group/decimal separators and percent symbol).
- For finer control, provide a `NumberFormatInfo` via `NumberFormat` or set dedicated properties (`PercentGroupSeparator`, `PercentDecimalSeparator`, `PercentGroupSizes`, `PercentDecimalDigits`).
- Display of group separators requires `GroupSeperatorEnabled = true`.
- `PercentPositivePattern` and `PercentNegativePattern` control placement of the percent symbol relative to the number.
- Precedence: Dedicated properties > `NumberFormat` > `Culture`.

Typical minimal example:

```xml
<syncfusion:PercentTextBox PercentValue="12345.67"
                           GroupSeperatorEnabled="True"
                           PercentDecimalDigits="2"/>
```

Use `Culture` when you want to follow user locale; use dedicated properties when you must guarantee a specific symbol or separator regardless of culture.
};

percentTextBox.NumberFormat = format;
percentTextBox.PercentGroupSeparator = ","; // This wins
percentTextBox.GroupSeperatorEnabled = true;

// Result uses "," not "."
```

### Precedence Example 2: NumberFormat > Culture

```csharp
// Culture says use "." for group separator
percentTextBox.Culture = new CultureInfo("de-DE");

// NumberFormat overrides culture
percentTextBox.NumberFormat = new NumberFormatInfo
{
    PercentGroupSeparator = "," // This wins over culture
};
```

### Precedence Example 3: All Three Combined

```csharp
// Culture (lowest priority)
percentTextBox.Culture = new CultureInfo("en-US"); // Group: ",", Decimal: "."

// NumberFormat (medium priority)
percentTextBox.NumberFormat = new NumberFormatInfo
{
    PercentGroupSeparator = "*", // Overrides culture ","
    PercentDecimalSeparator = "." // Uses this
};

// Dedicated property (highest priority)
percentTextBox.PercentGroupSeparator = "|"; // Overrides NumberFormat "*"
percentTextBox.GroupSeperatorEnabled = true;

// Final result uses: "|" for group, "." for decimal
```

### Best Practices

For consolidated best practices and guidance, see [references/best-practices.md](references/best-practices.md).

## Complete Formatting Examples

### Example 1: US Currency-Style Percentage

```xml
<syncfusion:PercentTextBox PercentValue="1234567.89"
                           Culture="en-US"
                           PercentDecimalDigits="2"
                           GroupSeperatorEnabled="True"/>
<!-- Result: 1,234,567.89 % -->
```

### Example 2: European-Style Percentage

```xml
<syncfusion:PercentTextBox PercentValue="1234567.89"
                           PercentGroupSeparator="."
                           PercentDecimalSeparator=","
                           PercentDecimalDigits="2"
                           GroupSeperatorEnabled="True"/>
<!-- Result: 1.234.567,89 % -->
```

### Example 3: Custom Format with Pattern

```xml
<syncfusion:PercentTextBox PercentValue="-1234.56"
                           PercentGroupSeparator="_"
                           PercentDecimalDigits="2"
                           PercentPositivePattern="1"
                           PercentNegativePattern="1"
                           GroupSeperatorEnabled="True"/>
<!-- Result: -1_234.56% -->
```

### Example 4: Indian Numbering System

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 12345678.90;
percentTextBox.GroupSeperatorEnabled = true;
percentTextBox.PercentGroupSeparator = ",";
percentTextBox.PercentDecimalSeparator = ".";
percentTextBox.PercentDecimalDigits = 2;
percentTextBox.PercentGroupSizes = new Int32Collection { 3, 2 };

// Result: 1,23,45,678.90 %
```
