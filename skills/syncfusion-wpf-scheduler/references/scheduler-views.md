# Scheduler Views

## Table of Contents
- [Overview](#overview)
- [Available View Types](#available-view-types)
- [Day, Week, and WorkWeek Views](#day-week-and-workweek-views)
- [Month View](#month-view)
  - [Month Agenda View](#month-agenda-view)
  - [Appointment Display Mode](#appointment-display-mode)
  - [Month Navigation Direction](#month-navigation-direction)
- [Timeline Views](#timeline-views)
  - [TimelineDay View](#timelineday-view)
  - [TimelineWeek and TimelineWorkWeek Views](#timelineweek-and-timelineworkweek-views)
  - [TimelineMonth View](#timelinemonth-view)
- [Setting and Switching Views](#setting-and-switching-views)
- [Current Date Display](#current-date-display)

## Overview

The WPF Scheduler (SfScheduler) provides eight different view types to display dates and appointments. Each view is optimized for different scheduling scenarios, from detailed hourly views to monthly overviews. The scheduler supports displaying the past or future dates by scrolling, and each view renders events accurately across time slots.

## Available View Types

| View Type | Description | Best For |
|-----------|-------------|----------|
| **Day** | Single day view with hourly time slots | Detailed daily schedule viewing |
| **Week** | 7-day week view with time slots | Weekly planning and overview |
| **WorkWeek** | 5-day workweek view (Monday-Friday) | Business week scheduling |
| **Month** | Calendar month view | Monthly overview and planning |
| **TimelineDay** | Horizontal timeline for single/multiple days | Resource-based daily scheduling |
| **TimelineWeek** | Horizontal timeline for a week | Resource scheduling across week |
| **TimelineWorkWeek** | Horizontal timeline for workweek | Business week resource scheduling |
| **TimelineMonth** | Horizontal timeline for a month | Long-term resource planning |

## Day, Week, and WorkWeek Views

These views display appointments in vertical time slots. The Day view shows a single day, Week shows 7 days, and WorkWeek shows 5 business days (Monday-Friday).

**Key Features:**
- Vertical time axis with customizable intervals
- Support for all-day appointments section
- Time slot selection and appointment creation
- Drag-and-drop for rescheduling
- Customizable working hours

**Example - Day View:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Day" />
```

**Example - Week View:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Week" />
```

**Example - WorkWeek View:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="WorkWeek" />
```

**C# Code:**

```csharp
// Day View
Schedule.ViewType = SchedulerViewType.Day;

// Week View
Schedule.ViewType = SchedulerViewType.Week;

// WorkWeek View
Schedule.ViewType = SchedulerViewType.WorkWeek;
```

**Appointment Arrangement:**
- Appointments are arranged based on start time and duration
- All-day and spanned appointments appear in a separate section at the top
- Overlapping appointments are displayed side-by-side

## Month View

The Month view displays the days of a specific month in a calendar format. It shows the current month by default initially, with the current date highlighted.

### Month Agenda View

Display a divided agenda view below the month calendar to show appointments for the selected date. Enable this by setting the `ShowAgendaView` property to `true`.

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Month">
    <syncfusion:SfScheduler.MonthViewSettings>
        <syncfusion:MonthViewSettings ShowAgendaView="True" />
    </syncfusion:SfScheduler.MonthViewSettings>
</syncfusion:SfScheduler>
```

**C#:**

```csharp
Schedule.ViewType = SchedulerViewType.Month;
Schedule.MonthViewSettings.ShowAgendaView = true;
```

**Agenda View Behavior:**
- Shows "No Selected Date" until a date is selected
- Shows "No Events" if the selected day has no appointments
- Updates automatically when a different date is clicked

**Customize Agenda View Height:**

```xaml
<syncfusion:MonthViewSettings 
    ShowAgendaView="True"
    AgendaViewHeight="300" />
```

```csharp
Schedule.MonthViewSettings.AgendaViewHeight = 300; // Default is 30% of scheduler height
```

### Appointment Display Mode

Control how appointments are displayed in month cells using the `AppointmentDisplayMode` property:

**Modes:**
1. **Appointment** (default) - Shows appointment subject in the cell
2. **Indicator** - Shows appointments as colored circles
3. **None** - Hides appointments

**Example:**

```xaml
<syncfusion:MonthViewSettings AppointmentDisplayMode="Appointment" />
```

```csharp
Schedule.MonthViewSettings.AppointmentDisplayMode = AppointmentDisplayMode.Indicator;
```

**Appointment Display Count:**

Customize the number of appointments displayed per cell:

```xaml
<syncfusion:MonthViewSettings 
    AppointmentDisplayMode="Appointment"
    AppointmentDisplayCount="4" />
```

```csharp
Schedule.MonthViewSettings.AppointmentDisplayCount = 4; // Default is 3
```

**Note:** When a cell has more appointments than `AppointmentDisplayCount`, a "more" option appears. Clicking it navigates to the Day view for that date.

### Month Navigation Direction

Navigate the month view horizontally or vertically using `MonthNavigationDirection`:

```xaml
<syncfusion:MonthViewSettings MonthNavigationDirection="Vertical" />
```

```csharp
Schedule.MonthViewSettings.MonthNavigationDirection = Orientation.Horizontal; // Default
// Or
Schedule.MonthViewSettings.MonthNavigationDirection = Orientation.Vertical;
```

## Timeline Views

Timeline views display dates in a horizontal time axis, providing an intuitive way to view and manage schedules, especially for resource-based scheduling.

### TimelineDay View

Shows a horizontal timeline for one or more days with customizable day count.

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="TimelineDay">
    <syncfusion:SfScheduler.TimelineViewSettings>
        <syncfusion:TimelineViewSettings DaysCount="2" />
    </syncfusion:SfScheduler.TimelineViewSettings>
</syncfusion:SfScheduler>
```

**C#:**

```csharp
Schedule.ViewType = SchedulerViewType.TimelineDay;
Schedule.TimelineViewSettings.DaysCount = 2; // Default is 1
```

### TimelineWeek and TimelineWorkWeek Views

Display a week (7 days) or workweek (5 days) in horizontal timeline format.

**TimelineWeek:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="TimelineWeek" />
```

**TimelineWorkWeek:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="TimelineWorkWeek" />
```

**C#:**

```csharp
Schedule.ViewType = SchedulerViewType.TimelineWeek;
// Or
Schedule.ViewType = SchedulerViewType.TimelineWorkWeek;
```

### TimelineMonth View

Displays a month in horizontal timeline format, ideal for long-term planning and resource allocation.

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="TimelineMonth" />
```

**C#:**

```csharp
Schedule.ViewType = SchedulerViewType.TimelineMonth;
```

**Blackout Dates (TimelineMonth only):**

Disable interaction for specific dates in TimelineMonth view:

```csharp
Schedule.ViewType = SchedulerViewType.TimelineMonth;
Schedule.BlackoutDates = new ObservableCollection<DateTime>
{
    DateTime.Now.Date.AddDays(1),
    DateTime.Now.Date.AddDays(5),
    DateTime.Now.Date.AddDays(10)
};
```

**Note:** Blackout dates are only applicable to TimelineMonth view, not other timeline views.

## Setting and Switching Views

### Setting Initial View

Set the initial view using the `ViewType` property:

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Week" />
```

**C#:**

```csharp
Schedule.ViewType = SchedulerViewType.Month;
```

### Switching Views Programmatically

Change views at runtime:

```csharp
// Switch to Day view
Schedule.ViewType = SchedulerViewType.Day;

// Switch to Timeline Week
Schedule.ViewType = SchedulerViewType.TimelineWeek;

// Switch to Month with agenda
Schedule.ViewType = SchedulerViewType.Month;
Schedule.MonthViewSettings.ShowAgendaView = true;
```

### Handling View Changes

Subscribe to the `ViewChanged` event to track view changes:

```xaml
<syncfusion:SfScheduler x:Name="Schedule" 
                        ViewType="Week"
                        ViewChanged="Schedule_ViewChanged" />
```

```csharp
private void Schedule_ViewChanged(object sender, ViewChangedEventArgs e)
{
    var oldView = e.OldValue; // Previous view type
    var newView = e.NewValue; // New view type
    
    // Perform actions based on view change
    Debug.WriteLine($"View changed from {oldView} to {newView}");
}
```

## Current Date Display

The scheduler displays the current date initially for all views. The current date is visually distinguished:

**Day/Week/WorkWeek Views:**
- Current date column is highlighted
- Current time indicator line shows the present time

**Month View:**
- Current date cell has a distinct background color
- Different from other dates in the month

**Timeline Views:**
- Current date/time is highlighted in the timeline
- Auto-scrolls to show current time initially

**Customizing Current Date Indicator:**

The current date appearance is controlled by the theme and can be customized through styles. The scheduler automatically updates the current date indicator as time progresses.

## View Selection Guidelines

**Choose Day View when:**
- Need detailed hourly scheduling
- Managing a single person's detailed schedule
- Require precise time slot visibility

**Choose Week/WorkWeek View when:**
- Planning across multiple days
- Need to see weekly patterns
- Balancing appointments across the week

**Choose Month View when:**
- Need monthly overview
- Planning long-term schedules
- Tracking monthly patterns and trends

**Choose Timeline Views when:**
- Managing multiple resources
- Need horizontal time representation
- Scheduling facilities, rooms, or equipment
- Viewing resource availability across time

## Next Steps

- **View Customization** - Learn about time intervals, working hours, and special time regions
- **Appointments** - Understand appointment management across different views
- **Resource Grouping** - Implement multi-resource scheduling in timeline views

For detailed view customization options, see [view-customization.md](view-customization.md).
