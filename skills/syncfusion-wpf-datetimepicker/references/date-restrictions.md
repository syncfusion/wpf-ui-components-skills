# Date Restrictions & Null Values — DateTimeEdit (WPF)

## Table of Contents
- [Min and Max DateTime Range](#min-and-max-datetime-range)
- [BlackoutDates — Block Specific Dates](#blackoutdates--block-specific-dates)
- [DisableDateSelection — Month/Year Only](#disabledateselection--monthyear-only)
- [IsReadOnly — Prevent All Input](#isreadonly--prevent-all-input)
- [Null Value Support](#null-value-support)
- [Watermark Text When Null](#watermark-text-when-null)

---

## Min and Max DateTime Range

Prevent users from selecting dates outside a valid range using `MinDateTime` and `MaxDateTime`. The picker stops at the boundary — it cannot be pushed past min or max.

```xml
<syncfusion:DateTimeEdit MinDateTime="01/01/2020"
                         MaxDateTime="12/31/2030"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.MinDateTime = new DateTime(2020, 1, 1);
dateTimeEdit.MaxDateTime = new DateTime(2030, 12, 31);
```

> **Important:** `MaxDateTime` must always be greater than `MinDateTime`. If you set `MinDateTime` to a value greater than the current `MaxDateTime`, `MinDateTime` is automatically reset to match `MaxDateTime`.

**Listen for range changes:**
```csharp
dateTimeEdit.MinDateTimeChanged += (d, e) =>
{
    var oldMin = e.OldValue;
    var newMin = e.NewValue;
};

dateTimeEdit.MaxDateTimeChanged += (d, e) =>
{
    var oldMax = e.OldValue;
    var newMax = e.NewValue;
};
```

---

## BlackoutDates — Block Specific Dates

Prevent selection of a specific date range (e.g., holidays, unavailable dates). Apply via the calendar's `BlackoutDates` collection in the `Loaded` event:

```xml
<syncfusion:DateTimeEdit Loaded="DateTimeEdit_Loaded" Name="dateTimeEdit"/>
```

```csharp
private void DateTimeEdit_Loaded(object sender, RoutedEventArgs e)
{
    // Block all dates from the 1st to two days before today
    DateTime startDate = new DateTime(DateTime.Now.Year, DateTime.Now.Month, 1);
    DateTime endDate   = DateTime.Now.AddDays(-2);

    var blackOutRange = new Syncfusion.Windows.Controls.CalendarDateRange
    {
        Start = startDate,
        End   = endDate
    };

    var calendar = dateTimeEdit.DateTimeCalender
                   as Syncfusion.Windows.Controls.Calendar;
    calendar.BlackoutDates.Add(blackOutRange);
}
```

**Blackout behavior:**
- Typing a blackout date → reverts to the nearest valid date before the blocked range
- Pressing Down arrow into a blocked range → skips to the next valid date after the range

> **Warning:** Do not set `DateTime` to a value inside a blackout range — it throws `ArgumentOutOfRangeException`. Always validate before programmatic assignment.

---

## DisableDateSelection — Month/Year Only

Prevent selecting a specific day from the calendar popup. Users can only select month and year. Combine with a `CustomPattern` to show only month/year:

```xml
<syncfusion:DateTimeEdit DisableDateSelection="True"
                         Pattern="CustomPattern"
                         CustomPattern="MM-yyyy"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.DisableDateSelection = true;
dateTimeEdit.Pattern              = DateTimePattern.CustomPattern;
dateTimeEdit.CustomPattern        = "MM-yyyy";
```

> **Use case:** Month-year pickers for reporting filters (e.g., "select billing month").

---

## IsReadOnly — Prevent All Input

Make the control display-only. Users can still select text but cannot change the value via keyboard, mouse, or the popup.

```xml
<syncfusion:DateTimeEdit IsReadOnly="True"
                         DateTime="06/20/2024"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.IsReadOnly = true;
dateTimeEdit.DateTime   = new DateTime(2024, 6, 20);
```

> The value **can** still be changed programmatically in code-behind even when `IsReadOnly="True"`.

---

## Null Value Support

By default, `DateTimeEdit` always shows a date. To allow an empty/null state:

**Step 1:** Enable null date mode:
```xml
<syncfusion:DateTimeEdit IsEmptyDateEnabled="True"
                         NullValue="{x:Null}"
                         ShowMaskOnNullValue="False"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.IsEmptyDateEnabled  = true;
dateTimeEdit.NullValue           = null;
dateTimeEdit.ShowMaskOnNullValue = false;
```

| Property | Purpose |
|---|---|
| `IsEmptyDateEnabled` | Must be `true` to allow null state |
| `NullValue` | The value shown when date is cleared (set to `null`) |
| `ShowMaskOnNullValue` | `true` = show mask placeholder; `false` = show empty/watermark |

**Show mask pattern when null (instead of blank):**
```xml
<syncfusion:DateTimeEdit IsEmptyDateEnabled="True"
                         NullValue="{x:Null}"
                         ShowMaskOnNullValue="True"
                         Name="dateTimeEdit"/>
```

---

## Watermark Text When Null

Display a text prompt when no date is selected. Requires null value to be enabled:

```xml
<syncfusion:DateTimeEdit NoneDateText="Select a date..."
                         IsEmptyDateEnabled="True"
                         NullValue="{x:Null}"
                         ShowMaskOnNullValue="False"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.NoneDateText        = "Select a date...";
dateTimeEdit.IsEmptyDateEnabled  = true;
dateTimeEdit.NullValue           = null;
dateTimeEdit.ShowMaskOnNullValue = false;
```

**Full null + watermark MVVM pattern:**
```csharp
// ViewModel.cs
public class ViewModel : NotificationObject
{
    private DateTime? _selectedDate = null;
    public DateTime? SelectedDate
    {
        get => _selectedDate;
        set { _selectedDate = value; RaisePropertyChanged(nameof(SelectedDate)); }
    }
}
```

```xml
<syncfusion:DateTimeEdit NoneDateText="No date selected"
                         IsEmptyDateEnabled="True"
                         ShowMaskOnNullValue="False"
                         NullValue="{x:Null}"
                         Name="dateTimeEdit"/>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `ArgumentOutOfRangeException` on DateTime set | Value is in `BlackoutDates` range | Validate date against blackout ranges before setting |
| Null value not working | `IsEmptyDateEnabled` is `false` | Set `IsEmptyDateEnabled="True"` |
| Watermark not showing | `ShowMaskOnNullValue` is `true` | Set `ShowMaskOnNullValue="False"` |
| MinDateTime > MaxDateTime silently resets | WPF property coercion behavior | Always set `MaxDateTime` before `MinDateTime`, or set them together |
| DisableDateSelection has no effect | `DropDownView` not showing calendar | Only affects calendar popup — no effect on text field editing |
