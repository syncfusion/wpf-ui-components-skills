# Binding Selected Colors in SfColorPalette

## Overview

The `SelectedColor` property of `SfColorPalette` returns a `System.Windows.Media.Color` object. To use this in data binding with UI elements like `Rectangle.Fill` or `TextBlock.Foreground`, you need a value converter to convert `Color` to `Brush`.

## Creating a Color-to-Brush Converter

The `SfColorPalette` returns `Color` objects, but most WPF controls expect `Brush` objects. Create a converter to bridge this gap:

### IValueConverter Implementation

```csharp
using System;
using System.Globalization;
using System.Windows.Data;
using System.Windows.Media;

public class ColorToSolidColorBrushValueConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        // Convert Color to SolidColorBrush
        if (value is Color color)
        {
            return new SolidColorBrush(color);
        }

        return null;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        // Convert Brush back to Color (optional)
        if (value is SolidColorBrush brush)
        {
            return brush.Color;
        }

        return null;
    }
}
```

## Basic Color Binding to Rectangle

### XAML Setup

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:local="clr-namespace:ColorPaletteDemo"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="Color Binding Example" Height="400" Width="500">

    <Grid>
        <Grid.Resources>
            <!-- Register the converter -->
            <local:ColorToSolidColorBrushValueConverter x:Key="ColorToBrushConverter"/>
        </Grid.Resources>

        <StackPanel Margin="20" VerticalAlignment="Top">
            <TextBlock Text="Select a Color:" FontSize="14" FontWeight="Bold" Margin="0,0,0,10"/>
            
            <!-- Color Palette Control -->
            <syncfusion:SfColorPalette 
                x:Name="colorPalette"
                Height="250"
                Width="300"
                HorizontalAlignment="Left"
                Margin="0,0,0,20"/>
            
            <TextBlock Text="Color Preview:" FontSize="14" FontWeight="Bold" Margin="0,0,0,10"/>
            
            <!-- Rectangle that changes color based on selection -->
            <Rectangle 
                Height="60"
                Width="300"
                HorizontalAlignment="Left"
                Stroke="Black"
                StrokeThickness="2"
                Fill="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}}"/>
        </StackPanel>
    </Grid>
</Window>
```

## Binding Multiple UI Elements

Use the same converter to bind the selected color to multiple controls:

```xaml
<StackPanel Margin="20">
    <!-- Color Palette -->
    <syncfusion:SfColorPalette 
        x:Name="colorPalette"
        Height="200"
        Width="250"
        Margin="0,0,0,20"/>
    
    <!-- Multiple Elements Using Same Binding -->
    <Rectangle 
        Height="50"
        Width="200"
        Fill="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}}"
        Margin="0,0,0,10"/>
    
    <Button 
        Content="Click Me"
        Background="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}}"
        Foreground="White"
        Padding="10"
        Margin="0,0,0,10"/>
    
    <TextBlock 
        Text="Colored Text"
        Foreground="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}}"
        FontSize="16"/>
</StackPanel>
```

## Two-Way Binding with MVVM

For MVVM patterns, bind through a ViewModel property:

### ViewModel Code

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Media;

public class ColorViewModel : INotifyPropertyChanged
{
    private Color _selectedColor;

    public Color SelectedColor
    {
        get { return _selectedColor; }
        set
        {
            if (_selectedColor != value)
            {
                _selectedColor = value;
                OnPropertyChanged();
            }
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### XAML with DataContext

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:local="clr-namespace:ColorPaletteDemo"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="MVVM Color Binding">

    <Window.DataContext>
        <local:ColorViewModel/>
    </Window.DataContext>

    <Grid>
        <Grid.Resources>
            <local:ColorToSolidColorBrushValueConverter x:Key="ColorToBrushConverter"/>
        </Grid.Resources>

        <StackPanel Margin="20">
            <TextBlock Text="Select Color:" FontSize="14" Margin="0,0,0,10"/>
            
            <!-- Bind SelectedColor property to ViewModel -->
            <syncfusion:SfColorPalette 
                SelectedColor="{Binding SelectedColor, Mode=TwoWay}"
                Height="250"
                Width="300"
                Margin="0,0,0,20"/>
            
            <!-- Rectangle responds to ViewModel changes -->
            <Rectangle 
                Height="60"
                Width="300"
                Fill="{Binding SelectedColor, Converter={StaticResource ColorToBrushConverter}}"/>
        </StackPanel>
    </Grid>
</Window>
```

## Binding to TextBlock for Color Value Display

Display the actual color value selected. Since `Color` is a struct and cannot be indexed via `StringFormat`, use code-behind or a MultiValueConverter:

### Using Code-Behind

