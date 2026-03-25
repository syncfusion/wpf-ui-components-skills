# Color Palette in WPF Radial Menu

This guide covers how to create color palettes using the Syncfusion WPF Radial Menu with the `SfRadialColorItem` control.

---

## Overview

The `SfRadialColorItem` is a specialized radial menu item designed for color selection scenarios. It allows you to create hierarchical color palettes where users can select from primary colors and drill down into color variants (shades, tints, tones).

---

## SfRadialColorItem Basics

The `SfRadialColorItem` inherits from `SfRadialMenuItem` but is optimized for displaying and selecting colors. It uses the `Color` property to define the color value.

### Basic Color Palette

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Font Color">
        <navigation:SfRadialColorItem Color="Red"/>
        <navigation:SfRadialColorItem Color="Green"/>
        <navigation:SfRadialColorItem Color="Blue"/>
        <navigation:SfRadialColorItem Color="Yellow"/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

### Code-Behind Implementation

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.IsOpen = true;

SfRadialMenuItem fontColor = new SfRadialMenuItem() { Header = "Font Color" };

SfRadialColorItem red = new SfRadialColorItem() { Color = Colors.Red };
SfRadialColorItem green = new SfRadialColorItem() { Color = Colors.Green };
SfRadialColorItem blue = new SfRadialColorItem() { Color = Colors.Blue };
SfRadialColorItem yellow = new SfRadialColorItem() { Color = Colors.Yellow };

fontColor.Items.Add(red);
fontColor.Items.Add(green);
fontColor.Items.Add(blue);
fontColor.Items.Add(yellow);

