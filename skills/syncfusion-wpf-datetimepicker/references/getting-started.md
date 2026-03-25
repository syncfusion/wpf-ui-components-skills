# Getting Started — DateTimeEdit (WPF)

Setting up the Syncfusion WPF DateTimeEdit control from scratch.

---

## Required Assembly

Only **one** assembly is needed:

| Assembly | NuGet Package |
|---|---|
| `Syncfusion.Shared.WPF` | `Syncfusion.Shared.WPF` |

```
Install-Package Syncfusion.Shared.WPF
```

**Namespace:**
```csharp
using Syncfusion.Windows.Shared;
```

---

## XAML Namespace

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

---

## Add via Designer

1. Open **Toolbox** → find `DateTimeEdit` under Syncfusion Controls
2. Drag onto the design surface — assembly reference added automatically
3. Customize via SmartTag or Properties panel

---

## Add via XAML

```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:DateTimeEdit x:Name="dateTimeEdit"
                                 Height="25" Width="200"
                                 VerticalAlignment="Center"/>
    </Grid>
</Window>
```

---

## Add via C#

```csharp
using Syncfusion.Windows.Shared;

DateTimeEdit dateTimeEdit = new DateTimeEdit();
dateTimeEdit.Height = 25;
dateTimeEdit.Width  = 200;
rootGrid.Children.Add(dateTimeEdit);
```

---

## Setting the DateTime Value

```xml
<!-- Inline XAML value -->
<syncfusion:DateTimeEdit DateTime="07/05/2024"
                         Height="25" Width="200"
                         Name="dateTimeEdit"/>
```

```csharp
// Code-behind
dateTimeEdit.DateTime = new DateTime(2024, 07, 05);
```

---

## Two-Way MVVM Binding

Bind the selected date to a ViewModel property using `Mode=TwoWay`:

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

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:DateTimeEdit DateTime="{Binding SelectedDate, Mode=TwoWay}"
                         Height="25" Width="200"
                         Name="dateTimeEdit"/>
```

**Sync two pickers to the same value:**
```xml
<syncfusion:DateTimeEdit DateTime="{Binding SelectedDate, Mode=TwoWay}"
                         Height="25" Width="200" Name="picker1"/>
<syncfusion:DateTimeEdit DateTime="{Binding SelectedDate, Mode=TwoWay}"
                         Height="25" Width="200" Name="picker2" Margin="0,10,0,0"/>
```

---

## DateTimeChanged Event

Fires whenever the `DateTime` property changes. Provides old and new values:

```xml
<syncfusion:DateTimeEdit DateTimeChanged="DateTimeEdit_DateTimeChanged"
                         Name="dateTimeEdit"/>
```

```csharp
private void DateTimeEdit_DateTimeChanged(DependencyObject d,
                                          DependencyPropertyChangedEventArgs e)
{
    var oldDate = e.OldValue as DateTime?;
    var newDate = e.NewValue as DateTime?;
    // respond to change
}
```

---

## Predefined Pattern Reference

Set via `Pattern` property. Default is `ShortDate`.

| Pattern Value | Example Output |
|---|---|
| `ShortDate` | 7/5/2024 |
| `LongDate` | Thursday, July 5, 2024 |
| `ShortTime` | 3:45 PM |
| `LongTime` | 3:45:22 PM |
| `FullDateTime` | Thursday, July 5, 2024 3:45:22 PM |
| `MonthDay` | July 5 |
| `YearMonth` | July 2024 |
| `RFC1123` | Thu, 05 Jul 2024 15:45:22 GMT |
| `ShortableDateTime` | 2024-07-05T15:45:22 |
| `UniversalShortableDateTime` | 2024-07-05 15:45:22Z |
| `CustomPattern` | *(use `CustomPattern` property)* |

```xml
<syncfusion:DateTimeEdit Pattern="FullDateTime" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.Pattern = DateTimePattern.FullDateTime;
```

---

## Basic Date Range

Prevent selection outside a valid range:

```xml
<syncfusion:DateTimeEdit MinDateTime="01/01/2020"
                         MaxDateTime="12/31/2030"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.MinDateTime = new DateTime(2020, 1, 1);
dateTimeEdit.MaxDateTime = new DateTime(2030, 12, 31);
```

> For blackout dates, DisableDateSelection, and IsReadOnly — see [date-restrictions.md](date-restrictions.md).

---

## Apply Theme

```csharp
using Syncfusion.SfSkinManager;

// After InitializeComponent()
SfSkinManager.SetVisualStyle(dateTimeEdit, VisualStyles.FluentLight);
```

Available: `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Windows11Light`, `Windows11Dark`, `Office2019Colorful`, and more.
