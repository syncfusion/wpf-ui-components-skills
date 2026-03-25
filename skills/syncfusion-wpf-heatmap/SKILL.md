---
name: syncfusion-wpf-heatmap
description: Implement Syncfusion WPF Heat Map (SfHeatMap) control for matrix-based data visualization using color gradients. Use this when visualizing tabular data with color-coded cells, creating heat map charts, or displaying data density patterns. This skill covers data mapping, color palette configuration, legend customization, and cell label formatting for WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing SfHeatMap (WPF)

## Table of Contents
- When to use this skill
- Control overview → [references/overview.md](references/overview.md)
- Quick start → [references/getting-started.md](references/getting-started.md)
- Color mapping → [references/color-mapping.md](references/color-mapping.md)
- Items mapping → [references/items-mapping.md](references/items-mapping.md)
- Legend & visualization → [references/legend.md](references/legend.md)
- Common patterns
- Troubleshooting

## When to use this skill

Use this skill when you need an actionable, copy-pasteable guide to integrate and configure the Syncfusion `SfHeatMap` control in a WPF app. This is focused on implementation (what to configure and why), not exhaustive API reference.

## Control overview

Read: [references/overview.md](references/overview.md)

Key points:
- Visualizes tabular data as color gradients
- Supports `TableMapping` and `CellMapping` data models
- Useful for spotting patterns and value ranges quickly

## Quick start

Read: [references/getting-started.md](references/getting-started.md)

What to do first:
- Add `SfHeatMap` to XAML
- Provide an `ItemsSource` and an `ItemsMapping` (TableMapping or CellMapping)
- Add `ColorMappingCollection` for value→color mapping
- Optionally add `SfHeatMapLegend` to make ranges clear

## Color mapping guidance

Read: [references/color-mapping.md](references/color-mapping.md)

When to tune color mapping:
- When you need semantic buckets (poor/average/good)
- When precise range-to-color mapping is required for interpretation

### Semantic buckets (red / yellow / green) — example

Copy-pasteable XAML + C# that maps explicit numeric ranges to semantic colors and shows a legend:

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="HeatMapSamples.MainWindow">
  <Window.Resources>
    <!-- Define semantic buckets: low=red, mid=yellow, high=green -->
    <syncfusion:ColorMappingCollection x:Key="semanticColorMapping">
      <!-- Value = numeric stop; Legend uses the Label text -->
      <syncfusion:ColorMapping Value="0"   Color="#FF0000" Label="Low (0 - 33)" />
      <syncfusion:ColorMapping Value="33"  Color="#FFFF00" Label="Medium (34 - 66)" />
      <syncfusion:ColorMapping Value="66"  Color="#00B300" Label="High (67 - 100)" />
    </syncfusion:ColorMappingCollection>

    <!-- CellMapping for row/column/value triplets -->
    <syncfusion:CellMapping x:Key="cellMapping">
      <syncfusion:CellMapping.Column>
        <syncfusion:ColumnMapping PropertyName="Column" DisplayName="Column" />
      </syncfusion:CellMapping.Column>
      <syncfusion:CellMapping.Row>
        <syncfusion:ColumnMapping PropertyName="Row" DisplayName="Row" />
      </syncfusion:CellMapping.Row>
      <syncfusion:CellMapping.Value>
        <syncfusion:ColumnMapping PropertyName="Value" />
      </syncfusion:CellMapping.Value>
    </syncfusion:CellMapping>
  </Window.Resources>

  <Grid Margin="12">
    <Grid.RowDefinitions>
      <RowDefinition Height="Auto" />
      <RowDefinition Height="*" />
    </Grid.RowDefinitions>

    <!-- Legend explains the semantic buckets clearly -->
    <syncfusion:SfHeatMapLegend
        Grid.Row="0"
        LegendMode="List"
        Orientation="Horizontal"
        ColorMappingCollection="{StaticResource semanticColorMapping}" />

    <!-- The HeatMap uses the same ColorMappingCollection -->
    <syncfusion:SfHeatMap
        Grid.Row="1"
        ItemsSource="{Binding Cells}"
        ItemsMapping="{StaticResource cellMapping}"
        ColorMappingCollection="{StaticResource semanticColorMapping}" />
  </Grid>
</Window>
```

```csharp
// Minimal code-behind (MainWindow.xaml.cs)
using System.Collections.ObjectModel;
using System.Windows;

public class CellData
{
    public string Row { get; set; }
    public string Column { get; set; }
    public double Value { get; set; }
}

public partial class MainWindow : Window
{
    public ObservableCollection<CellData> Cells { get; } = new ObservableCollection<CellData>();

    public MainWindow()
    {
        InitializeComponent();
        // sample values spanning 0..100
        Cells.Add(new CellData { Row = "R1", Column = "C1", Value = 10 });
        Cells.Add(new CellData { Row = "R1", Column = "C2", Value = 40 });
        Cells.Add(new CellData { Row = "R1", Column = "C3", Value = 80 });
        Cells.Add(new CellData { Row = "R2", Column = "C1", Value = 25 });
        Cells.Add(new CellData { Row = "R2", Column = "C2", Value = 55 });
        Cells.Add(new CellData { Row = "R2", Column = "C3", Value = 95 });

        DataContext = this;
    }
}
```

Notes:
- The `ColorMappingCollection` defines stops; values between stops are interpolated unless you purposely treat them as buckets. Using the shown stops with `SfHeatMapLegend LegendMode="List"` produces labeled ranges in the legend.
- Adjust exact `Value` thresholds to match your data distribution (e.g., 0/33/66 above). Ensure the `Value` range you define covers your data domain.

## Items mapping guidance

Read: [references/items-mapping.md](references/items-mapping.md)

When to use `TableMapping` vs `CellMapping`:
- Use `TableMapping` for wide objects with multiple numeric properties (years as columns)
- Use `CellMapping` for row/column/value triplet datasets

## Legend & visualization

Read: [references/legend.md](references/legend.md)

Use a legend to make color ranges discoverable. Choose `Gradient` for continuous ranges and `List` for labeled buckets.

## Common patterns

- Quick-start example: minimal XAML + items mapping + color mapping + legend
- Responsive layouts: limit legend width and use `Orientation` to adapt
- Accessibility: ensure labels and contrast are sufficient for screen readers

## Troubleshooting

- If colors look unexpected, verify `ColorMappingCollection` values and ensure numeric ranges align with data
- If items don't appear, confirm `ItemsMapping` matches the `ItemsSource` schema and keys

---
Generated by automation to provide a concise implementation router for the SfHeatMap control.
