# Creating and Customizing Themes

## Table of Contents
- [Overview](#overview)
- [Customizing Theme Colors Programmatically](#customizing-theme-colors-programmatically)
- [Exporting Theme Projects](#exporting-theme-projects)
- [Generating Theme Assemblies](#generating-theme-assemblies)
- [Integrating Custom Themes](#integrating-custom-themes)
- [Multi-Framework SDK-Style Projects](#multi-framework-sdk-style-projects)
- [Troubleshooting Custom Themes](#troubleshooting-custom-themes)

## Overview

Customize Syncfusion WPF themes in two ways:
1. **Theme Studio (Visual):** Use Theme Studio to graphically modify colors and fonts, then export as a complete theme project. After exporting from Theme Studio, follow the steps in this document.
2. **Programmatic (Code):** Modify specific theme colors and fonts at runtime using theme settings classes

Both approaches allow you to create branded themes that match your application's design guidelines.

## Customizing Theme Colors Programmatically

Modify theme colors at runtime without exporting a project:

### Step 1: Create Theme Settings Instance

Each theme has a corresponding settings class with the pattern `{ThemeName}ThemeSettings`:

```csharp
using Syncfusion.Themes.Windows11Light.WPF;
using Syncfusion.Themes.FluentDark.WPF;
using Syncfusion.SfSkinManager;

// Create settings instance for Windows 11 Light
var windows11Settings = new Windows11LightThemeSettings();

// Create settings instance for Fluent Dark
var fluentSettings = new FluentDarkThemeSettings();
```

### Step 2: Customize Palette

Access the palette property and modify colors:

```csharp
using Syncfusion.Themes.Windows11Light.WPF;
using Syncfusion.SfSkinManager;

var themeSettings = new Windows11LightThemeSettings();

// Create and customize palette
var palette = new Windows11Palette
{
    PrimaryColor = Colors.Blue,
    SecondaryColor = Colors.LightBlue,
    BackgroundColor = Colors.White,
    ForegroundColor = Colors.Black,
    ErrorColor = Colors.Red,
    WarningColor = Colors.Orange,
    SuccessColor = Colors.Green
};

// Assign palette to theme settings
themeSettings.Palette = palette;
```

### Step 3: Register Theme Settings

Register the customized settings before applying the theme:

```csharp
// Register customized theme settings
SfSkinManager.RegisterThemeSettings("Windows11Light", themeSettings);

// Now apply the theme (will use custom settings)
SfSkinManager.SetTheme(this, new Theme("Windows11Light"));
```

**Important:** Register theme settings before setting the theme.

### Step 4: Apply to Application

Example in App.xaml.cs:

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Enable theme as default
        SfSkinManager.ApplyThemeAsDefaultStyle = true;

        // Create and customize theme settings
        var themeSettings = new Windows11LightThemeSettings();
        themeSettings.Palette = new Windows11Palette
        {
            PrimaryColor = Color.FromArgb(255, 0, 120, 215), // Brand Blue
            SecondaryColor = Color.FromArgb(255, 200, 220, 240)
        };

        // Register custom theme
        SfSkinManager.RegisterThemeSettings("Windows11Light", themeSettings);

        // Apply globally
        SfSkinManager.ApplicationTheme = new Theme("Windows11Light");

        base.OnStartup(e);
    }
}
```

### Complete Programmatic Customization Example

```csharp
private void CustomizeFluentDarkTheme()
{
    // Create theme settings
    var fluentSettings = new FluentDarkThemeSettings();

    // Define custom palette
    var customPalette = new FluentPalette
    {
        PrimaryColor = Colors.Purple,        // Your brand color
        SecondaryColor = Colors.LightPurple,
        BackgroundColor = Color.FromArgb(255, 20, 20, 20),  // Very dark background
        ForegroundColor = Colors.White,
        BorderColor = Colors.Gray,
        SurfaceColor = Color.FromArgb(255, 40, 40, 40),     // Elevated surface
        DisabledColor = Colors.DarkGray
    };

    fluentSettings.Palette = customPalette;

    // Register and apply
    SfSkinManager.RegisterThemeSettings("FluentDark", fluentSettings);
    SfSkinManager.SetTheme(this, new Theme("FluentDark"));
}
```

### Available Theme Settings Classes

Common theme settings classes follow the pattern `{ThemeName}ThemeSettings`:

- `Windows11LightThemeSettings` + `Windows11Palette`
- `Windows11DarkThemeSettings` + `Windows11Palette`
- `FluentLightThemeSettings` + `FluentPalette`
- `FluentDarkThemeSettings` + `FluentPalette`
- `Material3LightThemeSettings` + `Material3Palette`
- `Material3DarkThemeSettings` + `Material3Palette`
- `MaterialLightThemeSettings` + `MaterialPalette`
- `MaterialDarkThemeSettings` + `MaterialPalette`
- And more for Office 2019 and other themes...

## Exporting Theme Projects

After customizing your theme in Theme Studio and exporting it, the theme project will be generated with the following structure:

```
MyBrandTheme/
├── MyBrandTheme.csproj
├── packages.config (or .csproj NuGet references)
├── Properties/
├── Themes/
│   ├── MyBrandTheme.xaml
│   ├── [ControlSpecificStyles]/
│   └── [ColorResources]/
└── ...
```

## Generating Theme Assemblies

### Prerequisites

- Visual Studio 2019 or later
- .NET Framework 4.6.2 or .NET 8.0+
- Referenced Syncfusion theme base assemblies

### Build in Visual Studio

1. **Open Exported Project:**
   - Open the exported theme project in Visual Studio
   - The project file (`.csproj`) contains the theme code

2. **Configure Target Frameworks (SDK-style Projects):**
   - For multi-framework support (.NET 4.6.2, .NET 8.0, .NET 9.0, .NET 10.0):
   - Edit `.csproj` and verify `<TargetFrameworks>` tag includes only installed frameworks
   - If frameworks are missing, edit `targets\MultiTargeting.targets` to remove unavailable frameworks

   **Example .csproj snippet:**
   ```xml
   <PropertyGroup>
       <TargetFrameworks>net462;net8.0;net9.0;net10.0</TargetFrameworks>
       <GenerateDocumentationFile>true</GenerateDocumentationFile>
       <SignAssembly>true</SignAssembly>
       <AssemblyOriginatorKeyFile>ThemeStudio.snk</AssemblyOriginatorKeyFile>
   </PropertyGroup>
   ```

3. **Update Syncfusion Installation Path (if needed):**
   - If you see reference errors, update the Syncfusion installation path
   - Edit `.csproj` file and locate `<SyncfusionInstallLocation>` tag:
   ```xml
   <SyncfusionInstallLocation>
       C:\Program Files (x86)\Syncfusion\Essential Studio\WPF\{product version}
   </SyncfusionInstallLocation>
   ```
   - Replace `{product version}` with your installed Syncfusion version (e.g., `26.1.35`)

4. **Set Build Configuration to Release:**
   - Visual Studio: Configuration dropdown → Select **Release**
   - Reason: Theme assembly must be in Release mode for production use

5. **Build Project:**
   - Right-click project → **Build**
   - Or: Build menu → **Build [ProjectName]**
   - Watch for compilation errors (resolve any missing references)

6. **Locate Generated Assembly:**
   - Theme assembly is generated in: `bin\Release\{framework}\`
   - Look for `[ThemeName].dll` file
   - Example: `MyBrandTheme.dll` or `MyBrandTheme.net80.dll` (for .NET 8.0)

### Signing the Assembly (Best Practice)

The exported project includes a default `ThemeStudio.snk` key file for signing:

1. **Use Existing Key (Default):**
   - Exported projects include `ThemeStudio.snk`
   - Assembly is signed automatically during build

2. **Create New Key Pair (Optional):**
   - Visual Studio → Project Properties → Signing tab
   - Check "Sign the assembly"
   - Create new key file or browse existing

3. **Why Sign Assemblies?**
   - Required for strong-named assembly references in .NET Framework
   - Ensures assembly integrity
   - Recommended for production themes

## Integrating Custom Themes

### Step 1: Add Theme Assembly to Your Project

1. **Copy Generated DLL:**
   - Copy the generated theme assembly (`.dll` file) from exported project's `bin\Release\`
   - Paste into your application's project or a dedicated `Themes` folder

2. **Add Reference:**
   - Visual Studio: Right-click project → Add → Reference
   - Browse to the copied theme `.dll` file
   - Click Add

3. **Verify Reference:**
   - Project → Properties → References should list your custom theme assembly

### Step 2: Add SfSkinManager Reference

Ensure your project has `Syncfusion.SfSkinManager.WPF` NuGet package:

```powershell
Install-Package Syncfusion.SfSkinManager.WPF
```

### Step 3: Register and Apply Custom Theme

Use the `RegisterTheme` method to register your custom theme with SfSkinManager:

```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        SfSkinManager.ApplyThemeAsDefaultStyle = true;

        // Register custom theme
        // Format: "{CustomThemeName};{BaseThemeName}"
        RegisterAndApplyCustomTheme("MyBrandTheme", "Windows11Light");

        InitializeComponent();
    }

    private void RegisterAndApplyCustomTheme(string customThemeName, string baseThemeName)
    {
        try
        {
            // Get the SkinHelper class from custom theme assembly
            string skinHelperTypeName = $"Syncfusion.Themes.{customThemeName}.WPF.{customThemeName}SkinHelper";
            Type skinHelperType = Type.GetType(skinHelperTypeName);

            if (skinHelperType != null)
            {
                // Create instance of SkinHelper
                SkinHelper skinHelper = Activator.CreateInstance(skinHelperType) as SkinHelper;

                if (skinHelper != null)
                {
                    // Register with SfSkinManager
                    SfSkinManager.RegisterTheme(customThemeName, skinHelper);

                    // Apply custom theme with base theme fallback
                    SfSkinManager.SetTheme(this, new Theme($"{customThemeName};{baseThemeName}"));
                }
            }
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Failed to load custom theme: {ex.Message}");
        }
    }
}
```

### Step 4: Use Custom Theme in XAML (Optional)

After registration, apply in XAML:

```xaml
<Window
    xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MyBrandTheme;Windows11Light}">
    <!-- Your content -->
</Window>
```

## Multi-Framework SDK-Style Projects

### Overview

Starting with Syncfusion version 31.2.12, exported themes are SDK-style projects supporting multiple frameworks:
- .NET Framework 4.6.2
- .NET 8.0
- .NET 9.0
- .NET 10.0

This single project generates assemblies for all frameworks, eliminating the need for separate projects per framework.

### Handling Missing Frameworks

If some frameworks aren't installed on your machine:

1. **Locate `MultiTargeting.targets` file:**
   - Path: `{exported-theme-folder}\targets\MultiTargeting.targets`

2. **Edit `<TargetFrameworks>` tag:**
   ```xml
   <TargetFrameworks>net462;net80;net90;net100</TargetFrameworks>
   ```

3. **Remove unavailable frameworks:**
   ```xml
   <!-- If .NET 10 not installed: -->
   <TargetFrameworks>net462;net80;net90</TargetFrameworks>
   ```

4. **Save and rebuild project**

### Setting Default Framework

When opening exported projects, .NET Framework 4.6.2 is selected by default:
- Visual Studio dropdown (bottom right): Shows active target framework
- Switch frameworks by clicking the dropdown
- Build for specific framework: Right-click project → Build (builds all frameworks)

### Building for Specific Framework

In Visual Studio Package Manager Console:

```powershell
# Build for specific framework
dotnet build -c Release -f net80

# Build for all configured frameworks
dotnet build -c Release
```

## Troubleshooting Custom Themes

| Issue | Solution |
|-------|----------|
| Theme not recognized | Ensure theme assembly is referenced and `RegisterTheme` called before applying |
| "SkinHelper not found" | Verify custom theme assembly namespace and class name match registration code |
| Reference errors during build | Update `<SyncfusionInstallLocation>` in `.csproj` with correct installation path |
| Theme not applying to controls | Verify `ApplyThemeAsDefaultStyle = true` is set before applying theme |
| Missing controls in custom theme | Ensure all referenced Syncfusion assemblies are available on build machine |
| Multi-framework build fails | Edit `MultiTargeting.targets` to remove unsupported frameworks |
| Assembly version conflicts | Ensure custom theme uses same Syncfusion version as main application |
| XAML preview not updating | Rebuild project and close/reopen XAML designer |
