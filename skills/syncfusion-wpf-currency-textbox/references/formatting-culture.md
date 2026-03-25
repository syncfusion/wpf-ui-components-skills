# Culture and Number Formatting

## Table of Contents
- [Overview](#overview)
- [Culture-Based Formatting](#culture-based-formatting)
- [NumberFormatInfo Configuration](#numberformatinfo-configuration)
- [Dedicated Formatting Properties](#dedicated-formatting-properties)
- [Currency Patterns](#currency-patterns)
- [Format Priority Rules](#format-priority-rules)
- [Common Formatting Scenarios](#common-formatting-scenarios)

## Overview

The CurrencyTextBox provides three flexible approaches to format currency values:

1. **Culture property** - Automatic formatting based on regional settings
2. **NumberFormat property** - Custom NumberFormatInfo configuration
3. **Dedicated properties** - Individual property settings (CurrencySymbol, CurrencyDecimalDigits, etc.)

Each approach has different use cases and priority levels when multiple are used together.

## Culture-Based Formatting

The `Culture` property applies culture-specific formatting for decimal separators, group separators, and currency symbols.

### Setting Culture Property

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Height="25" 
    Width="150" 
    Culture="fr-FR" 
    Value="1234567"/>
```

**C#:**

```csharp
using System.Globalization;

CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 150;
currencyTextBox.Height = 25;
currencyTextBox.Value = 1234567;
currencyTextBox.Culture = new CultureInfo("fr-FR");
```

### Culture Examples

**US Culture (en-US):**
- Group Separator: `,`
- Decimal Separator: `.`
- Currency Symbol: `$`
- Result: `$1,234,567.00`

**French Culture (fr-FR):**
- Group Separator: ` ` (space)
- Decimal Separator: `,`
- Currency Symbol: `€`
- Result: `1 234 567,00 €`

**German Culture (de-DE):**
- Group Separator: `.`
- Decimal Separator: `,`
- Currency Symbol: `€`
- Result: `1.234.567,00 €`

**Japanese Culture (ja-JP):**
- Group Separator: `,`
- Decimal Separator: `.`
- Currency Symbol: `¥`
- Result: `¥1,234,567`

### Multiple Culture Example

```xml
<StackPanel>
    <TextBlock Text="US Format:"/>
    <syncfusion:CurrencyTextBox Value="1234567.89" Culture="en-US" Width="150"/>
    
    <TextBlock Text="UK Format:" Margin="0,10,0,0"/>
    <syncfusion:CurrencyTextBox Value="1234567.89" Culture="en-GB" Width="150"/>
    
    <TextBlock Text="India Format:" Margin="0,10,0,0"/>
    <syncfusion:CurrencyTextBox Value="1234567.89" Culture="en-IN" Width="150"/>
</StackPanel>
```

## NumberFormatInfo Configuration

The `NumberFormat` property accepts a `NumberFormatInfo` object for complete formatting control.

### Basic NumberFormat Example

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Height="25" 
    Width="150"  
    Value="1234567">
    <syncfusion:CurrencyTextBox.NumberFormat>
        <numberformat:NumberFormatInfo 
            CurrencyGroupSeparator="/" 
            CurrencyDecimalDigits="4" 
            CurrencyDecimalSeparator="*" 
            CurrencySymbol="$"/>
    </syncfusion:CurrencyTextBox.NumberFormat>
</syncfusion:CurrencyTextBox>
```

**Result:** `$1/234/567*0000`

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 150;
currencyTextBox.Height = 25;
currencyTextBox.Value = 1234567;
currencyTextBox.NumberFormat = new NumberFormatInfo()
{
    CurrencyGroupSeparator = "/",
    CurrencyDecimalDigits = 4,
    CurrencyDecimalSeparator = "*",
    CurrencySymbol = "$"
};
```

### NumberFormatInfo Properties

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `CurrencySymbol` | `string` | Currency symbol | "$", "€", "£" |
| `CurrencyGroupSeparator` | `string` | Thousands separator | ",", ".", " " |
| `CurrencyDecimalSeparator` | `string` | Decimal separator | ".", "," |
| `CurrencyDecimalDigits` | `int` | Decimal places | 2, 3, 4 |
| `CurrencyGroupSizes` | `int[]` | Grouping pattern | {3}, {2,3,4} |
| `CurrencyPositivePattern` | `int` | Positive format (0-3) | 0 = $n |
| `CurrencyNegativePattern` | `int` | Negative format (0-15) | 1 = -$n |

### Currency Group Sizes

Control how digits are grouped using `CurrencyGroupSizes`.

**C# Example:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 150;
currencyTextBox.Height = 25;
currencyTextBox.Value = 123456789;
currencyTextBox.NumberFormat = new NumberFormatInfo()
{
    CurrencySymbol = "&",
    CurrencyDecimalDigits = 4,
    CurrencyGroupSeparator = "/",
    CurrencyDecimalSeparator = "*",
    // Grouping from right to left: 2, then 3, then 4
    CurrencyGroupSizes = new int[] { 2, 3, 4 }
};
```

**Result:** `&1234/567/89*0000`

**Explanation:** 
- Rightmost group: 2 digits (89)
- Next group: 3 digits (567)
- Remaining groups: 4 digits (1234)

## Dedicated Formatting Properties

Individual properties provide granular control without creating a NumberFormatInfo object.

### Available Properties

- `CurrencySymbol`
- `CurrencyDecimalDigits`
- `CurrencyDecimalSeparator`
- `CurrencyGroupSeparator`
- `CurrencyGroupSizes`
- `GroupSeperatorEnabled` (note the typo in the property name)
- `CurrencyPositivePattern`
- `CurrencyNegativePattern`

### Basic Example

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 150;
currencyTextBox.Height = 25;
currencyTextBox.Value = 123456789;
currencyTextBox.CurrencySymbol = "#";
currencyTextBox.CurrencyDecimalDigits = 4;
currencyTextBox.GroupSeperatorEnabled = true; // Note: Property has typo
currencyTextBox.CurrencyGroupSeparator = "/";
currencyTextBox.CurrencyDecimalSeparator = "*";
```

**Result:** `#123/456/789*0000`

### CurrencyGroupSizes with Dedicated Properties

**C#:**

```csharp
using Syncfusion.Windows.Shared;

CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 123456789;
currencyTextBox.CurrencySymbol = "#";
currencyTextBox.CurrencyDecimalDigits = 4;
currencyTextBox.GroupSeperatorEnabled = true;
currencyTextBox.CurrencyGroupSeparator = "/";
currencyTextBox.CurrencyDecimalSeparator = "*";

// Adding currency group size via CurrencyGroupSizes property
currencyTextBox.CurrencyGroupSizes = new Int32Collection() { 4, 3, 2 };
```

**Result:** `#12/345/6789*0000`

## Currency Patterns

### Positive Value Pattern

The `CurrencyPositivePattern` property customizes the position of the currency symbol relative to the number.

**Pattern Table:**

| Value | Pattern | Example (1234) |
|-------|---------|----------------|
| 0 | $n | $1234 |
| 1 | n$ | 1234$ |
| 2 | $ n | $ 1234 |
| 3 | n $ | 1234 $ |

**Example:**

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    Height="25" 
    Width="150" 
    Value="1234" 
    CurrencyPositivePattern="3"/>
```

**Result:** `1234 $`

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234;
currencyTextBox.CurrencyPositivePattern = 3; // n $
```

### Negative Value Pattern

The `CurrencyNegativePattern` property customizes negative currency display (16 patterns available).

**Pattern Table:**

| Value | Pattern | Example (-1234) |
|-------|---------|-----------------|
| 0 | ($n) | ($1234) |
| 1 | -$n | -$1234 |
| 2 | $-n | $-1234 |
| 3 | $n- | $1234- |
| 4 | (n$) | (1234$) |
| 5 | -n$ | -1234$ |
| 6 | n-$ | 1234-$ |
| 7 | n$- | 1234$- |
| 8 | -n $ | -1234 $ |
| 9 | -$ n | -$ 1234 |
| 10 | n $- | 1234 $- |
| 11 | $ n- | $ 1234- |
| 12 | $ -n | $ -1234 |
| 13 | n- $ | 1234- $ |
| 14 | ($ n) | ($ 1234) |
| 15 | (n $) | (1234 $) |

**Example:**

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    Height="25" 
    Width="150" 
    Value="-1234" 
    CurrencyNegativePattern="0"/>
```

**Result:** `($1234)`

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = -1234;
currencyTextBox.CurrencyNegativePattern = 0; // ($n)
```

## Format Priority Rules

When multiple formatting approaches are used simultaneously, the following priority applies:

### Priority Hierarchy

1. **Highest Priority:** Dedicated properties (`CurrencyGroupSeparator`, `CurrencyGroupSizes`)
2. **Medium Priority:** NumberFormat property
3. **Lowest Priority:** Culture property

### Priority Examples

**Example 1: NumberFormat overrides Culture**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234567;
currencyTextBox.Culture = new CultureInfo("en-US"); // $ and comma separator
currencyTextBox.NumberFormat = new NumberFormatInfo()
{
    CurrencySymbol = "€",
    CurrencyGroupSeparator = "."
};
// Result uses NumberFormat: €1.234.567,00
```

**Example 2: Dedicated properties override NumberFormat**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234567;
currencyTextBox.NumberFormat = new NumberFormatInfo()
{
    CurrencyGroupSeparator = ",",
    CurrencySymbol = "$"
};
// This overrides NumberFormat
currencyTextBox.CurrencyGroupSeparator = "/";
// Result: $1/234/567.00
```

**Example 3: Complete override chain**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234567.89;

// Set all three (dedicated properties win)
currencyTextBox.Culture = new CultureInfo("en-US");
currencyTextBox.NumberFormat = new NumberFormatInfo()
{
    CurrencySymbol = "€",
    CurrencyGroupSeparator = "."
};
currencyTextBox.CurrencyGroupSeparator = " ";
currencyTextBox.CurrencySymbol = "£";

// Result: £1 234 567.89 (dedicated properties used)
```

## Common Formatting Scenarios

### Scenario 1: European Format

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234567.89;
currencyTextBox.CurrencySymbol = "€";
currencyTextBox.CurrencyDecimalSeparator = ",";
currencyTextBox.CurrencyGroupSeparator = ".";
currencyTextBox.GroupSeperatorEnabled = true;
currencyTextBox.CurrencyDecimalDigits = 2;
// Result: €1.234.567,89
```

### Scenario 2: Accounting Format with Parentheses

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = -5000;
currencyTextBox.CurrencySymbol = "$";
currencyTextBox.CurrencyNegativePattern = 0; // ($n)
// Result: ($5,000.00)
```

### Scenario 3: Indian Numbering System

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 12345678;
currencyTextBox.CurrencySymbol = "₹";
currencyTextBox.GroupSeperatorEnabled = true;
currencyTextBox.CurrencyGroupSeparator = ",";
currencyTextBox.CurrencyGroupSizes = new Int32Collection() { 3, 2 };
// Result: ₹1,23,45,678.00
```

### Scenario 4: Custom Symbol and High Precision

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234.56789;
currencyTextBox.CurrencySymbol = "BTC";
currencyTextBox.CurrencyDecimalDigits = 8;
currencyTextBox.CurrencyPositivePattern = 1; // n$
// Result: 1234.56789000BTC
```

### Scenario 5: No Decimal Places

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234567;
currencyTextBox.CurrencySymbol = "¥";
currencyTextBox.CurrencyDecimalDigits = 0;
currencyTextBox.GroupSeperatorEnabled = true;
// Result: ¥1,234,567
```

## Best Practices

1. **Choose one approach** - Don't mix Culture, NumberFormat, and dedicated properties unless necessary
2. **Use Culture for standard locales** - Simplest for international apps
3. **Use dedicated properties for customization** - When you need specific formatting
4. **Test negative values** - Ensure negative patterns display correctly
5. **Consider user locale** - Respect system culture when appropriate
6. **Validate decimal digits** - Match your business rules for precision

## Troubleshooting

**Issue:** Format doesn't apply  
**Solution:** Check `GroupSeperatorEnabled` is true when using group separators

**Issue:** Dedicated properties not working  
**Solution:** Ensure they're set after NumberFormat or Culture

**Issue:** Unexpected decimal places  
**Solution:** `CurrencyDecimalDigits` defaults to culture; explicitly set if needed

**Issue:** Group separator not showing  
**Solution:** Set `GroupSeperatorEnabled = true`
