# Appearance & Customization

## Table of Contents
- [Arc Shape: StartAngle and EndAngle](#arc-shape-startangle-and-endangle)
- [Radius and Thickness](#radius-and-thickness)
- [Color Customization](#color-customization)
- [Range Colors](#range-colors)
- [Gradient](#gradient)
- [Corner Radius](#corner-radius)
- [Animation Duration](#animation-duration)
- [Animation Easing](#animation-easing)

---

## Arc Shape: StartAngle and EndAngle

By default, `SfCircularProgressBar` draws a full circle. Use `StartAngle` and `EndAngle` to create partial arcs like semi-circles or custom gauges.

**Semi-circle (bottom half):**
```xaml
<Syncfusion:SfCircularProgressBar Progress="75" StartAngle="180" EndAngle="360" />
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar { Progress = 75 };
circular.StartAngle = 180;
circular.EndAngle = 360;
grid.Children.Add(circular);
```

**Custom arc (270° span with rounded ends):**
```xaml
<Syncfusion:SfCircularProgressBar
    Progress="50"
    StartAngle="130"
    EndAngle="410"
    ShowProgressValue="False"
    Width="150" Height="150" />
```

Angles are in degrees; `0°` = right (3 o'clock), increasing clockwise. The default full circle uses `-90` to `270`.

---

## Radius and Thickness

Four radius properties control the size of the progress indicator and the background track. Values are expressed as a ratio (0.0–1.0) relative to the control size.

| Property | Controls |
|---|---|
| `IndicatorOuterRadius` | Outer edge of the progress arc |
| `IndicatorInnerRadius` | Inner edge of the progress arc (creates ring thickness) |
| `TrackOuterRadius` | Outer edge of the background track |
| `TrackInnerRadius` | Inner edge of the background track |

### Track progress outside (thin ring)
```xaml
<Syncfusion:SfCircularProgressBar
    Progress="80"
    IndicatorOuterRadius="0.7"
    IndicatorInnerRadius="0.65"
    ShowProgressValue="True" />
```

### Filled progress indicator (solid disc)
```xaml
<Syncfusion:SfCircularProgressBar
    Progress="80"
    IndicatorOuterRadius="0.7"
    IndicatorInnerRadius="0"
    ShowProgressValue="False" />
```

### Track inside the progress ring
```xaml
<Syncfusion:SfCircularProgressBar
    x:Name="CircularProgressBar"
    Progress="80"
    TrackOuterRadius="0.7"
    TrackInnerRadius="0">
    <Syncfusion:SfCircularProgressBar.ProgressContent>
        <TextBlock
            Text="{Binding Progress, StringFormat={}{0}%, ElementName=CircularProgressBar}"
            TextAlignment="Center" />
    </Syncfusion:SfCircularProgressBar.ProgressContent>
</Syncfusion:SfCircularProgressBar>
```

### Thin track style (separate track and indicator widths)
```xaml
<Syncfusion:SfCircularProgressBar
    Progress="70"
    IndicatorOuterRadius="0.75"
    IndicatorInnerRadius="0.6"
    TrackOuterRadius="0.7"
    TrackInnerRadius="0.65"
    ShowProgressValue="True" />
```

**Rule of thumb:** Keep `IndicatorOuterRadius` slightly larger than `TrackOuterRadius` so the progress arc visually overlays the track.

---

## Color Customization

Set solid colors for the progress arc and track:

```xaml
<Syncfusion:SfCircularProgressBar
    Progress="70"
    ProgressColor="LawnGreen"
    TrackColor="DarkOliveGreen" />
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
circular.Progress = 80;
circular.ProgressColor = new SolidColorBrush(Colors.LawnGreen);
circular.TrackColor = new SolidColorBrush(Colors.DarkOliveGreen);
grid.Children.Add(circular);
```

| Property | Purpose |
|---|---|
| `ProgressColor` | Color of the filled progress arc |
| `TrackColor` | Color of the background track arc |

---

## Range Colors

Map different colors to specific progress value ranges to create gauge-style visuals (e.g., green = safe, yellow = warning, red = critical):

```xaml
<Syncfusion:SfCircularProgressBar Progress="80">
    <Syncfusion:SfCircularProgressBar.RangeColors>
        <Syncfusion:RangeColorCollection>
            <Syncfusion:RangeColor Color="BlanchedAlmond" Start="5"  End="30" />
            <Syncfusion:RangeColor Color="Coral"          Start="30" End="60" />
            <Syncfusion:RangeColor Color="Crimson"        Start="60" End="100" />
        </Syncfusion:RangeColorCollection>
    </Syncfusion:SfCircularProgressBar.RangeColors>
</Syncfusion:SfCircularProgressBar>
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
RangeColorCollection rangeColors = new RangeColorCollection();
rangeColors.Add(new RangeColor { Color = Colors.BlanchedAlmond, Start = 5,  End = 30 });
rangeColors.Add(new RangeColor { Color = Colors.Coral,          Start = 30, End = 60 });
rangeColors.Add(new RangeColor { Color = Colors.Crimson,        Start = 60, End = 100 });
circular.Progress = 80;
circular.RangeColors = rangeColors;
grid.Children.Add(circular);
```

**`RangeColor` properties:**

| Property | Type | Description |
|---|---|---|
| `Color` | `Color` | Solid color for this range |
| `Start` | `double` | Range start value (0–100) |
| `End` | `double` | Range end value (0–100) |
| `IsGradient` | `bool` | Enable gradient transition (see below) |

**Use case:** CPU usage gauge — green (0–50), yellow (50–80), red (80–100).

---

## Gradient

Set `IsGradient="True"` on `RangeColor` entries to blend smoothly between range colors:

```xaml
<Syncfusion:SfCircularProgressBar Progress="80">
    <Syncfusion:SfCircularProgressBar.RangeColors>
        <Syncfusion:RangeColorCollection>
            <Syncfusion:RangeColor IsGradient="True" Color="SkyBlue"     Start="10" End="30" />
            <Syncfusion:RangeColor IsGradient="True" Color="DeepSkyBlue" Start="30" End="60" />
            <Syncfusion:RangeColor IsGradient="True" Color="Blue"        Start="60" End="100" />
        </Syncfusion:RangeColorCollection>
    </Syncfusion:SfCircularProgressBar.RangeColors>
</Syncfusion:SfCircularProgressBar>
```

```csharp
RangeColorCollection rangeColors = new RangeColorCollection();
rangeColors.Add(new RangeColor { IsGradient = true, Color = Colors.SkyBlue,     Start = 10, End = 30 });
rangeColors.Add(new RangeColor { IsGradient = true, Color = Colors.DeepSkyBlue, Start = 30, End = 60 });
rangeColors.Add(new RangeColor { IsGradient = true, Color = Colors.Blue,        Start = 60, End = 100 });
circular.RangeColors = rangeColors;
```

Mix `IsGradient="True"` and `IsGradient="False"` across ranges for partial gradient effects.

---

## Corner Radius

Round the ends of the progress arc with `IndicatorCornerRadius`:

```xaml
<Syncfusion:SfCircularProgressBar
    Width="150" Height="150"
    Progress="50"
    StartAngle="130"
    EndAngle="410"
    IndicatorCornerRadius="5"
    ShowProgressValue="False" />
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
circular.Width = 150;
circular.Height = 150;
circular.IndicatorCornerRadius = 5;
circular.Progress = 50;
circular.StartAngle = 130;
circular.EndAngle = 410;
grid.Children.Add(circular);
```

**Formula for correct value:**
```
IndicatorCornerRadius ≈ IndicatorOuterRadius × 10
```

Example: `IndicatorOuterRadius = 0.5` → `IndicatorCornerRadius = 5`

---

## Animation Duration

Controls how long one indeterminate animation cycle takes. Only applies when `IsIndeterminate="True"`. Default: `3000ms`.

```xaml
<Syncfusion:SfCircularProgressBar
    Width="150" Height="150"
    IsIndeterminate="True"
    Progress="50"
    ShowProgressValue="False"
    AnimationDuration="00:00:01" />
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
circular.Width = 150;
circular.Height = 150;
circular.AnimationDuration = new TimeSpan(0, 0, 1); // 1 second
circular.IsIndeterminate = true;
grid.Children.Add(circular);
```

---

## Animation Easing

Apply an easing function to the indeterminate animation for non-linear motion. Only applies when `IsIndeterminate="True"`.

```xaml
<Syncfusion:SfCircularProgressBar
    Width="150" Height="150"
    IsIndeterminate="True"
    ShowProgressValue="False">
    <Syncfusion:SfCircularProgressBar.AnimationEasing>
        <BounceEase Bounces="20" Bounciness="5" EasingMode="EaseOut" />
    </Syncfusion:SfCircularProgressBar.AnimationEasing>
</Syncfusion:SfCircularProgressBar>
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
circular.IsIndeterminate = true;
circular.AnimationEasing = new BounceEase
{
    Bounces = 20,
    Bounciness = 5,
    EasingMode = EasingMode.EaseOut
};
grid.Children.Add(circular);
```

Any WPF `EasingFunctionBase` works: `SineEase`, `QuadraticEase`, `CubicEase`, `ElasticEase`, `BounceEase`, `BackEase`, etc.

---

## Gotchas

- **`RangeColors` overrides `ProgressColor`** — when `RangeColors` is set, `ProgressColor` is ignored.
- **`IndicatorCornerRadius` formula** — using a value too large relative to the ring thickness causes visual clipping; use the `IndicatorOuterRadius × 10` formula as a guide.
- **`AnimationDuration` and `AnimationEasing`** apply only in indeterminate mode — they have no effect when `IsIndeterminate="False"`.
- **Radius values are relative (0.0–1.0)** — they scale proportionally with the control's `Width`/`Height`, not in pixels.
