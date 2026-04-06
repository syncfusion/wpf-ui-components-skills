# Formatting and Culture

## Overview

IntegerTextBox supports three formatting methods: `Culture`, `NumberFormat`, and dedicated properties (`NumberGroupSeparator`, `NumberGroupSizes`).

**Priority (highest → lowest):** Dedicated Properties > `NumberFormat` > `Culture`

## Culture-Based Formatting

```xml
<syncfusion:IntegerTextBox Value="1234567" Culture="en-US" GroupSeperatorEnabled="True" Width="150" Height="25"/>
```

> **Always set `GroupSeperatorEnabled="True"`** — separators are hidden without it.

## Quick Reference: Common Cultures

| Culture | Display of 1234567890 | Separator |
|---------|-----------------------|-----------|
| `en-US` | `1,234,567,890` | Comma |
| `en-IN` | `1,23,45,67,890` | Comma (2/3 grouping) |
| `de-DE` | `1.234.567.890` | Period |
| `fr-FR` | `1 234 567 890` | Space |
| `de-CH` | `1''234''567''890` | Apostrophe |

## NumberFormat Property

```xml
<syncfusion:IntegerTextBox Value="123456789012345" GroupSeperatorEnabled="True">
    <syncfusion:IntegerTextBox.NumberFormat>
        <numberformat:NumberFormatInfo NumberGroupSeparator="/"/>
    </syncfusion:IntegerTextBox.NumberFormat>
</syncfusion:IntegerTextBox>
```

With custom group sizes in C#:

```csharp
integerTextBox.GroupSeperatorEnabled = true;
integerTextBox.NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = "/",
    NumberGroupSizes = new int[] { 2, 3, 4 }
};
```

## Dedicated Properties

```xml
<syncfusion:IntegerTextBox Value="123456789012345" NumberGroupSeparator="/" GroupSeperatorEnabled="True" Width="250" Height="30">
    <syncfusion:IntegerTextBox.NumberGroupSizes>
        <sys:Int32>2</sys:Int32>
        <sys:Int32>3</sys:Int32>
        <sys:Int32>0</sys:Int32>
    </syncfusion:IntegerTextBox.NumberGroupSizes>
</syncfusion:IntegerTextBox>
```

`NumberGroupSizes` is read **right-to-left**. A trailing `0` repeats the previous group size for all remaining digits.
