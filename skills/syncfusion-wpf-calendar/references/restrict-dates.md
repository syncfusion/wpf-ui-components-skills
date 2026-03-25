# Restrict Date Selection in CalendarEdit

## Overview

The CalendarEdit control provides multiple mechanisms to restrict which dates users can select, including date range constraints, specific date blocking, and complete selection disabling.

---

## Restrict Date Selection Within a Range

### Set Minimum and Maximum Dates

Use the `MinDate` and `MaxDate` properties to define a valid date range. Users can only select dates within this range:

```xaml
<!-- Allow selection only in March 2024 -->
<syncfusion:CalendarEdit MinDate="03/01/2024" 
                         MaxDate="03/31/2024"
                         Name="calendarEdit"/>
```

```csharp
// Set date range
calendarEdit.MinDate = new DateTime(2024, 3, 1);
calendarEdit.MaxDate = new DateTime(2024, 3, 31);
```

### Important Constraint

The `MaxDate` must be greater than or equal to `MinDate`. If you try to set an invalid range:
- If `MaxDate` < `MinDate`, the `MinDate` will be automatically reset to match the new `MaxDate`
- This ensures the control always maintains a valid date range

```csharp
// Setting invalid range
calendarEdit.MinDate = new DateTime(2024, 3, 31);
calendarEdit.MaxDate = new DateTime(2024, 3, 1);  // MaxDate < MinDate

// Result: MinDate is automatically reset to match MaxDate (3/1/2024)
```

### Date Range Examples

```csharp
// Current month only
calendarEdit.MinDate = new DateTime(DateTime.Now.Year, DateTime.Now.Month, 1);
calendarEdit.MaxDate = new DateTime(DateTime.Now.Year, DateTime.Now.Month, 
    DateTime.DaysInMonth(DateTime.Now.Year, DateTime.Now.Month));

// Business days only (Mon-Fri)
calendarEdit.MinDate = DateTime.Today;
calendarEdit.MaxDate = DateTime.Today.AddYears(1);

// Appointment booking (next 90 days)
calendarEdit.MinDate = DateTime.Today.AddDays(1);  // Cannot select today
calendarEdit.MaxDate = DateTime.Today.AddDays(90);
```

---

## Show or Hide Disabled Dates

### Display Disabled Dates

By default, dates outside the MinDate/MaxDate range are hidden. Set `MinMaxHidden` to `false` to display them as disabled:

```xaml
<!-- Show disabled dates in gray -->
<syncfusion:CalendarEdit MinMaxHidden="False"
                         MinDate="03/05/2024" 
                         MaxDate="03/25/2024"
                         Name="calendarEdit"/>
```

```csharp
// Show disabled dates
calendarEdit.MinDate = new DateTime(2024, 3, 5);
calendarEdit.MaxDate = new DateTime(2024, 3, 25);
calendarEdit.MinMaxHidden = false;  // Display dates outside range as disabled
```

### Default Behavior (Hide Disabled Dates)

```xaml
<!-- Default: hide dates outside range -->
<syncfusion:CalendarEdit MinMaxHidden="True"
                         MinDate="03/05/2024" 
                         MaxDate="03/25/2024"
                         Name="calendarEdit"/>
```

### Visual Difference

When `MinMaxHidden` is `false`:
- Selectable dates (within range): Normal appearance
- Disabled dates (outside range): Grayed out, non-clickable
- Users see the full calendar layout but understand which dates are unavailable

When `MinMaxHidden` is `true`:
- Only selectable dates are displayed
- The calendar shows only the date range

---

## Disable All Date Selection

### Restrict Users from Selecting Dates

Use the `DisableDateSelection` property to prevent users from selecting individual dates while still allowing month/year/decade navigation:

```xaml
<syncfusion:CalendarEdit DisableDateSelection="true"
                         Name="calendarEdit"/>
```

```csharp
// Disable date selection
calendarEdit.DisableDateSelection = true;

// Users can still navigate months and years
// But cannot click individual dates
```

### Use Cases

- **Month/Year only picker:** When you need users to select a month or year, not a specific date
- **Read-only calendar:** Display calendar information without allowing selection
- **Navigation only:** Allow browsing through dates without selection

### What's Still Allowed

Even with `DisableDateSelection = true`, users can:
- ✓ Navigate to different months
- ✓ Navigate to year/decade views
- ✓ Click header to change navigation modes
- ✓ Use keyboard shortcuts (Alt+arrows)
- ✓ Scroll through months

---

## Block Specific Dates (Blackout Dates)

### Add Dates to BlackoutDates Collection

Use the `BlackoutDates` collection to block specific dates from selection:

```csharp
CalendarEdit calendarEdit = new CalendarEdit();

// Block specific dates
calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 5));   // March 5
calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 10));  // March 10
calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 15));  // March 15

// Users cannot select these dates
calendarEdit.Date = new DateTime(2024, 3, 15);  // This date cannot be selected
```

### Block Date Ranges

To block multiple consecutive dates, add them programmatically:

```csharp
// Block a date range (March 5-10)
for (int day = 5; day <= 10; day++)
{
    calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, day));
}
```

### Clear Blackout Dates

```csharp
// Remove all blackout dates
calendarEdit.BlackoutDates.Clear();

// Or remove specific dates
calendarEdit.BlackoutDates.Remove(new DateTime(2024, 3, 5));
```

### Combining Restrictions

You can combine multiple restriction methods:

