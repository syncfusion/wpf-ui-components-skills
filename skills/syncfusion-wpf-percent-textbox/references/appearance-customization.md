# Appearance Customization

## Table of Contents
- [Foreground Colors](#foreground-colors)
- [Background Customization](#background-customization)
- [Corner Radius](#corner-radius)
- [Selection Appearance](#selection-appearance)
- [Text Alignment](#text-alignment)
- [ToolTip Support](#tooltip-support)

## Foreground Colors

PercentTextBox allows different foreground colors based on the value's sign (positive, negative, or zero).

### Foreground for Positive Values

Set the foreground color when PercentValue is positive:

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           PositiveForeground="Blue"/>
```

**Result:** "50 %" displays in blue color

# Appearance Customization (summary)

This page summarizes appearance options for `PercentTextBox`.

- Foreground: use `PositiveForeground` / `NegativeForeground` and enable `ApplyNegativeForeground` to color values by sign. Use `ZeroColor` with `ApplyZeroColor` for zero state.
- Background and border: set `Background`, `BorderBrush`, `BorderThickness`, and `CornerRadius` to achieve desired shape and contrast.
- Selection: customize `SelectionBrush` and `SelectionOpacity` for selected text visibility.
- Text alignment: `TextAlignment` supports `Left`, `Center`, and `Right` to fit layout contexts (tables typically use right alignment).
- Tooltips: provide `ToolTip` or `ToolTip` templates for validation hints and range descriptions.

Design tips:
- Ensure sufficient contrast between foreground and background for accessibility.
- Keep corner radius proportional to control height for balanced visuals.
- Use subtle selection styling for numeric inputs to avoid distracting the user.

For reusable recommendations see `references/best-practices.md`.
Set the foreground color when PercentValue is negative:
