# Appearance & Segments

## Table of Contents
- [Corner Radius](#corner-radius)
- [Padding](#padding)
- [Color Customization](#color-customization)
- [Range Colors](#range-colors)
- [Gradient](#gradient)
- [Animation Duration](#animation-duration)
- [Animation Easing](#animation-easing)
- [Segments](#segments)
- [Segments with Corner Radius](#segments-with-corner-radius)

---

## Corner Radius

Round the ends of the progress bar with `IndicatorCornerRadius`:

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    IndicatorCornerRadius="10"
    Width="500" Height="20" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar
{
    Progress = 70,
    IndicatorCornerRadius = 10,
    Width = 500,
    Height = 20
};
grid.Children.Add(linear);
```

**Tip:** Set `IndicatorCornerRadius` to half the `Height` value for a fully pill-shaped bar. Example: `Height="20"` → `IndicatorCornerRadius="10"`.

---

## Padding

`IndicatorPadding` creates a gap between the outer track bar and the inner progress indicator — useful for an inset-style visual:

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    IndicatorPadding="5"
    Width="500" Height="20" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar();
linear.IndicatorPadding = new Thickness(5);
linear.Progress = 70;
linear.Width = 500;
linear.Height = 20;
grid.Children.Add(linear);
```

Combine with `IndicatorCornerRadius` for a rounded inset-style bar:

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    IndicatorPadding="3"
    IndicatorCornerRadius="8"
    Width="500" Height="20" />
```

---

## Color Customization

Set solid colors for the progress indicator and track:

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    ProgressColor="LawnGreen"
    TrackColor="DarkOliveGreen"
    Width="500" Height="20" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar();
linear.Progress = 70;
linear.ProgressColor = new SolidColorBrush(Colors.LawnGreen);
linear.TrackColor = new SolidColorBrush(Colors.DarkOliveGreen);
grid.Children.Add(linear);
```

| Property | Purpose |
|---|---|
| `ProgressColor` | Color of the filled progress bar |
| `TrackColor` | Color of the background track |

---

## Range Colors

Map different colors to specific progress value ranges for gauge-style visuals (e.g., green = safe, yellow = warning, red = critical):

```xaml
<Syncfusion:SfLinearProgressBar Progress="80" Width="500" Height="20">
    <Syncfusion:SfLinearProgressBar.RangeColors>
        <Syncfusion:RangeColorCollection>
            <Syncfusion:RangeColor Color="BlanchedAlmond" Start="10" End="30" />
            <Syncfusion:RangeColor Color="Coral"          Start="30" End="60" />
            <Syncfusion:RangeColor Color="Crimson"        Start="60" End="100" />
        </Syncfusion:RangeColorCollection>
    </Syncfusion:SfLinearProgressBar.RangeColors>
</Syncfusion:SfLinearProgressBar>
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar();
RangeColorCollection rangeColors = new RangeColorCollection();
rangeColors.Add(new RangeColor { Color = Colors.BlanchedAlmond, Start = 5,  End = 30 });
rangeColors.Add(new RangeColor { Color = Colors.Coral,          Start = 30, End = 60 });
rangeColors.Add(new RangeColor { Color = Colors.Crimson,        Start = 60, End = 100 });
linear.RangeColors = rangeColors;
linear.Progress = 80;
linear.Width = 500;
linear.Height = 20;
grid.Children.Add(linear);
```

**`RangeColor` properties:**

| Property | Type | Description |
|---|---|---|
| `Color` | `Color` | Solid color for this range segment |
| `Start` | `double` | Range start value (0–100) |
| `End` | `double` | Range end value (0–100) |
| `IsGradient` | `bool` | Enable smooth color transition |

---

## Gradient

Set `IsGradient="True"` on `RangeColor` entries to blend smoothly between range colors:

```xaml
<Syncfusion:SfLinearProgressBar Progress="80" Width="500" Height="20">
    <Syncfusion:SfLinearProgressBar.RangeColors>
        <Syncfusion:RangeColorCollection>
            <Syncfusion:RangeColor IsGradient="True" Color="SkyBlue"     Start="10" End="30" />
            <Syncfusion:RangeColor IsGradient="True" Color="DeepSkyBlue" Start="30" End="60" />
            <Syncfusion:RangeColor IsGradient="True" Color="Blue"        Start="60" End="100" />
        </Syncfusion:RangeColorCollection>
    </Syncfusion:SfLinearProgressBar.RangeColors>
</Syncfusion:SfLinearProgressBar>
```

```csharp
RangeColorCollection rangeColors = new RangeColorCollection();
rangeColors.Add(new RangeColor { IsGradient = true, Color = Colors.SkyBlue,     Start = 10, End = 30 });
rangeColors.Add(new RangeColor { IsGradient = true, Color = Colors.DeepSkyBlue, Start = 30, End = 60 });
rangeColors.Add(new RangeColor { IsGradient = true, Color = Colors.Blue,        Start = 60, End = 100 });
linear.RangeColors = rangeColors;
```

---

## Animation Duration

Controls how long one indeterminate animation cycle takes. Only applies when `IsIndeterminate="True"`. Default: `3000ms`.

```xaml
<Syncfusion:SfLinearProgressBar
    IsIndeterminate="True"
    AnimationDuration="00:00:01"
    Width="250" Height="4" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar();
linear.IsIndeterminate = true;
linear.AnimationDuration = new TimeSpan(0, 0, 1); // 1 second
linear.Width = 250;
linear.Height = 4;
grid.Children.Add(linear);
```

---

## Animation Easing

Apply a WPF easing function to the indeterminate animation. Only applies when `IsIndeterminate="True"`.

```xaml
<Syncfusion:SfLinearProgressBar
    IsIndeterminate="True"
    IndicatorCornerRadius="10"
    Width="250" Height="4">
    <Syncfusion:SfLinearProgressBar.AnimationEasing>
        <BounceEase Bounces="20" Bounciness="5" EasingMode="EaseOut" />
    </Syncfusion:SfLinearProgressBar.AnimationEasing>
</Syncfusion:SfLinearProgressBar>
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar();
linear.IsIndeterminate = true;
linear.AnimationEasing = new BounceEase
{
    Bounces = 20,
    Bounciness = 5,
    EasingMode = EasingMode.EaseOut
};
grid.Children.Add(linear);
```

Any WPF `EasingFunctionBase` works: `SineEase`, `QuadraticEase`, `CubicEase`, `ElasticEase`, `BounceEase`, `BackEase`, etc.

---

## Segments

Split the bar into discrete visual sections separated by gaps using `SegmentCount`:

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    SegmentCount="4"
    Width="500" Height="20" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar();
linear.Progress = 70;
linear.SegmentCount = 4;
linear.Width = 500;
linear.Height = 20;
grid.Children.Add(linear);
```

- `SegmentCount="4"` — divides the bar into 4 equal sections with gaps
- `SegmentCount="0"` — no segmentation (default, continuous bar)
- Progress fills segments sequentially as `Progress` value increases

**Use cases:**
- Wi-Fi signal strength (4 bars)
- Battery indicator (5 bars)
- Step-based upload progress

---

## Segments with Corner Radius

Combine `SegmentCount` and `IndicatorCornerRadius` for rounded segmented bars:

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    SegmentCount="4"
    IndicatorCornerRadius="10"
    Width="500" Height="20" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar();
linear.Progress = 70;
linear.SegmentCount = 4;
linear.IndicatorCornerRadius = 10;
linear.Width = 500;
linear.Height = 20;
grid.Children.Add(linear);
```

Each segment gets rounded ends individually, giving a pill-per-segment appearance.

---

## Gotchas

- **`RangeColors` overrides `ProgressColor`** — when `RangeColors` is set, `ProgressColor` is ignored.
- **`AnimationDuration` and `AnimationEasing`** only apply in indeterminate mode.
- **`IndicatorPadding` reduces effective bar height** — account for padding when calculating visible indicator thickness.
- **`SegmentCount` is visual only** — the `Progress` scale remains 0–100; segments are cosmetic divisions.
