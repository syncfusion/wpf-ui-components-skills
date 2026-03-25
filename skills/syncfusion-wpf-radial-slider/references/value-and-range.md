# Value and Range Configuration

## Table of Contents
- [Value, Minimum, Maximum](#value-minimum-maximum)
- [Display Value in Content Area](#display-value-in-content-area)
- [ContentTemplate](#contenttemplate)
- [SmallChange — Step Interval](#smallchange--step-interval)
- [TickFrequency](#tickfrequency)
- [ShowMaximumValue](#showmaximumvalue)
- [StartAngle and EndAngle](#startangle-and-endangle)
- [SweepDirection](#sweepdirection)
- [Gotchas](#gotchas)

---

## Value, Minimum, Maximum

| Property | Default | Description |
|---|---|---|
| `Value` | `0` | Currently selected value |
| `Minimum` | `0` | Start of the value range |
| `Maximum` | `100` | End of the value range |

```xaml
<syncfusion:SfRadialSlider Minimum="100"
                           Maximum="200"
                           Value="150"
                           Name="radialSlider"/>
```

```csharp
radialSlider.Minimum = 100;
radialSlider.Maximum = 200;
radialSlider.Value   = 150;
```

---

## Display Value in Content Area

Set `Content` equal to the selected value to show it inside the inner rim:

```xaml
<syncfusion:SfRadialSlider Value="34"
                           Content="34"
                           Height="200" Width="200"/>
```

With two-way MVVM binding (recommended):

```xaml
<syncfusion:SfRadialSlider Value="{Binding SelectedValue, Mode=TwoWay}"
                           Content="{Binding SelectedValue, Mode=TwoWay}"
                           Height="200" Width="200">
    <syncfusion:SfRadialSlider.DataContext>
        <local:ViewModel/>
    </syncfusion:SfRadialSlider.DataContext>
</syncfusion:SfRadialSlider>
```

Using `ElementName` binding (no ViewModel required):

```xaml
<syncfusion:SfRadialSlider Name="sfRadialSlider" Height="200" Width="200">
    <TextBlock Text="{Binding ElementName=sfRadialSlider, Path=Value}"
               FontSize="15"
               HorizontalAlignment="Center"
               VerticalAlignment="Center"/>
</syncfusion:SfRadialSlider>
```

> The `Content` property accepts any UIElement or value. Child elements placed inside `<SfRadialSlider>` tags appear in the inner rim content area.

---

## ContentTemplate

Customize the inner rim content area with a `DataTemplate`. The `DataContext` of `ContentTemplate` is the `SfRadialSlider` itself:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private double _selectedValue;
    public double SelectedValue
    {
        get => _selectedValue;
        set { _selectedValue = value; OnPropertyChanged(nameof(SelectedValue)); }
    }
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string n) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(n));
}
```

```xaml
<syncfusion:SfRadialSlider Value="{Binding SelectedValue, Mode=TwoWay}"
                           Height="200" Width="200">
    <syncfusion:SfRadialSlider.ContentTemplate>
        <DataTemplate>
            <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
                <TextBlock Text="{Binding SelectedValue}"
                           FontSize="18" FontWeight="Bold"
                           Foreground="DarkBlue"
                           HorizontalAlignment="Center"/>
                <TextBlock Text="°C"
                           FontSize="10"
                           HorizontalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfRadialSlider.ContentTemplate>
    <syncfusion:SfRadialSlider.DataContext>
        <local:ViewModel/>
    </syncfusion:SfRadialSlider.DataContext>
</syncfusion:SfRadialSlider>
```

> `ContentTemplate` takes priority over `Content`. Remove `Content` binding if using `ContentTemplate`.

---

## SmallChange — Step Interval

Controls the smallest selectable increment. Default is `0.1`.

```xaml
<!-- Snap to multiples of 5 -->
<syncfusion:SfRadialSlider SmallChange="5" Name="radialSlider"/>
```

```csharp
radialSlider.SmallChange = 5;
```

> If `SmallChange="5"`, only values 0, 5, 10, 15 … are selectable.

---

## TickFrequency

Controls the spacing between displayed tick marks (from `Minimum` to `Maximum`). Default is `10` (11 ticks at 0, 10, 20 … 100).

```xaml
<!-- Show ticks every 20 units: 0, 20, 40, 60, 80, 100 -->
<syncfusion:SfRadialSlider TickFrequency="20" Name="radialSlider"/>
```

```csharp
radialSlider.TickFrequency = 20;
```

> `TickFrequency` controls **display** of ticks only. Use `SmallChange` to control the **selectable** step size independently.

---

## ShowMaximumValue

By default, the `Maximum` value is **not shown** if it is not an exact multiple of `TickFrequency`. Set `ShowMaximumValue="True"` to force it to appear.

```xaml
<!-- TickFrequency=9: ticks at 0,9,18...99 — max=100 won't appear by default -->
<syncfusion:SfRadialSlider TickFrequency="9"
                           ShowMaximumValue="True"
                           Name="radialSlider"/>
```

```csharp
radialSlider.TickFrequency  = 9;
radialSlider.ShowMaximumValue = true;
```

---

## StartAngle and EndAngle

Define the arc extent of the circular track. Both default to `0` / `360` (full circle).

| Setup | StartAngle | EndAngle |
|---|---|---|
| Full circle | `0` | `360` |
| Top half only | `180` | `360` |
| 270° arc | `90` | `360` |
| Custom arc | `90` | `330` |

```xaml
<syncfusion:SfRadialSlider StartAngle="90"
                           EndAngle="330"
                           Name="radialSlider"/>
```

```csharp
radialSlider.StartAngle = 90;
radialSlider.EndAngle   = 330;
```

> Angles are measured in degrees, starting from the top (12 o'clock = 0°) clockwise.

---

## SweepDirection

Controls whether the ticks increase clockwise or counterclockwise. Default is `Clockwise`.

```xaml
<syncfusion:SfRadialSlider SweepDirection="Counterclockwise"
                           StartAngle="180"
                           EndAngle="360"
                           Name="radialSlider"/>
```

```csharp
radialSlider.SweepDirection = SweepDirection.Counterclockwise;
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Max value not shown | Not a `TickFrequency` multiple | Set `ShowMaximumValue="True"` |
| Value jumps by large amounts | `SmallChange` is too large | Reduce `SmallChange` |
| Value snaps to wrong tick | `TickFrequency` ≠ `SmallChange` | Align them (e.g., both `= 5`) or set `SmallChange` smaller |
| Arc doesn't show full range | `StartAngle`/`EndAngle` not set | Default is 0/360; adjust as needed |
| `ContentTemplate` binding is empty | `DataContext` not set | Set `DataContext` on `SfRadialSlider` or parent |
