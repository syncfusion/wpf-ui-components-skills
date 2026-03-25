# Troubleshooting

## Table of Contents
- [Overview](#overview)
- [Installation Errors](#installation-errors)
- [License and Registration Issues](#license-and-registration-issues)
- [Assembly Reference Problems](#assembly-reference-problems)
- [NuGet Package Errors](#nuget-package-errors)
- [Visual Studio Integration Issues](#visual-studio-integration-issues)
- [Runtime Errors](#runtime-errors)

## Overview

This reference provides solutions to common issues encountered during Syncfusion WPF setup, installation, and configuration.

## Installation Errors

### Unlocking License Installer with Trial Key

**Error Message:**
```
Sorry, the provided unlock key is a trial unlock key and cannot be used 
to unlock the licensed version of our Essential Studio for WPF installer.
```

**Reason:**
You are attempting to use a Trial unlock key to unlock the licensed installer.

**Solution:**
1. Only a licensed unlock key can unlock a licensed installer
2. Generate the licensed unlock key from [Syncfusion Account](https://www.syncfusion.com/account/downloads)
3. See [How to generate unlock key](http://syncfusion.com/kb/2326) for detailed instructions
4. Use the licensed unlock key with the licensed installer

---

### License Has Expired

**Error Message:**
```
Your license for Syncfusion Essential Studio for WPF has been expired 
since {date}. Please renew your subscription and try again.
```

**Reason:**
Your license subscription has expired.

**Solution - Choose one:**

1. **Renew Subscription**
   - Visit [My Renewals](https://www.syncfusion.com/account/my-renewals)
   - Renew your existing subscription

2. **Purchase New License**
   - Visit [Syncfusion Products](https://www.syncfusion.com/sales/products)
   - Purchase a new license

3. **Contact Sales**
   - Email: <salessupport@syncfusion.com>
   - Request assistance with licensing

4. **Extend Trial** (if applicable)
   - You can extend the 30-day trial period after license expiration
   - This provides temporary access while resolving licensing

---

### Unable to Find Valid License or Trial

**Error Message:**
```
Sorry, we are unable to find a valid license or trial for Essential Studio 
for WPF under your account.
```

**Possible Reasons:**
- Trial period has expired
- You don't have a license or active trial
- You are not the license holder
- License has not been assigned by your account administrator

**Solution - Choose one:**

1. **Purchase License**
   - Visit [Syncfusion Products](https://www.syncfusion.com/sales/products)

2. **Contact Administrator**
   - If you're part of a team, contact your account administrator
   - Request license assignment

3. **Request License**
   - Email: <clientrelations@syncfusion.com>
   - Request license information

4. **Contact Sales**
   - Email: <salessupport@syncfusion.com>
   - Discuss licensing options

---

### Another Installation in Progress

**Error Message:**
```
Another installation is in progress. You cannot start this installation 
without completing all other currently active installations. Click cancel 
to end this installer or retry to attempt after currently active 
installation completed to install again.
```

**Reason:**
Another installation process is running on your system.

**Solution:**

**Option 1: Kill msiexec Process**

1. Open **Windows Task Manager** (Ctrl+Shift+Esc)
2. Go to **Details** tab
3. Find **msiexec.exe** process
4. Right-click → Select **End task**
5. Retry Syncfusion installation

**Option 2: Restart Computer**

1. Save all work
2. Restart your computer
3. Retry Syncfusion installation after restart

---

### Controlled Folder Access Error

**Error Message (Offline):**
```
Controlled folder access seems to be enabled in your machine. 
The provided install or samples location (e.g., Public Documents) 
is protected by the controlled folder access settings.
```

**Error Message (Online/Web Installer):**
```
Controlled folder access seems to be enabled in your machine. 
The provided install, samples, or download location (e.g., Public Documents) 
is protected by the controlled folder access settings.
```

**Reason:**
Windows Controlled Folder Access feature is blocking installation to protected folders.

**Solution 1: Disable Controlled Folder Access (Recommended)**

1. By default, Syncfusion installs demos in Public Documents folder
2. Controlled folder access prevents writing to Documents folder
3. Follow [Microsoft's guide](https://support.microsoft.com/en-us/windows/allow-an-app-to-access-controlled-folders-b5b6627a-b008-2ca2-7931-7e51e912b034) to disable controlled folder access
4. Complete Syncfusion installation
5. Re-enable controlled folder access after installation

**Solution 2: Change Installation Directory**

1. During installation, choose custom installation
2. Change demo installation path to a non-protected directory
3. Example: `C:\Syncfusion\Demos` instead of Public Documents
4. Complete installation with new path

---

## License and Registration Issues

### Invalid License Key

**Error Message:**
```
The license key is invalid.
```

**Causes and Solutions:**

**Cause 1: Incorrect License Key**
- Verify you copied the entire license key
- Check for extra spaces or line breaks
- Regenerate key from [License & Downloads](https://www.syncfusion.com/account/downloads)

**Cause 2: Expired License**
- Check license expiration date
- Renew subscription if expired

**Cause 3: Wrong Product**
- Ensure license key is for WPF, not another platform
- Generate correct platform-specific key

**Cause 4: Trial Key Used for Licensed Installation**
- Use licensed key for licensed version
- Download licensed installer from account portal

---

### License Key Not Registered

**Error Message:**
```
This application was built using a trial version of Syncfusion Essential Studio.
```

**Reason:**
License key is not registered before controls are initialized.

**Solution:**

Register license key in `App.xaml.cs` **before** `InitializeComponent()`:

**C#:**
```csharp
public partial class App : Application
{
    public App()
    {
        // MUST come before InitializeComponent()
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        InitializeComponent();
    }
}
```

**VB.NET:**
```vb
Public Class Application
    Sub New()
        ' MUST come before InitializeComponent()
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY")
        InitializeComponent()
    End Sub
End Class
```

**Important:** Registration must happen before any Syncfusion control is created.

---

## Assembly Reference Problems

### Could Not Load File or Assembly

**Error Message:**
```
Could not load file or assembly 'Syncfusion.SfGrid.WPF' or one of its dependencies.
The system cannot find the file specified.
```

**Causes and Solutions:**

**Cause 1: Missing Assembly Reference**

**Solution:**
1. Right-click project → **Add Reference**
2. Browse to Syncfusion installation folder
3. Add missing assembly
4. Or install via NuGet: `Install-Package Syncfusion.SfGrid.WPF`

**Cause 2: Version Mismatch**

**Solution:**
1. Check all Syncfusion assemblies are same version
2. Update all to consistent version
3. Use Package Manager Console:
   ```powershell
   Get-Package | Where-Object { $_.Id -like "Syncfusion.*" }
   ```

**Cause 3: Missing Dependencies**

**Solution:**
1. Check control dependencies in [configuration.md](configuration.md)
2. Add all required dependent assemblies
3. Or use NuGet (handles dependencies automatically)

---

### Namespace Not Found

**Error in XAML:**
```
The name "SfDataGrid" does not exist in the namespace "http://schemas.syncfusion.com/wpf".
```

**Causes and Solutions:**

**Cause 1: Assembly Not Referenced**

**Solution:**
1. Add assembly reference to project
2. Rebuild solution

**Cause 2: Wrong Namespace URI**

**Solution:**
Verify namespace declaration:
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

**Cause 3: Control Name Misspelled**

**Solution:**
- Check correct control name (case-sensitive)
- Example: `SfDataGrid` not `SFDataGrid` or `sfDataGrid`

---

## NuGet Package Errors

### Unable to Find Package

**Error Message:**
```
Unable to find package 'Syncfusion.SfGrid.WPF'.
```

**Causes and Solutions:**

**Cause 1: nuget.org Not Configured**

**Solution:**
1. Go to **Tools** → **Options** → **NuGet Package Manager** → **Package Sources**
2. Verify `nuget.org` exists with URL: `https://api.nuget.org/v3/index.json`
3. If missing, add it:
   - Click **+** button
   - Name: `nuget.org`
   - Source: `https://api.nuget.org/v3/index.json`
   - Click **OK**

**Cause 2: No Internet Connection**

**Solution:**
- Check internet connectivity
- Try accessing nuget.org in browser
- Check firewall/proxy settings

**Cause 3: Package Name Misspelled**

**Solution:**
- Verify exact package name
- Search on [nuget.org](https://www.nuget.org/packages?q=syncfusion+wpf)

---

### Package Restore Failed

**Error Message:**
```
Package restore failed. Rolling back package changes.
```

**Causes and Solutions:**

**Cause 1: Network Issues**

**Solution:**
- Check internet connection
- Retry restore
- Clear NuGet cache:
  ```bash
  dotnet nuget locals all --clear
  ```

**Cause 2: Corrupted NuGet Cache**

**Solution:**
1. Close Visual Studio
2. Delete cache folders:
   - `%userprofile%\.nuget\packages`
   - `%localappdata%\NuGet\Cache`
3. Reopen Visual Studio
4. Restore packages

**Cause 3: Version Not Available**

**Solution:**
- Check package version exists on nuget.org
- Try latest version instead of specific version

---

### Version Conflicts

**Error Message:**
```
Found conflicts between different versions of the same dependent assembly.
```

**Causes and Solutions:**

**Cause 1: Mixed Syncfusion Versions**

**Solution:**
1. Update all Syncfusion packages to same version
2. Use Package Manager Console:
   ```powershell
   Get-Package | Where-Object { $_.Id -like "Syncfusion.*" } | Update-Package
   ```

**Cause 2: Indirect Dependencies**

**Solution:**
- Check Dependencies node in Solution Explorer
- Identify conflicting versions
- Update packages causing conflicts

**Cause 3: Third-Party Package Conflicts**

**Solution:**
1. Identify which package depends on different version
2. Update or remove conflicting package
3. Use binding redirects if necessary

---

## Visual Studio Integration Issues

### Controls Not in Toolbox

**Problem:**
Syncfusion controls don't appear in Visual Studio Toolbox.

**Causes and Solutions:**

**Cause 1: Full Installer Not Used**

**Solution:**
- Toolbox integration requires full installer (not NuGet only)
- Download and install from [Syncfusion.com](https://www.syncfusion.com/wpf-controls)

**Cause 2: Configuration Not Selected**

**Solution:**
1. Reinstall Syncfusion
2. During installation, check:
   - ☑ **Configure Syncfusion controls in Visual Studio**
   - ☑ **Register Syncfusion Assemblies in GAC** (required for toolbox)

**Cause 3: Toolbox Corrupted**

**Solution:**
1. Right-click Toolbox
2. Select **Reset Toolbox**
3. Restart Visual Studio

---

### Project Templates Missing

**Problem:**
Syncfusion WPF Application template not available in New Project dialog.

**Causes and Solutions:**

**Cause 1: Extensions Not Installed**

**Solution:**
1. Go to **Extensions** → **Manage Extensions** → **Online**
2. Search "Syncfusion WPF"
3. Download and install extension
4. Restart Visual Studio

**Cause 2: Full Installer Not Used**

**Solution:**
- Project templates come with full installer
- Download and install from Syncfusion.com

**Cause 3: Visual Studio Version**

**Solution:**
- Ensure Visual Studio 2015 or later
- Update to latest Visual Studio version

---

## Runtime Errors

### XamlParseException

**Error Message:**
```
System.Windows.Markup.XamlParseException: 
'Provide value on 'System.Windows.Baml2006.TypeConverterMarkupExtension' threw an exception.'
```

**Causes and Solutions:**

**Cause 1: Missing Assembly**

**Solution:**
- Verify all required assemblies are referenced
- Check dependencies for your control

**Cause 2: Assembly Version Mismatch**

**Solution:**
- Ensure all Syncfusion assemblies are same version
- Update to consistent version

---

### Control Not Rendering

**Problem:**
Control added but doesn't appear in application.

**Causes and Solutions:**

**Cause 1: Size Not Set**

**Solution:**
- Set Width and Height explicitly
- Or use layout properties (HorizontalAlignment, VerticalAlignment)

**Cause 2: Hidden by Other Controls**

**Solution:**
- Check Z-order
- Verify Grid.Row, Grid.Column if using Grid

**Cause 3: Theme Issue**

**Solution:**
- Apply theme resources in App.xaml
- Or use SfSkinManager

---

### Performance Issues

**Problem:**
Application slow when using Syncfusion controls.

**Causes and Solutions:**

**Cause 1: Large Datasets**

**Solution:**
- Enable UI virtualization
- Use data paging
- Load data asynchronously

**Cause 2: Unnecessary Rendering**

**Solution:**
- Disable animations if not needed
- Use lightweight themes
- Optimize data binding

---

## Getting Additional Help

### Syncfusion Support

**Direct Support:**
- Visit [Syncfusion Support Portal](https://www.syncfusion.com/support/directtrac)
- Submit support ticket with:
  - Detailed error description
  - Stack trace
  - Code sample
  - Syncfusion version

**Community Forums:**
- [Syncfusion Community Forums](https://www.syncfusion.com/forums)
- Search existing solutions
- Post new questions

**Knowledge Base:**
- [Syncfusion Knowledge Base](https://www.syncfusion.com/kb)
- Search for specific errors
- Find solutions to common issues

### Documentation

- [Official WPF Documentation](https://help.syncfusion.com/wpf/welcome-to-syncfusion-essential-wpf)
- [Control Dependencies](https://help.syncfusion.com/wpf/control-dependencies)
- [Upgrade Guide](https://help.syncfusion.com/upgrade-guide/wpf-ui-controls)

### Debugging Tips

**Enable Detailed Logging:**
```xml
<configuration>
  <system.diagnostics>
    <trace autoflush="true" indentsize="4">
      <listeners>
        <add name="myListener" type="System.Diagnostics.TextWriterTraceListener" 
             initializeData="application.log" />
      </listeners>
    </trace>
  </system.diagnostics>
</configuration>
```

**Check Output Window:**
- View → Output (Ctrl+Alt+O)
- Review build errors and warnings

**Use Debugger:**
- Set breakpoints
- Inspect variables
- Check call stack

## Prevention Best Practices

✓ **Keep packages updated** - Regularly update to latest stable version  
✓ **Maintain version consistency** - All Syncfusion assemblies same version  
✓ **Read release notes** - Review breaking changes before upgrading  
✓ **Test thoroughly** - Test after any Syncfusion update  
✓ **Backup before major changes** - Use version control  
✓ **Follow documentation** - Refer to official guides  
✓ **Register license correctly** - Before InitializeComponent()  
✓ **Use NuGet for dependencies** - Automatic dependency resolution

## Next Steps

After resolving issues:

1. **Verify application runs correctly**
2. **Document solution** for future reference
3. **Share findings** with team
4. **Report bugs** to Syncfusion if found
5. **Continue development** with resolved configuration
