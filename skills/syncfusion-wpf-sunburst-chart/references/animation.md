# Animation

Complete guide for adding smooth entry animations to the Sunburst Chart. Animations enhance the visual appeal and provide a polished user experience when the chart first loads or data changes.

## Overview

The Sunburst Chart supports two types of entry animations:
- **FadeIn** – Gradually increases opacity from 0% to 100%
- **Rotation** – Rotates segments from 0° to 360° during animation

Animations play when the chart initially loads or when the data source changes.

**Key Features:**
- Smooth visual entry effect
- Configurable duration (milliseconds)
- Two animation types (FadeIn, Rotation)
- Easy enable/disable toggle
- No performance impact after initial render

## Enabling Animation

### Basic Configuration

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value"
                          EnableAnimation="True" 
                          AnimationDuration="5000">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburstChart.EnableAnimation = true;
sunburstChart.AnimationDuration = 5000; // 5 seconds
```

**Properties:**
- **EnableAnimation** – Boolean to enable/disable animation
- **AnimationDuration** – Time in milliseconds (default: 1000ms)

### Disabling Animation

To remove animation effects:

**XAML:**
```xml
<sunburst:SfSunburstChart EnableAnimation="False">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburstChart.EnableAnimation = false;
```

**Use when:**
- Performance is critical
- Animations cause user distraction
- Rapid data updates occur
- Accessibility requirements (motion sensitivity)

## Animation Types

Control the animation style using the `AnimationType` property.

### FadeIn Animation

Segments gradually appear by transitioning from transparent to fully opaque.

**XAML:**
```xml
<sunburst:SfSunburstChart EnableAnimation="True"                                
                          AnimationType="FadeIn"
                          AnimationDuration="1500">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburstChart.EnableAnimation = true;
sunburstChart.AnimationType = AnimationType.FadeIn;
sunburstChart.AnimationDuration = 1500; // 1.5 seconds
```

**Effect:**
- Segments fade in from invisible to visible
- All segments animate simultaneously
- Smooth opacity transition

**Best for:**
- Subtle, professional appearance
- Dashboards and reports
- When chart updates frequently
- Minimalist design aesthetics

**Recommended duration:** 800-1500ms

### Rotation Animation

The entire chart rotates from 0° to 360° as segments appear.

**XAML:**
```xml
<sunburst:SfSunburstChart EnableAnimation="True"                                
                          AnimationType="Rotation"
                          AnimationDuration="2000">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburstChart.EnableAnimation = true;
sunburstChart.AnimationType = AnimationType.Rotation;
sunburstChart.AnimationDuration = 2000; // 2 seconds
```

**Effect:**
- Chart rotates clockwise from 0° to 360°
- Creates dynamic, eye-catching entrance
- More noticeable than FadeIn

**Best for:**
- Presentations and demos
- Marketing dashboards
- First-time user experiences
- Drawing attention to data

**Recommended duration:** 1200-2500ms

## Animation Duration

The `AnimationDuration` property controls how long the animation takes (in milliseconds).

### Duration Guidelines

**Fast (500-1000ms):**
```xml
<sunburst:SfSunburstChart AnimationDuration="800">
```
- Quick, snappy feel
- Good for frequently updating charts
- Minimal user wait time
- Best for FadeIn animations

**Medium (1000-2000ms):**
```xml
<sunburst:SfSunburstChart AnimationDuration="1500">
```
- Balanced speed and visibility
- Standard for most applications
- Pleasant viewing experience
- Works well for both animation types

**Slow (2000-5000ms):**
```xml
<sunburst:SfSunburstChart AnimationDuration="3000">
```
- Emphasized, dramatic effect
- Good for presentations
- Allows detailed observation
- Best for Rotation animations

**Recommendation:** Start with 1200-1500ms and adjust based on user feedback and context.

## Complete Examples

### Example 1: Subtle Dashboard Animation

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding SalesData}" 
                          ValueMemberPath="Amount"
                          EnableAnimation="True"
                          AnimationType="FadeIn"
                          AnimationDuration="1000"
                          Palette="Metro">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    </sunburst:SfSunburstChart.Levels>
    
</sunburst:SfSunburstChart>
```

**Use case:** Business dashboard where data updates periodically. Fast FadeIn provides smooth transitions without disruption.

### Example 2: Presentation Mode

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding EmployeeData}" 
                          ValueMemberPath="Count"
                          EnableAnimation="True"
                          AnimationType="Rotation"
                          AnimationDuration="2500"
                          Header="Company Structure">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.Legend>
        <sunburst:SunburstLegend DockPosition="Left"/>
    </sunburst:SfSunburstChart.Legend>
    
</sunburst:SfSunburstChart>
```

**Use case:** Executive presentation. Slow Rotation creates impact and draws audience attention to the visualization.

### Example 3: No Animation (Performance)

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding RealTimeData}" 
                          ValueMemberPath="Value"
                          EnableAnimation="False">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level1"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level2"/>
    </sunburst:SfSunburstChart.Levels>
    
</sunburst:SfSunburstChart>
```

**Use case:** Real-time monitoring where data updates every few seconds. No animation prevents visual fatigue.

## Troubleshooting

**Animation not playing:**
- Verify `EnableAnimation="True"`
- Check that `AnimationDuration` is not zero
- Ensure data is loaded (ItemsSource is not empty)
- Confirm chart is visible in UI

**Animation too slow/fast:**
- Adjust `AnimationDuration` value
- Test with different values (increment by 500ms)
- Consider device performance capabilities

**Animation stuttering:**
- Check for high CPU usage
- Simplify chart (reduce levels or data points)
- Disable other animations running simultaneously
- Test on target hardware

**Animation runs on every data update:**
- This is expected behavior when ItemsSource changes
- Disable animation if frequent updates occur
- Use conditional animation (first load only)

## Related Features

**Combine with other features:**

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value"
                          EnableAnimation="True"
                          AnimationType="FadeIn"
                          AnimationDuration="1200"
                          Palette="Metro">
    
    <!-- Animation plays while chart loads -->
    
    <sunburst:SfSunburstChart.Behaviors>
        <!-- Tooltip appears after animation completes -->
        <sunburst:SunburstToolTipBehavior ShowToolTip="True"/>
        
        <!-- Selection works after animation -->
        <sunburst:SunburstSelectionBehavior EnableSelection="True"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

Animations work seamlessly with all other chart features (tooltips, selection, zooming, legends, etc.).
