# SfSkinManager Setup and Basic Theme Application

## Overview

The **SfSkinManager** is the central component for applying themes to both Syncfusion and .NET Framework controls in WPF applications. It provides two approaches: XAML attached properties for declarative theme application and C# code for programmatic control.

## Adding SfSkinManager References

Before applying themes, add the required NuGet package or assembly reference:

### Step 1: Add SfSkinManager Assembly Reference

The `Syncfusion.SfSkinManager.WPF` package is required for all theming functionality.

**Via NuGet Package Manager:**
```
Install-Package Syncfusion.SfSkinManager.WPF
```

**Via Visual Studio UI:**
1. Right-click project → "Manage NuGet Packages"
2. Search for `Syncfusion.SfSkinManager.WPF`
3. Click Install

### Step 2: Import Syncfusion Namespace in XAML

Add the SfSkinManager namespace to your XAML files:

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"/>
```

### Step 3: Add Theme Assembly Reference

Add the specific theme assembly you want to use. For example, to use `Windows11Light` theme:

```
Install-Package Syncfusion.Themes.Windows11Light.WPF
```

## Applying a Theme to a Control

### Using XAML Attached Property

Apply theme to a Window and all child controls:

```xaml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=Windows11Light}"
        Title="Theme Example" Height="450" Width="800">
    <Grid>
        <syncfusion:SfDataGrid x:Name="grid" 
                               ItemsSource="{Binding Items}"/>
    </Grid>
</Window>
```

**Theme Inheritance:** When you apply a theme to a Window, it automatically cascades to all child elements (panels, buttons, data grids, etc.).

### Using C# Code-Behind

Apply theme programmatically after window initialization:

```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        // IMPORTANT: Enable theme as default style (required for Framework controls)
        SfSkinManager.ApplyThemeAsDefaultStyle = true;
        
        InitializeComponent();
        
        // Apply theme to this window
        SfSkinManager.SetTheme(this, new Theme("Windows11Light"));
    }
}
```

**Important:** The `ApplyThemeAsDefaultStyle` property must be set to `true` before applying themes to ensure Framework controls (Button, TextBox, etc.) are themed correctly.

## Applying Theme to Specific Controls

Apply theme to individual controls without affecting the entire window:

### XAML Approach
```xaml
<syncfusion:SfDataGrid 
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentDark}"
    ItemsSource="{Binding Items}"/>
```

### C# Approach
```csharp
// Apply theme to specific grid control
SfSkinManager.SetTheme(dataGrid, new Theme("FluentDark"));

// Apply theme to specific button
SfSkinManager.SetTheme(myButton, new Theme("Windows11Dark"));
```

## Applying Theme Globally to Application

To apply a theme to **all windows** in your application automatically, use `ApplicationTheme`:

### In App.xaml.cs
```csharp
using Syncfusion.SfSkinManager;

public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // IMPORTANT: Set before any windows are created
        SfSkinManager.ApplyThemeAsDefaultStyle = true;
        
        // Set global application theme
        SfSkinManager.ApplicationTheme = new Theme("Fluent
Dark");
        
        base.OnStartup(e);
    }
}
```

**Timing Consideration:** Set `ApplicationTheme` in `OnStartup` or in the `App` class constructor before any windows are instantiated. This ensures the theme applies to all new windows automatically.

### Alternative: In MainWindow Constructor
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        // Set global theme before InitializeComponent
        SfSkinManager.ApplyThemeAsDefaultStyle = true;
        SfSkinManager.ApplicationTheme = new Theme("Windows11Light");
        
        InitializeComponent();
    }
}
```

**Benefit:** Once set globally, you don't need to apply the theme individually to each window or control.

## Theme Application Flow

```
1. Enable ApplyThemeAsDefaultStyle = true
   ↓
2. Set theme using one of:
   - Window XAML: syncfusionskin:SfSkinManager.Theme
   - C#: SfSkinManager.SetTheme(control, theme)
   - Global: SfSkinManager.ApplicationTheme
   ↓
3. Theme cascades to all child controls
   ↓
4. Syncfusion + Framework controls render with theme styles
```

## Complete Example

Create a WPF application with global theming:

**App.xaml.cs:**
```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        SfSkinManager.ApplyThemeAsDefaultStyle = true;
        SfSkinManager.ApplicationTheme = new Theme("Windows11Light");
        base.OnStartup(e);
    }
}
```

**MainWindow.xaml:**
```xaml
<Window x:Class="ThemeDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="WPF Theme Demo" Height="450" Width="800">
    <Grid>
        <StackPanel>
            <Button Content="Standard Button" Margin="10"/>
            <syncfusion:SfButton Content="Syncfusion Button" Margin="10"/>
            <TextBox Text="Standard TextBox" Margin="10"/>
            <syncfusion:SfDataGrid ItemsSource="{Binding Items}" Margin="10"/>
        </StackPanel>
    </Grid>
</Window>
```

**MainWindow.xaml.cs:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Optional: Override global theme for this window
        // SfSkinManager.SetTheme(this, new Theme("FluentDark"));
    }
}
```

## Best Practices

1. **Set ApplyThemeAsDefaultStyle early** - Before any window initialization
2. **Use global ApplicationTheme for consistency** - Unless you need per-window themes
3. **Include proper namespace imports** - Both SfSkinManager and Syncfusion schemas
4. **Add theme assembly references** - Before using specific theme names
5. **Theme inheritance** - Rely on automatic cascading to child controls; avoid redundant theme application
6. **Framework control styling** - Ensure ApplyThemeAsDefaultStyle is true for standard WPF controls to be themed

## Common Issues and Solutions

| Issue | Solution |
|-------|----------|
| Framework controls not themed | Set `ApplyThemeAsDefaultStyle = true` before applying theme |
| Theme not applied | Verify theme assembly is referenced (via NuGet) |
| Child controls not inheriting theme | Ensure theme is applied to parent (Window or Container) |
| Namespace not recognized | Check XAML namespace imports are correct |
| Multiple themes conflicting | Apply only one theme per control/window |
