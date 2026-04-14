# Visual Customization

## Table of Contents
- [Overview](#overview)
- [Color Palettes](#color-palettes)
- [Custom Palettes](#custom-palettes)
- [Region Properties](#region-properties)
- [Complete Examples](#complete-examples)

## Overview

Visual customization allows you to control the appearance of the Sunburst Chart through color palettes, custom brushes, and region sizing properties. These features enable brand-consistent, visually appealing charts.

**Customization Options:**
- Predefined color palettes (12 built-in themes)
- Custom color palettes with gradient support
- Radius and inner radius control
- Start and end angle configuration
- Chart positioning and sizing

## Color Palettes

The `Palette` property applies predefined color schemes to chart segments.

### Predefined Palettes

Syncfusion provides 12 built-in palettes:

- **Metro** – Modern, vibrant colors
- **AutumnBrights** – Warm autumn tones
- **FloraHues** – Natural, floral colors
- **Pineapple** – Yellow and tropical tones
- **TomatoSpectrum** – Red and orange shades
- **RedChrome** – Red gradient variations
- **PurpleChrome** – Purple gradient variations
- **BlueChrome** – Blue gradient variations
- **GreenChrome** – Green gradient variations
- **Elite** – Professional, elegant colors
- **LightCandy** – Pastel, soft colors
- **SandyBeach** – Beach and sand tones

### Applying a Predefined Palette

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="EmployeesCount" 
                          Palette="SandyBeach">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobRole"/>
    </sunburst:SfSunburstChart.Levels>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ValueMemberPath = "EmployeesCount";
sunburst.SetBinding(SfSunburstChart.ItemsSourceProperty, "Data");

// Add levels
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "Country" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobDescription" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobGroup" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobRole" });

// Apply palette
sunburst.Palette = SunburstColorPalette.SandyBeach;
```

### Palette Selection Guide

**Metro:** Modern applications, dashboards, tech companies  
**AutumnBrights:** Seasonal reports, fall themes  
**FloraHues:** Environmental data, nature themes  
**BlueChrome:** Corporate, professional, finance  
**GreenChrome:** Environmental, growth, sustainability  
**Elite:** Executive presentations, formal reports  
**LightCandy:** User-friendly apps, consumer products

## Custom Palettes

Create your own color schemes using the `ColorModel` property with custom brushes.

### Basic Custom Palette

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="EmployeesCount"
                          Palette="Custom">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobRole"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.ColorModel>
        <sunburst:SunburstColorModel>
            <sunburst:SunburstColorModel.CustomBrushes>
                
                <!-- Solid colors -->
                <SolidColorBrush Color="#FF6B88E6"/>
                <SolidColorBrush Color="#FFE67E6B"/>
                <SolidColorBrush Color="#FF6BE6A8"/>
                <SolidColorBrush Color="#FFE6D06B"/>
                
            </sunburst:SunburstColorModel.CustomBrushes>
        </sunburst:SunburstColorModel>
    </sunburst:SfSunburstChart.ColorModel>
    
</sunburst:SfSunburstChart>
```

**Important:** Set `Palette="Custom"` before applying `ColorModel`.

### Custom Palette with Gradient Brushes

Create sophisticated color transitions with LinearGradientBrush:

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="EmployeesCount"
                          Palette="Custom">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobRole"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.ColorModel>
        <sunburst:SunburstColorModel>
            <sunburst:SunburstColorModel.CustomBrushes>
                
                <!-- Gradient 1: Pink to Gray -->
                <LinearGradientBrush>
                    <GradientStop Color="DeepPink" Offset="0"/>
                    <GradientStop Color="Gray" Offset="1"/>
                </LinearGradientBrush>
                
                <!-- Gradient 2: Green to Yellow -->
                <LinearGradientBrush>
                    <GradientStop Color="Green" Offset="0"/>
                    <GradientStop Color="Yellow" Offset="1"/>
                </LinearGradientBrush>
                
                <!-- Gradient 3: Orange to Brown -->
                <LinearGradientBrush>
                    <GradientStop Color="Orange" Offset="0"/>
                    <GradientStop Color="Brown" Offset="1"/>
                </LinearGradientBrush>
                
                <!-- Gradient 4: Violet to White -->
                <LinearGradientBrush>
                    <GradientStop Color="DarkViolet" Offset="0"/>
                    <GradientStop Color="White" Offset="1"/>
                </LinearGradientBrush>
                
            </sunburst:SunburstColorModel.CustomBrushes>
        </sunburst:SunburstColorModel>
    </sunburst:SfSunburstChart.ColorModel>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ValueMemberPath = "EmployeesCount";
sunburst.SetBinding(SfSunburstChart.ItemsSourceProperty, "Data");

// Add levels
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "Country" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobDescription" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobGroup" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobRole" });

// Set palette to Custom
sunburst.Palette = SunburstColorPalette.Custom;

// Create color model with gradients
SunburstColorModel colorModel = new SunburstColorModel();

// Gradient 1
LinearGradientBrush brush1 = new LinearGradientBrush();
brush1.GradientStops.Add(new GradientStop() { Color = Colors.DeepPink, Offset = 0 });
brush1.GradientStops.Add(new GradientStop() { Color = Colors.Gray, Offset = 1 });

// Gradient 2
LinearGradientBrush brush2 = new LinearGradientBrush();
brush2.GradientStops.Add(new GradientStop() { Color = Colors.Green, Offset = 0 });
brush2.GradientStops.Add(new GradientStop() { Color = Colors.Yellow, Offset = 1 });

// Gradient 3
LinearGradientBrush brush3 = new LinearGradientBrush();
brush3.GradientStops.Add(new GradientStop() { Color = Colors.Orange, Offset = 0 });
brush3.GradientStops.Add(new GradientStop() { Color = Colors.Brown, Offset = 1 });

// Gradient 4
LinearGradientBrush brush4 = new LinearGradientBrush();
brush4.GradientStops.Add(new GradientStop() { Color = Colors.DarkViolet, Offset = 0 });
brush4.GradientStops.Add(new GradientStop() { Color = Colors.White, Offset = 1 });

// Add to color model
colorModel.CustomBrushes.Add(brush1);
colorModel.CustomBrushes.Add(brush2);
colorModel.CustomBrushes.Add(brush3);
colorModel.CustomBrushes.Add(brush4);

sunburst.ColorModel = colorModel;
```

### Hex Color Custom Palette

Using hex color codes for brand colors:

```xml
<sunburst:SfSunburstChart.ColorModel>
    <sunburst:SunburstColorModel>
        <sunburst:SunburstColorModel.CustomBrushes>
            <SolidColorBrush Color="#FF1E88E5"/>  <!-- Primary Blue -->
            <SolidColorBrush Color="#FFFFC107"/>  <!-- Accent Yellow -->
            <SolidColorBrush Color="#FF43A047"/>  <!-- Success Green -->
            <SolidColorBrush Color="#FFE53935"/>  <!-- Error Red -->
            <SolidColorBrush Color="#FF8E24AA"/>  <!-- Purple -->
            <SolidColorBrush Color="#FF00ACC1"/>  <!-- Cyan -->
        </sunburst:SunburstColorModel.CustomBrushes>
    </sunburst:SunburstColorModel>
</sunburst:SfSunburstChart.ColorModel>
```

## Region Properties

Control the chart's size, position, and angular coverage using region properties.

### Radius Control

The `Radius` property controls the outer radius of the chart (0.0 to 1.0).

**XAML:**
```xml
<sunburst:SfSunburstChart Radius="0.6">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
chart.Radius = 0.6;
```

**Values:**
- **0.0** – No chart (invisible)
- **0.5** – Half the available space
- **0.9** – Default, nearly full space with small margin
- **1.0** – Full available space (may clip at edges)

**Recommendations:**
- Standard charts: 0.85-0.9 (default 0.9)
- With legend/labels: 0.7-0.8
- Multiple charts side-by-side: 0.6-0.7

### Inner Radius Control

The `InnerRadius` property creates a donut-style center hole (0.0 to 1.0).

**XAML:**
```xml
<sunburst:SfSunburstChart InnerRadius="0.5">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
chart.InnerRadius = 0.5;
```

**Values:**
- **0.0** – Solid center (no hole)
- **0.2** – Default, small center circle
- **0.4** – Moderate donut effect
- **0.6** – Large center hole, thin rings

**Recommendations:**
- Standard sunburst: 0.2 (default)
- Donut emphasis: 0.4-0.5
- Display center content: 0.5-0.6
- Avoid >0.7 (segments become too thin)

### Start and End Angles

Control the angular coverage of the chart (in degrees).

**Full Circle (Default):**
```xml
<sunburst:SfSunburstChart StartAngle="0" EndAngle="360">
</sunburst:SfSunburstChart>
```

**Semi-Circle (Bottom Half):**
```xml
<sunburst:SfSunburstChart StartAngle="180" EndAngle="360">
</sunburst:SfSunburstChart>
```

**Semi-Circle (Top Half):**
```xml
<sunburst:SfSunburstChart StartAngle="0" EndAngle="180">
</sunburst:SfSunburstChart>
```

**Quarter Circle:**
```xml
<sunburst:SfSunburstChart StartAngle="0" EndAngle="90">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburstChart.StartAngle = 180;
sunburstChart.EndAngle = 360;
```

**Angle Reference:**
- **0°** – Right (3 o'clock)
- **90°** – Bottom (6 o'clock)
- **180°** – Left (9 o'clock)
- **270°** – Top (12 o'clock)
- **360°** – Full circle return

**Use cases:**
- **Full circle (0-360):** Standard visualization
- **Semi-circle (180-360 or 0-180):** Gauge-like displays, limited space
- **Quarter circle:** Corner decorative elements

## Complete Examples

### Example 1: Corporate Brand Colors

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding SalesData}" 
                          ValueMemberPath="Revenue"
                          Palette="Custom"
                          Radius="0.85"
                          InnerRadius="0.3">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.ColorModel>
        <sunburst:SunburstColorModel>
            <sunburst:SunburstColorModel.CustomBrushes>
                <!-- Company brand colors -->
                <SolidColorBrush Color="#FF0066CC"/>  <!-- Brand Blue -->
                <SolidColorBrush Color="#FFFF6600"/>  <!-- Brand Orange -->
                <SolidColorBrush Color="#FF333333"/>  <!-- Dark Gray -->
                <SolidColorBrush Color="#FF99CC00"/>  <!-- Light Green -->
            </sunburst:SunburstColorModel.CustomBrushes>
        </sunburst:SunburstColorModel>
    </sunburst:SfSunburstChart.ColorModel>
    
</sunburst:SfSunburstChart>
```

### Example 2: Gradient Palette with Large Donut

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value"
                          Palette="Custom"
                          Radius="0.9"
                          InnerRadius="0.5"
                          Header="Distribution Analysis"
                          FontSize="18">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subcategory"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.ColorModel>
        <sunburst:SunburstColorModel>
            <sunburst:SunburstColorModel.CustomBrushes>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                    <GradientStop Color="#FF667EEA" Offset="0"/>
                    <GradientStop Color="#FF764BA2" Offset="1"/>
                </LinearGradientBrush>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                    <GradientStop Color="#FFF093FB" Offset="0"/>
                    <GradientStop Color="#FFF5576C" Offset="1"/>
                </LinearGradientBrush>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                    <GradientStop Color="#FF4FACFE" Offset="0"/>
                    <GradientStop Color="#FF00F2FE" Offset="1"/>
                </LinearGradientBrush>
            </sunburst:SunburstColorModel.CustomBrushes>
        </sunburst:SunburstColorModel>
    </sunburst:SfSunburstChart.ColorModel>
    
</sunburst:SfSunburstChart>
```

### Example 3: Semi-Circle Dashboard Widget

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Metrics}" 
                          ValueMemberPath="Score"
                          Palette="BlueChrome"
                          StartAngle="180"
                          EndAngle="360"
                          Radius="0.95"
                          InnerRadius="0.4">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Metric"/>
    </sunburst:SfSunburstChart.Levels>
    
</sunburst:SfSunburstChart>
```

## Best Practices

### 1. Color Selection

- Use 4-6 distinct colors for clarity
- Ensure sufficient contrast between adjacent colors
- Match brand guidelines when applicable
- Test color-blind accessibility
- Avoid overly bright or neon colors

### 2. Gradient Usage

- Use gradients sparingly for professional look
- Keep gradient transitions smooth (2-3 colors max)
- Ensure gradients don't obscure data labels
- Test readability with labels and legends

### 3. Radius Configuration

- Leave margin space (Radius < 1.0)
- Adjust based on container size
- Reduce when legend/labels are outside
- Increase for focal point emphasis

### 4. Inner Radius

- Default (0.2) works for most cases
- Increase (0.4-0.5) for donut effect
- Use larger values (0.5-0.6) to display center content
- Avoid extremes (<0.1 or >0.7)

### 5. Angle Customization

- Full circle (0-360) is standard
- Semi-circles useful for gauges or limited space
- Ensure angles make logical sense for data
- Consider orientation relative to legends

## Troubleshooting

**Custom palette not applying:**
- Verify `Palette="Custom"` is set
- Check ColorModel is defined before ItemsSource binding
- Ensure CustomBrushes collection has items

**Gradients not displaying correctly:**
- Verify GradientStop Offset values (0.0 to 1.0)
- Check Color values are valid
- Ensure LinearGradientBrush syntax is correct

**Chart too small/large:**
- Adjust Radius property (0.0-1.0)
- Check container size
- Consider legend and label space requirements

**Center hole too large/small:**
- Adjust InnerRadius (0.0-1.0)
- Balance with number of hierarchy levels
- Ensure segments remain visible

**Angles not working:**
- Verify StartAngle and EndAngle are set correctly
- Remember: 0° = right, 90° = bottom, 180° = left, 270° = top
- Ensure EndAngle > StartAngle for clockwise rendering
