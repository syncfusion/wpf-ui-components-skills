---
name: syncfusion-wpf-integer-textbox
description: Comprehensive guide for implementing Syncfusion WPF IntegerTextBox control for integer value input with validation, formatting, and customization. Use this skill when implementing integer input controls, numeric validation for integers, or integer value entry in WPF. Covers integer data entry forms, min/max value restrictions, number formatting with culture support, integer value scrolling, range adorner progress visualization, and integer-specific input control implementation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing IntegerTextBox

Syncfusion WPF IntegerTextBox restricts input to integer (Int64) values with validation, formatting, and customization.

**Assembly:** `Syncfusion.Shared.WPF`  **Namespace:** `Syncfusion.Windows.Shared`

## Reference Files

| File | Topics |
|------|--------|
| [getting-started.md](references/getting-started.md) | Installation, XAML/C# setup, MVVM binding |
| [value-management.md](references/value-management.md) | Value property, paste modes, spin buttons, null/watermark |
| [validation-restrictions.md](references/validation-restrictions.md) | Min/Max, validation modes, exceed behavior, read-only |
| [formatting-culture.md](references/formatting-culture.md) | Culture, NumberFormat, group separator/sizes |
| [appearance-customization.md](references/appearance-customization.md) | Colors, alignment, selection, corner radius |
| [step-interval-scrolling.md](references/step-interval-scrolling.md) | ScrollInterval, arrow keys, mouse wheel, drag, focus selection |
| [range-adorner.md](references/range-adorner.md) | EnableRangeAdorner, RangeAdornerBackground |

## Key Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| **Value** | long? | 0 | Current integer value of the control |
| **MinValue** | int | int.MinValue | Minimum allowed value |
| **MaxValue** | int | int.MaxValue | Maximum allowed value |
| **ScrollInterval** | int | 1 | Increment/decrement step value |
| **ShowSpinButton** | bool | false | Show up/down spinner buttons |
| **EnableRangeAdorner** | bool | false | Show progress bar-like visual |
| **GroupSeperatorEnabled** | bool | false | Enable number group separators |
| **NumberGroupSeparator** | string | Culture-based | Custom group separator character |
| **Culture** | CultureInfo | CurrentCulture | Culture for number formatting |
| **UseNullOption** | bool | false | Allow null values |
| **NullValue** | int? | null | Value to display when null |
| **WatermarkText** | string | string.Empty | Placeholder text when empty |
| **PositiveForeground** | Brush | Black | Foreground for positive values |
| **NegativeForeground** | Brush | Red | Foreground for negative values |
| **ZeroColor** | Brush | Green | Foreground for zero value |
| **IsReadOnly** | bool | false | Prevent user input |
| **TextAlignment** | TextAlignment | Left | Horizontal text alignment |
| **CornerRadius** | CornerRadius | 1 | Border corner rounding |


