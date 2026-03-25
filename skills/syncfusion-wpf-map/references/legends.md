# Legends in WPF Maps (SfMap)

## Table of Contents
- [Overview](#overview)
- [Legend Visibility](#legend-visibility)
- [Legend Position](#legend-position)
- [Legend Header](#legend-header)
- [Legend Type](#legend-type)
- [Legend Icons](#legend-icons)
- [Legend Layout](#legend-layout)
- [Complete Examples](#complete-examples)

## Overview

Legends provide a key to interpret the colors, symbols, and data ranges displayed on the map. They automatically generate based on color mappings and can be extensively customized for position, appearance, and layout.

**Key features:**
- Automatic generation from ColorMappings
- Flexible positioning (predefined positions + custom coordinates)
- Multiple icon shapes
- Multi-column layout support
- Custom headers
- Separate legends for shapes vs bubbles

## Legend Visibility

Control legend display using the `LegendVisibility` property on ShapeFileLayer.

**Type:** Visibility (Visible, Collapsed, Hidden)  
**Default:** Collapsed

```xml
<syncfusion:ShapeFileLayer LegendVisibility="Visible" />
```

```csharp
layer.LegendVisibility = Visibility.Visible;
```

## Legend Position

### Standard Positions

Use `LegendPosition` for predefined placements.

**Type:** LegendPosition enum  
**Default:** Default

**Available positions:**
- **TopLeft** - Upper left corner
- **TopCenter** - Top center
- **TopRight** - Upper right corner
- **MidLeft** - Middle left
- **Center** - Dead center
- **MidRight** - Middle right
- **BottomLeft** - Lower left corner
- **BottomCenter** - Bottom center
- **BottomRight** - Lower right corner
- **Default** - Use LegendPositionX/Y for custom placement

```xml
<syncfusion:ShapeFileLayer LegendVisibility="Visible"
                           LegendPosition="BottomLeft" />
```

```csharp
layer.LegendVisibility = Visibility.Visible;
layer.LegendPosition = LegendPosition.BottomLeft;
```

### Custom Positioning

For precise control, set `LegendPosition="Default"` and use LegendPositionX/Y.

**LegendPositionX** (double) - X-axis margin in pixels  
**LegendPositionY** (double) - Y-axis margin in pixels

```xml
<syncfusion:ShapeFileLayer LegendVisibility="Visible"
                           LegendPosition="Default"
                           LegendPositionX="50"
                           LegendPositionY="100" />
```

```csharp
layer.LegendVisibility = Visibility.Visible;
layer.LegendPosition = LegendPosition.Default;
layer.LegendPositionX = 50;
layer.LegendPositionY = 100;
```

**Position Calculation:**
- LegendPositionX = Distance from left edge
- LegendPositionY = Distance from top edge

## Legend Header

Add a title to the legend using `LegendHeader`.

**Type:** string  
**Default:** null (no header)

```xml
<syncfusion:ShapeFileLayer LegendVisibility="Visible"
                           LegendHeader="Population Density" />
```

```csharp
layer.LegendVisibility = Visibility.Visible;
layer.LegendHeader = "Population Density";
```

**Result:** Title appears above legend items.

## Legend Type

Specify whether the legend represents shape colors or bubble colors.

**Type:** LegendType enum  
**Values:**
- **Layers** - Legend for shape colors (default)
- **Bubbles** - Legend for bubble colors

```xml
<!-- Legend for shape color mapping -->
<syncfusion:ShapeFileLayer LegendType="Layers"
                           LegendVisibility="Visible" />

<!-- Legend for bubble color mapping -->
<syncfusion:ShapeFileLayer LegendType="Bubbles"
                           LegendVisibility="Visible" />
```

```csharp
// Shape legend
layer.LegendType = LegendType.Layers;

// Bubble legend
layer.LegendType = LegendType.Bubbles;
```

### Shape Legend

Used with ShapeSetting color mappings:

```xml
<syncfusion:ShapeFileLayer LegendType="Layers"
                           LegendVisibility="Visible">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeValuePath="Population">
            <syncfusion:ShapeSetting.FillSetting>
                <syncfusion:ShapeFillSetting AutoFillColors="False">
                    <syncfusion:ShapeFillSetting.ColorMappings>
                        <syncfusion:RangeColorMapping From="0" To="1000000" Color="#FEE5D9" LegendLabel="< 1M" />
                        <syncfusion:RangeColorMapping From="1000000" To="10000000" Color="#FB6A4A" LegendLabel="1M - 10M" />
                    </syncfusion:ShapeFillSetting.ColorMappings>
                </syncfusion:ShapeFillSetting>
            </syncfusion:ShapeSetting.FillSetting>
        </syncfusion:ShapeSetting>
    </syncfusion:ShapeFileLayer.ShapeSettings>
</syncfusion:ShapeFileLayer>
```

### Bubble Legend

Used with BubbleMarkerSetting color mappings:

```xml
<syncfusion:ShapeFileLayer LegendType="Bubbles"
                           LegendVisibility="Visible">
    <syncfusion:ShapeFileLayer.BubbleMarkerSetting>
        <syncfusion:BubbleMarkerSetting ValuePath="Population" ColorValuePath="GDP">
            <syncfusion:BubbleMarkerSetting.ColorMappings>
                <syncfusion:RangeColorMapping From="0" To="1000000000" Color="#81C784" LegendLabel="Low GDP" />
                <syncfusion:RangeColorMapping From="1000000000" To="10000000000" Color="#E57373" LegendLabel="High GDP" />
            </syncfusion:BubbleMarkerSetting.ColorMappings>
        </syncfusion:BubbleMarkerSetting>
    </syncfusion:ShapeFileLayer.BubbleMarkerSetting>
</syncfusion:ShapeFileLayer>
```

## Legend Icons

Customize the icon shape for legend items using `LegendIcon`.

**Type:** LegendIcons enum  
**Values:**
- **Rectangle** - Square/rectangular icon
- **Circle** - Circular icon
- **Diamond** - Diamond-shaped icon
- **Triangle** - Triangular icon

**Default:** Rectangle

```xml
<syncfusion:ShapeFileLayer LegendIcon="Circle" />
```

```csharp
layer.LegendIcon = LegendIcons.Circle;
```

### Icon Examples

```xml
<!-- Rectangle icons (default) -->
<syncfusion:ShapeFileLayer LegendIcon="Rectangle" LegendVisibility="Visible" />

<!-- Circular icons -->
<syncfusion:ShapeFileLayer LegendIcon="Circle" LegendVisibility="Visible" />

<!-- Diamond icons -->
<syncfusion:ShapeFileLayer LegendIcon="Diamond" LegendVisibility="Visible" />

<!-- Triangle icons -->
<syncfusion:ShapeFileLayer LegendIcon="Triangle" LegendVisibility="Visible" />
```

## Legend Layout

### Multi-Column Layout

Control the number of columns using `LegendColumnSplit`.

**Type:** int  
**Default:** 1 (single column)

```xml
<syncfusion:ShapeFileLayer LegendColumnSplit="2" />
```

```csharp
layer.LegendColumnSplit = 2;
```

**Example:** 6 legend items with LegendColumnSplit="3"
```
[Icon] Label 1    [Icon] Label 2    [Icon] Label 3
[Icon] Label 4    [Icon] Label 5    [Icon] Label 6
```

### Layout Calculation

Items are arranged in matrix format:
- `LegendColumnSplit` defines column count
- Rows are calculated automatically based on item count

```xml
<!-- 4 columns -->
<syncfusion:ShapeFileLayer LegendColumnSplit="4" />
```

## Complete Examples

### Example 1: Population Map with Legend

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                                   ItemsSource="{Binding Countries}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="NAME"
                                   LegendVisibility="Visible"
                                   LegendPosition="BottomLeft"
                                   LegendHeader="Population"
                                   LegendIcon="Rectangle"
                                   LegendColumnSplit="2">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeValuePath="Population"
                                         ShapeStroke="White"
                                         ShapeStrokeThickness="0.5">
                    <syncfusion:ShapeSetting.FillSetting>
                        <syncfusion:ShapeFillSetting AutoFillColors="False">
                            <syncfusion:ShapeFillSetting.ColorMappings>
                                <syncfusion:RangeColorMapping From="0" To="10000000" 
                                                              Color="#FEE5D9" 
                                                              LegendLabel="< 10M" />
                                <syncfusion:RangeColorMapping From="10000000" To="100000000" 
                                                              Color="#FCAE91" 
                                                              LegendLabel="10M - 100M" />
                                <syncfusion:RangeColorMapping From="100000000" To="500000000" 
                                                              Color="#FB6A4A" 
                                                              LegendLabel="100M - 500M" />
                                <syncfusion:RangeColorMapping From="500000000" To="2000000000" 
                                                              Color="#CB181D" 
                                                              LegendLabel="> 500M" />
                            </syncfusion:ShapeFillSetting.ColorMappings>
                        </syncfusion:ShapeFillSetting>
                    </syncfusion:ShapeSetting.FillSetting>
                </syncfusion:ShapeSetting>
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Example 2: Election Map with Custom Position

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           ItemsSource="{Binding ElectionResults}"
                           ShapeIDPath="State"
                           ShapeIDTableField="STATE_NAME"
                           LegendVisibility="Visible"
                           LegendPosition="Default"
                           LegendPositionX="20"
                           LegendPositionY="500"
                           LegendHeader="Election Results"
                           LegendIcon="Circle">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeColorValuePath="Candidate">
            <syncfusion:ShapeSetting.FillSetting>
                <syncfusion:ShapeFillSetting AutoFillColors="False">
                    <syncfusion:ShapeFillSetting.ColorMappings>
                        <syncfusion:EqualsColorMapping Value="Democrat" 
                                                       Color="#2196F3" 
                                                       LegendLabel="Democrat" />
                        <syncfusion:EqualsColorMapping Value="Republican" 
                                                       Color="#F44336" 
                                                       LegendLabel="Republican" />
                    </syncfusion:ShapeFillSetting.ColorMappings>
                </syncfusion:ShapeFillSetting>
            </syncfusion:ShapeSetting.FillSetting>
        </syncfusion:ShapeSetting>
    </syncfusion:ShapeFileLayer.ShapeSettings>
</syncfusion:ShapeFileLayer>
```

### Example 3: Bubble Legend

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                           ItemsSource="{Binding Countries}"
                           ShapeIDPath="Name"
                           ShapeIDTableField="NAME"
                           LegendType="Bubbles"
                           LegendVisibility="Visible"
                           LegendPosition="TopRight"
                           LegendHeader="GDP Classification"
                           LegendIcon="Circle">
    <syncfusion:ShapeFileLayer.BubbleMarkerSetting>
        <syncfusion:BubbleMarkerSetting ValuePath="Population"
                                        ColorValuePath="GDP"
                                        MinSize="10"
                                        MaxSize="50">
            <syncfusion:BubbleMarkerSetting.ColorMappings>
                <syncfusion:RangeColorMapping From="0" To="1000000000" 
                                              Color="#4CAF50" 
                                              LegendLabel="Low GDP" />
                <syncfusion:RangeColorMapping From="1000000000" To="10000000000" 
                                              Color="#FFC107" 
                                              LegendLabel="Medium GDP" />
                <syncfusion:RangeColorMapping From="10000000000" To="50000000000" 
                                              Color="#F44336" 
                                              LegendLabel="High GDP" />
            </syncfusion:BubbleMarkerSetting.ColorMappings>
        </syncfusion:BubbleMarkerSetting>
    </syncfusion:ShapeFileLayer.BubbleMarkerSetting>
</syncfusion:ShapeFileLayer>
```

### Example 4: Programmatic Legend Configuration

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";

// Enable and position legend
layer.LegendVisibility = Visibility.Visible;
layer.LegendPosition = LegendPosition.BottomRight;
layer.LegendHeader = "State Population";
layer.LegendIcon = LegendIcons.Diamond;
layer.LegendColumnSplit = 3;

// Data binding
layer.ItemsSource = viewModel.States;
layer.ShapeIDPath = "Name";
layer.ShapeIDTableField = "STATE_NAME";

// Shape settings with color mappings
ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeValuePath = "Population";

ShapeFillSetting fillSetting = new ShapeFillSetting();
fillSetting.AutoFillColors = false;

fillSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 0,
    To = 5000000,
    Color = Colors.LightGreen,
    LegendLabel = "Low"
});

fillSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 5000000,
    To = 15000000,
    Color = Colors.Orange,
    LegendLabel = "Medium"
});

fillSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 15000000,
    To = 40000000,
    Color = Colors.Red,
    LegendLabel = "High"
});

shapeSetting.FillSetting = fillSetting;
layer.ShapeSettings = shapeSetting;
```

## Best Practices

1. **Visibility** - Always set LegendVisibility="Visible" when using legends
2. **LegendLabel** - Use concise, descriptive labels in ColorMappings
3. **Header** - Add LegendHeader to explain what the legend represents
4. **Position** - Place legends where they don't obscure important map areas
5. **Columns** - Use 2-3 columns for legends with many items
6. **Icon choice** - Match icon shape to map visual style
7. **Legend type** - Set LegendType correctly (Layers vs Bubbles)

## Troubleshooting

### Legend not appearing

**Check:**
- LegendVisibility is set to Visible
- ColorMappings are defined in ShapeSetting or BubbleMarkerSetting
- LegendLabel is set for each ColorMapping

### Legend items missing

**Verify:**
- All ColorMappings have non-empty LegendLabel
- AutoFillColors is false (required for custom ColorMappings)
- LegendType matches your color mapping location (Layers vs Bubbles)

### Legend in wrong position

**Adjust:**
- Use LegendPosition enum for standard positions
- Use LegendPosition="Default" + LegendPositionX/Y for custom placement
- Account for map size when using custom coordinates

### Legend labels not showing

**Ensure:**
- LegendLabel property is set for each ColorMapping
- Labels are not empty strings
- Legend is not clipped by map boundaries
