---
name: syncfusion-wpf-linear-progressbar
description: Implement Syncfusion WPF SfLinearProgressBar for horizontal or vertical linear progress indicators. Use this when building determinate progress displays, indeterminate loading bars, buffer/secondary progress, or segmented progress bars in WPF. Covers RangeColors, gradient progress, IndicatorPadding, IndicatorCornerRadius, and IsIndeterminate using Syncfusion.SfProgressBar.WPF.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing SfLinearProgressBar (WPF)

## When to Use This Skill

Use this skill when:
- Displaying a horizontal progress bar for a known task (determinate)
- Showing an indeterminate bouncing animation while progress is unknown
- Visualizing primary + secondary (buffer) progress simultaneously
- Splitting progress into discrete segments
- Customizing colors, gradients, padding, or corner radius
- Applying range-based colors across progress value zones

**Assembly:** `Syncfusion.SfProgressBar.WPF`
**Namespace:** `Syncfusion.UI.Xaml.ProgressBar`
**XAML Schema:** `http://schemas.syncfusion.com/wpf`
**Key Class:** `SfLinearProgressBar`

---

## Quick Start

```xaml
<Window xmlns:Syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid x:Name="grid">
        <Syncfusion:SfLinearProgressBar
            Progress="70"
            Width="500"
            Height="20" />
    </Grid>
</Window>
```

```csharp
using Syncfusion.UI.Xaml.ProgressBar;

SfLinearProgressBar linear = new SfLinearProgressBar();
linear.Progress = 70;
linear.Width = 500;
linear.Height = 20;
grid.Children.Add(linear);
```

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet/assembly installation (`Syncfusion.SfProgressBar.WPF`)
- XAML namespace and schema declaration
- Declaring in XAML and code-behind
- Setting the `Progress` property
- Theme integration via SfSkinManager

### States
📄 **Read:** [references/states.md](references/states.md)
- Determinate state (default) for known progress
- Indeterminate state (`IsIndeterminate="True"`) for unknown duration
- Buffer state (`SecondaryProgress`, `SecondaryProgressColor`)
- Decision guide: which state to use when
- Switching states dynamically via MVVM binding

### Appearance & Segments
📄 **Read:** [references/appearance-and-segments.md](references/appearance-and-segments.md)
- `IndicatorCornerRadius` — rounded ends
- `IndicatorPadding` — gap between track and progress bar
- `ProgressColor` / `TrackColor` — solid color customization
- `RangeColors`: `RangeColor` with `Start`, `End`, `Color`
- Gradient: `RangeColor.IsGradient`
- `AnimationDuration` / `AnimationEasing` (indeterminate only)
- `SegmentCount` — split bar into sections
- Segments with corner radius

---

## Key Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Progress` | `double` | `0` | Current progress value (0–100) |
| `IsIndeterminate` | `bool` | `false` | Indeterminate bouncing animation |
| `SecondaryProgress` | `double` | `0` | Buffer/secondary progress value |
| `SecondaryProgressColor` | `Brush` | theme | Color of the buffer bar |
| `ProgressColor` | `Brush` | theme | Color of the progress bar |
| `TrackColor` | `Brush` | theme | Color of the background track |
| `IndicatorCornerRadius` | `double` | `0` | Rounded ends on progress bar |
| `IndicatorPadding` | `Thickness` | `0` | Gap between track and indicator |
| `SegmentCount` | `int` | `0` | Number of segments (0 = none) |
| `AnimationDuration` | `TimeSpan` | `3000ms` | Indeterminate animation cycle time |

---

## Common Use Cases

- **File download bar** — determinate with `Progress` bound to download percentage
- **Loading animation** — indeterminate (`IsIndeterminate="True"`)
- **Video buffering** — `SecondaryProgress` ahead of `Progress`
- **Step tracker** — `SegmentCount` matching total steps
- **Health/battery bar** — `RangeColors` mapping green/yellow/red zones
- **Styled progress pill** — `IndicatorCornerRadius="10"` for rounded bar
