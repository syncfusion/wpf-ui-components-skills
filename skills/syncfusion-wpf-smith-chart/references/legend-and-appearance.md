# Legend and Appearance

## Table of Contents
- [Overview](#overview)
- [Legend Configuration](#legend-configuration)
- [Legend Positioning](#legend-positioning)
- [Legend Icons](#legend-icons)
- [Legend Customization](#legend-customization)
- [Series Visibility via Legend](#series-visibility-via-legend)
- [SmithChart Palettes](#smithchart-palettes)
- [Chart Area Customization](#chart-area-customization)
- [Circle Radius Control](#circle-radius-control)
- [Getting Chart Properties](#getting-chart-properties)

## Overview

This guide covers legend configuration and overall SmithChart appearance customization, including:

**Legend Features:**
- Adding and configuring legends
- Positioning (docked and floating)
- Custom icons and styling
- Interactive series visibility control

**Appearance Features:**
- Color palettes (chart and series-level)
- Chart area styling
- Circle radius adjustment
- Programmatic access to chart metrics

## Legend Configuration

Legends display series information and allow interactive series toggling.

### Adding a Legend

```xml
<syncfusion:SfSmithChart>
    <syncfusion:LineSeries Label="Transmission-1" 
                           ItemsSource="{Binding Data1}"/>
    <syncfusion:LineSeries Label="Transmission-2" 
                           ItemsSource="{Binding Data2}"/>
    
    <!-- Add Legend -->
    <syncfusion:SfSmithChart.Legend>
        <syncfusion:SmithChartLegend/>
    </syncfusion:SfSmithChart.Legend>
</syncfusion:SfSmithChart>
```

```csharp
// Create series with labels
LineSeries series1 = new LineSeries
{
    Label = "Transmission-1",
    ItemsSource = Data1,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance"
};

LineSeries series2 = new LineSeries
{
    Label = "Transmission-2",
    ItemsSource = Data2,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance"
};

chart.Series.Add(series1);
chart.Series.Add(series2);

// Add legend
SmithChartLegend legend = new SmithChartLegend();
chart.Legend = legend;
```

**Important:** Series must have a `Label` property set to appear in the legend.

### Legend Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `DockPosition` | ChartDock | Right | Legend position (Left, Right, Top, Bottom, Floating) |
| `OffsetX` | double | 0 | Horizontal offset (Floating mode) |
| `OffsetY` | double | 0 | Vertical offset (Floating mode) |
| `LegendIcon` | SmithChartLegendIcon | Circle | Icon shape |
| `LegendIconTemplate` | DataTemplate | null | Custom icon template |
| `IconHeight` | double | 12 | Icon height |
| `IconWidth` | double | 12 | Icon width |
| `ItemMargin` | Thickness | 10 | Spacing between items |
| `FontSize` | double | - | Legend text size |
| `FontWeight` | FontWeight | - | Legend text weight |
| `Foreground` | Brush | - | Text color |
| `Background` | Brush | - | Legend background |
| `BorderBrush` | Brush | - | Legend border color |
| `BorderThickness` | Thickness | - | Legend border thickness |

## Legend Positioning

### Docked Position

Position legend at chart edges:

**Right:**
```xml
<syncfusion:SmithChartLegend DockPosition="Right"/>
```

**Bottom:**
```xml
<syncfusion:SmithChartLegend DockPosition="Bottom"/>
```

**Left:**
```xml
<syncfusion:SmithChartLegend DockPosition="Left"/>
```

**Top:**
```xml
<syncfusion:SmithChartLegend DockPosition="Top"/>
```
**Floating:**
```xml
<syncfusion:SmithChartLegend DockPosition="Floating"/>
```

```csharp
SmithChartLegend legend = new SmithChartLegend();
legend.DockPosition = ChartDock.Bottom;
chart.Legend = legend;
```

### Floating Position

Position legend anywhere inside the chart:

```xml
<syncfusion:SmithChartLegend DockPosition="Floating" 
                             OffsetX="200" 
                             OffsetY="120"/>
```

```csharp
SmithChartLegend legend = new SmithChartLegend();
legend.DockPosition = ChartDock.Floating;
legend.OffsetX = 200;  // X position from left
legend.OffsetY = 120;  // Y position from top
chart.Legend = legend;
```

**Offset Guidelines:**
- **OffsetX**: Distance from left edge in pixels
- **OffsetY**: Distance from top edge in pixels
- Use Floating for precise legend placement
- Ensure legend doesn't obscure important data

### Choosing Legend Position

**Use Right/Left for:**
- Standard chart layouts
- Wide charts
- Vertical legend orientation preferred

**Use Top/Bottom for:**
- Tall charts
- Horizontal legend layout
- Multiple series with short labels

**Use Floating for:**
- Custom layouts
- Specific positioning requirements
- Overlaying legend on empty chart areas

## Legend Icons

### Built-in Legend Icons

Available icon shapes:

```csharp
public enum SmithChartLegendIcon
{
    Circle,          // Round icon
    Rectangle,       // Square/rectangular icon
    Triangle,        // Triangle icon
    Diamond,         // Diamond icon
    Pentagon,        // Pentagon icon
    HorizontalLine,  // Horizontal line icon
    Cross,           // Cross/plus icon
    Custom           // Use LegendIconTemplate
}
```

### Setting Legend Icon

```xml
<syncfusion:SmithChartLegend LegendIcon="HorizontalLine"/>
```

```csharp
legend.LegendIcon = SmithChartLegendIcon.HorizontalLine;
```

### Icon Examples

**Circle Icon:**
```csharp
legend.LegendIcon = SmithChartLegendIcon.Circle;
```

**Diamond Icon:**
```csharp
legend.LegendIcon = SmithChartLegendIcon.Diamond;
```

**Horizontal Line Icon:**
```csharp
legend.LegendIcon = SmithChartLegendIcon.HorizontalLine;
```

### Custom Legend Icon Template

Create custom icons using DataTemplate:

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <DataTemplate x:Key="customIcon">
            <Ellipse Stretch="Fill" 
                     Fill="Black" 
                     Width="12" 
                     Height="5"/>
        </DataTemplate>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:SfSmithChart.Legend>
        <syncfusion:SmithChartLegend LegendIcon="Custom" 
                                     LegendIconTemplate="{StaticResource customIcon}"/>
    </syncfusion:SfSmithChart.Legend>
</syncfusion:SfSmithChart>
```

**Template Binding:**
- **Interior**: Series color (use for Fill)
- Automatically binds to series properties

**Custom Icon Examples:**

**Star Icon:**
```xml
<DataTemplate x:Key="starIcon">
    <Path Fill="Black" 
          Data="M 6,0 L 7,4 L 12,4 L 8,7 L 10,12 L 6,9 L 2,12 L 4,7 L 0,4 L 5,4 Z"/>
</DataTemplate>
```

**Gradient Icon:**
```xml
<DataTemplate x:Key="gradientIcon">
    <Rectangle Width="16" Height="10">
        <Rectangle.Fill>
            <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                <GradientStop Color="AliceBlue"  Offset="0"/>
                <GradientStop Color="White" Offset="1"/>
            </LinearGradientBrush>
        </Rectangle.Fill>
    </Rectangle>
</DataTemplate>
```

## Legend Customization

### Icon Size

Adjust legend icon dimensions:

```xml
<syncfusion:SmithChartLegend IconHeight="15" 
                             IconWidth="15"/>
```

```csharp
legend.IconHeight = 15;
legend.IconWidth = 15;
```

### Item Spacing

Control space between legend items:

```xml
<syncfusion:SmithChartLegend ItemMargin="10"/>
```

```csharp
legend.ItemMargin = new Thickness(10);
```

### Text Styling

Customize legend text appearance:

```xml
<syncfusion:SmithChartLegend FontSize="13" 
                             FontWeight="Bold"
                             Foreground="Red"/>
```

```csharp
legend.FontSize = 13;
legend.FontWeight = FontWeights.Bold;
legend.Foreground = new SolidColorBrush(Colors.Red);
```

### Background and Border

Style legend container:

```xml
<syncfusion:SmithChartLegend Background="LightGray" 
                             BorderBrush="CadetBlue"
                             BorderThickness="3"/>
```

```csharp
legend.Background = new SolidColorBrush(Colors.LightGray);
legend.BorderBrush = new SolidColorBrush(Colors.CadetBlue);
legend.BorderThickness = new Thickness(3);
```

### Complete Legend Customization Example

```csharp
SmithChartLegend legend = new SmithChartLegend
{
    DockPosition = ChartDock.Bottom,
    LegendIcon = SmithChartLegendIcon.HorizontalLine,
    IconHeight = 15,
    IconWidth = 15,
    ItemMargin = new Thickness(10),
    FontSize = 13,
    FontWeight = FontWeights.Bold,
    Foreground = new SolidColorBrush(Colors.DarkBlue),
    Background = new SolidColorBrush(Colors.LightSkyBlue),
    BorderBrush = new SolidColorBrush(Colors.CadetBlue),
    BorderThickness = new Thickness(2)
};
chart.Legend = legend;
```

## Series Visibility via Legend

### VisibilityOnLegend Property

Control whether a series appears in the legend:

```xml
<syncfusion:LineSeries VisibilityOnLegend="False" 
                       Label="Transmission-1"
                       ItemsSource="{Binding Data1}"/>
```

```csharp
series.VisibilityOnLegend = false;  // Hide from legend
```

**Use cases:**
- Hide helper/reference series from legend
- Reduce legend clutter
- Show only primary series

### Interactive Series Visibility

Use `IsSeriesVisible` to control series display:

```xml
<syncfusion:LineSeries IsSeriesVisible="False" 
                       Label="Transmission-1"
                       ItemsSource="{Binding Data1}"/>
```

```csharp
series.IsSeriesVisible = false;  // Initially hidden
```

**Behavior:**
- Hidden series show as shaded in legend
- Clicking legend item toggles series visibility
- Series data remains in memory
- Useful for comparing subsets of data

### Example: Toggling Multiple Series

```xml
<syncfusion:SfSmithChart>
    <!-- Initially hidden -->
    <syncfusion:LineSeries IsSeriesVisible="False" 
                           Label="50Ω Line"
                           ItemsSource="{Binding Data1}"/>
    
    <!-- Visible -->
    <syncfusion:LineSeries Label="75Ω Line"
                           ItemsSource="{Binding Data2}"/>
    
    <!-- Visible -->
    <syncfusion:LineSeries Label="100Ω Line"
                           ItemsSource="{Binding Data3}"/>
    
    <syncfusion:SfSmithChart.Legend>
        <syncfusion:SmithChartLegend DockPosition="Bottom"/>
    </syncfusion:SfSmithChart.Legend>
</syncfusion:SfSmithChart>
```

**User can click legend items to show/hide series interactively.**

## SmithChart Palettes

### Chart-Level Palette

Apply color palette to all series:

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.ColorModel>
        <syncfusion:SmithChartColorModel Palette="BlueChrome"/>
    </syncfusion:SfSmithChart.ColorModel>
</syncfusion:SfSmithChart>
```

```csharp
chart.ColorModel = new SmithChartColorModel();
chart.ColorModel.Palette = ColorPalette.BlueChrome;
```

### Available Palettes

Common palette options:
- **Metro**: Modern, vibrant colors (default)
- **BlueChrome**: Professional blue tones
- **AutumnBrights**: Warm autumn colors
- **Custom**: Custom palette will be set, and color values will be taken from CustomBrushes collection.
- **Elite**: Elite palette will be set
- **FloraHues**: FloraHues palette will be set
- **GreenChrome**: GreenChrome palette will be set
- **SandyBeach**: Beige and blue tones
- **LightCandy**:LightCandy palette will be set.
- **None**: No palette will be set.
- **Pineapple**: Pineapple palette will be set.
- **PurpleChrome**: PurpleChrome palette will be set.
- **RedChrome**: RedChrome palette will be set.
- **TomatoSpectrum**: TomatoSpectrum palette will be set

### Series-Specific Palette

Override chart palette for individual series:

```xml
<syncfusion:LineSeries>
    <syncfusion:LineSeries.ColorModel>
        <syncfusion:SmithChartColorModel Palette="Metro"/>
    </syncfusion:LineSeries.ColorModel>
</syncfusion:LineSeries>
```

```csharp
series.ColorModel = new SmithChartColorModel();
series.ColorModel.Palette = ColorPalette.Metro;
```

### Palette Priority

1. **Explicit Interior property** (highest priority)
2. **Series ColorModel.Palette**
3. **Chart ColorModel.Palette**
4. **Default (Metro)**

### Example: Mixed Palette Usage

```csharp
// Chart-level palette
chart.ColorModel = new SmithChartColorModel();
chart.ColorModel.Palette = ColorPalette.BlueChrome;

// Series 1: Uses chart palette (BlueChrome)
LineSeries series1 = new LineSeries
{
    Label = "Series 1",
    ItemsSource = Data1
};

// Series 2: Overrides with Metro palette
LineSeries series2 = new LineSeries
{
    Label = "Series 2",
    ItemsSource = Data2,
    ColorModel = new SmithChartColorModel { Palette = ColorPalette.Metro }
};

// Series 3: Uses explicit color (highest priority)
LineSeries series3 = new LineSeries
{
    Label = "Series 3",
    ItemsSource = Data3,
    Interior = new SolidColorBrush(Colors.Purple)
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
```

## Chart Area Customization

### Chart Background and Border

Customize the overall chart appearance:

```xml
<syncfusion:SfSmithChart Background="LightSteelBlue" 
                         BorderBrush="CadetBlue" 
                         BorderThickness="4"/>
```

```csharp
chart.Background = new SolidColorBrush(Colors.LightSteelBlue);
chart.BorderBrush = new SolidColorBrush(Colors.CadetBlue);
chart.BorderThickness = new Thickness(4);
```

### Chart Area (Circle Plot Area)

Customize the circular plotting area:

```xml
<syncfusion:SfSmithChart ChartAreaBackground="AliceBlue" 
                         ChartAreaBorderBrush="SkyBlue" 
                         ChartAreaBorderThickness="2"/>
```

```csharp
chart.ChartAreaBackground = new SolidColorBrush(Colors.AliceBlue);
chart.ChartAreaBorderBrush = new SolidColorBrush(Colors.SkyBlue);
chart.ChartAreaBorderThickness = new Thickness(2);
```

### Complete Appearance Customization

```csharp
SfSmithChart chart = new SfSmithChart
{
    // Overall chart styling
    Background = new SolidColorBrush(Colors.WhiteSmoke),
    BorderBrush = new SolidColorBrush(Colors.Gray),
    BorderThickness = new Thickness(2),
    
    // Chart area (circle) styling
    ChartAreaBackground = new SolidColorBrush(Colors.White),
    ChartAreaBorderBrush = new SolidColorBrush(Colors.DarkGray),
    ChartAreaBorderThickness = new Thickness(1),
    
    // Header
    Header = "Impedance Transmission",
    
    // Size
    Height = 500,
    Width = 500
};
```

### Professional Appearance Example

```csharp
// Clean, professional look
chart.Background = new SolidColorBrush(Color.FromRgb(245, 245, 245));
chart.BorderBrush = new SolidColorBrush(Color.FromRgb(180, 180, 180));
chart.BorderThickness = new Thickness(1);

chart.ChartAreaBackground = new SolidColorBrush(Colors.White);
chart.ChartAreaBorderBrush = new SolidColorBrush(Color.FromRgb(200, 200, 200));
chart.ChartAreaBorderThickness = new Thickness(1);
```

## Circle Radius Control

### Radius Property

Adjust the diameter of the SmithChart circle relative to the plot area:

```xml
<syncfusion:SfSmithChart Radius="0.5" 
                         ChartAreaBorderBrush="CadetBlue"/>
```

```csharp
chart.Radius = 0.5;  // 50% of available space
chart.ChartAreaBorderBrush = new SolidColorBrush(Colors.CadetBlue);
```

**Valid Range:** 0.1 to 1.0
**Default:** 0.95 (95% of available space)

### Radius Values

- **0.1 - 0.5**: Small circle, large margin
- **0.6 - 0.8**: Medium circle, balanced
- **0.85 - 0.95**: Standard size (recommended)
- **0.95 - 1.0**: Maximum size, minimal margin

### Use Cases for Radius

**Small Radius (0.5 - 0.7):**
- Large legend or annotations
- Compact chart in busy layout
- Emphasize surrounding content

**Medium Radius (0.75 - 0.85):**
- Balanced layout
- External labels need space
- Multiple charts side by side

**Large Radius (0.9 - 0.95):**
- Maximize data visualization
- Inside label placement
- Full-screen or primary chart

**Maximum Radius (0.95 - 1.0):**
- Detailed data analysis
- Publication/presentation
- No external decoration needed

### Example: Adjusting for External Labels

```csharp
// Reduce radius to accommodate outside labels
chart.Radius = 0.85;
chart.HorizontalAxis.LabelPlacement = LabelPlacement.Outside;
chart.RadialAxis.LabelPlacement = LabelPlacement.Outside;
```

## Getting Chart Properties

### ChartAreaInfo Properties

Access chart metrics programmatically:

```csharp
// Get area bounds
Rect areaBounds = chart.ChartAreaInfo.AreaBounds;
double left = areaBounds.Left;
double top = areaBounds.Top;
double width = areaBounds.Width;
double height = areaBounds.Height;

// Get center point
Point centerPoint = chart.ChartAreaInfo.CenterPoint;
double centerX = centerPoint.X;
double centerY = centerPoint.Y;

// Get radius
double radius = chart.ChartAreaInfo.Radius;
```

### Use Cases for Chart Properties

**Position Custom Elements:**
```csharp
// Position custom annotation at center
Point center = chart.ChartAreaInfo.CenterPoint;
myAnnotation.SetValue(Canvas.LeftProperty, center.X);
myAnnotation.SetValue(Canvas.TopProperty, center.Y);
```

**Calculate Positions:**
```csharp
// Place element at edge of circle
Point center = chart.ChartAreaInfo.CenterPoint;
double radius = chart.ChartAreaInfo.Radius;
double edgeX = center.X + radius;
double edgeY = center.Y;
```

**Dynamic Layout:**
```csharp
// Adjust other UI based on chart size
Rect bounds = chart.ChartAreaInfo.AreaBounds;
sidePanel.Width = parentWidth - bounds.Width - 20;
```

## Common Patterns

### Pattern 1: Professional Chart with Bottom Legend

```csharp
SfSmithChart chart = new SfSmithChart
{
    Header = "Transmission Line Analysis",
    Background = new SolidColorBrush(Colors.White),
    ChartAreaBackground = new SolidColorBrush(Color.FromRgb(250, 250, 250)),
    ChartAreaBorderBrush = new SolidColorBrush(Colors.Gray),
    ChartAreaBorderThickness = new Thickness(1),
    Radius = 0.9
};

SmithChartLegend legend = new SmithChartLegend
{
    DockPosition = ChartDock.Bottom,
    LegendIcon = SmithChartLegendIcon.HorizontalLine,
    IconHeight = 3,
    IconWidth = 20,
    FontSize = 11,
    Background = new SolidColorBrush(Colors.Transparent)
};
chart.Legend = legend;
```

### Pattern 2: Colorful Themed Chart

```csharp
// Apply BlueChrome palette
chart.ColorModel = new SmithChartColorModel();
chart.ColorModel.Palette = ColorPalette.BlueChrome;

// Matching appearance
chart.Background = new SolidColorBrush(Color.FromRgb(240, 245, 255));
chart.ChartAreaBackground = new SolidColorBrush(Colors.White);

SmithChartLegend legend = new SmithChartLegend
{
    DockPosition = ChartDock.Right,
    Background = new SolidColorBrush(Color.FromRgb(240, 245, 255)),
    BorderBrush = new SolidColorBrush(Colors.SteelBlue),
    BorderThickness = new Thickness(1)
};
chart.Legend = legend;
```

### Pattern 3: Compact Chart with Floating Legend

```csharp
chart.Radius = 0.8;  // Smaller circle for space

SmithChartLegend legend = new SmithChartLegend
{
    DockPosition = ChartDock.Floating,
    OffsetX = 50,
    OffsetY = 50,
    Background = new SolidColorBrush(Color.FromArgb(230, 255, 255, 255)),
    BorderBrush = new SolidColorBrush(Colors.Gray),
    BorderThickness = new Thickness(1),
    FontSize = 10
};
chart.Legend = legend;
```

### Pattern 4: Custom Icon Legend

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <DataTemplate x:Key="customIcon">
            <Rectangle Fill="{Binding Interior}" 
                       Width="20" 
                       Height="4" 
                       RadiusX="2" 
                       RadiusY="2"/>
        </DataTemplate>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:SfSmithChart.Legend>
        <syncfusion:SmithChartLegend LegendIcon="Custom"
                                     LegendIconTemplate="{StaticResource customIcon}"
                                     DockPosition="Bottom"/>
    </syncfusion:SfSmithChart.Legend>
</syncfusion:SfSmithChart>
```

## Summary

Legend and appearance customization provide extensive control over SmithChart presentation:

**Legend Features:**
- Add legends to display series information
- Position legends at chart edges or floating
- Customize legend icons (built-in or custom templates)
- Style legend appearance (colors, fonts, spacing)
- Control series visibility interactively via legend

**Appearance Features:**
- Apply color palettes at chart or series level
- Customize chart background and borders
- Style chart area (circle plot region)
- Adjust circle radius for layout flexibility
- Access chart properties programmatically for advanced scenarios

These features enable creating professional, branded, and highly functional SmithChart visualizations tailored to specific requirements.