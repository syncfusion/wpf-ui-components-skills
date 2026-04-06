# Getting Started with CalendarEdit

## Installation

Install the required assembly via NuGet:

```powershell
Install-Package Syncfusion.Shared.WPF
```

Or add `Syncfusion.Shared.WPF` via the Visual Studio **Add Reference** dialog.

## Add CalendarEdit (XAML)

Add the Syncfusion namespace and declare the control:

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"

<syncfusion:CalendarEdit Name="calendarEdit" Height="200" Width="200" />
```

## Add CalendarEdit (C#)

```csharp
var calendarEdit = new CalendarEdit { Height = 200, Width = 200 };
grid.Children.Add(calendarEdit);
```

## Basic Usage

Set or get the primary selected date:

```csharp
calendarEdit.Date = new DateTime(2024, 3, 15);
DateTime selected = calendarEdit.Date;
```

Bind the selected date to UI elements:

```xaml
<TextBlock Text="{Binding ElementName=calendarEdit, Path=Date, StringFormat='Selected: {0:yyyy-MM-dd}'}" />
```

## Where to go next

- Date selection details: [date-selection.md](date-selection.md)
- Navigation modes: [navigation.md](navigation.md)
- Date restrictions & blackout dates: [restrict-dates.md](restrict-dates.md)
- Styling and appearance: [appearance.md](appearance.md)