```csharp
// Set date range
calendarEdit.MinDate = new DateTime(2024, 3, 1);
calendarEdit.MaxDate = new DateTime(2024, 3, 31);

// Block specific dates within that range
calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 5));   // Maintenance day
calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 12));  // Holiday
calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 19));  // Closed

// Show disabled dates
calendarEdit.MinMaxHidden = false;
```

---

## Common Patterns

### Pattern 1: Booking System (Next 90 Days, No Weekends)

```csharp
public void SetupBookingCalendar()
{
    // Allow bookings for next 90 days only
    calendarEdit.MinDate = DateTime.Today.AddDays(1);  // No same-day bookings
    calendarEdit.MaxDate = DateTime.Today.AddDays(90);
    
    // Block weekends
    for (int i = 1; i <= 90; i++)
    {
        DateTime date = DateTime.Today.AddDays(i);
        if (date.DayOfWeek == DayOfWeek.Saturday || 
            date.DayOfWeek == DayOfWeek.Sunday)
        {
            calendarEdit.BlackoutDates.Add(date);
        }
    }
    
    calendarEdit.MinMaxHidden = false;  // Show disabled dates
}
```

### Pattern 2: Business Hours Only (Mon-Fri, 9-5)

```csharp
public void SetupBusinessDaysCalendar()
{
    calendarEdit.MinDate = DateTime.Today;
    calendarEdit.MaxDate = DateTime.Today.AddYears(1);
    
    // Block weekends
    for (DateTime date = DateTime.Today; date < DateTime.Today.AddYears(1); 
         date = date.AddDays(1))
    {
        if (date.DayOfWeek == DayOfWeek.Saturday || 
            date.DayOfWeek == DayOfWeek.Sunday)
        {
            calendarEdit.BlackoutDates.Add(date);
        }
    }
    
    calendarEdit.MinMaxHidden = false;
}
```

### Pattern 3: Current Month Only

```xaml
<syncfusion:CalendarEdit Name="calendarEdit" 
                         Loaded="CalendarEdit_Loaded" />
```

```csharp
private void CalendarEdit_Loaded(object sender, RoutedEventArgs e)
{
    DateTime now = DateTime.Now;
    calendarEdit.MinDate = new DateTime(now.Year, now.Month, 1);
    calendarEdit.MaxDate = new DateTime(now.Year, now.Month,
        DateTime.DaysInMonth(now.Year, now.Month));
}
```

### Pattern 4: Appointment Conflict Avoidance

```csharp
private void BlockConflictingDates(List<DateTime> bookedDates)
{
    foreach (DateTime date in bookedDates)
    {
        calendarEdit.BlackoutDates.Add(date);
    }
    
    calendarEdit.MinMaxHidden = false;  // Visual feedback for blocked dates
}

// Usage
List<DateTime> appointments = new List<DateTime>
{
    new DateTime(2024, 3, 5),
    new DateTime(2024, 3, 10),
    new DateTime(2024, 3, 15)
};
BlockConflictingDates(appointments);
```

### Pattern 5: Holiday Blocking

```csharp
private void BlockHolidays()
{
    // Block US holidays for March-May 2024
    calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 17));  // St. Patrick's Day
    calendarEdit.BlackoutDates.Add(new DateTime(2024, 4, 1));   // Easter Monday
    calendarEdit.BlackoutDates.Add(new DateTime(2024, 5, 27));  // Memorial Day
    
    calendarEdit.MinMaxHidden = false;  // Show blocked dates
}
```

---

## Validating Restrictions

### Check if a Date is Valid

```csharp
private bool IsDateSelectable(DateTime date)
{
    // Check MinDate/MaxDate range
    if (date < calendarEdit.MinDate || date > calendarEdit.MaxDate)
        return false;
    
    // Check if in blackout dates
    if (calendarEdit.BlackoutDates.Contains(date))
        return false;
    
    // Check if date selection is disabled entirely
    if (calendarEdit.DisableDateSelection)
        return false;
    
    return true;
}

// Usage
DateTime testDate = new DateTime(2024, 3, 15);
if (IsDateSelectable(testDate))
{
    calendarEdit.Date = testDate;
}
else
{
    MessageBox.Show("This date is not available for selection");
}
```

---

## Troubleshooting

### Issue: MinDate/MaxDate not being respected

**Solution:** Ensure you're setting these properties after the control is loaded. Set them in the Loaded event or during initialization, not in XAML markup if binding is involved.

### Issue: Blackout dates not showing as disabled

**Solution:** Set `MinMaxHidden = false` to display blackout dates visually. Otherwise, they're still blocked but may not be visually apparent.

### Issue: Users can still type dates outside restrictions

**Solution:** The Date property enforces restrictions. If invalid, the date will be clamped to the MinDate/MaxDate range automatically.

### Issue: Cannot set a date programmatically

**Solution:** Check if the date you're setting is within the allowed range. If it's outside MinDate/MaxDate or in BlackoutDates, the assignment will fail or be corrected.

```csharp
// This may not work if date is outside restrictions
calendarEdit.Date = new DateTime(2024, 12, 25);  // If MaxDate is 3/31/2024

// Better: Check first
DateTime targetDate = new DateTime(2024, 12, 25);
if (IsDateSelectable(targetDate))
{
    calendarEdit.Date = targetDate;
}
else
{
    calendarEdit.Date = calendarEdit.MaxDate;  // Fall back to max date
}
```

---

## Related Topics

- [Date Selection](date-selection.md) - Select dates once restrictions are set
- [Navigation Support](navigation.md) - Navigate between restricted dates
- [Appearance Customization](appearance.md) - Style disabled dates visually
