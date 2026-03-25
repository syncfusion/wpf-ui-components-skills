# Resource Grouping, Timezones, and Advanced Features

## Table of Contents
- [Resource Grouping](#resource-grouping)
  - [Overview](#overview)
  - [Creating Resources](#creating-resources)
  - [Resource Grouping Types](#resource-grouping-types)
  - [Assigning Resources to Appointments](#assigning-resources-to-appointments)
- [Timezone Support](#timezone-support)
  - [Scheduler Timezone](#scheduler-timezone)
  - [Appointment Timezones](#appointment-timezones)
  - [Timezone Conversion](#timezone-conversion)
- [UI Features](#ui-features)
  - [Header Customization](#header-customization)
  - [Context Menu](#context-menu)
- [Advanced Features](#advanced-features)
  - [Accessibility](#accessibility)
  - [Localization](#localization)
  - [Load On Demand](#load-on-demand)
  - [Reminders](#reminders)

## Resource Grouping

### Overview

The WPF Scheduler supports resource-based scheduling, allowing you to group appointments by resources (people, rooms, equipment) or dates. This is ideal for scenarios like employee scheduling, room booking, or equipment management.

### Creating Resources

Use the `SchedulerResource` class to define resources:

```csharp
// Create resources
var resources = new ObservableCollection<SchedulerResource>
{
    new SchedulerResource 
    { 
        Id = "1001", 
        Name = "Dr. Smith", 
        Background = Brushes.Blue,
        Foreground = Brushes.White
    },
    new SchedulerResource 
    { 
        Id = "1002", 
        Name = "Dr. Jones", 
        Background = Brushes.Green,
        Foreground = Brushes.White
    },
    new SchedulerResource 
    { 
        Id = "1003", 
        Name = "Dr. Brown", 
        Background = Brushes.Orange,
        Foreground = Brushes.White
    }
};

// Assign to scheduler
Schedule.ResourceCollection = resources;
Schedule.ResourceGroupType = ResourceGroupType.Resource;
```

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule"
                        ViewType="TimelineWeek"
                        ResourceGroupType="Resource"
                        ResourceCollection="{Binding ResourceCollection}" />
```

### Resource Grouping Types

**1. ResourceGroupType.Resource** - Group dates under each resource:

```csharp
Schedule.ResourceGroupType = ResourceGroupType.Resource;
```
Shows: Resource 1 (all dates) | Resource 2 (all dates) | Resource 3 (all dates)

**2. ResourceGroupType.Date** - Group resources under each date:

```csharp
Schedule.ResourceGroupType = ResourceGroupType.Date;
```
Shows: Date 1 (all resources) | Date 2 (all resources) | Date 3 (all resources)

**3. ResourceGroupType.None** - No resource grouping (default):

```csharp
Schedule.ResourceGroupType = ResourceGroupType.None;
```

### Assigning Resources to Appointments

Assign appointments to resources using `ResourceIdCollection`:

```csharp
var appointment = new ScheduleAppointment
{
    StartTime = DateTime.Now.AddHours(2),
    EndTime = DateTime.Now.AddHours(3),
    Subject = "Patient Consultation",
    ResourceIdCollection = new ObservableCollection<object> { "1001" }, // Assign to Dr. Smith
    AppointmentBackground = Brushes.LightBlue
};
```

**Multiple Resources:**

```csharp
var meeting = new ScheduleAppointment
{
    StartTime = DateTime.Now.AddHours(4),
    EndTime = DateTime.Now.AddHours(5),
    Subject = "Team Meeting",
    ResourceIdCollection = new ObservableCollection<object> { "1001", "1002", "1003" },
    AppointmentBackground = Brushes.Purple
};
```

**With Custom Objects:**

```csharp
// In AppointmentMapping
mapping.ResourceIdCollection = "ResourceIds";

// In custom class
public class Meeting
{
    public ObservableCollection<object> ResourceIds { get; set; }
}

// Usage
var meeting = new Meeting
{
    EventName = "Conference",
    From = DateTime.Now,
    To = DateTime.Now.AddHours(2),
    ResourceIds = new ObservableCollection<object> { "1001", "1002" }
};
```

## Timezone Support

### Scheduler Timezone

Set the scheduler's timezone to control how appointments are displayed:

```csharp
// Display all appointments in Pacific Time
Schedule.TimeZone = "Pacific Standard Time";
```

**Default:** Uses system's local timezone.

### Appointment Timezones

Appointments can have different timezones for start and end:

```csharp
var globalMeeting = new ScheduleAppointment
{
    StartTime = new DateTime(2026, 3, 25, 10, 0, 0),
    EndTime = new DateTime(2026, 3, 25, 11, 0, 0),
    Subject = "Global Team Meeting",
    StartTimeZone = "Pacific Standard Time",
    EndTimeZone = "Pacific Standard Time",
    AppointmentBackground = Brushes.Teal
};
```

The scheduler automatically converts and displays appointments in the scheduler's timezone.

### Timezone Conversion

**Common Timezone IDs:**
- "Pacific Standard Time"
- "Eastern Standard Time"
- "Central Standard Time"
- "Mountain Standard Time"
- "UTC"
- "GMT Standard Time"

**Automatic Features:**
- Daylight Saving Time adjustments
- Cross-timezone meeting display
- Automatic conversion to scheduler timezone

## UI Features

### Header Customization

**Header Height:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule"
                        ViewType="Month"
                        HeaderHeight="100" />
```

```csharp
Schedule.HeaderHeight = 100; // Default: 50
```

**Header Date Format:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule"
                        HeaderDateFormat="MMM/yyyy" />
```

```csharp
Schedule.HeaderDateFormat = "MMM/yyyy"; // Default: "MMMM yyyy"
```

**Header Template:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule">
    <syncfusion:SfScheduler.HeaderTemplate>
        <DataTemplate>
            <TextBlock FontStyle="Italic"
                       Foreground="Blue"
                       FontSize="25"
                       Text="{Binding}" />
        </DataTemplate>
    </syncfusion:SfScheduler.HeaderTemplate>
</syncfusion:SfScheduler>
```

### Context Menu

The scheduler provides built-in context menus for appointments and time slots. Context menus are automatically shown on right-click.

**Features:**
- Create new appointments
- Edit existing appointments
- Delete appointments
- Copy/paste appointments
- Customizable menu items

## Advanced Features

### Accessibility

The scheduler is WCAG compliant with full accessibility support:

**Keyboard Navigation:**
- Arrow keys: Navigate between time slots/cells
- Tab: Move between UI elements
- Enter: Open appointment editor
- Delete: Remove selected appointment
- Ctrl+C/V: Copy/paste appointments

**Screen Reader Support:**
- All UI elements have appropriate ARIA labels
- Appointment details are announced
- Navigation context provided

**Focus Management:**
- Clear focus indicators
- Keyboard-accessible all controls
- Logical tab order

### Localization

Localize the scheduler for different cultures:

**Step 1: Set Culture**

```csharp
public MainWindow()
{
    InitializeComponent();
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("fr-FR");
}
```

**Step 2: Add Resource Files**

1. Create `Resources` folder in project
2. Add resource file: `Syncfusion.SfScheduler.WPF.<culture>.resx`
   - Example: `Syncfusion.SfScheduler.WPF.fr-FR.resx`
3. Add localized name/value pairs

**Localized Elements:**
- Date/time formats
- Button labels
- Menu items
- Validation messages
- Navigation text

### Load On Demand

For large datasets spanning multiple years, use load-on-demand to improve performance:

```csharp
Schedule.LoadOnDemand = true;
```

**Benefits:**
- Faster initial load
- Reduced memory usage
- Loading indicator displayed during data fetch
- Appointments loaded as user navigates

**Implementation:**
The scheduler only loads appointments for the visible date range. As the user scrolls or navigates, additional appointments are loaded automatically.

### Reminders

Add reminders to appointments to notify users:

**Feature:**
- Set reminder time before appointment
- Notification displays at specified time
- Multiple reminders per appointment
- Snooze and dismiss options

**Usage:**
Reminders help users stay organized and ensure important appointments aren't missed.

## Common Scenarios

### Scenario 1: Doctor Scheduling System

```csharp
// Setup resources (doctors)
Schedule.ResourceCollection = new ObservableCollection<SchedulerResource>
{
    new SchedulerResource { Id = "DOC1", Name = "Dr. Smith", Background = Brushes.Blue },
    new SchedulerResource { Id = "DOC2", Name = "Dr. Jones", Background = Brushes.Green }
};
Schedule.ResourceGroupType = ResourceGroupType.Resource;
Schedule.ViewType = SchedulerViewType.TimelineDay;

// Patient appointment
var appointment = new ScheduleAppointment
{
    StartTime = DateTime.Today.AddHours(10),
    EndTime = DateTime.Today.AddHours(10.5),
    Subject = "Patient: John Doe",
    ResourceIdCollection = new ObservableCollection<object> { "DOC1" }
};
```

### Scenario 2: Meeting Room Booking

```csharp
// Setup resources (rooms)
Schedule.ResourceCollection = new ObservableCollection<SchedulerResource>
{
    new SchedulerResource { Id = "RM1", Name = "Conference Room A", Background = Brushes.Purple },
    new SchedulerResource { Id = "RM2", Name = "Conference Room B", Background = Brushes.Teal },
    new SchedulerResource { Id = "RM3", Name = "Training Room", Background = Brushes.Orange }
};
Schedule.ResourceGroupType = ResourceGroupType.Date;
Schedule.ViewType = SchedulerViewType.TimelineWeek;
```

### Scenario 3: Global Team with Timezones

```csharp
// Set scheduler to show times in user's timezone
Schedule.TimeZone = TimeZoneInfo.Local.Id;

// Meeting scheduled in different timezone
var meeting = new ScheduleAppointment
{
    StartTime = new DateTime(2026, 3, 25, 14, 0, 0), // 2 PM PST
    EndTime = new DateTime(2026, 3, 25, 15, 0, 0),
    Subject = "Global Standup",
    StartTimeZone = "Pacific Standard Time",
    EndTimeZone = "Pacific Standard Time"
    // Will display in user's local timezone automatically
};
```

## Next Steps

- **Events** - Handle user interactions and appointment changes
- **Appointment Editing** - Customize the appointment editor
- **Drag-Drop** - Implement appointment rescheduling

For event handling, see the main SKILL.md navigation guide.
