# Color Mapping in TreeMap

## Table of Contents
- [Overview](#overview)
- [LeafColorMapping vs Level ColorMapping](#leafcolormapping-vs-level-colormapping)
- [UniColorMapping](#unicolormapping)
- [RangeBrushColorMapping](#rangebrushcolormapping)
- [DesaturationColorMapping](#desaturationcolormapping)
- [PaletteColorMapping](#palettecolormapping)
- [GroupColorMapping](#groupcolormapping)
- [Choosing the Right Color Mapping](#choosing-the-right-color-mapping)

## Overview

Color mapping in TreeMap provides multiple strategies to color rectangles based on data values. The control supports five color mapping types:

1. **UniColorMapping** - Single solid color for all items
2. **RangeBrushColorMapping** - Colors based on value ranges
3. **DesaturationColorMapping** - Opacity variations of a single color
4. **PaletteColorMapping** - Cycling through a collection of colors
5. **GroupColorMapping** - Different color strategies per group

Color mapping requires the `ColorValuePath` property to be set, which specifies which data property drives the coloring logic.

## LeafColorMapping vs Level ColorMapping

### LeafColorMapping

Colors the leaf nodes (actual data rectangles) of the TreeMap.

**Property:** `SfTreeMap.LeafColorMapping`

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:UniColorMapping Color="Crimson"/>
    </syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

### Level ColorMapping

Colors the headers of specific TreeMap levels (group containers).

**Property:** `TreeMapFlatLevel.ColorMapping`

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:UniColorMapping Color="Orange"/>
    </syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:SfTreeMap.Levels>
        <!-- Color the Continent headers green -->
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10" HeaderHeight="20">
            <syncfusion:TreeMapFlatLevel.ColorMapping>
                <syncfusion:UniColorMapping Color="YellowGreen"/>
            </syncfusion:TreeMapFlatLevel.ColorMapping>
        </syncfusion:TreeMapFlatLevel>
        
        <!-- Color the Country headers red -->
        <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" HeaderHeight="15">
            <syncfusion:TreeMapFlatLevel.ColorMapping>
                <syncfusion:UniColorMapping Color="Crimson"/>
            </syncfusion:TreeMapFlatLevel.ColorMapping>
        </syncfusion:TreeMapFlatLevel>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## UniColorMapping

Applies a single solid color to all leaf nodes or level headers.

**Best for:** Simple visualizations where color coding isn't needed, or establishing a base color theme.

### Basic Usage

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population"
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:UniColorMapping Color="Crimson"/>
    </syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

### With Multiple Levels

```xaml
<Grid Background="Black">
    <Grid.DataContext>
        <local:PopulationViewModel/>
    </Grid.DataContext>
    
    <syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                          WeightValuePath="Population"
                          ColorValuePath="Growth">
        <syncfusion:SfTreeMap.LeafColorMapping>
            <syncfusion:UniColorMapping Color="Crimson"/>
        </syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:SfTreeMap.Levels>
            <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
            <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" ShowLabels="True"/>
        </syncfusion:SfTreeMap.Levels>
    </syncfusion:SfTreeMap>
</Grid>
```

**Key Property:**
- **Color** - The solid color to apply (Color, hex string, or named color)

## RangeBrushColorMapping

Colors leaf nodes based on their ColorValuePath value falling within defined ranges.

**Best for:** Heat maps, performance indicators, categorizing values into distinct bands.

### Basic Usage

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population" 
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:RangeBrushColorMapping>
            <syncfusion:RangeBrushColorMapping.Brushes>
                <syncfusion:RangeBrush From="0" To="1" Color="#A4C400"/>
                <syncfusion:RangeBrush From="1" To="2" Color="#AA00FF"/>
                <syncfusion:RangeBrush From="2" To="3" Color="#F0A30A"/>
                <syncfusion:RangeBrush From="3" To="4" Color="#1BA1E2"/>
            </syncfusion:RangeBrushColorMapping.Brushes>
        </syncfusion:RangeBrushColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

### Stock Performance Example

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding Stocks}" 
                      WeightValuePath="MarketCap" 
                      ColorValuePath="PercentChange">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:RangeBrushColorMapping>
            <syncfusion:RangeBrushColorMapping.Brushes>
                <!-- Negative performance: Red shades -->
                <syncfusion:RangeBrush From="-100" To="-5" Color="#8B0000"/>
                <syncfusion:RangeBrush From="-5" To="-2" Color="#FF4444"/>
                <syncfusion:RangeBrush From="-2" To="0" Color="#FF8888"/>
                
                <!-- Neutral: Gray -->
                <syncfusion:RangeBrush From="0" To="0.5" Color="#CCCCCC"/>
                
                <!-- Positive performance: Green shades -->
                <syncfusion:RangeBrush From="0.5" To="2" Color="#88FF88"/>
                <syncfusion:RangeBrush From="2" To="5" Color="#44FF44"/>
                <syncfusion:RangeBrush From="5" To="100" Color="#008B00"/>
            </syncfusion:RangeBrushColorMapping.Brushes>
        </syncfusion:RangeBrushColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
</syncfusion:SfTreeMap>
```

**Key Properties:**
- **From** - Lower bound of the range (inclusive)
- **To** - Upper bound of the range (exclusive)
- **Color** - Color to apply for values in this range

**Notes:**
- Ranges can overlap; first matching range is used
- Values outside all ranges use default color
- Supports negative ranges

## DesaturationColorMapping

Applies opacity variations of a single color based on value ranges. Higher values get more saturated (opaque) colors.

**Best for:** Emphasizing intensity, showing concentration, maintaining a consistent color theme with value-based emphasis.

### Basic Usage

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population" 
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:DesaturationColorMapping From="1" 
                                             To="0.5" 
                                             RangeMinimum="0" 
                                             RangeMaximum="4" 
                                             Color="DeepSkyBlue">
        </syncfusion:DesaturationColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

### Weather Visualization Example

```xaml
<!-- Humidity visualization: Higher opacity = Higher humidity -->
<syncfusion:SfTreeMap ItemsSource="{Binding WeatherData}"
                      WeightValuePath="Area"
                      ColorValuePath="Humidity">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:DesaturationColorMapping From="1" 
                                             To="0.2" 
                                             RangeMinimum="0" 
                                             RangeMaximum="100" 
                                             Color="Blue">
        </syncfusion:DesaturationColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
</syncfusion:SfTreeMap>
```

**Key Properties:**
- **Color** - Base color to desaturate
- **From** - Opacity for maximum value (1.0 = fully opaque)
- **To** - Opacity for minimum value (0.0 = fully transparent)
- **RangeMinimum** - Minimum data value
- **RangeMaximum** - Maximum data value

**How it works:**
- Values at RangeMaximum get opacity of From
- Values at RangeMinimum get opacity of To
- Values in between are interpolated linearly

## PaletteColorMapping

Cycles through a collection of colors, assigning each leaf node a color from the palette in sequence.

**Best for:** Distinguishing categories, creating visually distinct items without value-based meaning.

### Basic Usage

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population" 
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:PaletteColorMapping>
            <syncfusion:PaletteColorMapping.Colors>
                <SolidColorBrush Color="Red"/>
                <SolidColorBrush Color="Blue"/>
                <SolidColorBrush Color="Green"/>
                <SolidColorBrush Color="Yellow"/>
                <SolidColorBrush Color="Orange"/>
                <SolidColorBrush Color="Orchid"/>
                <SolidColorBrush Color="Brown"/>
                <SolidColorBrush Color="BlueViolet"/>
                <SolidColorBrush Color="OrangeRed"/>
                <SolidColorBrush Color="Magenta"/>
                <SolidColorBrush Color="Olive"/>
                <SolidColorBrush Color="Crimson"/>
                <SolidColorBrush Color="DeepSkyBlue"/>
            </syncfusion:PaletteColorMapping.Colors>
        </syncfusion:PaletteColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

### Category Visualization Example

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding Products}"
                      WeightValuePath="Sales"
                      ColorValuePath="Category">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:PaletteColorMapping>
            <syncfusion:PaletteColorMapping.Colors>
                <SolidColorBrush Color="#FF6B6B"/>  <!-- Electronics -->
                <SolidColorBrush Color="#4ECDC4"/>  <!-- Clothing -->
                <SolidColorBrush Color="#45B7D1"/>  <!-- Food -->
                <SolidColorBrush Color="#FFA07A"/>  <!-- Books -->
                <SolidColorBrush Color="#98D8C8"/>  <!-- Home -->
            </syncfusion:PaletteColorMapping.Colors>
        </syncfusion:PaletteColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
</syncfusion:SfTreeMap>
```

**Key Properties:**
- **Colors** - Collection of SolidColorBrush to cycle through

**How it works:**
- First item gets first color, second item gets second color, etc.
- When palette is exhausted, it cycles back to the first color
- Order is based on data order in ItemsSource

## GroupColorMapping

Applies different color mapping strategies to different groups, identified by GroupID.

**Best for:** Complex visualizations where different regions/categories need different color schemes.

### Basic Usage

```xaml
<syncfusion:SfTreeMap Name="TreeMap" 
                      ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Growth" 
                      ColorValuePath="Growth"
                      ItemsLayoutMode="Squarified">
    <syncfusion:SfTreeMap.GroupColorMappings>
        <syncfusion:GroupColorMapping GroupID="North America">
            <syncfusion:GroupColorMapping.TreeMapColorMapping>
                <syncfusion:DesaturationColorMapping From="1" 
                                                     To="0.1" 
                                                     Color="#F3D240" 
                                                     RangeMinimum="0" 
                                                     RangeMaximum="6"/>
            </syncfusion:GroupColorMapping.TreeMapColorMapping>
        </syncfusion:GroupColorMapping>
        
        <syncfusion:GroupColorMapping GroupID="Asia">
            <syncfusion:GroupColorMapping.TreeMapColorMapping>
                <syncfusion:DesaturationColorMapping From="1" 
                                                     To="0.1" 
                                                     Color="#77D8D8" 
                                                     RangeMinimum="0" 
                                                     RangeMaximum="7"/>
            </syncfusion:GroupColorMapping.TreeMapColorMapping>
        </syncfusion:GroupColorMapping>
        
        <syncfusion:GroupColorMapping GroupID="Africa">
            <syncfusion:GroupColorMapping.TreeMapColorMapping>
                <syncfusion:DesaturationColorMapping From="1" 
                                                     To="0.1" 
                                                     Color="#faafbe" 
                                                     RangeMinimum="0" 
                                                     RangeMaximum="6"/>
            </syncfusion:GroupColorMapping.TreeMapColorMapping>
        </syncfusion:GroupColorMapping>
        
        <syncfusion:GroupColorMapping GroupID="Europe">
            <syncfusion:GroupColorMapping.TreeMapColorMapping>
                <syncfusion:DesaturationColorMapping From="1" 
                                                     To="0.1" 
                                                     Color="#AED960" 
                                                     RangeMinimum="0" 
                                                     RangeMaximum="6"/>
            </syncfusion:GroupColorMapping.TreeMapColorMapping>
        </syncfusion:GroupColorMapping>
        
        <syncfusion:GroupColorMapping GroupID="South America">
            <syncfusion:GroupColorMapping.TreeMapColorMapping>
                <syncfusion:DesaturationColorMapping From="1" 
                                                     To="0" 
                                                     Color="#FFAF51" 
                                                     RangeMinimum="0" 
                                                     RangeMaximum="6"/>
            </syncfusion:GroupColorMapping.TreeMapColorMapping>
        </syncfusion:GroupColorMapping>
    </syncfusion:SfTreeMap.GroupColorMappings>
</syncfusion:SfTreeMap>
```

**Key Properties:**
- **GroupID** - Identifier matching a value in your GroupPath property
- **TreeMapColorMapping** - Any color mapping strategy (Uni, Range, Desaturation, Palette)

**Notes:**
- GroupID must match values in the property specified by GroupPath
- Each group can use a different color mapping type
- Useful for regional color coding with value-based variations

## Choosing the Right Color Mapping

| Use Case | Recommended Mapping | Reason |
|----------|-------------------|---------|
| All same color | UniColorMapping | Simple, no value interpretation needed |
| Heat map | RangeBrushColorMapping | Clear value ranges with distinct colors |
| Stock performance | RangeBrushColorMapping | Red for loss, green for gain |
| Intensity/concentration | DesaturationColorMapping | Same hue, opacity shows strength |
| Category distinction | PaletteColorMapping | Each category gets unique color |
| Regional themes | GroupColorMapping | Different regions, different schemes |
| Sales performance | RangeBrushColorMapping | Thresholds for poor/good/excellent |
| Weather data | DesaturationColorMapping | Maintain color theme, show intensity |

## Code-Behind Configuration

```csharp
// UniColorMapping
var uniColor = new UniColorMapping { Color = Colors.Crimson };
treeMap.LeafColorMapping = uniColor;

// RangeBrushColorMapping
var rangeMapping = new RangeBrushColorMapping();
rangeMapping.Brushes.Add(new RangeBrush { From = 0, To = 50, Color = Colors.Green });
rangeMapping.Brushes.Add(new RangeBrush { From = 50, To = 100, Color = Colors.Red });
treeMap.LeafColorMapping = rangeMapping;

// DesaturationColorMapping
var desaturationMapping = new DesaturationColorMapping
{
    From = 1,
    To = 0.2,
    RangeMinimum = 0,
    RangeMaximum = 100,
    Color = Colors.Blue
};
treeMap.LeafColorMapping = desaturationMapping;
```

## Troubleshooting

**Colors not appearing:**
- Ensure ColorValuePath is set and points to a valid property
- Verify color mapping is assigned to LeafColorMapping or Level ColorMapping
- Check that data values match your range definitions

**Wrong colors applied:**
- Verify ColorValuePath property name is correct (case-sensitive)
- Check range From/To values match your data range
- Ensure RangeMinimum/RangeMaximum are set correctly for Desaturation

**All items same color with RangeBrushColorMapping:**
- Verify ranges cover your data values
- Check that ColorValuePath values are numeric
- Ensure ranges don't all map to same color
