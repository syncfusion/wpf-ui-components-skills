---
name: syncfusion-wpf-timespan-editor
description: How to implement the Syncfusion WPF TimeSpanEdit (TimeSpan Editor) control for time duration inputs. Use this skill when creating time duration inputs, capturing Days:Hours:Minutes:Seconds values, building time interval editors, or implementing time range controls with keyboard and mouse-based time entry. Includes setup, formatting, user interactions, constraints, and styling.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF TimeSpanEdit

The [TimeSpanEdit](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Shared.TimeSpanEdit.html) control allows users to set or display time as **Days:Hours:Minutes:Seconds** (d.h:m:s) format with support for keyboard and mouse interactions, custom formatting, and theming.

## When to Use This Skill

Use the TimeSpanEdit control when you need to:
- Capture or display **time durations/intervals** in your WPF application
- Provide **Days:Hours:Minutes:Seconds** input with a user-friendly interface
- Enable users to **increment/decrement time values** using keyboard, mouse wheel, or buttons
- Display time with **custom formats** and labels (e.g., "5 days, 3 hours, 15 minutes, 30 sec")
- Apply **min/max constraints** to limit allowed time ranges
- Support **null values** with watermark text for empty states
- Style the control with **colors, themes, and RTL support**

## Component Overview

**Key Capabilities:**
- ✓ Display time as Days:Hours:Minutes:Seconds (with milliseconds support)
- ✓ Programmatic and user-based value changes
- ✓ Keyboard navigation between fields (arrow keys)
- ✓ Mouse wheel and click-drag interactions
- ✓ Custom format strings with field labels
- ✓ Min/Max value constraints
- ✓ ValueChanged event notifications
- ✓ ReadOnly mode for display-only scenarios
- ✓ Null value support with watermarks
- ✓ Theme integration (SfSkinManager, ThemeStudio)
- ✓ RTL (Right-to-Left) flow direction support

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly installation and NuGet setup
- Adding TimeSpanEdit via Designer
- Adding TimeSpanEdit via XAML
- Adding TimeSpanEdit via C# code
- Basic setup examples

### Value Setting & Custom Formats
📄 **Read:** [references/value-and-formats.md](references/value-and-formats.md)
- Setting time values programmatically
- Custom format strings (d, h, m, s, z)
- Format examples with labels ("days", "hours", etc.)
- Displaying milliseconds
- Null values and watermark text

### User Interactions
📄 **Read:** [references/user-interactions.md](references/user-interactions.md)
- Field navigation with mouse and keyboard
- Step intervals for precise increments
- UpDown button controls
- Mouse wheel interaction
- Click & drag (extended scrolling)
- Keyboard arrow key navigation
- ReadOnly mode for preventing edits

### Constraints & Events
📄 **Read:** [references/constraints-and-events.md](references/constraints-and-events.md)
- Min/Max value constraints
- ValueChanged event handling
- Event binding in XAML and C#
- Value change notifications

### Appearance & Theming
📄 **Read:** [references/appearance-and-theming.md](references/appearance-and-theming.md)
- Background and selection colors
- Foreground text color
- Flow direction (RTL support)
- Built-in theme application
- SfSkinManager integration
- ThemeStudio custom themes

---

## Quick Start Example

**XAML:**
```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <!-- Basic TimeSpanEdit -->
        <syncfusion:TimeSpanEdit 
            x:Name="timeSpanEdit"
            Value="5.12:30:45"
            Width="150"
            Height="30" />
    </Grid>
</Window>
```

**C# Code-Behind:**
```csharp
using Syncfusion.Windows.Shared;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        // Create and configure TimeSpanEdit
        TimeSpanEdit editor = new TimeSpanEdit();
        editor.Value = new TimeSpan(5, 12, 30, 45);
        editor.Width = 150;
        editor.Height = 30;
        
        this.Content = editor;
    }
}
```

**Assembly Reference:**
Add this to your WPF project:
- `Syncfusion.Shared.WPF`

---

## Common Patterns

### Pattern 1: Duration Entry with Constraints
Restrict time selection to specific range (e.g., 2-10 days):

