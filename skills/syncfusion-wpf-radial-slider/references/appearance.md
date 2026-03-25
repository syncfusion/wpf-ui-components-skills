# Appearance and Styling

## Table of Contents
- [Foreground](#foreground)
- [Background (Outer Rim Fill)](#background-outer-rim-fill)
- [Inner Rim Appearance](#inner-rim-appearance)
- [Outer Rim Stroke](#outer-rim-stroke)
- [FlowDirection (RTL)](#flowdirection-rtl)
- [Themes with SfSkinManager](#themes-with-sfskinmanager)
- [Gotchas](#gotchas)

---

## Foreground

Changes the color of the tick labels and pointer. Default is `Black`.

```xaml
<syncfusion:SfRadialSlider Foreground="DarkBlue"
                           Name="radialSlider"/>
```

```csharp
radialSlider.Foreground = Brushes.DarkBlue;
```

---

## Background (Outer Rim Fill)

`Background` sets the fill color of the outer rim (circular track area). Default is `White`.

```xaml
<syncfusion:SfRadialSlider Background="LightSteelBlue"
                           Name="radialSlider"/>
```

```csharp
radialSlider.Background = Brushes.LightSteelBlue;
```

---

## Inner Rim Appearance

Customize the center circle's fill color, border color, and border thickness.

| Property | Default | Description |
|---|---|---|
| `InnerRimFill` | (theme) | Background fill of the inner circle |
| `InnerRimStroke` | `LightSlateGray` | Border color of the inner circle |
| `InnerRimStrokeThickness` | `2` | Border thickness of the inner circle |

```xaml
<syncfusion:SfRadialSlider InnerRimFill="LightYellow"
                           InnerRimStroke="OrangeRed"
                           InnerRimStrokeThickness="3"
                           Name="radialSlider"/>
```

```csharp
radialSlider.InnerRimFill             = Brushes.LightYellow;
radialSlider.InnerRimStroke           = Brushes.OrangeRed;
radialSlider.InnerRimStrokeThickness  = 3;
```

---

## Outer Rim Stroke

Customize the border of the outer circular track.

| Property | Default | Description |
|---|---|---|
| `OuterRimStroke` | `RosyBrown` | Border color of the outer rim |
| `OuterRimStrokeThickness` | `2` | Border thickness of the outer rim |

```xaml
<syncfusion:SfRadialSlider Background="Lavender"
                           OuterRimStroke="Purple"
                           OuterRimStrokeThickness="3"
                           Name="radialSlider"/>
```

```csharp
radialSlider.Background              = Brushes.Lavender;
radialSlider.OuterRimStroke          = Brushes.Purple;
radialSlider.OuterRimStrokeThickness = 3;
```

---

## FlowDirection (RTL)

Flip the layout for right-to-left cultures. Default is `LeftToRight`.

```xaml
<syncfusion:SfRadialSlider FlowDirection="RightToLeft"
                           Name="radialSlider"/>
```

```csharp
radialSlider.FlowDirection = FlowDirection.RightToLeft;
```

---

## Themes with SfSkinManager

Apply a built-in Syncfusion theme to the entire control:

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<syncfusion:SfRadialSlider
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentLight}"
    Height="200" Width="200"/>
```

Apply to the whole window:
```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialDark}">
```

**Available themes:**

| Theme Name | Style |
|---|---|
| `FluentLight` | Windows 11 Fluent light |
| `FluentDark` | Windows 11 Fluent dark |
| `MaterialLight` | Material Design light |
| `MaterialDark` | Material Design dark |
| `Office2019Colorful` | Office 2019 colorful |
| `Office2019Black` | Office 2019 black |
| `Windows11Light` | Windows 11 light |
| `Windows11Dark` | Windows 11 dark |
| `SystemTheme` | Matches OS system theme |

Custom themes via ThemeStudio: export a resource dictionary and reference it in `App.xaml`.

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `InnerRimFill` not visible | Theme overrides default | Set `InnerRimFill` explicitly after theme is applied |
| `Foreground` changes only labels | `Foreground` does not affect rim colors | Use `InnerRimFill`/`Background` for rim colors separately |
| `OuterRimStroke` has no effect | Theme may override it | Try setting after `SfSkinManager.Theme` is applied |
| RTL layout looks mirrored | Expected — `FlowDirection="RightToLeft"` mirrors the whole control | Intended; combine with `SweepDirection="Counterclockwise"` if needed |
