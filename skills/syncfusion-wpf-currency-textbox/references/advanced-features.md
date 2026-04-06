# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Null Values](#null-values)
- [Watermark Text](#watermark-text)
- [Watermark Template](#watermark-template)
- [Paste Mode](#paste-mode)
- [Spin Buttons](#spin-buttons)
- [Range Adorner](#range-adorner)
- [Complete Advanced Example](#complete-advanced-example)

## Overview

CurrencyTextBox provides advanced features for enhanced user experience:

- **Null values** - Display specific values or text when null
- **Watermark text** - Placeholder text for empty fields
- **Custom watermark templates** - Rich visual watermarks
- **Advanced paste mode** - Intelligent clipboard insertion
- **Spin buttons** - UpDown buttons for value changes
- **Range adorner** - Visual progress indicator

## Null Values

Control how the control displays when the value is null or not set.

### NullValue Property

**Property:** `NullValue`  
**Type:** `double?`  
**Default:** `null`

**Property:** `UseNullOption`  
**Type:** `bool`  
**Default:** `false`

**⚠️ Critical:** You must set `UseNullOption = true` to enable null value support.

### Displaying Null

By default, CurrencyTextBox displays zero when value is null. With `UseNullOption = true` and `NullValue = null`, it displays nothing.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Height="25"
    Width="100" 
    UseNullOption="True"  
    NullValue="{x:Null}"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.NullValue = null;
currencyTextBox.UseNullOption = true;
```

**Result:** Control appears empty instead of showing "$0.00"

### Custom Null Value

Display a specific value when the control value is null.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Height="25"
    Width="100" 
    UseNullOption="True" 
    NullValue="10"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.NullValue = 10;
currencyTextBox.UseNullOption = true;
```

**Result:** When value is null, displays "$10.00"

```markdown
# Advanced Features (concise)

This file describes advanced but optional `CurrencyTextBox` behaviors. Keep examples minimal — prefer a single XAML and a single C# example per feature.

## Overview

- Null values (`UseNullOption`, `NullValue`) — opt-in display when value is null.
- Watermark (`WatermarkText`, `WatermarkTextIsVisible`, `WatermarkTemplate`) — placeholder guidance.
- Paste behavior (`PasteMode`) — `Default` (replace) or `Advanced` (cursor-aware insertion).
- Spin buttons (`ShowSpinButton`, `ScrollInterval`) — Up/Down increment controls.
- Range adorner (`EnableRangeAdorner`, `RangeAdornerBackground`) — visual progress between `MinValue` and `MaxValue`.

## Key Examples

### Null & Watermark (XAML)

```xml
<syncfusion:CurrencyTextBox
  Width="220"
  UseNullOption="True"
  NullValue="{x:Null}"
  WatermarkText="Enter amount"
  WatermarkTextIsVisible="True"/>
```

### Paste Mode & Spin Buttons (XAML)

```xml
<syncfusion:CurrencyTextBox
  Width="220"
  PasteMode="Advanced"
  ShowSpinButton="True"
  ScrollInterval="10"/>
```

### Range Adorner (XAML)

```xml
<syncfusion:CurrencyTextBox
  Width="220"
  MinValue="0" MaxValue="1000" Value="250"
  EnableRangeAdorner="True"
  RangeAdornerBackground="LightGreen"/>
```

### Programmatic Snippet (C#)

```csharp
var tb = new CurrencyTextBox { Width = 220, UseNullOption = true, PasteMode = PasteMode.Advanced };
tb.MinValue = 0; tb.MaxValue = 1000; tb.Value = 250;
tb.ShowSpinButton = true; tb.ScrollInterval = 10;
```

## Guidance

- Prefer concise examples: one XAML and one C# snippet per feature.
- Document expected behavior and minimal requirements (e.g., Range Adorner needs Min/Max).
- For longer walkthroughs, split into separate topic pages (e.g., `null-watermark.md`).

## Troubleshooting (short)

- Watermark not visible: ensure `UseNullOption = true`, `WatermarkTextIsVisible = true`, and `NullValue` is null.
- Range adorner missing: both `MinValue` and `MaxValue` must be set.

```
### WatermarkTextForeground
