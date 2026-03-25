---
name: syncfusion-wpf-getting-started
description: Comprehensive guide for setting up and getting started with Syncfusion WPF components in Windows Presentation Foundation applications. Covers installation methods (web installer, NuGet, offline installer), system requirements verification, adding controls to projects, configuring themes and localization, and troubleshooting setup issues. Use this skill when users need help with installing Syncfusion WPF, configuring NuGet packages, upgrading versions, or resolving setup and configuration problems.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Getting Started

Complete guide for installing, configuring, and getting started with Syncfusion® WPF components in Windows Presentation Foundation applications. This skill covers everything from system requirements to adding your first control.

## When to Use This Skill

Use this skill when you need to:
- **Install Syncfusion WPF** components (web installer, offline installer, or NuGet packages)
- **Check system requirements** for Syncfusion WPF development
- **Add Syncfusion controls** to a WPF project (Designer, XAML, or code-behind)
- **Configure NuGet packages** using Package Manager, CLI, or Console
- **Upgrade Syncfusion** to a newer version or migrate from trial to licensed version
- **Set up localization** for multi-language support with .resx files
- **Configure right-to-left (RTL)** support for Arabic, Hebrew, Urdu, etc.
- **Implement MVVM patterns** with Syncfusion controls (commands, data binding, NotificationObject)
- **Troubleshoot installation** or setup issues
- **Understand dependencies** between Syncfusion assemblies
- **Configure compatibility** with .NET Framework or .NET Core/.NET 8+

This skill is your starting point for all Syncfusion WPF setup and configuration tasks.

## Overview

Syncfusion® Essential® Studio for WPF provides a comprehensive suite of UI controls for building modern Windows desktop applications. Getting started involves:

1. **Verifying system requirements** (Windows OS, Visual Studio, .NET Framework/.NET)
2. **Choosing an installation method** (Web Installer, Offline Installer, or NuGet)
3. **Installing or downloading** the components
4. **Adding controls** to your WPF project
5. **Configuring** references, themes, and licensing

The setup process varies based on whether you're using:
- **Full installer** (includes all controls, project templates, documentation)
- **NuGet packages** (lightweight, control-by-control installation)
- **Trial version** (30-day evaluation) or **Licensed version**

## Documentation and Navigation Guide

### System Requirements

📄 **Read:** [references/system-requirements.md](references/system-requirements.md)

Check this reference when you need to verify:
- Operating systems supported (Windows XP through Windows 11)
- Hardware requirements (processor, RAM, disk space)
- Development environment (Visual Studio 2015/2017/2019/2022)
- .NET Framework versions (4.0, 4.6.2) and .NET versions (8.0, 9.0, 10.0)
- Compatibility with your existing development setup

### Installation Methods

📄 **Read:** [references/installation-methods.md](references/installation-methods.md)

Comprehensive guide for all installation approaches:
- **Web Installer**: Download and install from Syncfusion.com (requires internet during installation)
- **Offline Installer**: Download full package for offline installation
- **Trial vs Licensed**: Differences and how to download each
- Account setup and license key generation
- Installation wizard walkthrough
- Unlock key requirements for trial installations

### Adding Controls to Projects

📄 **Read:** [references/adding-controls.md](references/adding-controls.md)

Learn all methods for adding Syncfusion controls to your WPF application:
- **Using Designer**: Drag-and-drop from Visual Studio Toolbox
- **Using XAML**: Add namespace and control declarations manually
- **Using Code-Behind**: Create controls programmatically in C# or VB.NET
- **Using Project Templates**: Create new projects with Syncfusion templates
- Assembly reference requirements
- Namespace declarations

### NuGet Package Management

📄 **Read:** [references/nuget-package-management.md](references/nuget-package-management.md)

Complete guide to working with Syncfusion NuGet packages:
- **Package Manager UI**: Visual interface for package management
- **Dotnet CLI**: Command-line installation with `dotnet add package`
- **Package Manager Console**: PowerShell-based package commands
- Configuring nuget.org as a package source
- Installing specific versions
- Updating packages
- Working without full installer (NuGet-only workflow)

### Upgrading and Migration

📄 **Read:** [references/upgrading.md](references/upgrading.md)

Upgrade Syncfusion to newer versions or migrate licensing:
- Upgrading to latest version from Syncfusion Control Panel
- Downloading and installing newer versions
- Volume releases vs Service Pack releases
- Upgrading from trial to licensed version (installer or NuGet)
- License key registration
- Version compatibility considerations

### Configuration

📄 **Read:** [references/configuration.md](references/configuration.md)

Framework compatibility and core configuration:
- **.NET Framework compatibility**: Working with different framework versions
- **.NET Core/.NET 8+ compatibility**: Modern .NET support
- **Control dependencies**: Understanding assembly relationships (6 common examples)
- **Right-to-left (RTL)**: Configuring RTL support for international applications

### Localization

📄 **Read:** [references/localization.md](references/localization.md)

