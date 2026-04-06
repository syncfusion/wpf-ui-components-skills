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
Use WPF `Path` geometry to define item positions for `VisualMode="CustomPath"`.

For compact examples and advanced path patterns (infinity, figure-8, spiral, heart, arc examples), see `advanced-features.md`.

### Quick Path Example

```xml
<syncfusion:Carousel VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0" />
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```

This arranges items on a horizontal line; for curved, arc, and multi-segment examples see `advanced-features.md`.

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

The visual-effect controls (scaling, opacity, skew and per-position fraction collections) have been moved to `visual-effects.md` to avoid repeated content.

For concrete fraction-collection examples (`ScaleFractions`, `OpacityFractions`, `SkewAngle*Fractions`) and best-practice values, see: [visual-effects.md](visual-effects.md).

## Advanced Examples
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
