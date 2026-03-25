# States

## Overview

`SfLinearProgressBar` supports three states that control how progress is displayed. Choose based on whether the progress amount is known, unknown, or has a secondary buffer component.

---

## Decision Guide

| Scenario | State to Use |
|---|---|
| Progress percentage is known | **Determinate** (default) |
| Progress duration is unknown | **Indeterminate** |
| Download/stream with buffering | **Buffer** |

---

## Determinate

**Default state.** Displays a bar that fills proportionally to the `Progress` value (0–100).

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    Width="500"
    Height="20" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar();
linear.Progress = 70;
linear.Width = 500;
linear.Height = 20;
grid.Children.Add(linear);
```

**Use when:** File upload, installation, form completion, or any task where progress percentage can be calculated.

---

## Indeterminate

When the progress duration cannot be estimated, enable `IsIndeterminate` to show a continuous bouncing animation:

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    IsIndeterminate="True"
    Width="500"
    Height="20" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar
{
    Progress = 70,
    IsIndeterminate = true,
    Width = 500,
    Height = 20
};
grid.Children.Add(linear);
```

**Use when:** API calls, database queries, background processing where total duration is unknown.

**Animation control (indeterminate only):**
- `AnimationDuration` — one full cycle duration (default: `3000ms`)
- `AnimationEasing` — easing function applied to animation

```xaml
<Syncfusion:SfLinearProgressBar
    IsIndeterminate="True"
    AnimationDuration="00:00:01"
    Width="250" Height="4" />
```

See `appearance-and-segments.md` for full animation examples.

---

## Buffer

Displays two bars simultaneously — a primary progress and a secondary (buffer) bar. Useful for video streaming or downloads where data loads ahead of playback.

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    SecondaryProgress="90"
    Width="500"
    Height="20" />
```

```csharp
SfLinearProgressBar linear = new SfLinearProgressBar
{
    Progress = 70,
    SecondaryProgress = 90,
    Width = 500,
    Height = 20
};
grid.Children.Add(linear);
```

**Customize buffer color:**
```xaml
<Syncfusion:SfLinearProgressBar
    Progress="70"
    SecondaryProgress="90"
    SecondaryProgressColor="LightBlue"
    Width="500" Height="20" />
```

| Property | Purpose |
|---|---|
| `Progress` | Primary (foreground) bar (0–100) |
| `SecondaryProgress` | Buffer (background) bar (0–100, typically ≥ `Progress`) |
| `SecondaryProgressColor` | Color of the buffer bar |

---

## Switching States Dynamically

Bind `IsIndeterminate` and `Progress` to ViewModel properties for runtime state switching:

```xaml
<Syncfusion:SfLinearProgressBar
    Progress="{Binding UploadProgress}"
    IsIndeterminate="{Binding IsLoading}"
    Width="500" Height="20" />
```

```csharp
// Start indeterminate while connecting
IsLoading = true;
await ConnectAsync();

// Switch to determinate once progress is trackable
IsLoading = false;
for (int i = 0; i <= 100; i++)
{
    UploadProgress = i;
    await Task.Delay(50);
}
```

---

## Gotchas

- **`IsIndeterminate` ignores `Progress`** — the bar animates regardless of the `Progress` value when indeterminate.
- **`SecondaryProgress` should be ≥ `Progress`** — setting it lower results in overlapping bars.
- **Indeterminate animation continues even when hidden** — stop animation by setting `IsIndeterminate="False"` when the control is not visible to avoid unnecessary CPU usage.
