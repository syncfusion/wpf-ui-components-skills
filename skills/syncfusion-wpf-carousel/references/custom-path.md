# Custom Path in WPF Carousel

## Table of Contents
- [Overview](#overview)
- [Loading in Custom Path Mode](#loading-in-custom-path-mode)
- [Defining Custom Paths](#defining-custom-paths)
- [Items Per Page](#items-per-page)
- [Global Resize with ScaleFraction](#global-resize-with-scalefraction)
- [Individual Resize with ScaleFractions](#individual-resize-with-scalefractions)
- [Global Opacity with OpacityFraction](#global-opacity-with-opacityfraction)
- [Individual Opacity with OpacityFractions](#individual-opacity-with-opacityfractions)
- [Global Skewing](#global-skewing)
- [Individual Skewing with Fractions](#individual-skewing-with-fractions)
- [Advanced Examples](#advanced-examples)

## Overview

Custom Path mode allows you to define any geometric path for carousel item positioning. Items can be arranged along lines, curves, arcs, or complex shapes. This provides unlimited creative freedom for unique visual layouts beyond the standard circular arrangement.

**Key Features:**
- Custom geometric paths (lines, curves, beziers, arcs)
- Page-based item display
- Individual item scaling per position
- Individual item opacity per position
- Individual item skewing per position
- Looping support
- Full Path data support

## Loading in Custom Path Mode

Enable Custom Path mode and define the path geometry for item positioning.

### Enable Custom Path Mode

```xml
<syncfusion:Carousel Name="carousel" 
                     VisualMode="CustomPath"
                     SelectedIndex="3"
                     ItemsSource="{Binding HeaderCollection}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,100 L500,100" />
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Border Height="50" 
                    Width="100" 
                    BorderBrush="Purple" 
                    BorderThickness="5"
                    Background="LightBlue">
                <TextBlock HorizontalAlignment="Center" 
                           VerticalAlignment="Center" 
                           Text="{Binding Header}"/>
            </Border>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

```csharp
carousel.VisualMode = VisualMode.CustomPath;
carousel.SelectedIndex = 3;
```

**Required Properties:**
- `VisualMode="CustomPath"`: Enable custom path mode
- `Carousel.Path`: Define the geometric path

**Default Path:**
If no path is specified, items are arranged in a U-shaped default path.

### Complete Example with Data

```csharp
// Model.cs
public class CarouselModel
{
    public string Header { get; set; }
}

// ViewModel.cs
using System.Collections.ObjectModel;

public class ViewModel
{
    private ObservableCollection<CarouselModel> collection;
    
    public ObservableCollection<CarouselModel> HeaderCollection
    {
        get { return collection; }
        set { collection = value; }
    }
    
    public ViewModel()
    {
        HeaderCollection = new ObservableCollection<CarouselModel>();
        HeaderCollection.Add(new CarouselModel() { Header = "Buchanan" });
        HeaderCollection.Add(new CarouselModel() { Header = "Callahan" });
        HeaderCollection.Add(new CarouselModel() { Header = "Davolio" });
        HeaderCollection.Add(new CarouselModel() { Header = "Dodsworth" });
        HeaderCollection.Add(new CarouselModel() { Header = "Fuller" });
        HeaderCollection.Add(new CarouselModel() { Header = "King" });
        HeaderCollection.Add(new CarouselModel() { Header = "Leverling" });
        HeaderCollection.Add(new CarouselModel() { Header = "Suyama" });
    }
}
```

## Defining Custom Paths

Use WPF Path geometry to define how items are positioned.

### Linear Horizontal Path

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0" 
              Stroke="Blue" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```
- Items arranged in a straight horizontal line
- Start: (0,0), End: (500,0)

### Linear Vertical Path

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L0,400" 
              Stroke="Red" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```
- Items arranged vertically
- Start: (0,0), End: (0,400)

### Diagonal Path

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,100 L500,20" 
              Stroke="Green" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```
- Items arranged diagonally
- Creates ascending/descending effect

### Curved Path (Quadratic Bezier)

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,200 Q250,0 500,200" 
              Stroke="Purple" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```
- Smooth curve through control point
- Q: Quadratic bezier
- Format: `Q [control point] [end point]`

### Arc Path

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 A200,200 0 0 1 400,0" 
              Stroke="Orange" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```
- Circular arc segment
- A: Arc command
- Format: `A [radiusX,radiusY] [rotation] [large-arc-flag] [sweep-flag] [end point]`

### S-Curve Path (Cubic Bezier)

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,100 C100,0 200,200 300,100" 
              Stroke="DarkBlue" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```
- Smooth S-curve
- C: Cubic bezier
- Format: `C [control1] [control2] [end]`

### Wave Path

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,100 Q125,0 250,100 T500,100" 
              Stroke="Teal" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```
- Repeating wave pattern
- T: Smooth quadratic continuation

### Complex Multi-Segment Path

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,200 L100,50 Q200,0 300,50 L400,200" 
              Stroke="Crimson" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```
- Combines lines and curves
- Creates custom shape

### Path Visibility

Control whether the path line is visible:

```xml
<!-- Visible path for debugging -->
<syncfusion:Carousel.Path>
    <Path Data="M0,0 L500,0" 
          Stroke="Blue" 
          StrokeThickness="2"
          Visibility="Visible"/>
</syncfusion:Carousel.Path>

<!-- Hidden path (items still follow it) -->
<syncfusion:Carousel.Path>
    <Path Data="M0,0 L500,0" 
          Stroke="Transparent" 
          StrokeThickness="0"
          Visibility="Collapsed"/>
</syncfusion:Carousel.Path>
```

## Items Per Page

Control how many items are visible at once in custom path mode.

### Set Items Per Page

```xml
<syncfusion:Carousel ItemsPerPage="5"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding HeaderCollection}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```

```csharp
carousel.ItemsPerPage = 5;
carousel.VisualMode = VisualMode.CustomPath;
```

**Default Value:** `-1` (all items visible)

### Paging Behavior

**ItemsPerPage = -1 (Default):**
- All items displayed simultaneously
- Single view shows entire collection
- May overcrowd if many items

**ItemsPerPage = 3:**
- Only 3 items visible at once
- Navigate to see more items
- Cleaner layout with focused view

**ItemsPerPage = 7:**
- 7 items visible per page
- Good for medium-sized collections
- Balances visibility and clarity

### Page Navigation

Use Page Up/Down or commands to navigate between pages:

```csharp
// Navigate to next page
carousel.SelectNextPage();

// Navigate to previous page
carousel.SelectPreviousPage();
```

```xml
<Button Content="Previous Page" 
        Command="{Binding ElementName=carousel, Path=SelectPreviousPageCommand}"/>
<Button Content="Next Page" 
        Command="{Binding ElementName=carousel, Path=SelectNextPageCommand}"/>
```

**Note:** `ItemsPerPage` only works in `VisualMode="CustomPath"`. It has no effect in Standard mode.

## Global Resize with ScaleFraction

Apply uniform scaling to all non-selected items in custom path mode.

### Set Global Scale

```xml
<syncfusion:Carousel ScaleFraction="0.7" 
                     ScalingEnabled="True"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding Items}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```

```csharp
carousel.ScaleFraction = 0.7;
carousel.ScalingEnabled = true;
carousel.VisualMode = VisualMode.CustomPath;
```

**Properties:**
- `ScaleFraction`: Scale for non-selected items (0.0 to 1.0)
- `ScalingEnabled`: Enable/disable scaling

**Default Values:**
- `ScaleFraction`: `Double.NaN` (no scaling in CustomPath)
- `ScalingEnabled`: `true`

### Disable Global Scaling

```xml
<syncfusion:Carousel ScalingEnabled="False"
                     VisualMode="CustomPath"/>
```

```csharp
carousel.ScalingEnabled = false;
```

## Individual Resize with ScaleFractions

Define different scale factors for items at different positions along the path.

### Define Scale Fractions

```xml
<syncfusion:Carousel ScalingEnabled="True"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding Items}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0"/>
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.ScaleFractions>
        <syncfusion:PathFractionCollection>
            <!-- Fraction: position along path (0.0 to 1.0) -->
            <!-- Value: scale at that position (0.0 to 1.0) -->
            
            <!-- Start of path: 60% size -->
            <syncfusion:FractionValue Fraction="0" Value="0.6"/>
            
            <!-- Middle of path: 90% size (selected item) -->
            <syncfusion:FractionValue Fraction="0.5" Value="0.9"/>
            
            <!-- End of path: invisible -->
            <syncfusion:FractionValue Fraction="1" Value="0"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.ScaleFractions>
</syncfusion:Carousel>
```

### Code-Behind Definition

```csharp
using Syncfusion.Windows.Shared;

FractionValue startFraction = new FractionValue() { Fraction = 0, Value = 0.6 };
FractionValue middleFraction = new FractionValue() { Fraction = 0.5, Value = 0.9 };
FractionValue endFraction = new FractionValue() { Fraction = 1, Value = 0 };

PathFractionCollection pathFractions = new PathFractionCollection();
pathFractions.Add(startFraction);
pathFractions.Add(middleFraction);
pathFractions.Add(endFraction);

carousel.ScaleFractions = pathFractions;
carousel.ScalingEnabled = true;
carousel.VisualMode = VisualMode.CustomPath;
```

### Scale Pattern Examples

**Gradual Fade In/Out:**
```xml
<syncfusion:Carousel.ScaleFractions>
    <syncfusion:PathFractionCollection>
        <syncfusion:FractionValue Fraction="0" Value="0.3"/>
        <syncfusion:FractionValue Fraction="0.25" Value="0.6"/>
        <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
        <syncfusion:FractionValue Fraction="0.75" Value="0.6"/>
        <syncfusion:FractionValue Fraction="1" Value="0.3"/>
    </syncfusion:PathFractionCollection>
</syncfusion:Carousel.ScaleFractions>
```

**Center Focus:**
```xml
<syncfusion:Carousel.ScaleFractions>
    <syncfusion:PathFractionCollection>
        <syncfusion:FractionValue Fraction="0" Value="0.5"/>
        <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
        <syncfusion:FractionValue Fraction="1" Value="0.5"/>
    </syncfusion:PathFractionCollection>
</syncfusion:Carousel.ScaleFractions>
```

**Asymmetric Scaling:**
```xml
<syncfusion:Carousel.ScaleFractions>
    <syncfusion:PathFractionCollection>
        <syncfusion:FractionValue Fraction="0" Value="0.8"/>
        <syncfusion:FractionValue Fraction="0.3" Value="1.0"/>
        <syncfusion:FractionValue Fraction="1" Value="0.4"/>
    </syncfusion:PathFractionCollection>
</syncfusion:Carousel.ScaleFractions>
```

## Global Opacity with OpacityFraction

Apply uniform opacity to all non-selected items.

### Set Global Opacity

```xml
<syncfusion:Carousel OpacityFraction="0.5"
                     OpacityEnabled="True"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding Items}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```

```csharp
carousel.OpacityFraction = 0.5;
carousel.OpacityEnabled = true;
carousel.VisualMode = VisualMode.CustomPath;
```

**Properties:**
- `OpacityFraction`: Opacity for non-selected items (0.0 to 1.0)
- `OpacityEnabled`: Enable/disable opacity effects

**Default Values:**
- `OpacityFraction`: `Double.NaN` (no opacity change)
- `OpacityEnabled`: `true`

### Disable Global Opacity

```xml
<syncfusion:Carousel OpacityEnabled="False"
                     VisualMode="CustomPath"/>
```

```csharp
carousel.OpacityEnabled = false;
```

## Individual Opacity with OpacityFractions

Define different opacity values for items at different positions along the path.

### Define Opacity Fractions

```xml
<syncfusion:Carousel OpacityEnabled="True"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding Items}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0"/>
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.OpacityFractions>
        <syncfusion:PathFractionCollection>
            <!-- Start: invisible -->
            <syncfusion:FractionValue Fraction="0" Value="0"/>
            
            <!-- Middle: fully visible (selected) -->
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            
            <!-- End: 70% visible -->
            <syncfusion:FractionValue Fraction="1" Value="0.7"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.OpacityFractions>
</syncfusion:Carousel>
```

### Code-Behind Definition

```csharp
FractionValue startOpacity = new FractionValue() { Fraction = 0, Value = 0 };
FractionValue centerOpacity = new FractionValue() { Fraction = 0.5, Value = 1.0 };
FractionValue endOpacity = new FractionValue() { Fraction = 1, Value = 0.7 };

PathFractionCollection opacityFractions = new PathFractionCollection();
opacityFractions.Add(startOpacity);
opacityFractions.Add(centerOpacity);
opacityFractions.Add(endOpacity);

carousel.OpacityFractions = opacityFractions;
carousel.OpacityEnabled = true;
carousel.VisualMode = VisualMode.CustomPath;
```

### Opacity Pattern Examples

**Fade Through Center:**
```xml
<syncfusion:Carousel.OpacityFractions>
    <syncfusion:PathFractionCollection>
        <syncfusion:FractionValue Fraction="0" Value="0.2"/>
        <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
        <syncfusion:FractionValue Fraction="1" Value="0.2"/>
    </syncfusion:PathFractionCollection>
</syncfusion:Carousel.OpacityFractions>
```

**Gradual Fade In:**
```xml
<syncfusion:Carousel.OpacityFractions>
    <syncfusion:PathFractionCollection>
        <syncfusion:FractionValue Fraction="0" Value="0"/>
        <syncfusion:FractionValue Fraction="0.3" Value="0.5"/>
        <syncfusion:FractionValue Fraction="0.6" Value="0.8"/>
        <syncfusion:FractionValue Fraction="1" Value="1.0"/>
    </syncfusion:PathFractionCollection>
</syncfusion:Carousel.OpacityFractions>
```

## Global Skewing

Apply uniform skewing to all carousel items.

### Set Global Skewing

```xml
<syncfusion:Carousel SkewAngleXFraction="10"
                     SkewAngleYFraction="20" 
                     SkewAngleXEnabled="True"
                     SkewAngleYEnabled="True"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding Items}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```

```csharp
carousel.SkewAngleXFraction = 10;
carousel.SkewAngleYFraction = 20;
carousel.SkewAngleXEnabled = true;
carousel.SkewAngleYEnabled = true;
carousel.VisualMode = VisualMode.CustomPath;
```

**Properties:**
- `SkewAngleXFraction`: Horizontal skew angle
- `SkewAngleYFraction`: Vertical skew angle
- `SkewAngleXEnabled`: Enable X-axis skewing
- `SkewAngleYEnabled`: Enable Y-axis skewing

**Default Values:**
- `SkewAngleXFraction`: `Double.NaN`
- `SkewAngleYFraction`: `Double.NaN`
- `SkewAngleXEnabled`: `false`
- `SkewAngleYEnabled`: `false`

## Individual Skewing with Fractions

Define different skew angles for items at different positions.

### Define Skew Fractions

```xml
<syncfusion:Carousel SkewAngleXEnabled="True"
                     SkewAngleYEnabled="True"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding Items}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0"/>
    </syncfusion:Carousel.Path>
    
    <!-- X-axis skewing -->
    <syncfusion:Carousel.SkewAngleXFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="20"/>
            <syncfusion:FractionValue Fraction="0.5" Value="0"/>
            <syncfusion:FractionValue Fraction="1" Value="-20"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.SkewAngleXFractions>
    
    <!-- Y-axis skewing -->
    <syncfusion:Carousel.SkewAngleYFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="15"/>
            <syncfusion:FractionValue Fraction="0.5" Value="0"/>
            <syncfusion:FractionValue Fraction="1" Value="15"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.SkewAngleYFractions>
</syncfusion:Carousel>
```

## Advanced Examples

### Example 1: Magazine Rack Effect

```xml
<syncfusion:Carousel VisualMode="CustomPath"
                     ScalingEnabled="True"
                     OpacityEnabled="True"
                     SkewAngleYEnabled="True"
                     ItemsPerPage="5">
    <syncfusion:Carousel.Path>
        <Path Data="M50,300 L450,300"/>
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.ScaleFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="0.5"/>
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            <syncfusion:FractionValue Fraction="1" Value="0.5"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.ScaleFractions>
    
    <syncfusion:Carousel.OpacityFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="0.4"/>
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            <syncfusion:FractionValue Fraction="1" Value="0.4"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.OpacityFractions>
    
    <syncfusion:Carousel.SkewAngleYFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="25"/>
            <syncfusion:FractionValue Fraction="0.5" Value="0"/>
            <syncfusion:FractionValue Fraction="1" Value="-25"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.SkewAngleYFractions>
</syncfusion:Carousel>
```

### Example 2: Arch Gallery

```xml
<syncfusion:Carousel VisualMode="CustomPath"
                     ScalingEnabled="True"
                     OpacityEnabled="True">
    <syncfusion:Carousel.Path>
        <Path Data="M0,300 Q250,50 500,300"/>
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.ScaleFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="0.6"/>
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            <syncfusion:FractionValue Fraction="1" Value="0.6"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.ScaleFractions>
    
    <syncfusion:Carousel.OpacityFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="0.5"/>
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            <syncfusion:FractionValue Fraction="1" Value="0.5"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.OpacityFractions>
</syncfusion:Carousel>
```

### Example 3: Perspective Lineup

```xml
<syncfusion:Carousel VisualMode="CustomPath"
                     ScalingEnabled="True"
                     OpacityEnabled="True"
                     SkewAngleXEnabled="True"
                     SkewAngleYEnabled="True">
    <syncfusion:Carousel.Path>
        <Path Data="M100,200 L400,200"/>
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.ScaleFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="0.3"/>
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            <syncfusion:FractionValue Fraction="1" Value="0.3"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.ScaleFractions>
    
    <syncfusion:Carousel.OpacityFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="0.3"/>
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            <syncfusion:FractionValue Fraction="1" Value="0.3"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.OpacityFractions>
    
    <syncfusion:Carousel.SkewAngleXFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="30"/>
            <syncfusion:FractionValue Fraction="0.5" Value="0"/>
            <syncfusion:FractionValue Fraction="1" Value="-30"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.SkewAngleXFractions>
    
    <syncfusion:Carousel.SkewAngleYFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="20"/>
            <syncfusion:FractionValue Fraction="0.5" Value="0"/>
            <syncfusion:FractionValue Fraction="1" Value="20"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.SkewAngleYFractions>
</syncfusion:Carousel>
```
