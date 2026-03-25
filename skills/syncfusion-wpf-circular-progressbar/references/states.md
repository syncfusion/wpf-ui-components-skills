# States

## Overview

`SfCircularProgressBar` supports three states that control how progress is visualized. Choose the state based on whether the progress amount is known, unknown, or has a secondary (buffer) component.

---

## Decision Guide

| Scenario | State to Use |
|---|---|
| Progress percentage is known | **Determinate** (default) |
| Progress duration is unknown | **Indeterminate** |
| Download with buffering (primary + buffer) | **Buffer** |

---

## Determinate

**Default state.** Displays a fixed arc that fills proportionally to the `Progress` value (0–100).

Enable the numeric percentage display with `ShowProgressValue`:

```xaml
<Syncfusion:SfCircularProgressBar
    Progress="70"
    ShowProgressValue="True"
    Width="150" Height="150" />
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
circular.Progress = 70;
circular.ShowProgressValue = true;
grid.Children.Add(circular);
```

**Use when:** File upload, installation steps, form completion, or any task where progress percentage can be calculated.

**Note:** `ShowProgressValue="True"` renders the percentage text in the center. If you want custom center content instead, use `ProgressContent` (see `segments-and-custom-content.md`) and set `ShowProgressValue="False"`.

---

## Indeterminate

When the progress duration cannot be estimated, enable `IsIndeterminate` to show a continuous spinning animation:

```xaml
<Syncfusion:SfCircularProgressBar
    Progress="70"
    IsIndeterminate="True"
    ShowProgressValue="False"
    Width="150" Height="150" />
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
circular.Progress = 70;
circular.IsIndeterminate = true;
grid.Children.Add(circular);
```

**Use when:** API calls, database queries, background operations where total duration is unknown.

**Animation control:**
- `AnimationDuration` — sets one full cycle time (default: `3000ms`)
- `AnimationEasing` — applies easing function to the animation

```xaml
<Syncfusion:SfCircularProgressBar IsIndeterminate="True" AnimationDuration="00:00:01" />
```

See `appearance.md` for full `AnimationDuration` and `AnimationEasing` examples.

---

## Buffer

Displays two concentric progress arcs simultaneously — a primary progress and a secondary (buffer) progress. Useful for media streaming, downloads, or any task with pre-loading ahead.

```xaml
<Syncfusion:SfCircularProgressBar
    Progress="65"
    SecondaryProgress="75"
    Width="150" Height="150" />
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
circular.Progress = 65;
circular.SecondaryProgress = 75;
grid.Children.Add(circular);
```

**Customize buffer color:**
```xaml
<Syncfusion:SfCircularProgressBar
    Progress="65"
    SecondaryProgress="75"
    SecondaryProgressColor="LightBlue" />
```

| Property | Purpose |
|---|---|
| `Progress` | Primary progress arc (0–100) |
| `SecondaryProgress` | Buffer arc (0–100, typically ≥ `Progress`) |
| `SecondaryProgressColor` | Color of the buffer arc |

**Use when:** Video buffering, file download pre-caching, or any scenario where a "ready ahead" indicator is needed alongside current progress.

---

## Switching States Dynamically

Toggle states at runtime by binding `IsIndeterminate`:

```xaml
<Syncfusion:SfCircularProgressBar
    Progress="{Binding UploadProgress}"
    IsIndeterminate="{Binding IsLoading}"
    ShowProgressValue="{Binding IsNotLoading}" />
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

- **`IsIndeterminate` ignores `Progress`** — the arc animates continuously regardless of the `Progress` value when indeterminate.
- **`ShowProgressValue` and `ProgressContent` conflict** — if both are set, `ProgressContent` takes visual precedence; set `ShowProgressValue="False"` when using custom content.
- **`SecondaryProgress` should be ≥ `Progress`** — setting it lower than `Progress` results in overlapping arcs that look incorrect.
