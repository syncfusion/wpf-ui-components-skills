---
name: syncfusion-wpf-datetimepicker
description: Implements the Syncfusion WPF DateTimeEdit (DateTimePicker) control for date and time selection with dropdown calendar/clock support. Use this when adding datetime pickers, masked datetime input, or datetime selection controls in WPF applications. Covers DateTime binding, mask/free-form editing, pattern formatting, CultureInfo localization, dropdown views, date range restrictions, blackout dates, null value handling, watermarks, and theming with SfSkinManager.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing DateTimeEdit (WPF)

An enhanced date-time input control with mask editing, calendar/clock dropdown, date range restriction, formatting, and MVVM binding support.

## When to Use This Skill

Use this skill when the user needs to:
- Add a date/time picker input field to a WPF application
- Bind a selected date to a ViewModel property (`DateTime` two-way binding)
- Format the displayed date/time (predefined patterns or custom patterns)
- Restrict selectable dates with min/max range or blackout dates
- Enable inline editing (mask mode or free-form text editing)
- Show a calendar, clock, or combined dropdown picker
- Display watermark text when no date is selected
- Handle null date values
- Apply locale-specific date formats via `CultureInfo`
- Localize dropdown button labels (Today/None)

---

## Component Overview

| Class | Namespace | Assembly |
|---|---|---|
| `DateTimeEdit` | `Syncfusion.Windows.Shared` | `Syncfusion.Shared.WPF` |

**XAML Namespace:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

**NuGet:**
```
Install-Package Syncfusion.Shared.WPF
```

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly setup and XAML namespace
- Add via designer, XAML, or C#
- Setting `DateTime` value
- Two-way MVVM binding with `DateTimeChanged` event
- Predefined `Pattern` quick reference
- `MinDateTime` / `MaxDateTime` basics
- SfSkinManager theme setup

### DateTime Editing
📄 **Read:** [references/datetime-editing.md](references/datetime-editing.md)
- Mask editing (default) — field-by-field input with auto-validation
- Keyboard navigation between date/time fields
- Free-form text editing (`CanEdit="True"`)
- Mouse wheel value change (`EnableMouseWheelEdit`)
- Up/down repeat buttons (`IsVisibleRepeatButton`, `IsEnabledRepeatButton`)
- Custom repeat button UI templates
- Alpha key month navigation (`EnableAlphaKeyNavigation`)
- BackSpace/Delete key editing
- `OnFocusBehavior` and `AutoForwarding` focus control

### Formatting & Culture
📄 **Read:** [references/formatting-and-culture.md](references/formatting-and-culture.md)
- All 11 predefined `Pattern` values
- `CustomPattern` with `Pattern="CustomPattern"`
- `DateTimeFormat` with `DateTimeFormatInfo`
- `CultureInfo` for locale-specific formatting
- `PatternChanged` / `CustomPatternChanged` events

### Dropdown Popup
📄 **Read:** [references/dropdown-popup.md](references/dropdown-popup.md)
- `DropDownView`: Calendar / Clock / Combined
- Keyboard shortcuts (Alt+Down, F4)
- `TodayButtonAction` (Date vs DateTime)
- `IsEmptyDateEnabled` — None button / reset to null
- `AbbreviatedMonthNames` / `ShortestDayNames`
- `PopupDelay`, `IsButtonPopUpEnabled`
- `DropDownButtonTemplate` custom UI
- Custom calendar and clock (SfDateSelector / SfTimeSelector)
- `TimeStepInterval` for clock

### Date Restrictions & Null Values
📄 **Read:** [references/date-restrictions.md](references/date-restrictions.md)
- `MinDateTime` / `MaxDateTime` — prevent out-of-range selection
- `BlackoutDates` — block specific date ranges
- `DisableDateSelection` — month/year only
- `IsReadOnly` — prevent all user edits
- `NullValue` / `IsEmptyDateEnabled` / `ShowMaskOnNullValue`
- `NoneDateText` watermark

### Appearance
📄 **Read:** [references/appearance.md](references/appearance.md)
- `Foreground`, `Background`, `SelectionBrush`
- `FocusedBorderBrush`
- `FlowDirection` RTL
- Repeat button background and margins
- SfSkinManager themes list

### Localization
📄 **Read:** [references/localization.md](references/localization.md)
- `CurrentUICulture` setup
- Resource file naming: `Syncfusion.Shared.Wpf.{culture}.resx`
- Name/Value pairs for Today/None button labels
- French culture example

---

## Quick Start Example

```xml
<!-- MainWindow.xaml -->
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
        <syncfusion:DateTimeEdit DateTime="{Binding SelectedDate, Mode=TwoWay}"
                                 Pattern="ShortDate"
                                 MinDateTime="01/01/2020"
                                 MaxDateTime="12/31/2030"
                                 Height="25" Width="200"
                                 Name="dateTimeEdit"/>
    </StackPanel>
</Window>
```

```csharp
// ViewModel.cs
public class ViewModel : NotificationObject
{
    private DateTime _selectedDate = DateTime.Now;
    public DateTime SelectedDate
    {
        get => _selectedDate;
        set { _selectedDate = value; RaisePropertyChanged(nameof(SelectedDate)); }
    }
}
```

---

## Common Patterns

| Scenario | Properties / Approach |
|---|---|
| Two-way date binding | `DateTime="{Binding SelectedDate, Mode=TwoWay}"` |
| Show calendar + clock | `DropDownView="Combined" Pattern="FullDateTime"` |
| Free-form text editing | `CanEdit="True"` |
| Date-only range restriction | `MinDateTime="..." MaxDateTime="..."` |
| Allow null / clear value | `IsEmptyDateEnabled="True" NullValue="{x:Null}" ShowMaskOnNullValue="False"` |
| Watermark when null | Add `NoneDateText="Select a date"` alongside null value properties |
| Custom date display | `Pattern="CustomPattern" CustomPattern="dd-MMM-yyyy"` |
| Culture-aware formatting | `CultureInfo="fr-FR" Pattern="FullDateTime"` |
| Read-only input | `IsReadOnly="True"` |
| RTL layout | `FlowDirection="RightToLeft"` |
| Apply theme | `SfSkinManager.SetVisualStyle(dte, VisualStyles.FluentLight)` |

---

## Key Properties

| Property | Type | Default | Description |
|---|---|---|---|
| `DateTime` | `DateTime` | `DateTime.Now` | Selected date/time value |
| `Pattern` | `DateTimePattern` | `ShortDate` | Display format pattern |
| `CustomPattern` | `string` | `null` | Custom format string (requires `Pattern="CustomPattern"`) |
| `CultureInfo` | `CultureInfo` | System culture | Locale for date formatting |
| `CanEdit` | `bool` | `false` | Enable free-form text editing |
| `MinDateTime` | `DateTime` | `DateTime.MinValue` | Minimum selectable date |
| `MaxDateTime` | `DateTime` | `DateTime.MaxValue` | Maximum selectable date |
| `DropDownView` | `DropDownViews` | `Calendar` | Popup content: Calendar/Clock/Combined |
| `IsEmptyDateEnabled` | `bool` | `false` | Allow null date value |
| `NullValue` | `DateTime?` | `null` | Value when date is cleared |
| `NoneDateText` | `string` | `""` | Watermark text when value is null |
| `ShowMaskOnNullValue` | `bool` | `true` | Show mask vs watermark when null |
| `IsReadOnly` | `bool` | `false` | Prevent user edits |
| `IsVisibleRepeatButton` | `bool` | `false` | Show up/down spinner buttons |
| `EnableMouseWheelEdit` | `bool` | `true` | Allow mouse wheel to change value |
