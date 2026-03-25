# Rim and Appearance

## Table of Contents
- [Rim Overview](#rim-overview)
- [Rim Customization](#rim-customization)
- [Gauge Sizing](#gauge-sizing)
- [Background Styling](#background-styling)
- [Theming](#theming)
- [Common Patterns](#common-patterns)

## Rim Overview

The **rim** is the circular border around the scale. It provides visual structure and can enhance the gauge's appearance.

**Key Features:**
- Show/hide rim
- Customizable stroke and thickness
- Positioning control
- Coordinates with scale elements

## Rim Customization

### Showing the Rim

**XAML:**
```xml
<gauge:CircularScale ShowRim="True">
    <!-- Scale content -->
</gauge:CircularScale>
```

**C#:**
```csharp
scale.ShowRim = true;
```

**Default:** Rim is hidden unless explicitly shown.

### Rim Stroke (Color)

**XAML:**
```xml
<gauge:CircularScale ShowRim="True"
                     RimStroke="LightGray">
</gauge:CircularScale>
```

**C#:**
```csharp
scale.RimStroke = new SolidColorBrush(Colors.LightGray);
```

### Rim Thickness

**XAML:**
```xml
<gauge:CircularScale ShowRim="True"
                     RimStroke="LightGray"
                     RimStrokeThickness="5">
</gauge:CircularScale>
```

**C#:**
```csharp
scale.RimStrokeThickness = 5;
```

**Typical Values:**
- Thin accent: 1-3
- Standard: 4-8
- Bold: 10-20

### Complete Rim Styling

**XAML:**
```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100"
                     ShowRim="True"
                     RimStroke="#CCCCCC"
                     RimStrokeThickness="8">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="65"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

## Gauge Sizing

### Fixed Dimensions

**XAML:**
```xml
<gauge:SfCircularGauge Height="400" Width="400">
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale>
            <!-- Content -->
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

**Best Practice:** Use square dimensions (Height = Width) for circular gauges.

### Responsive Sizing

**XAML:**
```xml
<gauge:SfCircularGauge HorizontalAlignment="Stretch" 
                       VerticalAlignment="Stretch">
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale RadiusFactor="0.95">
            <!-- Content -->
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

**RadiusFactor:** Scale size as fraction of gauge size (0-1)

### Scale Radius

**Absolute:**
```xml
<gauge:CircularScale Radius="180">
```

**Relative:**
```xml
<gauge:CircularScale RadiusFactor="0.9">
```

**Use Case:**
- `Radius` - Fixed size layouts
- `RadiusFactor` - Responsive designs

## Background Styling

### Gauge Background

**XAML:**
```xml
<gauge:SfCircularGauge Background="WhiteSmoke">
    <!-- Scales -->
</gauge:SfCircularGauge>
```

**C#:**
```csharp
gauge.Background = new SolidColorBrush(Colors.WhiteSmoke);
```

### Scale Background

The rim acts as the scale's background when styled appropriately:

**XAML:**
```xml
<gauge:CircularScale ShowRim="True"
                     RimStroke="LightGray"
                     RimStrokeThickness="30"
                     SweepAngle="360">
    <!-- Progress pointer on top -->
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Value="75"
                               RangePointerStroke="DeepSkyBlue"
                               RangePointerStrokeThickness="30"
                               RangeCap="Both"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

## Theming

Apply consistent visual themes across gauges using SfSkinManager.

### Built-in Themes

Syncfusion WPF provides multiple built-in themes:

**Available Themes:**
- MaterialLight
- MaterialDark
- MaterialLightBlue
- MaterialDarkBlue
- FluentLight
- FluentDark
- Office2019Colorful
- Office2019Black
- Office2019White
- Windows11Light
- Windows11Dark
- And more...

### Applying Themes

**Method 1: SfSkinManager (XAML)**

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=MaterialLight}">
    
    <gauge:SfCircularGauge>
        <!-- Gauge automatically styled -->
    </gauge:SfCircularGauge>
    
</Window>
```

**Method 2: SfSkinManager (Code)**

```csharp
using Syncfusion.SfSkinManager;

// Apply to specific control
SfSkinManager.SetTheme(myGauge, new Theme("MaterialLight"));

// Apply to window (affects all controls)
SfSkinManager.SetTheme(this, new Theme("FluentDark"));
```

### Theme Examples

**Material Light:**
```csharp
SfSkinManager.SetTheme(gauge, new Theme("MaterialLight"));
```

**Fluent Dark:**
```csharp
SfSkinManager.SetTheme(gauge, new Theme("FluentDark"));
```

**Office 2019 Colorful:**
```csharp
SfSkinManager.SetTheme(gauge, new Theme("Office2019Colorful"));
```

### Custom Styling

Override specific properties after applying theme:

**XAML:**
```xml
<gauge:CircularScale ShowRim="True"
                     RimStroke="DeepSkyBlue"
                     RimStrokeThickness="6"
                     LabelStroke="DeepSkyBlue">
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Stroke="DeepSkyBlue"/>
    </gauge:CircularScale.MajorTickSettings>
</gauge:CircularScale>
```

### Theme Resources

Reference theme documentation:
https://help.syncfusion.com/wpf/themes/skin-manager

**Theme Studio:**
Create custom themes using Syncfusion Theme Studio:
https://help.syncfusion.com/wpf/themes/theme-studio

## Common Patterns

### Pattern 1: Modern Minimal Gauge

```xml
<gauge:SfCircularGauge Background="White">
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale ShowRim="False"
                             ShowTicks="False"
                             SweepAngle="360">
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer PointerType="RangePointer" 
                                       Value="75"
                                       RangePointerStroke="DeepSkyBlue"
                                       RangePointerStrokeThickness="25"
                                       RangeCap="Both"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

### Pattern 2: Classic Speedometer

```xml
<gauge:SfCircularGauge Background="#F5F5F5" 
                       Height="350" Width="350">
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale ShowRim="True"
                             RimStroke="#CCCCCC"
                             RimStrokeThickness="8"
                             StartAngle="130"
                             SweepAngle="280"
                             LabelStroke="#333"
                             FontSize="14">
            <gauge:CircularScale.MajorTickSettings>
                <gauge:MajorTickSetting Stroke="#333" Length="15"/>
            </gauge:CircularScale.MajorTickSettings>
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer PointerType="NeedlePointer" 
                                       Value="85"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

### Pattern 3: Progress Ring with Background

```xml
<gauge:CircularScale SweepAngle="360"
                     ShowRim="True"
                     RimStroke="#E0E0E0"
                     RimStrokeThickness="30"
                     LabelStroke="Transparent"
                     ShowTicks="False">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Value="75"
                               RangePointerStroke="LimeGreen"
                               RangePointerStrokeThickness="30"
                               RangeCap="Both"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Pattern 4: Themed Dark Gauge

```xml
<Window syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=MaterialDark}">
    <Grid Background="#303030">
        <gauge:SfCircularGauge Height="300" Width="300">
            <gauge:SfCircularGauge.Scales>
                <gauge:CircularScale ShowRim="True"
                                     RimStroke="#555"
                                     RimStrokeThickness="5">
                    <gauge:CircularScale.Pointers>
                        <gauge:CircularPointer Value="60"/>
                    </gauge:CircularScale.Pointers>
                </gauge:CircularScale>
            </gauge:SfCircularGauge.Scales>
        </gauge:SfCircularGauge>
    </Grid>
</Window>
```

### Pattern 5: Multi-Scale with Different Rims

```xml
<gauge:SfCircularGauge SpacingMargin="0.75">
    <gauge:SfCircularGauge.Scales>
        <!-- Outer scale -->
        <gauge:CircularScale Radius="180"
                             ShowRim="True"
                             RimStroke="Blue"
                             RimStrokeThickness="6">
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer Value="70"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
        
        <!-- Inner scale -->
        <gauge:CircularScale Radius="100"
                             ShowRim="True"
                             RimStroke="Red"
                             RimStrokeThickness="4">
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer Value="50"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

## Best Practices

### Visual Hierarchy

**Do:**
- Use rim to frame the gauge
- Keep rim color subtle (grays work well)
- Coordinate rim with pointer/range colors
- Use consistent thickness across similar gauges

**Avoid:**
- Very bright rim colors (distracting)
- Overly thick rims (>20px unless intentional)
- Clashing colors between rim and pointers

### Sizing

**For readability:**
- Minimum gauge size: 200x200 pixels
- Typical dashboard gauges: 250-350 pixels
- Large display gauges: 400-600 pixels

**For grids:**
- Use consistent sizing
- Allow adequate spacing between gauges
- Consider responsive RadiusFactor

### Theming Strategy

**Consistent themes:**
- Apply theme at Window/Page level
- Override sparingly for emphasis
- Test in light and dark modes
- Consider accessibility (contrast)

## Next Steps

- **Add Headers:** Title your styled gauge
  → Read: [header.md](header.md)

- **Add Annotations:** Overlay additional information
  → Read: [annotations.md](annotations.md)

- **Combine Elements:** Create complete dashboard gauges
  → Read: All reference files
