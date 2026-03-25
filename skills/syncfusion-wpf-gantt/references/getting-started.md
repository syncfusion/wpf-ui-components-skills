# Getting Started with WPF Gantt Control

Complete guide for installing, configuring, and setting up the Syncfusion WPF Gantt control in your Windows Presentation Foundation applications.

## Overview

The Essential Gantt for WPF is an MS Project-like project viewer with built-in grid, schedule, and resource assignment capabilities. It helps project managers develop plans, assign resources to tasks, track progress, and manage project timelines with features like drag-and-drop support, automatic data synchronization between grid and chart, and XML import/export capabilities.

## Gantt Control Architecture

The Gantt control is composed of three main components that work together:

### 1. GanttGrid

The GanttGrid is a table view control that displays scheduled tasks/activities in hierarchical format. It provides an editable interface for task properties.

**Key Elements:**
- **Header** - Table header containing field names (Task Name, Start Date, Finish Date, etc.)
- **Parent Task** - Summary task representing an activity split into child tasks
- **Child Task** - Individual task containing specific task information
- **Expand/Collapse Button** - Toggle button for expanding or collapsing task hierarchy

### 2. GanttChart (GanttChartVisualControl)

The GanttChart provides graphical representation of scheduled tasks/activities with visual indicators for task types, progress, and relationships.

**Visual Components:**
- **Node** - Rectangular bar representing an individual/child task
- **Header Node** - Bar representing a parent/summary task
- **Milestone** - Diamond indicator for single-day targets
- **Progress Indicator** - Visual representation of completion percentage
- **Connector** - Line showing dependency relationships between tasks

### 3. ScheduleHeader

The ScheduleHeader is a timeline that measures and tracks project progress. It displays the time scale (hours, days, weeks, months, years) and helps visualize task duration.

## Installation

### NuGet Package Installation

Install the Syncfusion Gantt control via NuGet Package Manager:

```powershell
Install-Package Syncfusion.SfGantt.WPF
```

Or search for `Syncfusion.SfGantt.WPF` in the NuGet Package Manager UI.

### Assembly References

Add the following assembly references to your WPF project:

- `Syncfusion.SfGantt.WPF` - Core Gantt control
- `Syncfusion.Data.WPF` - Data management utilities
- `Syncfusion.Shared.WPF` - Shared WPF utilities

### Namespace Imports

**In XAML:**

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:gantt="clr-namespace:Syncfusion.Windows.Controls.Gantt;assembly=Syncfusion.SfGantt.WPF">
```

**In C#:**

```csharp
using Syncfusion.Windows.Controls.Gantt;
```

## Adding GanttControl to Your Application

There are two methods to add the Gantt control to your WPF application:

### Method 1: Programmatically

**XAML:**

```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Gantt Control Demo" Height="600" Width="1000">
    
    <syncfusion:GanttControl x:Name="ganttControl" />
    
</Window>
```

**C# Code-Behind:**

```csharp
using Syncfusion.Windows.Controls.Gantt;
using System.Windows;

namespace MyApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // GanttControl is already defined in XAML
            // Additional configuration can be done here
        }
    }
}
```

**Note:** When run, this displays an empty Gantt control with a built-in TaskDetails collection.

### Method 2: Through XAML Designer

1. Open the XAML page in Visual Studio
2. Open the **Toolbox** (View → Toolbox)
3. Search for `GanttControl` in the Syncfusion WPF controls section
4. Drag and drop the GanttControl onto the designer surface
5. Visual Studio automatically adds assembly references
6. Customize properties using the Properties window

## Basic Configuration

### Adjusting Chart and Grid Size

Control the width allocation between the grid and chart sections:

**XAML:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         GridWidth="300"
                         ChartWidth="700">
</syncfusion:GanttControl>
```

**C#:**

```csharp
ganttControl.GridWidth = new GridLength(300);
ganttControl.ChartWidth = new GridLength(700);
```

### Schedule Range Padding

Extend the schedule view with padding at the start to improve user experience:

