# Upgrading Syncfusion WPF

## Table of Contents
- [Overview](#overview)
- [Upgrading to Latest Version](#upgrading-to-latest-version)
- [Upgrading from Trial to License](#upgrading-from-trial-to-license)
- [Upgrading NuGet Packages](#upgrading-nuget-packages)
- [Upgrading Visual Studio Extensions](#upgrading-visual-studio-extensions)
- [Version Release Cycle](#version-release-cycle)

## Overview

Syncfusion releases new volumes every three months with exciting new features, plus Service Pack releases with major bug fixes. You can upgrade to the latest version from any installed Syncfusion version without uninstalling previous versions.

### Release Types

**Volume Releases** (Quarterly)
- Major new features
- Performance improvements
- New controls
- Released every 3 months

**Service Pack Releases**
- Critical bug fixes
- Minor improvements
- Released between volumes

**Weekly NuGet Releases**
- Hot fixes for critical issues
- Available on nuget.org
- Quick patches without full installer

### Why Upgrade

- Access new features and controls
- Performance improvements
- Bug fixes and stability
- Security updates
- Modern framework support
- Latest Visual Studio integration

> **Important**: Always check the [Upgrade Guide](https://help.syncfusion.com/upgrade-guide/wpf-ui-controls) to learn about breaking changes, bug fixes, features, and known issues between versions.

## Upgrading to Latest Version

### Using Syncfusion Control Panel

The easiest way to upgrade to the latest version.

**Steps:**

1. **Open Syncfusion WPF Control Panel**
   - Find the Control Panel from Start Menu or Desktop shortcut
   - Or launch from previous installation

2. **Check for Updates**
   - At the top of the Control Panel, you'll see: **"Latest Version: {Version}"**
   - Click this link to download the latest version

3. **Download and Install**
   - You'll be redirected to the download page
   - Download the latest installer (web or offline)
   - Run the installer

4. **Installation Process**
   - Follow the installation wizard
   - Previous versions are NOT required to be uninstalled
   - Volume and Service Pack releases work independently
   - You can install the latest version directly

> **Note**: It's not required to install Volume release before installing Service Pack. They work independently, so you can install the latest version with major bug fixes directly.

### Using Syncfusion Website

**Steps:**

1. **Log in to Syncfusion Account**
   - Visit [Syncfusion License & Downloads](https://www.syncfusion.com/account/downloads)
   - Log in with your credentials

2. **Download Latest Version**
   - View your active licenses
   - Click **Download** button for WPF product
   - Latest version downloads automatically

3. **Download Older Versions** (if needed)
   - Go to [Downloads Older Versions](https://www.syncfusion.com/account/downloads/studio)
   - Select the version you need

4. **Install**
   - Run the downloaded installer
   - Follow installation wizard
   - No need to uninstall existing versions

### Important Notes

- **Existing installations remain**: Previous versions are not affected
- **Independent operation**: Each version operates independently
- **Side-by-side installation**: Multiple versions can coexist
- **Project-specific**: Each project can use different Syncfusion versions

## Upgrading from Trial to License

When your trial period ends or you purchase a license, you need to upgrade from trial to licensed version.

### Option 1: Upgrade Installer-Based Installation

**When to use**: If you installed Syncfusion via full installer (web or offline).

**Steps:**

1. **Download Licensed Installer**
   - Visit [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Download the licensed installer for WPF

2. **Run Licensed Installer**
   - Execute the downloaded installer
   - **No need to uninstall trial version**
   - Licensed installer replaces trial installation

3. **Complete Installation**
   - Follow installation wizard
   - Licensed version is now active
   - No watermarks or trial messages

> **Note**: The trial and licensed installers are the same file. The installer detects your license status from your account and installs accordingly.

### Option 2: Upgrade NuGet Package License

**When to use**: If you're using Syncfusion controls via NuGet packages from nuget.org.

**Steps:**

1. **Generate Licensed Key**
   - Visit [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Go to your active licensed products
   - Click **Generate License Key** or **Get License Key**
   - Copy the generated license key

2. **Replace Trial Key in Code**
   
   **App.xaml.cs (C#):**
   ```csharp
   public partial class App : Application
   {
       public App()
       {
           // Replace trial key with your licensed key
           Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSED_KEY_HERE");
           InitializeComponent();
       }
   }
   ```
   
   **App.xaml.vb (VB.NET):**
   ```vb
   Public Class Application
       Sub New()
           ' Replace trial key with your licensed key
           Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSED_KEY_HERE")
           InitializeComponent()
       End Sub
   End Class
   ```

3. **Rebuild Application**
   - Clean and rebuild your project
   - Trial messages and watermarks will be removed

### License Key Registration

**When license key is required:**
- Using trial installer
- Using NuGet packages from nuget.org

**When license key is NOT required:**
- Using licensed installer with assembly references from installation directory

**Best Practices:**
- Register license key before `InitializeComponent()`
- Store license key securely (consider configuration files or environment variables)
- Use different keys for different environments if needed

## Upgrading NuGet Packages

### Using Package Manager UI

**Steps:**

1. **Open NuGet Package Manager**
   - Right-click project/solution → **Manage NuGet Packages...**
   - Or: **Tools** → **NuGet Package Manager** → **Manage NuGet Packages for Solution...**

2. **Navigate to Updates Tab**
   - Click the **Updates** tab
   - Search for "Syncfusion" to filter Syncfusion packages

3. **Select Packages**
   - Select individual packages to update
   - Or check multiple packages for bulk update

4. **Choose Version**
   - Latest version selected by default
   - Or choose specific version from dropdown

5. **Update**
   - Click **Update** button
   - Accept license terms
   - Packages update with dependencies

### Using .NET CLI

**Update single package:**

```bash
dotnet add package Syncfusion.SfGrid.WPF
```

This installs the latest version by default.

**Update to specific version:**

```bash
dotnet add package Syncfusion.SfGrid.WPF -v 25.1.35
```

**Update all packages in project:**

```bash
dotnet list package --outdated
dotnet add package <PackageName>  # Repeat for each package
```

### Using Package Manager Console

**Update specified package:**

```powershell
Update-Package Syncfusion.SfGrid.WPF
```

**Update package in specific project:**

```powershell
Update-Package Syncfusion.SfGrid.WPF -ProjectName MyWpfApp
```

**Update to specific version:**

```powershell
Update-Package Syncfusion.SfGrid.WPF -Version 25.1.35
```

**Update all Syncfusion packages:**

```powershell
Get-Package | Where-Object { $_.Id -like "Syncfusion.*" } | Update-Package
```

### Important Considerations

**Version Consistency:**
- Keep all Syncfusion packages at the same version
- Mixing versions can cause compatibility issues
- Update all packages together when upgrading

**Breaking Changes:**
- Review [Upgrade Guide](https://help.syncfusion.com/upgrade-guide/wpf-ui-controls) before upgrading
- Test thoroughly after major version upgrades
- Check for API changes and obsolete members

**License Key:**
- Regenerate license key if moving to new major version
- Update license key in code after upgrading

## Upgrading Visual Studio Extensions

Syncfusion provides Visual Studio extensions that enhance productivity.

### Automatic Updates

**Via Visual Studio:**

1. **Open Extensions Manager**
   - **Visual Studio 2019+**: **Extensions** → **Manage Extensions** → **Updates**
   - **Visual Studio 2017**: **Tools** → **Extensions and Updates** → **Updates**

2. **Find Syncfusion WPF Extension**
   - Look for "Syncfusion WPF Extension" or "Essential Studio for WPF"
   - Check if update is available

3. **Update Extension**
   - Click **Update** button next to the extension
   - Close Visual Studio when prompted

4. **Install Update**
   - VSIX installer dialog appears
   - Click **Modify** button to install update

5. **Restart Visual Studio**
   - Extension updated and ready to use

### Manual Download

If automatic update doesn't work:

1. Download extension from [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SyncfusionInc.WPFExtension)
2. Close Visual Studio
3. Run downloaded `.vsix` file
4. Follow installation wizard
5. Restart Visual Studio

### Extension Features

After updating extensions, you'll have access to:
- **Project Templates**: Create Syncfusion WPF applications
- **Item Templates**: Add Syncfusion pages and controls
- **Upgrade Wizard**: Upgrade project to newer Syncfusion version
- **Troubleshooting Tools**: Fix common issues

## Version Release Cycle

### Understanding Syncfusion Versioning

**Format**: `MAJOR.MINOR.PATCH`

Example: `25.1.35`
- **25**: Year 2025
- **1**: Volume 1 (Quarter 1)
- **35**: Build number

**Volume Schedule:**
- **Volume 1**: Released around March
- **Volume 2**: Released around June  
- **Volume 3**: Released around September
- **Volume 4**: Released around December

**Service Packs:**
- Released 4-8 weeks after volume release
- Address critical bugs from volume release
- Version format: Same major.minor, incremented patch

**Weekly Releases:**
- NuGet packages updated weekly
- Hot fixes for critical issues
- Same major.minor, incremented patch

### Choosing When to Upgrade

**Immediately Upgrade When:**
- Critical bugs affecting your application
- Security vulnerabilities patched
- Required new features available
- Moving to new .NET version (8, 9, 10, etc.)

**Plan Upgrade for:**
- Major version releases (volume releases)
- After Service Pack 1 (more stable)
- During project planning phases
- When breaking changes are acceptable

**Defer Upgrade If:**
- Current version meets all needs
- No critical bugs affecting you
- Breaking changes would require significant refactoring
- Project in late stage / near release

## Upgrade Workflow

### Recommended Upgrade Process

1. **Research**
   - Read [Upgrade Guide](https://help.syncfusion.com/upgrade-guide/wpf-ui-controls)
   - Check breaking changes
   - Review new features
   - Check compatibility with your .NET version

2. **Backup**
   - Commit code to version control
   - Create a backup branch
   - Document current version

3. **Test Environment First**
   - Upgrade in development environment
   - Test thoroughly
   - Fix any issues

4. **Update Dependencies**
   - Update all Syncfusion packages to same version
   - Update license key if needed
   - Rebuild solution

5. **Test Application**
   - Run unit tests
   - Perform integration testing
   - Test all features using Syncfusion controls
   - Check for runtime errors

6. **Deploy**
   - Upgrade staging environment
   - User acceptance testing
   - Upgrade production after validation

## Troubleshooting Upgrades

### Version Conflicts

**Problem**: "Found conflicts between different versions"

**Solution**:
- Ensure all Syncfusion packages are same version
- Check indirect dependencies
- Use Package Manager Console to verify:
  ```powershell
  Get-Package | Where-Object { $_.Id -like "Syncfusion.*" }
  ```

### License Issues After Upgrade

**Problem**: License errors after upgrading

**Solution**:
- Generate new license key from account portal
- Update license key in `App.xaml.cs`
- Ensure key registration happens before `InitializeComponent()`

### Breaking Changes

**Problem**: Code doesn't compile after upgrade

**Solution**:
- Review [Upgrade Guide](https://help.syncfusion.com/upgrade-guide/wpf-ui-controls) for your version
- Check for renamed or moved APIs
- Update code according to migration guide
- Check Syncfusion community forums for solutions

### Control Panel Shows Old Version

**Problem**: Control Panel doesn't show new version after install

**Solution**:
- Refresh Control Panel
- Restart Visual Studio
- Check installation log for errors
- Reinstall if necessary

## Next Steps

After upgrading:

1. **Test your application** thoroughly
2. **Update documentation** with new version
3. **Update license key** (if applicable)
4. **Explore new features** from release notes
5. **Configure new settings** as needed (see [configuration.md](configuration.md))
