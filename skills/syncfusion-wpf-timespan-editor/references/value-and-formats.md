# Value Setting & Custom Formats

## Table of Contents
- [Setting Time Values Programmatically](#setting-time-values-programmatically)
- [Format Specifiers](#format-specifiers)
- [Format Examples with Labels](#format-examples-with-labels)
- [Displaying Milliseconds](#displaying-milliseconds)
- [Null Values & Watermarks](#null-values--watermarks)

---

## Setting Time Values Programmatically

| Method | Code | Note |
|--------|------|------|
| **XAML** | `<syncfusion:TimeSpanEdit Value="10.11:32:43" />` | String format (d.h:m:s) |
| **C# Direct** | `editor.Value = new TimeSpan(10, 11, 32, 43);` | Constructor with days, hours, min, sec |
| **C# Read** | `TimeSpan? val = editor.Value ?? TimeSpan.Zero;` | Handle nullable value |
| **Reset** | `editor.Value = TimeSpan.Zero;` | Clear value |

**TimeSpan Constructor Methods:**

| Constructor | Parameters | Result | Example |
|-----------|-----------|--------|---------|
| Full | Days, Hours, Minutes, Seconds | `TimeSpan(5, 12, 30, 45)` | 5.12:30:45 |
| Compact | Hours, Minutes, Seconds | `TimeSpan(12, 30, 45)` | 0.12:30:45 |
| With Milliseconds | Days, Hours, Minutes, Seconds, Milliseconds | `TimeSpan(5, 12, 30, 45, 500)` | 5.12:30:45.500 |
| FromHours | Total hours | `TimeSpan.FromHours(5)` | 0.05:00:00 |
| FromMinutes | Total minutes | `TimeSpan.FromMinutes(90)` | 0.01:30:00 |
| FromSeconds | Total seconds | `TimeSpan.FromSeconds(3600)` | 0.01:00:00 |
| FromMilliseconds | Total milliseconds | `TimeSpan.FromMilliseconds(5000)` | 0.00:00:05 |

**Quick Assignment Examples:**
```csharp
timeSpanEdit.Value = new TimeSpan(3, 0, 0, 0);     // 3 days
timeSpanEdit.Value = new TimeSpan(0, 2, 30, 0);    // 2 hours 30 minutes
timeSpanEdit.Value = TimeSpan.FromHours(5);        // 5 hours
timeSpanEdit.Value = TimeSpan.Zero;                // Clear value
```

---

## Format Specifiers

| Code | Meaning | Single | Double (Padded) |
|------|---------|--------|-----------------|
| **d** | Days | `5` | `dd` = `05` |
| **h** | Hours (0-23) | `12` | `hh` = `12` |
| **m** | Minutes (0-59) | `30` | `mm` = `30` |
| **s** | Seconds (0-59) | `45` | `ss` = `45` |
| **z** | Milliseconds | `500` | `zz` = `500` |

Use quotes for literals: `"d 'days' h 'hours'"`

---

## Format Examples with Labels

| Format String | Display Example | Use Case |
|----------------|-----------------|----------|
| `d.h:m:s` | 5.12:30:45 | Numeric/compact display |
| `d 'days' h 'hours' m 'minutes' s 'seconds'` | 5 days 12 hours 30 minutes 45 seconds | Full verbose labels |
| `d 'D' h 'H' m 'M' s 'S'` | 5 D 12 H 30 M 45 S | Abbreviated labels |
| `dd.hh:mm:ss` | 05.08:05:03 | Zero-padded (always 2 digits) |
| `h 'hrs' m 'mins' s 'secs'` | 12 hrs 30 mins 45 secs | Hours/minutes/seconds only |
| `d 'day(s)' h ':' m ':' s` | 5 day(s) 12:30:45 | Mixed format |

**Implementation:**
```xml
<!-- Numeric -->
<syncfusion:TimeSpanEdit Format="d.h:m:s" Value="5.12:30:45" />

<!-- Labeled -->
<syncfusion:TimeSpanEdit Format="d 'days' h 'hours' m 'minutes' s 'seconds'" 
                         Value="5.12:30:45" />

<!-- Zero-padded -->
<syncfusion:TimeSpanEdit Format="dd.hh:mm:ss" Value="5.08:05:03" />
```

---

## Displaying Milliseconds

Use the `z` specifier to include milliseconds in the format:

| Format | Display | C# Code |
|--------|---------|---------|
| `d.h:m:s.z` | 5.12:30:45.500 | `new TimeSpan(5, 12, 30, 45, 500)` |
| `d 'days' h ':' m ':' s '.' z 'ms'` | 5 days 12:30:45.250 ms | `new TimeSpan(5, 12, 30, 45, 250)` |

**Quick Setup:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.Format = "d.h:m:s.z";
editor.Value = new TimeSpan(5, 12, 30, 45, 500);  // Shows: 5.12:30:45.500
```

---

## Null Values & Watermarks

TimeSpanEdit displays `0.0:0:0` by default. Enable `AllowNull` to support empty values with placeholder text:

| Scenario | Property | XAML |
|----------|----------|------|
| Empty field (no text) | `AllowNull="True"` | `Value="{x:Null}"` |
| Placeholder text | `NullString` | `NullString="Enter duration..."` |
| Optional field | Both | `AllowNull="True"` + `NullString="Optional..."` |

**Setup Examples:**
```xml
<!-- Empty null with placeholder -->
<syncfusion:TimeSpanEdit AllowNull="True"
                         Value="{x:Null}"
                         NullString="Enter duration..." />
```

```csharp
// Programmatic setup
TimeSpanEdit editor = new TimeSpanEdit { AllowNull = true, NullString = "Click to enter time..." };

// Check for null before use
if (editor.Value == null) {
    MessageBox.Show("Please enter a duration");
    return;
}

TimeSpan duration = editor.Value.Value;
```

---

## Formatting Best Practices

| Use Case | Format | Key Attributes |
|----------|--------|-----------------|
| **Input** | `d.h:m:s` | `AllowNull="True"` `StepInterval="0.00:30:00"` |
| **Display** | `d 'days' h 'hours' m 'minutes'` | `IsReadOnly="True"` `ShowArrowButtons="False"` |
| **Export** | `dd.hh:mm:ss` | Consistent format for data persistence |