**XAML:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ScheduleRangePadding="5">
</syncfusion:GanttControl>
```

**C#:**

```csharp
ganttControl.ScheduleRangePadding = 5;
```

This adds 5 lower schedule units (e.g., 5 days if using WeekWithDays schedule type) to the beginning of the timeline.

## Schedule Types

Define the timeline scale using the `ScheduleType` property. Available options:

| Schedule Type | Description |
|---------------|-------------|
| `HoursWithSeconds` | Hour-based timeline with second intervals |
| `MinutesWithSeconds` | Minute-based timeline with second intervals |
| `DayWithHours` | Day-based timeline with hour intervals |
| `DayWithMinutes` | Day-based timeline with minute intervals |
| `WeekWithDays` | Week-based timeline with day intervals |
| `MonthWithDays` | Month-based timeline with day intervals |
| `MonthWithHours` | Month-based timeline with hour intervals |
| `YearWithMonths` | Year-based timeline with month intervals |
| `YearWithDays` | Year-based timeline with day intervals |
| `CustomDateTime` | Custom date-time based timeline |
| `CustomNumeric` | Custom numeric timeline |

**Example:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ScheduleType="WeekWithDays">
</syncfusion:GanttControl>
```

```csharp
ganttControl.ScheduleType = ScheduleType.YearWithMonths;
```

## Showing Date with Time in GanttGrid

By default, the GanttGrid displays only dates. To show both date and time:

**XAML:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ShowDateWithTime="True">
</syncfusion:GanttControl>
```

**C#:**

```csharp
ganttControl.ShowDateWithTime = true;
```

## Auto Expand Mode

Control the initial expansion state of task nodes when loading the Gantt control:

**Available Modes:**

- `None` - All tasks are collapsed when loaded
- `RootNodesExpanded` - Only root (top-level) tasks are expanded
- `AllNodesExpanded` - All tasks are expanded (default)

**XAML:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         AutoExpandMode="RootNodesExpanded">
</syncfusion:GanttControl>
```

**C#:**

```csharp
ganttControl.AutoExpandMode = GanttAutoExpandMode.None;
```

## Theme Support

The WPF Gantt control supports various built-in themes for modern appearance:

### Applying Themes with SfSkinManager

```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
}
```

### Available Themes

- MaterialLight
- MaterialDark
- Office2019Colorful
- Office2019Black
- Office2019White
- FluentLight
- FluentDark

### Creating Custom Themes

Use Syncfusion Theme Studio to create custom themes that match your application branding.

## Complete Example

Here's a complete example bringing together all getting-started concepts:

```xml
<Window x:Class="GanttDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Project Management" Height="600" Width="1200">
    
    <Grid>
        <syncfusion:GanttControl x:Name="ganttControl"
                                 GridWidth="350"
                                 ChartWidth="850"
                                 ScheduleType="WeekWithDays"
                                 ScheduleRangePadding="5"
                                 AutoExpandMode="RootNodesExpanded"
                                 ShowDateWithTime="False">
        </syncfusion:GanttControl>
    </Grid>
    
</Window>
```

```csharp
using Syncfusion.Windows.Controls.Gantt;
using Syncfusion.SfSkinManager;
using System.Windows;

namespace GanttDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Apply theme
            SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
        }
    }
}
```

## Next Steps

Now that you have the Gantt control set up:

1. **Data Binding** - Learn how to bind your project data (read `data-binding.md`)
2. **Task Scheduling** - Configure timelines and calendars (read `task-scheduling.md`)
3. **Task Relationships** - Set up dependencies between tasks (read `task-relationships.md`)
4. **Customization** - Style your Gantt control (read `customization.md`)

## Common Issues

### Issue: Control Not Displaying

**Solution:** Verify that:
- NuGet package is installed correctly
- Assembly references are added
- Namespace imports are correct in XAML
- Window has sufficient size (Height and Width)

### Issue: Grid or Chart Section Missing

**Solution:** Check GridWidth and ChartWidth properties. Both should have positive values.

### Issue: Tasks Not Visible

**Solution:** Ensure ItemsSource is bound to a valid TaskDetails collection (see `data-binding.md`).