Setting up multi-language support for Syncfusion WPF controls:
- **Changing application culture**: Setting CurrentUICulture
- **Creating .resx files**: Resource file setup for translations
- **Step-by-step localization**: Complete guide with examples
- **Common culture codes**: Supported languages and regions
- **Editing default strings**: Customizing English text
- **Troubleshooting**: Resolving localization issues

### Patterns and Practices

📄 **Read:** [references/patterns-and-practices.md](references/patterns-and-practices.md)

MVVM patterns and best practices for Syncfusion WPF development:
- **Getting started with MVVM**: Basic MVVM structure and setup
- **MVVM commands**: Using built-in commands (TabControlExt, etc.)
- **CommandParameter usage**: Passing data to command handlers
- **Command target**: Specifying command target elements
- **DataContext and data binding**: Two-way binding and ItemsSource
- **NotificationObject**: INotifyPropertyChanged implementation
- **Complete MVVM example**: Employee management with SfDataGrid
- **Theme management**: Applying themes and SfSkinManager
- **Performance best practices**: Optimization tips

### Troubleshooting

📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)

Solutions for common installation and setup issues:
- Installation errors and how to resolve them
- License registration problems
- Assembly reference issues ("Could not load file or assembly")
- NuGet package errors
- Version conflicts
- Visual Studio integration issues

## Quick Start Example

Here's the fastest way to get started with a Syncfusion WPF control:

### Option 1: Using NuGet (Recommended for Quick Start)

```powershell
# In Package Manager Console
Install-Package Syncfusion.SfGrid.WPF
```

```xml
<!-- In MainWindow.xaml -->
<Window x:Class="WpfApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Syncfusion WPF Demo" Height="450" Width="800">
    <Grid>
        <syncfusion:SfDataGrid x:Name="dataGrid" 
                               AutoGenerateColumns="True"
                               ItemsSource="{Binding Employees}"/>
    </Grid>
</Window>
```

### Option 2: Using Full Installer

