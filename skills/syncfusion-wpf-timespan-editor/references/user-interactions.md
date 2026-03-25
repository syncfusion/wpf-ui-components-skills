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

TimeSpanEdit displays four separate fields: **Days**, **Hours**, **Minutes**, and **Seconds**.

Users can navigate between fields and edit individual values.

**Mouse Navigation:**
- Click on any field to select it
- Selected field is highlighted and ready for editing

**Keyboard Navigation:**
- Use **Left Arrow** (`←`) and **Right Arrow** (`→`) to move between fields
- Fields navigate in order: Days → Hours → Minutes → Seconds
- **Tab** key also advances to the next field

**Example Workflow:**
```
Initial state:  5.12:30:45 (Days field selected by default)
Press Right →   5.12:30:45 (Hours field selected)
Press Right →   5.12:30:45 (Minutes field selected)
Press Right →   5.12:30:45 (Seconds field selected)
Press Left  ←   5.12:30:45 (Minutes field selected again)
```

**XAML Configuration (default navigation enabled):**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45" />
<!-- Navigation enabled by default -->
```

---

## Step Intervals for Increment/Decrement

The `StepInterval` property controls how much each field increases or decreases when using keyboard arrows, mouse wheel, or UpDown buttons.

**Default StepInterval:**
```csharp
StepInterval = new TimeSpan(1, 1, 1, 1)  // 1 day, 1 hour, 1 minute, 1 second
```

This means:
- Days field increases/decreases by **1 day**
- Hours field increases/decreases by **1 hour**
- Minutes field increases/decreases by **1 minute**
- Seconds field increases/decreases by **1 second**

**Custom StepInterval Examples:**

*Example 1: Large intervals (coarse control)*
```xml
<syncfusion:TimeSpanEdit Value="10.08:00:00"
                         StepInterval="2.06:15:30" />
```

This means:
- Days: +/- 2 days
- Hours: +/- 6 hours
- Minutes: +/- 15 minutes
- Seconds: +/- 30 seconds

*Example 2: Small intervals (precise control)*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         StepInterval="0.00:05:01" />
```

This means:
- Days: +/- 0 days (no change)
- Hours: +/- 0 hours (no change)
- Minutes: +/- 5 minutes
- Seconds: +/- 1 second

*Example 3: Hours/minutes focus*
```xml
<syncfusion:TimeSpanEdit Value="0.08:30:00"
                         StepInterval="0.01:15:00" />
```

- Days: +/- 0 days
- Hours: +/- 1 hour
- Minutes: +/- 15 minutes
- Seconds: +/- 0 seconds

**C# Code Configuration:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.Value = new TimeSpan(5, 12, 30, 45);

// Set custom step interval
// Format: new TimeSpan(days, hours, minutes, seconds)
editor.StepInterval = new TimeSpan(1, 2, 5, 10);

// Now:
// - Days field: +/- 1
// - Hours field: +/- 2
// - Minutes field: +/- 5
// - Seconds field: +/- 10
```

**Real-World Scenarios:**

*Time Duration for Task Tracking (15-minute increments):*
```xml
<syncfusion:TimeSpanEdit Value="0.08:00:00"
                         Format="h 'hours' m 'minutes'"
                         StepInterval="0.00:15:00" />
```

*Work Day Duration (30-minute & 1-hour increments):*
```xml
<syncfusion:TimeSpanEdit Value="0.08:00:00"
                         Format="h 'hrs' m 'mins'"
                         StepInterval="0.01:30:00" />
```

*Precise Timer (1-second increments):*
```xml
<syncfusion:TimeSpanEdit Value="0.00:05:00"
                         Format="m 'min' s 'sec'"
                         StepInterval="0.00:00:01" />
```

---

## UpDown Button Controls

UpDown spinner buttons allow users to increment/decrement the selected field quickly with mouse clicks.

**Default Behavior:**
- UpDown buttons are **visible** by default
- Clicking Up ↑ increases the selected field by the `StepInterval`
- Clicking Down ↓ decreases the selected field by the `StepInterval`

**Show/Hide Buttons:**

*Show buttons (default):*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         ShowArrowButtons="True" />
```

