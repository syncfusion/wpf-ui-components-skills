# Installation Methods

## Table of Contents
- [Overview](#overview)
- [Web Installer](#web-installer)
- [Offline Installer](#offline-installer)
- [Trial vs Licensed Versions](#trial-vs-licensed-versions)
- [Installation Comparison](#installation-comparison)
- [Choosing an Installation Method](#choosing-an-installation-method)

## Overview

Syncfusion provides multiple ways to install Essential Studio® for WPF. Choose the method that best suits your needs:

1. **Web Installer** - Downloads components during installation (smaller initial download)
2. **Offline Installer** - Complete package for installation without internet (EXE or ZIP format)
3. **NuGet Packages** - Install specific controls without full installer (covered in nuget-package-management.md)

## Web Installer

The Web Installer is a lightweight installer that downloads and installs components during the installation process.

### Advantages

- Small initial download size (typically under 10 MB)
- Always installs the latest available version
- Automatic dependency resolution
- Selective component installation

### Downloading Web Installer

#### Trial Version - Option 1: Download Free Trial Setup

1. Visit the [Download Free Trial](https://www.syncfusion.com/downloads) page
2. Select the **WPF** platform
3. Complete the required form or log in with your Syncfusion account
4. Download the WPF trial installer from the confirmation page

> **Note**: With a trial license, only the latest version's trial installer can be downloaded.

4. Before trial expiration, you can download the installer anytime from [Trials & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

#### Trial Version - Option 2: Start Trials for NuGet Users

If you're already using Syncfusion components through NuGet.org:

1. Go to the [Start Trial](https://www.syncfusion.com/account/manage-trials/start-trials) page
2. Sign up or log in with your Syncfusion account
3. Select the **WPF** product to begin your trial
4. After starting the trial, visit [Trials & Downloads](https://www.syncfusion.com/account/manage-trials/downloads) to download the installer
5. You can also generate the unlock key and license key from this page

> **Important**: You can generate a license key for your active trial products from the Trials & Downloads page. This license key is mandatory when using trial products in your application.

#### Licensed Version

1. Access [License & Downloads](https://www.syncfusion.com/account/downloads) page under your registered Syncfusion account
2. View all licenses (active and expired) associated with your account
3. Click the **Download** button to download the WPF web installer
4. The most recent version will be downloaded by default
5. For older versions, go to [Downloads Older Versions](https://www.syncfusion.com/account/downloads/studio)

> **Note**: For both trial and licensed products, there is no separate web installer. The installer type (trial or licensed) depends on your account license status.

### Installing Web Installer

1. **Launch the Installer**
   - Double-click the downloaded installer file
   - The installer wizard automatically opens and extracts the package

2. **Welcome Screen**
   - The Syncfusion WPF Web Installer welcome wizard appears
   - Click **Next** to continue

3. **Platform Selection**
   - **Available Tab**: Select products to install
     - Check individual products or select **Install All** to install everything
   - **Installed Tab**: View and manage already installed products
     - Select products to uninstall from the same version
   
   > **Alert**: If required software isn't installed, an "Additional Software Required" alert appears. You can continue installation and install required software later.

4. **Uninstall Previous Versions** (if applicable)
   - If previous versions exist, the uninstall wizard displays
   - View list of previously installed versions
   - Check **Uninstall All** to remove all versions
   - Click **Next**
   
   > **Note**: From 2021 Volume 1 release, Syncfusion provides the option to uninstall previous versions from 18.1 while installing new versions.

5. **Confirmation**
   - Confirmation popup displays to confirm uninstallation of selected versions
   - Review the list of products to be installed/uninstalled
   - Check download size and installation size by clicking the respective links
   - Click **Next**

6. **Configuration**
   - **Download Location**: Change where files are downloaded
   - **Install Location**: Change installation directory
   - **Demos Location**: Change where samples are installed
   
   **Additional Settings** (customize per product):
   - ☐ **Install Demos** - Install Syncfusion samples
   - ☐ **Register Syncfusion Assemblies in GAC** - Register assemblies in Global Assembly Cache
   - ☐ **Configure Syncfusion controls in Visual Studio** - Add controls to VS Toolbox (requires GAC registration)
   - ☐ **Configure Syncfusion Extensions in Visual Studio** - Add Syncfusion extensions to VS
   - ☐ **Create Desktop Shortcut** - Add shortcut for Syncfusion Control Panel
   - ☐ **Create Start Menu Shortcut** - Add Start Menu shortcut for Control Panel
   
   Click **Next** to continue with default settings or **Install** to begin.

7. **License Agreement**
   - Read the License Terms and Privacy Policy
   - Check **"I agree to the License Terms and Privacy Policy"**
   - Click **Next**

8. **Login**
   - Enter your Syncfusion email address and password
   - Click **Create an Account** if you don't have one
   - Click **Forgot Password** to reset your password
   - Click **Install**
   
   > **Note**: The products will be installed based on your Syncfusion License (Trial or Licensed).

9. **Download and Installation Progress**
   - Progress bar shows download and installation/uninstallation progress
   - Wait for completion

10. **Summary**
    - View successfully installed and failed products
    - Click **Launch Control Panel** to open Syncfusion Control Panel
    - Click **Finish** to close the installer

### Post-Installation

After installation, two Syncfusion Control Panel entries will appear:
- **Essential Studio** entry - Manages all Syncfusion products in the same version
- **Product-specific** entry - Uninstalls only specific product setup

## Offline Installer

The Offline Installer is a complete installation package that doesn't require internet during installation.

### Advantages

- No internet required during installation
- Consistent installation across multiple machines
- Available in EXE and ZIP formats
- Faster installation (no downloads during setup)
- Ideal for enterprise deployments

### Downloading Offline Installer

#### Trial Version - Option 1: Download Free Trial Setup

1. Visit the [Download Free Trial](https://www.syncfusion.com/downloads) page
2. Select the **WPF** platform
3. Complete the form or log in with your Syncfusion account
4. On the confirmation page, click **More Download Options**
5. Download the Essential Studio WPF Offline trial installer (available in EXE and ZIP format)
6. Download the installer anytime before expiration from [Trials & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

#### Trial Version - Option 2: Start Trials for NuGet Users

1. Start your 30-day free trial from [Start Trial](https://www.syncfusion.com/account/manage-trials/start-trials)
2. Select WPF product
3. Go to [Trials & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
4. Generate unlock key and license key if needed
5. Download the offline installer

#### Licensed Version

1. Access [License & Downloads](https://www.syncfusion.com/account/downloads)
2. View all licenses (active and expired)
3. Click **Download** button for the WPF product
4. For older versions, visit [Downloads Older Versions](https://www.syncfusion.com/account/downloads/studio)
5. Click **More Downloads Options** to access offline installer
6. Choose format: **EXE** or **ZIP** (both are offline installers for Windows OS)

### Installing Offline Installer

#### Installing with UI

1. **Extract and Launch**
   - Double-click the offline installer file
   - The installer wizard automatically extracts the package
   - File `syncfusionessentialwpf_(version).exe` extracts and opens

2. **Unlock the Installer**
   
   Choose one of two options:
   
   **Option A: Login To Install**
   - Enter your Syncfusion email address and password
   - Click **"Create an account"** if you don't have one
   - Click **"Forgot Password"** to reset password
   - Click **Next**
   
   **Option B: Use Unlock Key**
   - Unlock keys are platform and version specific
   - Use either Syncfusion licensed or trial unlock key
   - Trial unlock key is valid for 30 days only
   - See [How to generate unlock key](https://support.syncfusion.com/kb/article/2757/how-to-generate-syncfusion-setup-unlock-key-from-syncfusion-support-account) for instructions
   - Enter unlock key and click **Next**

3. **License Agreement**
   - Read the License Terms and Privacy Policy
   - Check **"I agree to the License Terms and Privacy Policy"**
   - Click **Next**

4. **Configuration**
   - Change install location and sample locations
   - Configure Additional Settings:
     - ☐ **Install Demos** - Install Syncfusion samples
     - ☐ **Register Syncfusion Assemblies in GAC**
     - ☐ **Configure Syncfusion controls in Visual Studio** (requires GAC)
     - ☐ **Configure Syncfusion Extensions in Visual Studio**
     - ☐ **Create Desktop Shortcut**
     - ☐ **Create Start Menu Shortcut**
   - Click **Next** or **Install**

5. **Uninstall Previous Versions** (if applicable)
   - If previous versions exist, uninstall wizard opens
   - Check **Uninstall** for versions to remove
   - Click **Proceed**
   
   > **Note**: From 2021 Volume 1, you can uninstall previous versions from 18.1 while installing new versions.
   
   - Confirmation screen appears if versions are selected for uninstall
   - Progress displays uninstall and install operations

6. **Installation Progress**
   - Progress bar shows installation status
   - If uninstalling previous versions, both uninstall and install progress shown
   - Wait for completion

7. **Completion**
   - Completed screen displays installation status
   - Shows both install and uninstall status if versions were uninstalled
   - Click **Launch Control Panel** to open Syncfusion Control Panel
   - Click **Finish** to complete

#### Installing in Silent Mode

The offline installer supports silent installation through command line.

**Steps:**

1. Run the installer by double-clicking to extract
2. The file `syncfusionessentialwpf_(version).exe` extracts to the Temp directory
3. Run `%temp%` to open Temp folder
4. Locate and copy the extracted `.exe` file to a local drive
5. Exit the wizard
6. Open Command Prompt in **administrator mode**
7. Enter the installation command with arguments

**Command Syntax:**

```cmd
"installer_path\SyncfusionEssentialStudio(platform)_(version).exe" /Install silent /UNLOCKKEY:"(product_unlock_key)" [/log "{Log_file_path}"] [/InstallPath:{Location}] [/InstallSamples:{true/false}] [/InstallAssemblies:{true/false}] [/UninstallExistAssemblies:{true/false}] [/InstallToolbox:{true/false}]
```

**Example:**

```cmd
"D:\Temp\syncfusionessentialwpf_25.1.35.exe" /Install silent /UNLOCKKEY:"ABC123XYZ456" /log "C:\Temp\EssentialStudio_WPF.log" /InstallPath:C:\Syncfusion\25.1.35 /InstallSamples:true /InstallAssemblies:true /UninstallExistAssemblies:true /InstallToolbox:true
```

> **Note**: Replace version numbers and unlock key with actual values.

**Parameters:**
- `/Install silent` - Silent installation mode
- `/UNLOCKKEY` - Product unlock key (required)
- `/log` - Log file path (optional)
- `/InstallPath` - Installation directory (optional)
- `/InstallSamples` - Install samples (optional, true/false)
- `/InstallAssemblies` - Install assemblies in GAC (optional, true/false)
- `/UninstallExistAssemblies` - Uninstall existing assemblies (optional, true/false)
- `/InstallToolbox` - Configure Visual Studio Toolbox (optional, true/false)

#### Uninstalling in Silent Mode

**Command Syntax:**

```cmd
"installer_path\syncfusionessentialwpf_(version).exe" /uninstall silent
```

**Example:**

```cmd
"D:\Temp\syncfusionessentialwpf_25.1.35.exe" /uninstall silent
```

## Trial vs Licensed Versions

### Trial Version

**Features:**
- Full feature access for 30 days
- All controls and components available
- Access to samples and documentation
- Watermark or license prompts may appear in some controls

**Limitations:**
- 30-day evaluation period
- License registration required when using NuGet packages
- Trial unlock key expires after 30 days

**Getting Trial:**
- Download from [Syncfusion Downloads](https://www.syncfusion.com/downloads)
- Start trial from [Account Portal](https://www.syncfusion.com/account/manage-trials/start-trials)
- Use trial NuGet packages from nuget.org

### Licensed Version

**Features:**
- No time restrictions
- No watermarks or license prompts
- Access to all updates within subscription period
- Priority support

**Getting Licensed:**
- Purchase from [Syncfusion Sales](https://www.syncfusion.com/sales/products)
- Download from [License & Downloads](https://www.syncfusion.com/account/downloads)
- Licensed NuGet packages available with license key

### Upgrading from Trial to Licensed

See the [upgrading.md](upgrading.md) reference for complete details on upgrading from trial to licensed version.

## Installation Comparison

| Feature | Web Installer | Offline Installer | NuGet Packages |
|---------|--------------|-------------------|----------------|
| **Internet Required** | During installation | Only for download | During package install |
| **Download Size** | ~10 MB initially | ~500 MB - 2 GB | Varies by package |
| **Installation Time** | 15-30 minutes | 10-15 minutes | 1-5 minutes per package |
| **Selective Install** | Yes | Yes | Yes (per control) |
| **Multiple Machines** | Re-download each time | Copy installer | NuGet server/feed |
| **Samples Included** | Yes | Yes | No |
| **VS Integration** | Yes | Yes | No (manual setup) |
| **Best For** | Single developer | Enterprise/offline | CI/CD, specific controls |

## Choosing an Installation Method

### Use Web Installer When:
- You have reliable internet connection
- You want the latest version automatically
- Installation on a single machine
- Disk space for download is limited
- You want Visual Studio integration and samples

### Use Offline Installer When:
- No internet during installation
- Installing on multiple machines (enterprise)
- You need consistent version across team
- You need reproducible installations
- You want complete control over installation files

### Use NuGet Packages When:
- You need only specific controls
- Working in CI/CD environments
- Want lightweight project setup
- Prefer package-based dependency management
- Building containerized applications

## Troubleshooting Installation

### Common Issues

#### "Unable to find valid license or trial"

**Solution**: 
- Verify your Syncfusion account has active license/trial
- Contact account administrator if license not assigned
- Start a new trial if expired

#### "Unlock key is invalid or expired"

**Solution**:
- Generate new unlock key from account portal
- Use licensed unlock key for licensed installer
- Trial unlock keys expire after 30 days

#### "Another installation is in progress"

**Solution**:
- Kill `msiexec.exe` process in Task Manager
- Restart computer and retry

#### "Controlled folder access" error

**Solution**:
- Temporarily disable controlled folder access
- Or choose different installation directory not protected by Windows

See [troubleshooting.md](troubleshooting.md) for comprehensive error solutions.

## Next Steps

After installation:
1. **Add controls to your project**: See [adding-controls.md](adding-controls.md)
2. **Configure NuGet packages**: Refer to [nuget-package-management.md](nuget-package-management.md)
3. **Set up licensing**: Check [upgrading.md](upgrading.md) for license registration
