# Customization and Theming

This guide covers customizing ToolBarAdv appearance through styling, themes, and visual properties.

## Table of Contents
- [FloatingToolBarStyle](#floatingtoolbarstyle)
- [Foreground Customization](#foreground-customization)
- [ControlsResourceDictionary](#controlsresourcedictionary)
- [SfSkinManager Theme Application](#sfskinmanager-theme-application)
- [ThemeStudio for Custom Themes](#themestudio-for-custom-themes)
- [Built-in Theme Options](#built-in-theme-options)
- [Best Practices](#best-practices)

## FloatingToolBarStyle

The `FloatingToolBarStyle` property customizes the appearance of floating toolbar windows.

### Basic FloatingToolBarStyle

**XAML:**

```xaml
<syncfusion:ToolBarAdv ToolBarName="Custom Floating">
    <syncfusion:ToolBarAdv.FloatingToolBarStyle>
        <Style TargetType="Window">
            <Setter Property="Background" Value="LightBlue"/>
            <Setter Property="BorderBrush" Value="DarkBlue"/>
            <Setter Property="BorderThickness" Value="2"/>
        </Style>
    </syncfusion:ToolBarAdv.FloatingToolBarStyle>
    
    <Button Content="Tool 1"/>
    <Button Content="Tool 2"/>
</syncfusion:ToolBarAdv>
```

**Result:** When toolbar floats, window has light blue background with dark blue 2px border.

### Advanced Floating Window Styling

```xaml
<syncfusion:ToolBarAdv ToolBarName="Styled Floating Toolbar">
    <syncfusion:ToolBarAdv.FloatingToolBarStyle>
        <Style TargetType="Window">
            <!-- Background -->
            <Setter Property="Background" Value="#F0F0F0"/>
            
            <!-- Border -->
            <Setter Property="BorderBrush" Value="#0078D7"/>
            <Setter Property="BorderThickness" Value="1"/>
            
            <!-- Title bar customization -->
            <Setter Property="FontFamily" Value="Segoe UI"/>
            <Setter Property="FontSize" Value="12"/>
            <Setter Property="FontWeight" Value="SemiBold"/>
            
            <!-- Window behavior -->
            <Setter Property="ResizeMode" Value="CanResize"/>
            <Setter Property="SizeToContent" Value="WidthAndHeight"/>
            <Setter Property="Topmost" Value="True"/>
            
            <!-- Shadow effect -->
            <Setter Property="Effect">
                <Setter.Value>
                    <DropShadowEffect Color="Black" 
                                      BlurRadius="10" 
                                      ShadowDepth="3" 
                                      Opacity="0.5"/>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:ToolBarAdv.FloatingToolBarStyle>
    
    <Button Content="Tool 1"/>
    <Button Content="Tool 2"/>
</syncfusion:ToolBarAdv>
```

### Setting in C#

```csharp
Style floatingStyle = new Style(typeof(Window));

floatingStyle.Setters.Add(new Setter(Window.BackgroundProperty, 
    new SolidColorBrush(Colors.LightGray)));
floatingStyle.Setters.Add(new Setter(Window.BorderBrushProperty, 
    new SolidColorBrush(Colors.DarkGray)));
floatingStyle.Setters.Add(new Setter(Window.BorderThicknessProperty, 
    new Thickness(2)));

toolBar.FloatingToolBarStyle = floatingStyle;
```

### Resource-Based Styling

**Define in App.xaml or Window.Resources:**

```xaml
<Window.Resources>
    <Style x:Key="CustomFloatingStyle" TargetType="Window">
        <Setter Property="Background" Value="White"/>
        <Setter Property="BorderBrush" Value="Gray"/>
        <Setter Property="BorderThickness" Value="1"/>
        <Setter Property="ResizeMode" Value="NoResize"/>
    </Style>
</Window.Resources>

<!-- Apply to toolbar -->
<syncfusion:ToolBarAdv FloatingToolBarStyle="{StaticResource CustomFloatingStyle}">
    <Button Content="Tool"/>
</syncfusion:ToolBarAdv>
```

## Foreground Customization

Change text and icon colors for toolbar items.

### Toolbar Foreground

```xaml
<syncfusion:ToolBarAdv Foreground="White" Background="DarkBlue">
    <Button Content="New" Foreground="{Binding Foreground, RelativeSource={RelativeSource AncestorType=syncfusion:ToolBarAdv}}"/>
    <Button Content="Open" Foreground="{Binding Foreground, RelativeSource={RelativeSource AncestorType=syncfusion:ToolBarAdv}}"/>
</syncfusion:ToolBarAdv>
```

### Individual Button Foreground

```xaml
<syncfusion:ToolBarAdv>
    <Button Content="Normal" Foreground="Black"/>
    <Button Content="Important" Foreground="Red" FontWeight="Bold"/>
    <Button Content="Success" Foreground="Green"/>
</syncfusion:ToolBarAdv>
```

### Foreground for Icon Paths

```xaml
<syncfusion:ToolBarAdv Foreground="White" Background="#2D2D30">
    <Button Width="22" Height="22" ToolTip="New">
        <!-- Icon inherits toolbar foreground -->
        <Path Data="M19,13H13V19H11V13H5V11H11V5H13V11H19V13Z" 
              Fill="{Binding Foreground, RelativeSource={RelativeSource AncestorType=syncfusion:ToolBarAdv}}"
              Stretch="Uniform"
              Width="16" 
              Height="16"/>
    </Button>
</syncfusion:ToolBarAdv>
```

### C# Foreground Setting

```csharp
toolBar.Foreground = new SolidColorBrush(Colors.White);
toolBar.Background = new SolidColorBrush(Color.FromRgb(45, 45, 48)); // Dark gray
```

## ControlsResourceDictionary

Use ControlsResourceDictionary to apply styles to FrameworkElement controls within toolbars.

### Basic ControlsResourceDictionary

```xaml
<syncfusion:ToolBarAdv>
    <syncfusion:ToolBarAdv.ControlsResourceDictionary>
        <ResourceDictionary>
            <!-- Style all buttons in this toolbar -->
            <Style TargetType="Button">
                <Setter Property="Background" Value="LightBlue"/>
                <Setter Property="Foreground" Value="DarkBlue"/>
                <Setter Property="Margin" Value="2"/>
                <Setter Property="Padding" Value="5,2"/>
            </Style>
            
            <!-- Style all textboxes -->
            <Style TargetType="TextBox">
                <Setter Property="Background" Value="White"/>
                <Setter Property="BorderBrush" Value="Gray"/>
                <Setter Property="Padding" Value="3"/>
            </Style>
        </ResourceDictionary>
    </syncfusion:ToolBarAdv.ControlsResourceDictionary>
    
    <Button Content="New"/>
    <Button Content="Open"/>
    <TextBox Width="100" Text="Search"/>
</syncfusion:ToolBarAdv>
```

### Targeted Styling with Keys

```xaml
<syncfusion:ToolBarAdv>
    <syncfusion:ToolBarAdv.ControlsResourceDictionary>
        <ResourceDictionary>
            <!-- Named style for specific buttons -->
            <Style x:Key="PrimaryButton" TargetType="Button">
                <Setter Property="Background" Value="#0078D7"/>
                <Setter Property="Foreground" Value="White"/>
                <Setter Property="FontWeight" Value="Bold"/>
                <Setter Property="Padding" Value="10,5"/>
            </Style>
            
            <Style x:Key="SecondaryButton" TargetType="Button">
                <Setter Property="Background" Value="LightGray"/>
                <Setter Property="Foreground" Value="Black"/>
                <Setter Property="Padding" Value="10,5"/>
            </Style>
        </ResourceDictionary>
    </syncfusion:ToolBarAdv.ControlsResourceDictionary>
    
    <Button Content="Save" Style="{StaticResource PrimaryButton}"/>
    <Button Content="Cancel" Style="{StaticResource SecondaryButton}"/>
</syncfusion:ToolBarAdv>
```

### Hover and Pressed States

```xaml
<syncfusion:ToolBarAdv>
    <syncfusion:ToolBarAdv.ControlsResourceDictionary>
        <ResourceDictionary>
            <Style TargetType="Button">
                <Setter Property="Background" Value="White"/>
                <Setter Property="BorderBrush" Value="Gray"/>
                <Setter Property="BorderThickness" Value="1"/>
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Button">
                            <Border Background="{TemplateBinding Background}"
                                    BorderBrush="{TemplateBinding BorderBrush}"
                                    BorderThickness="{TemplateBinding BorderThickness}"
                                    CornerRadius="3">
                                <ContentPresenter HorizontalAlignment="Center" 
                                                  VerticalAlignment="Center"
                                                  Margin="5,2"/>
                            </Border>
                            <ControlTemplate.Triggers>
                                <!-- Hover -->
                                <Trigger Property="IsMouseOver" Value="True">
                                    <Setter Property="Background" Value="#E5F3FF"/>
                                    <Setter Property="BorderBrush" Value="#0078D7"/>
                                </Trigger>
                                <!-- Pressed -->
                                <Trigger Property="IsPressed" Value="True">
                                    <Setter Property="Background" Value="#0078D7"/>
                                    <Setter Property="Foreground" Value="White"/>
                                </Trigger>
                            </ControlTemplate.Triggers>
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </syncfusion:ToolBarAdv.ControlsResourceDictionary>
    
    <Button Content="New"/>
    <Button Content="Open"/>
    <Button Content="Save"/>
</syncfusion:ToolBarAdv>
```

## SfSkinManager Theme Application

Apply Syncfusion themes consistently across ToolBarAdv and entire application.

### Available with Syncfusion.Themes.* NuGet Packages

Install theme package:

```powershell
Install-Package Syncfusion.Themes.FluentLight.WPF
```

### Applying Theme to Window

**C# (App.xaml.cs or Window code-behind):**

```csharp
using Syncfusion.SfSkinManager;
using Syncfusion.Themes.FluentLight.WPF;

public MainWindow()
{
    InitializeComponent();
    
    // Apply FluentLight theme to entire window
    SfSkinManager.SetTheme(this, new Theme("FluentLight"));
}
```

### Applying Theme to Specific ToolBarAdv

```csharp
using Syncfusion.SfSkinManager;

// Apply to toolbar only
SfSkinManager.SetTheme(toolBarAdv, new Theme("FluentDark"));
```

### XAML Theme Application

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    
    <!-- Apply theme to entire window -->
    <syncfusionskin:SfSkinManager.Theme>
        <syncfusionskin:Theme ThemeName="FluentLight"/>
    </syncfusionskin:SfSkinManager.Theme>
    
    <syncfusion:ToolBarManager>
        <syncfusion:ToolBarManager.TopToolBarTray>
            <syncfusion:ToolBarTrayAdv>
                <syncfusion:ToolBarAdv ToolBarName="File">
                    <Button Content="New"/>
                </syncfusion:ToolBarAdv>
            </syncfusion:ToolBarTrayAdv>
        </syncfusion:ToolBarManager.TopToolBarTray>
        
        <syncfusion:ToolBarManager.Content>
            <TextBox/>
        </syncfusion:ToolBarManager.Content>
    </syncfusion:ToolBarManager>
</Window>
```

### Theme Inheritance

When you apply a theme to the Window, all Syncfusion controls (including ToolBarAdv) automatically inherit it:

```csharp
// Set once at Window level
SfSkinManager.SetTheme(this, new Theme("MaterialDark"));

// All ToolBarAdv controls automatically use MaterialDark theme
```

## ThemeStudio for Custom Themes

Create custom themes using Syncfusion ThemeStudio.

### Steps to Create Custom Theme

1. **Open ThemeStudio**: https://help.syncfusion.com/wpf/themes/theme-studio
2. **Select Base Theme**: FluentLight, MaterialLight, Office2019, etc.
3. **Customize Colors**: Primary, secondary, background, foreground
4. **Export Theme**: Download generated theme package
5. **Add to Project**: Include theme assembly and resource dictionaries
6. **Apply Theme**: Use SfSkinManager

### Applying Custom Theme

**After exporting from ThemeStudio:**

```csharp
using Syncfusion.SfSkinManager;

// Apply custom theme (replace with your exported theme name)
SfSkinManager.SetTheme(this, new Theme("MyCustomTheme"));
```

### Custom Theme Resource Dictionary

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- Include custom theme -->
            <ResourceDictionary Source="/MyCustomTheme;component/MyCustomTheme.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

## Built-in Theme Options

Syncfusion provides multiple built-in themes for WPF controls.

### Available Themes

| Theme Name | Description | NuGet Package |
|------------|-------------|---------------|
| **FluentLight** | Modern light theme based on Microsoft Fluent | Syncfusion.Themes.FluentLight.WPF |
| **FluentDark** | Modern dark theme | Syncfusion.Themes.FluentDark.WPF |
| **MaterialLight** | Google Material Design light | Syncfusion.Themes.MaterialLight.WPF |
| **MaterialDark** | Google Material Design dark | Syncfusion.Themes.MaterialDark.WPF |
| **Office2019Colorful** | Microsoft Office 2019 style | Syncfusion.Themes.Office2019Colorful.WPF |
| **Office2019Black** | Office 2019 dark theme | Syncfusion.Themes.Office2019Black.WPF |
| **Office2019White** | Office 2019 light theme | Syncfusion.Themes.Office2019White.WPF |
| **MaterialLightBlue** | Material with blue accent | Syncfusion.Themes.MaterialLightBlue.WPF |
| **MaterialDarkBlue** | Material dark with blue | Syncfusion.Themes.MaterialDarkBlue.WPF |

### Installation

```powershell
# Install specific theme
Install-Package Syncfusion.Themes.FluentLight.WPF

# Or install multiple themes
Install-Package Syncfusion.Themes.FluentDark.WPF
Install-Package Syncfusion.Themes.MaterialLight.WPF
```

### Using Multiple Themes

**Theme Selector:**

```csharp
public void ApplyTheme(string themeName)
{
    switch (themeName)
    {
        case "FluentLight":
            SfSkinManager.SetTheme(this, new Theme("FluentLight"));
            break;
        case "FluentDark":
            SfSkinManager.SetTheme(this, new Theme("FluentDark"));
            break;
        case "MaterialLight":
            SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
            break;
        case "MaterialDark":
            SfSkinManager.SetTheme(this, new Theme("MaterialDark"));
            break;
        default:
            SfSkinManager.SetTheme(this, new Theme("FluentLight"));
            break;
    }
}
```

**Theme Switcher UI:**

```xaml
<syncfusion:ToolBarAdv ToolBarName="Settings">
    <Label Content="Theme:"/>
    <ComboBox Width="120" SelectionChanged="ThemeComboBox_SelectionChanged">
        <ComboBoxItem Content="Fluent Light" IsSelected="True"/>
        <ComboBoxItem Content="Fluent Dark"/>
        <ComboBoxItem Content="Material Light"/>
        <ComboBoxItem Content="Material Dark"/>
    </ComboBox>
</syncfusion:ToolBarAdv>
```

**Code-behind:**

```csharp
private void ThemeComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    ComboBox comboBox = sender as ComboBox;
    string selectedTheme = (comboBox.SelectedItem as ComboBoxItem).Content.ToString();
    
    string themeName = selectedTheme.Replace(" ", "");
    SfSkinManager.SetTheme(this, new Theme(themeName));
}
```

## Complete Styling Example

```xaml
<Window x:Class="CustomToolBarStyling.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        Title="Custom Toolbar Styling" Height="500" Width="800">
    
    <!-- Apply global theme -->
    <syncfusionskin:SfSkinManager.Theme>
        <syncfusionskin:Theme ThemeName="FluentDark"/>
    </syncfusionskin:SfSkinManager.Theme>
    
    <Window.Resources>
        <!-- Custom floating style -->
        <Style x:Key="CustomFloatingStyle" TargetType="Window">
            <Setter Property="Background" Value="#2D2D30"/>
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="BorderBrush" Value="#007ACC"/>
            <Setter Property="BorderThickness" Value="2"/>
            <Setter Property="Effect">
                <Setter.Value>
                    <DropShadowEffect Color="Black" BlurRadius="15" ShadowDepth="5" Opacity="0.7"/>
                </Setter.Value>
            </Setter>
        </Style>
    </Window.Resources>
    
    <syncfusion:ToolBarManager>
        <syncfusion:ToolBarManager.TopToolBarTray>
            <syncfusion:ToolBarTrayAdv>
                <!-- Styled toolbar -->
                <syncfusion:ToolBarAdv ToolBarName="File"
                                       Foreground="White"
                                       Background="#2D2D30"
                                       FloatingToolBarStyle="{StaticResource CustomFloatingStyle}">
                    
                    <!-- Custom button styles -->
                    <syncfusion:ToolBarAdv.ControlsResourceDictionary>
                        <ResourceDictionary>
                            <Style TargetType="Button">
                                <Setter Property="Background" Value="Transparent"/>
                                <Setter Property="Foreground" Value="White"/>
                                <Setter Property="BorderThickness" Value="0"/>
                                <Setter Property="Padding" Value="10,5"/>
                                <Setter Property="Margin" Value="2"/>
                                <Style.Triggers>
                                    <Trigger Property="IsMouseOver" Value="True">
                                        <Setter Property="Background" Value="#3E3E40"/>
                                    </Trigger>
                                    <Trigger Property="IsPressed" Value="True">
                                        <Setter Property="Background" Value="#007ACC"/>
                                    </Trigger>
                                </Style.Triggers>
                            </Style>
                        </ResourceDictionary>
                    </syncfusion:ToolBarAdv.ControlsResourceDictionary>
                    
                    <Button Content="New"/>
                    <Button Content="Open"/>
                    <Button Content="Save"/>
                </syncfusion:ToolBarAdv>
            </syncfusion:ToolBarTrayAdv>
        </syncfusion:ToolBarManager.TopToolBarTray>
        
        <syncfusion:ToolBarManager.Content>
            <Grid Background="#1E1E1E">
                <TextBox Background="#252526" 
                         Foreground="White" 
                         BorderBrush="#3F3F46"
                         TextWrapping="Wrap" 
                         AcceptsReturn="True"/>
            </Grid>
        </syncfusion:ToolBarManager.Content>
    </syncfusion:ToolBarManager>
</Window>
```

## Best Practices

### Theme Consistency

1. **Apply theme at Window level** for consistent appearance
2. **Use SfSkinManager** for Syncfusion controls
3. **Match custom styles** to selected theme colors

### Performance

1. **Define styles in Resources** rather than inline
2. **Reuse ResourceDictionary** across toolbars
3. **Avoid complex ControlTemplates** for toolbar buttons

### Visual Hierarchy

1. **Use Foreground** to indicate importance (red for delete, green for success)
2. **Background variations** for different toolbar types
3. **Border/Shadow effects** sparingly for floating toolbars

### Accessibility

1. **Maintain contrast ratio** (WCAG AA: 4.5:1 for text)
2. **Test with high contrast themes**
3. **Provide focus indicators** (keyboard navigation)

### Theme Switching

```csharp
// ✓ GOOD - Apply at window level
SfSkinManager.SetTheme(this, new Theme("FluentDark"));

// ✗ BAD - Individual control theming (inconsistent)
SfSkinManager.SetTheme(toolBar1, new Theme("FluentLight"));
SfSkinManager.SetTheme(toolBar2, new Theme("MaterialDark"));
```

## Related Topics

- **Getting Started**: Basic toolbar setup → [getting-started.md](getting-started.md)
- **Toolbar States**: FloatingToolBarStyle usage → [toolbar-states.md](toolbar-states.md)
- **ToolBarManager**: Layout and positioning → [toolbar-manager.md](toolbar-manager.md)