*Hide buttons:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         ShowArrowButtons="False" />
```

**Practical Example:**
```xml
<syncfusion:TimeSpanEdit Value="0.08:00:00"
                         Format="h 'hours' m 'minutes'"
                         StepInterval="0.00:30:00"
                         ShowArrowButtons="True"
                         Width="180" />
```

User workflow:
1. Click the Hours field to select it
2. Click Up button to add 30 minutes
3. Click Minutes field to select it
4. Click Up button multiple times to adjust minutes by 30-minute increments

**C# Control:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.ShowArrowButtons = true;   // Enable (default)
editor.ShowArrowButtons = false;  // Disable for read-only display
```

---

## Mouse Wheel Interaction

Users can scroll the mouse wheel over the control to increment/decrement the selected field.

**Default Behavior:**
- Mouse wheel interaction is **enabled** by default
- Scroll Up (wheel forward) increases the selected field
- Scroll Down (wheel backward) decreases the selected field
- Increment/decrement follows the `StepInterval`

**Enable/Disable:**

*Enable (default):*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         IncrementOnScrolling="True" />
```

*Disable:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         IncrementOnScrolling="False" />
```

**Practical Example:**
```xml
<!-- Duration picker with all interactions enabled -->
<syncfusion:TimeSpanEdit Value="0.08:30:00"
                         Format="d 'days' h 'hrs' m 'mins' s 'secs'"
                         StepInterval="0.00:15:00"
                         IncrementOnScrolling="True"
                         ShowArrowButtons="True"
                         Width="200" />
```

**User Workflow:**
1. Click on the Hours field
2. Scroll mouse wheel forward (up) to increase hours
3. Scroll mouse wheel backward (down) to decrease hours

**C# Control:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.IncrementOnScrolling = true;   // Enable (default)
editor.IncrementOnScrolling = false;  // Disable (e.g., for read-only mode)
```

---

## Click & Drag (Extended Scrolling)

Advanced interaction mode allowing users to drag their mouse up/down over the control to adjust values.

**Default Behavior:**
- Click-and-drag is **disabled** by default
- When enabled, clicking and dragging up increases the selected field
- Dragging down decreases the selected field
- Follows the `StepInterval`
- Effective only when the control is **NOT focused** (unfocused state)

**Enable/Disable:**

*Enable extended scrolling:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         EnableExtendedScrolling="True" />
```

*Disable (default):*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         EnableExtendedScrolling="False" />
```

**Practical Example:**
```xml
<!-- Interactive duration picker with all interaction modes -->
<syncfusion:TimeSpanEdit Value="0.08:30:00"
                         Format="h 'hours' m 'minutes' s 'seconds'"
                         StepInterval="0.00:15:00"
                         ShowArrowButtons="True"
                         IncrementOnScrolling="True"
                         EnableExtendedScrolling="True"
                         Width="220" />
```

**User Workflow:**
1. Click on Hours field to select it
2. Click and drag mouse upward to increase hours
3. Click and drag mouse downward to decrease hours
4. Release mouse to stop adjusting

**Important Note:**
Extended scrolling requires the control to be **unfocused** (not actively editing). Once you start typing in a field, extended scrolling is disabled until you exit edit mode.

**C# Control:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.EnableExtendedScrolling = true;   // Enable
editor.EnableExtendedScrolling = false;  // Disable (default)
```

---

## Keyboard Arrow Navigation

Use arrow keys to navigate between fields and adjust values.

**Left/Right Arrows (Field Navigation):**
- **Left Arrow** (`←`) moves to the previous field (Days ← Hours ← Minutes ← Seconds)
- **Right Arrow** (`→`) moves to the next field (Days → Hours → Minutes → Seconds)

**Up/Down Arrows (Value Adjustment):**
- **Up Arrow** (`↑`) increases the selected field by `StepInterval`
- **Down Arrow** (`↓`) decreases the selected field by `StepInterval`

**Tab Key (Field Navigation):**
- **Tab** advances to the next field
- **Shift+Tab** goes to the previous field

**Example Keyboard Workflow:**

