# Getting Started with SfColorPalette

## Assembly References

To use the `SfColorPalette` control, add the following assemblies as references to your WPF project:

- **Syncfusion.SfColorPalette.Wpf**
- **Syncfusion.SfShared.Wpf**

### Via NuGet Package Manager

Install the Syncfusion WPF NuGet package:

```
Install-Package Syncfusion.SfColorPalette.WPF
```

This automatically adds all required dependencies.

## Adding Control via Designer

The easiest way to add the Color Palette control:

1. Open your WPF window in Visual Studio Designer
2. Open the **Toolbox** (View > Toolbox)
3. Search for **SfColorPalette**
4. Drag and drop the control onto your window
5. Visual Studio automatically adds required assembly references

**Result:** The control appears with default styling and size.

## Adding Control Manually in XAML

### Step 1: Add Namespace Declaration

Import the Syncfusion namespace in your XAML file:

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

**Full Window Example:**

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="Color Palette Demo" Height="400" Width="500">
    <Grid>
        <!-- Control will go here -->
    </Grid>
</Window>
```

### Step 2: Declare the Control

Add the `SfColorPalette` control inside your layout container:

```xaml
<Grid>
    <syncfusion:SfColorPalette 
        HorizontalAlignment="Left" 
        Margin="20,20,0,0" 
        VerticalAlignment="Top" 
        Height="300" 
        Width="250"/>
</Grid>
```

**Result:** A color palette with default Material swatch appears in the upper-left corner.

## Adding Control Manually in C#

### Step 1: Import Namespace

```csharp
using Syncfusion.Windows.Controls.Media;
```

### Step 2: Create and Configure Control

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        // Create the Color Palette control
        SfColorPalette colorPalette = new SfColorPalette();
        colorPalette.Height = 300;
        colorPalette.Width = 250;
        colorPalette.HorizontalAlignment = HorizontalAlignment.Left;
        colorPalette.VerticalAlignment = VerticalAlignment.Top;
        colorPalette.Margin = new Thickness(20);

        // Add to window
        this.Content = colorPalette;
    }
}
```

## Selecting Colors

Users can select colors by clicking on any color item in the palette. The control provides visual feedback:

- **Hover Tooltip**: Shows a preview of the hovered color
- **Click Selection**: Clicking a color updates the `SelectedColor` property

### Retrieving the Selected Color

**In XAML (via binding):**

```xaml
<TextBlock Text="{Binding ElementName=colorPalette, Path=SelectedColor}"/>
<syncfusion:SfColorPalette x:Name="colorPalette"/>
```

**In C#:**

```csharp
SfColorPalette colorPalette = new SfColorPalette();
Color selectedColor = colorPalette.SelectedColor;
Console.WriteLine($"Selected color: {selectedColor}");
```

## Handling Selection Events

React to color selection with event handlers:

```csharp
public partial class MainWindow : Window
{
    private Rectangle previewRectangle;

    public MainWindow()
    {
        InitializeComponent();

        SfColorPalette colorPalette = new SfColorPalette();
        
        // Create a rectangle to show selected color
        previewRectangle = new Rectangle();
        previewRectangle.Height = 50;
        previewRectangle.Width = 100;
        
        // Subscribe to changes (via binding with INotifyPropertyChanged)
        // Or use a converter for automatic updates
        
        StackPanel stack = new StackPanel();
        stack.Children.Add(colorPalette);
        stack.Children.Add(previewRectangle);
        
        this.Content = stack;
    }
}
```

## Complete Basic Example

**XAML:**

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="Color Palette - Getting Started" Height="500" Width="400">
    <StackPanel Margin="20" Spacing="15">
        <TextBlock Text="Color Palette Demo" FontSize="18" FontWeight="Bold"/>
        
        <TextBlock Text="Select a Color:" FontSize="14"/>
        <syncfusion:SfColorPalette 
            x:Name="colorPalette"
            Height="250"
            Width="300"
            HorizontalAlignment="Left"/>
        
        <TextBlock Text="Selected Color Value:" FontSize="12"/>
        <TextBlock 
            Text="{Binding ElementName=colorPalette, Path=SelectedColor}"
            FontFamily="Courier New"
            Background="LightGray"
            Padding="5"/>
    </StackPanel>
</Window>
```

**C# Code-Behind:**

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Media;

namespace ColorPaletteDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

## Default Behavior

When you create an `SfColorPalette` with no additional configuration:

- **Default Swatch**: Material color palette
- **Default Size**: Automatically sized to container
- **Default Selection**: First color in the palette (typically transparent or first Material color)
- **Foreground**: Gray
- **Background**: Snow (off-white)
- **Flow Direction**: LeftToRight

## Next Steps

- **Bind colors to UI elements**: Read [color-binding.md](color-binding.md)
- **Navigate between swatches**: Read [swatches-navigation.md](swatches-navigation.md)
- **Customize appearance**: Read [appearance-theming.md](appearance-theming.md)