radialMenu.Items.Add(fontColor);
```

---

## Hierarchical Color Palettes

Create nested color structures where each primary color contains its shades and variants.

### Two-Level Color Palette

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Font Color">
        
        <!-- Primary color with no children -->
        <navigation:SfRadialMenuItem Header="Font"/>
        
        <!-- Red family with variants -->
        <navigation:SfRadialColorItem Color="Red">
            <navigation:SfRadialColorItem Color="DarkRed"/>
            <navigation:SfRadialColorItem Color="IndianRed"/>
            <navigation:SfRadialColorItem Color="OrangeRed"/>
            <navigation:SfRadialColorItem Color="MediumVioletRed"/>
        </navigation:SfRadialColorItem>
        
        <!-- Green family -->
        <navigation:SfRadialColorItem Color="Green"/>
        
        <!-- Blue family -->
        <navigation:SfRadialColorItem Color="Blue"/>
        
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

### Complete Color Family Example

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Colors">
        
        <!-- Red Family -->
        <navigation:SfRadialColorItem Color="Red">
            <navigation:SfRadialColorItem Color="DarkRed"/>
            <navigation:SfRadialColorItem Color="FireBrick"/>
            <navigation:SfRadialColorItem Color="Crimson"/>
            <navigation:SfRadialColorItem Color="IndianRed"/>
            <navigation:SfRadialColorItem Color="LightCoral"/>
            <navigation:SfRadialColorItem Color="Salmon"/>
        </navigation:SfRadialColorItem>
        
        <!-- Blue Family -->
        <navigation:SfRadialColorItem Color="Blue">
            <navigation:SfRadialColorItem Color="Navy"/>
            <navigation:SfRadialColorItem Color="DarkBlue"/>
            <navigation:SfRadialColorItem Color="MediumBlue"/>
            <navigation:SfRadialColorItem Color="RoyalBlue"/>
            <navigation:SfRadialColorItem Color="SteelBlue"/>
            <navigation:SfRadialColorItem Color="LightBlue"/>
        </navigation:SfRadialColorItem>
        
        <!-- Green Family -->
        <navigation:SfRadialColorItem Color="Green">
            <navigation:SfRadialColorItem Color="DarkGreen"/>
            <navigation:SfRadialColorItem Color="ForestGreen"/>
            <navigation:SfRadialColorItem Color="SeaGreen"/>
            <navigation:SfRadialColorItem Color="MediumSeaGreen"/>
            <navigation:SfRadialColorItem Color="LightGreen"/>
            <navigation:SfRadialColorItem Color="Lime"/>
        </navigation:SfRadialColorItem>
        
        <!-- Yellow Family -->
        <navigation:SfRadialColorItem Color="Yellow">
            <navigation:SfRadialColorItem Color="Gold"/>
            <navigation:SfRadialColorItem Color="Orange"/>
            <navigation:SfRadialColorItem Color="DarkOrange"/>
            <navigation:SfRadialColorItem Color="LightYellow"/>
            <navigation:SfRadialColorItem Color="LemonChiffon"/>
        </navigation:SfRadialColorItem>
        
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

---

## Programmatic Color Palette Creation

### Building Color Palettes from Code

```csharp
public class ColorPaletteBuilder
{
    public static SfRadialMenu CreateColorPalette()
    {
        SfRadialMenu radialMenu = new SfRadialMenu();
        radialMenu.IsOpen = true;
        
        SfRadialMenuItem colorPicker = new SfRadialMenuItem() { Header = "Colors" };
        
        // Define color families
        var redFamily = new[] 
        { 
            Colors.DarkRed, Colors.FireBrick, Colors.Crimson, 
            Colors.IndianRed, Colors.LightCoral, Colors.Salmon 
        };
        
        var blueFamily = new[] 
        { 
            Colors.Navy, Colors.DarkBlue, Colors.RoyalBlue, 
            Colors.SteelBlue, Colors.LightBlue, Colors.PowderBlue 
        };
        
        var greenFamily = new[] 
        { 
            Colors.DarkGreen, Colors.ForestGreen, Colors.SeaGreen, 
            Colors.MediumSeaGreen, Colors.LightGreen, Colors.Lime 
        };
        
        // Create Red family
        SfRadialColorItem redItem = new SfRadialColorItem() { Color = Colors.Red };
        foreach (var color in redFamily)
        {
            redItem.Items.Add(new SfRadialColorItem() { Color = color });
        }
        colorPicker.Items.Add(redItem);
        
        // Create Blue family
        SfRadialColorItem blueItem = new SfRadialColorItem() { Color = Colors.Blue };
        foreach (var color in blueFamily)
        {
            blueItem.Items.Add(new SfRadialColorItem() { Color = color });
        }
        colorPicker.Items.Add(blueItem);
        
        // Create Green family
        SfRadialColorItem greenItem = new SfRadialColorItem() { Color = Colors.Green };
        foreach (var color in greenFamily)
        {
            greenItem.Items.Add(new SfRadialColorItem() { Color = color });
        }
        colorPicker.Items.Add(greenItem);
        
        radialMenu.Items.Add(colorPicker);
        return radialMenu;
    }
}
```

### Dynamic Color Generation

```csharp
public class ColorVariantGenerator
{
    public static List<Color> GenerateShades(Color baseColor, int count)
    {
        List<Color> shades = new List<Color>();
        
        for (int i = 0; i < count; i++)
        {
            double factor = (double)(count - i) / count;
            byte r = (byte)(baseColor.R * factor);
            byte g = (byte)(baseColor.G * factor);
            byte b = (byte)(baseColor.B * factor);
            
            shades.Add(Color.FromRgb(r, g, b));
        }
        
        return shades;
    }
    
    public static List<Color> GenerateTints(Color baseColor, int count)
    {
        List<Color> tints = new List<Color>();
        
        for (int i = 0; i < count; i++)
        {
            double factor = (double)i / count;
            byte r = (byte)(baseColor.R + (255 - baseColor.R) * factor);
            byte g = (byte)(baseColor.G + (255 - baseColor.G) * factor);
            byte b = (byte)(baseColor.B + (255 - baseColor.B) * factor);
            
            tints.Add(Color.FromRgb(r, g, b));
        }
        
        return tints;
    }
}

