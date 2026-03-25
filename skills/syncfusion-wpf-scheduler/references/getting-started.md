# Getting Started with WPF Scheduler (SfScheduler)

## Table of Contents
- [Overview](#overview)
- [Assembly Deployment](#assembly-deployment)
  - [Required Assemblies](#required-assemblies)
  - [NuGet Package Installation](#nuget-package-installation)
- [Creating Simple Application](#creating-simple-application)
  - [Creating Project](#creating-project)
  - [Adding Control via Designer](#adding-control-via-designer)
  - [Adding Control Manually in XAML](#adding-control-manually-in-xaml)
  - [Adding Control Manually in C#](#adding-control-manually-in-c)
- [Basic Configuration](#basic-configuration)
  - [Changing Scheduler Views](#changing-scheduler-views)
  - [First Day of Week](#first-day-of-week)
  - [Busy Indicator](#busy-indicator)
- [Basic Appointments](#basic-appointments)
  - [Creating Schedule Appointments](#creating-schedule-appointments)
  - [Custom Events with Data Mapping](#custom-events-with-data-mapping)
- [Theme Support](#theme-support)

## Overview

The WPF Scheduler (SfScheduler) control is used to schedule and manage appointments through an intuitive user interface, similar to the Outlook calendar. It provides eight different types of views (Day, WorkWeek, Week, TimelineDay, TimelineWeek, TimelineWorkWeek, TimelineMonth, and Month) to display dates and appointments.

This guide provides a walkthrough to configure the SfScheduler control in a real-time scenario, from installation to creating your first working scheduler.

## Assembly Deployment

### Required Assemblies

The SfScheduler control depends on the following assembly:

**Assembly:** `Syncfusion.SfScheduler.WPF`

**Namespace:** `Syncfusion.UI.Xaml.Scheduler`

### NuGet Package Installation

The easiest way to add SfScheduler to your WPF application is through NuGet Package Manager:

1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.SfScheduler.WPF"
4. Install the package

**Package Name:** `Syncfusion.SfScheduler.WPF`

Alternatively, use the Package Manager Console:

```powershell
Install-Package Syncfusion.SfScheduler.WPF
```

**Useful Links:**
- [Control Dependencies Documentation](https://help.syncfusion.com/wpf/control-dependencies#sfscheduler)
- [NuGet Package Installation Guide](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages)
- [Syncfusion Reference Manager](https://help.syncfusion.com/wpf/visual-studio-integration/visual-studio-extensions/add-references)

## Creating Simple Application

### Creating Project

1. Open Visual Studio
2. Create a new **WPF Application (.NET Framework)** or **WPF Application (.NET Core/.NET 5+)** project
3. Name your project (e.g., "SchedulerDemo")
4. Add the required assembly reference as described above

### Adding Control via Designer

The SfScheduler control can be added to your application by dragging it from the Toolbox:

1. Build the project after installing the NuGet package
2. Open MainWindow.xaml in Design view
3. Locate "SfScheduler" in the Toolbox (under Syncfusion controls)
4. Drag and drop it onto the design surface

The required assembly references and namespaces will be added automatically.

### Adding Control Manually in XAML

To add the control manually in XAML:

1. Add the `Syncfusion.SfScheduler.WPF` assembly reference to your project
2. Import the WPF schema in your XAML page
3. Declare the `SfScheduler` control

**Example:**

```xaml
<Window
    x:Class="SchedulerDemo.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    Title="Scheduler Demo" 
    WindowStartupLocation="CenterScreen"
    Height="600" Width="900">
    <Grid>
        <syncfusion:SfScheduler x:Name="Schedule" ViewType="Month"/>
    </Grid>
</Window>
```

### Adding Control Manually in C#

To add the control programmatically in code-behind:

```csharp
using Syncfusion.UI.Xaml.Scheduler;
using System.Windows;

namespace SchedulerDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create scheduler instance
            SfScheduler schedule = new SfScheduler();
            schedule.ViewType = SchedulerViewType.Month;
            
            // Add to window
            this.Content = schedule;
        }
    }
}
```

## Basic Configuration

### Changing Scheduler Views

The SfScheduler control provides eight different view types. The view can be assigned using the `ViewType` property. By default, the control displays the `Month` view.

**Available Views:**
- `Day` - Single day view with time slots
- `Week` - 7-day week view
- `WorkWeek` - 5-day workweek view (Monday-Friday)
- `Month` - Calendar month view
- `TimelineDay` - Horizontal timeline for a single day
- `TimelineWeek` - Horizontal timeline for a week
- `TimelineWorkWeek` - Horizontal timeline for workweek
- `TimelineMonth` - Horizontal timeline for a month

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Week" />
```

**C#:**

```csharp
Schedule.ViewType = SchedulerViewType.Week;
```

The current date will be displayed initially for all scheduler views.

### First Day of Week

The scheduler will render with Sunday as the first day of the week by default. You can customize this using the `FirstDayOfWeek` property.

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" 
                        ViewType="Week"
                        FirstDayOfWeek="Monday" />
```

**C#:**

```csharp
Schedule.FirstDayOfWeek = DayOfWeek.Monday;
```

### Busy Indicator

The scheduler supports displaying a busy indicator while loading appointments or navigating between dates. Set the `ShowBusyIndicator` property to `true` to enable it.

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule"
                        ViewType="Month"
                        ShowBusyIndicator="True" />
```

**C#:**

```csharp
Schedule.ShowBusyIndicator = true;
```

The busy indicator is displayed during view changes or when the visible date is changed. The default value is `false`.

## Basic Appointments

### Creating Schedule Appointments

The SfScheduler has built-in capability to handle appointment arrangement internally based on the `ScheduleAppointmentCollection`. The `ScheduleAppointment` class includes basic properties such as:

- `StartTime` - Start date and time of the appointment
- `EndTime` - End date and time of the appointment
- `Subject` - Title or subject of the appointment
- `Notes` - Additional notes or description
- `Location` - Location of the appointment
- `IsAllDay` - Indicates if the appointment spans the entire day
- `AppointmentBackground` - Background color of the appointment
- `Foreground` - Text color of the appointment

**Creating Appointments in XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Week"/>
```

**Creating Appointments in C#:**

```csharp
using Syncfusion.UI.Xaml.Scheduler;
using System;

public MainWindow()
{
    InitializeComponent();
    
    // Create appointment collection
    ScheduleAppointmentCollection appointmentCollection = new ScheduleAppointmentCollection();
    
    // Create appointment
    ScheduleAppointment clientMeeting = new ScheduleAppointment();
    DateTime currentDate = DateTime.Now;
    DateTime startTime = new DateTime(currentDate.Year, currentDate.Month, currentDate.Day, 10, 0, 0);
    DateTime endTime = new DateTime(currentDate.Year, currentDate.Month, currentDate.Day, 12, 0, 0);
    
    clientMeeting.StartTime = startTime;
    clientMeeting.EndTime = endTime;
    clientMeeting.Subject = "Client Meeting";
    clientMeeting.Location = "Conference Room A";
    clientMeeting.Notes = "Discuss project requirements";
    clientMeeting.AppointmentBackground = new SolidColorBrush(Colors.LightBlue);
    
    appointmentCollection.Add(clientMeeting);
    
    // Assign to scheduler
    Schedule.ItemsSource = appointmentCollection;
}
```

### Custom Events with Data Mapping

Map custom appointments data to the scheduler using the `AppointmentMapping` property. This allows you to use your own business objects instead of `ScheduleAppointment`.

**Step 1: Create a Custom Data Model**

```csharp
using System;
using System.ComponentModel;
using System.Windows.Media;

public class Meeting : INotifyPropertyChanged
{
    private DateTime from, to;
    private string eventName;
    private bool isAllDay;
    private string startTimeZone, endTimeZone;
    private Brush color;

    public DateTime From
    {
        get { return from; }
        set { from = value; RaisePropertyChanged("From"); }
    }

    public DateTime To
    {
        get { return to; }
        set { to = value; RaisePropertyChanged("To"); }
    }

    public string EventName
    {
        get { return eventName; }
        set { eventName = value; RaisePropertyChanged("EventName"); }
    }

    public bool IsAllDay
    {
        get { return isAllDay; }
        set { isAllDay = value; RaisePropertyChanged("IsAllDay"); }
    }

    public string StartTimeZone
    {
        get { return startTimeZone; }
        set { startTimeZone = value; RaisePropertyChanged("StartTimeZone"); }
    }

    public string EndTimeZone
    {
        get { return endTimeZone; }
        set { endTimeZone = value; RaisePropertyChanged("EndTimeZone"); }
    }

    public Brush Color
    {
        get { return color; }
        set { color = value; RaisePropertyChanged("Color"); }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Step 2: Create ViewModel**

```csharp
using System.Collections.ObjectModel;

public class SchedulerViewModel
{
    public ObservableCollection<Meeting> Events { get; set; }

    public SchedulerViewModel()
    {
        Events = new ObservableCollection<Meeting>();
        Events.Add(new Meeting
        {
            EventName = "Team Meeting",
            From = DateTime.Now.Date.AddHours(9),
            To = DateTime.Now.Date.AddHours(10),
            Color = new SolidColorBrush((Color)ColorConverter.ConvertFromString("#FF339933")),
            IsAllDay = false
        });
    }
}
```

**Step 3: Bind to SfScheduler**

```xaml
<Window.DataContext>
    <local:SchedulerViewModel/>
</Window.DataContext>

<syncfusion:SfScheduler x:Name="Schedule"
                        ViewType="Week"
                        ItemsSource="{Binding Events}">
    <syncfusion:SfScheduler.AppointmentMapping>
        <syncfusion:AppointmentMapping
            Subject="EventName"
            StartTime="From"
            EndTime="To"
            AppointmentBackground="Color"
            IsAllDay="IsAllDay"
            StartTimeZone="StartTimeZone"
            EndTimeZone="EndTimeZone"/>
    </syncfusion:SfScheduler.AppointmentMapping>
</syncfusion:SfScheduler>
```

**Or in C#:**

```csharp
AppointmentMapping appointmentMapping = new AppointmentMapping();
appointmentMapping.Subject = "EventName";
appointmentMapping.StartTime = "From";
appointmentMapping.EndTime = "To";
appointmentMapping.AppointmentBackground = "Color";
appointmentMapping.IsAllDay = "IsAllDay";
appointmentMapping.StartTimeZone = "StartTimeZone";
appointmentMapping.EndTimeZone = "EndTimeZone";

Schedule.AppointmentMapping = appointmentMapping;

SchedulerViewModel viewModel = new SchedulerViewModel();
Schedule.ItemsSource = viewModel.Events;
```

**Important:** The custom appointment class must contain event start and end date time fields as mandatory requirements.

## Theme Support

WPF Scheduler supports various built-in themes for consistent styling:

**Using SfSkinManager:**

```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    SfSkinManager.SetTheme(this, new Theme("MaterialDark"));
}
```

## Next Steps

Now that you have a basic scheduler set up with appointments, explore these topics:

- **Views** - Learn about different view types and customization options
- **Appointments** - Deep dive into appointment types, recurrence, and advanced features
- **Resource Grouping** - Implement multi-resource scheduling
- **UI Customization** - Customize headers, templates, and appearance
- **Advanced Features** - Accessibility, localization, load-on-demand, and more

Refer to the main SKILL.md navigation guide to explore specific features based on your requirements.
