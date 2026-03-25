# Ticks, Labels, and Templates

## Table of Contents
- [Tick Radius and Visibility](#tick-radius-and-visibility)
- [TickTemplate](#ticktemplate)
- [Label Radius and Visibility](#label-radius-and-visibility)
- [LabelTemplate](#labeltemplate)
- [DrawLabel Event — Per-Label Formatting](#drawlabel-event--per-label-formatting)
- [Inner Rim Radius](#inner-rim-radius)
- [Outer Rim Radius](#outer-rim-radius)
- [Pointer Radius](#pointer-radius)
- [PointerStyle](#pointerstyle)
- [PreviewPointerStyle](#previewpointerstyle)
- [Gotchas](#gotchas)

---

## Tick Radius and Visibility

`TickRadiusFactor` sets the radial position of tick marks (0–1, relative to the control size). Default is `0.72`.  
`TickVisibility` shows or hides tick marks. Default is `Visible`.

```xaml
<!-- Move ticks closer to center, keep visible -->
<syncfusion:SfRadialSlider TickRadiusFactor="0.5"
                           TickVisibility="Visible"
                           Name="radialSlider"/>
```

```csharp
radialSlider.TickRadiusFactor = 0.5;
radialSlider.TickVisibility   = Visibility.Visible;
```

To hide ticks entirely:
```xaml
<syncfusion:SfRadialSlider TickVisibility="Hidden"/>
```

---

## TickTemplate

Replace the default tick mark with a custom `DataTemplate`. The `DataContext` is the tick value index (integer count).

```xaml
<!-- Red square ticks -->
<syncfusion:SfRadialSlider Name="radialSlider">
    <syncfusion:SfRadialSlider.TickTemplate>
        <DataTemplate>
            <Border Background="Red"
                    Width="6" Height="6"/>
        </DataTemplate>
    </syncfusion:SfRadialSlider.TickTemplate>
</syncfusion:SfRadialSlider>
```

Custom tick with a circle:
```xaml
<syncfusion:SfRadialSlider.TickTemplate>
    <DataTemplate>
        <Ellipse Fill="DarkBlue" Width="5" Height="5"/>
    </DataTemplate>
</syncfusion:SfRadialSlider.TickTemplate>
```

---

## Label Radius and Visibility

`LabelRadiusFactor` sets the radial position of tick labels (0–1). Default is `0.87`.  
`LabelVisibility` shows or hides tick labels. Default is `Visible`.

```xaml
<!-- Move labels closer to center -->
<syncfusion:SfRadialSlider LabelRadiusFactor="0.6"
                           LabelVisibility="Visible"
                           Name="radialSlider"/>
```

```csharp
radialSlider.LabelRadiusFactor = 0.6;
radialSlider.LabelVisibility   = Visibility.Visible;
```

To hide labels:
```xaml
<syncfusion:SfRadialSlider LabelVisibility="Hidden"/>
```

---

## LabelTemplate

Replace the default tick label text with a custom `DataTemplate`. The `DataContext` is the tick value (double).

```xaml
<!-- Red text on yellow background -->
<syncfusion:SfRadialSlider Name="radialSlider">
    <syncfusion:SfRadialSlider.LabelTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}"
                       Foreground="Red"
                       Background="Yellow"
                       FontSize="9"/>
        </DataTemplate>
    </syncfusion:SfRadialSlider.LabelTemplate>
</syncfusion:SfRadialSlider>
```

Add a unit suffix:
```xaml
<syncfusion:SfRadialSlider.LabelTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="{Binding}" FontSize="9"/>
            <TextBlock Text="°" FontSize="9"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:SfRadialSlider.LabelTemplate>
```

> Use `LabelTemplate` for static formatting. For **dynamic** per-value formatting (different colors per range), use the `DrawLabel` event instead.

---

## DrawLabel Event — Per-Label Formatting

Handle `DrawLabel` to customize individual tick labels based on their value. Set `e.Handled = true` to apply changes.

**Available `DrawLabelEventArgs` properties:**

| Property | Description |
|---|---|
| `e.Value` | The numeric tick value |
| `e.Text` | Label text (modify to change display) |
| `e.Foreground` | Label text color |
| `e.FontFamily` | Label font family |
| `e.FontSize` | Label font size |
| `e.Handled` | Set `true` to apply modifications |

### Example: Color-Coded Temperature Labels

```xaml
<syncfusion:SfRadialSlider DrawLabel="sfRadialSlider_DrawLabel"
                           Name="sfRadialSlider"
                           Height="200" Width="200">
    <TextBlock Text="{Binding ElementName=sfRadialSlider, Path=Value}"
               FontSize="15"
               HorizontalAlignment="Center"
               VerticalAlignment="Center"/>
</syncfusion:SfRadialSlider>
```

```csharp
private void sfRadialSlider_DrawLabel(object sender, DrawLabelEventArgs e)
{
    e.Handled = true;
    e.Text += "°C";                          // append unit to all labels

    if (e.Value <= 33)
    {
        e.FontSize   = 8;
        e.FontFamily = new FontFamily("Arial");
        e.Foreground = Brushes.Green;
    }
    else if (e.Value > 33 && e.Value <= 66)
    {
        e.FontSize   = 10;
        e.FontFamily = new FontFamily("Courier");
        e.Foreground = Brushes.Gold;
    }
    else
    {
        e.FontSize   = 12;
        e.FontFamily = new FontFamily("Georgia");
        e.Foreground = Brushes.Red;
    }
}
```

> If `e.Handled` is NOT set to `true`, changes to `DrawLabelEventArgs` are ignored.

---

## Inner Rim Radius

`InnerRimRadiusFactor` controls the size of the center circle (0–1). Default is `0.2`.

```xaml
<!-- Larger inner circle for more content space -->
<syncfusion:SfRadialSlider InnerRimRadiusFactor="0.5"
                           Name="radialSlider"/>
```

```csharp
radialSlider.InnerRimRadiusFactor = 0.5;
```

---

## Outer Rim Radius

`OuterRimRadiusFactor` controls how far the circular track extends from center (0–1). Default is `0.7`.

```xaml
<!-- Wider outer rim track -->
<syncfusion:SfRadialSlider OuterRimRadiusFactor="0.85"
                           Name="radialSlider"/>
```

```csharp
radialSlider.OuterRimRadiusFactor = 0.85;
```

> Ensure `OuterRimRadiusFactor` > `InnerRimRadiusFactor` to maintain a visible track band.

---

## Pointer Radius

`PointerRadiusFactor` controls where the selection pointer sits on the track (0–1). Default is `0.75`.

```xaml
<syncfusion:SfRadialSlider PointerRadiusFactor="0.5"
                           Name="radialSlider"/>
```

```csharp
radialSlider.PointerRadiusFactor = 0.5;
```

---

## PointerStyle

Customize the selection pointer using a `Style` with `TargetType="syncfusion:RadialPointer"`:

```xaml
<syncfusion:SfRadialSlider Name="radialSlider">
    <syncfusion:SfRadialSlider.PointerStyle>
        <Style TargetType="syncfusion:RadialPointer">
            <Setter Property="Height" Value="3"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="syncfusion:RadialPointer">
                        <Border Background="DarkRed"
                                CornerRadius="2"/>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfRadialSlider.PointerStyle>
</syncfusion:SfRadialSlider>
```

---

## PreviewPointerStyle

Customize the hover preview pointer using `TargetType="syncfusion:RadialPreviewPointer"`:

```xaml
<syncfusion:SfRadialSlider Name="radialSlider">
    <syncfusion:SfRadialSlider.PreviewPointerStyle>
        <Style TargetType="syncfusion:RadialPreviewPointer">
            <Setter Property="Height" Value="3"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="syncfusion:RadialPreviewPointer">
                        <Border Opacity="0.3"
                                Background="DarkRed"
                                CornerRadius="2"/>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfRadialSlider.PreviewPointerStyle>
</syncfusion:SfRadialSlider>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `DrawLabel` changes have no effect | `e.Handled` not set | Always set `e.Handled = true` |
| `LabelTemplate` binding is null | `{Binding}` with no path in a non-data context | `DataContext` of `LabelTemplate` is the tick value — use `{Binding}` (no path) |
| Pointer overlaps labels | `PointerRadiusFactor` ≈ `LabelRadiusFactor` | Separate them (e.g., pointer `0.65`, labels `0.87`) |
| Ticks invisible | `TickVisibility="Hidden"` or factor too small | Set `TickVisibility="Visible"` and `TickRadiusFactor` between `0.5`–`0.8` |
| `TickTemplate` DataContext is index, not value | By design | Use `LabelTemplate` or `DrawLabel` if you need the actual numeric value |
