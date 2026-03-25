# Palettes in WPF Surface Chart

## Table of Contents
- [Overview](#overview)
- [Predefined Palettes](#predefined-palettes)
- [Applying Predefined Palettes](#applying-predefined-palettes)
- [Custom Palettes](#custom-palettes)
- [Palette Selection Guide](#palette-selection-guide)
- [Best Practices](#best-practices)

## Overview

WPF Surface Chart provides options to apply different kinds of color palettes to enhance data visualization. Palettes define the color scheme used to represent values across the surface.

**Key Concepts:**
- **Predefined Palettes:** 12 built-in color schemes
- **Custom Palettes:** Create your own color combinations
- **Color Mapping:** Colors automatically map to Y-axis value ranges
- **Integration:** Works seamlessly with ColorBar for value interpretation

## Predefined Palettes

### Available Palettes

Syncfusion provides 12 predefined palettes optimized for various visualization needs:

1. **Metro** - Modern, professional colors
2. **AutumnBrights** - Warm, autumn-inspired hues
3. **FloraHues** - Floral, natural color tones
4. **Pineapple** - Tropical, vibrant colors
5. **TomotoSpectrum** - Red-based gradient
6. **RedChrome** - Red chromatic scale
7. **PurpleChrome** - Purple chromatic scale
8. **BlueChrome** - Blue chromatic scale
9. **GreenChrome** - Green chromatic scale
10. **Elite** - Elegant, refined colors
11. **LightCandy** - Light, pastel tones
12. **SandyBeach** - Beach-inspired colors

Each palette is designed to provide good contrast and visual clarity for different types of data and presentation contexts.

## Applying Predefined Palettes

### XAML Implementation

```xaml
<chart:SfSurfaceChart Palette="Metro">
</chart:SfSurfaceChart>
```

**Other Palette Examples:**

```xaml
<!-- Autumn-inspired -->
<chart:SfSurfaceChart Palette="AutumnBrights">
</chart:SfSurfaceChart>

<!-- Floral tones -->
<chart:SfSurfaceChart Palette="FloraHues">
</chart:SfSurfaceChart>

<!-- Tropical colors -->
<chart:SfSurfaceChart Palette="Pineapple">
</chart:SfSurfaceChart>

<!-- Blue gradient -->
<chart:SfSurfaceChart Palette="BlueChrome">
</chart:SfSurfaceChart>

<!-- Green gradient -->
<chart:SfSurfaceChart Palette="GreenChrome">
</chart:SfSurfaceChart>
```

### Code-Behind Implementation

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Palette = ChartColorPalette.Metro;
grid.Children.Add(chart);
```

**Switching Palettes Dynamically:**

```csharp
// Different palette options
chart.Palette = ChartColorPalette.Metro;
chart.Palette = ChartColorPalette.AutumnBrights;
chart.Palette = ChartColorPalette.FloraHues;
chart.Palette = ChartColorPalette.Pineapple;
chart.Palette = ChartColorPalette.TomotoSpectrum;
chart.Palette = ChartColorPalette.RedChrome;
chart.Palette = ChartColorPalette.PurpleChrome;
chart.Palette = ChartColorPalette.BlueChrome;
chart.Palette = ChartColorPalette.GreenChrome;
chart.Palette = ChartColorPalette.Elite;
chart.Palette = ChartColorPalette.LightCandy;
chart.Palette = ChartColorPalette.SandyBeach;
```

### Complete Example with Palette

```xaml
<chart:SfSurfaceChart Palette="Metro"
                     Type="Surface"
                     ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}">
    
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

## Custom Palettes

### Overview

The custom palette option allows you to define your own color scheme using the ColorModel property with CustomBrushes collection.

**When to Use Custom Palettes:**
- Brand-specific colors required
- Domain-specific color conventions (e.g., red=hot, blue=cold)
- Accessibility requirements (colorblind-friendly palettes)
- Matching existing design systems

### XAML Custom Palette

```xaml
<chart:SfSurfaceChart Palette="Custom">
    <chart:SfSurfaceChart.ColorModel>
        <chart:ChartColorModel>
            <chart:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="Blue"/>
                <SolidColorBrush Color="Lime"/>
                <SolidColorBrush Color="Yellow"/>
                <SolidColorBrush Color="Orange"/>
                <SolidColorBrush Color="Red"/>
            </chart:ChartColorModel.CustomBrushes>
        </chart:ChartColorModel>
    </chart:SfSurfaceChart.ColorModel>
</chart:SfSurfaceChart>
```

### Code-Behind Custom Palette

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Palette = ChartColorPalette.Custom;

ChartColorModel colorModel = new ChartColorModel();
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Blue));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Lime));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Yellow));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Orange));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Red));

