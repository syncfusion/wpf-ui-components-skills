---
name: syncfusion-wpf-calendar
description: Guide to using the Syncfusion WPF CalendarEdit control for date selection and navigation. Use this skill whenever the user needs to implement calendar functionality, select dates, restrict date ranges, customize calendar appearance, or work with multi-date selection. Essential for building date picker interfaces, booking systems, scheduling applications, and any WPF project requiring calendar controls.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# WPF Calendar (CalendarEdit) Implementation Guide

The [CalendarEdit](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Shared.CalendarEdit.html) control provides a comprehensive calendar UI for selecting and managing dates in WPF applications. It supports single and multiple date selection, date range restrictions, customizable appearance, and powerful navigation modes from day to decade views.

## When to Use This Skill

- **Date Selection:** When you need users to select single or multiple dates
- **Calendar UI:** Building calendar controls with month/year navigation
- **Date Restrictions:** Implementing date range constraints or blocking specific dates
- **Scheduling:** Creating booking, appointment, or event selection interfaces
- **Date Manipulation:** Using calendar methods for date arithmetic (add days, months, etc.)
- **Appearance Customization:** Styling calendar colors, brushes, and visual properties
- **Multi-language Support:** Supporting RTL (right-to-left) layouts and different cultures

## Component Overview

**Key Features:**
- Support for dates from year 0 to year 9999
- Single and multiple date selection modes
- Day, month, year, and decade navigation views
- Min/max date constraints and blackout date blocking
- Customizable colors, brushes, and animations
- RTL and culture support
- Built-in animation for smooth transitions

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly setup
- Adding CalendarEdit via designer, XAML, and C#
- Basic control structure and initial setup

### Date Selection
📄 **Read:** [references/date-selection.md](references/date-selection.md)
- Select single dates (mouse click and programmatically)
- Select multiple dates (drag selection and Ctrl+click)
- Working with Date and SelectedDates properties

### Navigation Features
📄 **Read:** [references/navigation.md](references/navigation.md)
- Navigate between day, month, year, and decade modes
- Header navigation, keyboard shortcuts, and animation controls
- Customizing header appearance and animation timing

### Restrict Date Selection
📄 **Read:** [references/restrict-dates.md](references/restrict-dates.md)
- Set date range constraints (MinDate and MaxDate)
- Hide or show disabled dates outside the range
- Disable all date selection or block specific date ranges

### Appearance & Customization
📄 **Read:** [references/appearance.md](references/appearance.md)
- Customize foreground, background, and brush colors
- RTL layout support and flow direction
- Calendar object methods for date arithmetic operations

## Quick Start

### Basic XAML Setup

```xaml
<Window x:Class="CalendarDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Calendar Demo" Height="450" Width="500">
    <Grid>
        <!-- Basic calendar control -->
        <syncfusion:CalendarEdit Name="calendarEdit"
                                 Height="250"
                                 Width="300"
                                 VerticalAlignment="Center"
                                 HorizontalAlignment="Center"/>
    </Grid>
</Window>
```

### Basic C# Setup

```csharp
using Syncfusion.Windows.Shared;

CalendarEdit calendarEdit = new CalendarEdit();
calendarEdit.Height = 250;
calendarEdit.Width = 300;

// Get selected date
DateTime selectedDate = calendarEdit.Date;

// Set selected date programmatically
calendarEdit.Date = new DateTime(2024, 3, 15);
```

## Common Patterns

### Pattern 1: Single Date Selection with Range Restriction
```csharp
// Allow only dates in March 2024
calendarEdit.MinDate = new DateTime(2024, 3, 1);
calendarEdit.MaxDate = new DateTime(2024, 3, 31);
calendarEdit.Date = new DateTime(2024, 3, 15);

// Get the selected date
DateTime selectedDate = calendarEdit.Date;
```

### Pattern 2: Multiple Date Selection
```csharp
// Enable multiple date selection
calendarEdit.AllowMultiplySelection = true;

// Select multiple dates by dragging or Ctrl+clicking
// Retrieve all selected dates
var selectedDates = calendarEdit.SelectedDates;
```

### Pattern 3: Block Specific Dates (Blackout Dates)
```csharp
// Block specific date ranges
calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 5));
calendarEdit.BlackoutDates.Add(new DateTime(2024, 3, 20));

// Users cannot select these dates
calendarEdit.Date = new DateTime(2024, 3, 15);
```

### Pattern 4: Customize Calendar Appearance
```csharp
using System.Windows.Media;

// Customize colors
calendarEdit.Foreground = Brushes.Blue;
calendarEdit.Background = Brushes.White;
calendarEdit.MouseOverForeground = Brushes.Red;
calendarEdit.MouseOverBackground = Brushes.Lavender;

// Customize header
calendarEdit.HeaderBackground = Brushes.Green;
calendarEdit.HeaderForeground = Brushes.Yellow;
```

## Key Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `Date` | DateTime | Gets or sets the currently selected date |
| `SelectedDates` | Collection | Gets all selected dates in multi-selection mode |
| `AllowMultiplySelection` | bool | Enables or disables multiple date selection |
| `MinDate` | DateTime | Sets the earliest selectable date |
| `MaxDate` | DateTime | Sets the latest selectable date |
| `BlackoutDates` | Collection | Contains dates that cannot be selected |
| `DisableDateSelection` | bool | Disables date selection (month/year still selectable) |
| `MinMaxHidden` | bool | Hides dates outside MinDate/MaxDate range |
| `HeaderBackground` | Brush | Customizes the header background color |
| `HeaderForeground` | Brush | Customizes the header text color |
| `ChangeModeTime` | int | Animation duration for mode changes (ms) |
| `FrameMovingTime` | int | Animation duration for month transitions (ms) |

---

**Next Steps:** Choose a reference file above based on what you need to implement. Start with "Getting Started" if setting up CalendarEdit for the first time.