```xaml
<StackPanel Margin="20">
    <syncfusion:SfColorPalette 
        x:Name="colorPalette"
        Height="200"
        Width="250"
        Margin="0,0,0,15"
        SelectedColorChanged="ColorPalette_SelectedColorChanged"/>
    
    <TextBlock Text="Color Details:" FontSize="12" FontWeight="Bold" Margin="0,0,0,5"/>
    
    <!-- Display color's ARGB values via code-behind update -->
    <TextBlock 
        FontFamily="Courier New"
        Background="LightGray"
        Padding="5"
        x:Name="colorDetailsText"/>
</StackPanel>
```

```csharp
private void ColorPalette_SelectedColorChanged(object sender, SelectedColorChangedEventArgs e)
{
    Color selectedColor = e.Color;
    colorDetailsText.Text = $"A={selectedColor.A:X2} R={selectedColor.R:X2} G={selectedColor.G:X2} B={selectedColor.B:X2}";
}
```

### Using MultiValueConverter (MVVM-friendly)

Create a converter for multi-part color display:

```csharp
public class ColorToARGBStringConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is Color color)
        {
            return $"A={color.A:X2} R={color.R:X2} G={color.G:X2} B={color.B:X2}";
        }
        return "No color selected";
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

```xaml
<Window.Resources>
    <local:ColorToARGBStringConverter x:Key="ColorToARGBConverter"/>
</Window.Resources>

<StackPanel Margin="20">
    <syncfusion:SfColorPalette 
        x:Name="colorPalette"
        Height="200"
        Width="250"
        Margin="0,0,0,15"/>
    
    <TextBlock Text="Color Details:" FontSize="12" FontWeight="Bold" Margin="0,0,0,5"/>
    
    <TextBlock 
        FontFamily="Courier New"
        Background="LightGray"
        Padding="5"
        Text="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToARGBConverter}}"/>
</StackPanel>
```

## Practical Example: Color Preview Panel

Complete example with color preview and information display:

### XAML

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:local="clr-namespace:ColorPaletteDemo"
    x:Class="ColorPaletteDemo.MainWindow"
    Title="Color Preview Panel" Height="500" Width="600">

    <Grid>
        <Grid.Resources>
            <local:ColorToSolidColorBrushValueConverter x:Key="ColorToBrushConverter"/>
        </Grid.Resources>

        <StackPanel Margin="20" VerticalAlignment="Top">
            <TextBlock Text="Color Selection Tool" FontSize="18" FontWeight="Bold" Margin="0,0,0,15"/>
            
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>

                <!-- Color Palette -->
                <StackPanel Grid.Column="0" Margin="0,0,30,0">
                    <TextBlock Text="Available Colors:" FontSize="12" FontWeight="Bold" Margin="0,0,0,10"/>
                    <syncfusion:SfColorPalette 
                        x:Name="colorPalette"
                        Height="300"
                        Width="280"/>
                </StackPanel>

                <!-- Preview Area -->
                <StackPanel Grid.Column="1">
                    <TextBlock Text="Preview:" FontSize="12" FontWeight="Bold" Margin="0,0,0,10"/>
                    
                    <Rectangle 
                        Height="100"
                        Margin="0,0,0,15"
                        Fill="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}}"
                        Stroke="DarkGray"
                        StrokeThickness="1"/>
                    
                    <TextBlock Text="Color Details:" FontSize="12" FontWeight="Bold" Margin="0,0,0,5"/>
                    <TextBlock 
                        Text="{Binding ElementName=colorPalette, Path=SelectedColor}"
                        FontFamily="Courier New"
                        Background="LightGray"
                        Padding="5"
                        Margin="0,0,0,15"/>
                    
                    <TextBlock Text="Use Cases:" FontSize="12" FontWeight="Bold" Margin="0,0,0,10"/>
                    <Button 
                        Content="Apply as Background"
                        Padding="10"
                        Margin="0,0,0,8"
                        Background="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}}"
                        Foreground="White"/>
                </StackPanel>
            </Grid>
        </StackPanel>
    </Grid>
</Window>
```

## Common Binding Scenarios

| Scenario | Binding Path | Converter | Example |
|----------|--------------|-----------|---------|
| Rectangle Fill | `SelectedColor` | ColorToBrush | `Fill="{Binding Path=SelectedColor, Converter={...}}"`  |
| Button Background | `SelectedColor` | ColorToBrush | `Background="{Binding Path=SelectedColor, Converter={...}}"` |
| TextBlock Foreground | `SelectedColor` | ColorToBrush | `Foreground="{Binding Path=SelectedColor, Converter={...}}"` |
| Border BorderBrush | `SelectedColor` | ColorToBrush | `BorderBrush="{Binding Path=SelectedColor, Converter={...}}"` |

## Edge Cases

**Null Selection**: If no color is selected, the converter returns `null`. Handle this by providing default values:

```xaml
<Rectangle Fill="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}, TargetNullValue=White}"/>
```

**Default Color on Startup**: Set initial color in code-behind:

```csharp
colorPalette.SelectedColor = Colors.Blue;
```
