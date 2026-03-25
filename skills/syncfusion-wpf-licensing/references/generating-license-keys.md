# Generating Syncfusion License Keys

A complete guide to generating license keys for Syncfusion Essential Studio across all platforms.

## Where to Generate License Keys

License keys can be generated from two primary locations on the Syncfusion website:

### For Licensed Customers
📍 **[License & Downloads](https://www.syncfusion.com/account/downloads)**
- For users with active paid licenses
- Access to all licensed versions
- Generate keys for any entitled platform and version

### For Trial Users
📍 **[Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)**
- For users with active 30-day trials
- Generate trial license keys
- Keys expire after 30 days

## Generation Process

### Step 1: Log In to Your Syncfusion Account

Navigate to the Syncfusion website and log in with your credentials.

### Step 2: Navigate to Downloads Section

- **Paid License:** Go to [License & Downloads](https://www.syncfusion.com/account/downloads)
- **Trial License:** Go to [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)

### Step 3: Select Platform and Version

License keys are **version and platform specific**. You must:
1. Choose the platform (WPF, Blazor, React, Angular, MAUI, etc.)
2. Select the exact version you're using (e.g., 26.2.4, 26.1.35)
3. Click "Generate License Key" or similar button

### Step 4: Copy the License Key

Once generated, copy the license key string. It will look something like:

```
Mgo+DSMBMAY9C3t2VFhhQlJBfV5AQmBIYVp/TGpJfl96cVxMZVVBJAtUQF1hSn5adEBjXH9acXVdQWhY
```

### Step 5: Register in Your Application

Use the key in your application code (see `registering-license-keys.md` for platform-specific examples).

## Using Claim License Key Feature

The "Claim License Key" page provides a streamlined way to generate license keys based on your account status.

### How to Access

From the Syncfusion licensing warning message that appears in your application:
1. Click the **"Claim License"** button
2. You'll be redirected to the Claim License Key page
3. The page will automatically detect your account status
4. Generate the appropriate key based on your entitlement

### License Key Generation by Account Status

#### Active License

If you have an active paid license:

✅ **Result:** Full license key generated immediately
- Valid for the version and platform you select
- No expiration date
- Full access to all features

**Process:**
1. Select your platform (WPF, Blazor, React, etc.)
2. Select your version
3. Click "Generate"
4. Copy the license key
5. Register in your application

#### Active Trial

If you have an active 30-day trial:

⏰ **Result:** Trial license key with expiration date
- Valid for 30 days from trial start
- Full feature access during trial period
- After expiration, must purchase or renew trial

**Process:**
1. Select platform and version
2. Generate trial key
3. Copy and register in application
4. Note the expiration date
5. Purchase license before trial expires

**Trial Expiration Handling:**
- After 30 days, trial key expires
- Application will show "trial expired" error
- Must purchase license and generate new key
- Or contact sales to extend trial in special cases

#### Expired License

If your license subscription has expired:

🔄 **Result:** Temporary 5-day license key
- Valid for 5 days only
- Provides time to renew subscription
- Limited to your previously entitled version

**Process:**
1. Generate temporary 5-day key
2. Use it in your application temporarily
3. Renew your license subscription
4. Generate new full license key
5. Replace temporary key with new key

**Renewal Process:**
- Visit Syncfusion renewal page
- Complete subscription renewal
- Generate new license key for latest version
- Update your application with new key

#### No License / No Trial / Expired Trial

If you don't have an active license or trial:

📝 **Result:** Redirect to trial/license acquisition
- Start a new 30-day trial, OR
- Purchase a license

**Options:**
1. **Start Free Trial:**
   - Click "Start Trial"
   - Get 30-day evaluation license
   - Generate trial license key
   - Full feature access for 30 days

2. **Purchase License:**
   - Click "Buy Now" or "Purchase"
   - Complete purchase process
   - Generate full license key
   - No expiration on key validity

## Platform and Version Considerations

### Selecting the Correct Platform

Choose the platform that matches your development framework:

| Your Framework | Platform to Select |
|---------------|-------------------|
| Windows Presentation Foundation | **WPF** |
| ASP.NET Core, Razor Pages | **ASP.NET Core** |
| Blazor Server or WebAssembly | **Blazor** |
| React applications | **React** |
| Angular applications | **Angular** |
| Vue.js applications | **Vue** |
| .NET MAUI apps | **MAUI** |
| Windows Forms | **Windows Forms** |
| Xamarin | **Xamarin** |
| TypeScript | **TypeScript** |

### Selecting the Correct Version

The version MUST match the Syncfusion package versions in your project:

**Example - Check your package version:**

```xml
<!-- In your .csproj or packages.config -->
<PackageReference Include="Syncfusion.SfGrid.WPF" Version="26.2.4" />
```

**Generate key for version:** 26.2.4

### Version Compatibility

⚠️ **Important:** License keys are NOT forward or backward compatible

- Key for v26.1.x will NOT work for v26.2.x
- Key for v26.2.4 will NOT work for v26.2.5
- Always generate keys for the exact version you're using

**Best Practice:** When upgrading Syncfusion versions:
1. Upgrade all Syncfusion packages to new version
2. Generate new license key for new version
3. Update `RegisterLicense()` call with new key
4. Test application to ensure no license errors

## Common Scenarios

### Scenario 1: Multiple Projects with Same Version

If you have multiple projects using the same Syncfusion version and platform:

✅ **Use the same license key** across all projects
- One key per version + platform combination
- No need to generate separate keys for each project
- Store key in shared configuration or environment variable

### Scenario 2: Multiple Projects with Different Platforms

If you have projects using different platforms (e.g., WPF and Blazor):

🔑 **Generate separate keys** for each platform
- WPF project: Generate WPF license key
- Blazor project: Generate Blazor license key
- Store both keys appropriately

### Scenario 3: Multiple Versions in Development

If you maintain multiple versions of your application:

🔑 **Generate separate keys** for each version
- v26.1.x project: Generate v26.1.x key
- v26.2.x project: Generate v26.2.x key
- Ensure each project uses the correct key

### Scenario 4: Upgrading from Trial to Paid

After purchasing a license:

1. ✅ **Option A - Licensed Installer:**
   - Uninstall trial installer
   - Download and install licensed version from License & Downloads
   - If using installed assemblies, no key registration needed

2. ✅ **Option B - NuGet Packages:**
   - Keep using NuGet packages
   - Generate new paid license key from License & Downloads
   - Replace trial key with paid key in your code
   - No need to reinstall anything

## Troubleshooting Key Generation

### Issue: Can't Find Generate License Key Option

**Solution:**
- Ensure you're logged in to your Syncfusion account
- Verify you have an active license or trial
- Navigate to the correct downloads section (License & Downloads or Trial & Downloads)
- Look for "Get License Key" or "Generate License Key" button

### Issue: Generated Key Doesn't Work

**Possible Causes:**
1. **Version Mismatch:** Key version doesn't match package version
   - Solution: Regenerate key for correct version

2. **Platform Mismatch:** Key platform doesn't match your framework
   - Solution: Regenerate key for correct platform (WPF, Blazor, etc.)

3. **Key Not Registered:** Forgot to call `RegisterLicense()`
   - Solution: Add registration code to application startup

4. **Key Copied Incorrectly:** Extra spaces or missing characters
   - Solution: Copy key again, ensure no trailing spaces

### Issue: Trial Key Expired

**Solution:**
- If within 30 days, key should work - check system date
- If after 30 days, trial has expired
- Purchase license from Syncfusion website
- Generate new paid license key
- Update application with new key

## Key Storage Best Practices

### Development Environment
```csharp
// Option 1: Configuration file (appsettings.json)
{
  "Syncfusion": {
    "LicenseKey": "your_license_key_here"
  }
}

// Read in code
var licenseKey = Configuration["Syncfusion:LicenseKey"];
SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

### Production Environment
```csharp
// Option 2: Environment variable
var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

### Security Considerations

❌ **Don't:**
- Hardcode license keys in public repositories
- Commit license keys to version control
- Share license keys publicly

✅ **Do:**
- Use environment variables
- Use secure configuration management (Azure Key Vault, AWS Secrets Manager)
- Add license keys to .gitignore
- Store in user secrets during development

## Additional Resources

- **KB:** [How to generate license key for licensed products](https://support.syncfusion.com/kb/article/7898/how-to-generate-license-key-for-licensed-products)
- **KB:** [Which version Syncfusion license key should I use?](https://support.syncfusion.com/kb/article/7865/which-version-syncfusion-license-key-should-i-use-in-my-application)
