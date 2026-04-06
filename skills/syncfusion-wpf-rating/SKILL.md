---
name: syncfusion-wpf-rating
description: Implements the Syncfusion WPF SfRating control for star-based rating input. Use when adding user feedback or review ratings, configuring rating precision (standard/half/exact), or presenting read-only rating displays.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Rating

The SfRating control for WPF provides a visual star-based rating system that allows users to rate items. It supports configurable precision modes, extensive appearance customization, tooltips, and read-only display. Use this control for rating movies, applications, products, services, or any scenario requiring user feedback through a rating interface.

## When to Use This Skill

Use this skill when user need to:
- Implement star-based rating controls in WPF applications
- Create product or service rating interfaces
- Build review systems with visual rating feedback
- Display read-only ratings (e.g., average ratings, historical ratings)
- Configure rating precision (full stars, half stars, or exact values)
- Customize rating appearance (colors, sizes, spacing, hover effects)
- Add tooltips to show rating values
- Handle rating value changes with ValueChanged events
- Integrate rating controls with data binding scenarios

## Component Overview

**SfRating** provides:
- Configurable number of rating items (typically 5 stars)
- Three precision modes: Standard (full), Half (0.5 increments), Exact (any value)
- Extensive styling options (fill colors, stroke colors, sizes, spacing)
- Tooltip support with customizable precision and appearance
- Read-only mode for displaying non-editable ratings
- ValueChanged event for handling rating updates
- Theme support (via SfSkinManager)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet package installation
- Adding SfRating control via designer, XAML, or C#
- Setting ItemsCount and Value properties
- Basic configuration and ValueChanged event
- Methods overview (Dispose, template methods, event handlers)
- Theme configuration

### Precision Modes
📄 **Read:** [references/precision.md](references/precision.md)
- Understanding precision modes (Standard, Half, Exact)
- Configuring rating accuracy levels
- Use cases for each precision type
- Code examples and visual comparisons

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)
- Customizing fill colors (rated/unrated items)
- Setting stroke colors and thickness
- Configuring mouse hover effects
- Adjusting item heights, sizes, and spacing
- Setting corner radius for rating items
- Using ItemContainerStyle for consistent styling
- PreviewValue property for hover feedback

### Tooltip Configuration
📄 **Read:** [references/tooltip.md](references/tooltip.md)
- Enabling/disabling tooltips
- Configuring tooltip precision (decimal places)
- Customizing tooltip foreground color
- Tooltip behavior and best practices

### Read-Only Mode
📄 **Read:** [references/readonly-configuration.md](references/readonly-configuration.md)
- Setting IsReadOnly property
- Use cases for non-editable ratings
- Combining read-only with styling options

## Quick Start Example

### Basic Rating Control (XAML)

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:SfRating ItemsCount="5" 
                             Value="3" 
                             Width="150"/>
    </Grid>
</Window>
```

### Basic Rating Control (C#)

```csharp
using Syncfusion.Windows.Controls.Input;

namespace RatingApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            SfRating rating = new SfRating()
            {
                ItemsCount = 5,
                Value = 3,
                Width = 150
            };
            
            this.Content = rating;
        }
    }
}
```

## Common Patterns

### 1. Rating with ValueChanged Event

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="3" 
                     Width="150"
                     ValueChanged="SfRating_ValueChanged"/>
```

```csharp
private void SfRating_ValueChanged(object sender, ValueChangedEventArgs e)
{
    var oldValue = e.OldValue;
    var newValue = e.NewValue;
    MessageBox.Show($"Rating changed from {oldValue} to {newValue}");
}
```

### 2. Half Precision Rating

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Precision="Half" 
                     Value="3.5" 
                     Width="150"/>
```

### 3. Custom Styled Rating

```xml
<syncfusion:SfRating ItemsCount="5" Value="4" Width="150">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="Gold"/>
            <Setter Property="UnratedFill" Value="LightGray"/>
            <Setter Property="RatedStroke" Value="Orange"/>
            <Setter Property="RatedStrokeThickness" Value="2"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

### 4. Read-Only Rating Display

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="4.5" 
                     Precision="Half"
                     IsReadOnly="True" 
                     Width="150"/>
```

### 5. Rating with Custom Size and Spacing

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="3" 
                     ItemSize="30"
                     ItemsSpacing="10"
                     Width="200"/>
```

### 6. Rating with Tooltip Configuration

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="3" 
                     Precision="Exact"
                     ShowToolTip="True"
                     AutoToolTipPrecision="2"
                     ToolTipForeground="DarkBlue"
                     Width="150"/>
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ItemsCount` | int | Number of rating items to display (e.g., 5 stars) |
| `Value` | double | Current rating value (0 to ItemsCount) |
| `Precision` | Precision | Rating accuracy: Standard, Half, or Exact |
| `IsReadOnly` | bool | Makes rating non-editable when true |
| `ShowToolTip` | bool | Shows/hides tooltip on hover (default: true) |
| `AutoToolTipPrecision` | int | Decimal places in tooltip (default: 1) |
| `ItemSize` | double | Size of each rating item (default: 20) |
| `ItemsSpacing` | double | Space between items (default: 4) |
| `CornerRadius` | CornerRadius | Corner radius for rating items |
| `PreviewValue` | double | Read-only preview value on hover |

### Styling Properties (via ItemContainerStyle)

| Property | Description |
|----------|-------------|
| `RatedFill` | Fill color for selected/rated items |
| `UnratedFill` | Fill color for unselected items |
| `RatedStroke` | Stroke color for rated items |
| `UnratedStroke` | Stroke color for unrated items |
| `RatedStrokeThickness` | Stroke thickness for rated items |
| `UnratedStrokeThickness` | Stroke thickness for unrated items |
| `PointerOverFill` | Fill color on mouse hover |
| `PointerOverStroke` | Stroke color on mouse hover |
| `PointerOverStrokeThickness` | Stroke thickness on hover |
| `Height` | Individual item height |

## Common Use Cases

1. **Product Reviews** - Display and collect product ratings in e-commerce applications
2. **Service Feedback** - Gather user satisfaction ratings for services
3. **Movie/Content Ratings** - Show and allow rating of media content
4. **Performance Evaluations** - Visual rating scales for assessments
5. **Survey Responses** - Rating scale questions in surveys
6. **Dashboard Metrics** - Display KPI or quality scores as visual ratings
7. **User Profiles** - Show user reputation or skill level ratings
8. **Comparison Tools** - Visual comparison of rated items

## Best Practices

- Use **Standard precision** for simple 1-5 star ratings
- Use **Half precision** for more granular feedback (0.5 increments)
- Use **Exact precision** when binding to calculated values or allowing precise input
- Set **IsReadOnly="True"** when displaying average or historical ratings
- Customize **RatedFill** to match your application's brand colors
- Use **ShowToolTip** to provide numeric feedback alongside visual stars
- Set appropriate **ItemSize** and **ItemsSpacing** for your UI layout
- Handle **ValueChanged** event for immediate feedback or data binding updates
- Consider using **Precision="Half"** with **IsReadOnly="True"** for displaying decimal averages

## Assembly Requirements

Required assemblies:
- `Syncfusion.SfInput.WPF`
- `Syncfusion.SfShared.WPF`

NuGet package: `Syncfusion.SfInput.WPF`

## Related Components

- TextBox - Text input controls
- Numeric TextBox - Numeric input with validation
- Input Mask - Formatted text input
- Slider - Range value selection
