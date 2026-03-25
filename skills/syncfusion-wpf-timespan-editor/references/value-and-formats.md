# Value Setting & Custom Formats

## Table of Contents
- [Setting Time Values Programmatically](#setting-time-values-programmatically)
- [Custom Format Strings](#custom-format-strings)
- [Format Specifiers](#format-specifiers)
- [Format Examples with Labels](#format-examples-with-labels)
- [Displaying Milliseconds](#displaying-milliseconds)
- [Null Values & Watermarks](#null-values--watermarks)

---

## Setting Time Values Programmatically

**Via XAML Binding:**
```xml
<syncfusion:TimeSpanEdit Value="10.11:32:43" />
```

**Via C# Code:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.Value = new TimeSpan(10, 11, 32, 43);
```

**Using the Value Property at Runtime:**
```csharp
// Set a new value
timeSpanEdit.Value = new TimeSpan(5, 8, 45, 30);  // 5 days, 8 hours, 45 minutes, 30 seconds

// Read the current value
TimeSpan currentDuration = timeSpanEdit.Value ?? TimeSpan.Zero;
MessageBox.Show($"Current value: {currentDuration}");

// Reset to zero
timeSpanEdit.Value = TimeSpan.Zero;
```

**TimeSpan Constructor Formats:**

The `TimeSpan` constructor supports multiple overloads:

```csharp
// Days, Hours, Minutes, Seconds
new TimeSpan(5, 12, 30, 45)

// Hours, Minutes, Seconds
new TimeSpan(12, 30, 45)

// Days, Hours, Minutes, Seconds, Milliseconds
new TimeSpan(5, 12, 30, 45, 500)

// Total milliseconds
new TimeSpan(milliseconds)

// Total ticks
TimeSpan.FromTicks(ticks)
```

**Practical Examples:**
```csharp
// 3 days
timeSpanEdit.Value = new TimeSpan(3, 0, 0, 0);

// 2 hours 30 minutes
timeSpanEdit.Value = new TimeSpan(0, 2, 30, 0);

// 1 day 12 hours 15 minutes 30 seconds
timeSpanEdit.Value = new TimeSpan(1, 12, 15, 30);

// Using helper methods
timeSpanEdit.Value = TimeSpan.FromHours(5);        // 5 hours
timeSpanEdit.Value = TimeSpan.FromMinutes(90);     // 90 minutes
timeSpanEdit.Value = TimeSpan.FromSeconds(3600);   // 3600 seconds (1 hour)
```

---

## Custom Format Strings

The `Format` property controls how the time is displayed in the control.

**Default Format:**
```csharp
Format = "d.h:m:s"  // Example: 5.12:30:45
```

---

## Format Specifiers

| Specifier | Meaning | Example |
|-----------|---------|---------|
| `d` | Days | `5` |
| `h` | Hours (0-23) | `12` |
| `m` | Minutes (0-59) | `30` |
| `s` | Seconds (0-59) | `45` |
| `z` | Milliseconds | `500` |

**Numeric Formatting:**
- Single specifier (`d`, `h`) → single or double digit
- Double specifier (`dd`, `hh`) → always two digits (zero-padded)

---

## Format Examples with Labels

**Simple Numeric Format:**
```xml
<syncfusion:TimeSpanEdit Format="d.h:m:s"
                         Value="5.12:30:45" />
<!-- Displays: 5.12:30:45 -->
```

**Labeled Format:**
```xml
<syncfusion:TimeSpanEdit Format="d 'days' h 'hours' m 'minutes' s 'seconds'"
                         Value="5.12:30:45" />
<!-- Displays: 5 days 12 hours 30 minutes 45 seconds -->
```

**Abbreviated Labels:**
```xml
<syncfusion:TimeSpanEdit Format="d 'D' h 'H' m 'M' s 'S'"
                         Value="5.12:30:45" />
<!-- Displays: 5 D 12 H 30 M 45 S -->
```

**User-Friendly Format:**
```xml
<syncfusion:TimeSpanEdit Format="d 'day(s)' h ':' m ':' s"
                         Value="5.12:30:45" />
<!-- Displays: 5 day(s) 12:30:45 -->
```

**Zero-Padded Format:**
```xml
<syncfusion:TimeSpanEdit Format="dd.hh:mm:ss"
                         Value="5.08:05:03" />
<!-- Displays: 05.08:05:03 (always 2 digits) -->
```

**Partial Time Display:**
```xml
<!-- Hours, Minutes, Seconds only (no days) -->
<syncfusion:TimeSpanEdit Format="h 'hrs' m 'mins' s 'secs'"
                         Value="5.12:30:45" />
<!-- Displays: 12 hrs 30 mins 45 secs (days component is part of total hours) -->
```

**Time Entry Form Format:**
```xml
<syncfusion:TimeSpanEdit Format="d':'h':'m':'s"
                         Value="2.14:25:30" />
<!-- Displays: 2:14:25:30 -->
```

---

## Displaying Milliseconds

To include milliseconds in the display, use the `z` specifier.

**With Milliseconds:**
```xml
<syncfusion:TimeSpanEdit Format="d.h:m:s.z"
                         Value="5.12:30:45.500" />
<!-- Displays: 5.12:30:45.500 -->
```

**Labeled Milliseconds:**
```xml
<syncfusion:TimeSpanEdit Format="d 'days' h ':' m ':' s '.' z 'ms'"
                         Value="5.12:30:45.250" />
<!-- Displays: 5 days 12:30:45.250 ms -->
```

**C# Example with Milliseconds:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.Format = "d.h:m:s.z";
editor.Value = new TimeSpan(5, 12, 30, 45, 500);  // 5d 12h 30m 45s 500ms
```

---

## Null Values & Watermarks

By default, TimeSpanEdit displays `0.0:0:0` for uninitialized values. You can allow null values and display placeholder text.

**Allow Null with No Text:**
```xml
<syncfusion:TimeSpanEdit AllowNull="True"
                         Value="{x:Null}" />
<!-- Displays: empty field -->
```

**Allow Null with Watermark (Placeholder):**
```xml
<syncfusion:TimeSpanEdit AllowNull="True"
                         Value="{x:Null}"
                         NullString="Enter duration..." />
<!-- When empty, shows: "Enter duration..." -->
```

**C# Code Example:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.AllowNull = true;
editor.Value = null;
editor.NullString = "Click to enter time...";
```

**Practical Null Value Scenarios:**

*Scenario 1: Optional Field*
```xml
<syncfusion:TimeSpanEdit AllowNull="True"
                         Value="{x:Null}"
                         NullString="Optional - leave blank if not applicable" />
```

*Scenario 2: Form Reset*
```csharp
// Clear the field
timeSpanEdit.AllowNull = true;
timeSpanEdit.Value = null;
```

*Scenario 3: Check for Null Before Processing*
```csharp
if (timeSpanEdit.Value == null) {
    MessageBox.Show("Please enter a duration");
    return;
}

TimeSpan duration = timeSpanEdit.Value.Value;
```

---

## Formatting Best Practices

**For Data Entry:**
- Use clear, numeric-only format: `"d.h:m:s"`
- Enable all interaction modes (keyboard, mouse, buttons)

**For Display (Read-Only):**
- Use labeled format: `"d 'days' h 'hours' m 'minutes' s 'secs'"`
- Set `IsReadOnly="True"`
- Hide buttons: `ShowArrowButtons="False"`

**For Time Intervals (Partial Days):**
```xml
<!-- Show hours:minutes:seconds for durations under 24 hours -->
<syncfusion:TimeSpanEdit Format="h 'h' m 'min' s 'sec'"
                         Value="0.12:30:45" />
<!-- Displays: 12 h 30 min 45 sec -->
```

**For Logging/Reports:**
```xml
<!-- Consistent format for data export -->
<syncfusion:TimeSpanEdit Format="dd.hh:mm:ss"
                         IsReadOnly="True" />
```

---

## Next Steps

1. **Add interactions** — Read [user-interactions.md](user-interactions.md) to enable keyboard/mouse controls
2. **Handle changes** — Check [constraints-and-events.md](constraints-and-events.md) for ValueChanged events
3. **Style it** — Use [appearance-and-theming.md](appearance-and-theming.md) to customize colors and themes
