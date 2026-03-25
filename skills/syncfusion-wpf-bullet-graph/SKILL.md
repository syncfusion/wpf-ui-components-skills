---
name: syncfusion-wpf-bullet-graph
description: Implement Syncfusion WPF Bullet Graph (SfBulletGraph) components for performance indicators and KPI visualization. Use this when displaying metrics against targets, creating dashboard gauges, or visualizing performance in qualitative ranges. This skill covers featured measures, comparative measures, qualitative ranges, goal tracking, and compact data visualization for dashboards.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Bullet Graphs in WPF

Guide for implementing the Syncfusion® WPF Bullet Graph (`SfBulletGraph`) component. A bullet graph is a compact, linear visualization designed to display performance metrics by showing a primary measure (current value) against a target measure within qualitative ranges that indicate performance levels.

## When to Use This Skill

Use this skill when you need to:
- **Display performance metrics** with current values versus targets
- **Create KPI dashboards** with space-efficient visualizations
- **Show progress toward goals** with visual performance indicators
- **Replace traditional gauges** with more compact bullet graph displays
- **Visualize data in context** using qualitative range backgrounds
- **Implement revenue analysis** or expense tracking visualizations
- **Build dashboard environments** where screen space is limited
- **Compare actual vs. target values** in a single compact visualization

## Component Overview

The `SfBulletGraph` is a variation of the bar graph designed as a replacement for dashboard gauges and meters. It features:

- **Featured Measure:** The primary data value (e.g., current year-to-date revenue) displayed as a prominent bar
- **Comparative Measure:** A target or reference value displayed as a perpendicular line marker
- **Qualitative Ranges:** Background color bands that provide context (e.g., poor, satisfactory, good)
- **Quantitative Scale:** Configurable axis with major/minor ticks and labels
- **Caption Support:** Descriptive labels to identify what the bullet graph represents
- **Orientation Options:** Horizontal or vertical layout
- **Compact Design:** Shows substantial data in minimal space

**Typical Use Cases:**
- Revenue vs. target tracking
- Expense analysis against budgets
- Performance metrics on dashboards
- KPI visualization
- Goal progress indicators

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation via NuGet or Syncfusion Reference Manager
- Assembly references (Syncfusion.SfBulletGraph.WPF)
- Namespace declarations
- Basic bullet graph creation in XAML and C#
- Theme support and customization
- First working example

### Quantitative Scale Configuration
📄 **Read:** [references/quantitative-scale.md](references/quantitative-scale.md)
- Setting scale range (Minimum, Maximum, Interval)
- Major tick configuration (size, stroke, color)
- Minor tick configuration (per interval, size, stroke)
- Label customization and formatting
- Scale length adjustment
- Tick and label visibility options

### Performance Measures
📄 **Read:** [references/measures.md](references/measures.md)
- Featured measure implementation (primary data value)
- Featured measure bar customization (stroke, thickness)
- Comparative measure implementation (target/goal marker)
- Comparative measure symbol customization
- Visual hierarchy and layering
- Measure value binding

### Qualitative Ranges
📄 **Read:** [references/ranges.md](references/ranges.md)
- Creating range collections (QualitativeRanges)
- Range properties (RangeStart, RangeEnd, RangeStroke)
- Range size and opacity customization
- Binding range colors to ticks and labels
- Common range patterns (Bad/Satisfactory/Good)
- Best practices for range design

### Caption Customization
📄 **Read:** [references/caption.md](references/caption.md)
- Adding captions to bullet graphs
- Caption positioning (Near, Far)
- Custom caption content and layouts
- Multi-line captions with StackPanel
- Caption styling and formatting
- XAML and C# caption examples

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Orientation (Horizontal, Vertical)
- Flow direction (RTL support)
- Tooltip configuration and customization
- Custom tooltip templates for measures and ranges
- Data binding scenarios
- MVVM pattern implementation
- Performance optimization tips

## Quick Start Example

### Basic Bullet Graph in XAML

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:SfBulletGraph 
            Minimum="0" 
            Maximum="10" 
            Interval="2"
            FeaturedMeasure="5" 
            ComparativeMeasure="7"
            MinorTicksPerInterval="3"
            MajorTickSize="15" 
            MinorTickSize="10">
            
            <syncfusion:SfBulletGraph.QualitativeRanges>
                <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="#EBEBEB"/>
                <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="#D8D8D8"/>
                <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="#7F7F7F"/>
            </syncfusion:SfBulletGraph.QualitativeRanges>
        </syncfusion:SfBulletGraph>
    </Grid>
