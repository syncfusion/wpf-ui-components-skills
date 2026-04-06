# User Interactions

## Table of Contents
- [Field Navigation](#field-navigation)
- [Step Intervals for Increment/Decrement](#step-intervals-for-incrementdecrement)
- [UpDown Button Controls](#updown-button-controls)
- [Mouse Wheel Interaction](#mouse-wheel-interaction)
- [Click & Drag (Extended Scrolling)](#click--drag-extended-scrolling)
- [Keyboard Arrow Navigation](#keyboard-arrow-navigation)
- [ReadOnly Mode](#readonly-mode)

---

## Field Navigation

TimeSpanEdit displays four fields: **Days**, **Hours**, **Minutes**, **Seconds**.

**Mouse:** Click any field to select and edit it.

**Keyboard:** 
- **Left/Right arrows** move between fields (Days → Hours → Minutes → Seconds)
- **Tab/Shift+Tab** also navigate fields
- **Up/Down arrows** increment/decrement the current field value

Navigation enabled by default; no configuration needed.

---

## Step Intervals for Increment/Decrement

The `StepInterval` property (default: 1 day, 1 hour, 1 minute, 1 second) controls how much each field increments/decrements via keyboard, mouse wheel, or buttons.

**Common StepInterval patterns:**

| Scenario | StepInterval | Use Case |
|----------|---|---|
| Default | 1.01:01:01 | General purpose |
| Coarse | 2.06:15:30 | Large time blocks |
| Precise | 0.00:05:01 | Fine-grained control |
| Task tracking | 0.00:15:00 | 15-minute increments |
| Work hours | 0.01:30:00 | 1h 30min increments |
| Timer | 0.00:00:01 | Second-level precision |

**XAML example:**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45" StepInterval="0.01:15:00" />
```

**C# code:**
```csharp
editor.StepInterval = new TimeSpan(1, 2, 5, 10);  // Days, Hours, Minutes, Seconds
```

---

## UpDown Buttons & Mouse Wheel

| Control | Property | Default | Behavior |
|---------|----------|---------|----------|
| **Buttons** | `ShowArrowButtons` | `true` | Click Up/Down to increment/decrement by `StepInterval` |
| **Wheel** | `IncrementOnScrolling` | `true` | Scroll up/down to change selected field by `StepInterval` |
| **Drag** | `EnableExtendedScrolling` | `false` | Click & drag to adjust value (requires unfocused state) |

**Quick Setup:**
```xml
<!-- Full interactions -->
<syncfusion:TimeSpanEdit Value="0.08:30:00"
                         Format="d 'days' h 'hrs' m 'mins' s 'secs'"
                         StepInterval="0.00:15:00"
                         ShowArrowButtons="True"
                         IncrementOnScrolling="True"
                         EnableExtendedScrolling="False" />
```

**C# Control:**
```csharp
editor.ShowArrowButtons = true;          // Show up/down buttons
editor.IncrementOnScrolling = true;      // Enable mouse wheel
editor.EnableExtendedScrolling = false;  // Disable drag mode (optional)
```

## Click & Drag (Extended Scrolling)

Click-and-drag adjustment (disabled by default). Drag mouse up to increase, down to decrease the selected field (requires unfocused state).

**XAML:**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45" EnableExtendedScrolling="True"/>
```

**C# code:**
```csharp
editor.EnableExtendedScrolling = true;   // Enable
editor.EnableExtendedScrolling = false;  // Disable (default)
```

## Keyboard Navigation & ReadOnly Mode

**Keyboard Navigation (enabled by default):**
- **Left/Right arrows:** Navigate between Days, Hours, Minutes, Seconds fields
- **Up/Down arrows:** Adjust value by `StepInterval` in current field
- **Tab/Shift+Tab:** Move to next/previous field

**ReadOnly Mode (display only):**
```xml
<!-- Display-only, no editing -->
<syncfusion:TimeSpanEdit Value="5.12:30:45" IsReadOnly="True" ShowArrowButtons="False"/>
```

```csharp
editor.IsReadOnly = true;  // Disable all user input
editor.Value = new TimeSpan(6, 10, 15, 20);  // Programmatic updates still work
```

