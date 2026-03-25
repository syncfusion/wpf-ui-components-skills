# Formatting & Culture — DateTimeEdit (WPF)

Control how dates and times are displayed using predefined patterns, custom patterns, `DateTimeFormatInfo`, and culture settings.

---

## Predefined Patterns

Set the `Pattern` property to one of the built-in `DateTimePattern` enum values. Default is `ShortDate`.

| Pattern | Example (en-US) | Use Case |
|---|---|---|
| `ShortDate` | 7/5/2024 | Default date input |
| `LongDate` | Thursday, July 5, 2024 | Full date display with day name |
| `ShortTime` | 3:45 PM | Time-only input |
| `LongTime` | 3:45:22 PM | Time with seconds |
| `FullDateTime` | Thursday, July 5, 2024 3:45 PM | Combined date and time |
| `MonthDay` | July 5 | Month and day only |
| `YearMonth` | July 2024 | Month and year only |
| `RFC1123` | Thu, 05 Jul 2024 15:45:22 GMT | HTTP/email date format |
| `ShortableDateTime` | 2024-07-05T15:45:22 | ISO-like sortable |
| `UniversalShortableDateTime` | 2024-07-05 15:45:22Z | UTC sortable |
| `CustomPattern` | *(defined by `CustomPattern` property)* | Any custom format |

```xml
<syncfusion:DateTimeEdit Pattern="FullDateTime" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.Pattern = DateTimePattern.FullDateTime;
```

### PatternChanged Event

```csharp
dateTimeEdit.PatternChanged += (d, e) =>
{
    var oldPattern = e.OldValue;
    var newPattern = e.NewValue;
};
```

---

## Custom Pattern

Display the date in any format by setting `Pattern="CustomPattern"` and specifying the format string in `CustomPattern`:

```xml
<syncfusion:DateTimeEdit Pattern="CustomPattern"
                         CustomPattern="dd-MMM-yyyy"
                         DateTime="07/05/2024"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.Pattern       = DateTimePattern.CustomPattern;
dateTimeEdit.CustomPattern = "dd-MMM-yyyy";
```

**Common custom patterns:**

| CustomPattern | Example Output |
|---|---|
| `dd/MM/yyyy` | 05/07/2024 |
| `MM-dd-yy` | 07-05-24 |
| `dd-MMM-yyyy` | 05-Jul-2024 |
| `MM**dd**yy hh:mm:ss` | 07\*\*05\*\*24 03:45:22 |
| `MM-yyyy` | 07-2024 *(month + year only)* |
| `yyyy/MM/dd HH:mm` | 2024/07/05 15:45 |

> **Tip:** Use `MM-yyyy` with `DisableDateSelection="True"` to create a month-year-only picker.

### CustomPatternChanged Event

```csharp
dateTimeEdit.CustomPatternChanged += (d, e) =>
{
    var oldPattern = e.OldValue;
    var newPattern = e.NewValue;
};
```

---

## DateTimeFormat Property

Use `DateTimeFormat` with a `DateTimeFormatInfo` object for fine-grained control over separators and field order:

```xml
<syncfusion:DateTimeEdit Name="dateTimeEdit">
    <syncfusion:DateTimeEdit.DateTimeFormat>
        <global:DateTimeFormatInfo ShortDatePattern="MM/dd/yy hh:mm:ss"/>
    </syncfusion:DateTimeEdit.DateTimeFormat>
</syncfusion:DateTimeEdit>
```

```csharp
dateTimeEdit.DateTimeFormat = new System.Globalization.DateTimeFormatInfo
{
    ShortDatePattern = "MM/dd/yy hh:mm:ss"
};
```

> `DateTimeFormat` gives lower-level control than `Pattern`. Use it when the predefined patterns don't match the exact separator or field-order requirement.

---

## CultureInfo — Locale-Specific Formatting

Change the display locale to match a specific culture's date format conventions:

```xml
<!-- French locale -->
<syncfusion:DateTimeEdit CultureInfo="fr-FR"
                         Pattern="FullDateTime"
                         Name="dateTimeEdit"/>

<!-- Japanese locale -->
<syncfusion:DateTimeEdit CultureInfo="ja-JP"
                         Pattern="LongDate"
                         Name="dateTimeEdit"/>
```

```csharp
using System.Globalization;

// French
dateTimeEdit.CultureInfo = new CultureInfo("fr-FR");
dateTimeEdit.Pattern = DateTimePattern.FullDateTime;

// US English (default)
dateTimeEdit.CultureInfo = new CultureInfo("en-US");
```

**Effect of CultureInfo:**
- Changes date separator characters (e.g., `.` in German, `/` in US)
- Changes month and day name language (e.g., "juillet" for July in French)
- Changes order of date fields (DD/MM vs MM/DD)
- Does **not** translate button labels — use localization resource files for that

> For translating Today/None button labels, see [localization.md](localization.md).

---

## Custom Month Names

Override the abbreviated month names shown in the popup calendar header:

```xml
<syncfusion:DateTimeEdit Name="dateTimeEdit">
    <syncfusion:DateTimeEdit.AbbreviatedMonthNames>
        <x:Array Type="sys:String" xmlns:sys="clr-namespace:System;assembly=mscorlib">
            <sys:String>[1]Jan</sys:String>
            <sys:String>[2]Feb</sys:String>
            <sys:String>[3]Mar</sys:String>
            <sys:String>[4]Apr</sys:String>
            <sys:String>[5]May</sys:String>
            <sys:String>[6]Jun</sys:String>
            <sys:String>[7]Jul</sys:String>
            <sys:String>[8]Aug</sys:String>
            <sys:String>[9]Sep</sys:String>
            <sys:String>[10]Oct</sys:String>
            <sys:String>[11]Nov</sys:String>
            <sys:String>[12]Dec</sys:String>
        </x:Array>
    </syncfusion:DateTimeEdit.AbbreviatedMonthNames>
</syncfusion:DateTimeEdit>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `CustomPattern` not applied | `Pattern` not set to `CustomPattern` | Set `Pattern="CustomPattern"` explicitly |
| Culture changed but buttons still in English | `CultureInfo` only affects format, not UI strings | Use localization resource files for Today/None labels |
| `DateTimeFormat` not working | Pattern overrides DateTimeFormat | Set `Pattern` before `DateTimeFormat`, or use `CustomPattern` instead |
