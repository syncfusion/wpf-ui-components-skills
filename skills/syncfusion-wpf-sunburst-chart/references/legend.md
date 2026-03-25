# Legend

## Table of Contents
- [Overview](#overview)
- [Basic Legend Configuration](#basic-legend-configuration)
- [Legend Icons](#legend-icons)
- [Legend Positioning](#legend-positioning)
- [Custom Legend Templates](#custom-legend-templates)
- [Legend Interactivity](#legend-interactivity)
- [Best Practices](#best-practices)

## Overview

The legend displays the first-level categories of the sunburst chart, helping users identify segments by color. It provides a visual key for understanding the chart's color coding.

**Key Features:**
- Displays first hierarchy level categories
- Customizable icons (Circle, Pentagon, Rectangle, etc.)
- Flexible positioning (Left, Right, Top, Bottom)
- Interactive click actions (selection, visibility toggle)
- Full template customization support
- Automatic color mapping from chart palette

## Basic Legend Configuration

### Enabling the Legend

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Legend>
        <sunburst:SunburstLegend/>
    </sunburst:SfSunburstChart.Legend>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstLegend legend = new SunburstLegend();
chart.Legend = legend;
```

By default, the legend displays with Circle icons and is positioned on the right side.

### Disabling the Legend

To remove the legend, set the Legend property to null:

**C#:**
```csharp
chart.Legend = null;
```

Or simply omit the Legend element in XAML.

## Legend Icons

Customize legend markers using the `LegendIcon` property.

### Available Icon Shapes

Predefined shapes:
- **Circle** (default)
- **Rectangle**
- **Diamond**
- **Pentagon**
- **Triangle**
- **Cross**
- **Custom** (for custom templates)

### Setting Icon Shape

**XAML:**
```xml
<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend LegendIcon="Pentagon"/>
</sunburst:SfSunburstChart.Legend>
```

**C#:**
```csharp
SunburstLegend legend = new SunburstLegend()
{
    LegendIcon = SunburstLegendIcon.Pentagon
};
chart.Legend = legend;
```

### Icon Sizing

Control icon dimensions with `IconHeight` and `IconWidth`:

**XAML:**
```xml
<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend LegendIcon="Diamond"
                            IconHeight="15"
                            IconWidth="15"/>
</sunburst:SfSunburstChart.Legend>
```

**C#:**
```csharp
legend.IconHeight = 15;
legend.IconWidth = 15;
```

**Default size:** 12x12 pixels

**Recommendations:**
- Small charts: 10-12px
- Medium charts: 12-15px
- Large charts: 15-20px

### Custom Legend Icons

Create custom icon templates using the `LegendIconTemplate` property.

**XAML:**
```xml
<Grid.Resources>
    <DataTemplate x:Key="customLegendIcon">
        <Path Stroke="{Binding Stroke}" 
              Stretch="Fill" 
              Fill="{Binding Interior}"  
              StrokeThickness="{Binding StrokeThickness}"
              Data="M 133,45 L 154,24 L 175,45 L 154,66 L 175,88 L 154,109 L 133,88 L 111,109 L 90,88 L 111,66 L 90,45 L 111,24 L 133,45 Z" />
    </DataTemplate>
</Grid.Resources>

<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend LegendIcon="Custom" 
                            LegendIconTemplate="{StaticResource customLegendIcon}"/>
</sunburst:SfSunburstChart.Legend>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
// ... configure chart ...

SunburstLegend legend = new SunburstLegend()
{
    LegendIcon = SunburstLegendIcon.Custom,
    LegendIconTemplate = (DataTemplate)grid.Resources["customLegendIcon"]
};
sunburst.Legend = legend;
```

**Important:** Set `LegendIcon` to `Custom` when using custom templates.

**Available bindings in template:**
- **Interior** – Segment fill color
- **Stroke** – Segment border color
- **StrokeThickness** – Border width

## Legend Positioning

### DockPosition Property

Control legend placement using `DockPosition`:

**Options:**
- **Left** – Legend on left side
- **Right** – Legend on right side (default)
- **Top** – Legend above chart
- **Bottom** – Legend below chart

### Positioning Examples

**Left Position:**
```xml
<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend DockPosition="Left"/>
</sunburst:SfSunburstChart.Legend>
```

**Top Position:**
```xml
<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend DockPosition="Top"/>
</sunburst:SfSunburstChart.Legend>
```

**C#:**
```csharp
SunburstLegend legend = new SunburstLegend()
{
    DockPosition = ChartDock.Top
};
chart.Legend = legend;
```

### Layout Recommendations

- **Left/Right:** Best for tall charts or many categories (vertical stacking)
- **Top/Bottom:** Best for wide charts or few categories (horizontal layout)

## Custom Legend Templates

For advanced layouts, customize the entire legend appearance using `ItemsPanel` and `ItemTemplate`.

### Custom Layout with WrapPanel

Arrange legend items in a wrapped grid:

```xml
<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend DockPosition="Top" Margin="0,25,0,0">
        
        <!-- Custom panel for layout -->
        <sunburst:SunburstLegend.ItemsPanel>
            <ItemsPanelTemplate>
                <WrapPanel Height="100" Width="250"/>
            </ItemsPanelTemplate>
        </sunburst:SunburstLegend.ItemsPanel>
        
        <!-- Custom item template -->
        <sunburst:SunburstLegend.ItemTemplate>
            <ItemContainerTemplate>
                <StackPanel Width="75" Height="30" Orientation="Horizontal">
                    <Ellipse Stroke="{Binding Interior}" 
                             StrokeThickness="2" 
                             Fill="White"
                             Height="10" 
                             Width="10" 
                             Margin="5"/>
                    <TextBlock Text="{Binding Label}" 
                               HorizontalAlignment="Center"   
                               VerticalAlignment="Center"/>
                </StackPanel>
            </ItemContainerTemplate>
        </sunburst:SunburstLegend.ItemTemplate>
        
    </sunburst:SunburstLegend>
</sunburst:SfSunburstChart.Legend>
```

### Horizontal Layout Example

```xml
<sunburst:SunburstLegend DockPosition="Bottom">
    <sunburst:SunburstLegend.ItemsPanel>
        <ItemsPanelTemplate>
            <StackPanel Orientation="Horizontal" 
                       HorizontalAlignment="Center"/>
        </ItemsPanelTemplate>
    </sunburst:SunburstLegend.ItemsPanel>
    
    <sunburst:SunburstLegend.ItemTemplate>
        <ItemContainerTemplate>
            <StackPanel Orientation="Horizontal" Margin="10,0">
                <Rectangle Fill="{Binding Interior}" 
                          Width="12" Height="12" 
                          Margin="0,0,5,0"/>
                <TextBlock Text="{Binding Label}" 
                          FontSize="11"/>
            </StackPanel>
        </ItemContainerTemplate>
    </sunburst:SunburstLegend.ItemTemplate>
</sunburst:SunburstLegend>
```

### Available Template Bindings

- **Label** – Category text
- **Interior** – Fill color (brush)
- **Stroke** – Border color
- **StrokeThickness** – Border width

## Legend Interactivity

Enable interactive behaviors using the `ClickAction` property.

### Click Actions

- **None** – No interaction (default)
- **ToggleSegmentSelection** – Click to select/highlight segments
- **ToggleSegmentVisibility** – Click to show/hide segments

### Toggle Segment Selection

Click legend items to highlight corresponding segments:

**XAML:**
```xml
<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend ClickAction="ToggleSegmentSelection"/>
</sunburst:SfSunburstChart.Legend>
```

**C#:**
```csharp
SunburstLegend legend = new SunburstLegend()
{
    ClickAction = LegendClickAction.ToggleSegmentSelection
};
chart.Legend = legend;
```

**Behavior:**
- Click legend item → Corresponding segments highlight
- Click again → Remove highlight
- Visual feedback through selection brush

**Use when:** Users need to focus on specific categories by highlighting them.

### Toggle Segment Visibility

Click legend items to show/hide segments:

**XAML:**
```xml
<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend ClickAction="ToggleSegmentVisibility"/>
</sunburst:SfSunburstChart.Legend>
```

**C#:**
```csharp
SunburstLegend legend = new SunburstLegend()
{
    ClickAction = LegendClickAction.ToggleSegmentVisibility
};
chart.Legend = legend;
```

**Behavior:**
- Click legend item → Corresponding segments disappear
- Click again → Segments reappear
- Chart proportions adjust dynamically

**Use when:** Users need to filter out categories to focus on remaining data.

### Combined with Selection Behavior

For full interactivity, combine legend clicks with selection behavior:

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <!-- Interactive legend -->
    <sunburst:SfSunburstChart.Legend>
        <sunburst:SunburstLegend ClickAction="ToggleSegmentSelection"/>
    </sunburst:SfSunburstChart.Legend>
    
    <!-- Selection behavior -->
    <sunburst:SfSunburstChart.Behaviors>
        <sunburst:SunburstSelectionBehavior EnableSelection="True"
                                           SelectionBrush="#FFFF6347"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

## Complete Example

Comprehensive legend configuration:

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding EmployeeData}" 
                          ValueMemberPath="Count"
                          Palette="Metro">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.Legend>
        <sunburst:SunburstLegend DockPosition="Left"
                                LegendIcon="Pentagon"
                                IconHeight="14"
                                IconWidth="14"
                                ClickAction="ToggleSegmentSelection"
                                FontSize="11"
                                Margin="10">
            
            <!-- Custom layout for better spacing -->
            <sunburst:SunburstLegend.ItemsPanel>
                <ItemsPanelTemplate>
                    <StackPanel Orientation="Vertical" 
                               Margin="5"/>
                </ItemsPanelTemplate>
            </sunburst:SunburstLegend.ItemsPanel>
            
        </sunburst:SunburstLegend>
    </sunburst:SfSunburstChart.Legend>
    
    <!-- Enable selection for legend clicks -->
    <sunburst:SfSunburstChart.Behaviors>
        <sunburst:SunburstSelectionBehavior EnableSelection="True"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

## Best Practices

### 1. Positioning

- **Many categories (6+):** Use Left or Right position for vertical stacking
- **Few categories (2-5):** Use Top or Bottom for horizontal layout
- **Small chart areas:** Place legend outside the chart (any position)

### 2. Icon Selection

- **Simple, clear data:** Use Circle (default) or Rectangle
- **Formal presentations:** Use Pentagon or Diamond
- **Brand consistency:** Use Custom icons matching corporate identity
- **Size appropriately:** Match icon size to font size (typically 12-15px)

### 3. Interactivity

- **Exploratory analysis:** Use `ToggleSegmentVisibility` to filter categories
- **Highlighting focus areas:** Use `ToggleSegmentSelection` to emphasize
- **Static reports:** Leave as `None` (no interaction)

### 4. Layout Optimization

- Ensure legend doesn't overlap chart
- Leave adequate margin (5-10px minimum)
- For many items, use WrapPanel to prevent vertical overflow
- Consider scrollable legend for 15+ categories

### 5. Accessibility

- Ensure sufficient color contrast for legend text
- Use clear, descriptive category labels
- Maintain minimum font size of 10px
- Provide alternative data identification (tooltips, data labels)

### 6. Performance

- Avoid complex custom templates for large datasets
- Use simple shapes (Circle, Rectangle) for best performance
- Limit legend items to first level only (default behavior)

## Troubleshooting

**Legend not appearing:**
- Verify Legend property is set (not null)
- Check that data has first-level categories
- Ensure legend is not positioned outside visible area

**Legend items incorrect:**
- Legend shows first hierarchy level only
- Verify first level's GroupMemberPath has valid data
- Check that categories are distinct (no duplicates needed)

**Icons not displaying correctly:**
- Verify LegendIcon is set to valid value
- For Custom icons, ensure LegendIcon="Custom" is set
- Check IconHeight and IconWidth are not zero

**Click actions not working:**
- Verify ClickAction is set properly
- For ToggleSegmentSelection, ensure SelectionBehavior is added
- Check that chart has EnableSelection="True"

**Layout issues:**
- Adjust DockPosition (try different sides)
- Use custom ItemsPanel for better control
- Set explicit Width/Height on custom panels
- Add appropriate Margin values

**Custom template not rendering:**
- Verify DataTemplate syntax
- Check binding paths (Label, Interior, Stroke)
- Ensure template elements have proper sizing
- Test with simple template first, then add complexity
