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

Set `StartPoint` and `EndPoint` properties to control gradient direction:

| Direction | StartPoint | EndPoint | Use Case |
|-----------|-----------|----------|----------|
| Left to Right | (0, 0) | (1, 0) | Horizontal fade |
| Top to Bottom | (0, 0) | (0, 1) | Vertical fade |
| Diagonal (↘) | (0, 0) | (1, 1) | Corner-to-corner |
| Diagonal (↙) | (1, 0) | (0, 1) | Reverse diagonal |

Add multiple `GradientStop` entries to the `GradientStops` collection for smooth color transitions. Use offset values from 0.0 (start) to 1.0 (end) to position each color stop.

## Working with Radial Gradients

Radial gradients blend colors outward from a center point in a circular pattern. Use `RadialGradientBrush` with the `Brush` property.

### Creating a Radial Gradient

In XAML, use `RadialGradientBrush` with `GradientOrigin`, `Center`, `RadiusX`, and `RadiusY`. In C#, create a `RadialGradientBrush` object, set properties, populate `GradientStops` collection, and assign to `colorPicker.Brush`.

### Understanding Radial Gradient Properties

- **GradientOrigin** - Center point where gradient originates (normalized 0.0-1.0)
- **Center** - Center of the circular gradient
- **RadiusX** - Horizontal radius (0.5 = 50% of width)
- **RadiusY** - Vertical radius (0.5 = 50% of height)
- **GradientStops** - Collection of colors and their positions (same as linear)

### Oval vs Circular Radial Gradients

Set `RadiusX` and `RadiusY` equal for circular gradients (e.g., 0.5, 0.5). Set unequal for oval gradients (e.g., 0.7, 0.3).

### Offset Gradient Origin

Set `GradientOrigin` and `Center` properties to control where gradient starts and ends. Equal values center the gradient; offset values create directional effects.

## Runtime Color Editing

ColorPicker enables interactive editing through picker region (hue/saturation), hue slider, and value editors (RGB/HSV/Hex input). Runtime editing is enabled by default. Subscribe to `ColorChanged` event to track changes: `colorPicker.ColorChanged += (d, e) => { ... }`. Update color programmatically: `colorPicker.Color = Colors.Red;` (triggers event).

## Managing Opacity (Alpha Channel)

The alpha channel (A value) controls transparency: 0 = fully transparent, 255 = fully opaque. Show controls with `IsAlphaVisible="True"` (default). Hide with `IsAlphaVisible="False"` for opaque-only colors. Create semi-transparent colors: `Color.FromArgb(128, 255, 0, 0)` for 50% red. Retrieve alpha: `byte alpha = selectedColor.A; double opacity = alpha / 255.0;`

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

### Programmatically Switch Modes
Set `Color` property to switch to solid mode: `colorPicker.Color = Colors.Red;`. Set `Brush` property to switch to gradient mode: assign `LinearGradientBrush` or `RadialGradientBrush` to `colorPicker.Brush`.
