# Dropdown Popup — DateTimeEdit (WPF)

## Table of Contents
- [Dropdown Views](#dropdown-views)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Today Button](#today-button)
- [None Button and Empty Date](#none-button-and-empty-date)
- [Custom Month and Weekday Names](#custom-month-and-weekday-names)
- [Popup Delay](#popup-delay)
- [Disable Dropdown Button](#disable-dropdown-button)
- [Custom Dropdown Button UI](#custom-dropdown-button-ui)
- [Clock Time Step Interval](#clock-time-step-interval)
- [Custom Calendar](#custom-calendar)
- [Custom Clock](#custom-clock)
- [Hide Today Button](#hide-today-button)

---

## Dropdown Views

Control what appears in the dropdown popup using the `DropDownView` property:

| Value | Popup Shows |
|---|---|
| `Calendar` | Calendar only *(default)* |
| `Clock` | Analog/digital clock only |
| `Combined` | Calendar + Clock together |

```xml
<!-- Calendar only (default) -->
<syncfusion:DateTimeEdit DropDownView="Calendar" Name="dateTimeEdit"/>

<!-- Clock only -->
<syncfusion:DateTimeEdit DropDownView="Clock"
                         Pattern="ShortTime"
                         Name="dateTimeEdit"/>

<!-- Combined calendar + clock -->
<syncfusion:DateTimeEdit DropDownView="Combined"
                         Pattern="FullDateTime"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.DropDownView = DropDownViews.Combined;
dateTimeEdit.Pattern      = DateTimePattern.FullDateTime;
```

---

## Keyboard Shortcuts

| Key Combo | Action |
|---|---|
| `Alt + Down` | Open dropdown popup |
| `F4` | Open dropdown popup |
| `Esc` | Close dropdown popup |

---

## Today Button

The popup includes a **Today** button that sets the picker to today's date.

**Default behavior:** Sets only the date (time unchanged).

**Set date AND current time:**
```xml
<syncfusion:DateTimeEdit TodayButtonAction="DateTime"
                         Pattern="FullDateTime"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.TodayButtonAction = TodayButtonAction.DateTime;
dateTimeEdit.Pattern           = DateTimePattern.FullDateTime;
```

| `TodayButtonAction` | Effect |
|---|---|
| `Date` *(default)* | Sets today's date, preserves existing time |
| `DateTime` | Sets today's date and current time |

---

## None Button and Empty Date

The **None** button resets the picker value to `null`. Hidden by default — enable with `IsEmptyDateEnabled="True"`:

```xml
<syncfusion:DateTimeEdit IsEmptyDateEnabled="True"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.IsEmptyDateEnabled = true;
```

**Show the None button and allow null value:**
```xml
<syncfusion:DateTimeEdit IsEmptyDateEnabled="True"
                         NullValue="{x:Null}"
                         ShowMaskOnNullValue="False"
                         Name="dateTimeEdit"/>
```

> For full null value and watermark configuration, see [date-restrictions.md](date-restrictions.md).

---

## Custom Month and Weekday Names

Override the calendar popup's month abbreviations:

```xml
<syncfusion:DateTimeEdit Name="dateTimeEdit">
    <syncfusion:DateTimeEdit.AbbreviatedMonthNames>
        <x:Array Type="sys:String" xmlns:sys="clr-namespace:System;assembly=mscorlib">
            <sys:String>[1]Jan</sys:String>
            <sys:String>[2]Feb</sys:String>
            <sys:String>[3]Mar</sys:String>
            <sys:String>[4]Apr</sys:String>
            <sys:String>[5]May</sys:String>
            <sys:String>[6]Jun</sys:String>
            <sys:String>[7]Jul</sys:String>
            <sys:String>[8]Aug</sys:String>
            <sys:String>[9]Sep</sys:String>
            <sys:String>[10]Oct</sys:String>
            <sys:String>[11]Nov</sys:String>
            <sys:String>[12]Dec</sys:String>
        </x:Array>
    </syncfusion:DateTimeEdit.AbbreviatedMonthNames>
</syncfusion:DateTimeEdit>
```

Override weekday header names:

```xml
<syncfusion:DateTimeEdit Name="dateTimeEdit">
    <syncfusion:DateTimeEdit.ShortestDayNames>
        <x:Array Type="sys:String" xmlns:sys="clr-namespace:System;assembly=mscorlib">
            <sys:String>Sun</sys:String>
            <sys:String>Mon</sys:String>
            <sys:String>Tue</sys:String>
            <sys:String>Wed</sys:String>
            <sys:String>Thu</sys:String>
            <sys:String>Fri</sys:String>
            <sys:String>Sat</sys:String>
        </x:Array>
    </syncfusion:DateTimeEdit.ShortestDayNames>
</syncfusion:DateTimeEdit>
```

```csharp
dateTimeEdit.AbbreviatedMonthNames = new string[]
    { "[1]Jan","[2]Feb","[3]Mar","[4]Apr","[5]May","[6]Jun",
      "[7]Jul","[8]Aug","[9]Sep","[10]Oct","[11]Nov","[12]Dec" };

dateTimeEdit.ShortestDayNames = new string[]
    { "Sun","Mon","Tue","Wed","Thu","Fri","Sat" };
```

---

## Popup Delay

Add a delay before the dropdown opens after clicking the button:

```xml
<!-- 2-second delay -->
<syncfusion:DateTimeEdit PopupDelay="0:0:2" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.PopupDelay = new TimeSpan(0, 0, 2);
```

---

## Disable Dropdown Button

Hide the dropdown calendar button entirely (forces mask or free-form text entry only):

```xml
<syncfusion:DateTimeEdit IsButtonPopUpEnabled="False" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.IsButtonPopUpEnabled = false;
```

> **Combine with repeat buttons:** Set `IsButtonPopUpEnabled="False"` + `IsVisibleRepeatButton="True"` for a spinner-only interface.

---

## Custom Dropdown Button UI

Replace the default dropdown button arrow with a custom visual:

```xml
<syncfusion:DateTimeEdit DropDownView="Calendar" Name="dateTimeEdit">
    <syncfusion:DateTimeEdit.DropDownButtonTemplate>
        <ControlTemplate>
            <Border Background="#0078D4" CornerRadius="2">
                <TextBlock Text="📅"
                           FontSize="14"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"/>
            </Border>
        </ControlTemplate>
    </syncfusion:DateTimeEdit.DropDownButtonTemplate>
</syncfusion:DateTimeEdit>
```

---

## Clock Time Step Interval

Control the increment step for hours/minutes/seconds in the clock dropdown:

```xml
<!-- Clock increments by 3 each click/scroll -->
<syncfusion:DateTimeEdit DropDownView="Clock"
                         TimeStepInterval="3"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.DropDownView    = DropDownViews.Clock;
dateTimeEdit.TimeStepInterval = 3;
```

---

## Custom Calendar

Replace the default calendar popup with a `SfDateSelector`:

```csharp
// ViewModel.cs
public DateTime SelectedDate { get; set; } = DateTime.Now;
```

```xml
<syncfusion:DateTimeEdit DateTime="{Binding SelectedDate, Mode=TwoWay}"
                         DropDownView="Calendar"
                         Pattern="ShortDate"
                         Name="dateTimeEdit">
    <syncfusion:DateTimeEdit.DateTimeCalender>
        <syncfusion:SfDateSelector SelectorItemWidth="80"
                                   SelectorItemHeight="80"
                                   ShowDoneButton="False"
                                   SelectedDateTime="{Binding SelectedDate, Mode=TwoWay}"/>
    </syncfusion:DateTimeEdit.DateTimeCalender>
</syncfusion:DateTimeEdit>
```

---

## Custom Clock

Replace the default clock with a `SfTimeSelector`:

```xml
<syncfusion:DateTimeEdit DateTime="{Binding SelectedTime, Mode=TwoWay}"
                         DropDownView="Clock"
                         Pattern="ShortTime"
                         Name="dateTimeEdit">
    <syncfusion:DateTimeEdit.Clock>
        <syncfusion:SfTimeSelector SelectorItemWidth="70"
                                   SelectorItemHeight="70"
                                   ShowCancelButton="False"
                                   ShowDoneButton="False"
                                   SelectedTime="{Binding SelectedTime, Mode=TwoWay}"/>
    </syncfusion:DateTimeEdit.Clock>
</syncfusion:DateTimeEdit>
```

> Use `DropDownView="Combined"` with both `DateTimeCalender` and `Clock` set for a full custom date+time selector.

---

## Hide Today Button

Remove the Today button from the popup via the control template:

```csharp
private void DateTimeEdit_Loaded(object sender, RoutedEventArgs e)
{
    Button todayButton = dateTimeEdit.Template.FindName("todayButton", dateTimeEdit) as Button;
    if (todayButton != null)
        todayButton.Visibility = Visibility.Collapsed;
}
```

```xml
<syncfusion:DateTimeEdit Loaded="DateTimeEdit_Loaded" Name="dateTimeEdit"/>
```
