# Shapes Color Customization in WPF Maps (SfMap)

## Table of Contents
- [Overview](#overview)
- [Basic Color Properties](#basic-color-properties)
- [AutoFillColors Property](#autofillcolors-property)
- [Range Color Mapping](#range-color-mapping)
- [Equals Color Mapping](#equals-color-mapping)
- [Complete Examples](#complete-examples)

## Overview

WPF Maps provides multiple approaches to customize shape colors based on bound data values. You can use simple fill colors, automatic color palettes, or data-driven color mapping for choropleth map visualizations.

**Color customization methods:**
1. Basic properties (ShapeFill, ShapeStroke, ShapeStrokeThickness)
2. Tree map-like support with color mapping
3. Automatic color palettes

The `AutoFillColors` property in `ShapeFillSetting` controls which method is used.

## Basic Color Properties

These properties in `ShapeSetting` control shape appearance when AutoFillColors is false.

### ShapeFill

Sets the fill color for all shapes.

**Type:** Brush  
**Default:** System default

```xml
<syncfusion:ShapeFileLayer.ShapeSettings>
    <syncfusion:ShapeSetting ShapeFill="LightBlue" />
</syncfusion:ShapeFileLayer.ShapeSettings>
```

```csharp
ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeFill = new SolidColorBrush(Colors.LightBlue);
```

### ShapeStroke

Sets the border color for shapes.

**Type:** Brush  
**Default:** Transparent

```xml
<syncfusion:ShapeSetting ShapeStroke="DarkBlue" />
```

```csharp
shapeSetting.ShapeStroke = new SolidColorBrush(Colors.DarkBlue);
```

### ShapeStrokeThickness

Sets the border width.

**Type:** double  
**Default:** 0

```xml
<syncfusion:ShapeSetting ShapeStrokeThickness="1.5" />
```

```csharp
shapeSetting.ShapeStrokeThickness = 1.5;
```

### Basic Example

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeFill="Gray" 
                                 ShapeStroke="Black" 
                                 ShapeStrokeThickness="1">
            <syncfusion:ShapeSetting.FillSetting>
                <syncfusion:ShapeFillSetting AutoFillColors="False" />
            </syncfusion:ShapeSetting.FillSetting>
        </syncfusion:ShapeSetting>
    </syncfusion:ShapeFileLayer.ShapeSettings>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";

ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeFill = new SolidColorBrush(Colors.Gray);
shapeSetting.ShapeStroke = new SolidColorBrush(Colors.Black);
shapeSetting.ShapeStrokeThickness = 1;

ShapeFillSetting fillSetting = new ShapeFillSetting();
fillSetting.AutoFillColors = false;
shapeSetting.FillSetting = fillSetting;

layer.ShapeSettings = shapeSetting;
```

## AutoFillColors Property

Controls whether colors are automatically assigned or manually configured.

**Type:** bool  
**Location:** ShapeFillSetting  
**Default:** true

**When true:**
- Maps automatically assigns colors from built-in palette
- Each shape receives a different color
- No data-driven coloring

**When false:**
- Use ShapeFill for uniform color, OR
- Use ColorMappings for data-driven colors

```xml
<syncfusion:ShapeSetting.FillSetting>
    <syncfusion:ShapeFillSetting AutoFillColors="False" />
</syncfusion:ShapeSetting.FillSetting>
```

```csharp
ShapeFillSetting fillSetting = new ShapeFillSetting();
fillSetting.AutoFillColors = false;
shapeSetting.FillSetting = fillSetting;
```

## Range Color Mapping

Assigns colors based on numeric value ranges - ideal for continuous data like population, temperature, or income.

### Configuration

**Required properties:**
- **AutoFillColors** = false
- **ShapeValuePath** - Property with numeric data
- **ColorMappings** - Collection of RangeColorMapping

### RangeColorMapping Properties

**From** (double) - Start of range (inclusive)  
**To** (double) - End of range (inclusive)  
**Color** (Color) - Color for this range  
**LegendLabel** (string) - Text shown in legend

### Basic Range Mapping

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           ItemsSource="{Binding States}"
                           ShapeIDPath="Name"
                           ShapeIDTableField="STATE_NAME">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeValuePath="Population"
                                 ShapeStroke="White"
                                 ShapeStrokeThickness="1">
            <syncfusion:ShapeSetting.FillSetting>
                <syncfusion:ShapeFillSetting AutoFillColors="False">
                    <syncfusion:ShapeFillSetting.ColorMappings>
                        <syncfusion:RangeColorMapping From="0" To="5000000" 
                                                      Color="#FEE5D9" 
                                                      LegendLabel="0 - 5M" />
                        <syncfusion:RangeColorMapping From="5000000" To="15000000" 
                                                      Color="#FCAE91" 
                                                      LegendLabel="5M - 15M" />
                        <syncfusion:RangeColorMapping From="15000000" To="25000000" 
                                                      Color="#FB6A4A" 
                                                      LegendLabel="15M - 25M" />
                        <syncfusion:RangeColorMapping From="25000000" To="40000000" 
                                                      Color="#CB181D" 
                                                      LegendLabel="25M - 40M" />
                    </syncfusion:ShapeFillSetting.ColorMappings>
                </syncfusion:ShapeFillSetting>
            </syncfusion:ShapeSetting.FillSetting>
        </syncfusion:ShapeSetting>
    </syncfusion:ShapeFileLayer.ShapeSettings>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";
layer.ItemsSource = viewModel.States;
layer.ShapeIDPath = "Name";
layer.ShapeIDTableField = "STATE_NAME";

ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeValuePath = "Population";
shapeSetting.ShapeStroke = new SolidColorBrush(Colors.White);
shapeSetting.ShapeStrokeThickness = 1;

ShapeFillSetting fillSetting = new ShapeFillSetting();
fillSetting.AutoFillColors = false;

// Add range color mappings
fillSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 0,
    To = 5000000,
    Color = (Color)ColorConverter.ConvertFromString("#FEE5D9"),
    LegendLabel = "0 - 5M"
});

fillSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 5000000,
    To = 15000000,
    Color = (Color)ColorConverter.ConvertFromString("#FCAE91"),
    LegendLabel = "5M - 15M"
});

fillSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 15000000,
    To = 25000000,
    Color = (Color)ColorConverter.ConvertFromString("#FB6A4A"),
    LegendLabel = "15M - 25M"
});

fillSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 25000000,
    To = 40000000,
    Color = (Color)ColorConverter.ConvertFromString("#CB181D"),
    LegendLabel = "25M - 40M"
});

shapeSetting.FillSetting = fillSetting;
layer.ShapeSettings = shapeSetting;
```

### Range Mapping Tips

**Continuous ranges:**
```csharp
// Good - no gaps
Range 1: From = 0,   To = 100
Range 2: From = 100, To = 500
Range 3: From = 500, To = 1000

// Values at boundaries (e.g., 100) belong to the first matching range
```

**Overlapping ranges:**
```csharp
// Avoid overlaps - first matching range wins
```

**Color progression:**
```csharp
// Use color scales for gradual transition
// Light to dark: #FEE5D9 → #FCAE91 → #FB6A4A → #CB181D
```

## Equals Color Mapping

Assigns colors based on categorical values - ideal for discrete categories like election results, climate zones, or product types.

### Configuration

**Required properties:**
- **AutoFillColors** = false
- **ShapeColorValuePath** - Property with categorical data
- **ColorMappings** - Collection of EqualsColorMapping

### EqualsColorMapping Properties

**Value** (string) - Exact value to match  
**Color** (Color) - Color for this value  
**LegendLabel** (string) - Text shown in legend

### Basic Equals Mapping

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           ItemsSource="{Binding ElectionResults}"
                           ShapeIDPath="State"
                           ShapeIDTableField="STATE_NAME">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeColorValuePath="Winner"
                                 ShapeStroke="White"
                                 ShapeStrokeThickness="0.5">
            <syncfusion:ShapeSetting.FillSetting>
                <syncfusion:ShapeFillSetting AutoFillColors="False">
                    <syncfusion:ShapeFillSetting.ColorMappings>
                        <syncfusion:EqualsColorMapping Value="Democrat" 
                                                       Color="#2196F3" 
                                                       LegendLabel="Democrat" />
                        <syncfusion:EqualsColorMapping Value="Republican" 
                                                       Color="#F44336" 
                                                       LegendLabel="Republican" />
                        <syncfusion:EqualsColorMapping Value="Independent" 
                                                       Color="#FFC107" 
                                                       LegendLabel="Independent" />
                    </syncfusion:ShapeFillSetting.ColorMappings>
                </syncfusion:ShapeFillSetting>
            </syncfusion:ShapeSetting.FillSetting>
        </syncfusion:ShapeSetting>
    </syncfusion:ShapeFileLayer.ShapeSettings>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";
layer.ItemsSource = viewModel.ElectionResults;
layer.ShapeIDPath = "State";
layer.ShapeIDTableField = "STATE_NAME";

ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeColorValuePath = "Winner";
shapeSetting.ShapeStroke = new SolidColorBrush(Colors.White);
shapeSetting.ShapeStrokeThickness = 0.5;

ShapeFillSetting fillSetting = new ShapeFillSetting();
fillSetting.AutoFillColors = false;

// Add equals color mappings
fillSetting.ColorMappings.Add(new EqualsColorMapping
{
    Value = "Democrat",
    Color = (Color)ColorConverter.ConvertFromString("#2196F3"),
    LegendLabel = "Democrat"
});

fillSetting.ColorMappings.Add(new EqualsColorMapping
{
    Value = "Republican",
    Color = (Color)ColorConverter.ConvertFromString("#F44336"),
    LegendLabel = "Republican"
});

fillSetting.ColorMappings.Add(new EqualsColorMapping
{
    Value = "Independent",
    Color = (Color)ColorConverter.ConvertFromString("#FFC107"),
    LegendLabel = "Independent"
});

shapeSetting.FillSetting = fillSetting;
layer.ShapeSettings = shapeSetting;
```

### Data Model

```csharp
public class ElectionResult
{
    public string State { get; set; }
    public string Winner { get; set; }  // "Democrat", "Republican", "Independent"
    public int ElectoralVotes { get; set; }
}
```

## Complete Examples

### Example 1: Population Density Choropleth Map

```xml
<Window.DataContext>
    <local:PopulationViewModel />
</Window.DataContext>

<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                                   ItemsSource="{Binding Countries}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="NAME"
                                   LegendVisibility="Visible"
                                   LegendPosition="BottomLeft"
                                   LegendHeader="Population Density">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeValuePath="PopulationDensity"
                                         ShapeStroke="#757575"
                                         ShapeStrokeThickness="0.5">
                    <syncfusion:ShapeSetting.FillSetting>
                        <syncfusion:ShapeFillSetting AutoFillColors="False">
                            <syncfusion:ShapeFillSetting.ColorMappings>
                                <syncfusion:RangeColorMapping From="0" To="50" Color="#FFFFB2" LegendLabel="Low (0-50)" />
                                <syncfusion:RangeColorMapping From="50" To="100" Color="#FECC5C" LegendLabel="Medium (50-100)" />
                                <syncfusion:RangeColorMapping From="100" To="200" Color="#FD8D3C" LegendLabel="High (100-200)" />
                                <syncfusion:RangeColorMapping From="200" To="500" Color="#F03B20" LegendLabel="Very High (200-500)" />
                                <syncfusion:RangeColorMapping From="500" To="2000" Color="#BD0026" LegendLabel="Extreme (500+)" />
                            </syncfusion:ShapeFillSetting.ColorMappings>
                        </syncfusion:ShapeFillSetting>
                    </syncfusion:ShapeSetting.FillSetting>
                </syncfusion:ShapeSetting>
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Example 2: Climate Zones Map

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.regions.shp"
                                   ItemsSource="{Binding Regions}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="REGION_NAME"
                                   LegendVisibility="Visible"
                                   LegendHeader="Climate Zones">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeColorValuePath="ClimateZone"
                                         ShapeStroke="Black"
                                         ShapeStrokeThickness="1">
                    <syncfusion:ShapeSetting.FillSetting>
                        <syncfusion:ShapeFillSetting AutoFillColors="False">
                            <syncfusion:ShapeFillSetting.ColorMappings>
                                <syncfusion:EqualsColorMapping Value="Tropical" Color="#4CAF50" LegendLabel="Tropical" />
                                <syncfusion:EqualsColorMapping Value="Arid" Color="#FF9800" LegendLabel="Arid" />
                                <syncfusion:EqualsColorMapping Value="Temperate" Color="#2196F3" LegendLabel="Temperate" />
                                <syncfusion:EqualsColorMapping Value="Continental" Color="#9C27B0" LegendLabel="Continental" />
                                <syncfusion:EqualsColorMapping Value="Polar" Color="#FFFFFF" LegendLabel="Polar" />
                            </syncfusion:ShapeFillSetting.ColorMappings>
                        </syncfusion:ShapeFillSetting>
                    </syncfusion:ShapeSetting.FillSetting>
                </syncfusion:ShapeSetting>
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Example 3: Combined Range and Border Customization

```csharp
public class StateViewModel
{
    public ObservableCollection<StateData> States { get; set; }

    public StateViewModel()
    {
        States = new ObservableCollection<StateData>
        {
            new StateData { Name = "California", Income = 75235 },
            new StateData { Name = "Texas", Income = 59206 },
            new StateData { Name = "Florida", Income = 52594 },
            new StateData { Name = "New York", Income = 68486 }
        };
    }
}

public class StateData
{
    public string Name { get; set; }
    public int Income { get; set; }
}
```

```xml
<syncfusion:ShapeSetting ShapeValuePath="Income"
                         ShapeStroke="#212121"
                         ShapeStrokeThickness="1.5">
    <syncfusion:ShapeSetting.FillSetting>
        <syncfusion:ShapeFillSetting AutoFillColors="False">
            <syncfusion:ShapeFillSetting.ColorMappings>
                <syncfusion:RangeColorMapping From="0" To="50000" Color="#C8E6C9" LegendLabel="< $50K" />
                <syncfusion:RangeColorMapping From="50000" To="60000" Color="#81C784" LegendLabel="$50K - $60K" />
                <syncfusion:RangeColorMapping From="60000" To="70000" Color="#4CAF50" LegendLabel="$60K - $70K" />
                <syncfusion:RangeColorMapping From="70000" To="100000" Color="#2E7D32" LegendLabel="> $70K" />
            </syncfusion:ShapeFillSetting.ColorMappings>
        </syncfusion:ShapeFillSetting>
    </syncfusion:ShapeSetting.FillSetting>
</syncfusion:ShapeSetting>
```

## Best Practices

1. **Color selection** - Use colorblind-friendly palettes
2. **Range definition** - Cover entire data range without gaps
3. **Visual hierarchy** - Light colors for low values, dark for high values
4. **Legend labels** - Use clear, descriptive text
5. **Stroke contrast** - Use white or dark strokes to separate shapes
6. **Data normalization** - Consider per capita or percentage values for fair comparison
7. **Color count** - Use 3-7 color ranges for optimal readability

## Troubleshooting

### Colors not applying

**Check:**
- AutoFillColors is set to false
- ShapeValuePath or ShapeColorValuePath matches property name
- ColorMappings are defined
- Data values fall within defined ranges

### Some shapes not colored

**Verify:**
- All data values are covered by ColorMappings
- Value matching is exact for EqualsColorMapping (case-sensitive)
- No null values in data properties
- ShapeIDPath mapping is correct

### Wrong colors appearing

**Ensure:**
- Range values are correct (From < To)
- No overlapping ranges in RangeColorMapping
- EqualsColorMapping Value matches exactly
- Color format is valid (#RRGGBB or named color)
