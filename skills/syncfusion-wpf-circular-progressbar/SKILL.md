---
name: syncfusion-wpf-circular-progressbar
description: Implement Syncfusion WPF SfCircularProgressBar for circular and radial progress indicators. Use this when building circular progress displays, determinate or indeterminate progress animations, buffer/secondary progress, segmented progress rings, or arc-shaped progress with custom center content. Covers RangeColors, StartAngle, EndAngle, SegmentCount, and ProgressContent using Syncfusion.SfProgressBar.WPF.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing SfCircularProgressBar (WPF)

## When to Use This Skill

Use this skill when:
- Displaying circular/radial progress for a known task (determinate)
- Showing an indeterminate spinner while progress is unknown
- Visualizing primary + secondary (buffer) progress simultaneously
- Splitting progress into segments (e.g., steps in a workflow)
- Customizing arc angles (semi-circle, full circle, partial arc)
- Styling progress ring colors, gradients, or range-based colors
- Placing custom content (text, buttons, images) at the center of the ring

**Assembly:** `Syncfusion.SfProgressBar.WPF`
**Namespace:** `Syncfusion.UI.Xaml.ProgressBar`
**XAML Schema:** `http://schemas.syncfusion.com/wpf`
**Key Class:** `SfCircularProgressBar`

---

## Quick Start

```xaml
<Window
    xmlns:Syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <!-- Determinate: shows 70% progress -->
        <Syncfusion:SfCircularProgressBar
            Progress="70"
            ShowProgressValue="True"
            Width="150" Height="150" />
    </Grid>
</Window>
```

```csharp
using Syncfusion.UI.Xaml.ProgressBar;

SfCircularProgressBar circular = new SfCircularProgressBar();
circular.Progress = 70;
circular.ShowProgressValue = true;
circular.Width = 150;
grid.Children.Add(circular);
```

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet/assembly installation (`Syncfusion.SfProgressBar.WPF`)
- XAML namespace and schema declaration
- Declaring the control in XAML and code-behind
- Setting the `Progress` property
- Theme integration via SfSkinManager

### States
📄 **Read:** [references/states.md](references/states.md)
- Determinate state (default) with `ShowProgressValue`
- Indeterminate state (`IsIndeterminate="True"`) for unknown progress
- Buffer state (`SecondaryProgress`, `SecondaryProgressColor`)
- Decision guide: which state to use when

### Appearance & Customization
📄 **Read:** [references/appearance.md](references/appearance.md)
- Arc shape via `StartAngle` / `EndAngle`
- Radius and thickness: `IndicatorOuterRadius`, `IndicatorInnerRadius`, `TrackOuterRadius`, `TrackInnerRadius`
- Solid color: `ProgressColor`, `TrackColor`
- Range-based colors: `RangeColor` with `Start`, `End`, `Color`
- Gradient effect: `RangeColor.IsGradient`
- Rounded ends: `IndicatorCornerRadius`
- Animation: `AnimationDuration`, `AnimationEasing`

### Segments & Custom Content
📄 **Read:** [references/segments-and-custom-content.md](references/segments-and-custom-content.md)
- `SegmentCount` to split ring into discrete steps
- `ProgressContent` for center UI (text, buttons, images)
- Binding center text to `Progress` value
- Play/Pause button with `DispatcherTimer` pattern

---

## Key Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Progress` | `double` | `0` | Current progress value (0–100) |
| `ShowProgressValue` | `bool` | `false` | Display numeric % in center |
| `IsIndeterminate` | `bool` | `false` | Indeterminate spinning animation |
| `SecondaryProgress` | `double` | `0` | Buffer/secondary progress value |
| `StartAngle` | `double` | `-90` | Arc start angle in degrees |
| `EndAngle` | `double` | `270` | Arc end angle in degrees |
| `SegmentCount` | `int` | `0` | Number of segments (0 = none) |
| `ProgressColor` | `Brush` | theme | Color of the progress arc |
| `TrackColor` | `Brush` | theme | Color of the track arc |
| `IndicatorCornerRadius` | `double` | `0` | Rounded ends on the progress arc |
| `AnimationDuration` | `TimeSpan` | `3000ms` | Indeterminate animation cycle time |
| `ProgressContent` | `object` | `null` | Custom center content |

---

## Common Use Cases

- **File upload indicator** — determinate with `ShowProgressValue="True"`
- **Loading spinner** — indeterminate (`IsIndeterminate="True"`)
- **Media player** — `ProgressContent` with play/pause button
- **Step tracker** — `SegmentCount` matching total steps
- **CPU/memory gauge** — `RangeColors` mapping green/yellow/red zones
- **Semi-circle gauge** — `StartAngle="180"` `EndAngle="360"`
