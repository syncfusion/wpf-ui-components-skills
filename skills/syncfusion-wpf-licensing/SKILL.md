---
name: syncfusion-wpf-licensing
description: Comprehensive guide for implementing, registering, and troubleshooting Syncfusion licensing across all platforms. Covers license key registration, license validation errors, RegisterLicense API, SyncfusionLicenseProvider configuration, trial vs. licensed versions, CI/CD build server setup, and platform-specific licensing requirements. Use this skill when users need help with Syncfusion license registration, licensing validation issues, or license-related troubleshooting.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Licensing

A comprehensive guide for generating, registering, and troubleshooting Syncfusion license keys across all platforms (WPF, Blazor, React, MAUI, Angular, Vue, ASP.NET Core, etc.).

## Overview

Starting with version 16.2.0.x, Syncfusion introduced a licensing system that applies to:
- **All evaluators** using trial installers
- **Paid customers** using NuGet packages from [nuget.org](https://www.nuget.org/)

If you use the evaluation installer or NuGet packages, you must register a valid license key in your application to avoid license validation messages.

**Key Points:**
- License keys are **version and platform specific**
- Registration is done **offline** (no internet connection required)
- Different from the installer unlock key
- Required for NuGet packages from nuget.org and trial installers
- NOT required for licensed installers (assemblies from licensed installation)

## When to Use This Skill

Use this skill immediately when users need to:

- **Generate license keys** for Syncfusion components
- **Register license keys** in their applications (WPF, Blazor, React, etc.)
- **Troubleshoot licensing errors** (trial expired, invalid key, platform/version mismatch)
- **Set up CI/CD license validation** (Azure Pipelines, GitHub Actions, Jenkins)
- **Upgrade from trial to licensed version**
- **Understand license vs unlock key differences**
- **Configure build server licensing**
- **Resolve "license validation message" errors**
- **Register Syncfusion accounts for NuGet.org users**
- **Validate licenses programmatically** using `ValidateLicense()` method

## Understanding License Types

### When License Registration is Required

| Syncfusion Source | License Registration Required? | Notes |
|-------------------|-------------------------------|-------|
| **NuGet packages** (nuget.org) | ✅ YES | Version and platform specific key required |
| **Trial installer** | ✅ YES | Free 30-day trial key required |
| **Licensed installer** | ❌ NO | No registration needed |

### License Key vs Unlock Key

- **License Key**: Registered in application code using `SyncfusionLicenseProvider.RegisterLicense()`
- **Unlock Key**: Used to unlock the Syncfusion installer (older system)
- These are **different keys** and serve different purposes

## Documentation and Navigation Guide

### Getting Started with Licensing
📄 **Read:** [references/overview-and-differences.md](references/overview-and-differences.md)
- Licensing system overview
- License key vs unlock key differences
- When license keys are required (NuGet, trial, licensed installers)
- Build server licensing requirements
- Platform and version specificity
- Registering keys in build environments

### Generating License Keys
📄 **Read:** [references/generating-license-keys.md](references/generating-license-keys.md)
- Where to generate keys (License & Downloads, Trial & Downloads)
- Using the "Claim License Key" feature
- Handling active licenses vs active trials
- Dealing with expired licenses (temporary 5-day keys)
- Platform and version selection guidance

### Registering License Keys
📄 **Read:** [references/registering-license-keys.md](references/registering-license-keys.md)
- Platform-specific registration code placement
- WPF: App.xaml.cs constructor registration (C# and VB)
- Blazor, React, Angular, Vue, MAUI registration patterns
- Syncfusion.Licensing.dll requirements
- Offline validation capabilities
- Registration code examples

### Troubleshooting Licensing Errors
📄 **Read:** [references/licensing-errors.md](references/licensing-errors.md)
- "License key not registered" error
- "Invalid key" error
- "Trial expired" error
- "Platform mismatch" error
- "Version mismatch" error
- "Could not load Syncfusion.Licensing.dll" error
- Solutions for each error type
- Copy Local settings for assemblies
- Error messages by version (16.2.0+ vs 20.3.0+)

### CI/CD License Validation
📄 **Read:** [references/ci-license-validation.md](references/ci-license-validation.md)
- LicenseKeyValidator utility setup and usage
- Azure Pipelines (YAML and Classic) integration
- GitHub Actions integration
- Jenkins pipeline integration
- ValidateLicense() method for programmatic validation
- Unit test project validation approach
- Preventing licensing errors during deployment

### Upgrading and Account Setup
📄 **Read:** [references/upgrading-and-account-setup.md](references/upgrading-and-account-setup.md)
- Upgrading from trial version after purchasing a license
- Uninstall and reinstall process
- Replacing trial keys with paid license keys
- Registering Syncfusion account for NuGet.org users
- Starting free 30-day trials
- Internet connection requirements (offline validation works)

## Quick Start Example

### WPF Application

```csharp
// In App.xaml.cs
using Syncfusion.Licensing;

public partial class App : Application
{
    public App()
    {
        // Register Syncfusion license key BEFORE initializing any components
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE");
        
        InitializeComponent();
    }
}
```

### Blazor Server

```csharp
// In Program.cs
using Syncfusion.Licensing;

var builder = WebApplication.CreateBuilder(args);

// Register license key at application startup
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE");

// ... rest of your configuration
```

### React/Angular/Vue

```javascript
// In your main entry point (e.g., index.js, main.ts)
import { registerLicense } from '@syncfusion/ej2-base';

// Register license key before rendering components
registerLicense('YOUR_LICENSE_KEY_HERE');
```

## Common Licensing Scenarios

### Scenario 1: Trial User Setup
1. Register for a free Syncfusion account
2. Start a 30-day trial from the account portal
3. Generate trial license key from Trial & Downloads section
4. Register the key in your application using `RegisterLicense()`
5. Trial key expires after 30 days

### Scenario 2: Licensed User Setup
1. Log in to your Syncfusion account
2. Go to License & Downloads section
3. Generate license key for your specific version and platform
4. Register the key in your application using `RegisterLicense()`
5. Key is valid for the specified version and platform

### Scenario 3: NuGet.org User Setup
1. Using Syncfusion packages from nuget.org requires license registration
2. Register for a Syncfusion account if you don't have one
3. Start a trial or purchase a license
4. Generate and register the license key
5. Without registration, you'll see license validation warnings

### Scenario 4: Build Server / CI/CD
1. License registration is required on build servers using NuGet packages
2. Use any developer license to generate keys for build environments
3. Register the key in your application code
4. Optionally set up CI license validation to catch errors early
5. No internet connection needed during build (offline validation)

### Scenario 5: Upgrading from Trial
1. Purchase a Syncfusion license
2. **Option A**: Uninstall trial installer, install licensed version from License & Downloads
3. **Option B**: If using NuGet, generate paid license key and replace trial key
4. Licensed installer assemblies don't require key registration
5. NuGet packages always require key registration

## Key Concepts

### Platform and Version Specificity
- License keys are **version-specific**: Key for v26.1.35 won't work for v26.2.4
- License keys are **platform-specific**: WPF key won't work for Blazor
- Generate separate keys for each platform/version combination
- Use the correct version in your `RegisterLicense()` call

### Offline Validation
- License validation happens **offline** during application execution
- No internet connection required for apps to run
- You can deploy to air-gapped systems
- Validation checks happen at application startup

### Build Servers
- Build servers using NuGet packages need license registration
- Use any developer license to generate keys for CI/CD
- Licensed installer assemblies on build servers don't need keys
- Set up CI validation to prevent deployment errors

### Syncfusion.Licensing.dll
- Required assembly for license registration
- Ensure it's referenced in projects using `RegisterLicense()`
- Set "Copy Local" to `true` to include in output
- Available via NuGet: `Syncfusion.Licensing`

## Registration Best Practices

1. **Register early**: Call `RegisterLicense()` before initializing any Syncfusion components
2. **One registration per app**: Only need to register once at startup
3. **Use string literal**: Place license key between double quotes in code
4. **Version matching**: Ensure all Syncfusion packages use the same version
5. **Environment variables**: Consider storing keys in environment variables or secure configuration
6. **Don't commit keys**: Avoid committing license keys to public repositories
7. **CI/CD validation**: Set up automated validation in build pipelines

## Error Prevention Checklist

Before deploying your application:

- [ ] License key generated for correct version and platform
- [ ] `RegisterLicense()` called before any Syncfusion component initialization
- [ ] All Syncfusion NuGet packages are the same version
- [ ] Syncfusion.Licensing.dll is referenced and has "Copy Local" = true
- [ ] License key matches the version of Syncfusion packages in use
- [ ] For CI/CD: License validation configured in build pipeline
- [ ] License key stored securely (not hardcoded in public repos)

## Related Skills

- [Implementing Syncfusion WPF Components](../implementing-syncfusion-wpf-components/) - WPF-specific component implementation
- [Implementing Syncfusion Blazor Components](../implementing-syncfusion-blazor-components/) - Blazor components
- [Implementing Syncfusion React Components](../implementing-syncfusion-react-components/) - React components
