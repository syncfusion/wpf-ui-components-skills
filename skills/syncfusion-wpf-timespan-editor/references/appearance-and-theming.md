# Appearance & Theming

## Table of Contents
- [Background & Selection Colors](#background--selection-colors)
- [Foreground Text Color](#foreground-text-color)
- [Flow Direction & RTL](#flow-direction--rtl)
- [Built-In Themes](#built-in-themes)
- [SfSkinManager Integration](#sfskinmanager-integration)
- [ThemeStudio Custom Themes](#themestudio-custom-themes)

---

## Background & Selection Colors

Customize the control's background color and the highlight color used when selecting/editing fields.

**Default Colors:**
- **Background:** White
- **SelectionBrush:** Royal Blue (field highlight)

**XAML Example:**

```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Background="Yellow"
                         SelectionBrush="Red" />
```

**C# Code Example:**

```csharp
using System.Windows.Media;

TimeSpanEdit editor = new TimeSpanEdit();
editor.Value = new TimeSpan(5, 12, 30, 45);
editor.Background = Brushes.Yellow;
editor.SelectionBrush = Brushes.Red;
```

**Predefined Colors:**

```csharp
Brushes.White
Brushes.Black
Brushes.Red
Brushes.Green
Brushes.Blue
Brushes.Yellow
Brushes.Cyan
Brushes.Magenta
Brushes.Gray
Brushes.LightGray
Brushes.DarkGray
```

**Custom Solid Color Brush:**

```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Background="#F0F0F0"
                         SelectionBrush="#2196F3" />
```

```csharp
editor.Background = new SolidColorBrush(Color.FromArgb(255, 240, 240, 240));
editor.SelectionBrush = new SolidColorBrush(Color.FromArgb(255, 33, 150, 243));
```

**Practical Color Schemes:**

*Light Theme (Default):*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Background="White"
                         SelectionBrush="#2196F3"
                         Foreground="Black" />
```

*Dark Theme:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Background="#2D2D2D"
                         SelectionBrush="#4CAF50"
                         Foreground="White" />
```

*High Contrast (Accessibility):*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Background="White"
                         SelectionBrush="Black"
                         Foreground="Black" />
```

*Professional Blue:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Background="#F5F5F5"
                         SelectionBrush="#1976D2"
                         Foreground="#333333" />
```

---

## Foreground Text Color

Control the text color of the displayed time value.

**Default Color:** Black

**XAML Example:**

```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Foreground="Red" />
```

**C# Code Example:**

```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.Foreground = Brushes.Red;
```

**Color Combinations:**

*Error State (Red text):*
```xml
<syncfusion:TimeSpanEdit Value="25.00:00:00"
                         Foreground="Red"
                         Background="#FFE0E0"
                         SelectionBrush="DarkRed" />
```

*Success State (Green text):*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Foreground="Green"
                         Background="#E0FFE0"
                         SelectionBrush="DarkGreen" />
```

*Warning State (Orange text):*
```xml
<syncfusion:TimeSpanEdit Value="12.00:00:00"
                         Foreground="Orange"
                         Background="#FFF3E0"
                         SelectionBrush="OrangeRed" />
```

*Info State (Blue text):*
```xml
<syncfusion:TimeSpanEdit Value="3.08:30:00"
                         Foreground="DodgerBlue"
                         Background="#E3F2FD"
                         SelectionBrush="CornflowerBlue" />
```

**Dynamic Color Changes (C#):**

```csharp
private void UpdateStatusColors(TimeSpan value) {
    if (value.TotalHours > 12) {
        // Warning
        editor.Foreground = Brushes.Orange;
        editor.Background = new SolidColorBrush(Colors.FloralWhite);
    }
    else if (value.TotalHours > 8) {
        // Normal
        editor.Foreground = Brushes.Black;
        editor.Background = Brushes.White;
    }
    else {
        // Low
        editor.Foreground = Brushes.Green;
        editor.Background = new SolidColorBrush(Colors.LightGreen);
    }
}
```

---

## Flow Direction & RTL

Support right-to-left (RTL) text layout for languages like Arabic and Hebrew.

**Default Direction:** Left-to-Right (LTR)

**XAML Example - RTL:**

```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         FlowDirection="RightToLeft" />
```

**XAML Example - LTR (default):**

```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         FlowDirection="LeftToRight" />
```

**C# Code Example:**

```csharp
using System.Windows;

TimeSpanEdit editor = new TimeSpanEdit();
editor.Value = new TimeSpan(5, 12, 30, 45);

// Enable RTL
editor.FlowDirection = FlowDirection.RightToLeft;

// Reset to LTR
editor.FlowDirection = FlowDirection.LeftToRight;
```

**Complete RTL Example:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="RTLDemo.MainWindow"
        Title="RTL Demo"
        FlowDirection="RightToLeft"
        Height="300"
        Width="400">
    <Grid>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <TextBlock Text="المدة الزمنية:"
                       FontSize="14"
                       Margin="0,0,0,10" />
            
            <syncfusion:TimeSpanEdit x:Name="timeSpanEdit"
                                     Value="5.12:30:45"
                                     FlowDirection="RightToLeft"
                                     Width="150"
                                     Height="35" />
        </StackPanel>
    </Grid>
</Window>
```

**When to Use RTL:**

- Applications targeting Arabic-speaking regions
- Hebrew language support
- Bidirectional text applications
- Localized WPF applications for RTL languages

---

## Built-In Themes

Syncfusion TimeSpanEdit supports predefined themes that apply coordinated color schemes.

**Available Themes:**
- Default
- Light (White/Light Gray)
- Dark
- Office2016White
- Office2016Black
- Office2016Colorful
- VisualStudio2015
- VisualStudioDark
- MaterialLight
- MaterialDark
- FluentLight
- FluentDark

**Applying Themes via SfSkinManager:**

The easiest way to apply themes is using `SfSkinManager`.

**XAML Example:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="ThemeDemo.MainWindow"
        Title="Theme Demo"
        Height="300"
        Width="400">
    
    <Grid>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <TextBlock Text="Duration (MaterialDark Theme):" 
                       FontSize="14" 
                       Margin="0,0,0,10" />
            
            <syncfusion:TimeSpanEdit x:Name="timeSpanEdit"
                                     Value="5.12:30:45"
                                     syncfusion:SfSkinManager.VisualStyle="MaterialDark"
                                     Width="150"
                                     Height="35" />
        </StackPanel>
    </Grid>
</Window>
```

**C# Code Example:**

```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        // Apply theme to the entire window
        SfSkinManager.SetVisualStyle(this, VisualStyles.MaterialDark);
        
        // Or apply theme to specific control
        SfSkinManager.SetVisualStyle(timeSpanEdit, VisualStyles.FluentLight);
    }
}
```

**Theme Combinations (XAML):**

*Office 2016 Black:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         syncfusion:SfSkinManager.VisualStyle="Office2016Black" />
```

*Material Light:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         syncfusion:SfSkinManager.VisualStyle="MaterialLight" />
```

*Fluent Dark:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         syncfusion:SfSkinManager.VisualStyle="FluentDark" />
```

*Visual Studio Dark:*
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         syncfusion:SfSkinManager.VisualStyle="VisualStudioDark" />
```

---

## SfSkinManager Integration

`SfSkinManager` is Syncfusion's built-in theme manager for applying and managing visual styles.

**Basic Setup:**

```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        // Apply theme to window (affects all controls)
        SfSkinManager.SetVisualStyle(this, VisualStyles.FluentLight);
    }
}
```

**Apply Theme to Individual Control:**

```csharp
// Apply MaterialDark theme to specific TimeSpanEdit
SfSkinManager.SetVisualStyle(timeSpanEdit, VisualStyles.MaterialDark);
```

**All Available Visual Styles (Enum):**

```csharp
VisualStyles.Default              // Default Syncfusion theme
VisualStyles.Office2016White      // White Office 2016 style
VisualStyles.Office2016Black      // Black Office 2016 style
VisualStyles.Office2016Colorful   // Colorful Office 2016 style
VisualStyles.VisualStudio2015     // Visual Studio 2015 Light
VisualStyles.VisualStudioDark     // Visual Studio Dark
VisualStyles.MaterialLight        // Material Design Light
VisualStyles.MaterialDark         // Material Design Dark
VisualStyles.FluentLight          // Microsoft Fluent Light
VisualStyles.FluentDark           // Microsoft Fluent Dark
```

**Dynamic Theme Switching (C#):**

```csharp
private void DarkModeButton_Click(object sender, RoutedEventArgs e) {
    SfSkinManager.SetVisualStyle(this, VisualStyles.MaterialDark);
}

private void LightModeButton_Click(object sender, RoutedEventArgs e) {
    SfSkinManager.SetVisualStyle(this, VisualStyles.MaterialLight);
}
```

**Complete Example with Theme Selector:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="ThemeSwitcher.MainWindow"
        Title="Theme Switcher"
        Height="350"
        Width="500">
    <Grid>
        <StackPanel Margin="20" VerticalAlignment="Center">
            <TextBlock Text="Select a Theme:" 
                       FontSize="16" 
                       FontWeight="Bold"
                       Margin="0,0,0,15" />
            
            <ComboBox x:Name="themeCombo"
                      SelectionChanged="ThemeCombo_SelectionChanged"
                      Margin="0,0,0,20"
                      Width="200"
                      Height="35">
                <ComboBoxItem>MaterialLight</ComboBoxItem>
                <ComboBoxItem>MaterialDark</ComboBoxItem>
                <ComboBoxItem>FluentLight</ComboBoxItem>
                <ComboBoxItem>FluentDark</ComboBoxItem>
                <ComboBoxItem>Office2016Black</ComboBoxItem>
                <ComboBoxItem>VisualStudioDark</ComboBoxItem>
            </ComboBox>
            
            <TextBlock Text="Duration Input:" 
                       FontSize="14"
                       Margin="0,0,0,10" />
            
            <syncfusion:TimeSpanEdit x:Name="timeSpanEdit"
                                     Value="5.12:30:45"
                                     Width="200"
                                     Height="35" />
        </StackPanel>
    </Grid>
</Window>
```

```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
    }
    
    private void ThemeCombo_SelectionChanged(object sender, SelectionChangedEventArgs e) {
        string selectedTheme = themeCombo.SelectedItem as string;
        
        // Map string to VisualStyles enum
        VisualStyles style = selectedTheme switch {
            "MaterialLight" => VisualStyles.MaterialLight,
            "MaterialDark" => VisualStyles.MaterialDark,
            "FluentLight" => VisualStyles.FluentLight,
            "FluentDark" => VisualStyles.FluentDark,
            "Office2016Black" => VisualStyles.Office2016Black,
            "VisualStudioDark" => VisualStyles.VisualStudioDark,
            _ => VisualStyles.MaterialLight
        };
        
        SfSkinManager.SetVisualStyle(this, style);
    }
}
```

---

## ThemeStudio Custom Themes

For advanced customization, use **ThemeStudio** to create custom themes.

**What is ThemeStudio?**
ThemeStudio is a web-based tool for creating custom Syncfusion themes without writing code.

**Access ThemeStudio:**
1. Visit: [Syncfusion ThemeStudio](https://themebuilder.syncfusion.com)
2. Select "Syncfusion WPF" platform
3. Customize colors, fonts, and component styles
4. Export the theme file

**Applying Custom Theme:**

After exporting from ThemeStudio, you get a theme resource dictionary (`.xaml` file).

**XAML Setup with Custom Theme:**

```xml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="CustomThemeApp.App"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
        <!-- Merge custom theme -->
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="pack://application:,,,/Syncfusion.SfSkinManager.WPF;component/Themes/MaterialLight.xaml" />
                <!-- Or your custom theme file -->
                <ResourceDictionary Source="CustomTheme.xaml" />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

**C# Application Setup with Custom Theme:**

```csharp
using Syncfusion.SfSkinManager;

public partial class App : Application {
    public App() {
        // Register custom theme
        ThemeSettings.RegisterTheme("CustomTheme", "pack://application:,,,/Themes/CustomTheme.xaml");
        
        // Apply custom theme
        SfSkinManager.SetVisualStyle(MainWindow, "CustomTheme");
    }
}
```

**ThemeStudio Customization Tips:**

1. **Color Palette:** Adjust primary, accent, and background colors
2. **Typography:** Change fonts and sizes for headers and text
3. **Component-Specific:** Customize TimeSpanEdit colors, borders, hover effects
4. **Export Options:** Choose between XAML or theme package format

**Example Custom Theme Scenario:**

You want a corporate branding theme with:
- Primary Color: Company brand color (e.g., #0066CC)
- Accent Color: Complementary color (e.g., #FF9933)
- Font: Company standard font

Use ThemeStudio to define these, export, and apply to your entire application.

---

## Styling Best Practices

**Scenario 1: Display-Only Control (No Editing)**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         IsReadOnly="True"
                         Background="#F5F5F5"
                         Foreground="#333333"
                         syncfusion:SfSkinManager.VisualStyle="FluentLight" />
```

**Scenario 2: Data Entry Form (Editable)**
```xml
<syncfusion:TimeSpanEdit Value="0.08:00:00"
                         Format="h 'hrs' m 'mins'"
                         Background="White"
                         SelectionBrush="#2196F3"
                         Foreground="Black"
                         syncfusion:SfSkinManager.VisualStyle="MaterialLight" />
```

**Scenario 3: Error State**
```xml
<syncfusion:TimeSpanEdit Value="25.00:00:00"
                         Foreground="Red"
                         Background="#FFE0E0"
                         SelectionBrush="DarkRed"
                         syncfusion:SfSkinManager.VisualStyle="FluentLight" />
```

**Scenario 4: High Contrast (Accessibility)**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Background="White"
                         Foreground="Black"
                         SelectionBrush="Black"
                         syncfusion:SfSkinManager.VisualStyle="VisualStudioDark" />
```

---

## Next Steps

1. **Review all interactions** — Check [user-interactions.md](user-interactions.md) for keyboard/mouse controls
2. **Add value validation** — Read [constraints-and-events.md](constraints-and-events.md) for events
3. **Customize formats** — See [value-and-formats.md](value-and-formats.md) for display options
