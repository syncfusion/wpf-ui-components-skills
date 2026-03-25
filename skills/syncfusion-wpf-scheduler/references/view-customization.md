# View Customization

## Table of Contents
- [Overview](#overview)
- [Time Interval Customization](#time-interval-customization)
- [Time Interval Height/Width](#time-interval-heightwidth)
- [Working Days and Hours](#working-days-and-hours)
- [Special Time Regions](#special-time-regions)
  - [Creating Special Regions](#creating-special-regions)
  - [Selection Restriction](#selection-restriction)
  - [Merging Adjacent Regions](#merging-adjacent-regions)
  - [Appearance Customization](#appearance-customization)

## Overview

The WPF Scheduler provides extensive customization options for views, allowing you to tailor the time slots, working hours, and visual appearance to match your application's requirements. You can customize time intervals, restrict certain time periods, and configure working hours for different business needs.

## Time Interval Customization

Customize the interval of timeslots in Day, Week, WorkWeek, and Timeline views using the `TimeInterval` property.

**For Day/Week/WorkWeek Views:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Week">
    <syncfusion:SfScheduler.DaysViewSettings>
        <syncfusion:DaysViewSettings TimeInterval="0:120:0" />
    </syncfusion:SfScheduler.DaysViewSettings>
</syncfusion:SfScheduler>
```

```csharp
Schedule.ViewType = SchedulerViewType.Week;
Schedule.DaysViewSettings.TimeInterval = new TimeSpan(0, 120, 0); // 2 hours
```

**For Timeline Views:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="TimelineWeek">
    <syncfusion:SfScheduler.TimelineViewSettings>
        <syncfusion:TimelineViewSettings TimeInterval="0:120:0" />
    </syncfusion:SfScheduler.TimelineViewSettings>
</syncfusion:SfScheduler>
```

```csharp
Schedule.ViewType = SchedulerViewType.TimelineWeek;
Schedule.TimelineViewSettings.TimeInterval = new TimeSpan(0, 120, 0);
```

**Important Note:** If you modify the time interval (in minutes), you should change the time labels format to `hh:mm` for proper display:

```csharp
Schedule.DaysViewSettings.TimeInterval = new TimeSpan(0, 90, 0); // 90 minutes
// Update time format accordingly
```

## Time Interval Height/Width

Customize the height (Day/Week views) or width (Timeline views) of time slots.

### Time Interval Height (Day/Week/WorkWeek Views)

Control the visual height of each time slot using `TimeIntervalSize`:

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Week">
    <syncfusion:SfScheduler.DaysViewSettings>
        <syncfusion:DaysViewSettings TimeIntervalSize="120" />
    </syncfusion:SfScheduler.DaysViewSettings>
</syncfusion:SfScheduler>
```

```csharp
Schedule.ViewType = SchedulerViewType.Week;
Schedule.DaysViewSettings.TimeIntervalSize = 120; // Pixels
```

### Time Interval Width (Timeline Views)

Control the width of time slots in timeline views:

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="TimelineWeek">
    <syncfusion:SfScheduler.TimelineViewSettings>
        <syncfusion:TimelineViewSettings TimeIntervalSize="120" />
    </syncfusion:SfScheduler.TimelineViewSettings>
</syncfusion:SfScheduler>
```

```csharp
Schedule.ViewType = SchedulerViewType.TimelineWeek;
Schedule.TimelineViewSettings.TimeIntervalSize = 120; // Default: 50 for timeline day/week, 150 for month
```

**Default Values:**
- Day/Week/WorkWeek: Not specified (auto-sized)
- TimelineDay/Week/WorkWeek: 50 pixels
- TimelineMonth: 150 pixels

## Working Days and Hours

### Flexible Working Hours

Configure the visible time range in scheduler views using `StartHour` and `EndHour` properties. By default, the scheduler shows all 24 hours (StartHour = 0, EndHour = 24).

**For Day/Week/WorkWeek Views:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Week">
    <syncfusion:SfScheduler.DaysViewSettings>
        <syncfusion:DaysViewSettings 
            StartHour="8"
            EndHour="18" />
    </syncfusion:SfScheduler.DaysViewSettings>
</syncfusion:SfScheduler>
```

```csharp
Schedule.ViewType = SchedulerViewType.Week;
Schedule.DaysViewSettings.StartHour = 8;  // 8:00 AM
Schedule.DaysViewSettings.EndHour = 18;   // 6:00 PM
```

**For Timeline Views:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="TimelineWeek">
    <syncfusion:SfScheduler.TimelineViewSettings>
        <syncfusion:TimelineViewSettings 
            StartHour="8"
            EndHour="15" />
    </syncfusion:SfScheduler.TimelineViewSettings>
</syncfusion:SfScheduler>
```

```csharp
Schedule.ViewType = SchedulerViewType.TimelineWeek;
Schedule.TimelineViewSettings.StartHour = 8;
Schedule.TimelineViewSettings.EndHour = 15;
```

**Important Notes:**
- StartHour and EndHour are not applicable to TimelineMonth view
- Appointments outside StartHour/EndHour range won't be visible (or will be clipped if partially visible)
- You don't need decimal points if not setting minutes (e.g., use 8 for 8:00)
- Number of time slots = ((EndHour - StartHour) * 60) / TimeInterval

### Non-Working Days

Configure non-working days for WorkWeek and TimelineWorkWeek views using the `NonWorkingDays` property:

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="WorkWeek">
    <syncfusion:SfScheduler.DaysViewSettings>
        <syncfusion:DaysViewSettings>
            <syncfusion:DaysViewSettings.NonWorkingDays>
                <system:DayOfWeek>Sunday</system:DayOfWeek>
                <system:DayOfWeek>Saturday</system:DayOfWeek>
            </syncfusion:DaysViewSettings.NonWorkingDays>
        </syncfusion:DaysViewSettings>
    </syncfusion:SfScheduler.DaysViewSettings>
</syncfusion:SfScheduler>
```

```csharp
Schedule.ViewType = SchedulerViewType.WorkWeek;
Schedule.DaysViewSettings.NonWorkingDays = new ObservableCollection<DayOfWeek>
{
    DayOfWeek.Saturday,
    DayOfWeek.Sunday
};
```

**Note:** The `NonWorkingDays` property is only applicable to WorkWeek and TimelineWorkWeek views.

### Time Slot Calculations

When customizing time intervals and working hours, the scheduler calculates time slots automatically:

**Rules:**
1. Total slots = (Total minutes in range) / (TimeInterval in minutes)
2. Result must be an integer; if not, the scheduler adjusts the TimeInterval
3. For custom intervals, ensure (total minutes % TimeInterval) = 0

**Example:**
- If TimeInterval = 2:15 (135 minutes) and total = 1440 minutes (24 hours)
- 1440 % 135 ≠ 0, so scheduler adjusts to nearest valid interval
- Next valid: 144 minutes (1440 % 144 = 0)

## Special Time Regions

Special time regions allow you to highlight and restrict specific time periods (like lunch breaks, holidays, or blocked times) in Day, Week, WorkWeek, and Timeline views.

### Creating Special Regions

Create special time regions using the `SpecialTimeRegions` property:

**XAML:**

```xaml
<syncfusion:SfScheduler x:Name="Schedule" ViewType="Week">
    <syncfusion:SfScheduler.DaysViewSettings>
        <syncfusion:DaysViewSettings>
            <syncfusion:DaysViewSettings.SpecialTimeRegions>
                <syncfusion:SpecialTimeRegion
                    StartTime="2026/03/24 13:0:0"
                    EndTime="2026/03/24 14:0:0"
                    Text="Lunch Break"
                    Background="LightGray"
                    Foreground="Black"
                    CanEdit="False" />
                    
                <syncfusion:SpecialTimeRegion
                    StartTime="2026/03/24 17:0:0"
                    EndTime="2026/03/24 17:30:0"
                    Text="Team Meeting"
                    Background="LightBlue"
                    Foreground="Navy" />
            </syncfusion:DaysViewSettings.SpecialTimeRegions>
        </syncfusion:DaysViewSettings>
    </syncfusion:SfScheduler.DaysViewSettings>
</syncfusion:SfScheduler>
```

**C#:**

```csharp
Schedule.DaysViewSettings.SpecialTimeRegions.Add(new SpecialTimeRegion
{
    StartTime = new DateTime(2026, 3, 24, 13, 0, 0),
    EndTime = new DateTime(2026, 3, 24, 14, 0, 0),
    Text = "Lunch Break",
    CanEdit = false,
    Background = Brushes.LightGray,
    Foreground = Brushes.Black
});
```

**Properties:**
- `StartTime` - Start date and time of the region
- `EndTime` - End date and time of the region
- `Text` - Text to display in the region
- `Background` - Background color
- `Foreground` - Text color
- `CanEdit` - Enable/disable user interaction
- `TimeZone` - Specific timezone for the region
- `CanMergeAdjacentRegions` - Merge continuous regions in Week view

### Selection Restriction

Restrict user interaction in special time regions using the `CanEdit` property:

```csharp
var lunchBreak = new SpecialTimeRegion
{
    StartTime = new DateTime(2026, 3, 24, 12, 0, 0),
    EndTime = new DateTime(2026, 3, 24, 13, 0, 0),
    Text = "Lunch - No Scheduling",
    CanEdit = false, // Disable selection and appointment creation
    Background = Brushes.Gray,
    Foreground = Brushes.White
};

Schedule.DaysViewSettings.SpecialTimeRegions.Add(lunchBreak);
```

**When CanEdit = false:**
- Users cannot select the time region
- Appointments cannot be created in this time slot
- Existing appointments can overlap but interaction is limited
- Visual indicator that the time is restricted

**When CanEdit = true (default):**
- Users can select and interact normally
- Appointments can be created
- Region serves as a visual highlight only

### Merging Adjacent Regions

In Week and WorkWeek views, merge adjacent special time regions that span multiple days into a single continuous region:

```xaml
<syncfusion:SpecialTimeRegion
    StartTime="2026/03/24 12:0:0"
    EndTime="2026/03/28 13:0:0"
    Text="Conference Week - Limited Availability"
    CanMergeAdjacentRegions="True"
    Background="LightYellow"
    Foreground="DarkOrange" />
```

```csharp
var conferenceWeek = new SpecialTimeRegion
{
    StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
    EndTime = new DateTime(2026, 3, 28, 17, 0, 0),
    Text = "Annual Conference",
    CanMergeAdjacentRegions = true, // Merge across days
    Background = Brushes.LightYellow,
    Foreground = Brushes.DarkOrange
};

Schedule.DaysViewSettings.SpecialTimeRegions.Add(conferenceWeek);
```

**Default:** `CanMergeAdjacentRegions = false` (shows separate regions for each day)

### Appearance Customization

Customize the visual appearance of special time regions:

**With Icon:**

```csharp
var holidayRegion = new SpecialTimeRegion
{
    StartTime = new DateTime(2026, 12, 25, 0, 0, 0),
    EndTime = new DateTime(2026, 12, 25, 23, 59, 0),
    Text = "Christmas Holiday",
    // Icon = new BitmapImage(new Uri("pack://application:,,,/Images/holiday.png")),
    Background = new SolidColorBrush(Color.FromRgb(255, 240, 240)),
    Foreground = Brushes.DarkRed,
    CanEdit = false
};
```

**Gradient Background:**

```csharp
var gradientRegion = new SpecialTimeRegion
{
    StartTime = new DateTime(2026, 3, 24, 8, 0, 0),
    EndTime = new DateTime(2026, 3, 24, 9, 0, 0),
    Text = "Morning Setup",
    Background = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 1),
        GradientStops = new GradientStopCollection
        {
            new GradientStop(Colors.LightBlue, 0.0),
            new GradientStop(Colors.White, 1.0)
        }
    },
    Foreground = Brushes.Navy
};
```

## Common Use Cases

### Use Case 1: Office Hours with Lunch Break

```csharp
// Set office hours (9 AM - 6 PM)
Schedule.DaysViewSettings.StartHour = 9;
Schedule.DaysViewSettings.EndHour = 18;

// Add lunch break (12 PM - 1 PM, no scheduling)
Schedule.DaysViewSettings.SpecialTimeRegions.Add(new SpecialTimeRegion
{
    StartTime = new DateTime(2026, 3, 24, 12, 0, 0),
    EndTime = new DateTime(2026, 3, 24, 13, 0, 0),
    Text = "Lunch Break",
    CanEdit = false,
    Background = Brushes.LightGray,
    Foreground = Brushes.Black
});
```

### Use Case 2: Recurring Weekly Meeting Block

```csharp
// Block every Monday 2-3 PM for team meeting
for (int i = 0; i < 12; i++) // Next 12 weeks
{
    DateTime monday = DateTime.Now.Date.AddDays((7 - (int)DateTime.Now.DayOfWeek + (int)DayOfWeek.Monday) + (i * 7));
    
    Schedule.DaysViewSettings.SpecialTimeRegions.Add(new SpecialTimeRegion
    {
        StartTime = monday.AddHours(14),
        EndTime = monday.AddHours(15),
        Text = "Team Meeting",
        CanEdit = false,
        Background = Brushes.LightBlue
    });
}
```

### Use Case 3: 24/7 Operation with Shift Highlights

```csharp
// Show full day
Schedule.TimelineViewSettings.StartHour = 0;
Schedule.TimelineViewSettings.EndHour = 24;

// Highlight shift times
Schedule.TimelineViewSettings.SpecialTimeRegions.Add(new SpecialTimeRegion
{
    StartTime = DateTime.Today.AddHours(6),
    EndTime = DateTime.Today.AddHours(14),
    Text = "Morning Shift",
    Background = new SolidColorBrush(Color.FromArgb(50, 173, 216, 230))
});

Schedule.TimelineViewSettings.SpecialTimeRegions.Add(new SpecialTimeRegion
{
    StartTime = DateTime.Today.AddHours(14),
    EndTime = DateTime.Today.AddHours(22),
    Text = "Evening Shift",
    Background = new SolidColorBrush(Color.FromArgb(50, 255, 165, 0))
});
```

## Next Steps

- **Appointments** - Learn how appointments interact with special time regions
- **Events** - Handle selection and interaction events for time slots
- **UI Features** - Further customize headers and visual elements

For appointment management, see [appointments.md](appointments.md).