// Usage
var redShades = ColorVariantGenerator.GenerateShades(Colors.Red, 6);
SfRadialColorItem redItem = new SfRadialColorItem() { Color = Colors.Red };
foreach (var shade in redShades)
{
    redItem.Items.Add(new SfRadialColorItem() { Color = shade });
}
```

---

## Handling Color Selection

### Click Event for Color Items

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Colors">
        <navigation:SfRadialColorItem Color="Red" Click="ColorItem_Click"/>
        <navigation:SfRadialColorItem Color="Green" Click="ColorItem_Click"/>
        <navigation:SfRadialColorItem Color="Blue" Click="ColorItem_Click"/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

```csharp
private void ColorItem_Click(object sender, EventArgs e)
{
    SfRadialColorItem colorItem = sender as SfRadialColorItem;
    if (colorItem != null)
    {
        Color selectedColor = colorItem.Color;
        ApplyColor(selectedColor);
        MessageBox.Show($"Selected color: {selectedColor}");
    }
}

private void ApplyColor(Color color)
{
    // Apply the selected color to text, background, etc.
    SelectedTextBrush = new SolidColorBrush(color);
}
```

### Command Binding for Color Selection

```csharp
public class ColorCommand : ICommand
{
    private readonly Action<Color> _execute;
    
    public ColorCommand(Action<Color> execute)
    {
        _execute = execute;
    }
    
    public bool CanExecute(object parameter) => true;
    
    public void Execute(object parameter)
    {
        if (parameter is Color color)
        {
            _execute(color);
        }
    }
    
    public event EventHandler CanExecuteChanged;
}

// ViewModel
public class EditorViewModel
{
    public ICommand ColorSelectedCommand { get; }
    
    public EditorViewModel()
    {
        ColorSelectedCommand = new ColorCommand(OnColorSelected);
    }
    
