# NuGet Package Management

## Table of Contents
- [Overview](#overview)
- [Package Manager UI](#package-manager-ui)
- [Dotnet CLI](#dotnet-cli)
- [Package Manager Console](#package-manager-console)
- [Package Sources Configuration](#package-sources-configuration)
- [Version Management](#version-management)
- [Common Scenarios](#common-scenarios)

## Overview

NuGet is a package management system for Visual Studio that makes it easy to add, update, and remove external libraries. Syncfusion publishes all WPF NuGet packages on [nuget.org](https://www.nuget.org/packages?q=Tags%3A%22Wpf%22+syncfusion), allowing you to use Syncfusion WPF components without installing the full Essential Studio setup.

### Advantages of NuGet Packages

- ✓ **Lightweight**: Install only the controls you need
- ✓ **No Installer Required**: Works without full Syncfusion installation
- ✓ **Easy Updates**: Update packages individually or all at once
- ✓ **Version Control**: Specify exact package versions per project
- ✓ **CI/CD Friendly**: Automatic package restore in build pipelines
- ✓ **Cross-Team**: Consistent dependencies across development teams

### When to Use NuGet Packages

- Building applications with specific controls (not all Syncfusion controls)
- CI/CD environments
- Containerized applications
- Projects with strict dependency management
- When disk space or installation time is a concern

> **Note**: From v16.2.0.46 (2018 Volume 2 Service Pack 1) onwards, all Syncfusion WPF components are available as NuGet packages at nuget.org.

## Package Manager UI

The NuGet Package Manager UI provides a visual interface for managing packages.

### Installing Packages

**Step 1: Open Package Manager**

Option A:
- Right-click on WPF application or solution in Solution Explorer
- Select **Manage NuGet Packages...**

Option B:
- Go to **Tools** menu
- Hover over **NuGet Package Manager**
- Select **Manage NuGet Packages for Solution...**

**Step 2: Browse for Packages**

1. The Manage NuGet Packages window opens
2. Navigate to the **Browse** tab
3. Search for Syncfusion WPF packages using term: **"Syncfusion.WPF"**
4. Select the appropriate package for your needs

> **Note**: The [nuget.org](https://api.nuget.org/v3/index.json) package source is selected by default. If not configured, see [Package Sources Configuration](#package-sources-configuration).

**Step 3: Review Package Information**

- Right-side panel shows package details:
  - Description
  - Version history
  - Dependencies
  - Download statistics
  - Project URL

**Step 4: Install Package**

1. Select the desired version (latest by default)
2. Click the **Install** button
3. Review and accept the license terms
4. Package will be installed along with dependencies

**Step 5: Verify Installation**

- Required Syncfusion assemblies are added to your project
- Ready to start using the controls
- Refer to [Syncfusion WPF help documentation](https://help.syncfusion.com/wpf/welcome-to-syncfusion-essential-wpf) for control usage

### Example: Installing SfGrid.WPF

1. Open Manage NuGet Packages
2. Browse tab → Search "Syncfusion.SfGrid.WPF"
3. Select **Syncfusion.SfGrid.WPF** package
4. Choose version (e.g., 25.1.35)
5. Click **Install**
6. Accept license agreement

**Result**: 
- `Syncfusion.SfGrid.WPF` added with dependencies:
  - Syncfusion.Data.WPF
  - Syncfusion.Shared.WPF
  - Syncfusion.Licensing

### Updating Packages

1. Open Manage NuGet Packages
2. Navigate to **Updates** tab
3. Search for "Syncfusion" to see available updates
4. Select packages to update
5. Choose target version
6. Click **Update** button

**Bulk Update**:
- Check multiple packages using checkboxes
- Click **Update** to update all selected packages at once

### Uninstalling Packages

1. Open Manage NuGet Packages
2. Navigate to **Installed** tab
3. Search for package to remove
4. Click **Uninstall** button
5. Confirm uninstallation

> **Warning**: Uninstalling a package that other packages depend on may cause errors. Review dependencies before uninstalling.

## Dotnet CLI

The [.NET Command Line Interface (CLI)](https://docs.microsoft.com/en-us/nuget/consume-packages/install-use-packages-dotnet-cli) allows package management via command line without modifying project files manually.

### Installing Packages

**Command Syntax:**

```bash
dotnet add package <Package_Name>
```

**Example:**

```bash
dotnet add package Syncfusion.SfGrid.WPF
```

**With Specific Version:**

```bash
dotnet add package Syncfusion.SfGrid.WPF -v 19.3.0.43
```

> **Note**: If you don't provide a version flag, the latest version is installed by default.

### Steps

1. Open a command prompt or terminal
2. Navigate to the directory containing your project file (`.csproj`)
3. Run the `dotnet add package` command
4. The package reference is added to the project file
5. Run `dotnet restore` to install the package

**Example Session:**

```bash
cd C:\Projects\MyWpfApp
dotnet add package Syncfusion.SfGrid.WPF -v 27.1.48
```

### Verifying Installation

Open the `.csproj` file to see the added reference:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net8.0-windows</TargetFramework>
    <UseWPF>true</UseWPF>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Syncfusion.SfGrid.WPF" Version="27.1.48" />
  </ItemGroup>
</Project>
```

### Restoring Packages

Run `dotnet restore` to restore all packages listed in the project file:

```bash
dotnet restore
```

> **Note**: Restoring is done automatically with `dotnet build` and `dotnet run` in .NET Core 2.0 and later.

### Advantages

- ✓ Fast and scriptable
- ✓ CI/CD friendly
- ✓ No IDE required
- ✓ Works cross-platform
- ✓ Easy to automate

### Use Cases

- **CI/CD Pipelines**: Automate package installation
- **Docker Containers**: Add packages during image build
- **Command-Line Development**: Using VS Code or other editors
- **Scripted Setup**: Batch installation across multiple projects

## Package Manager Console

The Package Manager Console is a PowerShell-based interface within Visual Studio for NuGet operations.

### Opening Package Manager Console

1. Open your WPF application in Visual Studio
2. Navigate to **Tools** → **NuGet Package Manager** → **Package Manager Console**
3. Console appears at the bottom of Visual Studio

### Installing Packages

**Install Specified Package:**

```powershell
Install-Package <Package_Name>
```

**Example:**

```powershell
Install-Package Syncfusion.SfGrid.WPF
```

**Install in Specific Project:**

```powershell
Install-Package <Package_Name> -ProjectName <Project_Name>
```

**Example:**

```powershell
Install-Package Syncfusion.SfGrid.WPF -ProjectName SyncfusionWPFApp
```

**Install Specific Version:**

```powershell
Install-Package Syncfusion.SfGrid.WPF -Version 19.3.0.44
```

### Updating Packages

**Update Specified Package:**

```powershell
Update-Package <Package_Name>
```

**Example:**

```powershell
Update-Package Syncfusion.SfGrid.WPF
```

**Update in Specific Project:**

```powershell
Update-Package <Package_Name> -ProjectName <Project_Name>
```

**Example:**

```powershell
Update-Package Syncfusion.SfGrid.WPF -ProjectName SyncfusionWPFApp
```

**Update to Specific Version:**

```powershell
Update-Package Syncfusion.SfGrid.WPF -Version 25.1.35
```

### Uninstalling Packages

```powershell
Uninstall-Package <Package_Name>
```

**Example:**

```powershell
Uninstall-Package Syncfusion.SfGrid.WPF
```

### Listing Packages

**List all installed packages:**

```powershell
Get-Package
```

**List packages in specific project:**

```powershell
Get-Package -ProjectName SyncfusionWPFApp
```

### Finding Packages

**Search for packages:**

```powershell
Find-Package Syncfusion.WPF
```

### Example Session

```powershell
PM> Install-Package Syncfusion.SfGrid.WPF
Installing Syncfusion.SfGrid.WPF 25.1.35
Installing Syncfusion.Data.WPF 25.1.35
Installing Syncfusion.Shared.WPF 25.1.35
Successfully installed 'Syncfusion.SfGrid.WPF 25.1.35' to SyncfusionWPFApp
```

### Advantages

- ✓ Quick command execution
- ✓ No need to search for packages
- ✓ PowerShell scripting capability
- ✓ Batch operations possible
- ✓ Integrated in Visual Studio

### Use Cases

- **Experienced Developers**: Fast package management
- **Bulk Operations**: Installing multiple packages quickly
- **Scripted Setup**: Automating project setup
- **Troubleshooting**: Direct package manipulation

## Package Sources Configuration

### Verifying nuget.org Source

1. Open Visual Studio
2. Go to **Tools** → **Options**
3. Expand **NuGet Package Manager**
4. Select **Package Sources**
5. Verify `nuget.org` is listed with URL: `https://api.nuget.org/v3/index.json`

### Adding nuget.org Source

If nuget.org is not configured:

1. Go to **Tools** → **Options** → **NuGet Package Manager** → **Package Sources**
2. Click the **+** button
3. **Name**: `nuget.org`
4. **Source**: `https://api.nuget.org/v3/index.json`
5. Click **Update**
6. Click **OK**

### Using Multiple Sources

You can configure multiple package sources:

- **nuget.org**: Public Syncfusion packages
- **Private feed**: Internal company packages
- **Local folder**: Offline packages

### Configuring via NuGet.config

Create or edit `NuGet.config` in your solution folder:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
    <add key="Company Feed" value="https://company.feed.com/nuget" />
  </packageSources>
</configuration>
```

## Version Management

### Specifying Exact Versions

In `.csproj` file:

```xml
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="25.1.35" />
```

### Version Ranges

Allow updates within a range:

```xml
<!-- Accept any 25.x version -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="25.*" />

<!-- Accept versions >= 25.1.35 but < 26.0 -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="[25.1.35, 26.0)" />
```

### Floating Versions

**Latest stable version:**

```xml
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="*" />
```

> **Warning**: Using floating versions can lead to unexpected breaking changes. Pin to specific versions for production applications.

### Checking Installed Versions

**Via Package Manager UI:**
1. Manage NuGet Packages → Installed tab
2. View version number next to each package

**Via Package Manager Console:**

```powershell
Get-Package | Select-Object Id, Version
```

**Via .csproj file:**

Open `.csproj` and check `PackageReference` elements.

### Upgrading Across Major Versions

When upgrading to a new major version (e.g., from 24.x to 25.x):

1. Check [Upgrade Guide](https://help.syncfusion.com/upgrade-guide/wpf-ui-controls) for breaking changes
2. Update all Syncfusion packages to same version
3. Test thoroughly before deploying

> **Best Practice**: Keep all Syncfusion packages at the same version to avoid compatibility issues.

## Common Scenarios

### Scenario 1: Fresh Project Setup

**Goal**: Add SfDataGrid to a new WPF project.

**Steps**:
1. Create new WPF project in Visual Studio
2. Right-click project → Manage NuGet Packages
3. Browse → Search "Syncfusion.SfGrid.WPF"
4. Install latest version
5. Register license key (see below)
6. Add control to XAML

### Scenario 2: Updating All Syncfusion Packages

**Goal**: Update all Syncfusion packages to latest version.

**Steps**:
1. Open Package Manager Console
2. Run commands:

```powershell
Get-Package | Where-Object { $_.Id -like "Syncfusion.*" } | Update-Package
```

Or via UI:
1. Manage NuGet Packages → Updates tab
2. Search "Syncfusion"
3. Select all Syncfusion packages
4. Click Update

### Scenario 3: Offline/Air-Gapped Environment

**Goal**: Use NuGet packages without internet access.

**Steps**:
1. Download packages on internet-connected machine:
   ```bash
   nuget.exe install Syncfusion.SfGrid.WPF -OutputDirectory C:\NuGetCache
   ```
2. Copy `C:\NuGetCache` to offline machine
3. Configure local package source:
   - Tools → Options → NuGet Package Manager → Package Sources
   - Add source pointing to `C:\NuGetCache`
4. Install from local source

### Scenario 4: CI/CD Pipeline

**Goal**: Automated package restore in build pipeline.

**Azure DevOps YAML:**

```yaml
- task: DotNetCoreCLI@2
  displayName: 'Restore NuGet Packages'
  inputs:
    command: 'restore'
    projects: '**/*.csproj'
    feedsToUse: 'select'
    vstsFeed: 'nuget.org'
```

**GitHub Actions:**

```yaml
- name: Restore dependencies
  run: dotnet restore
```

### Scenario 5: License Key Registration

**Required for**: Trial versions and NuGet packages

**App.xaml.cs:**

```csharp
public partial class App : Application
{
    public App()
    {
        // Register license key before InitializeComponent
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE");
        InitializeComponent();
    }
}
```

**VB.NET - App.xaml.vb:**

```vb
Public Class Application
    Sub New()
        ' Register license key
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE")
        InitializeComponent()
    End Sub
End Class
```

Generate license key from: [License & Downloads](https://www.syncfusion.com/account/downloads)

## Troubleshooting

### "Unable to find package"

**Problem**: Package not found when searching.

**Solution**:
- Verify nuget.org is configured as package source
- Check internet connection
- Try searching with exact package name
- Clear NuGet cache: `dotnet nuget locals all --clear`

### "Package restore failed"

**Problem**: Packages don't restore during build.

**Solution**:
- Check NuGet.config is correctly configured
- Verify package source URL is accessible
- Run `dotnet restore` manually
- Check for version conflicts

### Version conflicts

**Problem**: "Found conflicts between different versions of the same dependent assembly."

**Solution**:
- Update all Syncfusion packages to the same version
- Check for indirect dependencies
- Use binding redirects if necessary

### License key errors

**Problem**: "License key is invalid" or "Unable to find a valid license."

**Solution**:
- Verify license key is correct and not expired
- Ensure license key is registered before `InitializeComponent()`
- Generate new license key from account portal
- See [troubleshooting.md](troubleshooting.md) for detailed solutions

## Available NuGet Packages

### Common WPF Packages

| Package Name | Description |
|--------------|-------------|
| Syncfusion.SfGrid.WPF | Data Grid controls (SfDataGrid, SfTreeGrid) |
| Syncfusion.SfChart.WPF | Chart controls (SfChart, sparklines) |
| Syncfusion.SfInput.WPF | Input controls (SfTextBox, SfDatePicker, etc.) |
| Syncfusion.Tools.WPF | Tools controls (Ribbon, Docking, etc.) |
| Syncfusion.Shared.WPF | Shared controls and utilities |
| Syncfusion.SfSkinManager.WPF | Theme management |

### Finding All Packages

Browse all Syncfusion WPF packages:
[NuGet.org - Syncfusion WPF](https://www.nuget.org/packages?q=Tags%3A%22wpf%22+syncfusion)

## Next Steps

After installing NuGet packages:

1. **Register license key**: See above or [upgrading.md](upgrading.md)
2. **Add controls**: Refer to [adding-controls.md](adding-controls.md)
3. **Configure dependencies**: Check [configuration.md](configuration.md)
4. **Explore control features**: Use component-specific skills
