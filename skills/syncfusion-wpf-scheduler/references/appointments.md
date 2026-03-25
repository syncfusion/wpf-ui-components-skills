# Appointments in WPF Scheduler

## Table of Contents
- [Overview](#overview)
- [ScheduleAppointment Class](#scheduleappointment-class)
- [Creating Appointments](#creating-appointments)
- [Custom Business Objects and Data Mapping](#custom-business-objects-and-data-mapping)
- [Appointment Types](#appointment-types)
  - [Normal Appointments](#normal-appointments)
  - [All-Day Appointments](#all-day-appointments)
  - [Spanned Appointments](#spanned-appointments)
- [Recurrence Appointments](#recurrence-appointments)
  - [Recurrence Rule Format (RRULE)](#recurrence-rule-format-rrule)
  - [Creating Recurring Appointments](#creating-recurring-appointments)
  - [Recurrence Patterns](#recurrence-patterns)
  - [Recurrence Exceptions](#recurrence-exceptions)
- [Appointment Rendering](#appointment-rendering)

## Overview

The WPF Scheduler has built-in capability to handle appointment arrangement internally based on the ScheduleAppointmentCollection. The Scheduler supports rendering normal, all-day, spanned, recurring appointments, and recurrence exception dates. Appointments are automatically arranged based on their start time and duration in all views.

## ScheduleAppointment Class

The `ScheduleAppointment` class represents a scheduled appointment with basic and advanced properties:

**Basic Properties:**
- `StartTime` (DateTime) - Start date and time
- `EndTime` (DateTime) - End date and time
- `Subject` (string) - Title/subject of the appointment
- `Notes` (string) - Additional notes or description
- `Location` (string) - Location of the appointment

**Appearance Properties:**
- `AppointmentBackground` (Brush) - Background color
- `Foreground` (Brush) - Text color

**Special Properties:**
- `IsAllDay` (bool) - Marks appointment as all-day
- `RecurrenceRule` (string) - RRULE for recurring appointments
- `RecurrenceId` (object) - Links to pattern appointment
- `RecurrenceExceptionDates` (ObservableCollection<DateTime>) - Exception dates
- `ResourceIdCollection` (ObservableCollection<object>) - Associated resources

**Example:**

```csharp
var appointment = new ScheduleAppointment
{
    StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
    EndTime = new DateTime(2026, 3, 24, 12, 0, 0),
    Subject = "Client Meeting",
    Location = "Conference Room A",
    Notes = "Discuss project requirements",
    AppointmentBackground = Brushes.LightBlue,
    Foreground = Brushes.White
};
```

## Creating Appointments

### Using ScheduleAppointmentCollection

```csharp
using Syncfusion.UI.Xaml.Scheduler;

// Create appointment collection
var appointments = new ScheduleAppointmentCollection();

// Add appointments
appointments.Add(new ScheduleAppointment
{
    StartTime = DateTime.Now.Date.AddHours(9),
    EndTime = DateTime.Now.Date.AddHours(10),
    Subject = "Team Meeting",
    AppointmentBackground = Brushes.Green
});

// Assign to scheduler
Schedule.ItemsSource = appointments;
```

### XAML Binding

```xaml
<syncfusion:SfScheduler x:Name="Schedule"
                        ViewType="Week"
                        ItemsSource="{Binding AppointmentCollection}" />
```

## Custom Business Objects and Data Mapping

Instead of using `ScheduleAppointment` directly, map your own business objects using `AppointmentMapping`.

### Step 1: Create Custom Class

```csharp
public class Meeting : INotifyPropertyChanged
{
    private DateTime from, to;
    private string eventName, location, notes;
    private Brush backgroundColor, foregroundColor;
    private bool isAllDay;
    private string recurrenceRule;
    private object id, recurrenceId;
    private ObservableCollection<DateTime> recurrenceExceptions;
    private ObservableCollection<object> resourceIds;

    public DateTime From
    {
        get => from;
        set { from = value; OnPropertyChanged("From"); }
    }

    public DateTime To
    {
        get => to;
        set { to = value; OnPropertyChanged("To"); }
    }

    public string EventName
    {
        get => eventName;
        set { eventName = value; OnPropertyChanged("EventName"); }
    }

    public Brush BackgroundColor { get; set; }
    public Brush ForegroundColor { get; set; }
    public bool IsAllDay { get; set; }
    public string RecurrenceRule { get; set; }
    public object Id { get; set; }
    public object RecurrenceId { get; set; }
    public ObservableCollection<DateTime> RecurrenceExceptions { get; set; }
    public ObservableCollection<object> ResourceIds { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

### Step 2: Map Properties

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule"
                        ViewType="Week"
                        ItemsSource="{Binding Meetings}">
    <syncfusion:SfScheduler.AppointmentMapping>
        <syncfusion:AppointmentMapping
            Subject="EventName"
            StartTime="From"
            EndTime="To"
            Id="Id"
            AppointmentBackground="BackgroundColor"
            Foreground="ForegroundColor"
            IsAllDay="IsAllDay"
            RecurrenceRule="RecurrenceRule"
            RecurrenceId="RecurrenceId"
            RecurrenceExceptionDates="RecurrenceExceptions"
            ResourceIdCollection="ResourceIds"
            Location="Location"
            Notes="Notes" />
    </syncfusion:SfScheduler.AppointmentMapping>
</syncfusion:SfScheduler>
```

**C#:**

```csharp
var mapping = new AppointmentMapping
{
    Subject = "EventName",
    StartTime = "From",
    EndTime = "To",
    Id = "Id",
    AppointmentBackground = "BackgroundColor",
    Foreground = "ForegroundColor",
    IsAllDay = "IsAllDay",
    RecurrenceRule = "RecurrenceRule",
    RecurrenceId = "RecurrenceId",
    RecurrenceExceptionDates = "RecurrenceExceptions",
    ResourceIdCollection = "ResourceIds"
};

Schedule.AppointmentMapping = mapping;
```

**AppointmentMapping Properties Table:**

| Mapping Property | Maps To | Description |
|------------------|---------|-------------|
| StartTime | DateTime | Appointment start time |
| EndTime | DateTime | Appointment end time |
| Subject | string | Appointment title |
| Id | object | Unique identifier |
| AppointmentBackground | Brush | Background color |
| Foreground | Brush | Text color |
| IsAllDay | bool | All-day indicator |
| RecurrenceRule | string | RRULE format string |
| RecurrenceId | object | Parent recurrence ID |
| RecurrenceExceptionDates | ObservableCollection<DateTime> | Exception dates |
| ResourceIdCollection | ObservableCollection<object> | Resource IDs |
| Location | string | Appointment location |
| Notes | string | Additional notes |
| StartTimeZone | string | Start timezone |
| EndTimeZone | string | End timezone |

## Appointment Types

### Normal Appointments

Standard appointments that occupy time slots based on duration.

```csharp
var meeting = new ScheduleAppointment
{
    StartTime = DateTime.Now.Date.AddHours(14),
    EndTime = DateTime.Now.Date.AddHours(15),
    Subject = "Project Review",
    AppointmentBackground = Brushes.Blue
};
```

### All-Day Appointments

Appointments that last the entire day, displayed in the all-day panel.

```csharp
var holiday = new ScheduleAppointment
{
    StartTime = new DateTime(2026, 7, 4, 0, 0, 0),
    EndTime = new DateTime(2026, 7, 4, 23, 59, 59),
    Subject = "Independence Day",
    IsAllDay = true,
    AppointmentBackground = Brushes.Red
};
```

**Automatic All-Day Detection:**
Appointments spanning exactly 24 hours (e.g., 06/24/2026 12:00 AM to 06/25/2026 12:00 AM) are automatically treated as all-day even without setting `IsAllDay`.

### Spanned Appointments

Appointments lasting more than 24 hours appear in the all-day panel.

```csharp
var conference = new ScheduleAppointment
{
    StartTime = DateTime.Now.Date.AddHours(10),
    EndTime = DateTime.Now.Date.AddDays(2).AddHours(16),
    Subject = "Annual Conference",
    AppointmentBackground = Brushes.Orange
};
```

**Note:** Spanned appointments don't block time slots; they render exclusively in the AllDayAppointmentPanel.

## Recurrence Appointments

Recurring appointments repeat on daily, weekly, monthly, or yearly intervals using the RRULE format (RFC 5545 standard).

### Recurrence Rule Format (RRULE)

The `RecurrenceRule` is a string containing recurrence pattern details:

**RRULE Properties:**

| Property | Description | Example |
|----------|-------------|---------|
| FREQ | Frequency type | DAILY, WEEKLY, MONTHLY, YEARLY |
| INTERVAL | Interval between occurrences | 1, 2, 3, etc. |
| COUNT | Number of occurrences | COUNT=10 |
| UNTIL | End date | UNTIL=20261231 |
| BYDAY | Days of week | MO, TU, WE, TH, FR, SA, SU |
| BYMONTHDAY | Day of month | 1-31 |
| BYMONTH | Month of year | 1-12 |
| BYSETPOS | Week position | 1 (first), 2 (second), -1 (last) |

### Creating Recurring Appointments

**Daily Recurrence:**

```csharp
var dailyMeeting = new ScheduleAppointment
{
    Id = 1,
    StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
    EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
    Subject = "Daily Standup",
    RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=30",
    AppointmentBackground = Brushes.Green
};
```

**Weekly Recurrence:**

```csharp
var weeklyMeeting = new ScheduleAppointment
{
    Id = 2,
    StartTime = new DateTime(2026, 3, 24, 14, 0, 0),
    EndTime = new DateTime(2026, 3, 24, 15, 0, 0),
    Subject = "Team Sync",
    RecurrenceRule = "FREQ=WEEKLY;BYDAY=MO,WE,FR;COUNT=12",
    AppointmentBackground = Brushes.Blue
};
```

**Monthly Recurrence:**

```csharp
var monthlyReview = new ScheduleAppointment
{
    Id = 3,
    StartTime = new DateTime(2026, 3, 15, 10, 0, 0),
    EndTime = new DateTime(2026, 3, 15, 11, 0, 0),
    Subject = "Monthly Review",
    RecurrenceRule = "FREQ=MONTHLY;BYMONTHDAY=15;COUNT=12",
    AppointmentBackground = Brushes.Purple
};
```

**Yearly Recurrence:**

```csharp
var anniversary = new ScheduleAppointment
{
    Id = 4,
    StartTime = new DateTime(2026, 6, 15, 0, 0, 0),
    EndTime = new DateTime(2026, 6, 15, 23, 59, 59),
    Subject = "Company Anniversary",
    IsAllDay = true,
    RecurrenceRule = "FREQ=YEARLY;BYMONTHDAY=15;BYMONTH=6",
    AppointmentBackground = Brushes.Gold
};
```

### Recurrence Patterns

**Every Weekday (Monday-Friday):**
```
FREQ=WEEKLY;BYDAY=MO,TU,WE,TH,FR;INTERVAL=1
```

**Every Other Week:**
```
FREQ=WEEKLY;INTERVAL=2;COUNT=10
```

**First Monday of Every Month:**
```
FREQ=MONTHLY;BYDAY=MO;BYSETPOS=1
```

**Last Friday of Every Month:**
```
FREQ=MONTHLY;BYDAY=FR;BYSETPOS=-1
```

**Every 3 Months:**
```
FREQ=MONTHLY;INTERVAL=3;COUNT=8
```

### Recurrence Exceptions

Exclude specific dates from a recurring series using `RecurrenceExceptionDates`:

```csharp
var recurringMeeting = new ScheduleAppointment
{
    Id = 1,
    StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
    EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
    Subject = "Daily Meeting",
    RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=10",
    RecurrenceExceptionDates = new ObservableCollection<DateTime>
    {
        new DateTime(2026, 3, 26),  // Skip March 26
        new DateTime(2026, 3, 30)   // Skip March 30
    },
    AppointmentBackground = Brushes.Teal
};
```

**Modifying Single Occurrence:**

To modify a specific occurrence while keeping the pattern:

```csharp
// Create exception appointment
var modifiedOccurrence = new ScheduleAppointment
{
    StartTime = new DateTime(2026, 3, 26, 14, 0, 0), // Different time
    EndTime = new DateTime(2026, 3, 26, 15, 0, 0),
    Subject = "Daily Meeting - Rescheduled",
    RecurrenceId = 1, // Links to pattern appointment ID
    AppointmentBackground = Brushes.Orange
};

// Add exception date to pattern appointment
patternAppointment.RecurrenceExceptionDates.Add(new DateTime(2026, 3, 26));
```

### Recurrence Helper Methods

**Parse RRULE:**

```csharp
DateTime startDate = new DateTime(2026, 3, 24, 10, 0, 0);
RecurrenceProperties props = RecurrenceHelper.RRuleParser("FREQ=DAILY;INTERVAL=1;COUNT=10", startDate);

// Access properties
var frequency = props.RecurrenceType; // RecurrenceType.Daily
var interval = props.Interval; // 1
var count = props.RecurrenceCount; // 10
```

**Get Occurrence Dates:**

```csharp
DateTime startDate = new DateTime(2026, 3, 24, 9, 0, 0);
IEnumerable<DateTime> dates = RecurrenceHelper.GetRecurrenceDateTimeCollection(
    "FREQ=DAILY;INTERVAL=1;COUNT=5", 
    startDate
);

// Returns: 3/24, 3/25, 3/26, 3/27, 3/28
```

**Get Pattern Appointment:**

```csharp
// In event handler
private void Schedule_AppointmentTapped(object sender, AppointmentTappedArgs e)
{
    if (e.Appointment != null)
    {
        var patternAppointment = RecurrenceHelper.GetPatternAppointment(Schedule, e.Appointment);
        // Use pattern appointment
    }
}
```

## Appointment Rendering

**Arrangement Rules:**

**Day/Week/WorkWeek Views:**
- Normal appointments arranged by start time and duration
- All-day panel: Ordered by start time, then IsSpanned, then IsAllDay

**Month View:**
- Depends on `AppointmentDisplayMode` setting
- Ordered by start time within cells

**Timeline Views:**
- All appointments ordered by start time
- Then by duration, IsSpanned, IsAllDay, normal

**Overlapping Appointments:**
- Displayed side-by-side with reduced width
- Automatic collision detection and arrangement

## Important Notes

- Custom appointment classes must contain StartTime and EndTime fields (mandatory)
- Implement `INotifyPropertyChanged` for dynamic updates
- Use unique `Id` values for appointments (required for recurrence and editing)
- RecurrenceRule follows RFC 5545 iCalendar standard
- Exception dates must match occurrence dates exactly (including time)
- Appointments outside visible hours are clipped or hidden

## Next Steps

- **Appointment Editing** - Learn about the built-in appointment editor
- **Appointment Drag-Drop** - Implement drag-and-drop rescheduling
- **Resource Grouping** - Assign appointments to resources
- **Events** - Handle appointment interactions and changes

For interactive appointment management, see [appointment-editing.md](appointment-editing.md).
