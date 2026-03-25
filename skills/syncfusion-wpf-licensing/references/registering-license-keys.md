# Registering Syncfusion License Keys

## Table of Contents
- [Overview](#overview)
- [General Registration Principles](#general-registration-principles)
- [Platform-Specific Registration](#platform-specific-registration)
- [Offline Validation](#offline-validation)
- [CI License Validation](#ci-license-validation)
- [Troubleshooting Registration](#troubleshooting-registration)

## Overview

After generating a Syncfusion license key, you must register it in your application code before initializing any Syncfusion components. This registration is done using the `SyncfusionLicenseProvider.RegisterLicense()` method.

**Key Points:**
- Registration happens in application startup code
- Must be called **before** any Syncfusion control is initialized
- Registration string is just the license key between double quotes
- Syncfusion.Licensing.dll must be referenced in your project
- Works offline - no internet connection required

## General Registration Principles

### Basic Registration Syntax

```csharp
using Syncfusion.Licensing;

SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

### Important Notes

1. **Place between double quotes:** The license key must be a string literal
2. **Reference Syncfusion.Licensing.dll:** Ensure this assembly is referenced
3. **Call early:** Register before initializing any Syncfusion components
4. **One-time registration:** Only need to call once per application startup
5. **Offline validation:** No internet connection needed during execution

### When to Register

Register the license key at the **earliest possible point** in your application lifecycle:

- **WPF:** In App constructor (App.xaml.cs)
- **Blazor Server:** In Program.cs before building the app
- **ASP.NET Core:** In Program.cs or Startup.cs
- **Console App:** First line in Main() method
- **MAUI:** In MauiProgram.cs
- **React/Angular/Vue:** In main entry point (index.js, main.ts)

## Platform-Specific Registration

### WPF (Windows Presentation Foundation)

Register in the App constructor in **App.xaml.cs**:

```csharp
using System.Windows;
using Syncfusion.Licensing;

namespace YourNamespace
{
    public partial class App : Application
    {
        public App()
        {
            // Register Syncfusion license BEFORE InitializeComponent
            SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
            
            InitializeComponent();
        }
    }
}
```

**Visual Basic (App.xaml.vb):**

```vb
Imports Syncfusion.Licensing

Partial Public Class App
    Inherits Application
    
    Private Sub New()
        ' Register Syncfusion License
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY")
    End Sub
End Class
```

**Creating App Constructor if Not Present:**

If App.xaml.cs doesn't have a constructor, create one:

```csharp
public partial class App : Application
{
    // Add this constructor
    public App()
    {
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    }
}
```

### Blazor Server

Register in **Program.cs** before building the application:

```csharp
using Syncfusion.Licensing;

var builder = WebApplication.CreateBuilder(args);

// Register Syncfusion license at application startup
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

// Add services to the container
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

// Configure the HTTP request pipeline
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();

app.MapBlazorHub();
app.MapFallbackToPage("/_Host");

app.Run();
```

### Blazor WebAssembly

Register in **Program.cs**:

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Licensing;

var builder = WebAssemblyHostBuilder.CreateDefault(args);

// Register Syncfusion license
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

builder.RootComponents.Add<App>("#app");

await builder.Build().RunAsync();
```

### ASP.NET Core MVC

Register in **Program.cs** (ASP.NET Core 6+):

```csharp
using Syncfusion.Licensing;

var builder = WebApplication.CreateBuilder(args);

// Register Syncfusion license
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

// Add services
builder.Services.AddControllersWithViews();

var app = builder.Build();

// Configure middleware
app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();
app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

**For ASP.NET Core 5 and earlier (Startup.cs):**

```csharp
using Syncfusion.Licensing;

public class Startup
{
    public Startup(IConfiguration configuration)
    {
        // Register Syncfusion license in constructor
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        Configuration = configuration;
    }

    public IConfiguration Configuration { get; }

    // Rest of Startup class...
}
```

### .NET MAUI

Register in **MauiProgram.cs**:

```csharp
using Syncfusion.Licensing;

namespace YourMauiApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            // Register Syncfusion license
            SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            return builder.Build();
        }
    }
}
```

### React

Register in your main entry point (**index.js** or **App.js**):

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { registerLicense } from '@syncfusion/ej2-base';
import App from './App';

// Register Syncfusion license before rendering
registerLicense('YOUR_LICENSE_KEY');

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

### Angular

Register in **main.ts**:

```typescript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { registerLicense } from '@syncfusion/ej2-base';
import { AppModule } from './app/app.module';

// Register Syncfusion license before bootstrapping
registerLicense('YOUR_LICENSE_KEY');

platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
```

### Vue.js

Register in **main.js** or **main.ts**:

```javascript
import { createApp } from 'vue';
import { registerLicense } from '@syncfusion/ej2-base';
import App from './App.vue';

// Register Syncfusion license before creating app
registerLicense('YOUR_LICENSE_KEY');

createApp(App).mount('#app');
```

### TypeScript

Register in **main.ts** or your application entry point:

```typescript
import { registerLicense } from '@syncfusion/ej2-base';

// Register license before using any Syncfusion components
registerLicense('YOUR_LICENSE_KEY');
```

### Windows Forms

Register in **Program.cs**:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Licensing;

namespace YourWindowsFormsApp
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // Register Syncfusion license
            SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new Form1());
        }
    }
}
```

### Console Application

Register at the start of **Main()** method:

```csharp
using System;
using Syncfusion.Licensing;

namespace YourConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // Register Syncfusion license first
            SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

            // Your application code
            Console.WriteLine("Syncfusion license registered successfully!");
        }
    }
}
```

## Offline Validation

One of the key benefits of Syncfusion's licensing system is that it works completely offline.

### How Offline Validation Works

1. **No Internet Required:** License validation happens locally during application execution
2. **No External Calls:** The validation logic is contained within Syncfusion.Licensing.dll
3. **Deployment Friendly:** Apps can be deployed to air-gapped systems without internet access

### Deployment to Offline Systems

You can deploy Syncfusion applications to systems that have no internet connection:

```csharp
// This works even without internet connection
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

**Benefits:**
- Deploy to secure networks without internet access
- No latency from external validation calls
- No dependency on external services
- Works in completely isolated environments

### When Does Validation Happen?

Validation occurs at **application startup** when you call `RegisterLicense()`:
- Validates the key format
- Checks version compatibility
- Checks platform compatibility
- Stores validation result for the session

No further validation or internet calls are made during the application lifecycle.

## CI License Validation

For continuous integration and continuous deployment (CI/CD) pipelines, you may want to validate licenses during the build process.

### Basic CI Registration

In your application code, register the license as usual:

```csharp
// Read from environment variable in CI
var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

Then set the environment variable in your CI pipeline:

**Azure Pipelines:**
```yaml
variables:
  SYNCFUSION_LICENSE_KEY: 'your_license_key_here'
```

**GitHub Actions:**
```yaml
env:
  SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}
```

**Jenkins:**
```groovy
environment {
    SYNCFUSION_LICENSE_KEY = credentials('syncfusion-license-key')
}
```

For detailed CI/CD validation setup including the LicenseKeyValidator utility, see `ci-license-validation.md`.

## Troubleshooting Registration

### Issue: "Could not load Syncfusion.Licensing.dll"

**Cause:** The Syncfusion.Licensing assembly is not referenced or not copied to output.

**Solution:**

1. **Add NuGet package:**
   ```bash
   Install-Package Syncfusion.Licensing
   ```

2. **Set Copy Local to True:**
   - Right-click on Syncfusion.Licensing.dll in References
   - Select Properties
   - Set "Copy Local" to `True`

3. **Verify in .csproj:**
   ```xml
   <PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />
   ```

### Issue: License Error Despite Registration

**Possible Causes:**

1. **Registration called too late:**
   - Ensure `RegisterLicense()` is called before any Syncfusion control initialization

2. **Version mismatch:**
   - License key version doesn't match package version
   - Generate new key for correct version

3. **Platform mismatch:**
   - Using WPF key in Blazor app (or vice versa)
   - Generate correct platform-specific key

4. **Key not copied correctly:**
   - Extra spaces or line breaks in key string
   - Copy key again carefully

### Issue: Works Locally But Not in Production

**Common Causes:**

1. **Missing Syncfusion.Licensing.dll in deployment:**
   - Ensure Copy Local is set to True
   - Verify DLL is in output folder
   - Check deployment package includes the DLL

2. **Environment variable not set:**
   - If using environment variables, ensure they're set in production
   - Verify configuration files are deployed correctly

3. **Version mismatch in deployment:**
   - Check all Syncfusion packages are same version
   - Verify license key matches deployed version

## Best Practices

### 1. Centralized Registration

Create a helper class for license registration:

```csharp
public static class SyncfusionLicenseHelper
{
    private static bool _isRegistered = false;

    public static void RegisterLicense()
    {
        if (!_isRegistered)
        {
            var licenseKey = GetLicenseKey();
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            _isRegistered = true;
        }
    }

    private static string GetLicenseKey()
    {
        // Try environment variable first
        var key = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
        
        // Fall back to configuration
        if (string.IsNullOrEmpty(key))
        {
            key = ConfigurationManager.AppSettings["SyncfusionLicenseKey"];
        }
        
        return key;
    }
}

// Usage in App.xaml.cs
public App()
{
    SyncfusionLicenseHelper.RegisterLicense();
    InitializeComponent();
}
```

### 2. Configuration-Based Keys

Store keys in configuration files:

```json
// appsettings.json
{
  "Syncfusion": {
    "LicenseKey": "YOUR_LICENSE_KEY"
  }
}
```

```csharp
// Program.cs
var licenseKey = builder.Configuration["Syncfusion:LicenseKey"];
SyncfusionLicenseProvider.RegisterLicense(licenseKey);
```

### 3. Environment-Specific Keys

Use different keys for development, staging, and production:

```json
// appsettings.Development.json
{
  "Syncfusion": {
    "LicenseKey": "DEV_LICENSE_KEY"
  }
}

// appsettings.Production.json
{
  "Syncfusion": {
    "LicenseKey": "PROD_LICENSE_KEY"
  }
}
```

### 4. Security Considerations

❌ **Don't:**
- Hardcode keys in source code committed to public repositories
- Store keys in plain text in version control
- Share keys publicly

✅ **Do:**
- Use environment variables
- Use secret management systems (Azure Key Vault, AWS Secrets Manager)
- Use user secrets during development
- Add keys to .gitignore

**Example using user secrets:**
```bash
dotnet user-secrets set "Syncfusion:LicenseKey" "YOUR_LICENSE_KEY"
```

## Validation

To verify your license registration is working:

1. Run your application
2. If no license error appears, registration was successful
3. If error appears, check:
   - License key is correct
   - `RegisterLicense()` is called before component initialization
   - Syncfusion.Licensing.dll is referenced and deployed
   - Version and platform match

For programmatic validation, see the `ValidateLicense()` method in `ci-license-validation.md`.
