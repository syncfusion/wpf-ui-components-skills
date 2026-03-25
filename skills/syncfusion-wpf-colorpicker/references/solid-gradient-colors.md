# Solid and Gradient Colors

This reference covers selecting and managing solid colors and gradient brushes in the WPF ColorPicker.

## Table of Contents
- [Selecting Solid Colors](#selecting-solid-colors)
- [Working with Linear Gradients](#working-with-linear-gradients)
- [Working with Radial Gradients](#working-with-radial-gradients)
- [Runtime Color Editing](#runtime-color-editing)
- [Managing Opacity (Alpha Channel)](#managing-opacity-alpha-channel)
- [Switching Between Solid and Gradient Modes](#switching-between-solid-and-gradient-modes)

## Selecting Solid Colors

Solid colors are the most common color selection mode in ColorPicker. Use the `Color` property to set or retrieve a solid color value.

### Setting Initial Solid Color in XAML
```xaml
<syncfusion:ColorPicker x:Name="colorPicker" Color="Yellow"/>
```

### Setting Initial Solid Color in C#
```csharp
ColorPicker colorPicker = new ColorPicker();
colorPicker.Color = Colors.Yellow;
```

### Retrieving Selected Color
```csharp
Color selectedColor = colorPicker.Color;
MessageBox.Show($"Selected color: {selectedColor.ToString()}");
```

### Using Named Colors
```xaml
<syncfusion:ColorPicker Color="Red"/>
<syncfusion:ColorPicker Color="Blue"/>
<syncfusion:ColorPicker Color="LimeGreen"/>
<syncfusion:ColorPicker Color="Purple"/>
```

### Creating Custom Colors (RGB)
```csharp
// Create a custom color: RGB(255, 100, 50)
Color customColor = Color.FromArgb(255, 255, 100, 50);
colorPicker.Color = customColor;
```

### Built-in Color Palette
Enable the color palette dropdown for quick color selection:
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="True"/>
```

Users can click the palette dropdown to select from predefined colors without using the picker region.

## Working with Linear Gradients

Linear gradients blend multiple colors along a straight line defined by start and end points. Use the `Brush` property to set a `LinearGradientBrush`.

### Creating a Linear Gradient in XAML
```xaml
<syncfusion:ColorPicker x:Name="colorPicker" Width="200">
    <syncfusion:ColorPicker.Brush>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" />
            <GradientStop Color="Red" Offset="0.25" />
            <GradientStop Color="Blue" Offset="0.75" />
            <GradientStop Color="LimeGreen" Offset="1.0" />
        </LinearGradientBrush>
    </syncfusion:ColorPicker.Brush>
</syncfusion:ColorPicker>
```

### Creating a Linear Gradient in C#
```csharp
LinearGradientBrush linearGradient = new LinearGradientBrush();
linearGradient.StartPoint = new Point(0, 0);
linearGradient.EndPoint = new Point(1, 1);

linearGradient.GradientStops.Add(new GradientStop(Colors.Yellow, 0.0));
linearGradient.GradientStops.Add(new GradientStop(Colors.Red, 0.25));
linearGradient.GradientStops.Add(new GradientStop(Colors.Blue, 0.75));
linearGradient.GradientStops.Add(new GradientStop(Colors.LimeGreen, 1.0));

ColorPicker colorPicker = new ColorPicker();
colorPicker.Brush = linearGradient;
```

### Understanding Linear Gradient Properties

- **StartPoint** - Coordinates where the gradient begins (typically 0,0 for top-left)
- **EndPoint** - Coordinates where the gradient ends (1,1 for bottom-right creates diagonal)
- **GradientStops** - Collection of Color + Offset pairs
  - **Color** - The color at this stop
  - **Offset** - Position (0.0-1.0) along the gradient axis

### Common Gradient Directions

**Left to Right:**
```csharp
linearGradient.StartPoint = new Point(0, 0);
linearGradient.EndPoint = new Point(1, 0);
```

**Top to Bottom:**
```csharp
linearGradient.StartPoint = new Point(0, 0);
linearGradient.EndPoint = new Point(0, 1);
```

**Diagonal (Top-Left to Bottom-Right):**
```csharp
linearGradient.StartPoint = new Point(0, 0);
linearGradient.EndPoint = new Point(1, 1);
```

**Diagonal (Top-Right to Bottom-Left):**
```csharp
linearGradient.StartPoint = new Point(1, 0);
linearGradient.EndPoint = new Point(0, 1);
```

### Adding More Gradient Stops
```csharp
LinearGradientBrush gradient = new LinearGradientBrush();
gradient.StartPoint = new Point(0, 0);
gradient.EndPoint = new Point(1, 0);

// Add 5 color stops for smooth transition
gradient.GradientStops.Add(new GradientStop(Colors.Red, 0.0));
gradient.GradientStops.Add(new GradientStop(Colors.Yellow, 0.25));
gradient.GradientStops.Add(new GradientStop(Colors.Green, 0.5));
gradient.GradientStops.Add(new GradientStop(Colors.Blue, 0.75));
gradient.GradientStops.Add(new GradientStop(Colors.Purple, 1.0));

colorPicker.Brush = gradient;
```

## Working with Radial Gradients

Radial gradients blend colors outward from a center point in a circular pattern. Use `RadialGradientBrush` with the `Brush` property.

### Creating a Radial Gradient in XAML
```xaml
<syncfusion:ColorPicker x:Name="colorPicker" Width="200">
    <syncfusion:ColorPicker.Brush>
        <RadialGradientBrush GradientOrigin="0.5,0.5" 
                             Center="0.5,0.5" 
                             RadiusX="0.5" 
                             RadiusY="0.5">
            <GradientStop Color="Yellow" Offset="0" />
            <GradientStop Color="Red" Offset="0.25" />
            <GradientStop Color="Blue" Offset="0.75" />
            <GradientStop Color="LimeGreen" Offset="1" />
        </RadialGradientBrush>
    </syncfusion:ColorPicker.Brush>
</syncfusion:ColorPicker>
```

### Creating a Radial Gradient in C#
```csharp
RadialGradientBrush radialGradient = new RadialGradientBrush();
radialGradient.GradientOrigin = new Point(0.5, 0.5);
radialGradient.Center = new Point(0.5, 0.5);
radialGradient.RadiusX = 0.5;
radialGradient.RadiusY = 0.5;

radialGradient.GradientStops.Add(new GradientStop(Colors.Yellow, 0.0));
radialGradient.GradientStops.Add(new GradientStop(Colors.Red, 0.25));
radialGradient.GradientStops.Add(new GradientStop(Colors.Blue, 0.75));
radialGradient.GradientStops.Add(new GradientStop(Colors.LimeGreen, 1.0));

ColorPicker colorPicker = new ColorPicker();
colorPicker.Brush = radialGradient;
```

### Understanding Radial Gradient Properties

- **GradientOrigin** - Center point where gradient originates (normalized 0.0-1.0)
- **Center** - Center of the circular gradient
- **RadiusX** - Horizontal radius (0.5 = 50% of width)
- **RadiusY** - Vertical radius (0.5 = 50% of height)
- **GradientStops** - Collection of colors and their positions (same as linear)

### Oval vs Circular Radial Gradients

**Circular (equal radius):**
```csharp
radialGradient.RadiusX = 0.5;
radialGradient.RadiusY = 0.5;
```

**Oval (different radii):**
```csharp
radialGradient.RadiusX = 0.7;  // Wider horizontally
radialGradient.RadiusY = 0.3;  // Narrower vertically
```

### Offset Gradient Origin
```csharp
// Gradient originates from top-left corner
radialGradient.GradientOrigin = new Point(0, 0);
radialGradient.Center = new Point(0, 0);

// Gradient originates from center but displays from offset point
radialGradient.GradientOrigin = new Point(0.7, 0.3);
radialGradient.Center = new Point(0.5, 0.5);
```

## Runtime Color Editing

ColorPicker allows users to edit colors interactively at runtime through:

1. **Picker Region** - Click and drag to select hue and saturation
2. **Hue Slider** - Drag the vertical slider to change hue
3. **Value Editors** - Directly input RGB, HSV, or Hex values
4. **Alpha Slider** - Adjust opacity (if visible)

### Enabling Runtime Editing
By default, runtime editing is enabled. Simply display the ColorPicker and users can:
- Drag within the color picker region to select hue and saturation
- Drag the hue slider to change the hue value
- Edit RGB, HSV, or Hex input fields directly
- Type decimal values (0-255 for RGB, 0-360 for H, 0-100 for S/V)

### Retrieving Runtime Changes
```csharp
ColorPicker colorPicker = new ColorPicker();

// Subscribe to color changes
colorPicker.ColorChanged += (d, e) =>
{
    Color newColor = (Color)e.NewValue;
    UpdateUI(newColor);
};

// Retrieve current color anytime
Color current = colorPicker.Color;
```

### Programmatic Color Updates
```csharp
// Update color from code (triggers ColorChanged)
colorPicker.Color = Colors.BlueViolet;

// Update brush from code (triggers SelectedBrushChanged)
colorPicker.Brush = new SolidColorBrush(Colors.Orange);
```

## Managing Opacity (Alpha Channel)

The alpha channel controls color transparency (0 = fully transparent, 255 = fully opaque).

### Show Alpha Controls (Default)
```xaml
<syncfusion:ColorPicker IsAlphaVisible="True"/>
```
Users can adjust alpha via the alpha slider and A (alpha) input field.

### Hide Alpha Controls
```xaml
<syncfusion:ColorPicker IsAlphaVisible="False"/>
```
Use this when you only need opaque colors. Colors are automatically fully opaque.

### Creating Semi-Transparent Colors in C#
```csharp
// Create a color with 50% opacity
Color semiTransparent = Color.FromArgb(128, 255, 0, 0);  // Semi-transparent red
colorPicker.Color = semiTransparent;

// Create fully opaque color
Color opaque = Color.FromArgb(255, 0, 255, 0);  // Fully opaque green
```

### Retrieving Alpha Value
```csharp
Color selectedColor = colorPicker.Color;
byte alphaValue = selectedColor.A;  // 0-255
double opacity = alphaValue / 255.0;  // 0.0-1.0
MessageBox.Show($"Opacity: {opacity:P}");  // Display as percentage
```

## Switching Between Solid and Gradient Modes

ColorPicker can operate in three modes: Solid, Linear Gradient, and Radial Gradient.

### Enable Mode Switching (Default)
```xaml
<syncfusion:ColorPicker EnableSolidToGradientSwitch="True"/>
```
Users see buttons at the bottom-right to switch between Solid, Linear, and Gradient modes.

### Disable Mode Switching
```xaml
<syncfusion:ColorPicker EnableSolidToGradientSwitch="False"/>
```
Use this to restrict users to the current mode (typically solid color selection).

### Programmatically Switch Modes (in ColorPicker or ColorEdit)
Unfortunately, there's no direct property to set the mode programmatically. Instead:
1. Set the `Color` property to switch to solid mode
2. Set the `Brush` property to a `LinearGradientBrush` for linear mode
3. Set the `Brush` property to a `RadialGradientBrush` for radial mode

```csharp
// Switch to solid mode
colorPicker.Color = Colors.Red;

// Switch to linear gradient mode
LinearGradientBrush linearGrad = new LinearGradientBrush();
linearGrad.GradientStops.Add(new GradientStop(Colors.Red, 0.0));
linearGrad.GradientStops.Add(new GradientStop(Colors.Blue, 1.0));
colorPicker.Brush = linearGrad;

// Switch to radial gradient mode
RadialGradientBrush radialGrad = new RadialGradientBrush();
radialGrad.GradientStops.Add(new GradientStop(Colors.Yellow, 0.0));
radialGrad.GradientStops.Add(new GradientStop(Colors.Purple, 1.0));
colorPicker.Brush = radialGrad;
```

### Common Pattern: Toggle Between Solid and Gradient
```csharp
bool useSolidColor = true;

if (useSolidColor)
{
    colorPicker.Color = Colors.Blue;
}
else
{
    LinearGradientBrush gradient = new LinearGradientBrush();
    gradient.GradientStops.Add(new GradientStop(Colors.Blue, 0.0));
    gradient.GradientStops.Add(new GradientStop(Colors.Cyan, 1.0));
    colorPicker.Brush = gradient;
}
```
