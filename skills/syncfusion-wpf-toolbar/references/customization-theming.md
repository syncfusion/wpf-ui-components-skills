# Customization and Theming

This guide covers customizing ToolBarAdv appearance through styling, themes, and visual properties.

## Table of Contents
- [FloatingToolBarStyle](#floatingtoolbarstyle)
- [Foreground Customization](#foreground-customization)
- [ControlsResourceDictionary](#controlsresourcedictionary)
- [SfSkinManager Theme Application](#sfskinmanager-theme-application)
- [ThemeStudio for Custom Themes](#themestudio-for-custom-themes)
- [Built-in Theme Options](#built-in-theme-options)

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

### Theme Switching

Apply the theme at Window level so all Syncfusion controls inherit it automatically:

```csharp
// Apply at window level — all ToolBarAdv controls inherit this
SfSkinManager.SetTheme(this, new Theme("FluentDark"));
```

````
