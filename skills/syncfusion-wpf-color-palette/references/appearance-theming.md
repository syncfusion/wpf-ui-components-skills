# Appearance and Theming in SfColorPalette

## Table of Contents
- [Overview](#overview)
- [Setting Foreground Color](#setting-foreground-color)
- [Setting Background Color](#setting-background-color)
- [Flow Direction and RTL Support](#flow-direction-and-rtl-support)
- [Applying Built-in Themes](#applying-built-in-themes)
- [Theme Studio Customization](#theme-studio-customization)
- [Complete Styling Examples](#complete-styling-examples)

## Overview

The `SfColorPalette` control supports customization of its appearance through:

1. **Foreground/Background Properties**: Change UI element colors
2. **Flow Direction**: Support right-to-left (RTL) languages
3. **Built-in Themes**: Apply predefined professional themes
4. **Theme Studio**: Create custom theme variations

These options allow you to match the Color Palette to your application's design system and cultural requirements.

## Setting Foreground Color

The `Foreground` property controls the color of UI elements within the palette (text, icons, outlines).

**Default Value**: Gray

### XAML Implementation

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="Custom Foreground Example" Height="400" Width="500">

    <Grid Margin="20">
        <StackPanel>
            <TextBlock Text="Color Palette - Red Foreground" FontSize="14" FontWeight="Bold" Margin="0,0,0,15"/>
            
            <!-- Red foreground for control UI -->
            <syncfusion:SfColorPalette 
                x:Name="colorPalette1"
                Foreground="Red"
                Height="250"
                Width="300"
                HorizontalAlignment="Left"
                Margin="0,0,0,20"/>
            
            <TextBlock Text="Color Palette - Blue Foreground" FontSize="14" FontWeight="Bold" Margin="0,0,0,15"/>
            
            <!-- Blue foreground for control UI -->
            <syncfusion:SfColorPalette 
                x:Name="colorPalette2"
                Foreground="Blue"
                Height="250"
                Width="300"
                HorizontalAlignment="Left"/>
        </StackPanel>
    </Grid>
</Window>
```

### C# Implementation

```csharp
using System.Windows.Media;
using Syncfusion.Windows.Controls.Media;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        SfColorPalette colorPalette = new SfColorPalette();
        colorPalette.Height = 300;
        colorPalette.Width = 250;
        colorPalette.Foreground = Brushes.Red;  // Set red foreground

        this.Content = colorPalette;
    }
}
```

**Result**: Labels, icons, and outlines appear in red instead of the default gray.

## Setting Background Color

The `Background` property controls the overall background color of the palette.

**Default Value**: Snow (off-white, #FFFAFA)

### XAML Implementation

```xaml
<StackPanel Margin="20">
    <TextBlock Text="Default Background (Snow)" FontSize="12" FontWeight="Bold" Margin="0,0,0,10"/>
    <syncfusion:SfColorPalette 
        Height="250"
        Width="300"
        HorizontalAlignment="Left"
        Margin="0,0,0,25"/>
    
    <TextBlock Text="Custom Background (LightBlue)" FontSize="12" FontWeight="Bold" Margin="0,0,0,10"/>
    <syncfusion:SfColorPalette 
        Background="LightBlue"
        Height="250"
        Width="300"
        HorizontalAlignment="Left"
        Margin="0,0,0,25"/>
    
    <TextBlock Text="Custom Background (Transparent with Olive)" FontSize="12" FontWeight="Bold" Margin="0,0,0,10"/>
    <syncfusion:SfColorPalette 
        Background="Olive"
        Height="250"
        Width="300"
        HorizontalAlignment="Left"/>
</StackPanel>
```

### C# Implementation

```csharp
SfColorPalette colorPalette = new SfColorPalette();
colorPalette.Height = 300;
colorPalette.Width = 250;
colorPalette.Background = Brushes.LightBlue;  // Set light blue background

this.Content = colorPalette;
```

**Result**: The entire palette container has a custom background color.

### Common Background Choices

| Color | Use Case | Example |
|-------|----------|---------|
| Snow (default) | Professional, neutral | Business applications |
| LightGray | Subtle background | Palette on light UI |
| LightBlue | Cool tone | Data applications |
| Beige | Warm tone | Creative tools |
| White | High contrast | Standalone components |
| Transparent | Blends with parent | Embedded in forms |

## Flow Direction and RTL Support

The `FlowDirection` property controls layout direction for right-to-left (RTL) language support.

**Default Value**: LeftToRight

### XAML Implementation

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="RTL Support Example" Height="500" Width="400">

    <StackPanel Margin="20">
        <TextBlock Text="Left-to-Right (LTR)" FontSize="12" FontWeight="Bold" Margin="0,0,0,10"/>
        <syncfusion:SfColorPalette 
            x:Name="ltrPalette"
            FlowDirection="LeftToRight"
            Height="200"
            Width="300"
            HorizontalAlignment="Left"
            Margin="0,0,0,25"/>
        
        <TextBlock Text="Right-to-Left (RTL)" FontSize="12" FontWeight="Bold" Margin="0,0,0,10"/>
        <syncfusion:SfColorPalette 
            x:Name="rtlPalette"
            FlowDirection="RightToLeft"
            Height="200"
            Width="300"
            HorizontalAlignment="Right"
            Margin="0,0,0,25"/>
    </StackPanel>
</Window>
```

### C# Implementation

```csharp
// Left-to-Right (default)
SfColorPalette ltrPalette = new SfColorPalette();
ltrPalette.FlowDirection = FlowDirection.LeftToRight;

// Right-to-Left (for Arabic, Hebrew, etc.)
SfColorPalette rtlPalette = new SfColorPalette();
rtlPalette.FlowDirection = FlowDirection.RightToLeft;

this.Content = rtlPalette;
```

### RTL Layout Example

```xaml
<!-- Window-level RTL setting -->
<Window FlowDirection="RightToLeft">
    <Grid>
        <!-- All children inherit RTL -->
        <syncfusion:SfColorPalette 
            Height="250"
            Width="300"/>
    </Grid>
</Window>
```

**Result**: 
- LTR: Swatch button on right, labels on left
- RTL: Swatch button on left, labels on right

## Applying Built-in Themes

Syncfusion provides multiple built-in themes. Apply themes using the `SfSkinManager` class.

### Available Themes

- **Office2016Colorful**: Modern Office colors
- **Office2016White**: Office white theme
- **Office2016Black**: Office dark theme
- **Material**: Google Material Design
- **MaterialDark**: Dark Material variant
- **Lime**: Green-based theme
- **Saffron**: Orange-based theme
- **BlueAddiction**: Deep blue theme

### Applying Themes in XAML

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
    syncfusionskin:SfSkinManager.VisualStyle="Office2016Blue"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="Theme Application Example" Height="500" Width="400">

    <Grid Margin="20">
        <StackPanel>
            <TextBlock Text="Color Palette with Office2016Blue Theme" FontSize="14" FontWeight="Bold" Margin="0,0,0,15"/>
            
            <!-- Inherits Office2016Blue theme from window -->
            <syncfusion:SfColorPalette 
                Height="300"
                Width="300"
                HorizontalAlignment="Left"/>
        </StackPanel>
    </Grid>
</Window>
```

### Applying Themes in C#

```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        // Apply Office2016Colorful theme to the window
        SfSkinManager.SetVisualStyle(this, VisualStyles.Office2016Colorful);

        SfColorPalette colorPalette = new SfColorPalette();
        colorPalette.Height = 300;
        colorPalette.Width = 250;

        // Color palette inherits theme from parent
        this.Content = colorPalette;
    }
}
```

### Per-Control Theme Application

```xaml
<Window
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">

    <StackPanel Margin="20">
        <!-- Material theme -->
        <syncfusion:SfColorPalette 
            syncfusionskin:SfSkinManager.VisualStyle="Material"
            Height="250"
            Width="300"
            Margin="0,0,0,25"/>
        
        <!-- Office2016 theme -->
        <syncfusion:SfColorPalette 
            syncfusionskin:SfSkinManager.VisualStyle="Office2016Colorful"
            Height="250"
            Width="300"/>
    </StackPanel>
</Window>
```

## Theme Studio Customization

Create custom theme variations using Theme Studio.

### Steps to Create Custom Theme

1. **Open Theme Studio**: Launch from Syncfusion installation or web version
2. **Select Base Theme**: Choose Office2016Colorful, Material, etc.
3. **Customize Colors**: Modify primary, secondary, and accent colors
4. **Preview Changes**: See Color Palette with new theme colors
5. **Export Theme**: Save as XAML or JSON
6. **Apply in Project**: Import and apply custom theme

### Applying Custom Theme

**Custom XAML Theme (CustomTheme.xaml):**

```xaml
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation">
    <!-- Custom theme colors -->
    <SolidColorBrush x:Key="PrimaryBrush" Color="#FF2196F3"/>
    <SolidColorBrush x:Key="AccentBrush" Color="#FFFF9800"/>
    <SolidColorBrush x:Key="BackgroundBrush" Color="#FFF5F5F5"/>
    <!-- Additional theme definitions -->
</ResourceDictionary>
```

**Apply in MainWindow.xaml:**

```xaml
<Window>
    <Window.Resources>
        <ResourceDictionary Source="CustomTheme.xaml"/>
    </Window.Resources>

    <Grid>
        <syncfusion:SfColorPalette Height="300" Width="250"/>
    </Grid>
</Window>
```

## Complete Styling Examples

### Example 1: Professional Business Palette

```xaml
<syncfusion:SfColorPalette 
    x:Name="businessPalette"
    Background="White"
    Foreground="#333333"
    Height="300"
    Width="300"
    Margin="20"/>
```

**Result**: Clean, professional appearance suitable for business applications.

### Example 2: Dark Mode Palette

```xaml
<syncfusion:SfColorPalette 
    x:Name="darkPalette"
    Background="#1E1E1E"
    Foreground="White"
    Height="300"
    Width="300"
    Margin="20"/>
```

**Result**: Dark background with light text for eye comfort in low-light environments.

### Example 3: Accessibility-Focused Palette

```xaml
<syncfusion:SfColorPalette 
    x:Name="accessiblePalette"
    Background="White"
    Foreground="#000000"
    Height="300"
    Width="300"
    Margin="20"/>
```

**Result**: Maximum contrast (black on white) for visually impaired users.

### Example 4: RTL-Enabled Palette with Theme

```xaml
<Window
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
    FlowDirection="RightToLeft"
    syncfusionskin:SfSkinManager.VisualStyle="Office2016Colorful">
    
    <Grid Margin="20">
        <syncfusion:SfColorPalette 
            Height="300"
            Width="300"
            HorizontalAlignment="Right"/>
    </Grid>
</Window>
```

**Result**: Palette with RTL layout and Office2016 theme applied.

### Example 5: Material Design Palette

```xaml
<Window
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
    syncfusionskin:SfSkinManager.VisualStyle="Material">
    
    <StackPanel Margin="20">
        <TextBlock Text="Material Design Color Palette" FontSize="16" FontWeight="Bold" Margin="0,0,0,15"/>
        <syncfusion:SfColorPalette 
            Height="300"
            Width="300"
            HorizontalAlignment="Left"/>
    </StackPanel>
</Window>
```

**Result**: Palette with Material Design theme colors and styling.

## Theme Comparison Table

| Theme | Best For | Appearance |
|-------|----------|------------|
| Office2016Colorful | Business apps | Modern, colorful |
| Material | Modern apps | Flat, vibrant |
| Lime | Green accents | Nature-inspired |
| Saffron | Orange accents | Warm tone |
| BlueAddiction | Professional | Deep blue |
| Office2016Black | Dark mode | Dark background |

## Related Topics

- Basic setup: See [getting-started.md](getting-started.md)
- Color binding: See [color-binding.md](color-binding.md)
- Swatch navigation: See [swatches-navigation.md](swatches-navigation.md)