chart.ColorModel = colorModel;
grid.Children.Add(chart);
```

### Custom Palette Examples

#### Heat Map Palette (Cold to Hot)

```csharp
ChartColorModel heatMapPalette = new ChartColorModel();
heatMapPalette.CustomBrushes.Add(new SolidColorBrush(Colors.Blue));        // Cold
heatMapPalette.CustomBrushes.Add(new SolidColorBrush(Colors.Cyan));
heatMapPalette.CustomBrushes.Add(new SolidColorBrush(Colors.Green));
heatMapPalette.CustomBrushes.Add(new SolidColorBrush(Colors.Yellow));
heatMapPalette.CustomBrushes.Add(new SolidColorBrush(Colors.Orange));
heatMapPalette.CustomBrushes.Add(new SolidColorBrush(Colors.Red));         // Hot

chart.Palette = ChartColorPalette.Custom;
chart.ColorModel = heatMapPalette;
```

#### Elevation Palette (Topographical)

```csharp
ChartColorModel elevationPalette = new ChartColorModel();
elevationPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(0, 0, 139)));      // Deep blue (ocean)
elevationPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(34, 139, 34)));    // Green (lowland)
elevationPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(139, 90, 0)));     // Brown (hills)
elevationPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(105, 105, 105))); // Gray (mountains)
elevationPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(255, 255, 255))); // White (peaks)

chart.Palette = ChartColorPalette.Custom;
chart.ColorModel = elevationPalette;
```

#### Grayscale Palette

```csharp
ChartColorModel grayscalePalette = new ChartColorModel();
grayscalePalette.CustomBrushes.Add(new SolidColorBrush(Colors.Black));
grayscalePalette.CustomBrushes.Add(new SolidColorBrush(Colors.DarkGray));
grayscalePalette.CustomBrushes.Add(new SolidColorBrush(Colors.Gray));
grayscalePalette.CustomBrushes.Add(new SolidColorBrush(Colors.LightGray));
grayscalePalette.CustomBrushes.Add(new SolidColorBrush(Colors.White));

chart.Palette = ChartColorPalette.Custom;
chart.ColorModel = grayscalePalette;
```

#### Brand Colors Palette

```csharp
ChartColorModel brandPalette = new ChartColorModel();
// Example using company brand colors
brandPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(0, 83, 155)));    // Brand Primary
brandPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(0, 159, 227)));   // Brand Secondary
brandPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(255, 203, 5)));   // Brand Accent
brandPalette.CustomBrushes.Add(new SolidColorBrush(Color.FromRgb(236, 0, 140)));   // Brand Highlight

chart.Palette = ChartColorPalette.Custom;
chart.ColorModel = brandPalette;
```

### Complete Custom Palette Example

```xaml
<chart:SfSurfaceChart Palette="Custom"
                     Type="Surface"
                     ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     ApplyGradientBrush="True">
    
    <chart:SfSurfaceChart.ColorModel>
        <chart:ChartColorModel>
            <chart:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="Blue"/>
                <SolidColorBrush Color="Cyan"/>
                <SolidColorBrush Color="Lime"/>
                <SolidColorBrush Color="Yellow"/>
                <SolidColorBrush Color="Orange"/>
                <SolidColorBrush Color="Red"/>
            </chart:ChartColorModel.CustomBrushes>
        </chart:ChartColorModel>
    </chart:SfSurfaceChart.ColorModel>
    
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

## Palette Selection Guide

### By Use Case

**Scientific Visualization:**
- **BlueChrome** - Temperature data (cold)
- **RedChrome** - Temperature data (hot)
- **Metro** - General purpose scientific data

