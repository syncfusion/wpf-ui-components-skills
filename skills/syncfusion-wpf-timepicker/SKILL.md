---
name: syncfusion-wpf-timepicker
description: Create and customize WPF time picker controls using Syncfusion's SfTimePicker component. Use this when implementing time selection UI, time input fields, or time value formatting. This skill covers SfTimePicker setup, time selector customization, value binding, and appearance styling in WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF TimePicker (SfTimePicker)

The [SfTimePicker](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfTimePicker.html) is a WPF control that enables touch-friendly time value selection with customizable formatting, time selector UI, and comprehensive styling options. This skill guides you through implementing, configuring, and customizing time picker controls for WPF applications.

## When to Use This Skill

- **Adding a time picker control** to a WPF application for user time input
- **Setting and managing time values** programmatically or through binding
- **Formatting time display** in different formats (12-hour, 24-hour, culture-specific)
- **Customizing the time selector** with templates, sizing, and spacing
- **Styling and theming** the time picker (colors, backgrounds, RTL support)
- **Handling time value changes** with events and callbacks
- **Controlling editing modes** (free editing vs. dropdown selection)

## Navigation Guide

Choose your task below to jump to the relevant documentation:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly references
- Adding control via designer, XAML, and C#
- Basic time picker setup
- Handling the ValueChanged event
- Applying themes

### Setting Time Values
📄 **Read:** [references/setting-time-values.md](references/setting-time-values.md)
- Setting time using the Value property
- Allowing and setting null values
- Watermark text for prompts
- UpdateValueOnLostFocus behavior
- Event-based time detection

### Time Formatting
📄 **Read:** [references/time-formatting.md](references/time-formatting.md)
- FormatString for display formatting
- SelectorFormatString for time picker format
- Common format patterns
- Culture-specific formatting

### Customizing the Time Selector
📄 **Read:** [references/time-selector-customization.md](references/time-selector-customization.md)
- Overview of SfTimeSelector control
- Hour, minute, and meridiem cell templates
- Cell sizing and spacing properties
- Advanced template customization

### Appearance & Styling
📄 **Read:** [references/appearance-and-styling.md](references/appearance-and-styling.md)
- Foreground and background colors
- Selected item styling
- Flow direction (right-to-left support)
- Editing modes (inline vs. dropdown)
- Theme integration

## Quick Start

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <!-- Basic time picker -->
        <syncfusion:SfTimePicker x:Name="timePicker" 
                                 Width="200" 
                                 Value="04:30:00"
                                 ValueChanged="TimePicker_ValueChanged"/>
    </Grid>
</Window>
```

```csharp
using Syncfusion.Windows.Controls.Input;

SfTimePicker timePicker = new SfTimePicker();
timePicker.Value = new TimeSpan(14, 30, 00); // 2:30 PM
timePicker.ValueChanged += (d, e) => 
    Console.WriteLine($"Time changed to: {e.NewValue}");
```

## Common Patterns

### Pattern 1: 24-Hour Time Display
```xaml
<syncfusion:SfTimePicker FormatString="HH:mm:ss" 
                         SelectorFormatString="HH:mm"/>
```

### Pattern 2: Allow Null Values with Watermark
```xaml
<syncfusion:SfTimePicker AllowNull="True"
                         Value="{x:Null}"
                         Watermark="Select a time"/>
```

### Pattern 3: Read-Only Time Display
```xaml
<syncfusion:SfTimePicker AllowInlineEditing="False"
                         ShowDropDownButton="False"
                         Value="09:00:00"/>
```

### Pattern 4: Custom Cell Size and Spacing
```xaml
<syncfusion:SfTimePicker SelectorItemWidth="60"
                         SelectorItemHeight="60"
                         SelectorItemSpacing="20"/>
```

## Key Properties Quick Reference

| Property | Default | Purpose |
|----------|---------|---------|
| `Value` | Current system time | Get/set the selected time as TimeSpan |
| `FormatString` | "h:mm tt" | Display format (12-hr/24-hr) |
| `SelectorFormatString` | "h:mm tt" | Format for time picker cells |
| `AllowNull` | false | Enable null value support |
| `Watermark` | — | Placeholder text when Value is null |
| `AllowInlineEditing` | true | Allow direct text editing |
| `ShowDropDownButton` | true | Show dropdown button |
| `SetValueOnLostFocus` | false | Update value when focus lost |
| `SelectorItemWidth` | 30 | Width of selector cells (pixels) |
| `SelectorItemHeight` | 30 | Height of selector cells (pixels) |
| `SelectorItemSpacing` | 4 | Space between selector items (pixels) |
| `Foreground` | — | Text color |
| `Background` | — | Control background color |
| `AccentBrush` | — | Selected item highlight color |
| `FlowDirection` | LeftToRight | Support RTL layouts |
