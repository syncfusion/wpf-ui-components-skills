# Customization and Styling in WPF Tabbed Window

## Table of Contents
- [Overview](#overview)
- [Applying Syncfusion Themes](#applying-syncfusion-themes)
- [Window Chrome Customization](#window-chrome-customization)
- [Tab Header Styling](#tab-header-styling)
- [Content Area Styling](#content-area-styling)
- [Template Customization](#template-customization)
- [Close Button Customization](#close-button-customization)
- [Best Practices](#best-practices)

## Overview

The Tabbed Window control provides extensive customization options for creating professional, branded applications. Customize:

- **Window appearance** - Chrome, borders, title bar
- **Tab styling** - Headers, colors, fonts, spacing
- **Content presentation** - Layout, margins, backgrounds
- **Interactive elements** - Close buttons, new tab button, hover effects
- **Visual feedback** - Drag-drop indicators, selection highlights
- **Theming** - Syncfusion themes and custom styles

This guide covers all customization aspects to create polished, professional applications.

## Applying Syncfusion Themes

Syncfusion provides built-in themes for consistent, professional appearance.

### Available Themes

- **Metro** - Modern, flat design
- **Office 2016 White** - Light, clean Office style
- **Office 2016 Colorful** - Vibrant Office colors
- **Office 2016 Black** - Dark Office theme
- **Office 2019 White** - Updated Office design
- **Visual Studio 2013** - Development tool styling
- **Visual Studio 2015** - Modern IDE theme
- **Blend** - Expression Blend inspired
- **Lime** - Fresh green accent
- **Saffron** - Warm orange accent

### Applying Theme via NuGet

**Step 1: Install Theme Package**

```powershell
Install-Package Syncfusion.Themes.Office2019White.WPF
```

**Step 2: Apply Theme in App.xaml**

```xml
<Application x:Class="MyApp.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="/Syncfusion.Themes.Office2019White.WPF;component/MSControl/MSControl.xaml"/>
                <ResourceDictionary Source="/Syncfusion.Themes.Office2019White.WPF;component/SfChromelessWindow.xaml"/>
                <ResourceDictionary Source="/Syncfusion.Themes.Office2019White.WPF;component/SfTabControl.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

**Step 3: Set Theme on Window**

```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    
    // Apply theme to this window
    SfSkinManager.SetTheme(this, new Theme("Office2019White"));
}
```



## Window Chrome Customization

Customize the chromeless window appearance.

### Basic Window Properties

```xml
<syncfusion:SfChromelessWindow 
    x:Class="MyApp.MainWindow"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    WindowType="Tabbed"
    Title="My Application"
    Height="600" 
    Width="900"
    MinHeight="400"
    MinWidth="600"
    Background="#F5F5F5"
    BorderBrush="#0078D4"
    BorderThickness="1">
    
    <!-- Content -->
</syncfusion:SfChromelessWindow>
```

### Chrome Color Customization

```xml
<syncfusion:SfChromelessWindow 
    WindowType="Tabbed"
    TitleBarBackground="#0078D4"
    TitleBarForeground="White"
    BorderBrush="#0078D4">
    
    <!-- Content -->
</syncfusion:SfChromelessWindow>
```

### Custom Title Bar Height

```xml
<syncfusion:SfChromelessWindow 
    WindowType="Tabbed"
    TitleBarHeight="35">
    
    <!-- Content -->
</syncfusion:SfChromelessWindow>
```

### Window Icon Customization

```xml
<syncfusion:SfChromelessWindow 
    Icon="/Images/app-icon.png"
    ShowIcon="True">
    
    <!-- Content -->
</syncfusion:SfChromelessWindow>
```

### Removing Window Controls

```xml
<syncfusion:SfChromelessWindow 
    ShowMinimizeButton="True"
    ShowMaximizeButton="True"
    ShowCloseButton="True"
    ShowHelpButton="False">
    
    <!-- Content -->
</syncfusion:SfChromelessWindow>
```

## Tab Header Styling

Customize the appearance of tab headers.

### Basic Tab Style

```xml
<syncfusion:SfTabControl>
    <syncfusion:SfTabControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfTabItem">
            <Setter Property="Height" Value="32"/>
            <Setter Property="MinWidth" Value="100"/>
            <Setter Property="MaxWidth" Value="200"/>
            <Setter Property="FontSize" Value="12"/>
            <Setter Property="FontFamily" Value="Segoe UI"/>
            <Setter Property="Foreground" Value="#333333"/>
            <Setter Property="Background" Value="Transparent"/>
            <Setter Property="Padding" Value="12,8"/>
        </Style>
    </syncfusion:SfTabControl.ItemContainerStyle>
</syncfusion:SfTabControl>
```

### Tab Hover and Selection Effects

Use Style.Triggers to apply hover colors, selected state styling, and multi-trigger combinations for IsSelected + IsMouseOver.

### Tab with Bottom Border

Use Border with BorderThickness 0,0,0,2 for bottom-only active indicator.

### Rounded Tab Corners

Use CornerRadius on Border in ControlTemplate.

### Tab Separator Lines

Add vertical Rectangle separator between tabs in ControlTemplate.

## Content Area Styling

Customize the content area where tab content is displayed.

### Content Background and Margins

```xml
<syncfusion:SfTabControl Background="White" Padding="10">
    <syncfusion:SfTabItem Header="Tab 1">
        <Border Background="#F9F9F9" 
               BorderBrush="#E0E0E0" 
               BorderThickness="1"
               CornerRadius="4"
               Padding="15">
            <TextBlock Text="Content with styled container"/>
        </Border>
    </syncfusion:SfTabItem>
</syncfusion:SfTabControl>
```

### Content Template for All Tabs

Use ContentTemplate to wrap content in Border with ScrollViewer for consistent styling.

## Template Customization

### Custom Header Template

Create DataTemplate with StackPanel or Grid to display icons, text, and indicators.

### Header with Icon and Modified Indicator

Use Grid with 3 columns: Icon, Title, Modified indicator dot. Bind indicator visibility to IsModified property.

## Close Button Customization

Customize close button appearance using HeaderTemplate with custom Button styling for hover effects.

### Best Practices

- Store consistent styles in App.xaml ResourceDictionary for reuse across windows
- Use VirtualizationMode for performance when rendering many tabs
- Ensure sufficient color contrast (WCAG AA) and clear focus indicators for accessibility
- Use StaticResource keys for theme switching and testing
- Document brand colors and style guidelines for team consistency
