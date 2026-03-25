# Swatches Navigation in SfColorPalette

## Table of Contents
- [Overview](#overview)
- [Available Swatches](#available-swatches)
- [Accessing the Swatch Button](#accessing-the-swatch-button)
- [Switching Between Swatches](#switching-between-swatches)
- [Understanding Color Organization](#understanding-color-organization)
- [Practical Examples](#practical-examples)

## Overview

The `SfColorPalette` provides multiple color swatches (predefined color sets). Users can switch between different swatch collections using the Swatch button located at the top-right corner of the control. This allows access to various professionally-organized color palettes.

**Key Concept**: A "swatch" is a collection of related colors. The control ships with several built-in swatches like Material, Office, Metro, and Grayscale.

## Available Swatches

### Default Swatches

The `SfColorPalette` includes these built-in swatches:

| Swatch Name | Purpose | Characteristics |
|------------|---------|-----------------|
| **Material** | Google Material Design colors | Modern, vibrant, organized by hue |
| **Office** | Microsoft Office color palette | Professional, business-oriented |
| **Metro** | Microsoft Metro design colors | Flat, bold, minimalist |
| **Grayscale** | Neutral grays | Monochrome, from white to black |
| **HighContrast** | Accessibility-focused | High contrast ratios for visibility |

### Swatch Organization

Each swatch groups colors by:
- **Hue**: Primary color categories (red, blue, green, etc.)
- **Saturation**: Intensity of color (pale to vibrant)
- **Lightness**: Brightness variations (light to dark tones)

## Accessing the Swatch Button

The Swatch button is automatically visible in the top-right corner of the `SfColorPalette` control:

```
┌─────────────────────────┐
│                    [≡]  │  ← Swatch button (top-right)
│                         │
│  [Color Grid]           │
│  ┌─ ┬─ ┬─ ┬─ ┬─ ┬─┐    │
│  │  │  │  │  │  │  │    │
│  ├─ ┼─ ┼─ ┼─ ┼─ ┼─┤    │
│  │  │  │  │  │  │  │    │
│  └─ ┴─ ┴─ ┴─ ┴─ ┴─┘    │
│                         │
└─────────────────────────┘
```

**No additional code required** – The button is part of the default UI.

## Switching Between Swatches

### User Interaction

1. Click the **Swatch button** (≡ menu icon) in the top-right corner
2. A dropdown list appears showing all available swatches
3. Click any swatch name to switch to that collection
4. The color grid updates to show the new swatch's colors

### Example: User Switches from Material to Office

```
Step 1: Click swatch button
┌─────────────────────────┐
│ Material               │
│                    [≡]◄─┘ Click here
│                         │
│ [Material colors...]    │
└─────────────────────────┘

Step 2: Select from dropdown
┌─ Material
├─ Office ◄─ Select this
├─ Metro
├─ Grayscale
└─ HighContrast

Step 3: Office swatch displayed
┌─────────────────────────┐
│ Office                 │
│                    [≡]  │
│                         │
│ [Office colors...]      │
└─────────────────────────┘
```

## Understanding Color Organization

### Material Swatch Structure

The Material swatch follows Google's Material Design color system:

```
Primary Colors:
Red, Pink, Purple, Deep Purple, Indigo, Blue, Light Blue,
Cyan, Teal, Green, Light Green, Lime, Yellow, Amber, Orange, Deep Orange, Brown, Gray, Blue Gray

Variations per Color:
- 50 (lightest)
- 100, 200, 300, 400
- 500 (primary)
- 600, 700, 800
- 900 (darkest)
```

**Result**: ~200+ color options to choose from.

### Office Swatch Structure

The Office swatch provides:

```
Standard Colors (10 colors):
Red, Orange, Yellow, Green, Teal, Blue, Purple, Pink, Gray, Black

Variations:
- Base color
- Light variations (lighter tones)
- Dark variations (darker tones)
```

**Result**: ~30-40 professional colors.

## XAML Implementation

### Basic Setup with Multiple Swatches

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="Swatch Navigation Demo" Height="500" Width="400">

    <Grid>
        <StackPanel Margin="20">
            <TextBlock 
                Text="Color Palette with Swatch Selector" 
                FontSize="16" 
                FontWeight="Bold" 
                Margin="0,0,0,15"/>
            
            <TextBlock 
                Text="Click the button (≡) to switch between color swatches:"
                FontSize="12"
                Margin="0,0,0,10"/>
            
            <!-- Color Palette with Swatch Button -->
            <syncfusion:SfColorPalette 
                x:Name="colorPalette"
                Height="350"
                Width="350"
                HorizontalAlignment="Left"/>
            
            <TextBlock 
                Text="Available swatches: Material, Office, Metro, Grayscale, HighContrast"
                FontSize="10"
                Foreground="Gray"
                Margin="0,15,0,0"/>
        </StackPanel>
    </Grid>
</Window>
```

## C# Code-Behind Examples

### Checking Current Swatch (Indirect)

The current swatch cannot be directly queried, but you can track it:

```csharp
public partial class MainWindow : Window
{
    private string currentSwatchName = "Material"; // Default

    public MainWindow()
    {
        InitializeComponent();
        
        // Monitor color changes to infer swatch context
        // (Colors from different swatches have different values)
    }

    private void OnColorSelected(object sender, EventArgs e)
    {
        Color selectedColor = colorPalette.SelectedColor;
        Console.WriteLine($"Selected from {currentSwatchName}: {selectedColor}");
    }
}
```

### Getting Colors Programmatically

Access specific colors from different swatches:

```csharp
using System.Windows.Media;

public class SwatchHelper
{
    // Material Swatch Examples
    public static readonly Color MaterialRed500 = Color.FromRgb(244, 67, 54);
    public static readonly Color MaterialBlue500 = Color.FromRgb(33, 150, 243);
    public static readonly Color MaterialGreen500 = Color.FromRgb(76, 175, 80);

    // Office Swatch Examples
    public static readonly Color OfficeRed = Color.FromRgb(192, 0, 0);
    public static readonly Color OfficeBlue = Color.FromRgb(0, 0, 192);
    public static readonly Color OfficeGreen = Color.FromRgb(0, 176, 80);

    public static Color GetSwatchColor(string swatch, string colorName)
    {
        return swatch switch
        {
            "Material" => GetMaterialColor(colorName),
            "Office" => GetOfficeColor(colorName),
            _ => Colors.White
        };
    }

    private static Color GetMaterialColor(string name)
    {
        // Implementation for Material colors
        return Colors.Blue;
    }

    private static Color GetOfficeColor(string name)
    {
        // Implementation for Office colors
        return Colors.Red;
    }
}
```

## Practical Examples

### Example 1: Using Material Swatch for UI Design

```xaml
<StackPanel Margin="20">
    <TextBlock Text="Material Design Colors" FontSize="14" FontWeight="Bold" Margin="0,0,0,15"/>
    
    <syncfusion:SfColorPalette 
        x:Name="designPalette"
        Height="300"
        Width="350"
        Margin="0,0,0,20"/>
    
    <TextBlock Text="Recommended: Switch to Material swatch for modern, vibrant colors" Foreground="Green"/>
</StackPanel>
```

**Result**: User can browse Material's organized color system for consistent modern design.

### Example 2: Using Office Swatch for Business Applications

```xaml
<StackPanel Margin="20">
    <TextBlock Text="Business Report Color Selector" FontSize="14" FontWeight="Bold" Margin="0,0,0,15"/>
    
    <syncfusion:SfColorPalette 
        x:Name="businessPalette"
        Height="300"
        Width="350"
        Margin="0,0,0,20"/>
    
    <TextBlock Text="Tip: Use Office swatch for professional, muted color choices" Foreground="Blue"/>
</StackPanel>
```

**Result**: User accesses professional colors suitable for charts, reports, and business documents.

### Example 3: Accessibility with HighContrast Swatch

```xaml
<StackPanel Margin="20">
    <TextBlock Text="Accessible Color Selection" FontSize="14" FontWeight="Bold" Margin="0,0,0,15"/>
    
    <CheckBox Content="Use High Contrast Colors" Margin="0,0,0,10"/>
    
    <syncfusion:SfColorPalette 
        x:Name="accessibilityPalette"
        Height="300"
        Width="350"
        Margin="0,0,0,20"/>
    
    <TextBlock Text="Select HighContrast swatch for better visibility" Foreground="Green"/>
</StackPanel>
```

**Result**: Users with visual impairments can select high-contrast colors for readability.

## Common Workflows

### Workflow: Color Theme Selection

```
1. User opens application
2. Sees default Material swatch
3. Clicks swatch button (≡)
4. Sees dropdown: Material, Office, Metro, Grayscale, HighContrast
5. Selects "Office" swatch
6. Color palette updates with Office colors
7. User selects desired color from Office palette
8. Selected color applied to theme
```

### Workflow: Accessibility Compliance

```
1. Application needs accessible colors
2. User opens Color Palette
3. Clicks swatch button to access HighContrast
4. Selects high-contrast color pair
5. Contrast ratio guaranteed ≥ 7:1
6. Applied to UI for WCAG AAA compliance
```

## Edge Cases

**Limited Color Needs**: If you only need a few colors, Office swatch might be preferable (fewer options, faster selection).

**Design System Compliance**: If your app follows Material Design, default to Material swatch. If using Office/Windows styles, use Office swatch.

**User Preferences**: Let users explore all swatches to find colors they prefer, rather than restricting to one swatch.

## Related Topics

- Customizing appearance: See [appearance-theming.md](appearance-theming.md)
- Color binding to UI elements: See [color-binding.md](color-binding.md)
- Basic setup: See [getting-started.md](getting-started.md)
