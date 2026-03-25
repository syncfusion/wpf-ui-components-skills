# Bubbles in WPF Maps (SfMap)

## Table of Contents
- [Overview](#overview)
- [BubbleMarkerSetting Configuration](#bubblemarkersetting-configuration)
- [Size Properties](#size-properties)
- [Color Properties](#color-properties)
- [Color Mappings for Bubbles](#color-mappings-for-bubbles)
- [Bubble Legends](#bubble-legends)
- [Complete Examples](#complete-examples)

## Overview

Bubbles visualize data values as circles overlaid on map shapes. The size and color of bubbles represent bound data values, making them ideal for displaying metrics like population, sales volume, or any quantitative data across geographical regions.

**Key features:**
- Data-driven sizing with MinSize/MaxSize constraints
- Automatic or custom color assignment
- Range and equals color mapping support
- Stroke and fill customization
- Opacity control
- Legend integration

## BubbleMarkerSetting Configuration

Enable bubbles by setting the `BubbleMarkerSetting` property on ShapeFileLayer.

### Basic Setup

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                           ItemsSource="{Binding Countries}"
                           ShapeIDPath="Name"
                           ShapeIDTableField="NAME">
    <syncfusion:ShapeFileLayer.BubbleMarkerSetting>
        <syncfusion:BubbleMarkerSetting ValuePath="Population"
                                        MinSize="10"
                                        MaxSize="50"
                                        Fill="Blue" />
    </syncfusion:ShapeFileLayer.BubbleMarkerSetting>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.world1.shp";
layer.ItemsSource = viewModel.Countries;
layer.ShapeIDPath = "Name";
layer.ShapeIDTableField = "NAME";

BubbleMarkerSetting bubbleSetting = new BubbleMarkerSetting();
bubbleSetting.ValuePath = "Population";
bubbleSetting.MinSize = 10;
bubbleSetting.MaxSize = 50;
bubbleSetting.Fill = new SolidColorBrush(Colors.Blue);
layer.BubbleMarkerSetting = bubbleSetting;
```

### Data Model

```csharp
public class CountryData
{
    public string Name { get; set; }
    public int Population { get; set; }
    public double GDP { get; set; }
    public string EconomyType { get; set; }
}
```

## Size Properties

### ValuePath

Specifies the property in your data source that determines bubble size.

**Type:** string  
**Required:** Yes

```xml
<syncfusion:BubbleMarkerSetting ValuePath="Population" />
```

```csharp
bubbleSetting.ValuePath = "Population";
```

The control calculates bubble sizes proportionally based on this value between MinSize and MaxSize.

### MinSize

Sets the minimum diameter for bubbles (representing smallest data value).

**Type:** double  
**Default:** 10  
**Unit:** Pixels

```xml
<syncfusion:BubbleMarkerSetting MinSize="15" />
```

```csharp
bubbleSetting.MinSize = 15;
```

### MaxSize

Sets the maximum diameter for bubbles (representing largest data value).

**Type:** double  
**Default:** 30  
**Unit:** Pixels

```xml
<syncfusion:BubbleMarkerSetting MaxSize="80" />
```

```csharp
bubbleSetting.MaxSize = 80;
```

### SizeRatio

Automatically calculated ratio between min and max sizes based on data range.

**Type:** double  
**Read-only:** Yes

This property is used internally for size calculations. The control computes bubble diameter using:

```
Diameter = MinSize + (NormalizedValue * (MaxSize - MinSize))
```

### Size Configuration Example

```xml
<syncfusion:BubbleMarkerSetting ValuePath="Population"
                                MinSize="20"
                                MaxSize="100" />
```

Result:
- Country with smallest population → 20px diameter bubble
- Country with largest population → 100px diameter bubble
- Other countries → Proportional sizes between 20-100px

## Color Properties

### AutoFillColor

Enables automatic color assignment using built-in color palette.

**Type:** bool  
**Default:** false

```xml
<syncfusion:BubbleMarkerSetting AutoFillColor="True" />
```

```csharp
bubbleSetting.AutoFillColor = true;
```

When true, bubbles receive colors from a default palette automatically.

### Fill

Sets a single fill color for all bubbles when AutoFillColor is false.

**Type:** Brush  
**Default:** System default

```xml
<syncfusion:BubbleMarkerSetting AutoFillColor="False"
                                Fill="Orange" />
```

```csharp
bubbleSetting.AutoFillColor = false;
bubbleSetting.Fill = new SolidColorBrush(Colors.Orange);
```

### Stroke

Sets the border color for bubbles.

**Type:** Brush  
**Default:** Transparent

```xml
<syncfusion:BubbleMarkerSetting Stroke="White" />
```

```csharp
bubbleSetting.Stroke = new SolidColorBrush(Colors.White);
```

### StrokeThickness

Sets the border width for bubbles.

**Type:** double  
**Default:** 0

```xml
<syncfusion:BubbleMarkerSetting Stroke="White" StrokeThickness="2" />
```

```csharp
bubbleSetting.Stroke = new SolidColorBrush(Colors.White);
bubbleSetting.StrokeThickness = 2;
```

### Opacity

Sets transparency level for bubbles.

**Type:** double  
**Range:** 0.0 (fully transparent) to 1.0 (fully opaque)  
**Default:** 1.0

```xml
<syncfusion:BubbleMarkerSetting Fill="Red" Opacity="0.6" />
```

```csharp
bubbleSetting.Fill = new SolidColorBrush(Colors.Red);
bubbleSetting.Opacity = 0.6;
```

## Color Mappings for Bubbles

Use `ColorValuePath` with `ColorMappings` to assign colors based on data values.

### ColorValuePath

Specifies the property for color determination.

**Type:** string

```xml
<syncfusion:BubbleMarkerSetting ColorValuePath="GDP" />
```

```csharp
bubbleSetting.ColorValuePath = "GDP";
```

### Range Color Mapping

Assigns colors based on numeric value ranges.

```xml
<syncfusion:BubbleMarkerSetting ValuePath="Population"
                                ColorValuePath="GDP"
                                MinSize="20"
                                MaxSize="80">
    <syncfusion:BubbleMarkerSetting.ColorMappings>
        <syncfusion:RangeColorMapping From="0" To="1000000000" Color="#FEE5D9" LegendLabel="Low GDP" />
        <syncfusion:RangeColorMapping From="1000000000" To="5000000000" Color="#FCAE91" LegendLabel="Medium GDP" />
        <syncfusion:RangeColorMapping From="5000000000" To="20000000000" Color="#FB6A4A" LegendLabel="High GDP" />
        <syncfusion:RangeColorMapping From="20000000000" To="50000000000" Color="#CB181D" LegendLabel="Very High GDP" />
    </syncfusion:BubbleMarkerSetting.ColorMappings>
</syncfusion:BubbleMarkerSetting>
```

```csharp
BubbleMarkerSetting bubbleSetting = new BubbleMarkerSetting();
bubbleSetting.ValuePath = "Population";
bubbleSetting.ColorValuePath = "GDP";
bubbleSetting.MinSize = 20;
bubbleSetting.MaxSize = 80;

// Add range color mappings
bubbleSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 0,
    To = 1000000000,
    Color = (Color)ColorConverter.ConvertFromString("#FEE5D9"),
    LegendLabel = "Low GDP"
});

bubbleSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 1000000000,
    To = 5000000000,
    Color = (Color)ColorConverter.ConvertFromString("#FCAE91"),
    LegendLabel = "Medium GDP"
});

bubbleSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 5000000000,
    To = 20000000000,
    Color = (Color)ColorConverter.ConvertFromString("#FB6A4A"),
    LegendLabel = "High GDP"
});

bubbleSetting.ColorMappings.Add(new RangeColorMapping
{
    From = 20000000000,
    To = 50000000000,
    Color = (Color)ColorConverter.ConvertFromString("#CB181D"),
    LegendLabel = "Very High GDP"
});
```

### Equals Color Mapping

Assigns colors based on categorical values.

```xml
<syncfusion:BubbleMarkerSetting ValuePath="Population"
                                ColorValuePath="EconomyType"
                                MinSize="15"
                                MaxSize="60">
    <syncfusion:BubbleMarkerSetting.ColorMappings>
        <syncfusion:EqualsColorMapping Value="Developed" Color="#4CAF50" LegendLabel="Developed" />
        <syncfusion:EqualsColorMapping Value="Developing" Color="#FFC107" LegendLabel="Developing" />
        <syncfusion:EqualsColorMapping Value="Underdeveloped" Color="#F44336" LegendLabel="Underdeveloped" />
    </syncfusion:BubbleMarkerSetting.ColorMappings>
</syncfusion:BubbleMarkerSetting>
```

```csharp
BubbleMarkerSetting bubbleSetting = new BubbleMarkerSetting();
bubbleSetting.ValuePath = "Population";
bubbleSetting.ColorValuePath = "EconomyType";

bubbleSetting.ColorMappings.Add(new EqualsColorMapping
{
    Value = "Developed",
    Color = (Color)ColorConverter.ConvertFromString("#4CAF50"),
    LegendLabel = "Developed"
});

bubbleSetting.ColorMappings.Add(new EqualsColorMapping
{
    Value = "Developing",
    Color = (Color)ColorConverter.ConvertFromString("#FFC107"),
    LegendLabel = "Developing"
});

bubbleSetting.ColorMappings.Add(new EqualsColorMapping
{
    Value = "Underdeveloped",
    Color = (Color)ColorConverter.ConvertFromString("#F44336"),
    LegendLabel = "Underdeveloped"
});
```

## Bubble Legends

Display bubble legends by setting layer's `LegendType` to `Bubbles`.

```xml
<syncfusion:ShapeFileLayer LegendType="Bubbles"
                           LegendVisibility="Visible"
                           LegendPosition="BottomLeft"
                           LegendHeader="Population by Economy Type">
    <syncfusion:ShapeFileLayer.BubbleMarkerSetting>
        <syncfusion:BubbleMarkerSetting ValuePath="Population"
                                        ColorValuePath="EconomyType">
            <syncfusion:BubbleMarkerSetting.ColorMappings>
                <syncfusion:EqualsColorMapping Value="Developed" Color="#4CAF50" LegendLabel="Developed" />
                <syncfusion:EqualsColorMapping Value="Developing" Color="#FFC107" LegendLabel="Developing" />
            </syncfusion:BubbleMarkerSetting.ColorMappings>
        </syncfusion:BubbleMarkerSetting>
    </syncfusion:ShapeFileLayer.BubbleMarkerSetting>
</syncfusion:ShapeFileLayer>
```

Legend shows bubble circles with colors and labels from ColorMappings.

## Complete Examples

### Example 1: Simple Population Bubbles

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                                   ItemsSource="{Binding Countries}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="NAME">
            
            <!-- Shape styling -->
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="#E0E0E0"
                                         ShapeStroke="White"
                                         ShapeStrokeThickness="1" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
            
            <!-- Bubble configuration -->
            <syncfusion:ShapeFileLayer.BubbleMarkerSetting>
                <syncfusion:BubbleMarkerSetting ValuePath="Population"
                                                MinSize="10"
                                                MaxSize="70"
                                                Fill="#2196F3"
                                                Stroke="White"
                                                StrokeThickness="1"
                                                Opacity="0.7" />
            </syncfusion:ShapeFileLayer.BubbleMarkerSetting>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Example 2: GDP-Based Color Bubbles with Legend

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                                   ItemsSource="{Binding Countries}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="NAME"
                                   LegendType="Bubbles"
                                   LegendVisibility="Visible"
                                   LegendPosition="TopRight"
                                   LegendHeader="GDP Classification">
            
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="LightGray"
                                         ShapeStroke="White"
                                         ShapeStrokeThickness="0.5" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
            
            <syncfusion:ShapeFileLayer.BubbleMarkerSetting>
                <syncfusion:BubbleMarkerSetting ValuePath="Population"
                                                ColorValuePath="GDP"
                                                MinSize="15"
                                                MaxSize="60"
                                                Stroke="White"
                                                StrokeThickness="2"
                                                Opacity="0.8">
                    <syncfusion:BubbleMarkerSetting.ColorMappings>
                        <syncfusion:RangeColorMapping From="0" To="1000000000" Color="#81C784" LegendLabel="< $1B" />
                        <syncfusion:RangeColorMapping From="1000000000" To="10000000000" Color="#FFB74D" LegendLabel="$1B - $10B" />
                        <syncfusion:RangeColorMapping From="10000000000" To="50000000000" Color="#E57373" LegendLabel="> $10B" />
                    </syncfusion:BubbleMarkerSetting.ColorMappings>
                </syncfusion:BubbleMarkerSetting>
            </syncfusion:ShapeFileLayer.BubbleMarkerSetting>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Example 3: Category-Based Bubble Colors

```csharp
public class MapViewModel
{
    public ObservableCollection<CountryData> Countries { get; set; }

    public MapViewModel()
    {
        Countries = new ObservableCollection<CountryData>
        {
            new CountryData { Name = "United States", Population = 331000000, GDP = 21430000000, EconomyType = "Developed" },
            new CountryData { Name = "China", Population = 1400000000, GDP = 14720000000, EconomyType = "Developing" },
            new CountryData { Name = "India", Population = 1380000000, GDP = 2870000000, EconomyType = "Developing" },
            new CountryData { Name = "Ethiopia", Population = 115000000, GDP = 96000000, EconomyType = "Underdeveloped" }
        };
    }
}
```

```xml
<syncfusion:BubbleMarkerSetting ValuePath="Population"
                                ColorValuePath="EconomyType"
                                MinSize="20"
                                MaxSize="80"
                                Stroke="Black"
                                StrokeThickness="1.5"
                                Opacity="0.75">
    <syncfusion:BubbleMarkerSetting.ColorMappings>
        <syncfusion:EqualsColorMapping Value="Developed" Color="#00C853" LegendLabel="Developed Nations" />
        <syncfusion:EqualsColorMapping Value="Developing" Color="#FFD600" LegendLabel="Developing Nations" />
        <syncfusion:EqualsColorMapping Value="Underdeveloped" Color="#D50000" LegendLabel="Underdeveloped Nations" />
    </syncfusion:BubbleMarkerSetting.ColorMappings>
</syncfusion:BubbleMarkerSetting>
```

## Best Practices

1. **Size range** - Choose MinSize/MaxSize that work well with your map scale
2. **Opacity** - Use 0.6-0.8 for overlapping bubbles to show all data
3. **Stroke visibility** - Add white or contrasting stroke to separate overlapping bubbles
4. **Color contrast** - Ensure bubble colors contrast with map shapes
5. **Legend clarity** - Use descriptive LegendLabel values
6. **Data normalization** - Consider logarithmic scaling for data with large ranges
7. **Performance** - Limit bubble count to 100-200 for smooth interaction

## Troubleshooting

### Bubbles not appearing
- Verify BubbleMarkerSetting is set
- Check ValuePath matches property in data source
- Ensure data values are numeric and not null
- Confirm ItemsSource binding is working

### All bubbles same size
- Verify ValuePath points to varying numeric data
- Check MinSize and MaxSize are different values
- Ensure data range has variation

### Colors not applying
- Check ColorValuePath matches property name
- Verify ColorMappings cover all data value ranges
- Ensure AutoFillColor is false when using custom colors

### Bubbles too large/small
- Adjust MinSize and MaxSize values
- Consider data value distribution (use logarithmic scale if needed)
- Test with different zoom levels