**Engineering Analysis:**
- **Custom (Blue-Red)** - Stress/temperature distribution
- **GreenChrome** - Safety/efficiency metrics
- **TomotoSpectrum** - Heat/pressure analysis

**Business Presentations:**
- **Metro** - Professional, corporate look
- **Elite** - Executive presentations
- **FloraHues** - Eco-friendly, natural data

**Academic/Research:**
- **BlueChrome** or **GreenChrome** - Neutral, professional
- **Metro** - Papers and publications
- **Grayscale (Custom)** - Black and white printing

**Creative/Marketing:**
- **Pineapple** - Vibrant, eye-catching
- **SandyBeach** - Relaxed, approachable
- **LightCandy** - Soft, friendly

### By Data Type

**Temperature Data:**
```csharp
// Cold to hot gradient
chart.Palette = ChartColorPalette.Custom;
// Blue → Cyan → Green → Yellow → Orange → Red
```

**Elevation/Topography:**
```csharp
// Sea level to peaks
chart.Palette = ChartColorPalette.Custom;
// Blue → Green → Brown → Gray → White
```

**Pressure/Stress:**
```csharp
// Low to high
chart.Palette = ChartColorPalette.TomotoSpectrum;
// or BlueChrome to RedChrome
```

**Generic Mathematical Functions:**
```csharp
// Clean, professional
chart.Palette = ChartColorPalette.Metro;
// or BlueChrome
```

## Best Practices

### 1. Match Palette to Data Domain

```csharp
// Temperature data - use thermal colors
if (dataType == DataType.Temperature)
{
    chart.Palette = ChartColorPalette.Custom;
    // Blue (cold) to Red (hot)
}
// Elevation data - use topographical colors
else if (dataType == DataType.Elevation)
{
    chart.Palette = ChartColorPalette.Custom;
    // Blue (low) to White (high)
}
```

### 2. Consider Audience and Context

```csharp
// Professional/corporate
chart.Palette = ChartColorPalette.Metro;

// Academic presentation
chart.Palette = ChartColorPalette.BlueChrome;

// Marketing material
chart.Palette = ChartColorPalette.Pineapple;
```

### 3. Ensure Sufficient Contrast

```csharp
// Good contrast for visibility
ChartColorModel highContrastPalette = new ChartColorModel();
highContrastPalette.CustomBrushes.Add(new SolidColorBrush(Colors.DarkBlue));
highContrastPalette.CustomBrushes.Add(new SolidColorBrush(Colors.Yellow));
highContrastPalette.CustomBrushes.Add(new SolidColorBrush(Colors.Red));
```

### 4. Test for Colorblind Accessibility

```csharp
// Avoid red-green combinations for colorblind users
// Use blue-yellow or include other visual cues
ChartColorModel accessiblePalette = new ChartColorModel();
accessiblePalette.CustomBrushes.Add(new SolidColorBrush(Colors.Blue));
accessiblePalette.CustomBrushes.Add(new SolidColorBrush(Colors.Orange));
accessiblePalette.CustomBrushes.Add(new SolidColorBrush(Colors.Yellow));
```

### 5. Use ApplyGradientBrush for Smoothness

```xaml
<chart:SfSurfaceChart Palette="BlueChrome" ApplyGradientBrush="True" BrushCount="10">
</chart:SfSurfaceChart>
```

Smooth gradients are more professional and easier to interpret.

### 6. Always Include a ColorBar

```xaml
<chart:SfSurfaceChart Palette="Metro">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

Color bars are essential for interpreting what colors mean.

### 7. Allow Users to Switch Palettes

```csharp
// Provide palette selection UI
private void PaletteComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string selectedPalette = (PaletteComboBox.SelectedItem as ComboBoxItem)?.Content.ToString();
    
    switch (selectedPalette)
    {
        case "Metro":
            chart.Palette = ChartColorPalette.Metro;
            break;
        case "Blue Chrome":
            chart.Palette = ChartColorPalette.BlueChrome;
            break;
        case "Heat Map":
            ApplyHeatMapPalette();
            break;
        // Add more options...
    }
}
```

---
