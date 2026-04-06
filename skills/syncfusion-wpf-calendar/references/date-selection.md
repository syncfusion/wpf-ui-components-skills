# Date Selection in CalendarEdit

## Overview

This document describes single and multiple date selection, the primary `Date` property, `SelectedDates` collection, and selection events.

## Single Date Selection

- Click a date in the calendar to set the `Date` property.
- Set `Date` programmatically: `calendarEdit.Date = new DateTime(2024, 3, 15);`

## Multiple Date Selection

- Use `AllowMultiplySelection = true` to enable multiple selection (default).
- Select a date range by dragging or pick non-contiguous dates with Ctrl+click.
- Read selected dates from `calendarEdit.SelectedDates`.

```csharp
// Iterate selected dates
foreach (DateTime d in calendarEdit.SelectedDates)
{
    Debug.WriteLine(d.ToString("yyyy-MM-dd"));
}
```

## Selection Events

- `SelectedDateChanged` fires when a selection changes.

```csharp
private void CalendarEdit_SelectedDateChanged(object sender, SelectionChangedEventArgs e)
{
    // Primary selected date
    DateTime primary = calendarEdit.Date;
}
```

## Common Patterns (short)

- Single-date picker: `AllowMultiplySelection = false;` and bind `Date` to UI.
- Range picker: enable `AllowMultiplySelection` and use `SelectedDates` for start/end.
- Clear selection: `calendarEdit.SelectedDates.Clear(); calendarEdit.Date = DateTime.Today;`

## Troubleshooting (short)

- If `SelectedDates` is empty, confirm `AllowMultiplySelection` and user selection method.
- If programmatic `Date` assignment has no effect, ensure date is within `MinDate/MaxDate`.

## Related

- [Navigation Support](navigation.md)
- [Restrict Date Selection](restrict-dates.md)
- [Appearance Customization](appearance.md)
