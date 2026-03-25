---
name: syncfusion-wpf-scheduler
description: Implement Syncfusion WPF Scheduler (SfScheduler) for managing appointments and calendar views in desktop applications. Use this when building scheduling interfaces, appointment management systems, or resource booking applications. This skill covers calendar views, appointment handling, resource scheduling, timeline customization, and Outlook-style calendar functionality.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# WPF Scheduler (SfScheduler) Implementation

The WPF Scheduler (SfScheduler) is a comprehensive scheduling control for managing appointments through an intuitive user interface similar to Outlook calendar. It provides eight different view types, appointment management, resource grouping, drag-and-drop, recurrence patterns, timezone support, and extensive customization options.

## When to Use This Skill

Use this skill when the user needs to:
- Implement calendar or scheduler functionality in WPF applications
- Create appointment management systems
- Build resource scheduling applications
- Implement meeting or event management interfaces
- Add Outlook-like calendar views to applications
- Schedule and manage recurring appointments
- Handle multi-resource scheduling scenarios
- Display appointments across different timezones
- Implement drag-and-drop appointment rescheduling
- Create custom appointment editors or views

## Component Overview

**Key Capabilities:**
- **8 Built-in Views:** Day, Week, WorkWeek, Month, TimelineDay, TimelineWeek, TimelineWorkWeek, TimelineMonth
- **Appointment Management:** Normal, all-day, spanned, and recurring appointments with full CRUD operations
- **Resource Scheduling:** Multi-resource support with grouping by resource or date
- **Drag-and-Drop:** Intuitive appointment rescheduling with visual feedback
- **Recurrence Patterns:** Daily, weekly, monthly, yearly with exception handling
- **Timezone Support:** Multiple timezone handling with automatic DST adjustments
- **Built-in Editor:** Appointment creation and editing with validation
- **Customization:** Extensive styling, templates, and appearance options
- **Accessibility:** WCAG compliance with full keyboard navigation
- **Advanced Features:** Load-on-demand, reminders, localization

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet packages
- Creating basic scheduler application (Designer, XAML, C#)
- Basic appointment setup and configuration
- First day of week and theme support
- Initial setup and quick start examples

### Views and Display
📄 **Read:** [references/scheduler-views.md](references/scheduler-views.md)
- Available view types (Day, Week, Month, Timeline variants)
- ViewType property and switching between views
- View-specific features and rendering
- Current date display

📄 **Read:** [references/view-customization.md](references/view-customization.md)
- Time interval and slot height customization
- Working days and hours configuration
- Special time regions (lunch breaks, holidays)
- Selection restrictions in timeslots

### Appointments and Data
📄 **Read:** [references/appointments.md](references/appointments.md)
- ScheduleAppointment class and properties
- Creating and managing appointments
- ItemSource and AppointmentMapping
- Custom business objects and data binding
- All-day and spanned appointments
- Recurrence appointments and patterns (RRULE)
- Recurrence exceptions

📄 **Read:** [references/appointment-editing.md](references/appointment-editing.md)
- Built-in appointment editor
- Creating, editing, and deleting appointments via UI
- Editor customization and validation
- Editor events and custom logic

📄 **Read:** [references/appointment-drag-drop.md](references/appointment-drag-drop.md)
- Drag-and-drop functionality
- Rescheduling appointments
- Drag-drop restrictions and events

### Resource Management
📄 **Read:** [references/resource-grouping.md](references/resource-grouping.md)
- Resource concepts and SchedulerResource class
- ResourceGroupType (Resource vs Date grouping)
- Assigning resources to appointments
- Multi-resource scheduling
- Resource appearance customization

### Calendar and Time Management
📄 **Read:** [references/calendar-types.md](references/calendar-types.md)
- Calendar systems (Gregorian, custom)
- Calendar configuration

📄 **Read:** [references/date-navigation.md](references/date-navigation.md)
- DisplayDate property
- Programmatic navigation methods
- Date change events

📄 **Read:** [references/timezones.md](references/timezones.md)
- Scheduler and appointment timezones
- Automatic timezone conversion
- Daylight saving time handling
- Multi-timezone scheduling scenarios

### UI Customization
📄 **Read:** [references/ui-features.md](references/ui-features.md)
- Header customization and templates
- Context menu and commands
- Busy indicator
- Selection modes and events

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Accessibility (WCAG, keyboard navigation)
- Localization and culture-specific formatting
- Load on demand for large datasets
- Reminders and notifications
- Migration from SfSchedule to SfScheduler

## Quick Start Example

### Basic Scheduler with Appointments

**XAML:**
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <syncfusion:SfScheduler x:Name="Schedule" 
                            ViewType="Week"
                            FirstDayOfWeek="Sunday" />
</Window>
```

**C# Code-Behind:**
```csharp
using Syncfusion.UI.Xaml.Scheduler;

public MainWindow()
{
    InitializeComponent();
    
    // Create appointment collection
    var appointments = new ScheduleAppointmentCollection();
    
    // Add sample appointment
    appointments.Add(new ScheduleAppointment
    {
        StartTime = DateTime.Now.Date.AddHours(10),
        EndTime = DateTime.Now.Date.AddHours(12),
        Subject = "Client Meeting",
        Location = "Conference Room A",
        Notes = "Discuss project timeline",
        AppointmentBackground = Brushes.LightBlue
    });
    
    Schedule.ItemsSource = appointments;
}
```

## Common Patterns

### Pattern 1: Custom Appointments with Data Binding

```csharp
// Custom business object
public class Meeting : INotifyPropertyChanged
{
    public string EventName { get; set; }
    public DateTime From { get; set; }
    public DateTime To { get; set; }
    public Brush Color { get; set; }
    public bool IsAllDay { get; set; }
}

// ViewModel
public class SchedulerViewModel
{
    public ObservableCollection<Meeting> Events { get; set; }
    
    public SchedulerViewModel()
    {
        Events = new ObservableCollection<Meeting>
        {
            new Meeting
            {
                EventName = "Team Standup",
                From = DateTime.Now.Date.AddHours(9),
                To = DateTime.Now.Date.AddHours(9.5),
                Color = Brushes.Green,
                IsAllDay = false
            }
        };
    }
}
```

**XAML:**
```xaml
<syncfusion:SfScheduler x:Name="Schedule" 
                        ViewType="Week"
                        ItemsSource="{Binding Events}">
    <syncfusion:SfScheduler.AppointmentMapping>
        <syncfusion:AppointmentMapping
            Subject="EventName"
            StartTime="From"
            EndTime="To"
            AppointmentBackground="Color"
            IsAllDay="IsAllDay" />
    </syncfusion:SfScheduler.AppointmentMapping>
</syncfusion:SfScheduler>
```

### Pattern 2: Recurring Weekly Meeting

```csharp
var recurringMeeting = new ScheduleAppointment
{
    StartTime = new DateTime(2026, 3, 24, 14, 0, 0), // Monday 2PM
    EndTime = new DateTime(2026, 3, 24, 15, 0, 0),
    Subject = "Weekly Status Meeting",
    // Recur every Monday for 12 weeks
    RecurrenceRule = "FREQ=WEEKLY;BYDAY=MO;COUNT=12",
    AppointmentBackground = Brushes.Orange
};
```

### Pattern 3: Resource-Based Scheduling

```csharp
// Add resources
Schedule.ResourceCollection = new ObservableCollection<SchedulerResource>
{
    new SchedulerResource { Id = "1001", Name = "Dr. Smith", Background = Brushes.Blue },
    new SchedulerResource { Id = "1002", Name = "Dr. Jones", Background = Brushes.Green }
};

Schedule.ResourceGroupType = ResourceGroupType.Resource;

// Assign appointment to resource
var appointment = new ScheduleAppointment
{
    StartTime = DateTime.Now.AddHours(2),
    EndTime = DateTime.Now.AddHours(3),
    Subject = "Patient Consultation",
    ResourceIdCollection = new ObservableCollection<object> { "1001" }
};
```

### Pattern 4: Timezone-Aware Appointments

```csharp
var globalMeeting = new ScheduleAppointment
{
    StartTime = new DateTime(2026, 3, 25, 10, 0, 0),
    EndTime = new DateTime(2026, 3, 25, 11, 0, 0),
    Subject = "Global Team Sync",
    StartTimeZone = "Pacific Standard Time",
    EndTimeZone = "Pacific Standard Time"
};

// Scheduler displays in local timezone automatically
Schedule.TimeZone = TimeZoneInfo.Local.Id;
```

## Key Properties Overview

### Core Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **ViewType** | SchedulerViewType | Sets the view (Day, Week, WorkWeek, Month, TimelineDay, TimelineWeek, TimelineWorkWeek, TimelineMonth) | Month |
| **ItemsSource** | IEnumerable | Appointment collection to display | null |
| **DisplayDate** | DateTime | Currently displayed date | DateTime.Now |
| **SelectedDate** | DateTime | Currently selected date | DateTime.Now |
| **FirstDayOfWeek** | DayOfWeek | Starting day of the week | Sunday |
| **TimeZone** | string | Scheduler's timezone identifier | Local |

### View Navigation & Configuration

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **AllowedViewTypes** | SchedulerViewType | Allowed view types for navigation | All |
| **AllowViewNavigation** | bool | Enable/disable view navigation | true |
| **ShowDatePickerButton** | bool | Show date picker button in header | true |

### Date Range Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **MinimumDate** | DateTime | Minimum date that can be displayed | DateTime.MinValue |
| **MaximumDate** | DateTime | Maximum date that can be displayed | DateTime.MaxValue |
| **BlackoutDates** | ObservableCollection\<DateTime\> | Dates where interaction is disabled (TimelineMonth only) | null |

### Data Binding & Mapping

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **AppointmentMapping** | AppointmentMapping | Maps custom objects to ScheduleAppointment properties | null |
| **ResourceMapping** | ResourceMapping | Maps custom objects to SchedulerResource properties | null |

### Header Customization

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **HeaderHeight** | double | Height of the scheduler header | 50 |
| **HeaderDateFormat** | string | Date format string in header | "MMMM yyyy" |
| **HeaderTemplate** | DataTemplate | Custom template for header | null |

### Resource Scheduling

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **ResourceCollection** | ObservableCollection\<SchedulerResource\> | Collection of scheduler resources | null |
| **ResourceGroupType** | ResourceGroupType | Grouping mode (Resource, Date, None) | None |
| **ResourceHeaderTemplate** | DataTemplate | Custom template for resource headers | null |
| **ResourceHeaderTemplateSelector** | DataTemplateSelector | Template selector for resource headers | null |

### View-Specific Settings

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **DaysViewSettings** | DaysViewSettings | Settings for Day/Week/WorkWeek views | Default |
| **MonthViewSettings** | MonthViewSettings | Settings for Month view | Default |
| **TimelineViewSettings** | TimelineViewSettings | Settings for Timeline views | Default |

### Calendar & Culture

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **CalendarIdentifier** | string | Calendar system identifier (Gregorian, etc.) | Gregorian |

### UI & Interaction

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **ShowBusyIndicator** | bool | Shows loading indicator during operations | false |
| **EnableToolTip** | bool | Enable tooltips on appointments | true |
| **ToolTipTemplate** | DataTemplate | Custom template for tooltips | null |
| **EnableReminder** | bool | Enable reminder functionality | false |

### Context Menus

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **AppointmentContextMenu** | ContextMenu | Context menu for appointments | Default |
| **CellContextMenu** | ContextMenu | Context menu for cells | Default |

### Drag & Drop

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **DragDropSettings** | DragDropSettings | Configure drag-drop behavior | Default |

### Appointment Editing

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **AppointmentEditFlag** | AppointmentEditFlag | Controls which edit operations are allowed | All |
| **AppointmentResizeController** | IAppointmentResizeController | Custom resize behavior controller | null |

### Load On Demand

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **LoadOnDemandCommand** | ICommand | Command executed when loading appointments on demand | null |

## Key Methods

| Method | Description |
|--------|-------------|
| **Forward()** | Navigate forward by one view interval |
| **Backward()** | Navigate backward by one view interval |
| **SchedulerDateToPoint(DateTime)** | Convert date to screen coordinates |
| **PointToSchedulerDate(Point)** | Convert screen coordinates to date/time |

## Key Events

| Event | Description |
|-------|-------------|
| **ViewChanged** | Raised when the view type changes |
| **DisplayDateChanged** | Raised when the display date changes |
| **SelectionChanged** | Raised when the selected date/appointment changes |
| **CellTapped** | Raised when a cell is tapped/clicked |
| **CellDoubleTapped** | Raised when a cell is double-clicked |
| **CellLongPressed** | Raised when a cell is long-pressed |
| **AppointmentTapped** | Raised when an appointment is tapped/clicked |
| **AppointmentDoubleTapped** | Raised when an appointment is double-clicked |
| **AppointmentDragStarting** | Raised when appointment drag begins (can be cancelled) |
| **AppointmentDragOver** | Raised while dragging appointment over scheduler |
| **AppointmentDrop** | Raised when appointment is dropped |
| **AppointmentResizeStarting** | Raised when appointment resize begins (can be cancelled) |
| **AppointmentResizing** | Raised while appointment is being resized |
| **AppointmentResizeCompleted** | Raised when appointment resize completes |
| **AppointmentEditorOpening** | Raised when appointment editor is opening (can be cancelled) |
| **AppointmentEditorClosing** | Raised when appointment editor is closing (can be cancelled) |
| **HeaderTapped** | Raised when header is tapped/clicked |

## Common Use Cases

1. **Meeting Room Scheduler** - Manage conference room bookings with resource grouping
2. **Medical Appointments** - Doctor scheduling with patient appointments and recurring slots
3. **Project Timeline** - Track tasks, milestones, and team member assignments
4. **Event Calendar** - Public event management with all-day and multi-day events
5. **Employee Shift Planning** - Resource-based shift scheduling with drag-drop
6. **Class Schedule** - Educational timetables with recurring sessions
7. **Service Booking** - Appointment booking system with availability management

## Important Considerations

- **Performance:** Use LoadOnDemand for large datasets spanning multiple years
- **Data Binding:** Custom objects must implement INotifyPropertyChanged for updates
- **Recurrence:** Use RRULE format for recurrence patterns (standard RFC 5545)
- **Timezones:** Always specify timezones for global applications
- **Resources:** ResourceIdCollection must match ResourceCollection IDs
- **Views:** Choose appropriate view based on use case (Day for detailed, Month for overview)

## Next Steps

Start with [Getting Started](references/getting-started.md) for installation and basic setup, then explore specific features based on your requirements using the navigation guide above.
