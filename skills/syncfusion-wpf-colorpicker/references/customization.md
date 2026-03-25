# Customization Options

This reference covers customizing the appearance and behavior of the WPF ColorPicker control.

## Table of Contents
- [Color Palette Visibility](#color-palette-visibility)
- [Gradient Brush Display Modes](#gradient-brush-display-modes)
- [Inverted Color Calculation](#inverted-color-calculation)
- [ScRGB Color Support](#scrgb-color-support)
- [Alpha Channel Control](#alpha-channel-control)
- [Restricting Brush Mode Transitions](#restricting-brush-mode-transitions)

## Color Palette Visibility

The built-in color palette provides users with quick access to predefined colors without using the picker region.

### Enable Color Palette (Default)
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="True"/>
```

When enabled, a dropdown button appears that displays a grid of standard colors for quick selection.

```csharp
colorPicker.IsColorPaletteVisible = true;
```

### Disable Color Palette
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="False"/>
```

Use this to force users to use the picker region or manually enter color values, keeping the UI cleaner.

```csharp
colorPicker.IsColorPaletteVisible = false;
```

### Palette Use Cases

**Enable palette when:**
- You want quick color selection for common colors
- Users are less familiar with color pickers
- You want to suggest a standard color set

**Disable palette when:**
- Space is limited
- You need complete control over color values
- Fine-grained color selection is required

## Gradient Brush Display Modes

The gradient brush display mode controls how gradient colors are visually represented in the ColorPicker interface.

### Default Display Mode
```xaml
<syncfusion:ColorPicker GradientBrushDisplayMode="Default"/>
```

```csharp
colorPicker.GradientBrushDisplayMode = Syncfusion.Windows.Tools.GradientBrushDisplayMode.Default;
```

The Default mode shows a compact representation of the gradient.

### Extended Display Mode
```xaml
<syncfusion:ColorPicker GradientBrushDisplayMode="Extended"/>
```

```csharp
colorPicker.GradientBrushDisplayMode = Syncfusion.Windows.Tools.GradientBrushDisplayMode.Extended;
```

The Extended mode shows a more detailed, larger representation of the gradient with additional editing controls.

### Mode Comparison

| Feature | Default | Extended |
|---------|---------|----------|
| Gradient Preview Size | Compact | Large |
| Visual Clarity | Standard | Enhanced |
| UI Space Usage | Minimal | More |
| Best For | Compact layouts | Feature-rich interfaces |

### Choosing a Display Mode

**Use Default when:**
```xaml
<!-- Space-constrained dialogs or panels -->
<syncfusion:ColorPicker GradientBrushDisplayMode="Default" Width="200" Height="150"/>
```

**Use Extended when:**
```xaml
<!-- Dedicated color editing panels -->
<syncfusion:ColorPicker GradientBrushDisplayMode="Extended" Width="400" Height="500"/>
```

## Inverted Color Calculation

The inverted color feature calculates a contrasting color (the inverse) of the selected color. This is useful for automatically determining complementary text colors or accent colors.

### Getting the Inverted Color

Note: The inverted color property is available in the `ColorEdit` control (a related component), not directly in `ColorPicker`. If using `ColorEdit`:

```xaml
<syncfusion:ColorEdit x:Name="colorEdit"   />
<TextBlock Text="Sample Text" 
           Background="{Binding ElementName=colorEdit, Path=Brush, UpdateSourceTrigger=PropertyChanged}"
           Foreground="{Binding ElementName=colorEdit, Path=InvertColor, UpdateSourceTrigger=PropertyChanged}"/>
```

In C#:
```csharp
ColorEdit colorEdit = new ColorEdit();

// Get background brush and inverted foreground color
Brush bgBrush = colorEdit.Brush;
Color fgColor = colorEdit.InvertColor;
```

### Practical Use Case: Auto-Contrasting Text

```xaml
<Grid>
    <syncfusion:ColorEdit x:Name="colorEdit" Width="300" Height="200"/>
    
    <!-- Text that automatically contrasts with selected background -->
    <TextBlock Text="This text contrasts with the background"
               TextAlignment="Center"
               FontSize="16"
               Background="{Binding ElementName=colorEdit, Path=Brush, UpdateSourceTrigger=PropertyChanged}"
               Foreground="{Binding ElementName=colorEdit, Path=InvertColor, UpdateSourceTrigger=PropertyChanged}"
               VerticalAlignment="Center"
               Padding="10"/>
</Grid>
```

### Invert Color Algorithm

The invert color calculation uses the RGB color space to create a contrasting color suitable for text or UI elements that need to stand out against the selected background.

Formula (approximate):
- Inverted R = 255 - Original R
- Inverted G = 255 - Original G
- Inverted B = 255 - Original B

This creates a color with maximum contrast in the opposite direction of the color spectrum.

## ScRGB Color Support

ScRGB (Scene referred RGB) is an extended color space that supports colors outside the standard sRGB gamut, enabling more accurate color representation for advanced graphics applications.

### Enable ScRGB Color Support

Note: ScRGB is available in the `ColorEdit` control.

```xaml
<syncfusion:ColorEdit IsScRGBColor="True"/>
```

```csharp
colorEdit.IsScRGBColor = true;
```

### ScRGB vs Standard RGB

| Aspect | Standard RGB | ScRGB |
|--------|--------------|-------|
| Color Space | sRGB (8-bit per channel) | Extended color space |
| Value Range | 0-255 | 0-255+ (can exceed standard range) |
| Use Case | General purpose UI colors | Professional graphics, color-accurate work |
| Precision | Standard | Higher color fidelity |

### When to Use ScRGB

**Enable ScRGB when:**
- Working with professional graphics or publishing applications
- Color accuracy is critical
- Supporting wide color gamuts (e.g., Adobe RGB)

**Use standard RGB (default) when:**
- Building typical business applications
- Working with standard web colors
- Color precision requirements are standard

### ScRGB Implementation

```csharp
ColorEdit colorEdit = new ColorEdit();
colorEdit.IsScRGBColor = true;

// Now ColorEdit supports extended color values
Color extendedColor = colorEdit.Color;
```

## Alpha Channel Control

The alpha channel controls the transparency of selected colors. You can show or hide alpha controls based on your application's needs.

### Show Alpha Controls (Default)
```xaml
<syncfusion:ColorPicker IsAlphaVisible="True"/>
```

```csharp
colorPicker.IsAlphaVisible = true;
```

When visible, users see:
- An alpha slider (typically horizontal)
- An "A" value input field (0-255)
- An opacity label or indicator

### Hide Alpha Controls
```xaml
<syncfusion:ColorPicker IsAlphaVisible="False"/>
```

```csharp
colorPicker.IsAlphaVisible = false;
```

All selected colors are automatically fully opaque (alpha = 255).

### Alpha Control Use Cases

**Show alpha when:**
```xaml
<!-- Image editing, design tools -->
<syncfusion:ColorPicker IsAlphaVisible="True"/>
```

**Hide alpha when:**
```xaml
<!-- UI theming, solid color selection -->
<syncfusion:ColorPicker IsAlphaVisible="False"/>
```

### Programmatic Alpha Control

```csharp
// Enable/disable alpha based on context
if (isImageEditor)
{
    colorPicker.IsAlphaVisible = true;
}
else
{
    colorPicker.IsAlphaVisible = false;
}

// Retrieve alpha value
if (colorPicker.IsAlphaVisible)
{
    Color selectedColor = colorPicker.Color;
    byte alpha = selectedColor.A;  // 0-255
    double opacity = alpha / 255.0;  // 0.0-1.0
}
```

## Restricting Brush Mode Transitions

By default, users can switch between Solid, Linear Gradient, and Radial Gradient modes using buttons in the ColorPicker interface.

### Allow Mode Switching (Default)
```xaml
<syncfusion:ColorPicker EnableSolidToGradientSwitch="True"/>
```

```csharp
colorPicker.EnableSolidToGradientSwitch = true;
```

Buttons for Solid, Linear, and Gradient modes are visible at the bottom-right of the ColorPicker.

### Disable Mode Switching
```xaml
<syncfusion:ColorPicker EnableSolidToGradientSwitch="False"/>
```

```csharp
colorPicker.EnableSolidToGradientSwitch = false;
```

The mode-switching buttons are hidden. Users can only work with the current mode.

### Mode Transition Use Cases

**Allow transitions when:**
```xaml
<!-- Full-featured color picker -->
<syncfusion:ColorPicker EnableSolidToGradientSwitch="True"/>
```

**Disable transitions when:**
```xaml
<!-- Simple solid color picker -->
<syncfusion:ColorPicker EnableSolidToGradientSwitch="False" Color="Blue"/>

<!-- Gradient-only picker (set Brush to gradient) -->
<syncfusion:ColorPicker EnableSolidToGradientSwitch="False">
    <syncfusion:ColorPicker.Brush>
        <LinearGradientBrush>
            <GradientStop Color="Red" Offset="0.0"/>
            <GradientStop Color="Blue" Offset="1.0"/>
        </LinearGradientBrush>
    </syncfusion:ColorPicker.Brush>
</syncfusion:ColorPicker>
```

### Practical Pattern: Context-Based Restrictions

```csharp
public void ConfigureColorPickerByMode(ColorPicker picker, ColorSelectionMode mode)
{
    switch (mode)
    {
        case ColorSelectionMode.SolidOnly:
            picker.EnableSolidToGradientSwitch = false;
            picker.Color = Colors.Black;
            break;

        case ColorSelectionMode.GradientOnly:
            picker.EnableSolidToGradientSwitch = false;
            picker.Brush = new LinearGradientBrush(Colors.Black, Colors.White, 
                                                    new Point(0, 0), new Point(1, 0));
            break;

        case ColorSelectionMode.Both:
            picker.EnableSolidToGradientSwitch = true;
            break;
    }
}

public enum ColorSelectionMode
{
    SolidOnly,
    GradientOnly,
    Both
}
```

## Combining Customization Options

### Example 1: Simple Solid Color Picker
```xaml
<syncfusion:ColorPicker IsAlphaVisible="False"
                        IsColorPaletteVisible="True"
                        EnableSolidToGradientSwitch="False"
                        Width="250"
                        Height="120"/>
```

### Example 2: Professional Gradient Editor
```xaml
<syncfusion:ColorPicker IsAlphaVisible="True"
                        IsColorPaletteVisible="False"
                        EnableSolidToGradientSwitch="True"
                        GradientBrushDisplayMode="Extended"
                        Width="400"
                        Height="450"/>
```

### Example 3: Compact with Limited Options
```xaml
<syncfusion:ColorPicker IsAlphaVisible="False"
                        IsColorPaletteVisible="True"
                        EnableSolidToGradientSwitch="False"
                        GradientBrushDisplayMode="Default"
                        Width="180"
                        Height="100"/>
```