```xml
<syncfusion:TimeSpanEdit 
    Value="5.08:30:00"
    MinValue="2.00:00:00"
    MaxValue="10.00:00:00"
    ShowArrowButtons="True" />
```

### Pattern 2: Custom Formatted Display
Show user-friendly format with labels:

```xml
<syncfusion:TimeSpanEdit 
    Value="3.14:25:30"
    Format="d 'days' h 'hours' m 'minutes' s 'seconds'" />
```

### Pattern 3: Precise Step Intervals
Control increment/decrement precision:

```xml
<syncfusion:TimeSpanEdit 
    Value="5.10:00:00"
    StepInterval="1.00:15:30"
    IncrementOnScrolling="True" />
```

### Pattern 4: Read-Only Display
Show time value without allowing user edits:

```xml
<syncfusion:TimeSpanEdit 
    Value="7.18:45:30"
    IsReadOnly="True"
    AllowNull="False" />
```

### Pattern 5: Null Value with Watermark
Allow empty state with placeholder text:

```xml
<syncfusion:TimeSpanEdit 
    AllowNull="True"
    Value="{x:Null}"
    NullString="Enter time duration..." />
```

---

## Key Props Reference

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Value` | `TimeSpan?` | `0.0:0:0` | The time duration to display/edit |
| `Format` | `string` | `d.h:m:s` | Custom format string (d=days, h=hours, m=minutes, s=seconds, z=milliseconds) |
| `MinValue` | `TimeSpan` | — | Minimum allowed time duration |
| `MaxValue` | `TimeSpan` | — | Maximum allowed time duration |
| `StepInterval` | `TimeSpan` | `1.01:01:01` | Increment/decrement interval for buttons/wheel/keyboard |
| `AllowNull` | `bool` | `false` | Allow null/empty values |
| `NullString` | `string` | `""` | Watermark text when value is null |
| `ShowArrowButtons` | `bool` | `true` | Show up/down spinner buttons |
| `IncrementOnScrolling` | `bool` | `true` | Allow mouse wheel to change values |
| `EnableExtendedScrolling` | `bool` | `false` | Allow click & drag to change values |
| `IsReadOnly` | `bool` | `false` | Prevent user edits (read-only mode) |
| `Background` | `Brush` | `White` | Control background color |
| `Foreground` | `Brush` | `Black` | Text color |
| `SelectionBrush` | `Brush` | `RoyalBlue` | Selection highlight color |
| `FlowDirection` | `FlowDirection` | `LeftToRight` | RTL support (RightToLeft) |

---

## Common Use Cases

**Use Case 1: Time Duration Picker**
Users need to input work duration, task time, or interval:
- Use custom format with labels: `"d 'days' h 'hrs' m 'min'"`
- Enable all interaction modes (buttons, wheel, keyboard)
- Set reasonable Min/Max constraints

**Use Case 2: Display-Only Duration**
Show calculated time values without editing:
- Set `IsReadOnly="True"`
- Use clear format with labels
- Hide UpDown buttons: `ShowArrowButtons="False"`

**Use Case 3: Precise Time Entry**
Users need exact control over hours/minutes:
- Set `StepInterval` for coarse control (e.g., 15-min increments)
- Enable keyboard navigation for field-by-field entry
- Consider smaller step intervals for seconds

**Use Case 4: Flexible Time Input**
Users prefer multiple input methods:
- Keep all interactions enabled (buttons, wheel, keyboard, drag)
- Use logical step intervals
- Provide visual feedback with custom colors

---

## Next Steps

1. **Start with Getting Started**: Read [getting-started.md](references/getting-started.md) to set up the control
2. **Format Your Display**: Check [value-and-formats.md](references/value-and-formats.md) for custom formats
3. **Enable Interactions**: Review [user-interactions.md](references/user-interactions.md) for optimal UX
4. **Add Logic**: Use [constraints-and-events.md](references/constraints-and-events.md) for events and limits
5. **Style It**: Apply colors and themes with [appearance-and-theming.md](references/appearance-and-theming.md)