```
Initial:     5.12:30:45 (Days field selected)
Press Right: 5.12:30:45 (Hours field selected)
Press Right: 5.12:30:45 (Minutes field selected)
Press Up:    5.12:45:45 (Minutes increased by 15 [StepInterval])
Press Down:  5.12:30:45 (Minutes decreased by 15)
Press Left:  5.12:30:45 (Hours field selected)
```

**XAML Example (default keyboard support):**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         StepInterval="0.00:15:00"
                         Format="d.h:m:s" />
<!-- All keyboard navigation enabled by default -->
```

**Best Practices for Keyboard-Focused Users:**

```xml
<!-- Data entry form optimized for keyboard -->
<syncfusion:TimeSpanEdit Value="0.08:00:00"
                         Format="h 'hours' m 'minutes' s 'seconds'"
                         StepInterval="0.00:15:00"
                         ShowArrowButtons="True"
                         IncrementOnScrolling="False"
                         EnableExtendedScrolling="False"
                         Width="220" />
```

Users can:
1. Tab to the field
2. Use arrow keys to navigate between Days/Hours/Minutes/Seconds
3. Use Up/Down arrows to adjust the selected field
4. Tab to next control

---

## ReadOnly Mode

Prevent user editing while allowing display and value updates via code.

**Enable ReadOnly:**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         IsReadOnly="True" />
```

**ReadOnly Behavior:**
- ✓ Users **cannot** type in the field
- ✓ Users **cannot** use arrow keys to change values
- ✓ Users **cannot** click buttons to change values
- ✓ Mouse wheel scrolling is **disabled**
- ✓ Click-drag is **disabled**
- ✓ Value can still be changed **programmatically** in C# code
- ✓ Users can still **select/copy** the text

**Display-Only Scenario:**
```xml
<!-- Show calculated duration - read only -->
<syncfusion:TimeSpanEdit Value="7.18:45:30"
                         Format="d 'days' h 'hours' m 'minutes' s 'seconds'"
                         IsReadOnly="True"
                         ShowArrowButtons="False" />
```

**Disabling Interactions in Code:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.Value = new TimeSpan(5, 12, 30, 45);

// Make it read-only
editor.IsReadOnly = true;

// Can still update programmatically
void UpdateDuration() {
    editor.Value = new TimeSpan(6, 10, 15, 20);  // This works even in read-only mode
}

// Make it editable again
editor.IsReadOnly = false;
```

**Common Use Cases for ReadOnly:**

*Case 1: Display total elapsed time*
```xml
<syncfusion:TimeSpanEdit Value="0.12:45:30"
                         IsReadOnly="True"
                         NullString="No data"
                         Format="h 'hrs' m 'mins' s 'secs'" />
```

*Case 2: Summary/confirmation view*
```xml
<syncfusion:TimeSpanEdit Value="3.08:30:00"
                         IsReadOnly="True"
                         ShowArrowButtons="False"
                         Format="d 'day(s)' - h 'hours' - m 'minutes'" />
```

*Case 3: Toggling edit mode*
```csharp
bool isEditing = false;

void ToggleEditMode() {
    isEditing = !isEditing;
    timeSpanEdit.IsReadOnly = !isEditing;
    submitButton.IsEnabled = isEditing;
}
```

---

## Interaction Mode Summary

| Interaction | Default | Property | Use Case |
|-------------|---------|----------|----------|
| **Keyboard Navigation** | Enabled | Built-in | Data entry, accessibility |
| **UpDown Buttons** | Visible | `ShowArrowButtons` | Mouse-based adjustment |
| **Mouse Wheel** | Enabled | `IncrementOnScrolling` | Quick adjustments |
| **Click & Drag** | Disabled | `EnableExtendedScrolling` | Advanced users |
| **User Editing** | Enabled | `IsReadOnly` | Display-only mode |

---

## Next Steps

1. **Handle value changes** — Read [constraints-and-events.md](constraints-and-events.md) for ValueChanged events
2. **Add constraints** — Set Min/Max values in [constraints-and-events.md](constraints-and-events.md)
3. **Style it** — Customize colors and appearance in [appearance-and-theming.md](appearance-and-theming.md)
