# Task Scheduling and Timeline Management

## Table of Contents
- [Overview](#overview)
- [Schedule Types](#schedule-types)
- [Custom Schedules](#custom-schedules)
- [Calendar Customization](#calendar-customization)
- [Holidays Configuration](#holidays-configuration)
- [Zooming](#zooming)
- [Schedule Display Options](#schedule-display-options)

## Overview

The Gantt control provides comprehensive scheduling capabilities to track project timelines across different time scales—from minutes to years. You can customize the schedule view, configure working calendars, define holidays, and implement zooming for dynamic timeline navigation.

## Schedule Types

### Built-in Schedule Types

The `ScheduleType` property defines the timeline scale and cell structure:

| Schedule Type | Description | Use Case |
|---------------|-------------|----------|
| `HoursWithSeconds` | Hour timeline with second intervals | Real-time monitoring |
| `MinutesWithSeconds` | Minute timeline with second intervals | Minute-level tracking |
| `DayWithHours` | Day timeline with hour intervals | Daily shift planning |
| `DayWithMinutes` | Day timeline with minute intervals | Detailed hourly tasks |
| `WeekWithDays` | Week timeline with day intervals | Weekly planning (default) |
| `MonthWithDays` | Month timeline with day intervals | Monthly projects |
| `MonthWithHours` | Month timeline with hour intervals | Month view with hours |
| `YearWithMonths` | Year timeline with month intervals | Long-term planning |
| `YearWithDays` | Year timeline with day intervals | Yearly with daily detail |
| `CustomDateTime` | Custom date-time schedule | Quarterly, fiscal periods |
| `CustomNumeric` | Custom numeric scale | Non-datetime metrics |

### Setting Schedule Type

**XAML:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ScheduleType="MonthWithDays">
</syncfusion:GanttControl>
```

**C#:**

```csharp
// Weekly view with days
ganttControl.ScheduleType = ScheduleType.WeekWithDays;

// For long-term projects
ganttControl.ScheduleType = ScheduleType.YearWithMonths;

// For hourly tracking
ganttControl.ScheduleType = ScheduleType.DayWithHours;
```

### Schedule Range Padding

Extend the schedule view for better visualization:

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ScheduleRangePadding="5">
</syncfusion:GanttControl>
```

```csharp
// Adds 5 units of padding (e.g., 5 days for WeekWithDays)
ganttControl.ScheduleRangePadding = 5;
```

## Custom Schedules

### Custom DateTime Schedule

Create custom schedules for fiscal years, quarters, or non-standard calendars:

```csharp
// Set schedule type to CustomDateTime
ganttControl.ScheduleType = ScheduleType.CustomDateTime;

// Define quarterly schedule
private IList<GanttScheduleRowInfo> GetQuarterlySchedule()
{
    List<GanttScheduleRowInfo> schedule = new List<GanttScheduleRowInfo>();
    
    // Top row: Years
    schedule.Add(new GanttScheduleRowInfo
    {
        TimeUnit = TimeUnit.Years,
        CellsPerUnit = 1,
        HorizontalAlignment = HorizontalAlignment.Left
    });
    
    // Bottom row: Quarters (3 months each)
    schedule.Add(new GanttScheduleRowInfo
    {
        TimeUnit = TimeUnit.Months,
        CellsPerUnit = 3, // 3 months per quarter
        PixelsPerUnit = 15
    });
    
    return schedule;
}

// Assign custom schedule
ganttControl.CustomScheduleSource = GetQuarterlySchedule();

// Customize quarter labels
ganttControl.ScheduleCellCreated += OnScheduleCellCreated;

private void OnScheduleCellCreated(object sender, ScheduleCellCreatedEventArgs args)
{
    DateTime currentDate = args.CurrentCell.CellDate;
    
    if (args.CurrentCell.CellTimeUnit == TimeUnit.Months)
    {
        args.CurrentCell.Foreground = new SolidColorBrush(Colors.White);
        
        if (currentDate.Month <= 3)
        {
            args.CurrentCell.Content = "Q1";
            args.CurrentCell.CellToolTip = "Quarter 1";
            args.CurrentCell.Background = new SolidColorBrush(Colors.DarkGray);
        }
        else if (currentDate.Month <= 6)
        {
            args.CurrentCell.Content = "Q2";
            args.CurrentCell.CellToolTip = "Quarter 2";
            args.CurrentCell.Background = new SolidColorBrush(Colors.LightGray);
        }
        else if (currentDate.Month <= 9)
        {
            args.CurrentCell.Content = "Q3";
            args.CurrentCell.CellToolTip = "Quarter 3";
            args.CurrentCell.Background = new SolidColorBrush(Colors.DarkGray);
        }
        else
        {
            args.CurrentCell.Content = "Q4";
            args.CurrentCell.CellToolTip = "Quarter 4";
            args.CurrentCell.Background = new SolidColorBrush(Colors.LightGray);
        }
    }
}
```

### Custom Numeric Schedule

For non-datetime measurements (e.g., sprint points, percentage completion):

```csharp
ganttControl.ScheduleType = ScheduleType.CustomNumeric;

// Define numeric scale
private ObservableCollection<GanttScheduleRowInfo> GetNumericSchedule()
{
    var schedule = new ObservableCollection<GanttScheduleRowInfo>();
    
    // Top level (groups of 5 units)
    schedule.Add(new GanttScheduleRowInfo { CellsPerUnit = 5 });
    
    // Detail level (individual units)
    schedule.Add(new GanttScheduleRowInfo 
    { 
        CellsPerUnit = 2, 
        PixelsPerUnit = 30d 
    });
    
    return schedule;
}

ganttControl.CustomScheduleSource = GetNumericSchedule();

// Use StartPointMapping and FinishPointMapping for numeric values
taskAttributeMapping.StartPointMapping = "Start"; // numeric value
taskAttributeMapping.FinishPointMapping = "End";   // numeric value
```

## Calendar Customization

### Week Configuration

**Set Week Start Day:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         WeekBeginsOn="Monday">
</syncfusion:GanttControl>
```

```csharp
// Start week on Friday
ganttControl.WeekBeginsOn = DayOfWeek.Friday;
```

### Working Hours

Define business hours for the project:

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         DefaultStartTime="09:00:00"
                         DefaultEndTime="18:00:00">
</syncfusion:GanttControl>
```

```csharp
// 9 AM to 6 PM working hours
ganttControl.DefaultStartTime = new TimeSpan(9, 0, 0);
ganttControl.DefaultEndTime = new TimeSpan(18, 0, 0);

// Extended hours (7 AM to 10 PM)
ganttControl.DefaultStartTime = new TimeSpan(7, 0, 0);
ganttControl.DefaultEndTime = new TimeSpan(22, 0, 0);
```

### Fiscal Year Configuration

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         FiscalYearBeginsOn="April"
                         IsFiscalYearNumberingEnabled="True">
</syncfusion:GanttControl>
```

```csharp
// Fiscal year starts in April
ganttControl.FiscalYearBeginsOn = Month.April;
ganttControl.IsFiscalYearNumberingEnabled = true;
```

### Weekend Configuration

**Define Weekends:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         Weekends="Saturday,Sunday"
                         ShowWeekends="True"
                         ExcludeWeekends="True"
                         ShowNonWorkingHoursBackground="True">
</syncfusion:GanttControl>
```

```csharp
// Standard weekends (Saturday, Sunday)
ganttControl.Weekends = Days.Saturday | Days.Sunday;

// Custom weekends (Thursday, Friday)
ganttControl.Weekends = Days.Thursday | Days.Friday;

// Show/hide weekends in schedule
ganttControl.ShowWeekends = true;

// Exclude weekends from duration calculation
ganttControl.ExcludeWeekends = true;

// Show background for non-working hours
ganttControl.ShowNonWorkingHoursBackground = true;
```

## Holidays Configuration

Define non-working days for the project:

```csharp
// Create holiday collection
GanttHolidayCollection holidays = new GanttHolidayCollection();

// Add holidays
holidays.Add(new GanttHoliday 
{ 
    Day = new DateTime(2024, 1, 1), 
    Background = Brushes.LightCoral,
    Description = "New Year's Day"
});

holidays.Add(new GanttHoliday 
{ 
    Day = new DateTime(2024, 7, 4), 
    Background = Brushes.LightBlue,
    Description = "Independence Day"
});

holidays.Add(new GanttHoliday 
{ 
    Day = new DateTime(2024, 12, 25), 
    Background = Brushes.LightGreen,
    Description = "Christmas"
});

// Assign to Gantt control
ganttControl.GanttHolidays = holidays;

// Show/hide holidays
ganttControl.ShowHolidays = true;

// Exclude holidays from duration calculation
ganttControl.ExcludeHolidays = true;
```

**Complete Calendar Example:**

```csharp
// Full calendar customization
ganttControl.WeekBeginsOn = DayOfWeek.Monday;
ganttControl.Weekends = Days.Saturday | Days.Sunday;
ganttControl.ShowWeekends = true;
ganttControl.ExcludeWeekends = true;
ganttControl.FiscalYearBeginsOn = Month.April;
ganttControl.IsFiscalYearNumberingEnabled = true;
ganttControl.DefaultStartTime = new TimeSpan(9, 0, 0);
ganttControl.DefaultEndTime = new TimeSpan(17, 0, 0);
ganttControl.ShowNonWorkingHoursBackground = true;
ganttControl.GanttHolidays = GetHolidayCollection();
ganttControl.ShowHolidays = true;
ganttControl.ExcludeHolidays = true;
```

## Zooming

Zooming allows dynamic timeline navigation from year to minute scales.

### Built-in Zooming

Automatic schedule adjustment based on zoom factor:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <!-- Zoom control -->
    <Slider x:Name="zoomSlider"
            Grid.Row="0"
            Minimum="80"
            Maximum="600"
            Value="100"
            Width="200"/>
    
    <!-- Gantt with zooming -->
    <syncfusion:GanttControl x:Name="ganttControl"
                             Grid.Row="1"
                             UseOnDemandSchedule="True"
                             ZoomFactor="{Binding ElementName=zoomSlider, Path=Value}"
                             BaseCellMinLength="20"
                             BaseCellMaxLength="40">
    </syncfusion:GanttControl>
</Grid>
```

**C#:**

```csharp
// Enable on-demand schedule for zooming
ganttControl.UseOnDemandSchedule = true;

// Set cell size constraints
ganttControl.BaseCellMinLength = 20;  // Minimum cell width
ganttControl.BaseCellMaxLength = 40;  // Maximum cell width

// Set zoom factor
ganttControl.ZoomFactor = 200; // Zoom in (2x)
```

### Custom Zooming

Handle zooming manually for complete control:

```csharp
ganttControl.UseOnDemandSchedule = true;
ganttControl.ZoomChanged += OnGanttZoomChanged;

private void OnGanttZoomChanged(object sender, ZoomChangedEventArgs args)
{
    if (args.ZoomFactor == 100)
    {
        // Week view
        args.ScheduleHeaderInfo = new List<GanttScheduleRowInfo> 
        {
            new GanttScheduleRowInfo { CellsPerUnit = 1, TimeUnit = TimeUnit.Weeks },
            new GanttScheduleRowInfo { CellsPerUnit = 1, TimeUnit = TimeUnit.Days, PixelsPerUnit = 20 }
        };
    }
    else if (args.ZoomFactor == 200)
    {
        // Day view with hours
        args.ScheduleHeaderInfo = new List<GanttScheduleRowInfo> 
        {
            new GanttScheduleRowInfo { CellsPerUnit = 1, TimeUnit = TimeUnit.Days, CellTextFormat = "ddd d MMM" },
            new GanttScheduleRowInfo { CellsPerUnit = 12, TimeUnit = TimeUnit.Hours, PixelsPerUnit = 4, CellTextFormat = "hh tt" }
        };
    }
    else if (args.ZoomFactor == 400)
    {
        // Hourly view
        args.ScheduleHeaderInfo = new List<GanttScheduleRowInfo> 
        {
            new GanttScheduleRowInfo { CellsPerUnit = 1, TimeUnit = TimeUnit.Days, CellTextFormat = "ddd d MMM" },
            new GanttScheduleRowInfo { CellsPerUnit = 1, TimeUnit = TimeUnit.Hours, PixelsPerUnit = 69, CellTextFormat = "hh tt" }
        };
    }
    
    args.Handled = true;
}
```

## Schedule Display Options

### Show Date with Time

Display both date and time in the Gantt grid:

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ShowDateWithTime="True">
</syncfusion:GanttControl>
```

```csharp
// Show time along with date
ganttControl.ShowDateWithTime = true;
```

### Chart Lines and Backgrounds

```csharp
// Show/hide grid lines in chart
ganttControl.ShowChartLines = true;

// Show/hide non-working hours background
ganttControl.ShowNonWorkingHoursBackground = true;
```

## Common Patterns

### Pattern 1: Long-Term Project (Years)

```csharp
ganttControl.ScheduleType = ScheduleType.YearWithMonths;
ganttControl.ScheduleRangePadding = 6; // 6 months padding
ganttControl.ShowDateWithTime = false;
```

### Pattern 2: Sprint Planning (Weeks)

```csharp
ganttControl.ScheduleType = ScheduleType.WeekWithDays;
ganttControl.WeekBeginsOn = DayOfWeek.Monday;
ganttControl.Weekends = Days.Saturday | Days.Sunday;
ganttControl.ExcludeWeekends = true;
ganttControl.ScheduleRangePadding = 3; // 3 days padding
```

### Pattern 3: Shift Scheduling (Hours)

```csharp
ganttControl.ScheduleType = ScheduleType.DayWithHours;
ganttControl.DefaultStartTime = new TimeSpan(7, 0, 0);
ganttControl.DefaultEndTime = new TimeSpan(19, 0, 0);
ganttControl.ShowDateWithTime = true;
ganttControl.ScheduleRangePadding = 2; // 2 hours padding
```

### Pattern 4: Financial Year View

```csharp
ganttControl.ScheduleType = ScheduleType.CustomDateTime;
ganttControl.FiscalYearBeginsOn = Month.April;
ganttControl.IsFiscalYearNumberingEnabled = true;
ganttControl.CustomScheduleSource = GetQuarterlySchedule();
ganttControl.ScheduleCellCreated += OnScheduleCellCreated; // Custom Q1-Q4 labels
```

## Best Practices

1. **Match Schedule Type to Project Duration:**
   - Days/weeks for short projects (< 3 months)
   - Months for medium projects (3-12 months)
   - Years for long-term projects (> 1 year)

2. **Configure Working Calendar:**
   - Set appropriate weekends for your organization
   - Define holidays upfront
   - Configure working hours for accurate duration calculations

3. **Use Zoom for Navigation:**
   - Enable `UseOnDemandSchedule` for better performance
   - Provide zoom controls for user flexibility
   - Set reasonable BaseCellMinLength/MaxLength

4. **Custom Schedules for Special Cases:**
   - Fiscal years with quarters
   - Non-standard work weeks
   - Numeric progress tracking

5. **Performance Optimization:**
   - Use `UseOnDemandSchedule=true` for large projects
   - Set `ExcludeWeekends=true` if weekends don't matter
   - Limit `ScheduleRangePadding` to necessary values

## Troubleshooting

### Issue: Schedule Not Displaying Correctly

**Solution:** Verify ScheduleType matches your date ranges and ensure tasks have valid StartDate/FinishDate.

### Issue: Zooming Performance Issues

**Solution:** Enable `UseOnDemandSchedule=true` and set appropriate BaseCellMinLength/MaxLength values.

### Issue: Custom Schedule Not Appearing

**Solution:** Ensure `ScheduleType=CustomDateTime` or `CustomNumeric`, and CustomScheduleSource is properly assigned before loading data.
