# Time Selector Customization in SfTimePicker

## Table of Contents
- [Overview](#overview)
- [Cell Templates](#cell-templates)
- [Cell Sizing](#cell-sizing)
- [Item Spacing](#item-spacing)
- [Selector Styling](#selector-styling)
- [Common Patterns](#common-patterns)

---

## Overview

The **SfTimeSelector** is the dropdown control that appears when the time picker is expanded. It contains three main selectable components:

1. **Hour cells** - Display available hours (1-12 or 0-23)
2. **Minute cells** - Display available minutes (0-59)
3. **Meridiem cells** - Display AM/PM (only in 12-hour mode)

Each component has its own cell template that you can customize independently.

## Cell Templates

Customize cell appearance using templates nested in `SelectorStyle`. Available templates:

- **HourCellTemplate** - DataContext: `DateTimeWrapper.HourNumber`
- **MinuteCellTemplate** - DataContext: `DateTimeWrapper.MinuteNumber`
- **MeridiemCellTemplate** - DataContext: `DateTimeWrapper.AmPmString` (12-hour mode only)

**DataContext properties available:** `HourNumber`, `MinuteNumber`, `AmPmString`, `SecondNumber` (if seconds enabled).

Use `DataTemplate` with `Binding` to display values. Example: `<TextBlock Text="{Binding HourNumber}"/>` displays the hour. Wrap in containers (Grid, StackPanel) for layout and add icons/styling as needed.

## Cell Sizing

Control cell dimensions using `SelectorItemWidth` and `SelectorItemHeight` properties (default: 30x30 pixels). Set in XAML (`SelectorItemWidth="60"`, `SelectorItemHeight="60"`) or C# (`timePicker.SelectorItemWidth = 60;`). Larger sizes (60-80) are touch-friendly; combine with `DropDownHeight` for responsive layouts.

## Item Spacing

Control spacing between selectors using `SelectorItemSpacing` (default: 4 pixels). Compact layouts: `SelectorItemSpacing="2"` with small cells. Spacious layouts: `SelectorItemSpacing="30"` with large cells. Set in XAML or C# (`timePicker.SelectorItemSpacing = 20;`).

## Selector Styling

Apply custom styles using `SelectorStyle` property targeting `syncfusion:SfTimeSelector`. 

**Key color properties:**
- `Foreground` - Unselected items
- `SelectedForeground` - Selected/highlighted items
- `Background` - Dropdown background
- `AccentBrush` - Selected item highlight

Nest `SelectorStyle` in `SfTimePicker` with `Style` of type `syncfusion:SfTimeSelector` and use `Setter` elements for property values.

## Common Patterns

**Touch-Friendly Large Cells:** `SelectorItemWidth="70"`, `SelectorItemHeight="70"`, `SelectorItemSpacing="15"`, `DropDownHeight="400"`. Set `Foreground="DarkGray"`, `SelectedForeground="White"` in `SelectorStyle`.

**Compact Desktop View:** `SelectorItemWidth="35"`, `SelectorItemHeight="35"`, `SelectorItemSpacing="3"`, `DropDownHeight="250"`. Use `Background="WhiteSmoke"` in `SelectorStyle`.

**Dark Theme:** Set `Background="#333333"`, `Foreground="#CCCCCC"`, `SelectedForeground="#FFFF00"`, `AccentBrush="#0066FF"` in `SelectorStyle`.

**Icon-Enhanced Cells:** Use `HourCellTemplate` and `MinuteCellTemplate` with emoji (🕐, ⏱) or Unicode icons in `DataTemplate` alongside `{Binding HourNumber}` and `{Binding MinuteNumber}`.

## Troubleshooting

### Issue: Cell Templates Not Appearing

**Solution:** Ensure the SelectorStyle is properly nested within SfTimePicker and target type is `syncfusion:SfTimeSelector`

### Issue: Spacing Too Large/Small

**Solution:** Adjust SelectorItemSpacing. Default is 4 pixels; increase for more space between selectors.

### Issue: Custom Templates Break Layout

**Solution:** Ensure your DataTemplate contains appropriate sizing (VerticalAlignment/HorizontalAlignment) to fill the cell dimensions.

## Next Steps

- Learn **[Appearance & Styling](appearance-and-styling.md)** for overall control styling
- Explore **[Time Formatting](time-formatting.md)** to control selector format strings
