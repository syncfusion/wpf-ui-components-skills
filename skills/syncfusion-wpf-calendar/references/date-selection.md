# Date Selection in CalendarEdit

## Table of Contents
- [Single Date Selection](#single-date-selection)
- [Multiple Date Selection](#multiple-date-selection)
- [Using Selection Properties](#using-selection-properties)
- [Selection Events](#selection-events)
- [Common Patterns](#common-patterns)

---

## Single Date Selection

Users can select a single date by clicking on the date in the calendar. The selected date is stored in the `Date` property.

### Select a Date by Mouse Click

```xaml
<!-- Basic calendar for single date selection -->
<syncfusion:CalendarEdit Name="calendarEdit" />
```

Users click on any date in the calendar to select it. The clicked date becomes the current selection.

### Select a Date Programmatically

Set the date in code-behind or event handlers:

```csharp
// Set date to March 15, 2024
calendarEdit.Date = new DateTime(2024, 3, 15);

// Or set to today's date
calendarEdit.Date = DateTime.Today;
```

### In XAML

```xaml
<!-- Set initial date in XAML -->
<syncfusion:CalendarEdit Date="03/15/2024"
                         Name="calendarEdit" />
```

### Formatting Date Display

```xaml
<!-- Binding the date to display it -->
<TextBlock Text="{Binding ElementName=calendarEdit, Path=Date, StringFormat='Selected: {0:yyyy-MM-dd}'}" />
```

---

## Multiple Date Selection

### Enable Multiple Date Selection

By default, `AllowMultiplySelection` is `true`, enabling multiple date selection. Disable it by setting the property to `false`.

```xaml
<!-- Enable multiple date selection (default is true) -->
<syncfusion:CalendarEdit AllowMultiplySelection="True"
                         Name="calendarEdit" />
```

```csharp
// Enable multiple selection programmatically
calendarEdit.AllowMultiplySelection = true;
```

### Select Multiple Dates by Dragging

Users can select a contiguous range of dates by dragging from the start date to the end date:

1. **Click** on the starting date
2. **Hold and drag** to the ending date
3. **Release** to select the entire range

The selected dates are highlighted in the calendar.

### Select Specific Multiple Dates

Users can select non-contiguous dates by pressing **Ctrl** while clicking:

1. **Click** on the first date
2. **Hold Ctrl** and **click** on additional dates
3. Each clicked date is added to the selection

```csharp
// Example: User selects dates 5, 10, 15, 20
// Using Ctrl+Click in the UI
```

### Retrieve All Selected Dates

Use the `SelectedDates` property to get all selected dates:

```csharp
// Get all selected dates
var selectedDates = calendarEdit.SelectedDates;

// Iterate through selected dates
foreach (DateTime date in selectedDates)
{
    MessageBox.Show($"Selected: {date:yyyy-MM-dd}");
}

// Count selected dates
int count = calendarEdit.SelectedDates.Count;
MessageBox.Show($"Total dates selected: {count}");
```

### Disable Multiple Date Selection

To restrict selection to a single date:

```xaml
<syncfusion:CalendarEdit AllowMultiplySelection="False"
                         Name="calendarEdit" />
```

```csharp
// Disable multiple selection
calendarEdit.AllowMultiplySelection = false;
```

---

## Using Selection Properties

### Date Property

The `Date` property stores the currently selected date (primary selection).

```csharp
// Get the current date
DateTime currentDate = calendarEdit.Date;

// Set the current date
calendarEdit.Date = new DateTime(2024, 3, 15);

// Set to today
calendarEdit.Date = DateTime.Today;
```

### SelectedDates Collection

The `SelectedDates` property contains all selected dates when multiple selection is enabled.

```csharp
// Check if a specific date is selected
DateTime targetDate = new DateTime(2024, 3, 15);
bool isSelected = calendarEdit.SelectedDates.Contains(targetDate);

if (isSelected)
{
    MessageBox.Show("Date is selected");
}

// Get first selected date
DateTime firstDate = calendarEdit.SelectedDates.Count > 0 
    ? calendarEdit.SelectedDates[0] 
    : DateTime.MinValue;

// Get last selected date
DateTime lastDate = calendarEdit.SelectedDates.Count > 0 
    ? calendarEdit.SelectedDates[calendarEdit.SelectedDates.Count - 1] 
    : DateTime.MinValue;
```

### Difference Between Date and SelectedDates

- **`Date` property:** Always refers to the "current" or "focused" date (single)
- **`SelectedDates` collection:** Contains all selected dates (multiple)

When `AllowMultiplySelection` is `false`, `SelectedDates` mirrors the single `Date` selection.

---

## Selection Events

### SelectedDateChanged Event

Fires when the selected date changes:

```xaml
<syncfusion:CalendarEdit SelectedDateChanged="CalendarEdit_SelectedDateChanged"
                         Name="calendarEdit" />
```

```csharp
private void CalendarEdit_SelectedDateChanged(object sender, SelectionChangedEventArgs e)
{
    // Get the newly selected date
    DateTime selectedDate = calendarEdit.Date;
    MessageBox.Show($"New date selected: {selectedDate:yyyy-MM-dd}");
}
```

### Handling Multiple Selection Changes

```csharp
private void CalendarEdit_SelectedDateChanged(object sender, SelectionChangedEventArgs e)
{
    // Get all selected dates when multiple selection is enabled
    if (calendarEdit.AllowMultiplySelection)
    {
        int count = calendarEdit.SelectedDates.Count;
        MessageBox.Show($"Total selected dates: {count}");
        
        // Display all selected dates
        foreach (DateTime date in calendarEdit.SelectedDates)
        {
            Debug.WriteLine($"Selected: {date:yyyy-MM-dd}");
        }
    }
    else
    {
        DateTime date = calendarEdit.Date;
        MessageBox.Show($"Selected: {date:yyyy-MM-dd}");
    }
}
```

---

## Common Patterns

### Pattern 1: Single Date Picker

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Single date selection (default)
    calendarEdit.AllowMultiplySelection = false;
    calendarEdit.Date = DateTime.Today;
    calendarEdit.SelectedDateChanged += (s, e) =>
    {
        selectedDateTextBlock.Text = $"Selected: {calendarEdit.Date:yyyy-MM-dd}";
    };
}
```

### Pattern 2: Multiple Date Range Selection

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Enable multiple selection for range picking
    calendarEdit.AllowMultiplySelection = true;
    calendarEdit.Date = DateTime.Today;
    
    calendarEdit.SelectedDateChanged += (s, e) =>
    {
        // Display date range
        if (calendarEdit.SelectedDates.Count > 0)
        {
            DateTime start = calendarEdit.SelectedDates[0];
            DateTime end = calendarEdit.SelectedDates[calendarEdit.SelectedDates.Count - 1];
            rangeTextBlock.Text = $"Range: {start:yyyy-MM-dd} to {end:yyyy-MM-dd}";
        }
    };
}
```

### Pattern 3: Validate Selected Date

```csharp
private void ValidateSelection()
{
    DateTime selected = calendarEdit.Date;
    
    // Check if date is in the past
    if (selected < DateTime.Today)
    {
        MessageBox.Show("Past dates are not allowed");
        calendarEdit.Date = DateTime.Today;
    }
    
    // Check if date is on a weekend
    if (selected.DayOfWeek == DayOfWeek.Saturday || 
        selected.DayOfWeek == DayOfWeek.Sunday)
    {
        MessageBox.Show("Weekends are not available");
        calendarEdit.Date = DateTime.Today;
    }
}
```

### Pattern 4: Get Date Range for a Report

```csharp
private void GenerateReport()
{
    if (calendarEdit.AllowMultiplySelection && calendarEdit.SelectedDates.Count >= 2)
    {
        DateTime startDate = calendarEdit.SelectedDates[0];
        DateTime endDate = calendarEdit.SelectedDates[calendarEdit.SelectedDates.Count - 1];
        
        // Generate report for date range
        MessageBox.Show($"Report for {startDate:yyyy-MM-dd} to {endDate:yyyy-MM-dd}");
    }
    else
    {
        MessageBox.Show("Please select a date range");
    }
}
```

### Pattern 5: Clear Selection

```csharp
private void ClearSelection()
{
    calendarEdit.Date = DateTime.Today;
    
    if (calendarEdit.AllowMultiplySelection)
    {
        calendarEdit.SelectedDates.Clear();
    }
}
```

---

## Troubleshooting

### Issue: SelectedDates is empty in multiple selection mode

**Solution:** Ensure `AllowMultiplySelection` is `true` and users have actually selected multiple dates by dragging or Ctrl+clicking.

### Issue: Date property changes don't update the calendar

**Solution:** Verify the `Date` property is set to a valid `DateTime` and is within `MinDate`/`MaxDate` range if set.

### Issue: Selection changes not firing

**Solution:** Check that the `SelectedDateChanged` event handler is properly attached to the CalendarEdit control.

---

## Related Topics

- [Navigation Support](navigation.md) - Navigate between calendar views
- [Restrict Date Selection](restrict-dates.md) - Set date range constraints
- [Appearance Customization](appearance.md) - Style the calendar
