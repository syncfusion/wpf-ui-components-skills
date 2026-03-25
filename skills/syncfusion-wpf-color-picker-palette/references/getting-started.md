# Getting Started with ColorPickerPalette

## Table of Contents
- [Installation & Assembly References](#installation--assembly-references)
- [Adding via Designer](#adding-via-designer)
- [Adding via XAML](#adding-via-xaml)
- [Adding via C#](#adding-via-c)
- [Control Structure](#control-structure)
- [Accessing Colors Programmatically](#accessing-colors-programmatically)
- [Applying Themes](#applying-themes)
- [Localization Support](#localization-support)

## Installation & Assembly References

### Required Assemblies
To use the ColorPickerPalette control, add the following assembly reference to your WPF project:

**Assembly:**
- `Syncfusion.Shared.WPF`

### NuGet Package Installation
If you're using NuGet Package Manager, install the Syncfusion WPF package:
```
Install-Package Syncfusion.Shared.WPF
```

### Creating a New WPF Project
1. Create a new WPF project in Visual Studio
2. Add the `Syncfusion.Shared.WPF` assembly reference
3. Import the Syncfusion WPF schema in your XAML

## Adding via Designer

The easiest way to add ColorPickerPalette is through the Visual Studio designer:

1. Open your WPF Window in the designer
2. Locate the toolbox and find the **Syncfusion Controls** section
3. Drag **ColorPickerPalette** onto your window
4. The `Syncfusion.Shared.WPF` assembly will be automatically added

The designer creates a basic control:
```xaml
<syncfusion:ColorPickerPalette x:Name="colorPickerPalette" 
                               Width="60" 
                               Height="40" />
```

## Adding via XAML

To manually add ColorPickerPalette in XAML:

**Step 1: Add the Syncfusion namespace**
```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
</Window>
```

**Step 2: Declare the control**
```xaml
<Grid>
    <syncfusion:ColorPickerPalette x:Name="colorPickerPalette" 
                                   Width="60" 
                                   Height="40" />
</Grid>
```

**Complete XAML Example:**
```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="ColorPickerPaletteSample.MainWindow"
        Title="ColorPickerPalette Sample" 
        Height="350" 
        Width="525">
    <Grid>
        <!-- ColorPickerPalette control -->
        <StackPanel Margin="20">
            <TextBlock Text="Select a Color:" FontSize="14" Margin="0,0,0,10"/>
            <syncfusion:ColorPickerPalette x:Name="colorPickerPalette" 
                                           Width="60" 
                                           Height="40" />
        </StackPanel>
    </Grid>
</Window>
```

## Adding via C#

To create ColorPickerPalette programmatically in code-behind:

**Step 1: Add using statement**
```csharp
using Syncfusion.Windows.Tools.Controls;
```

**Step 2: Create and add to window**
```csharp
public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();

        // Creating an instance of ColorPickerPalette control
        ColorPickerPalette colorPickerPalette = new ColorPickerPalette();
        colorPickerPalette.Width = 60;
        colorPickerPalette.Height = 40;

        // Adding ColorPickerPalette as window content
        this.Content = colorPickerPalette;
    }
}
```

**With StackPanel:**
```csharp
public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();

        var stackPanel = new StackPanel { Margin = new Thickness(20) };

        var label = new TextBlock 
        { 
            Text = "Select a Color:",
            FontSize = 14,
            Margin = new Thickness(0, 0, 0, 10)
        };

        var colorPickerPalette = new ColorPickerPalette 
        { 
            Width = 60,
            Height = 40
        };

        stackPanel.Children.Add(label);
        stackPanel.Children.Add(colorPickerPalette);

        this.Content = stackPanel;
    }
}
```

## Control Structure

The ColorPickerPalette consists of these key visual components:

- **Selected Color**: Displays the currently selected color at the top
- **Dropdown Button**: Opens the color palette popup (in Dropdown/Split mode)
- **Automatic Color**: Shows the default/reset color (clicking resets to this)
- **Theme Colors Panel**: Displays theme-based color variants
- **Standard Colors Panel**: Shows predefined standard colors
- **Custom Colors Panel**: User-defined or application-provided colors
- **Recently Used Panel**: Shows previously selected colors
- **More Colors Button**: Opens extended color selection window
- **No Color Button**: Resets selection to transparent

## Accessing Colors Programmatically

### Setting Initial Color
```xaml
<!-- XAML -->
<syncfusion:ColorPickerPalette Color="Red"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
// C#
ColorPickerPalette colorPickerPalette = new ColorPickerPalette();
colorPickerPalette.Color = Colors.Red;
colorPickerPalette.Width = 60;
colorPickerPalette.Height = 40;
```

### Getting the Selected Color
```csharp
Color selectedColor = colorPickerPalette.Color;
string colorName = colorPickerPalette.ColorName;
```

### Setting Brush Instead of Color
```xaml
<!-- XAML -->
<syncfusion:ColorPickerPalette SelectedBrush="Yellow"
                               Name="colorPickerPalette"/>
```

```csharp
// C#
colorPickerPalette.SelectedBrush = Brushes.Yellow;
```

### Setting Automatic (Default) Color
The automatic color is the default color users can reset to by clicking the automatic color button:

```xaml
<syncfusion:ColorPickerPalette AutomaticColor="Green"
                               AutomaticColorVisibility="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.AutomaticColor = Colors.Green;
colorPickerPalette.AutomaticColorVisibility = Visibility.Visible;
```

### Setting Transparent Color
```xaml
<syncfusion:ColorPickerPalette Color="Transparent"
                               Name="colorPickerPalette"/>
```

```csharp
colorPickerPalette.Color = Colors.Transparent;
```

## Applying Themes

ColorPickerPalette supports various built-in themes using the SfSkinManager:

### Apply Theme via SfSkinManager
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:ColorPickerPalette x:Name="colorPickerPalette" />
    </Grid>
</Window>
```

```csharp
// Apply theme programmatically
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        // Apply a specific theme
        SfSkinManager.SetTheme(this, new FluentDarkTheme());
    }
}
```

### Available Themes
- Fluent Light / Fluent Dark
- Material Light / Material Dark
- Office2019 Colorful
- High Contrast

## Localization Support

To provide localized UI for ColorPickerPalette, create resource files for each language:

**Step 1: Create resource file**
- Add a resx file for each language in your project
- Example: `ColorPickerResources.en-US.resx`, `ColorPickerResources.de-DE.resx`

**Step 2: Add translations**
Add these key-value pairs to your resource files:
- `AutomaticColorHeader` = "Automatic Color"
- `StandardColorsHeader` = "Standard Colors"
- `ThemeColorsHeader` = "Theme Colors"
- `CustomColorsHeader` = "Custom Colors"
- `RecentlyUsedHeader` = "Recently Used"
- `MoreColorsButtonText` = "More Colors"
- `NoColorButtonText` = "No Color"

**Step 3: Apply culture**
```csharp
// Set culture before creating controls
CultureInfo.CurrentUICulture = new CultureInfo("de-DE");
```

## Common Patterns

**Pattern: Basic Color Selection**
```xaml
<syncfusion:ColorPickerPalette x:Name="colorPickerPalette"
                               GenerateThemeVariants="True"
                               GenerateStandardVariants="True"
                               Width="60"
                               Height="40" />
```

**Pattern: Minimal Palette (Theme Only)**
```xaml
<syncfusion:ColorPickerPalette x:Name="colorPickerPalette"
                               ThemePanelVisibility="Visible"
                               StandardPanelVisibility="Collapsed"
                               MoreColorOptionVisibility="Collapsed"
                               Width="60"
                               Height="40" />
```

**Pattern: Full Features Palette**
```xaml
<syncfusion:ColorPickerPalette x:Name="colorPickerPalette"
                               Color="Blue"
                               GenerateThemeVariants="True"
                               GenerateStandardVariants="True"
                               RecentlyUsedPanelVisibility="Visible"
                               MoreColorOptionVisibility="Visible"
                               NoColorVisibility="Visible"
                               Width="60"
                               Height="40" />
```

## Troubleshooting

**Assembly Not Found**
- Verify that `Syncfusion.Shared.WPF` is installed
- Check project references for the assembly
- Ensure Syncfusion NuGet is up to date

**Control Not Appearing**
- Confirm XAML namespace: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`
- Verify Width and Height are set
- Check that parent container allows child elements

**Color Not Changing**
- Ensure `Color` property is being set before control loads
- Verify `ColorName` property reflects the color you set
- Check for binding conflicts with data context