1. Download and install from [Syncfusion.com](https://www.syncfusion.com/wpf-controls)
2. Open Visual Studio → Create new WPF project
3. Right-click project → Manage NuGet Packages → Browse "Syncfusion.SfGrid.WPF"
4. Or drag control from Toolbox (Syncfusion controls appear after installation)

### License Registration (Required for Trial/NuGet)

```csharp
// In App.xaml.cs or before InitializeComponent()
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

> **Note**: License registration is required if using trial installer or NuGet packages. Generate your license key from the [Syncfusion License & Downloads](https://www.syncfusion.com/account/downloads) page.

## Common Patterns

### Pattern 1: Installing via Web Installer

**When to use**: You have reliable internet and want the latest version with all features.

1. Visit [Syncfusion.com Downloads](https://www.syncfusion.com/downloads)
2. Select WPF platform
3. Download the web installer
4. Run installer (requires internet during installation)
5. Follow wizard to select components
6. Launch Visual Studio and start developing

### Pattern 2: Installing via NuGet (No Installer)

**When to use**: You want lightweight installation, control-by-control, or CI/CD environments.

```powershell
# Install specific controls as needed
Install-Package Syncfusion.SfGrid.WPF
Install-Package Syncfusion.SfChart.WPF
Install-Package Syncfusion.SfBusyIndicator.WPF
```

**Advantages**:
- No full installer required
- Install only the controls you need
- Easy version management per project
- Works great in CI/CD pipelines

**Note**: You'll need to register a license key in your application.

### Pattern 3: Creating a New Project with Syncfusion Template

**When to use**: Starting a new WPF application with Syncfusion components.

1. Install Syncfusion WPF from full installer
2. Visual Studio → File → New Project
3. Search for "Syncfusion WPF Application"
4. Select template → Configure project
5. Choose controls, theme, and language in Project Configuration Wizard
6. Project created with references, XAML, and licensing setup

### Pattern 4: Adding Control via Designer

**When to use**: Visual development, quick prototyping.

1. Ensure Syncfusion is installed (full installer)
2. Open your WPF project in Visual Studio
3. Open MainWindow.xaml in Designer view
4. Open Toolbox (View → Toolbox)
5. Search for control (e.g., "SfDataGrid")
6. Drag and drop onto the designer
7. Visual Studio automatically adds references and namespace

### Pattern 5: Upgrading from Trial to Licensed

**When to use**: Your trial period is ending or you've purchased a license.

**If using installer**:
1. Download licensed installer from [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Run installer (no need to uninstall trial)
3. Licensed version replaces trial

**If using NuGet packages**:
1. Generate license key from [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Replace trial license key in your code:

```csharp
// Replace trial key with licensed key
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSED_KEY");
```

## Installation Workflow Decision Tree

Use this decision tree to choose your installation approach:

```
Do you need all Syncfusion WPF controls?
├─ YES → Do you have reliable internet?
│   ├─ YES → Use Web Installer
│   └─ NO → Use Offline Installer
│
└─ NO → Do you need only a few specific controls?
    └─ YES → Use NuGet Packages
        ├─ Package Manager UI (visual approach)
        ├─ Package Manager Console (PowerShell commands)
        └─ Dotnet CLI (command-line for .NET Core/.NET 8+)
```

**Special cases**:
- **CI/CD environments**: Use NuGet packages with automated license registration
- **Enterprise deployments**: Use Offline Installer for consistent installations
- **Rapid prototyping**: Use Web Installer or NuGet with specific controls
- **Learning/evaluation**: Use Trial version (web or offline installer)

## Key Concepts

### Assembly References

Syncfusion controls are distributed across multiple assemblies. Common assemblies include:

- `Syncfusion.SfGrid.WPF.dll` - Data Grid controls
- `Syncfusion.SfChart.WPF.dll` - Charting controls
- `Syncfusion.SfShared.WPF.dll` - Shared functionality
- `Syncfusion.SfInput.WPF.dll` - Input controls
- `Syncfusion.Tools.WPF.dll` - Tool controls (Ribbon, etc.)

Each control may have dependencies on shared assemblies. NuGet handles these automatically; manual references require dependency understanding.

### Namespace Declarations

Standard namespace declaration for XAML:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

For code-behind, use specific namespaces:

```csharp
using Syncfusion.UI.Xaml.Grid;        // For SfDataGrid
using Syncfusion.UI.Xaml.Charts;      // For SfChart
using Syncfusion.Windows.Controls.Input; // For input controls
```

### License Key Management

**When license key is required**:
- Using trial installer
- Using NuGet packages from nuget.org

**When license key is NOT required**:
- Using licensed installer with assembly references from installation directory

**How to register**:

```csharp
// In App.xaml.cs constructor, before InitializeComponent()
public App()
{
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    InitializeComponent();
}
```

### Theme Configuration

Syncfusion provides multiple themes. To apply a theme:

```xml
<!-- In App.xaml -->
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Syncfusion.Themes.MaterialLight.WPF;component/MSControl/SfDataGrid.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

See the configuration reference for complete theme setup.

## Common Use Cases

### Use Case 1: New Developer Evaluating Syncfusion

**Goal**: Try Syncfusion WPF controls with minimal setup.

**Approach**:
1. Check [system requirements](references/system-requirements.md)
2. Download [trial web installer](references/installation-methods.md)
3. Install with default options
4. Create project using [Syncfusion WPF Application template](references/adding-controls.md)
5. Explore controls from Toolbox

### Use Case 2: Adding Syncfusion to Existing Project

**Goal**: Add specific Syncfusion controls to an existing WPF application.

**Approach**:
1. Install control via [NuGet](references/nuget-package-management.md):
   ```powershell
   Install-Package Syncfusion.SfGrid.WPF
   ```
2. Register license key in App.xaml.cs
3. Add control using [XAML](references/adding-controls.md)

### Use Case 3: Enterprise Deployment

**Goal**: Deploy Syncfusion to multiple developer machines without internet dependency.

**Approach**:
1. Download [offline installer](references/installation-methods.md)
2. Distribute installer package internally
3. Install on each developer machine
4. Provide [license keys](references/upgrading.md) for registration

### Use Case 4: Upgrading Existing Project

**Goal**: Update project from older Syncfusion version to latest.

**Approach**:
1. Review [Upgrade Guide](https://help.syncfusion.com/upgrade-guide/wpf-ui-controls)
2. Download latest version from [account downloads](references/upgrading.md)
3. Update NuGet packages or install new version
4. Test for breaking changes
5. Update license key if needed

### Use Case 5: Localized Application

**Goal**: Support multiple languages in WPF application with Syncfusion controls.

**Approach**:
1. Set up [localization resources](references/configuration.md)
2. Create .resx files for target cultures
3. Set application culture at startup
4. Syncfusion controls automatically use localized strings

## Next Steps

After completing setup:

1. **Set up localization** (if building international apps):
   - See [references/localization.md](references/localization.md) for complete .resx setup guide

2. **Learn MVVM patterns** with Syncfusion controls:
   - See [references/patterns-and-practices.md](references/patterns-and-practices.md) for commands, data binding, and best practices

3. **Explore component-specific skills**:
   - For charting: See implementing-syncfusion-wpf-components → Charts
   - For data grids: See implementing-syncfusion-wpf-components → Data Grids
   - For editors: See implementing-syncfusion-wpf-components → Editors

4. **Configure framework compatibility**:
   - See [references/configuration.md](references/configuration.md) for .NET Framework and .NET Core/.NET setup

## Related Resources

- **Official Documentation**: [Syncfusion WPF Documentation](https://help.syncfusion.com/wpf/welcome-to-syncfusion-essential-wpf)
- **Upgrade Guide**: [Breaking Changes and Updates](https://help.syncfusion.com/upgrade-guide/wpf-ui-controls)
- **Licensing**: [License Key Overview](https://help.syncfusion.com/wpf/licensing/overview)
- **Component Library**: For specific component implementation, use the implementing-syncfusion-wpf-components skill
