# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Foreground Colors](#foreground-colors)
- [Background](#background)
- [Corner Radius](#corner-radius)
- [Selection Styling](#selection-styling)
- [Text Alignment](#text-alignment)
- [Tooltip](#tooltip)
- [Complete Styling Example](#complete-styling-example)
```markdown
# Appearance and Styling (concise)

Short reference for common styling needs. Keep one clear example per concept.

## Foreground: Positive / Negative / Zero

- `PositiveForeground` — brush for >0 values.
- `NegativeForeground` + `ApplyNegativeForeground` — enabled when you want color for negative values.
- `ZeroColor` + `ApplyZeroColor` — optional zero-state color.

Example (XAML):

```xml
<syncfusion:CurrencyTextBox Width="200" Value="-50"
  ApplyNegativeForeground="True" NegativeForeground="Red"/>
```

## Background & CornerRadius

- Use `Background`, `BorderBrush`, `BorderThickness` for surfaces.
- `CornerRadius` controls rounding (e.g., `CornerRadius="6"`).

Example (XAML):

```xml
<syncfusion:CurrencyTextBox Width="200" Value="123.45"
  Background="#F8F9FA" CornerRadius="8"/>
```

## Selection & Alignment

- `SelectionBrush` / `SelectionOpacity` for selected text visuals.
- `TextAlignment` set to `Left`, `Center`, or `Right` for numeric alignment.

Example (XAML):

```xml
<syncfusion:CurrencyTextBox Width="150" TextAlignment="Right"
  SelectionBrush="LightBlue" SelectionOpacity="0.6"/>
```

## Tooltip

- Use `ToolTip` for non-essential guidance (avoid putting required info only in tooltips).

Example (XAML):

```xml
<syncfusion:CurrencyTextBox Width="150" ToolTip="Enter amount between $0 and $10,000"/>
```

## Guidance

- Prefer minimal, focused examples in docs.
- Keep accessibility notes short; avoid repeating long best-practices blocks.
- If you need extended patterns, move them to a separate `styling-examples.md` file.

```
**XAML:**
