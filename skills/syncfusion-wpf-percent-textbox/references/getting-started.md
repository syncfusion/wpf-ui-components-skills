# Getting Started (summary)

This abbreviated guide shows the minimal steps to add and use `PercentTextBox`:

- Install `Syncfusion.Shared.WPF` (NuGet) and reference the `http://schemas.syncfusion.com/wpf` XML namespace.
- Add `PercentTextBox` in XAML inside a `Window` or panel; prefer `PercentValue` for numeric binding instead of `Text`.
- For quick setup, set `MinValue`/`MaxValue` and `PercentValue` to constrain and initialize display.

Minimal XAML:

```xml
<syncfusion:PercentTextBox Width="150" Height="30" PercentValue="50" MinValue="0" MaxValue="100"/>
```

Key tips:
- Use `UpdateSourceTrigger=PropertyChanged` for view-model synchronization when needed.
- Enable `UseNullOption` with `NullValue` and `WatermarkText` to show placeholders for empty values.
- Prefer dedicated reference files in `references/` for advanced formatting, validation, and appearance scenarios.

See `references/getting-started.md` (this file) for the concise checklist and the other `references/` files for detail.
