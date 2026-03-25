# Standard Path in WPF Carousel

## Table of Contents
- [Overview](#overview)
- [Loading in Standard Path Mode](#loading-in-standard-path-mode)
- [Changing Radius](#changing-radius)
- [Rotation Speed and Animation](#rotation-speed-and-animation)
- [Resizing Items](#resizing-items)
- [Opacity Effects](#opacity-effects)
- [Skewing Items](#skewing-items)
- [Best Practices](#best-practices)

## Overview

Standard Path mode displays carousel items in a circular 3D rotating pattern. This is the default visual mode and provides an elegant circular interface for browsing items. Items are arranged along an elliptical path with the selected item typically positioned at the center or front.

**Key Features:**
- Circular/elliptical item arrangement
- 3D perspective effects
- Configurable radius (RadiusX, RadiusY)
- Rotation animation with adjustable speed
- Scaling effects for depth perception
- Opacity effects for focus
- Skewing for 3D appearance

## Loading in Standard Path Mode

Standard mode is the default visual mode. Items are automatically arranged in a circular pattern.

### Enable Standard Mode

```xml
<syncfusion:Carousel Name="carousel" 
                     VisualMode="Standard"
                     ItemsSource="{Binding HeaderCollection}">
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
carousel.VisualMode = VisualMode.Standard;
```

**Default Value:** `VisualMode.Standard`

### Complete Example with Data Binding

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

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <syncfusion:Carousel Name="carousel" 
                         VisualMode="Standard"
                         ItemsSource="{Binding HeaderCollection}">
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
</Grid>
```

## Changing Radius

Control the size and shape of the circular path using `RadiusX` and `RadiusY` properties. These determine the horizontal and vertical dimensions of the ellipse on which items are positioned.

### Set Radius Properties

```xml
<syncfusion:Carousel RadiusX="200" 
                     RadiusY="150" 
                     VisualMode="Standard"
                     ItemsSource="{Binding Items}"/>
```

```csharp
carousel.RadiusX = 200;
carousel.RadiusY = 150;
carousel.VisualMode = VisualMode.Standard;
```

**Default Values:**
- `RadiusX`: `250`
- `RadiusY`: `150`

### Creating Different Shapes

**Perfect Circle:**
```xml
<syncfusion:Carousel RadiusX="200" 
                     RadiusY="200"
                     VisualMode="Standard"/>
```

**Wide Ellipse:**
```xml
<syncfusion:Carousel RadiusX="300" 
                     RadiusY="100"
                     VisualMode="Standard"/>
```

**Tall Ellipse:**
```xml
<syncfusion:Carousel RadiusX="100" 
                     RadiusY="250"
                     VisualMode="Standard"/>
```

**Small Tight Circle:**
```xml
<syncfusion:Carousel RadiusX="80" 
                     RadiusY="80"
                     VisualMode="Standard"/>
```

### Radius Considerations

**Large Radius (RadiusX/Y > 250):**
- Items spread further apart
- More screen space required
- Better for larger items
- More dramatic rotation effect

**Small Radius (RadiusX/Y < 100):**
- Items clustered tightly
- Less screen space needed
- Better for smaller items
- More compact appearance

**Responsive Radius:**
```csharp
// Adjust radius based on window size
private void Window_SizeChanged(object sender, SizeChangedEventArgs e)
{
    double width = e.NewSize.Width;
    double height = e.NewSize.Height;
    
    carousel.RadiusX = Math.Min(width * 0.35, 300);
    carousel.RadiusY = Math.Min(height * 0.3, 200);
}
```

## Rotation Speed and Animation

Control how quickly items rotate when navigating between selections.

### Set Rotation Speed

```xml
<syncfusion:Carousel RotationSpeed="300"
                     VisualMode="Standard"
                     ItemsSource="{Binding Items}"/>
```

```csharp
carousel.RotationSpeed = 300;
carousel.VisualMode = VisualMode.Standard;
```

**Default Value:** `200` (milliseconds)

### Speed Examples

**Fast Rotation (RotationSpeed < 150):**
```xml
<syncfusion:Carousel RotationSpeed="100"
                     VisualMode="Standard"/>
```
- Quick, snappy transitions
- Less dramatic effect
- Better for rapid browsing

**Normal Rotation (RotationSpeed = 200-300):**
```xml
<syncfusion:Carousel RotationSpeed="250"
                     VisualMode="Standard"/>
```
- Balanced animation speed
- Smooth user experience
- Default recommended speed

**Slow Rotation (RotationSpeed > 400):**
```xml
<syncfusion:Carousel RotationSpeed="500"
                     VisualMode="Standard"/>
```
- Dramatic, cinematic effect
- More visible rotation
- Better for presentations

### Disable Rotation Animation

Turn off animated rotation for instant item switching:

```xml
<syncfusion:Carousel EnableRotationAnimation="False" 
                     VisualMode="Standard"
                     ItemsSource="{Binding Items}"/>
```

```csharp
carousel.EnableRotationAnimation = false;
carousel.VisualMode = VisualMode.Standard;
```

**Default Value:** `true`

**When to disable animation:**
- Performance-critical scenarios
- Accessibility requirements
- User preference for reduced motion
- Rapid item switching needed

## Resizing Items

Apply scaling effects to non-selected items to create depth perception and focus on the selected item.

### Enable Scaling

```xml
<syncfusion:Carousel ScaleFraction="0.7" 
                     ScalingEnabled="True"
                     VisualMode="Standard"
                     ItemsSource="{Binding Items}"/>
```

```csharp
carousel.ScaleFraction = 0.7;
carousel.ScalingEnabled = true;
carousel.VisualMode = VisualMode.Standard;
```

**Properties:**
- `ScaleFraction`: Scale factor for non-selected items (0.0 to 1.0)
- `ScalingEnabled`: Enable/disable scaling effect

**Default Values:**
- `ScaleFraction`: `0` (no scaling)
- `ScalingEnabled`: `true`

### Scale Fraction Values

**No Scaling (ScaleFraction = 0):**
```xml
<syncfusion:Carousel ScaleFraction="0"/>
```
- All items same size
- No depth effect
- Uniform appearance

**Subtle Scaling (ScaleFraction = 0.8-0.9):**
```xml
<syncfusion:Carousel ScaleFraction="0.85"/>
```
- Non-selected items slightly smaller
- Gentle depth effect
- Selected item stands out subtly

**Moderate Scaling (ScaleFraction = 0.5-0.7):**
```xml
<syncfusion:Carousel ScaleFraction="0.6"/>
```
- Clear size difference
- Good depth perception
- Recommended for most scenarios

**Dramatic Scaling (ScaleFraction = 0.2-0.4):**
```xml
<syncfusion:Carousel ScaleFraction="0.3"/>
```
- Very small non-selected items
- Strong focus on selected item
- Dramatic 3D effect

**Extreme Scaling (ScaleFraction < 0.2):**
```xml
<syncfusion:Carousel ScaleFraction="0.1"/>
```
- Non-selected items tiny
- Maximum focus on selection
- May reduce usability

### Disable Scaling

```xml
<syncfusion:Carousel ScalingEnabled="False"
                     VisualMode="Standard"/>
```

```csharp
carousel.ScalingEnabled = false;
```

## Opacity Effects

Apply opacity effects to non-selected items to further emphasize the selected item.

### Enable Opacity Effects

```xml
<syncfusion:Carousel OpacityFraction="0.5"
                     OpacityEnabled="True"
                     VisualMode="Standard"
                     ItemsSource="{Binding Items}"/>
```

```csharp
carousel.OpacityFraction = 0.5;
carousel.OpacityEnabled = true;
carousel.VisualMode = VisualMode.Standard;
```

**Properties:**
- `OpacityFraction`: Opacity for non-selected items (0.0 to 1.0)
- `OpacityEnabled`: Enable/disable opacity effect

**Default Values:**
- `OpacityFraction`: `0` (no opacity change)
- `OpacityEnabled`: `true`

### Opacity Fraction Values

**Full Opacity (OpacityFraction = 1.0 or OpacityEnabled = False):**
```xml
<syncfusion:Carousel OpacityFraction="1.0"/>
```
- All items fully visible
- No emphasis on selection
- No transparency effect

**Subtle Transparency (OpacityFraction = 0.7-0.9):**
```xml
<syncfusion:Carousel OpacityFraction="0.8"/>
```
- Non-selected items slightly faded
- Gentle focus on selection
- Items still clearly visible

**Moderate Transparency (OpacityFraction = 0.4-0.6):**
```xml
<syncfusion:Carousel OpacityFraction="0.5"/>
```
- Clear visual hierarchy
- Good balance between visible and faded
- Recommended for most scenarios

**Heavy Transparency (OpacityFraction = 0.2-0.3):**
```xml
<syncfusion:Carousel OpacityFraction="0.25"/>
```
- Non-selected items very faded
- Strong focus on selected item
- May reduce context visibility

**Near Invisible (OpacityFraction < 0.2):**
```xml
<syncfusion:Carousel OpacityFraction="0.1"/>
```
- Non-selected items barely visible
- Maximum focus on selection
- May confuse users about available items

### Disable Opacity Effects

```xml
<syncfusion:Carousel OpacityEnabled="False"
                     VisualMode="Standard"/>
```

```csharp
carousel.OpacityEnabled = false;
```

### Combining Scaling and Opacity

For maximum 3D effect, combine scaling and opacity:

```xml
<syncfusion:Carousel ScaleFraction="0.6"
                     ScalingEnabled="True"
                     OpacityFraction="0.4"
                     OpacityEnabled="True"
                     VisualMode="Standard"
                     ItemsSource="{Binding Items}"/>
```

This creates a strong visual hierarchy where:
- Selected item is full size and fully opaque
- Non-selected items are 60% size and 40% opaque
- Creates excellent depth perception

## Skewing Items

Apply skew transformations to create a 3D tilted appearance for carousel items.

### Enable Skewing

```xml
<syncfusion:Carousel SkewAngleXFraction="20"
                     SkewAngleYFraction="10" 
                     SkewAngleXEnabled="True"
                     SkewAngleYEnabled="True"
                     VisualMode="Standard"
                     ItemsSource="{Binding Items}"/>
```

```csharp
carousel.SkewAngleXFraction = 20;
carousel.SkewAngleYFraction = 10;
carousel.SkewAngleXEnabled = true;
carousel.SkewAngleYEnabled = true;
carousel.VisualMode = VisualMode.Standard;
```

**Properties:**
- `SkewAngleXFraction`: Horizontal skew angle
- `SkewAngleYFraction`: Vertical skew angle
- `SkewAngleXEnabled`: Enable/disable X-axis skewing
- `SkewAngleYEnabled`: Enable/disable Y-axis skewing

**Default Values:**
- `SkewAngleXFraction`: `0`
- `SkewAngleYFraction`: `0`
- `SkewAngleXEnabled`: `false`
- `SkewAngleYEnabled`: `false`

### Skew Examples

**Horizontal Skew Only:**
```xml
<syncfusion:Carousel SkewAngleXFraction="15"
                     SkewAngleXEnabled="True"
                     SkewAngleYEnabled="False"
                     VisualMode="Standard"/>
```
- Items tilted left/right
- Creates horizontal perspective

**Vertical Skew Only:**
```xml
<syncfusion:Carousel SkewAngleYFraction="20"
                     SkewAngleXEnabled="False"
                     SkewAngleYEnabled="True"
                     VisualMode="Standard"/>
```
- Items tilted up/down
- Creates vertical perspective

**Combined Skewing:**
```xml
<syncfusion:Carousel SkewAngleXFraction="15"
                     SkewAngleYFraction="15" 
                     SkewAngleXEnabled="True"
                     SkewAngleYEnabled="True"
                     VisualMode="Standard"/>
```
- Items tilted both directions
- Creates complex 3D effect

**Subtle Skewing (Angles < 10):**
```xml
<syncfusion:Carousel SkewAngleXFraction="5"
                     SkewAngleYFraction="5"/>
```
- Gentle 3D effect
- Maintains readability
- Professional appearance

**Dramatic Skewing (Angles > 20):**
```xml
<syncfusion:Carousel SkewAngleXFraction="30"
                     SkewAngleYFraction="25"/>
```
- Strong 3D effect
- May reduce readability
- Artistic appearance

### Disable Skewing

```xml
<syncfusion:Carousel SkewAngleXEnabled="False"
                     SkewAngleYEnabled="False"
                     VisualMode="Standard"/>
```

```csharp
carousel.SkewAngleXEnabled = false;
carousel.SkewAngleYEnabled = false;
```

## Best Practices

### 1. Balanced Visual Effects

Don't overuse all effects simultaneously:

**Good - Moderate Effects:**
```xml
<syncfusion:Carousel ScaleFraction="0.7"
                     OpacityFraction="0.6"
                     RotationSpeed="250"
                     VisualMode="Standard"/>
```

**Avoid - Too Many Heavy Effects:**
```xml
<syncfusion:Carousel ScaleFraction="0.2"
                     OpacityFraction="0.1"
                     SkewAngleXFraction="40"
                     SkewAngleYFraction="40"
                     RotationSpeed="800"/>
```

### 2. Performance Considerations

**For Better Performance:**
- Use moderate rotation speeds (200-300ms)
- Avoid extreme scaling (<0.3)
- Enable virtualization for large datasets
- Disable animations if not needed

```xml
<syncfusion:Carousel EnableVirtualization="True"
                     RotationSpeed="200"
                     ScaleFraction="0.7"/>
```

### 3. Accessibility

**Consider Users with Motion Sensitivity:**
```csharp
// Check system settings
bool reduceMotion = SystemParameters.ClientAreaAnimation == false;

if (reduceMotion)
{
    carousel.EnableRotationAnimation = false;
    carousel.RotationSpeed = 0;
}
```

### 4. Responsive Design

Adjust properties based on available space:

```csharp
private void UpdateCarouselForSize(double width, double height)
{
    if (width < 600)
    {
        // Small screen
        carousel.RadiusX = 150;
        carousel.RadiusY = 100;
        carousel.ScaleFraction = 0.8;
    }
    else if (width < 1000)
    {
        // Medium screen
        carousel.RadiusX = 200;
        carousel.RadiusY = 150;
        carousel.ScaleFraction = 0.7;
    }
    else
    {
        // Large screen
        carousel.RadiusX = 300;
        carousel.RadiusY = 200;
        carousel.ScaleFraction = 0.6;
    }
}
```

### 5. Content-Appropriate Settings

**Text-Heavy Items:**
```xml
<syncfusion:Carousel ScaleFraction="0.8"
                     OpacityFraction="0.7"
                     SkewAngleXEnabled="False"
                     SkewAngleYEnabled="False"/>
```
- Maintain readability
- Avoid extreme skewing

**Image Gallery:**
```xml
<syncfusion:Carousel ScaleFraction="0.5"
                     OpacityFraction="0.3"
                     SkewAngleXFraction="10"
                     SkewAngleYFraction="10"/>
```
- Dramatic effects work well
- Images remain recognizable even when scaled

### 6. Testing

Test your configuration with:
- Different numbers of items (3, 8, 15, 50+)
- Various item sizes
- Different screen resolutions
- Keyboard navigation
- Mouse wheel scrolling
- Touch interactions (if applicable)
