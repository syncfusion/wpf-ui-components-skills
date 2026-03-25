# Configuration

## Table of Contents
- [Overview](#overview)
- [.NET Framework Compatibility](#net-framework-compatibility)
- [.NET Core and .NET Compatibility](#net-core-and-net-compatibility)
- [Control Dependencies](#control-dependencies)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)

## Overview

This reference covers framework compatibility, control dependencies, and Right-to-Left (RTL) support for Syncfusion WPF components. For localization and MVVM patterns, see the dedicated reference files: [localization.md](localization.md) and [patterns-and-practices.md](patterns-and-practices.md).

## Control Dependencies

Understanding control dependencies helps you add the correct assembly references to your project.

### How Dependencies Work

Each Syncfusion WPF control requires:
1. **Primary Assembly** - Contains the control itself
2. **Dependency Assemblies** - Required supporting libraries
3. **Syncfusion.Licensing.dll** - Required for all controls (from v16.2+)

### Common Dependency Assemblies

**Shared Dependencies:**
- `Syncfusion.Shared.WPF` - Common utilities and controls
- `Syncfusion.SfShared.WPF` - Shared SfControls utilities
- `Syncfusion.Licensing` - License validation (v16.2+)

### Example Control Dependencies

Below are assembly and NuGet package requirements for commonly used Syncfusion WPF controls.

#### SfDataGrid

**Assembly References:**
- Syncfusion.SfGrid.WPF
- Syncfusion.Data.WPF
- Syncfusion.Shared.WPF

**NuGet Package:**
- Syncfusion.SfGrid.WPF (includes dependencies automatically)

#### SfChart

**Assembly References:**
- Syncfusion.SfChart.WPF

**NuGet Package:**
- Syncfusion.SfChart.WPF

#### ButtonAdv

**Assembly References:**
- Syncfusion.Tools.WPF
- Syncfusion.Shared.WPF

**NuGet Package:**
- Syncfusion.Tools.WPF

#### Ribbon

**Assembly References:**
- Syncfusion.Tools.WPF
- Syncfusion.Shared.WPF

**NuGet Package:**
- Syncfusion.Tools.WPF

#### SfRichTextBoxAdv

**Assembly References:**
- Syncfusion.SfRichTextBoxAdv.WPF
- Syncfusion.Compression.Base
- Syncfusion.OfficeChart.Base
- Syncfusion.Shared.WPF
- For .NET 3.5/4.0: Syncfusion.DocIO.ClientProfile
- For .NET 4.5+: Syncfusion.DocIO.Base

**NuGet Package:**
- Syncfusion.SfRichTextBoxAdv.WPF

#### DockingManager

**Assembly References:**
- Syncfusion.Tools.WPF
- Syncfusion.Shared.WPF

**NuGet Package:**
- Syncfusion.Tools.WPF

### Finding Control Dependencies

**Option 1: Official Documentation (Recommended)**
- Visit [Control Dependencies Page](https://help.syncfusion.com/wpf/control-dependencies)
- Search for your control
- View complete list of required assemblies and NuGet package
- Check for export/converter assemblies if using export features

**Option 2: NuGet Package**
- Install NuGet package (automatically resolves dependencies)
- View Dependencies node in Solution Explorer
- All required assemblies are added automatically

**Option 3: Build and Resolve**
- Add control to XAML
- Build project
- Visual Studio shows missing assembly errors
- Add referenced assemblies based on error messages

### Best Practices

✓ **Use NuGet packages** - Automatic dependency resolution prevents version conflicts  
✓ **Keep versions consistent** - All Syncfusion assemblies must be the same version  
✓ **Check documentation first** - Before manually adding references  
✓ **Install export packages separately** - Excel/PDF export requires additional NuGet packages  
✓ **Test after adding** - Verify control works correctly  
✓ **Reference official docs** - For complete list of 80+ controls and their dependencies

## .NET Framework Compatibility

Syncfusion WPF controls support multiple .NET Framework versions.

### Version Compatibility Table

| Syncfusion Version | .NET 3.5 | .NET 4.0 | .NET 4.5 | .NET 4.6 | .NET 4.6.2+ |
|-------------------|----------|----------|----------|----------|-------------|
| Earlier versions | Yes | Yes | No | No | No |
| From 11.2 (2013 Vol2) | Yes | Yes | Yes | No | No |
| From 13.3 (2015 Vol3) | Yes | Yes | Yes | Yes | No |
| From 21.1 (2023 Vol1) | No | Yes | Yes | Yes | No |
| From 25.1 (2024 Vol1) | No | Yes | No | No | Yes |
| From 28.1 (2024 Vol4) | No | No | No | No | Yes |
| From 29.1 (2025 Vol1) | No | No | No | No | Yes |

### Current Support

**As of Syncfusion 28.1 (2024 Vol4):**
- **Supported**: .NET Framework 4.6.2 and above
- **Dropped**: .NET Framework 3.5, 4.0, 4.5, 4.6

### Setting Target Framework

**In Visual Studio:**
1. Right-click project → **Properties**
2. **Application** tab
3. **Target framework** dropdown
4. Select `.NET Framework 4.6.2` or higher

**In .csproj file:**

```xml
<PropertyGroup>
  <TargetFramework>net462</TargetFramework>
</PropertyGroup>
```

### Migration Recommendations

**From .NET Framework 4.0 to 4.6.2:**
- Generally straightforward upgrade
- Minimal code changes required
- Test thoroughly after migration

**From .NET Framework 3.5:**
- Requires more significant changes
- Update third-party dependencies
- Consider migrating to .NET 8/9/10 instead

## .NET Core and .NET Compatibility

Syncfusion supports modern .NET versions (.NET Core 3.1, .NET 8+).

### Version Compatibility Table

| Syncfusion Version | .NET 6.0 | .NET 7.0 | .NET 8.0 | .NET 9.0 | .NET 10.0 |
|-------------------|----------|----------|----------|----------|-----------|
| Earlier versions | No | No | No | No | No |
| From 20.4 (2022 Vol4) | Yes | No | No | No | No |
| From 21.1 (2023 Vol1) | Yes | Yes | No | No | No |
| From 23.2 (2023 Vol3 SP) | Yes | Yes | Yes | No | No |
| From 27.2 (2024 Vol3 SP) | Yes | Yes | Yes | Yes | No |
| From 29.1 (2025 Vol1) | No | No | Yes | Yes | No |
| From 31.2 (2025 Vol3 SP) | No | No | Yes | Yes | Yes |

### Important Notes

- ✓ All Syncfusion WPF controls support .NET Core/.NET except controls labeled as "classic"
- ✓ .NET 6+ provides better performance than .NET Framework
- ✓ .NET 8 is LTS (Long Term Support) - recommended for new projects

### Creating .NET 6+ WPF Project

**Via Visual Studio:**
1. Create new project
2. Select **WPF App** (not "WPF App (.NET Framework)")
3. Choose target framework (.NET 8.0, .NET 9.0, .NET 10.0, etc.)

**Project File (.csproj):**

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net8.0-windows</TargetFramework>
    <UseWPF>true</UseWPF>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Syncfusion.SfGrid.WPF" Version="25.1.35" />
  </ItemGroup>
</Project>
```

### Assembly References Location

**For .NET 8+:**
```
C:\Program Files (x86)\Syncfusion\Essential Studio\WPF\{version}\precompiledassemblies\net8.0
```

### Adding Controls in .NET 8+ Project

Same as .NET Framework - use NuGet packages (recommended):

```bash
dotnet add package Syncfusion.SfGrid.WPF
```

**XAML remains the same:**

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <syncfusion:SfDataGrid x:Name="dataGrid" />
</Window>
```

## Right-to-Left (RTL) Support

RTL support displays content from right-to-left for languages like Arabic, Hebrew, and Urdu.

### Enabling RTL

Set the `FlowDirection` property to `RightToLeft`:

**Window Level:**
```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        FlowDirection="RightToLeft">
    <!-- All controls inside will use RTL -->
</Window>
```

**Control Level:**
```xml
<syncfusion:SfDataGrid FlowDirection="RightToLeft" />
```

**Code-Behind (C#):**
```csharp
dataGrid.FlowDirection = FlowDirection.RightToLeft;
```

**Code-Behind (VB.NET):**
```vb
dataGrid.FlowDirection = FlowDirection.RightToLeft
```

### RTL with Localization

Combine RTL with localization for complete international support:

**App.xaml.cs:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        // Set Arabic culture
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("ar");
        
        // Set RTL flow direction
        this.FlowDirection = FlowDirection.RightToLeft;
        
        InitializeComponent();
    }
}
```

### All Syncfusion Controls Support RTL

All WPF Syncfusion controls support RTL based on `FlowDirection` property.

## Additional Resources

- **Syncfusion WPF Documentation:** [Welcome to Syncfusion Essential WPF](https://help.syncfusion.com/wpf/welcome-to-syncfusion-essential-wpf)
- **Control Dependencies Reference:** [Complete List of Control Dependencies](https://help.syncfusion.com/wpf/control-dependencies)
- **Right-to-Left Support:** [RTL Documentation](https://help.syncfusion.com/wpf/right-to-left)
- **.NET Framework Compatibility:** [System Requirements](https://help.syncfusion.com/wpf/installation-and-upgrade/system-requirements)

**Related References:**
- For localization setup, see [localization.md](localization.md)
- For MVVM patterns and practices, see [patterns-and-practices.md](patterns-and-practices.md)