</Window>
```

### Basic Bullet Graph in C#

```csharp
using Syncfusion.UI.Xaml.BulletGraph;

SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.FeaturedMeasure = 5;
bulletGraph.ComparativeMeasure = 7;
bulletGraph.MinorTicksPerInterval = 3;
bulletGraph.MajorTickSize = 15;
bulletGraph.MinorTickSize = 10;

bulletGraph.QualitativeRanges.Add(new QualitativeRange() 
{ 
    RangeEnd = 4.5, 
    RangeStroke = new SolidColorBrush(Color.FromRgb(235, 235, 235)) 
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange() 
{ 
    RangeEnd = 7.5, 
    RangeStroke = new SolidColorBrush(Color.FromRgb(216, 216, 216)) 
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange() 
{ 
    RangeEnd = 10, 
    RangeStroke = new SolidColorBrush(Color.FromRgb(127, 127, 127)) 
});

this.Grid.Children.Add(bulletGraph);
```

## Common Patterns

### Revenue Dashboard with Caption

```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="100" 
    Interval="20"
    FeaturedMeasure="65" 
    ComparativeMeasure="80"
    CaptionPosition="Far"
    QualitativeRangesSize="30">
    
    <syncfusion:SfBulletGraph.Caption>
        <StackPanel Margin="0,0,10,0">
            <TextBlock Text="Revenue YTD" FontSize="13" HorizontalAlignment="Center"/>
            <TextBlock Text="$ in Thousands" FontSize="13" HorizontalAlignment="Center"/>
        </StackPanel>
    </syncfusion:SfBulletGraph.Caption>
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="35" RangeStroke="Red" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="70" RangeStroke="Yellow" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="Green" RangeOpacity="1"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

### Vertical Bullet Graph

```xml
<syncfusion:SfBulletGraph 
    Orientation="Vertical"
    Minimum="0" 
    Maximum="100" 
    Interval="25"
    FeaturedMeasure="75" 
    ComparativeMeasure="90"
    FeaturedMeasureBarStroke="Black">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="40" RangeStroke="#FF6B6B"/>
        <syncfusion:QualitativeRange RangeEnd="75" RangeStroke="#FFE66D"/>
        <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="#95E1D3"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

### Bullet Graph with Tooltips

```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    FeaturedMeasure="6.5" 
    ComparativeMeasure="8"
    ShowToolTip="True"
    QualitativeRangesSize="30">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="4" RangeStroke="Red" RangeOpacity="0.8"/>
        <syncfusion:QualitativeRange RangeEnd="7" RangeStroke="Yellow" RangeOpacity="0.8"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="Green" RangeOpacity="0.8"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

## Key Properties

### Scale Properties
- **Minimum** - Starting value of the quantitative scale
- **Maximum** - Ending value of the quantitative scale
- **Interval** - Distance between major ticks
- **MinorTicksPerInterval** - Number of minor ticks between major ticks
- **QuantitativeScaleLength** - Length of the scale in pixels

### Measure Properties
- **FeaturedMeasure** - Primary data value (current performance)
- **FeaturedMeasureBarStroke** - Color of the featured measure bar
- **FeaturedMeasureBarStrokeThickness** - Thickness of the featured measure bar
- **ComparativeMeasure** - Target or comparison value
- **ComparativeMeasureSymbolStroke** - Color of the comparative measure marker
- **ComparativeMeasureSymbolStrokeThickness** - Thickness of the comparative measure marker

### Tick and Label Properties
- **MajorTickSize** - Size of major tick marks
- **MajorTickStroke** - Color of major tick marks
- **MinorTickSize** - Size of minor tick marks
- **MinorTickStroke** - Color of minor tick marks
- **LabelStroke** - Color of scale labels

### Range Properties
- **QualitativeRanges** - Collection of qualitative range objects
- **QualitativeRangesSize** - Height/width of the range bands
- **BindRangeStrokeToLabels** - Sync label colors with range colors
- **BindRangeStrokeToTicks** - Sync tick colors with range colors

### Layout Properties
- **Orientation** - Horizontal or Vertical layout
- **FlowDirection** - LeftToRight or RightToLeft
- **Caption** - Descriptive content for the bullet graph
- **CaptionPosition** - Near (left/top) or Far (right/bottom)

### Tooltip Properties
- **ShowToolTip** - Enable/disable tooltip display
- **FeaturedMeasureToolTipTemplate** - Custom template for featured measure tooltip
- **ComparativeMeasureToolTipTemplate** - Custom template for comparative measure tooltip
- **QualitativeRangeToolTipTemplate** - Custom template for range tooltip