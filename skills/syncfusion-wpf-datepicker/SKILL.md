---
name: syncfusion-wpf-datepicker
description: Implements the Syncfusion WPF DatePicker (SfDatePicker) control for touch-friendly date selection. Use this when adding date input controls, customizing date picker appearance, formatting date displays, or restricting date ranges in WPF applications. Covers installation, date formatting, styling, customization, value management, and event handling.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF DatePicker (SfDatePicker)

The **SfDatePicker** control allows users to select date values in a touch-friendly manner with a customizable dropdown date selector. This skill covers everything from basic setup to advanced customization and styling.

## When to Use This Skill

- **Basic date selection** – Add a DatePicker control to your WPF application
- **Date formatting** – Display dates in various formats (short date, long date, month-only, etc.)
- **Customizing the date selector** – Modify cell templates, sizes, and spacing of the dropdown selector
- **Styling and appearance** – Change colors, backgrounds, themes, and flow direction (RTL support)
- **Managing date values** – Set, validate, and restrict date ranges; handle date change events
- **Advanced interactions** – Inline editing, watermarks, focus-based updates, keyboard input
- **User-friendly UX** – Implement null values, watermark text, and on-screen keyboard support

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet installation and assembly references
- Adding the control via Designer, XAML, or C# code
- Setting initial date values
- Handling date change events (ValueChanged)
- Basic setup and initialization

### Date Formatting
📄 **Read:** [references/date-formatting.md](references/date-formatting.md)
- Display formatting using FormatString property
- Selector formatting using SelectorFormatString
- Common format specifiers and examples
- Date display in various formats

### Date Selector Customization
📄 **Read:** [references/date-selector-customization.md](references/date-selector-customization.md)
- Understanding SfDateSelector structure
- Customizing cell templates (day, month, year)
- Adjusting cell sizes and spacing
- Style-based customization
- Visual template examples

### Appearance and Styling
📄 **Read:** [references/appearance-and-styling.md](references/appearance-and-styling.md)
- Changing foreground and background colors
- Accent colors and selected item styling
- Right-to-left (RTL) flow direction
- Built-in themes (SfSkinManager, ThemeStudio)
- Watermark text and templates

### Value and Date Management
📄 **Read:** [references/value-and-date-management.md](references/value-and-date-management.md)
- Setting date values programmatically
- Handling null values
- Watermark and placeholder text
- Inline editing and keyboard input
- Min/Max date range restrictions
- Focus-based value updates

### Features and Interaction
📄 **Read:** [references/features-and-interaction.md](references/features-and-interaction.md)
- Inline editing mode
- On-screen keyboard input scope
- Date range validation
- Localization support
- Common workflows and best practices

## Quick Start

### Basic DatePicker in XAML

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="DatePicker Sample">
    <Grid>
        <syncfusion:SfDatePicker x:Name="sfDatePicker" 
                                 Width="200"
                                 Value="3/19/2026"/>
    </Grid>
</Window>
```

### Basic DatePicker in C#

```csharp
using Syncfusion.Windows.Controls.Input;

SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.Value = new DateTime(2026, 3, 19);
sfDatePicker.Width = 200;
this.Content = sfDatePicker;
```

### Handle Date Selection

```csharp
sfDatePicker.ValueChanged += (d, e) => 
{
    DateTime oldDate = (DateTime)e.OldValue;
    DateTime newDate = (DateTime)e.NewValue;
    Console.WriteLine($"Date changed from {oldDate:d} to {newDate:d}");
};
```

## Common Patterns

### Pattern 1: Set Date Format
```xaml
<!-- Display as month only -->
<syncfusion:SfDatePicker FormatString="M" x:Name="sfDatePicker"/>
```

### Pattern 2: Restrict Date Range
```csharp
sfDatePicker.MinDate = new DateTime(2026, 1, 1);
sfDatePicker.MaxDate = new DateTime(2026, 12, 31);
```

### Pattern 3: Allow Null with Watermark
```xaml
<syncfusion:SfDatePicker AllowNull="True" 
                         Value="{x:Null}"
                         Watermark="Select a date"
                         x:Name="sfDatePicker"/>
```

### Pattern 4: Inline Editing with Validation
```xaml
<syncfusion:SfDatePicker AllowInlineEditing="True" 
                         InputScope="Number"
                         x:Name="sfDatePicker"/>
```

### Pattern 5: Custom Styling with Colors
```xaml
<syncfusion:SfDatePicker Foreground="Blue"
                         Background="LightGray"
                         AccentBrush="Green"
                         x:Name="sfDatePicker"/>
```

## Key Properties Reference

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| **Value** | DateTime? | Current date | Selected date |
| **FormatString** | string | "d" | Display date format |
| **SelectorFormatString** | string | "M/d/yyyy" | Dropdown selector format |
| **AllowNull** | bool | false | Allow null values |
| **Watermark** | string | null | Placeholder text |
| **MinDate** | DateTime | Min value | Earliest selectable date |
| **MaxDate** | DateTime | Max value | Latest selectable date |
| **AllowInlineEditing** | bool | false | Enable inline date editing |
| **ShowDropDownButton** | bool | true | Show/hide dropdown button |
| **DropDownHeight** | double | Auto | Height of dropdown popup |
| **Foreground** | Brush | Black | Text color |
| **Background** | Brush | White | Control background |
| **AccentBrush** | Brush | Blue | Selected item highlight |
