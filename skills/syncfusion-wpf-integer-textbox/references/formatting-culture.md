# Formatting and Culture

## Table of Contents
- [Overview](#overview)
- [Culture-Based Formatting](#culture-based-formatting)
- [NumberFormatInfo Formatting](#numberformatinfo-formatting)
- [Dedicated Properties Formatting](#dedicated-properties-formatting)
- [GroupSeparator Configuration](#groupseparator-configuration)
- [NumberGroupSizes](#numbergroupsizes)
- [Formatting Priority Rules](#formatting-priority-rules)
- [Common Culture Examples](#common-culture-examples)
- [Best Practices](#best-practices)

## Overview

IntegerTextBox provides three methods for formatting number display: culture-based formatting, NumberFormatInfo customization, and dedicated formatting properties. This allows you to display integers according to regional conventions or custom requirements with group separators and sizing.

## Culture-Based Formatting

Use the `Culture` property to format numbers according to specific regional conventions.

**Property:** `Culture`  
**Type:** `CultureInfo`  
**Default:** Current system culture

### Basic Culture Configuration

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Height="25" 
                           Width="150" 
                           Culture="en-US" 
                           Value="1234567"
                           GroupSeperatorEnabled="True"/>
```

```csharp
using System.Globalization;

IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Height = 25;
integerTextBox.Width = 150;
integerTextBox.Value = 1234567;
integerTextBox.Culture = new CultureInfo("en-US");
integerTextBox.GroupSeperatorEnabled = true;
```

**Display:** `1,234,567` (US format with comma separator)

### Culture Property Behavior

The `Culture` property automatically configures:
- **NumberGroupSeparator** - Character used to separate number groups
- **NumberGroupSizes** - How many digits in each group
- **Number formatting conventions** - Based on regional standards

### Common Culture Examples

**US English (en-US):**
```xml
<syncfusion:IntegerTextBox Culture="en-US" 
                           Value="1234567"
                           GroupSeperatorEnabled="True"
                           Width="150" 
                           Height="30"/>
```
**Display:** `1,234,567`

**German (de-DE):**
```xml
<syncfusion:IntegerTextBox Culture="de-DE" 
                           Value="1234567"
                           GroupSeperatorEnabled="True"
                           Width="150" 
                           Height="30"/>
```
**Display:** `1.234.567` (period as separator)

**French (fr-FR):**
```xml
<syncfusion:IntegerTextBox Culture="fr-FR" 
                           Value="1234567"
                           GroupSeperatorEnabled="True"
                           Width="150" 
                           Height="30"/>
```
**Display:** `1 234 567` (space as separator)

**Latin (bs-Latn):**
```xml
<syncfusion:IntegerTextBox Culture="bs-Latn" 
                           Value="1234567"
                           GroupSeperatorEnabled="True"
                           Width="150" 
                           Height="30"/>
```
**Display:** `1.234.567`

```csharp
// Setting Latin culture
integerTextBox.Culture = new CultureInfo("bs-Latn");
integerTextBox.GroupSeperatorEnabled = true;
integerTextBox.Value = 1234567;
```

### Enabling Group Separators

**Important:** Set `GroupSeperatorEnabled` to `true` to display group separators.

```xml
<syncfusion:IntegerTextBox Value="123456789"
                           Culture="en-US"
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```

Without `GroupSeperatorEnabled="True"`, the value displays as `123456789` regardless of culture.

## NumberFormatInfo Formatting

Customize number formatting using the `NumberFormat` property with `NumberFormatInfo` class.

**Property:** `NumberFormat`  
**Type:** `NumberFormatInfo`

### Basic NumberFormat Configuration

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Height="25" 
                           Width="150" 
                           Culture="en-US"
                           Value="123456789012345" 
                           GroupSeperatorEnabled="True">
    <syncfusion:IntegerTextBox.NumberFormat>
        <numberformat:NumberFormatInfo NumberGroupSeparator="/"/>
    </syncfusion:IntegerTextBox.NumberFormat>
</syncfusion:IntegerTextBox>
```

**Display:** `123/456/789/012/345` (forward slash as separator)

### NumberFormat in C#

```csharp
using System.Globalization;

integerTextBox.GroupSeperatorEnabled = true;
integerTextBox.Value = 123456789012345;
integerTextBox.Culture = new CultureInfo("en-US");
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "/"
};
```

### Custom Separator Examples

**Hyphen separator:**
```csharp
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "-"
};
// Display: 123-456-789
```

**Underscore separator:**
```csharp
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "_"
};
// Display: 123_456_789
```

**Pipe separator:**
```csharp
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "|"
};
// Display: 123|456|789
```

### NumberFormat with NumberGroupSizes

Configure both separator and group sizes:

```csharp
integerTextBox.Value = 123456789012345;
integerTextBox.GroupSeperatorEnabled = true;
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "/",
    NumberGroupSizes = new int[] { 2, 3, 4 }
};
```

**Display:** `1234/5678/901/23/45`

**Grouping explanation:**
- Last (rightmost) group: 2 digits (`45`)
- Next group: 3 digits (`23`)
- Next group: 4 digits (`901`)
- Remaining groups: repeats last pattern (4 digits each)

## Dedicated Properties Formatting

Use specific properties for more direct control over formatting.

**Properties:**
- `NumberGroupSeparator` - Custom separator character
- `NumberGroupSizes` - Custom group size pattern
- `GroupSeperatorEnabled` - Enable/disable separators

### Basic Dedicated Properties

```xml
<syncfusion:IntegerTextBox Value="123456789"
                           NumberGroupSeparator=","
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```

```csharp
integerTextBox.Value = 123456789;
integerTextBox.NumberGroupSeparator = ",";
integerTextBox.GroupSeperatorEnabled = true;
```

**Display:** `123,456,789`

### Custom Separator with Dedicated Property

```xml
<syncfusion:IntegerTextBox Value="123456789012345"
                           NumberGroupSeparator="/"
                           GroupSeperatorEnabled="True"
                           Width="250"
                           Height="30"/>
```

```csharp
integerTextBox.Value = 123456789012345;
integerTextBox.NumberGroupSeparator = "/";
integerTextBox.GroupSeperatorEnabled = true;
```

**Display:** `123/456/789/012/345`

## GroupSeparator Configuration

The `NumberGroupSeparator` property defines the character used to separate digit groups.

**Property:** `NumberGroupSeparator`  
**Type:** `string`  
**Default:** Culture-dependent (e.g., `,` for en-US)

### Separator Examples

```xml
<!-- Comma separator -->
<syncfusion:IntegerTextBox Value="1234567"
                           NumberGroupSeparator=","
                           GroupSeperatorEnabled="True"/>
<!-- Display: 1,234,567 -->

<!-- Period separator -->
<syncfusion:IntegerTextBox Value="1234567"
                           NumberGroupSeparator="."
                           GroupSeperatorEnabled="True"/>
<!-- Display: 1.234.567 -->

<!-- Space separator -->
<syncfusion:IntegerTextBox Value="1234567"
                           NumberGroupSeparator=" "
                           GroupSeperatorEnabled="True"/>
<!-- Display: 1 234 567 -->

<!-- Custom separator -->
<syncfusion:IntegerTextBox Value="1234567"
                           NumberGroupSeparator=" | "
                           GroupSeperatorEnabled="True"/>
<!-- Display: 1 | 234 | 567 -->
```

## NumberGroupSizes

Configure custom grouping patterns using `NumberGroupSizes`.

**Property:** `NumberGroupSizes`  
**Type:** `Int32Collection`  
**Default:** Culture-dependent (typically `{3}`)

### Basic NumberGroupSizes

```xml
<syncfusion:IntegerTextBox Value="123456789012345"
                           NumberGroupSeparator="/"
                           GroupSeperatorEnabled="True">
    <syncfusion:IntegerTextBox.NumberGroupSizes>
        <sys:Int32>2</sys:Int32>
        <sys:Int32>3</sys:Int32>
        <sys:Int32>0</sys:Int32>
    </syncfusion:IntegerTextBox.NumberGroupSizes>
</syncfusion:IntegerTextBox>
```

**Display:** `12345678/901/23/45`

### NumberGroupSizes in C#

```csharp
using Syncfusion.Windows.Shared;

integerTextBox.Value = 123456789012345;
integerTextBox.NumberGroupSeparator = "/";
integerTextBox.GroupSeperatorEnabled = true;
integerTextBox.NumberGroupSizes = new Int32Collection { 2, 3, 0 };
```

### NumberGroupSizes Patterns

**Pattern: {3}** (standard grouping)
```csharp
integerTextBox.NumberGroupSizes = new Int32Collection { 3 };
// Value: 123456789
// Display: 123,456,789
```

**Pattern: {2, 3, 4}** (mixed grouping)
```csharp
integerTextBox.NumberGroupSizes = new Int32Collection { 2, 3, 4 };
// Value: 123456789012345
// Display: 1234/5678/901/23/45
// Groups from right: 45 (2), 23 (3), 901 (4), 5678 (4), 1234 (4)
```

**Pattern: {2, 2, 0}** (two groups of 2, rest ungrouped)
```csharp
integerTextBox.NumberGroupSizes = new Int32Collection { 2, 2, 0 };
// Value: 123456789
// Display: 12345/67/89
```

**Pattern: {4}** (groups of 4)
```csharp
integerTextBox.NumberGroupSizes = new Int32Collection { 4 };
// Value: 123456789012
// Display: 1234/5678/9012
```

### Understanding Group Size Patterns

The array is read **right to left** (from least significant digits):
1. First element: rightmost group size
2. Second element: next group size
3. Last element (if 0): repeat previous size for remaining digits
4. Last element (if non-zero): repeat that size for remaining digits

**Example breakdown for {2, 3, 4}:**
```
Value: 123456789012345
       1234|5678|901|23|45
             ↑    ↑   ↑  ↑
             4    4   3  2 (group sizes)
```

## Formatting Priority Rules

When multiple formatting methods are configured, priority determines which takes effect.

### Priority Order (Highest to Lowest)

1. **Dedicated Properties** (`NumberGroupSeparator`, `NumberGroupSizes`)
2. **NumberFormat** property
3. **Culture** property

### Priority Example 1: Dedicated Properties Win

```csharp
integerTextBox.Value = 123456789;
integerTextBox.GroupSeperatorEnabled = true;

// Set Culture (lowest priority)
integerTextBox.Culture = new CultureInfo("en-US"); // Would give: 123,456,789

// Set NumberFormat (medium priority)
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "-" // Would give: 123-456-789
};

// Set Dedicated Property (highest priority)
integerTextBox.NumberGroupSeparator = "/"; // WINS: 123/456/789
```

**Result:** `123/456/789` (dedicated property takes precedence)

### Priority Example 2: NumberFormat Over Culture

```csharp
integerTextBox.Value = 123456789;
integerTextBox.GroupSeperatorEnabled = true;

// Set Culture
integerTextBox.Culture = new CultureInfo("en-US"); // Would give: 123,456,789

// Set NumberFormat (overrides Culture)
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "|" // WINS: 123|456|789
};
```

**Result:** `123|456|789` (NumberFormat overrides Culture)

### Best Practice: Choose One Method

To avoid confusion, use only one formatting method:

**Option 1: Culture-based (for standard regional formats)**
```xml
<syncfusion:IntegerTextBox Culture="de-DE"
                           GroupSeperatorEnabled="True"/>
```

**Option 2: NumberFormat (for custom formats)**
```xml
<syncfusion:IntegerTextBox GroupSeperatorEnabled="True">
    <syncfusion:IntegerTextBox.NumberFormat>
        <numberformat:NumberFormatInfo NumberGroupSeparator="/"/>
    </syncfusion:IntegerTextBox.NumberFormat>
</syncfusion:IntegerTextBox>
```

**Option 3: Dedicated properties (for simple customization)**
```xml
<syncfusion:IntegerTextBox NumberGroupSeparator="/"
                           GroupSeperatorEnabled="True"/>
```

## Common Culture Examples

### United States (en-US)

```xml
<syncfusion:IntegerTextBox Culture="en-US"
                           Value="1234567890"
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```
**Display:** `1,234,567,890`  
**Separator:** Comma (`,`)  
**Grouping:** Groups of 3

### India (en-IN)

```xml
<syncfusion:IntegerTextBox Culture="en-IN"
                           Value="1234567890"
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```
**Display:** `1,23,45,67,890`  
**Separator:** Comma (`,`)  
**Grouping:** First group of 3, then groups of 2

### Germany (de-DE)

```xml
<syncfusion:IntegerTextBox Culture="de-DE"
                           Value="1234567890"
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```
**Display:** `1.234.567.890`  
**Separator:** Period (`.`)  
**Grouping:** Groups of 3

### France (fr-FR)

```xml
<syncfusion:IntegerTextBox Culture="fr-FR"
                           Value="1234567890"
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```
**Display:** `1 234 567 890`  
**Separator:** Space (` `)  
**Grouping:** Groups of 3

### Switzerland (de-CH)

```xml
<syncfusion:IntegerTextBox Culture="de-CH"
                           Value="1234567890"
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```
**Display:** `1'234'567'890`  
**Separator:** Apostrophe (`'`)  
**Grouping:** Groups of 3

## Best Practices

### Use Culture for Standard Formats

When targeting specific regions, use culture-based formatting:

```xml
<syncfusion:IntegerTextBox Culture="{Binding UserCulture}"
                           Value="{Binding Amount}"
                           GroupSeperatorEnabled="True"/>
```

### Use Dedicated Properties for Simple Customization

For straightforward customization, dedicated properties are clearest:

```xml
<syncfusion:IntegerTextBox NumberGroupSeparator=","
                           GroupSeperatorEnabled="True"
                           Value="1234567"/>
```

### Use NumberFormat for Complex Requirements

For advanced scenarios with multiple format specifications:

```csharp
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "/",
    NumberGroupSizes = new int[] { 2, 3, 4 },
    NegativeSign = "−"
};
```

### Always Enable GroupSeperatorEnabled

Remember to set `GroupSeperatorEnabled="True"` to see separators:

```xml
<!-- ❌ Separators won't display -->
<syncfusion:IntegerTextBox Culture="en-US" Value="1234567"/>

<!-- ✅ Separators will display -->
<syncfusion:IntegerTextBox Culture="en-US" 
                           Value="1234567"
                           GroupSeperatorEnabled="True"/>
```

### Consider User Locale

Respect user's regional settings when possible:

```csharp
// Use system culture
integerTextBox.Culture = CultureInfo.CurrentCulture;

// Or user's UI culture
integerTextBox.Culture = CultureInfo.CurrentUICulture;
```

### Test with Large Numbers

Always test formatting with large values to ensure grouping works correctly:

```xml
<syncfusion:IntegerTextBox Value="999999999999999"
                           Culture="en-US"
                           GroupSeperatorEnabled="True"
                           Width="250"
                           Height="30"/>
```
**Display:** `999,999,999,999,999`

## Complete Example

```xml
<StackPanel Margin="20">
    <TextBlock Text="Number Formatting Examples" 
               FontSize="16" 
               FontWeight="Bold"
               Margin="0,0,0,15"/>
    
    <!-- US Format -->
    <TextBlock Text="US Format:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Culture="en-US"
                               Value="1234567890"
                               GroupSeperatorEnabled="True"
                               Width="200"
                               Height="30"
                               Margin="0,0,0,10"/>
    
    <!-- German Format -->
    <TextBlock Text="German Format:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Culture="de-DE"
                               Value="1234567890"
                               GroupSeperatorEnabled="True"
                               Width="200"
                               Height="30"
                               Margin="0,0,0,10"/>
    
    <!-- Custom Format -->
    <TextBlock Text="Custom Format (/):" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="1234567890"
                               NumberGroupSeparator="/"
                               GroupSeperatorEnabled="True"
                               Width="200"
                               Height="30"
                               Margin="0,0,0,10"/>
    
    <!-- Indian Format -->
    <TextBlock Text="Indian Format:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Culture="en-IN"
                               Value="1234567890"
                               GroupSeperatorEnabled="True"
                               Width="200"
                               Height="30"/>
</StackPanel>
```

This comprehensive formatting system allows IntegerTextBox to display integers according to any regional convention or custom requirement.
