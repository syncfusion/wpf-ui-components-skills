# Syncfusion Licensing Overview

## Table of Contents
- [Introduction](#introduction)
- [Licensing System Overview](#licensing-system-overview)
- [License Key vs Unlock Key](#license-key-vs-unlock-key)
- [When License Keys Are Required](#when-license-keys-are-required)
- [Build Server and CI/CD Licensing](#build-server-and-cicd-licensing)
- [Platform and Version Specificity](#platform-and-version-specificity)
- [Key Takeaways](#key-takeaways)

## Introduction

Syncfusion introduced a new licensing system starting with version 16.2.0.x of Essential Studio. This guide explains the licensing requirements, differences between license types, and when license registration is necessary.

**Who This Applies To:**
- All users with evaluation/trial installers
- Paid customers using NuGet packages from [nuget.org](https://www.nuget.org/)

**Who This Does NOT Apply To:**
- Paid customers using assemblies from licensed installers (no registration needed)

## Licensing System Overview

Starting with version 16.2.0.x, if you use Syncfusion assemblies from:
- **Evaluation installer**, OR
- **NuGet feed** (nuget.org)

You MUST include the corresponding platform and version license key in your projects.

### What Happens Without a License Key?

If a license key is not registered, you'll see this error message:

```
This application was built using a trial version of Syncfusion Essential Studio. 
Please include a valid license to permanently remove this license validation message. 
You can also obtain a free 30 day evaluation license to temporarily remove this 
message during the evaluation period.
```

## License Key vs Unlock Key

**IMPORTANT:** The Syncfusion license key is completely different from the installer unlock key.

### Unlock Key (Old System)
- Used to unlock the Syncfusion installer
- Entered during installation process
- Unlocks the installer files
- **Not used in application code**

### License Key (New System)
- Generated from Syncfusion website
- **Registered in your application code** using `SyncfusionLicenseProvider.RegisterLicense()`
- Version and platform specific
- Required for NuGet packages and trial installers
- Works offline (no internet connection required)

**Key Difference:** You use the unlock key to install Syncfusion on your machine, but you use the license key in your application code to register the license.

**Reference:** [KB Article - Difference between unlock key and licensing key](https://www.syncfusion.com/kb/8950/difference-between-the-unlock-key-and-licensing-key)

## When License Keys Are Required

### Source of Assemblies Determines Requirements

| Assembly Source | License Registration Required? | Where to Get License Key |
|----------------|-------------------------------|-------------------------|
| **NuGet packages** (nuget.org) | ✅ **YES** | [License & Downloads](https://www.syncfusion.com/account/downloads) or [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads) |
| **Trial installer** | ✅ **YES** | [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads) |
| **Licensed installer** | ❌ **NO** | Not applicable - no registration needed |

### NuGet Packages from nuget.org

If you install Syncfusion packages from NuGet.org:

```bash
Install-Package Syncfusion.SfGrid.WPF
```

You MUST:
1. Generate a license key for your version and platform
2. Register it in your application using `SyncfusionLicenseProvider.RegisterLicense()`
3. Include `Syncfusion.Licensing` NuGet package

### Trial Installer

If you download and install the trial/evaluation version:
1. You get a 30-day trial period
2. Generate a trial license key from Trial & Downloads
3. Register the key in your application
4. After 30 days, the trial expires and you'll need to purchase a license

### Licensed Installer

If you download and install the full licensed version:
- No license registration is required in your code
- The licensing is handled by the installer itself
- You can use Syncfusion assemblies directly without calling `RegisterLicense()`

**However:** Even licensed customers must register keys if they use NuGet packages from nuget.org instead of installed assemblies.

## Build Server and CI/CD Licensing

License registration requirements on build servers depend on the assembly source:

### Build Server Scenarios

| Scenario | License Key Required? | Solution |
|----------|----------------------|----------|
| Using NuGet packages from nuget.org | ✅ **YES** | Register license key in application code |
| Using trial installer assemblies | ✅ **YES** | Register trial license key in application code |
| Using licensed installer assemblies | ❌ **NO** | No registration needed |

### Best Practices for Build Servers

1. **Use any developer license** to generate keys for build environments
2. **Register the key in application code** before building
3. **Don't require internet connection** - validation works offline
4. **Consider CI validation** - Use LicenseKeyValidator utility to catch errors early
5. **Store keys securely** - Use environment variables or secret management

### Example Build Server Setup

If your build server uses NuGet packages:

```csharp
// In your application startup code
using Syncfusion.Licensing;

public class Startup
{
    public Startup()
    {
        // Read from environment variable or configuration
        var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
        SyncfusionLicenseProvider.RegisterLicense(licenseKey);
    }
}
```

Then set the environment variable on your build server:
```bash
SYNCFUSION_LICENSE_KEY=your_license_key_here
```

## Platform and Version Specificity

### Version-Specific Keys

License keys are tied to specific Syncfusion versions:

- Key for **v26.1.35** will NOT work for **v26.2.4**
- Key for **v25.x** will NOT work for **v26.x**
- Generate new keys when upgrading Syncfusion versions

**Why?** This ensures that license validation matches the version you're entitled to use.

### Platform-Specific Keys

License keys are tied to specific platforms:

- **WPF** key won't work for **Blazor**
- **React** key won't work for **Angular**
- **ASP.NET Core** key won't work for **MAUI**

**Why?** Different platforms have different licensing models and entitlements.

### Generating Keys for Multiple Versions/Platforms

If you use Syncfusion across multiple projects:

1. **Different platforms**: Generate separate keys for each platform
   - Example: One key for WPF, another for Blazor

2. **Different versions**: Generate separate keys for each version
   - Example: One key for v26.1.x projects, another for v26.2.x projects

3. **Organization**: Store keys in a secure configuration system
   - Use environment variables
   - Use Azure Key Vault or similar secret management
   - Never commit keys to public repositories

### Version Matching Best Practice

Ensure all Syncfusion packages in your project use the **same version**:

```xml
<!-- ✅ CORRECT - All packages same version -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
<PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />
<PackageReference Include="Syncfusion.Themes.FluentDark.WPF" Version="26.2.4" />

<!-- ❌ INCORRECT - Mixed versions will cause errors -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
<PackageReference Include="Syncfusion.Licensing" Version="26.1.35" />
```

## Key Takeaways

1. **License Key ≠ Unlock Key** - They serve different purposes
2. **NuGet from nuget.org = License Required** - Always register keys when using NuGet packages
3. **Licensed Installer = No License Required** - Installed assemblies don't need registration
4. **Offline Validation** - No internet connection needed during runtime
5. **Version + Platform Specific** - Generate keys for each combination
6. **Build Servers** - Use developer licenses to generate keys for CI/CD
7. **Trial Period** - 30 days for evaluation, then purchase required

## Common Questions

**Q: I have a licensed installer. Do I need to register a license key?**  
A: No, if you reference assemblies from the licensed installer, no key registration is needed.

**Q: I'm using NuGet packages and have a paid license. Do I still need a key?**  
A: Yes, NuGet packages from nuget.org always require license registration, even for paid customers.

**Q: Can I use the same license key across different versions?**  
A: No, license keys are version-specific. Generate a new key when upgrading versions.

**Q: Does my application need internet access for license validation?**  
A: No, license validation happens offline. No internet connection is required.

**Q: I'm getting a platform mismatch error. What's wrong?**  
A: You're using a license key for a different platform. Generate a new key for your specific platform (e.g., WPF, Blazor, React).

**Q: Can I use a developer's license key on a build server?**  
A: Yes, you can use any developer license to generate keys for build environments.
