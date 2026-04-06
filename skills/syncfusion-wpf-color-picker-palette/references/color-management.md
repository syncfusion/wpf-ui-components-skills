# Color Management in ColorPickerPalette

## Table of Contents
- [Accessing Colors Programmatically](#accessing-colors-programmatically)
- [Working with Color Brushes](#working-with-color-brushes)
- [Automatic Color Defaults](#automatic-color-defaults)
- [Transparent Color Selection](#transparent-color-selection)
- [Theme Colors & Variants](#theme-colors--variants)
- [Standard Colors](#standard-colors)
- [Custom Colors Collection](#custom-colors-collection)
- [Recently Used Colors Panel](#recently-used-colors-panel)
- [Black and White Variants](#black-and-white-variants)

## Accessing Colors Programmatically

### Get the Selected Color
```csharp
// Get the Color value
Color selectedColor = colorPickerPalette.Color;

// Get the color name
string colorName = colorPickerPalette.ColorName;

// Example: Display color info
MessageBox.Show($"Selected Color: {colorName} - RGB: {selectedColor.R},{selectedColor.G},{selectedColor.B}");
```

### Set a Specific Color
```csharp
// Set by named color
colorPickerPalette.Color = Colors.Red;
colorPickerPalette.Color = Colors.Transparent;
colorPickerPalette.Color = Colors.Black;

// Set by RGB
colorPickerPalette.Color = Color.FromRgb(255, 128, 0);  // Orange

// Set by ARGB (with alpha/transparency)
colorPickerPalette.Color = Color.FromArgb(128, 255, 0, 0);  // Semi-transparent red
```

### Bind Color to ViewModel
```xaml
<!-- XAML with binding -->
<syncfusion:ColorPickerPalette Color="{Binding SelectedColor, Mode=TwoWay}"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40" />
```

```csharp
// ViewModel
public class ViewModel : INotifyPropertyChanged
{
    private Color selectedColor = Colors.Blue;
    
    public Color SelectedColor
    {
        get { return selectedColor; }
        set 
        { 
            selectedColor = value;
            OnPropertyChanged(nameof(SelectedColor));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Working with Color Brushes

Access `SelectedBrush` property to get/set brush values. Set XAML: `SelectedBrush="Yellow"` or C#: `colorPickerPalette.SelectedBrush = Brushes.Green`. Bind to UI elements: `Fill="{Binding SelectedBrush, ElementName=colorPickerPalette}"`.

## Automatic Color Defaults

Set `AutomaticColor` to a default (e.g., `Colors.Green`) and show with `AutomaticColorVisibility="Visible"`. Hide with `AutomaticColorVisibility="Collapsed"`.

```csharp
colorPickerPalette.AutomaticColorVisibility = Visibility.Collapsed;
```

### Practical Example: Text Editor
```csharp
public class TextEditorViewModel
{
    public Color DefaultForegroundColor { get; set; } = Colors.Black;

    public void InitializeColorPicker(ColorPickerPalette picker)
    {
        // Set automatic color to document default
        picker.AutomaticColor = DefaultForegroundColor;
        picker.AutomaticColorVisibility = Visibility.Visible;
        
        // Initialize with default
        picker.Color = DefaultForegroundColor;
    }
}
```

## Transparent Color Selection

Enable "No Color" with `NoColorVisibility="Visible"`. Set transparent: `colorPickerPalette.Color = Colors.Transparent`. Detect in code: `if (colorPickerPalette.Color == Colors.Transparent)` or in event: `if (e.NewColor == Colors.Transparent)`.

## Theme Colors & Variants

### Select Theme
```xaml
<syncfusion:ColorPickerPalette Themes="Metro"
                               GenerateThemeVariants="True" 
                               ThemePanelVisibility="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
// Change theme programmatically
colorPickerPalette.Themes = PaletteTheme.Metro;
colorPickerPalette.GenerateThemeVariants = true;
colorPickerPalette.ThemePanelVisibility = Visibility.Visible;
```

### Available Themes
- **Office** (default)
- **Apex**
- **Grayscale**
- **Metro**

### Show/Hide Theme Variants
```csharp
// Show only base theme colors (no variants)
colorPickerPalette.GenerateThemeVariants = false;

// Show base + variant colors (light, medium, dark)
colorPickerPalette.GenerateThemeVariants = true;
```

### Practical Example: Brand Colors
```csharp
public void SetBrandTheme(ColorPickerPalette picker)
{
    // Use professional theme
    picker.Themes = PaletteTheme.Office;
    
    // Show variants for flexibility
    picker.GenerateThemeVariants = true;
    picker.ThemePanelVisibility = Visibility.Visible;
    
    // Hide unnecessary panels
    picker.StandardPanelVisibility = Visibility.Collapsed;
}
```

## Standard Colors

### Enable Standard Colors
```xaml
<syncfusion:ColorPickerPalette GenerateStandardVariants="True" 
                               StandardPanelVisibility="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.GenerateStandardVariants = true;
colorPickerPalette.StandardPanelVisibility = Visibility.Visible;
```

### Show/Hide Standard Variants
```csharp
// Show base standard colors only
colorPickerPalette.GenerateStandardVariants = false;

// Show base + variant (light/dark) standard colors
colorPickerPalette.GenerateStandardVariants = true;
```

### Hide Standard Colors
```csharp
colorPickerPalette.StandardPanelVisibility = Visibility.Collapsed;
```

## Custom Colors Collection

### Adding Custom Colors
```csharp
// Create custom color collection
var customColors = new ObservableCollection<CustomColor>
{
    new CustomColor { Color = Color.FromArgb(255, 17, 235, 248), ColorName = "Aqua" },
    new CustomColor { Color = Color.FromArgb(255, 248, 15, 166), ColorName = "Deep Pink" },
    new CustomColor { Color = Color.FromArgb(255, 139, 167, 194), ColorName = "Dark Gray" },
    new CustomColor { Color = Color.FromArgb(255, 245, 60, 207), ColorName = "Hot Pink" },
    new CustomColor { Color = Color.FromArgb(255, 194, 146, 149), ColorName = "Olive Drab" }
};

// Apply to picker
colorPickerPalette.CustomColorsCollection = customColors;
colorPickerPalette.SetCustomColors = true;
colorPickerPalette.CustomHeaderText = "Custom Colors";
colorPickerPalette.CustomHeaderVisibility = Visibility.Visible;
```

### Custom Colors via XAML
```xaml
<Window.Resources>
    <local:ViewModel x:Key="viewModel">
        <local:ViewModel.NewColorCollection>
            <syncfusion:CustomColor Color="#FF11EBF8" ColorName="Aqua" />
            <syncfusion:CustomColor Color="#FFF80FA6" ColorName="Deep Pink" />
            <syncfusion:CustomColor Color="#FF8BA7C2" ColorName="Dark Gray" />
            <syncfusion:CustomColor Color="#F53CDF07" ColorName="Hot Pink" />
            <syncfusion:CustomColor Color="#C2929545" ColorName="Olive Drab" />
        </local:ViewModel.NewColorCollection>
    </local:ViewModel>
</Window.Resources>

<syncfusion:ColorPickerPalette CustomColorsCollection="{Binding NewColorCollection}"  
                               CustomHeaderText="Brand Colors"
                               CustomHeaderVisibility="Visible"
                               SetCustomColors="True"
                               DataContext="{StaticResource viewModel}"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

### Brand Color Example
```csharp
public class BrandColorsProvider
{
    public static ObservableCollection<CustomColor> GetBrandColors()
    {
        return new ObservableCollection<CustomColor>
        {
            new CustomColor { Color = Color.FromRgb(51, 122, 183), ColorName = "Brand Blue" },
            new CustomColor { Color = Color.FromRgb(220, 53, 69), ColorName = "Brand Red" },
            new CustomColor { Color = Color.FromRgb(40, 167, 69), ColorName = "Brand Green" },
            new CustomColor { Color = Color.FromRgb(255, 193, 7), ColorName = "Brand Yellow" }
        };
    }

    public static void ApplyBrandColors(ColorPickerPalette picker)
    {
        picker.CustomColorsCollection = GetBrandColors();
        picker.SetCustomColors = true;
        picker.CustomHeaderText = "Company Colors";
    }
}
```

## Recently Used Colors Panel

### Enable Recently Used Panel
```xaml
<syncfusion:ColorPickerPalette RecentlyUsedPanelVisibility="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.RecentlyUsedPanelVisibility = Visibility.Visible;
```

### Disable Recently Used Panel
```csharp
colorPickerPalette.RecentlyUsedPanelVisibility = Visibility.Collapsed;
```

### How It Works
- When user selects a color from standard or custom tabs, it's added to recently used
- Recently used colors appear in the RecentlyUsedPanel for quick access
- Panel is sorted by recency (most recent first)

### Practical Example: Quick Recoloring
```xaml
<StackPanel>
    <TextBlock Text="Text Foreground Color:" FontWeight="Bold" />
    <syncfusion:ColorPickerPalette x:Name="colorPicker"
                                   Color="Black"
                                   GenerateThemeVariants="True"
                                   GenerateStandardVariants="True"
                                   RecentlyUsedPanelVisibility="Visible"
                                   SelectedBrushChanged="ColorPicker_SelectedBrushChanged"
                                   Width="60"
                                   Height="40" />
</StackPanel>
```

```csharp
private void ColorPicker_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e)
{
    // User can quickly reapply previously used colors via RecentlyUsedPanel
    selectedTextBlock.Foreground = e.NewBrush;
}
```

## Black and White Variants

### Enable Black/White Variants
```xaml
<syncfusion:ColorPickerPalette BlackWhiteVisibility="Both"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
// Show both black and white variants
colorPickerPalette.BlackWhiteVisibility = BlackWhiteVisible.Both;

// Show only white variants
colorPickerPalette.BlackWhiteVisibility = BlackWhiteVisible.White;

// Show only black variants
colorPickerPalette.BlackWhiteVisibility = BlackWhiteVisible.Black;

// Hide (default)
colorPickerPalette.BlackWhiteVisibility = BlackWhiteVisible.None;
```

### Use Case: Contrast Variants
```csharp
public void SetupAccessibleColors(ColorPickerPalette picker)
{
    // Show white and black variants for accessibility
    picker.BlackWhiteVisibility = BlackWhiteVisible.Both;
    
    // Users can choose high-contrast variants for better readability
}
```
