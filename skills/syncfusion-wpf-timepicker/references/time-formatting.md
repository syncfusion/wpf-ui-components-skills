# Time Formatting in SfTimePicker

## FormatString Property: Display Formatting

The **FormatString** property controls how the selected time is displayed in the text portion of the control. The default is `"h:mm tt"` (12-hour format with AM/PM).

### Common Format Strings

| Format | Example | Description |
|--------|---------|-------------|
| `"h:mm tt"` | 2:30 PM | 12-hour format with AM/PM (default) |
| `"HH:mm:ss"` | 14:30:45 | 24-hour format with seconds |
| `"HH:mm"` | 14:30 | 24-hour format, no seconds |
| `"h:mm:ss tt"` | 2:30:45 PM | 12-hour with seconds and AM/PM |
| `"h mm tt"` | 2 30 PM | 12-hour with space separator |

### Setting FormatString in XAML

```xaml
<!-- 24-hour format -->
<syncfusion:SfTimePicker FormatString="HH:mm:ss" 
                         Name="timePicker"/>

<!-- 12-hour format (default) -->
<syncfusion:SfTimePicker FormatString="h:mm tt" 
                         Name="timePicker"/>
```

### Setting FormatString in C#

```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.FormatString = "HH:mm:ss";  // 24-hour with seconds
```

### Default Behavior

If you don't specify FormatString, the control uses `"h:mm tt"`:

```xaml
<!-- Displays as: 2:30 PM -->
<syncfusion:SfTimePicker Value="14:30:00" Name="timePicker"/>
```

## SelectorFormatString Property: Time Selector Format

The **SelectorFormatString** property controls which components appear in the time selector dropdown (hour, minute, second, meridiem). This determines what the user can select.

### Format String Codes

| Code | Component | Example |
|------|-----------|---------|
| `"h"` | Hour (12-hour) | 1-12 |
| `"H"` | Hour (24-hour) | 0-23 |
| `"m"` | Minute | 0-59 |
| `"s"` | Second | 0-59 |
| `"t"` | Meridiem (AM/PM) | AM or PM |

### Common Combinations

| Format | Components | Use Case |
|--------|------------|----------|
| `"h:mm tt"` | Hour + Minute + AM/PM | Standard 12-hour (default) |
| `"HH:mm"` | Hour + Minute (24-hr) | 24-hour format |
| `"HH:mm:ss"` | Hour + Minute + Second | Precise time with seconds |
| `"h/t"` | Hour + AM/PM | Hour and period only |
| `"m"` | Minute only | Select minutes only |
| `"H"` | Hour (24-hr) only | Select hours only |

### Setting SelectorFormatString in XAML

```xaml
<!-- User can select hour, minute, seconds, and AM/PM -->
<syncfusion:SfTimePicker SelectorFormatString="h:mm:ss tt" 
                         Name="timePicker"/>

<!-- User can select hour and AM/PM only -->
<syncfusion:SfTimePicker SelectorFormatString="h/t" 
                         Name="timePicker"/>
```

### Setting SelectorFormatString in C#

```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.SelectorFormatString = "HH:mm";  // 24-hour, hour + minute
```

## Combining FormatString and SelectorFormatString

Use both properties together for complete control over display and selection:

```xaml
<!-- Display and select in 24-hour format -->
<syncfusion:SfTimePicker FormatString="HH:mm:ss"
                         SelectorFormatString="HH:mm:ss"
                         Name="timePicker"/>

<!-- Display 12-hour, select hour and minute only -->
<syncfusion:SfTimePicker FormatString="h:mm tt"
                         SelectorFormatString="h:mm"
                         Name="timePicker"/>
```

## Culture-Specific Formatting

The resulting time display is influenced by **Regional Settings** and **Culture**. Different machines with different cultural settings may display time differently:

```csharp
// Set specific culture for formatting
System.Globalization.CultureInfo culture = 
    new System.Globalization.CultureInfo("en-US");

TimeSpan time = new TimeSpan(14, 30, 00);
string formatted = time.ToString(@"hh\:mm\:ss", culture);
// Result: 14:30:00
```

## Format String Examples

### Example 1: 24-Hour Military Time

```xaml
<syncfusion:SfTimePicker FormatString="HH:mm:ss"
                         SelectorFormatString="HH:mm:ss"
                         Value="14:30:00"
                         Name="timePicker"/>
```
**Display:** 14:30:00

### Example 2: 12-Hour with Seconds

```xaml
<syncfusion:SfTimePicker FormatString="h:mm:ss tt"
                         SelectorFormatString="h:mm:ss tt"
                         Value="14:30:45"
                         Name="timePicker"/>
```
**Display:** 2:30:45 PM

### Example 3: Hour and Period Selection Only

```xaml
<syncfusion:SfTimePicker FormatString="h:mm tt"
                         SelectorFormatString="h/t"
                         Value="14:30:00"
                         Name="timePicker"/>
```
**Display:** 2:30 PM  
**User can select:** Hour (1-12) and AM/PM only

### Example 4: Minute Selection Only

```xaml
<syncfusion:SfTimePicker FormatString="mm:ss"
                         SelectorFormatString="m"
                         Name="timePicker"/>
```
**User can select:** Minutes only (0-59)

## Standard .NET Time Format Specifiers

For advanced formatting, refer to [Microsoft's Standard Time Format Specifiers](https://docs.microsoft.com/en-us/previous-versions/dotnet/netframework-1.1/az4se3k1(v=vs.71)).

Common specifiers:
- `"h"` - 12-hour hour
- `"H"` - 24-hour hour
- `"m"` - Minute
- `"s"` - Second
- `"t"` - AM/PM

## Common Patterns

### Pattern 1: Business Hours (9 AM - 5 PM, 30-min increments)

```xaml
<syncfusion:SfTimePicker FormatString="h:mm tt"
                         SelectorFormatString="h/t"
                         Value="09:00:00"
                         Name="businessHours"/>
```

### Pattern 2: Precise Medical/Scientific Time

```xaml
<syncfusion:SfTimePicker FormatString="HH:mm:ss"
                         SelectorFormatString="HH:mm:ss"
                         Name="preciseTime"/>
```

### Pattern 3: Airport/Travel Time (24-hour)

```xaml
<syncfusion:SfTimePicker FormatString="HH:mm"
                         SelectorFormatString="HH:mm"
                         Name="departureTime"/>
```

## Next Steps

- Explore **[Time Selector Customization](time-selector-customization.md)** for advanced UI styling
- Learn **[Setting Time Values](setting-time-values.md)** for programmatic time management
