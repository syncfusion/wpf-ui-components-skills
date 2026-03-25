# Number Formatting and Culture

## Table of Contents
- [Culture-Based Formatting](#culture-based-formatting)
- [NumberFormatInfo Configuration](#numberformatinfo-configuration)
- [Dedicated Formatting Properties](#dedicated-formatting-properties)
- [Group Separators and Sizes](#group-separators-and-sizes)
- [Decimal Configuration](#decimal-configuration)
- [Percent Symbol Customization](#percent-symbol-customization)
- [Positive and Negative Patterns](#positive-and-negative-patterns)
- [Property Precedence](#property-precedence)

## Culture-Based Formatting

Apply culture-specific number formatting using the `Culture` property.

### Basic Culture Setup

```xml
<syncfusion:PercentTextBox PercentValue="1234567.89"
                           Culture="en-US"/>
```

**Result:** "1,234,567.89 %" (US format: comma for thousands, period for decimal)

### Different Culture Examples

**German (Germany):**
```xml
<syncfusion:PercentTextBox PercentValue="1234567.89"
                           Culture="de-DE"/>
```
**Result:** "1.234.567,89 %" (period for thousands, comma for decimal)

**French (France):**
```xml
<syncfusion:PercentTextBox PercentValue="1234567.89"
                           Culture="fr-FR"/>
```
**Result:** "1 234 567,89 %" (space for thousands, comma for decimal)

**Latin (Bosnia and Herzegovina):**
```xml
<syncfusion:PercentTextBox PercentValue="1234567.89"
                           Culture="bs-Latn"/>
```
**Result:** "1.234.567,89 %" (period for thousands, comma for decimal)

### Programmatic Culture Setting

```csharp
using System.Globalization;

PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 1234567.89;

// Set German culture
percentTextBox.Culture = new CultureInfo("de-DE");

// Or US culture
percentTextBox.Culture = new CultureInfo("en-US");

// Or use current thread culture
percentTextBox.Culture = CultureInfo.CurrentCulture;
```

### Culture Comparison Table

| Culture | Group Separator | Decimal Separator | Example |
|---------|----------------|-------------------|---------|
| en-US | , (comma) | . (period) | 1,234.56 % |
| de-DE | . (period) | , (comma) | 1.234,56 % |
| fr-FR | (space) | , (comma) | 1 234,56 % |
| ja-JP | , (comma) | . (period) | 1,234.56 % |
| ar-SA | , (comma) | . (period) | ٪1,234.56 |

## NumberFormatInfo Configuration

Customize number formatting using the `NumberFormat` property with `NumberFormatInfo` object.

### Basic NumberFormat Setup

```xml
<syncfusion:PercentTextBox PercentValue="1234567.89">
    <syncfusion:PercentTextBox.NumberFormat>
        <numberformat:NumberFormatInfo PercentGroupSeparator="/"
                                       PercentSymbol="%"
                                       PercentDecimalDigits="4"
                                       PercentDecimalSeparator="*"/>
    </syncfusion:PercentTextBox.NumberFormat>
</syncfusion:PercentTextBox>
```

**Result:** "1/234/567*8900 %"

### Programmatic NumberFormat Setup

```csharp
using System.Globalization;

PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 1234567.89;

NumberFormatInfo numberFormat = new NumberFormatInfo
{
    PercentGroupSeparator = "/",
    PercentDecimalDigits = 4,
    PercentDecimalSeparator = "*",
    PercentSymbol = "%"
};

percentTextBox.NumberFormat = numberFormat;
```

### Complete NumberFormatInfo Example

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 123456789.12345;

NumberFormatInfo format = new NumberFormatInfo
{
    // Separators
    PercentGroupSeparator = ",",
    PercentDecimalSeparator = ".",
    
    // Decimal places
    PercentDecimalDigits = 2,
    
    // Symbol
    PercentSymbol = "%",
    
    // Group sizes (right to left)
    PercentGroupSizes = new int[] { 3 } // Groups of 3
};

percentTextBox.NumberFormat = format;
```

**Result:** "123,456,789.12 %"

## Dedicated Formatting Properties

PercentTextBox provides dedicated properties for common formatting needs.

### PercentGroupSeparator

Character used to separate digit groups:

```xml
<syncfusion:PercentTextBox PercentValue="123456789"
                           PercentGroupSeparator="*"
                           GroupSeperatorEnabled="True"/>
```

**Result:** "123*456*789 %"

```csharp
percentTextBox.PercentGroupSeparator = "_";
percentTextBox.GroupSeperatorEnabled = true;
// Result: 123_456_789 %
```

### PercentDecimalSeparator

Character used to separate decimal portion:

```xml
<syncfusion:PercentTextBox PercentValue="1234.56"
                           PercentDecimalSeparator=":"/>
```

**Result:** "1234:56 %"

```csharp
percentTextBox.PercentDecimalSeparator = ",";
// German-style decimal separator
```

### PercentageSymbol

Custom percent symbol:

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           PercentageSymbol=" percent"/>
```

**Result:** "50 percent"

```csharp
percentTextBox.PercentageSymbol = "pct";
// Result: 50 pct
```

### GroupSeperatorEnabled

Enable/disable group separator display:

```xml
<!-- With group separator -->
<syncfusion:PercentTextBox PercentValue="123456"
                           GroupSeperatorEnabled="True"/>
<!-- Result: 123,456 % -->

<!-- Without group separator -->
<syncfusion:PercentTextBox PercentValue="123456"
                           GroupSeperatorEnabled="False"/>
<!-- Result: 123456 % -->
```

**Important:** Group separator must be enabled explicitly to display, even if separator is defined.

## Group Separators and Sizes

### PercentGroupSizes

Define custom grouping patterns (applied right to left):

**XAML (using Int32Collection):**
```xml
<syncfusion:PercentTextBox PercentValue="123456789">
    <syncfusion:PercentTextBox.PercentGroupSizes>
        <syncfusion:Int32Collection>
            <x:Int32>2</x:Int32>
            <x:Int32>3</x:Int32>
            <x:Int32>4</x:Int32>
        </syncfusion:Int32Collection>
    </syncfusion:PercentTextBox.PercentGroupSizes>
</syncfusion:PercentTextBox>
```

**C#:**
```csharp
using Syncfusion.Windows.Shared;

PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 123456789;
percentTextBox.GroupSeperatorEnabled = true;
percentTextBox.PercentGroupSeparator = ",";

// Group sizes: 2, then 3, then 4, repeating last
percentTextBox.PercentGroupSizes = new Int32Collection { 2, 3, 4 };
```

**Result:** "1,2345,678,89 %" (groups of 2, 3, 4 from right to left)

### Understanding Group Sizes

Group sizes are applied **right to left** (from decimal point):

**Example: {3, 2}**
- Value: 123456789
- First group (rightmost): 3 digits → "...789"
- Second group: 2 digits → "...67,789"
- Remaining: Last size repeats → "1,23,45,67,789"

```csharp
percentTextBox.PercentGroupSizes = new Int32Collection { 3, 2 };
// Result: 1,23,45,67,89 %
```

### Standard Group Sizes

**US Standard (groups of 3):**
```csharp
percentTextBox.PercentGroupSizes = new Int32Collection { 3 };
// Result: 123,456,789 %
```

**Indian Numbering System:**
```csharp
percentTextBox.PercentGroupSizes = new Int32Collection { 3, 2 };
// Result: 12,34,56,789 %
```

### Group Sizes with NumberFormat

```csharp
NumberFormatInfo format = new NumberFormatInfo
{
    PercentGroupSeparator = ",",
    PercentDecimalSeparator = ".",
    PercentSymbol = "%",
    PercentGroupSizes = new int[] { 2, 3, 4 }
};

percentTextBox.NumberFormat = format;
percentTextBox.PercentValue = 123456789;
// Result: 1,2345,678,89 %
```

## Decimal Configuration

### PercentDecimalDigits

Set number of decimal places:

```xml
<syncfusion:PercentTextBox PercentValue="123.456789"
                           PercentDecimalDigits="2"/>
```

**Result:** "123.46 %" (rounded)

```xml
<syncfusion:PercentTextBox PercentValue="100"
                           PercentDecimalDigits="4"/>
```

**Result:** "100.0000 %"

### Complete Decimal Configuration Example

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 123456.789;
percentTextBox.PercentDecimalDigits = 2;
percentTextBox.PercentDecimalSeparator = ".";
percentTextBox.GroupSeperatorEnabled = true;
percentTextBox.PercentGroupSeparator = ",";

// Result: 123,456.79 %
```

## Percent Symbol Customization

### Different Symbol Styles

**Standard:**
```xml
<syncfusion:PercentTextBox PercentValue="50"
                           PercentageSymbol="%"/>
<!-- Result: 50 % -->
```

**Text-based:**
```xml
<syncfusion:PercentTextBox PercentValue="50"
                           PercentageSymbol=" percent"/>
<!-- Result: 50 percent -->
```

**Abbreviated:**
```xml
<syncfusion:PercentTextBox PercentValue="50"
                           PercentageSymbol="pct"/>
<!-- Result: 50pct -->
```

**Empty (no symbol):**
```xml
<syncfusion:PercentTextBox PercentValue="50"
                           PercentageSymbol=""/>
<!-- Result: 50 -->
```

## Positive and Negative Patterns

### PercentPositivePattern

Define layout for positive values (4 patterns available):

**Pattern Table:**

| Value | Pattern | Example (50%) |
|-------|---------|---------------|
| 0 | n % | 50 % |
| 1 | n% | 50% |
| 2 | %n | %50 |
| 3 | % n | % 50 |

Where: `n` = number, `%` = percent symbol

**XAML:**
```xml
<!-- Pattern 0: "n %" (default) -->
<syncfusion:PercentTextBox PercentValue="50"
                           PercentPositivePattern="0"/>
<!-- Result: 50 % -->

<!-- Pattern 1: "n%" -->
<syncfusion:PercentTextBox PercentValue="50"
                           PercentPositivePattern="1"/>
<!-- Result: 50% -->

<!-- Pattern 2: "%n" -->
<syncfusion:PercentTextBox PercentValue="50"
                           PercentPositivePattern="2"/>
<!-- Result: %50 -->

<!-- Pattern 3: "% n" -->
<syncfusion:PercentTextBox PercentValue="50"
                           PercentPositivePattern="3"/>
<!-- Result: % 50 -->
```

**C#:**
```csharp
percentTextBox.PercentPositivePattern = 1; // No space: 50%
```

### PercentNegativePattern

Define layout for negative values (12 patterns available):

**Pattern Table:**

| Value | Pattern | Example (-50%) |
|-------|---------|----------------|
| 0 | -n % | -50 % |
| 1 | -n% | -50% |
| 2 | -%n | -%50 |
| 3 | %-n | %-50 |
| 4 | %n- | %50- |
| 5 | n-% | 50-% |
| 6 | n%- | 50%- |
| 7 | -% n | -% 50 |
| 8 | n %- | 50 %- |
| 9 | % n- | % 50- |
| 10 | % -n | % -50 |
| 11 | n- % | 50- % |

**XAML:**
```xml
<!-- Pattern 0: "-n %" (default) -->
<syncfusion:PercentTextBox PercentValue="-50"
                           PercentNegativePattern="0"/>
<!-- Result: -50 % -->

<!-- Pattern 2: "-%n" -->
<syncfusion:PercentTextBox PercentValue="-50"
                           PercentNegativePattern="2"/>
<!-- Result: -%50 -->

<!-- Pattern 7: "-% n" -->
<syncfusion:PercentTextBox PercentValue="-50"
                           PercentNegativePattern="7"/>
<!-- Result: -% 50 -->
```

**C#:**
```csharp
percentTextBox.PercentNegativePattern = 1; // -50%
```

### Pattern Usage Examples

**Accounting Style (Pattern 3):**
```xml
<syncfusion:PercentTextBox PercentValue="-25"
                           PercentNegativePattern="3"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"/>
<!-- Result: %-25 (in red) -->
```

**Scientific Style:**
```xml
<syncfusion:PercentTextBox PercentValue="75"
                           PercentPositivePattern="1"
                           PercentageSymbol="%"/>
<!-- Result: 75% (no space) -->
```

## Property Precedence

When multiple formatting properties are set, precedence determines which takes effect.

### Precedence Hierarchy

**Highest to Lowest:**
1. **Dedicated Properties** (PercentGroupSeparator, PercentGroupSizes, PercentDecimalSeparator, etc.)
2. **NumberFormat** (NumberFormatInfo object)
3. **Culture** (CultureInfo object)

### Precedence Example 1: Dedicated > NumberFormat

```csharp
NumberFormatInfo format = new NumberFormatInfo
{
    PercentGroupSeparator = ".", // Will be overridden
    PercentDecimalDigits = 2
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

### Best Practice: Choose One Approach

**Option 1: Culture-based (simplest)**
```xml
<syncfusion:PercentTextBox Culture="de-DE"/>
```

**Option 2: NumberFormat (moderate control)**
```xml
<syncfusion:PercentTextBox>
    <syncfusion:PercentTextBox.NumberFormat>
        <numberformat:NumberFormatInfo PercentGroupSeparator=","/>
    </syncfusion:PercentTextBox.NumberFormat>
</syncfusion:PercentTextBox>
```

**Option 3: Dedicated properties (maximum control)**
```xml
<syncfusion:PercentTextBox PercentGroupSeparator=","
                           PercentDecimalSeparator="."
                           GroupSeperatorEnabled="True"/>
```

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
