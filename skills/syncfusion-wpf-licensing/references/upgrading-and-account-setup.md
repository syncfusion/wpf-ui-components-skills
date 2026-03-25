# Upgrading from Trial and Account Setup

## Table of Contents
- [Upgrading from Trial to Licensed Version](#upgrading-from-trial-to-licensed-version)
- [Registering Syncfusion Account for NuGet.org Users](#registering-syncfusion-account-for-nugetorg-users)
- [Internet Connection Requirements](#internet-connection-requirements)
- [Common Upgrade Scenarios](#common-upgrade-scenarios)
- [Troubleshooting Upgrades](#troubleshooting-upgrades)

## Upgrading from Trial to Licensed Version

After purchasing a Syncfusion license, you need to upgrade from the trial version to remove the trial limitations and licensing warnings.

### Two Upgrade Options

You have two main options for upgrading from trial to licensed version:

#### Option A: Uninstall and Reinstall (Recommended for Installed Assemblies)

**When to use:** If you're referencing Syncfusion assemblies from the installed location (not NuGet).

**Steps:**

1. **Uninstall the trial version:**
   - Windows: Control Panel → Programs and Features
   - Find "Syncfusion Essential Studio" with trial version number
   - Uninstall completely

2. **Download the licensed installer:**
   - Log in to your Syncfusion account
   - Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Download the installer for your platform and version

3. **Install the licensed version:**
   - Run the downloaded installer
   - Complete the installation process
   - No license key registration needed in code when using installed assemblies

4. **Update your project references** (if needed):
   - If assembly paths changed, update references in your project
   - Clean and rebuild your solution

**Benefits:**
- No license key registration required in code
- Cleanest upgrade path
- Removes all trial version files

#### Option B: Replace License Key (Recommended for NuGet Packages)

**When to use:** If you're using Syncfusion NuGet packages from [nuget.org](https://www.nuget.org/).

**Steps:**

1. **Keep your existing project setup:**
   - No need to uninstall anything
   - Continue using NuGet packages

2. **Generate a paid license key:**
   - Log in to your Syncfusion account
   - Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Select your platform and version
   - Click "Generate License Key"
   - Copy the generated key

3. **Replace the trial key in your code:**
   ```csharp
   // Before (trial key)
   SyncfusionLicenseProvider.RegisterLicense("TRIAL_LICENSE_KEY");
   
   // After (paid license key)
   SyncfusionLicenseProvider.RegisterLicense("PAID_LICENSE_KEY");
   ```

4. **Clean and rebuild:**
   ```bash
   dotnet clean
   dotnet build
   ```

5. **Test your application:**
   - Run the application
   - Verify no license warnings appear
   - Confirm full feature access

**Benefits:**
- No reinstallation needed
- Quick and simple upgrade
- Keep existing project structure

### Important Notes

**License Registration Rules:**

| Assembly Source | After Purchasing License | License Registration Needed? |
|----------------|-------------------------|------------------------------|
| **Licensed installer** | No uninstall/reinstall needed if using NuGet | ❌ NO (for installed assemblies) |
| **NuGet packages** | Keep using NuGet packages | ✅ YES (replace trial key with paid key) |
| **Trial installer** | Uninstall and install licensed version | ❌ NO (after installing licensed version) |

**Key Point:** License registration is NOT required if you reference Syncfusion assemblies from the licensed installer. Registration is only required for:
- Evaluators/trial users
- Users of NuGet packages from [nuget.org](https://www.nuget.org/)

### Version Considerations

When upgrading, ensure version consistency:

```xml
<!-- Check current versions -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.1.35" />
<PackageReference Include="Syncfusion.Licensing" Version="26.1.35" />

<!-- Generate license key for version 26.1.35 -->
```

If you upgrade Syncfusion packages to a new version, generate a new license key for that version:

```xml
<!-- After upgrading packages -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
<PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />

<!-- Generate NEW license key for version 26.2.4 -->
```

## Registering Syncfusion Account for NuGet.org Users

If you've been using Syncfusion assemblies directly from [NuGet.org](https://www.nuget.org/) without a Syncfusion account, you need to register an account to obtain a license key.

### Who Needs to Register?

You need a Syncfusion account if you:
- Use Syncfusion NuGet packages from nuget.org
- Don't have a Syncfusion account yet
- Need a license key for your application
- Want to start a free 30-day trial

### Registration Steps

#### Step 1: Create a Syncfusion Account

1. **Go to the registration page:**
   - URL: [https://www.syncfusion.com/account/register](https://www.syncfusion.com/account/register)

2. **Fill in your information:**
   - Email address
   - Name
   - Company (optional)
   - Create password

3. **Verify your email:**
   - Check your email for verification link
   - Click the link to verify your account

4. **Complete registration:**
   - Your Syncfusion account is now active

#### Step 2: Start a Trial

1. **Go to the trials page:**
   - URL: [https://syncfusion.com/account/manage-trials/start-trials](https://syncfusion.com/account/manage-trials/start-trials)

2. **Select the platform:**
   - Choose the platform you're using (WPF, Blazor, React, etc.)
   - Click "Start Trial" or "Activate Trial"

3. **Trial activation:**
   - Your 30-day trial period begins
   - You now have access to license key generation

#### Step 3: Generate License Key

1. **Go to Trial & Downloads:**
   - URL: [https://www.syncfusion.com/account/manage-trials/downloads](https://www.syncfusion.com/account/manage-trials/downloads)

2. **Generate your license key:**
   - Select your platform (WPF, Blazor, etc.)
   - Select your version
   - Click "Generate License Key"
   - Copy the generated key

3. **Register the key in your application:**
   ```csharp
   using Syncfusion.Licensing;
   
   SyncfusionLicenseProvider.RegisterLicense("YOUR_TRIAL_LICENSE_KEY");
   ```

### Trial Period Details

**Duration:** 30 days from trial activation

**Features:**
- Full access to all Syncfusion components
- No feature limitations during trial
- Same functionality as paid license

**After Trial Expires:**
- Trial license key becomes invalid
- You'll see "trial expired" error message
- Need to purchase a license to continue

**To Continue After Trial:**
1. **Purchase a license:**
   - Visit [Syncfusion sales page](https://www.syncfusion.com/sales/teamlicense)
   - Choose the appropriate license for your needs

2. **Generate paid license key:**
   - After purchase, go to [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Generate license key for your platform and version

3. **Update your application:**
   - Replace trial key with paid license key in your code
   - Rebuild and test

## Internet Connection Requirements

A common question about Syncfusion licensing is whether an internet connection is required.

### License Validation is Offline

**Important:** Syncfusion license validation is done **offline** during application execution and does **NOT require internet access**.

### How It Works

1. **Registration happens at startup:**
   ```csharp
   SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
   ```

2. **Validation is local:**
   - The license key is validated against the Syncfusion assemblies
   - Validation logic is contained within Syncfusion.Licensing.dll
   - No external calls or internet requests are made

3. **Runtime execution:**
   - Application runs normally without internet
   - No continuous validation or checking
   - No communication with Syncfusion servers

### Deployment to Offline Systems

Apps registered with a Syncfusion license key can be deployed to systems without internet:

**Supported scenarios:**
- ✅ Air-gapped networks
- ✅ Secure isolated environments
- ✅ Systems without internet access
- ✅ Offline workstations
- ✅ Restricted network environments

**Example deployment:**
```csharp
// This works even without internet connection
public App()
{
    SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    InitializeComponent();
}
```

### When Internet IS Required

Internet connection is only needed for:

1. **Downloading installers** - From Syncfusion website
2. **Generating license keys** - From your Syncfusion account portal
3. **Downloading NuGet packages** - From nuget.org (one-time download)
4. **Account registration** - Creating/managing your Syncfusion account

Once you have:
- License key generated
- NuGet packages downloaded/restored
- Application built

Your application runs completely offline with no internet dependency.

## Common Upgrade Scenarios

### Scenario 1: Trial User Purchasing License (Using NuGet)

**Current state:**
- Using NuGet packages from nuget.org
- Trial license key registered in code
- Trial period about to expire or expired

**Upgrade steps:**
1. Purchase Syncfusion license
2. Log in to Syncfusion account
3. Go to License & Downloads
4. Generate paid license key for your version and platform
5. Replace trial key with paid key:
   ```csharp
   // Replace this:
   SyncfusionLicenseProvider.RegisterLicense("TRIAL_KEY");
   
   // With this:
   SyncfusionLicenseProvider.RegisterLicense("PAID_KEY");
   ```
6. Clean and rebuild solution
7. Test application - no more trial warnings

**Time required:** 5-10 minutes

### Scenario 2: Trial User Purchasing License (Using Trial Installer)

**Current state:**
- Using assemblies from trial installer
- Trial period expired
- Want to upgrade to licensed version

**Upgrade steps:**
1. Purchase Syncfusion license
2. Uninstall trial version from Control Panel
3. Download licensed installer from License & Downloads
4. Install licensed version
5. Update project references if needed
6. Remove `RegisterLicense()` call from code (not needed with licensed installer)
7. Clean and rebuild solution
8. Test application

**Time required:** 20-30 minutes

### Scenario 3: Moving from Licensed Installer to NuGet

**Current state:**
- Using assemblies from licensed installer
- No license registration in code
- Want to switch to NuGet packages

**Upgrade steps:**
1. Generate license key from License & Downloads
2. Replace installer assembly references with NuGet packages:
   ```bash
   # Remove old references
   # Add NuGet packages
   Install-Package Syncfusion.SfGrid.WPF -Version 26.2.4
   Install-Package Syncfusion.Licensing -Version 26.2.4
   ```
3. Add license registration to code:
   ```csharp
   using Syncfusion.Licensing;
   
   public App()
   {
       SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
       InitializeComponent();
   }
   ```
4. Clean and rebuild
5. Test application

**Time required:** 15-20 minutes

### Scenario 4: Upgrading Syncfusion Version

**Current state:**
- Using Syncfusion v26.1.35
- License key for v26.1.35
- Want to upgrade to v26.2.4

**Upgrade steps:**
1. Update all Syncfusion NuGet packages to v26.2.4:
   ```bash
   Update-Package Syncfusion.SfGrid.WPF -Version 26.2.4
   Update-Package Syncfusion.Licensing -Version 26.2.4
   # Update all other Syncfusion packages
   ```
2. Generate NEW license key for v26.2.4
3. Update license registration:
   ```csharp
   // Replace old key
   SyncfusionLicenseProvider.RegisterLicense("NEW_v26.2.4_KEY");
   ```
4. Clean and rebuild
5. Test application
6. Update any breaking changes from release notes

**Time required:** 15-30 minutes

### Scenario 5: Multiple Developers/Environments

**Current state:**
- Team of developers
- Multiple environments (dev, staging, prod)
- Need consistent licensing

**Setup approach:**

1. **Generate one license key** for the version/platform

2. **Store securely:**
   ```json
   // appsettings.json (don't commit to public repo)
   {
     "Syncfusion": {
       "LicenseKey": "YOUR_LICENSE_KEY"
     }
   }
   ```

3. **Use environment variables in production:**
   ```csharp
   var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY") 
                   ?? Configuration["Syncfusion:LicenseKey"];
   SyncfusionLicenseProvider.RegisterLicense(licenseKey);
   ```

4. **Set environment variable per environment:**
   - Dev: Use appsettings.Development.json
   - Staging: Use environment variable
   - Production: Use secure secret management

5. **Share with team:**
   - Add to team documentation
   - Store in password manager
   - Use Azure Key Vault or similar for automation

**Benefits:**
- One key for whole team
- Consistent across environments
- Secure storage
- Easy updates

## Troubleshooting Upgrades

### Issue: Still Seeing Trial Message After Upgrade

**Possible causes:**

1. **Old trial key still in code:**
   - Search project for old `RegisterLicense()` calls
   - Ensure you replaced with new paid key

2. **Cached builds:**
   ```bash
   # Solution:
   dotnet clean
   Remove-Item -Recurse -Force bin, obj
   dotnet build
   ```

3. **Multiple registration calls:**
   - Check for duplicate `RegisterLicense()` calls
   - Ensure only one registration at startup

4. **Version mismatch:**
   - Verify license key version matches package version
   - Generate new key if versions don't match

### Issue: Application Not Finding License Key

**Solutions:**

1. **Verify registration placement:**
   ```csharp
   // ✅ CORRECT - Before InitializeComponent
   public App()
   {
       SyncfusionLicenseProvider.RegisterLicense("KEY");
       InitializeComponent();
   }
   
   // ❌ INCORRECT - After InitializeComponent
   public App()
   {
       InitializeComponent();
       SyncfusionLicenseProvider.RegisterLicense("KEY");  // Too late!
   }
   ```

2. **Check Syncfusion.Licensing.dll reference:**
   - Ensure package is installed
   - Verify "Copy Local" is true

3. **Check configuration:**
   ```csharp
   // If using configuration, ensure file is being read
   var config = new ConfigurationBuilder()
       .AddJsonFile("appsettings.json")
       .Build();
   var key = config["Syncfusion:LicenseKey"];
   ```

### Issue: "Invalid Key" After Purchasing

**Likely causes:**

1. **Generated key for wrong version:**
   - Check package versions in .csproj
   - Regenerate key for correct version

2. **Generated key for wrong platform:**
   - Check project type (WPF, Blazor, etc.)
   - Regenerate key for correct platform

3. **Key copied incorrectly:**
   - Copy key again from account portal
   - Check for extra spaces or line breaks

### Issue: Multiple Versions in Use

**Problem:** Using different Syncfusion versions in different projects

**Solution:**

```xml
<!-- Standardize versions across all projects -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
<PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />

<!-- Generate one key per version -->
<!-- Project A (v26.2.4): Use key for v26.2.4 -->
<!-- Project B (v26.1.35): Use key for v26.1.35 -->
```

**Best practice:** Standardize on one version across your organization when possible.

## Post-Upgrade Checklist

After upgrading from trial to licensed version:

- [ ] License key updated in code
- [ ] Application builds without errors
- [ ] No license warning messages on startup
- [ ] All Syncfusion components working correctly
- [ ] Version numbers match between packages and license key
- [ ] Platform matches between project type and license key
- [ ] License key stored securely (not in public repository)
- [ ] Team members informed of new license key
- [ ] CI/CD pipelines updated with new key
- [ ] Documentation updated with license information

## Support Resources

If you encounter issues during upgrade:

- **Documentation:** [Syncfusion Licensing Help](https://help.syncfusion.com/)
- **Support Portal:** [https://support.syncfusion.com/](https://support.syncfusion.com/)
- **Community Forums:** [https://www.syncfusion.com/forums](https://www.syncfusion.com/forums)
- **Direct Support:** Available to licensed customers

For license-related questions:
- **Email:** [licensing@syncfusion.com](mailto:licensing@syncfusion.com)
- **Sales:** [sales@syncfusion.com](mailto:sales@syncfusion.com)
