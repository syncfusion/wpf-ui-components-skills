---
name: syncfusion-wpf-step-progressbar
description: Implement Syncfusion WPF SfStepProgressBar for multi-step process visualization. Use this when building wizard steps, order tracking, stepper indicators, checkout flows, registration wizards, or onboarding step sequences in WPF. Covers StepViewItem, SelectedIndex, SelectedItemStatus, step marker customization, connector lines, data binding, and template selectors based on step status.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF SfStepProgressBar

The `SfStepProgressBar` is a multi-step progress indicator for visualizing sequential workflows. Each step is a `StepViewItem` with active, inactive, or indeterminate status. Use it for order tracking, registration wizards, checkout flows, onboarding, or any process with discrete numbered stages.

## When to Use This Skill

- User needs a step-by-step progress UI (wizard, checkout, order tracker)
- Displaying multiple stages with active/inactive/in-progress states
- Building vertical or horizontal step indicators with custom markers
- Binding steps from a data collection (MVVM / XML)
- Customizing marker shapes, connector colors, or step templates by status

## Quick Start

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
    x:Class="MyApp.MainWindow">
    <Grid x:Name="grid">
        <Syncfusion:SfStepProgressBar SelectedIndex="2" SelectedItemStatus="Indeterminate">
            <Syncfusion:StepViewItem Content="Ordered" />
            <Syncfusion:StepViewItem Content="Packed" />
            <Syncfusion:StepViewItem Content="Shipped" />
            <Syncfusion:StepViewItem Content="Delivered" />
        </Syncfusion:SfStepProgressBar>
    </Grid>
</Window>
```

```csharp
using Syncfusion.UI.Xaml.ProgressBar;

SfStepProgressBar stepProgressBar = new SfStepProgressBar();
stepProgressBar.Items.Add(new StepViewItem { Content = "Ordered" });
stepProgressBar.Items.Add(new StepViewItem { Content = "Packed" });
stepProgressBar.Items.Add(new StepViewItem { Content = "Shipped" });
stepProgressBar.Items.Add(new StepViewItem { Content = "Delivered" });
stepProgressBar.SelectedIndex = 2;
stepProgressBar.SelectedItemStatus = StepStatus.Indeterminate;
grid.Children.Add(stepProgressBar);
```

- `SelectedIndex` — zero-based index of the last active step (all steps before it become Active)
- `SelectedItemStatus` — status of the selected step: `Active`, `Inactive`, or `Indeterminate`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package and assembly setup
- XAML namespace and schema import
- Basic step bar with 4 items
- Code-behind creation
- Applying SfSkinManager themes

### Selected Item & Status
📄 **Read:** [references/selected-item-and-status.md](references/selected-item-and-status.md)
- SelectedIndex — controlling which step is active
- SelectedItemStatus (Active / Inactive / Indeterminate)
- SelectedItemProgress — partial fill on the current step
- AnimationDuration — transition speed between steps
- MVVM binding patterns for dynamic step advancement

### Appearance
📄 **Read:** [references/appearance.md](references/appearance.md)
- MarkerShapeType (Circle vs Square)
- Orientation (Horizontal vs Vertical)
- Connector color and thickness customization
- MarkerClicked event for click-to-navigate behavior

### Layouts
📄 **Read:** [references/layouts.md](references/layouts.md)
- ItemsStretch: None / Fill / Auto
- ItemSpacing — gap in None mode
- MinimumItemSpacing — minimum gap in Fill mode
- Auto stretch for vertical content-driven sizing

### Data Binding & Templates
📄 **Read:** [references/data-binding-and-templates.md](references/data-binding-and-templates.md)
- ItemsSource + ItemContainerStyle for MVVM binding
- XML data source binding
- ItemTemplate — custom step marker content
- ItemTemplateSelector — status-based template switching
- MarkerTemplateSelector — custom marker per step status
- SecondaryContentTemplate — labels below/beside markers
- SecondaryContentTemplateSelector

## Key Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `SelectedIndex` | `int` | — | Zero-based index of last active step |
| `SelectedItemStatus` | `StepStatus` | `Inactive` | Status of the selected step |
| `SelectedItemProgress` | `double` | `100` | Partial fill % on the selected step |
| `AnimationDuration` | `TimeSpan` | `500ms` | Step transition animation speed |
| `Orientation` | `Orientation` | `Horizontal` | Horizontal or vertical layout |
| `MarkerShapeType` | `MarkerShapeType` | `Circle` | Circle or Square step markers |
| `ActiveConnectorColor` | `Brush` | — | Color of connector between active steps |
| `ConnectorColor` | `Brush` | — | Color of connector between inactive steps |
| `ConnectorThickness` | `double` | — | Thickness of connector lines |
| `ItemsStretch` | `ItemsStretch` | `None` | How items fill available space |
| `ItemSpacing` | `double` | `80` | Space between steps (None stretch) |

## Common Use Cases

- **Order tracking:** 4-step bar (Ordered → Packed → Shipped → Delivered) with `SelectedItemStatus="Indeterminate"` for the in-transit step
- **Registration wizard:** Vertical orientation with `ItemsStretch="Fill"` to fill the panel height
- **Onboarding flow:** Custom `MarkerTemplateSelector` to show checkmarks on completed steps and spinner on current
- **MVVM dashboard:** `ItemsSource` bound to `ObservableCollection<StepItem>` with `SelectedIndex` bound to a command
