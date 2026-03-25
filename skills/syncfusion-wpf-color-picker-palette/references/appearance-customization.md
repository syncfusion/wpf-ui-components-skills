# Appearance & Customization

## Table of Contents
- [Flow Direction (RTL)](#flow-direction-rtl)
- [Panel Visibility Options](#panel-visibility-options)
- [Display Modes](#display-modes)
- [Color Item Sizing](#color-item-sizing)
- [Palette Popup Sizing](#palette-popup-sizing)
- [Icon Customization](#icon-customization)
- [Header Template Customization](#header-template-customization)
- [Theme Support](#theme-support)

## Flow Direction (RTL)

### Enable Right-to-Left Layout
For applications supporting RTL languages (Arabic, Hebrew, Urdu, etc.):

```xaml
<syncfusion:ColorPickerPalette FlowDirection="RightToLeft"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.FlowDirection = FlowDirection.RightToLeft;
```

### RTL Example: Arabic UI
```xaml
<Grid FlowDirection="RightToLeft">
    <StackPanel>
        <TextBlock Text="اختر لون:" FontSize="14" Margin="0,0,0,10"/>
        <syncfusion:ColorPickerPalette x:Name="colorPickerPalette"
                                       FlowDirection="RightToLeft"
                                       Width="60"
                                       Height="40" />
    </StackPanel>
</Grid>
```

## Panel Visibility Options

Control which color panels are visible in the palette dropdown.

### Show All Panels
```xaml
<syncfusion:ColorPickerPalette ThemePanelVisibility="Visible"
                               StandardPanelVisibility="Visible"
                               RecentlyUsedPanelVisibility="Visible"
                               MoreColorOptionVisibility="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

### Theme Colors Panel Only
```xaml
<syncfusion:ColorPickerPalette ThemePanelVisibility="Visible"
                               StandardPanelVisibility="Collapsed"
                               RecentlyUsedPanelVisibility="Collapsed"
                               MoreColorOptionVisibility="Collapsed"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

### Standard Colors Panel Only
```xaml
<syncfusion:ColorPickerPalette ThemePanelVisibility="Collapsed"
                               StandardPanelVisibility="Visible"
                               RecentlyUsedPanelVisibility="Collapsed"
                               MoreColorOptionVisibility="Collapsed"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

### Custom + Recently Used
```xaml
<syncfusion:ColorPickerPalette ThemePanelVisibility="Collapsed"
                               StandardPanelVisibility="Collapsed"
                               RecentlyUsedPanelVisibility="Visible"
                               SetCustomColors="True"
                               CustomHeaderVisibility="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

### Programmatic Control
```csharp
public class ColorPanelConfigurator
{
    public static void ShowMinimalPalette(ColorPickerPalette picker)
    {
        picker.ThemePanelVisibility = Visibility.Visible;
        picker.StandardPanelVisibility = Visibility.Collapsed;
        picker.RecentlyUsedPanelVisibility = Visibility.Collapsed;
        picker.MoreColorOptionVisibility = Visibility.Collapsed;
    }

    public static void ShowFullPalette(ColorPickerPalette picker)
    {
        picker.ThemePanelVisibility = Visibility.Visible;
        picker.StandardPanelVisibility = Visibility.Visible;
        picker.RecentlyUsedPanelVisibility = Visibility.Visible;
        picker.MoreColorOptionVisibility = Visibility.Visible;
    }

    public static void ShowBrandOnly(ColorPickerPalette picker)
    {
        picker.ThemePanelVisibility = Visibility.Collapsed;
        picker.StandardPanelVisibility = Visibility.Collapsed;
        picker.RecentlyUsedPanelVisibility = Visibility.Collapsed;
        picker.SetCustomColors = true;
        picker.CustomHeaderVisibility = Visibility.Visible;
    }
}
```

## Display Modes

ColorPickerPalette supports three display modes controlling how the palette opens:

### Mode 1: Dropdown (Default)
Clicking anywhere on the control opens the palette:

```xaml
<syncfusion:ColorPickerPalette Mode="Dropdown"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.Mode = PickerMode.Dropdown;
```

**Use Case:** Text formatting toolbar with quick palette access

### Mode 2: Palette (Expanded)
Shows the palette directly without a dropdown - always visible:

```xaml
<syncfusion:ColorPickerPalette Mode="Palette"
                               Name="colorPickerPalette"
                               Width="200"
                               Height="250">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.Mode = PickerMode.Palette;
```

**Use Case:** Dedicated color selection panel in your UI

### Mode 3: Split (Button + Dropdown)
- Click header: executes SelectedCommand (apply last color)
- Click dropdown arrow: opens palette

```xaml
<syncfusion:ColorPickerPalette Mode="Split"
                               SelectedCommand="{Binding ApplyColorCommand}"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.Mode = PickerMode.Split;
```

**Use Case:** Text editor with "apply last color" button functionality

### Split Mode Example: Text Formatting
```csharp
public class ViewModel : INotifyPropertyChanged
{
    public ICommand ApplyColorCommand { get; }

    public ViewModel()
    {
        ApplyColorCommand = new DelegateCommand(ApplySelectedColor);
    }

    private void ApplySelectedColor()
    {
        // Apply previously selected color to selected text
        var selectedText = textEditor.SelectedText;
        if (!string.IsNullOrEmpty(selectedText))
        {
            // Format text with last selected color
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

```xaml
<Grid>
    <StackPanel Orientation="Horizontal" Margin="10">
        <syncfusion:ColorPickerPalette x:Name="colorPicker"
                                       Mode="Split"
                                       SelectedCommand="{Binding ApplyColorCommand}"
                                       Width="60"
                                       Height="40"
                                       Margin="0,0,10,0" />
        <TextBox x:Name="textEditor" 
                 Width="300" 
                 Height="100" 
                 VerticalAlignment="Top" />
    </StackPanel>
</Grid>
```

## Color Item Sizing

### Change Individual Color Item Size
Adjust the width and height of each color swatch:

```xaml
<syncfusion:ColorPickerPalette BorderWidth="30" 
                               BorderHeight="30"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
// Large color items
colorPickerPalette.BorderWidth = 30;
colorPickerPalette.BorderHeight = 30;

// Small color items
colorPickerPalette.BorderWidth = 12;
colorPickerPalette.BorderHeight = 12;

// Default
colorPickerPalette.BorderWidth = 17;
colorPickerPalette.BorderHeight = 17;
```

### Size Recommendations
- **Default (17x17):** Standard palette
- **Small (12x12):** Compact toolbar
- **Large (25-30):** Accessible/touchscreen

### Example: Responsive Sizing
```csharp
public void ConfigureForDevice(ColorPickerPalette picker)
{
    if (IsTabletDevice())
    {
        // Large touch targets
        picker.BorderWidth = 30;
        picker.BorderHeight = 30;
    }
    else
    {
        // Standard size
        picker.BorderWidth = 17;
        picker.BorderHeight = 17;
    }
}
```

## Palette Popup Sizing

### Change Palette Popup Dimensions
```xaml
<syncfusion:ColorPickerPalette PopupWidth="120" 
                               PopupHeight="100"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
// Compact popup
colorPickerPalette.PopupWidth = 120;
colorPickerPalette.PopupHeight = 100;

// Large popup
colorPickerPalette.PopupWidth = 250;
colorPickerPalette.PopupHeight = 300;

// Default
colorPickerPalette.PopupWidth = 175;
colorPickerPalette.PopupHeight = 200;
```

### Priority Note
When both `BorderWidth`/`BorderHeight` and `PopupWidth`/`PopupHeight` are set, border properties take priority and determine final popup size.

## Icon Customization

### Customize Header Icon
```xaml
<syncfusion:ColorPickerPalette Icon="/images/fill-icon.png"
                               IconSize="18,18"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.Icon = new BitmapImage(new Uri("/images/fill-icon.png", UriKind.Relative));
colorPickerPalette.IconSize = new Size(18, 18);
```

### Customize "More Colors" Icon
```xaml
<syncfusion:ColorPickerPalette MoreColorsIcon="/images/more-colors.png"
                               MoreColorsIconSize="20,20"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.MoreColorsIcon = new BitmapImage(new Uri("/images/more-colors.png", UriKind.Relative));
colorPickerPalette.MoreColorsIconSize = new Size(20, 20);
```

### Example: Custom Icons for Toolbar
```xaml
<syncfusion:ColorPickerPalette Icon="/images/text-color.png"
                               IconSize="16,16"
                               MoreColorsIcon="/images/extended-colors.png"
                               MoreColorsIconSize="16,16"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40" />
```

## Header Template Customization

### Basic Header Template
Customize the appearance of the control header area:

```xaml
<Window.Resources>
    <DataTemplate x:Key="CustomHeaderTemplate">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            
            <!-- Color indicator -->
            <Border Width="20" Height="20" CornerRadius="3" Margin="5">
                <Border.Background>
                    <SolidColorBrush Color="{Binding Color, 
                        RelativeSource={RelativeSource FindAncestor, 
                            AncestorType={x:Type syncfusion:ColorPickerPalette}}}"/>
                </Border.Background>
            </Border>
            
            <!-- Label -->
            <TextBlock Grid.Column="1" 
                       Text="Select Color"
                       VerticalAlignment="Center"
                       Margin="5,0,0,0"/>
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:ColorPickerPalette HeaderTemplate="{DynamicResource CustomHeaderTemplate}"
                               Name="colorPickerPalette"
                               Width="120"
                               Height="40" />
```

### Advanced: Fill Indicator Template
```xaml
<Window.Resources>
    <DataTemplate x:Key="FillIndicatorTemplate">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="Auto"/>
            </Grid.ColumnDefinitions>
            
            <Image x:Name="image" 
                   Source="/images/fill.png"
                   Height="12" 
                   Width="12"
                   Margin="2"/>
            
            <Border Grid.Column="1"
                    Width="3" 
                    Height="20"
                    Margin="2,0,2,0">
                <Border.Background>
                    <SolidColorBrush Color="{Binding Color, 
                        RelativeSource={RelativeSource FindAncestor, 
                            AncestorLevel=1,
                            AncestorType={x:Type syncfusion:ColorPickerPalette}}, 
                            UpdateSourceTrigger=PropertyChanged}"/>
                </Border.Background>
            </Border>
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:ColorPickerPalette HeaderTemplate="{DynamicResource FillIndicatorTemplate}"
                               Name="colorPickerPalette"
                               Mode="Split"
                               Width="60"
                               Height="40" />
```

### DataContext in Header Template
The DataContext of HeaderTemplate is the ColorPickerPalette control itself, allowing access to:
- `Color` - current selected color
- `SelectedBrush` - current selected brush
- `ColorName` - name of current color

## Theme Support

### Apply Theme via SfSkinManager
```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Apply theme to window
        SfSkinManager.SetTheme(this, new FluentLightTheme());
    }
}
```

### Available Themes
- **FluentLightTheme** / **FluentDarkTheme**
- **MaterialLightTheme** / **MaterialDarkTheme**
- **Office2019ColorfulTheme**
- **HighContrastTheme**

### Theme Selectors
```xaml
<ComboBox x:Name="themeCombo" SelectedValueChanged="ThemeCombo_SelectedValueChanged">
    <ComboBoxItem Content="Fluent Light"/>
    <ComboBoxItem Content="Fluent Dark"/>
    <ComboBoxItem Content="Material Light"/>
    <ComboBoxItem Content="Material Dark"/>
</ComboBox>

<syncfusion:ColorPickerPalette x:Name="colorPickerPalette"
                               Width="60"
                               Height="40" />
```

```csharp
private void ThemeCombo_SelectedValueChanged(object sender, RoutedEventArgs e)
{
    var selectedTheme = (themeCombo.SelectedItem as ComboBoxItem).Content.ToString();
    
    switch (selectedTheme)
    {
        case "Fluent Light":
            SfSkinManager.SetTheme(this, new FluentLightTheme());
            break;
        case "Fluent Dark":
            SfSkinManager.SetTheme(this, new FluentDarkTheme());
            break;
        case "Material Light":
            SfSkinManager.SetTheme(this, new MaterialLightTheme());
            break;
        case "Material Dark":
            SfSkinManager.SetTheme(this, new MaterialDarkTheme());
            break;
    }
}
```

### Custom Theme via ThemeStudio
1. Open Theme Studio in Syncfusion
2. Customize ColorPickerPalette colors
3. Generate custom theme file
4. Apply in your application

```csharp
// Apply custom theme from resource
var customThemeUri = new Uri("/Themes/CustomTheme.xaml", UriKind.Relative);
Application.Current.Resources.MergedDictionaries.Add(
    new ResourceDictionary { Source = customThemeUri }
);
```

## Complete Customization Example

```xaml
<Grid Background="White">
    <StackPanel Margin="20">
        <TextBlock Text="Shape Fill Color" FontSize="14" FontWeight="Bold" Margin="0,0,0,10"/>
        
        <syncfusion:ColorPickerPalette x:Name="shapeFillPicker"
                                       Mode="Split"
                                       Color="Blue"
                                       FlowDirection="LeftToRight"
                                       GenerateThemeVariants="True"
                                       GenerateStandardVariants="True"
                                       RecentlyUsedPanelVisibility="Visible"
                                       NoColorVisibility="Visible"
                                       BorderWidth="25"
                                       BorderHeight="25"
                                       PopupWidth="200"
                                       PopupHeight="220"
                                       Icon="/images/paint-bucket.png"
                                       IconSize="18,18"
                                       Width="70"
                                       Height="45"
                                       Margin="0,0,0,20" />
        
        <Border x:Name="shapePreview"
                Width="150"
                Height="150"
                CornerRadius="10"
                Background="{Binding SelectedBrush, ElementName=shapeFillPicker}" />
    </StackPanel>
</Grid>
```
