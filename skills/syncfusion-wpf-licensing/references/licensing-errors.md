# Syncfusion Licensing Errors and Solutions

## Table of Contents
- [Overview](#overview)
- [Current Error Messages (v20.3.0+)](#current-error-messages-v2030)
- [Legacy Error Messages (v16.2.0 - v20.3.0)](#legacy-error-messages-v1620---v2030)
- [Assembly Loading Errors](#assembly-loading-errors)
- [Quick Error Resolution Guide](#quick-error-resolution-guide)
- [Prevention Checklist](#prevention-checklist)

## Overview

Syncfusion displays licensing error popups when license validation fails. This guide covers all error types, their causes, and solutions across different Syncfusion versions.

**Error categories:**
- License key not registered
- Invalid license key
- Trial expired
- Platform mismatch
- Version mismatch
- Assembly loading issues

## Current Error Messages (v20.3.0+)

These error messages appear in Syncfusion versions 20.3.0 and later.

### Error 1: License Key Not Registered / Trial Expired

**Error Message:**
```
This application was built using a trial version of Syncfusion Essential Studio. 
You should include the valid license key to remove the license validation message permanently.
```

**When This Appears:**
- No license key has been registered in the application
- Trial license key has expired after 30 days
- `RegisterLicense()` was not called

**Solutions:**

#### For Licensed Users:

1. **Generate a valid license key:**
   - Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Select your platform and version
   - Click "Generate License Key"
   - Copy the generated key

2. **Register the key in your application:**
   ```csharp
   using Syncfusion.Licensing;
   
   // In App constructor (WPF) or Program.cs (other platforms)
   SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
   ```

3. **Verify placement:**
   - Ensure `RegisterLicense()` is called **before** any Syncfusion component initialization
   - For WPF: In App.xaml.cs constructor
   - For Blazor: In Program.cs before builder.Build()
   - For ASP.NET Core: In Program.cs startup

#### For Trial Users:

1. **Generate a trial license key:**
   - Go to [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
   - Select your platform and version
   - Generate trial key (valid for 30 days)

2. **Register the trial key:**
   ```csharp
   SyncfusionLicenseProvider.RegisterLicense("YOUR_TRIAL_KEY");
   ```

3. **Purchase before trial expires:**
   - Trial keys expire after 30 days
   - Purchase license from [Syncfusion sales](https://www.syncfusion.com/sales/teamlicense)
   - Generate new paid license key
   - Replace trial key with paid key

#### Using Claim License Button:

From the error popup, you can click **"Claim License"** to:
- Be redirected to the license key generation page
- Automatically detect your account status
- Generate appropriate key based on your entitlement

See `generating-license-keys.md` for detailed claim license workflow.

### Error 2: Invalid Key

**Error Message:**
```
The included Syncfusion license key is invalid.
```

**When This Appears:**
- License key format is incorrect
- Using a key from another Syncfusion version
- Using a key from another platform
- Key was copied incorrectly (extra spaces, missing characters)
- Using an expired trial key

**Solutions:**

1. **Verify key format:**
   - License keys are long Base64 strings
   - Should have no spaces, line breaks, or special characters
   - Example format: `Mgo+DSMBMAY9C3t2VFhhQlJBfV5AQmBIYVp/TGpJfl96cVxMZVVBJAtUQF1h...`

2. **Check version match:**
   ```xml
   <!-- In .csproj -->
   <PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
   ```
   - Generate key for version **26.2.4**
   - Key for v26.1.x won't work for v26.2.x

3. **Check platform match:**
   - WPF key won't work in Blazor app
   - React key won't work in Angular app
   - Generate platform-specific key

4. **Regenerate key:**
   - Go to [License & Downloads](https://www.syncfusion.com/account/downloads) or [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
   - Select correct platform and version
   - Generate new key
   - Copy carefully (use Copy button if available)
   - Replace in your code

5. **Verify registration code:**
   ```csharp
   // ✅ CORRECT
   SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
   
   // ❌ INCORRECT - Missing quotes
   SyncfusionLicenseProvider.RegisterLicense(YOUR_LICENSE_KEY);
   
   // ❌ INCORRECT - Extra spaces
   SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY ");
   ```

### Error 3: Platform Mismatch

**Error Message:**
```
The included Syncfusion license is invalid (Platform mismatch).
```

**When This Appears:**
- Using a license key generated for a different platform
- Example: Using WPF key in a Blazor application

**Platform Examples:**
- WPF key in Blazor app
- React key in Angular app
- ASP.NET Core key in MAUI app

**Solution:**

1. **Identify your platform:**
   - Check your project type
   - WPF = Windows Presentation Foundation
   - Blazor = Blazor Server or WebAssembly
   - React, Angular, Vue = Web frameworks

2. **Generate correct platform key:**
   - Go to license generation page
   - Select the **correct platform** for your project
   - Generate new key

3. **Register the new key:**
   ```csharp
   // For WPF applications
   SyncfusionLicenseProvider.RegisterLicense("WPF_LICENSE_KEY");
   
   // For Blazor applications
   SyncfusionLicenseProvider.RegisterLicense("BLAZOR_LICENSE_KEY");
   ```

### Error 4: Version Mismatch

**Error Message:**
```
The included Syncfusion license ({Registered Version}) is invalid for version {Required version}.
```

**Example:**
```
The included Syncfusion license (26.1.35) is invalid for version 26.2.4.
```

**When This Appears:**
- License key version doesn't match Syncfusion package version
- Using an older key after upgrading packages
- Using a newer key with older packages

**Solution:**

1. **Check your package versions:**
   ```bash
   # For NuGet packages
   dotnet list package | findstr Syncfusion
   ```
   
   Or check .csproj:
   ```xml
   <PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
   <PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />
   ```

2. **Ensure all Syncfusion packages use same version:**
   ```xml
   <!-- ✅ CORRECT - All same version -->
   <PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
   <PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />
   <PackageReference Include="Syncfusion.Themes.FluentDark.WPF" Version="26.2.4" />
   
   <!-- ❌ INCORRECT - Mixed versions -->
   <PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
   <PackageReference Include="Syncfusion.Licensing" Version="26.1.35" />
   ```

3. **Generate key for correct version:**
   - Go to license generation page
   - Select version that matches your packages (e.g., 26.2.4)
   - Generate new key
   - Update your registration code

4. **Update registration:**
   ```csharp
   // Replace old key with new version-specific key
   SyncfusionLicenseProvider.RegisterLicense("NEW_LICENSE_KEY_FOR_v26.2.4");
   ```

**Reference:** [KB - How to generate license key for licensed products](https://support.syncfusion.com/kb/article/7898/how-to-generate-license-key-for-licensed-products)

## Legacy Error Messages (v16.2.0 - v20.3.0)

These error messages appear in Syncfusion versions 16.2.0 through 20.3.0.

### Legacy Error 1: License Key Not Registered

**Error Message:**
```
This application was built using a trial version of Syncfusion Essential Studio. 
Please include a valid license to permanently remove this license validation message. 
You can also obtain a free 30 day evaluation license to temporarily remove this 
message during the evaluation period.
```

**Solution:** Same as current "License Key Not Registered" error above.

### Legacy Error 2: Invalid Key

**Error Message:**
```
The included Syncfusion license is invalid.
```

**Solution:** Same as current "Invalid Key" error above.

### Legacy Error 3: Trial Expired

**Error Message:**
```
Your Syncfusion trial license has expired.
```

**When This Appears:**
- Trial license key has expired after 30 days
- Trial period is over

**Solution:**

1. **Purchase a license:**
   - Visit [Syncfusion sales page](https://www.syncfusion.com/sales/teamlicense)
   - Purchase appropriate license for your needs

2. **Generate paid license key:**
   - After purchase, go to [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Generate license key for your platform and version

3. **Update registration:**
   ```csharp
   // Replace expired trial key with paid license key
   SyncfusionLicenseProvider.RegisterLicense("NEW_PAID_LICENSE_KEY");
   ```

### Legacy Error 4: Platform Mismatch

**Error Message:**
```
The included Syncfusion license is invalid (Platform mismatch).
```

**Solution:** Same as current "Platform Mismatch" error above.

### Legacy Error 5: Version Mismatch

**Error Message:**
```
The included Syncfusion license ({Registered Version}) is invalid for version {Required version}.
```

**Solution:** Same as current "Version Mismatch" error above.

## Assembly Loading Errors

### Error: "Could not load Syncfusion.Licensing.dll assembly version...?"

**When This Appears:**
- Syncfusion.Licensing.dll is not referenced in project
- Assembly is not being copied to output directory
- Version mismatch in assembly references
- Missing NuGet package installation

**Solutions:**

#### 1. Verify NuGet Package Installation

```bash
# Install Syncfusion.Licensing package
Install-Package Syncfusion.Licensing

# Or via .NET CLI
dotnet add package Syncfusion.Licensing
```

#### 2. Check Package References

Verify in .csproj or packages.config:

```xml
<!-- .csproj -->
<PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />

<!-- packages.config -->
<package id="Syncfusion.Licensing" version="26.2.4" targetFramework="net48" />
```

#### 3. Set Copy Local to True

**For project references:**
1. Right-click on Syncfusion.Licensing.dll in Solution Explorer under References
2. Select Properties
3. Set **"Copy Local"** to **True**

**Why this matters:**
- "Copy Local" determines if the assembly is copied to the output directory
- Without it, the DLL won't be in bin\Debug or bin\Release
- Application won't find the assembly at runtime

**Visual representation:**

```
Solution Explorer
└── References
    └── Syncfusion.Licensing
        Properties:
        └── Copy Local: True  ← Set this to True
```

#### 4. Verify Output Directory

Check that Syncfusion.Licensing.dll exists in your output folder:

```
YourProject/
├── bin/
│   ├── Debug/
│   │   ├── YourApp.exe
│   │   ├── Syncfusion.Licensing.dll  ← Should be here
│   │   └── Other Syncfusion DLLs
│   └── Release/
│       ├── YourApp.exe
│       ├── Syncfusion.Licensing.dll  ← Should be here
│       └── Other Syncfusion DLLs
```

#### 5. Ensure Version Consistency

All Syncfusion assemblies should be the same version:

```xml
<!-- ✅ CORRECT - All version 26.2.4 -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
<PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />
<PackageReference Include="Syncfusion.Themes.FluentDark.WPF" Version="26.2.4" />
```

#### 6. Clean and Rebuild

Sometimes cached files cause issues:

```bash
# In Visual Studio
1. Build > Clean Solution
2. Delete bin and obj folders manually
3. Build > Rebuild Solution

# Via command line
dotnet clean
Remove-Item -Recurse -Force bin, obj
dotnet build
```

#### 7. Check Deployment Configuration

For web applications, ensure the DLL is included in deployment:

**Web.config or csproj:**
```xml
<ItemGroup>
  <Content Include="bin\Syncfusion.Licensing.dll">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

**Reference:** [KB - How to resolve "Could not load assembly" errors](https://www.syncfusion.com/kb/4808/how-to-resolve-server-error-could-not-load-or-assembly-when-publishing-an-application)

## Quick Error Resolution Guide

Use this flowchart to quickly diagnose and fix licensing errors:

### Step 1: Identify Your Error

| Error Message Contains | Go To |
|------------------------|-------|
| "trial version" OR "not registered" | [License Key Not Registered](#error-1-license-key-not-registered--trial-expired) |
| "invalid" (no other details) | [Invalid Key](#error-2-invalid-key) |
| "Platform mismatch" | [Platform Mismatch](#error-3-platform-mismatch) |
| "invalid for version" | [Version Mismatch](#error-4-version-mismatch) |
| "trial license has expired" | [Trial Expired](#legacy-error-3-trial-expired) |
| "Could not load Syncfusion.Licensing.dll" | [Assembly Loading Errors](#assembly-loading-errors) |

### Step 2: Check Common Issues

Before generating new keys, verify:

- [ ] `RegisterLicense()` is called in code
- [ ] Registration happens before component initialization
- [ ] License key is in double quotes
- [ ] No extra spaces or line breaks in key
- [ ] Syncfusion.Licensing.dll is referenced
- [ ] Copy Local is set to True for Syncfusion.Licensing.dll

### Step 3: Verify Version and Platform

```bash
# Check your Syncfusion package versions
dotnet list package | findstr Syncfusion
```

- All packages should be the same version
- License key must match this exact version
- License key must match your platform (WPF, Blazor, React, etc.)

### Step 4: Generate and Register New Key

If issues persist:

1. Go to [License & Downloads](https://www.syncfusion.com/account/downloads) or [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
2. Select correct platform and version
3. Generate new key
4. Copy key carefully
5. Update `RegisterLicense()` call
6. Clean and rebuild solution
7. Test application

## Prevention Checklist

Prevent licensing errors before they occur:

### During Development

- [ ] Reference Syncfusion.Licensing package
- [ ] Set Copy Local to True for all Syncfusion assemblies
- [ ] Register license in application startup code
- [ ] Use same version for all Syncfusion packages
- [ ] Store license keys securely (environment variables, user secrets)
- [ ] Don't commit license keys to public repositories

### Before Upgrading Syncfusion

- [ ] Note current Syncfusion version
- [ ] Upgrade all Syncfusion packages to same new version
- [ ] Generate new license key for new version
- [ ] Update RegisterLicense() with new key
- [ ] Clean and rebuild solution
- [ ] Test application thoroughly

### Before Deployment

- [ ] Verify Syncfusion.Licensing.dll is in output folder
- [ ] Confirm license registration code is in deployed code
- [ ] Test in staging environment first
- [ ] Check all Syncfusion DLLs are same version
- [ ] Verify Copy Local is True for all Syncfusion references
- [ ] Ensure environment variables are set (if used)

### CI/CD Pipeline

- [ ] Store license key in CI secrets/variables
- [ ] Register license in application code
- [ ] Set up CI license validation (optional but recommended)
- [ ] Verify build output includes Syncfusion.Licensing.dll
- [ ] Test deployment in pre-production environment

## Additional Resources

- **Overview:** `overview-and-differences.md` - Understanding licensing system
- **Generation:** `generating-license-keys.md` - How to generate keys
- **Registration:** `registering-license-keys.md` - Platform-specific registration
- **CI/CD:** `ci-license-validation.md` - Build pipeline validation