    private void OnColorSelected(Color color)
    {
        // Handle color selection
        CurrentFontColor = color;
    }
}
```

---

## Advanced Color Palette Patterns

### Pattern 1: Theme-Based Color Palette

Organize colors by usage rather than color family:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Theme Colors">
        
        <navigation:SfRadialMenuItem Header="Primary">
            <navigation:SfRadialColorItem Color="#2196F3"/>
            <navigation:SfRadialColorItem Color="#1976D2"/>
            <navigation:SfRadialColorItem Color="#0D47A1"/>
        </navigation:SfRadialMenuItem>
        
        <navigation:SfRadialMenuItem Header="Success">
            <navigation:SfRadialColorItem Color="#4CAF50"/>
            <navigation:SfRadialColorItem Color="#388E3C"/>
            <navigation:SfRadialColorItem Color="#1B5E20"/>
        </navigation:SfRadialMenuItem>
        
        <navigation:SfRadialMenuItem Header="Warning">
            <navigation:SfRadialColorItem Color="#FFC107"/>
            <navigation:SfRadialColorItem Color="#FFA000"/>
            <navigation:SfRadialColorItem Color="#FF6F00"/>
        </navigation:SfRadialMenuItem>
        
        <navigation:SfRadialMenuItem Header="Danger">
            <navigation:SfRadialColorItem Color="#F44336"/>
            <navigation:SfRadialColorItem Color="#D32F2F"/>
            <navigation:SfRadialColorItem Color="#B71C1C"/>
        </navigation:SfRadialMenuItem>
        
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

### Pattern 2: Grayscale with Primary Colors

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Colors">
        
        <!-- Grayscale -->
        <navigation:SfRadialMenuItem Header="Gray">
            <navigation:SfRadialColorItem Color="Black"/>
            <navigation:SfRadialColorItem Color="DarkGray"/>
            <navigation:SfRadialColorItem Color="Gray"/>
            <navigation:SfRadialColorItem Color="LightGray"/>
            <navigation:SfRadialColorItem Color="White"/>
        </navigation:SfRadialMenuItem>
        
        <!-- Primary Colors -->
        <navigation:SfRadialColorItem Color="Red"/>
        <navigation:SfRadialColorItem Color="Green"/>
        <navigation:SfRadialColorItem Color="Blue"/>
        <navigation:SfRadialColorItem Color="Yellow"/>
        <navigation:SfRadialColorItem Color="Magenta"/>
        <navigation:SfRadialColorItem Color="Cyan"/>
        
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

### Pattern 3: Recent Colors + Color Families

```csharp
public class ColorPaletteViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Color> _recentColors = new ObservableCollection<Color>();
    
    public ObservableCollection<Color> RecentColors
    {
        get => _recentColors;
        set
        {
            _recentColors = value;
            OnPropertyChanged(nameof(RecentColors));
        }
    }
    
    public void AddRecentColor(Color color)
    {
        // Remove if already exists
        if (RecentColors.Contains(color))
            RecentColors.Remove(color);
        
        // Add to front
        RecentColors.Insert(0, color);
        
        // Keep only last 8 colors
        while (RecentColors.Count > 8)
            RecentColors.RemoveAt(RecentColors.Count - 1);
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```

---

## Best Practices

### Color Organization

1. **Logical Grouping:** Group related colors together (warm colors, cool colors, neutrals)
2. **Consistent Hierarchy:** Use the same structure across all color families
3. **Limit Options:** Don't overwhelm users with too many choices (6-8 primary colors recommended)
4. **Include Common Colors:** Always include frequently-used colors at the top level

### User Experience

1. **Visual Feedback:** Ensure selected color is visually indicated
2. **Recent Colors:** Provide quick access to recently used colors
3. **Default Selection:** Pre-select a sensible default color
4. **Close on Select:** Consider setting `CloseOnExecute="True"` for color items

### Accessibility

1. **Color Names:** Provide tooltips with color names for accessibility
2. **High Contrast:** Ensure color items are distinguishable in high contrast mode
3. **Keyboard Navigation:** Test that colors are selectable via keyboard

---

## Use Cases

### Use Case 1: Text Editor Color Picker

```xaml
<navigation:SfRadialMenu x:Name="textColorPicker">
    <navigation:SfRadialMenuItem Header="Text Color">
        <navigation:SfRadialColorItem Color="Black" ToolTip="Black" CloseOnExecute="True"/>
        <navigation:SfRadialColorItem Color="Red" ToolTip="Red" CloseOnExecute="True">
            <navigation:SfRadialColorItem Color="DarkRed" ToolTip="Dark Red" CloseOnExecute="True"/>
            <navigation:SfRadialColorItem Color="LightCoral" ToolTip="Light Coral" CloseOnExecute="True"/>
        </navigation:SfRadialColorItem>
        <navigation:SfRadialColorItem Color="Blue" ToolTip="Blue" CloseOnExecute="True">
            <navigation:SfRadialColorItem Color="Navy" ToolTip="Navy" CloseOnExecute="True"/>
            <navigation:SfRadialColorItem Color="SkyBlue" ToolTip="Sky Blue" CloseOnExecute="True"/>
        </navigation:SfRadialColorItem>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

### Use Case 2: Drawing Application Color Palette

```csharp
public class DrawingColorPalette
{
    public static SfRadialMenu CreatePalette()
    {
        var menu = new SfRadialMenu() { IsOpen = true };
        var colors = new SfRadialMenuItem() { Header = "Brush" };
        
        // Add standard drawing colors
        var standardColors = new[] 
        {
            Colors.Black, Colors.White, Colors.Red, Colors.Blue,
            Colors.Green, Colors.Yellow, Colors.Orange, Colors.Purple
        };
        
        foreach (var color in standardColors)
        {
            var item = new SfRadialColorItem() 
            { 
                Color = color, 
                CloseOnExecute = true 
            };
            item.Click += (s, e) => ApplyBrushColor(color);
            colors.Items.Add(item);
        }
        
        menu.Items.Add(colors);
        return menu;
    }
    
    private static void ApplyBrushColor(Color color)
    {
        // Apply color to drawing brush
    }
}
```

---

## Troubleshooting

### Issue: Color Items Not Displaying Colors

**Problem:** SfRadialColorItem added but colors don't show.

**Solution:** Ensure the `Color` property is set correctly:
```xaml
<!-- Wrong -->
<navigation:SfRadialColorItem Header="Red"/>

<!-- Correct -->
<navigation:SfRadialColorItem Color="Red"/>
```

### Issue: Can't Detect Selected Color

**Problem:** Unable to retrieve the selected color from a color item.

**Solution:** Cast sender to SfRadialColorItem and access Color property:
```csharp
private void ColorItem_Click(object sender, EventArgs e)
{
    if (sender is SfRadialColorItem colorItem)
    {
        Color selectedColor = colorItem.Color;
        // Use selectedColor
    }
}
```
