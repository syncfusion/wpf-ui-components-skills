# CI/CD License Validation

## Table of Contents
- [Overview](#overview)
- [LicenseKeyValidator Utility](#licensekeyvalidator-utility)
- [Azure Pipelines Integration](#azure-pipelines-integration)
- [GitHub Actions Integration](#github-actions-integration)
- [Jenkins Integration](#jenkins-integration)
- [ValidateLicense Method](#validatelicense-method)
- [Unit Test Validation](#unit-test-validation)
- [Best Practices](#best-practices)

## Overview

Syncfusion license key validation in CI/CD services ensures that components are properly licensed during continuous integration processes. Validating the license key at the CI level prevents licensing errors during deployment.

**Benefits:**
- Catch licensing errors early in the build process
- Prevent deployment of applications with invalid licenses
- Fail builds when license validation fails
- Automate license verification across environments

**Validation Options:**
1. **LicenseKeyValidator Utility** - PowerShell-based validation tool
2. **ValidateLicense() Method** - Programmatic validation in code
3. **Unit Test Validation** - Test-based license verification

## LicenseKeyValidator Utility

The LicenseKeyValidator is a standalone utility for validating Syncfusion license keys in CI/CD pipelines.

### Download and Setup

1. **Download the utility:**
   - URL: [LicenseKeyValidator.zip](https://s3.amazonaws.com/files2.syncfusion.com/Installs/LicenseKeyValidation/LicenseKeyValidator.zip)
   - Download and extract to a location accessible by your CI system

2. **Extract contents:**
   ```
   LicenseKeyValidator/
   ├── LicenseKeyValidatorConsole.exe
   ├── LicenseKeyValidation.ps1
   └── Supporting DLLs
   ```

### Configure the PowerShell Script

Open **LicenseKeyValidation.ps1** in a text editor:

```powershell
# LicenseKeyValidation.ps1

# Replace the parameters with the desired platform, version, and actual license key
$result = & $PSScriptRoot"\LicenseKeyValidatorConsole.exe" `
    /platform:"WPF" `
    /version:"26.2.4" `
    /licensekey:"Your License Key"

Write-Host $result
```

**Parameters:**

| Parameter | Description | Example |
|-----------|-------------|---------|
| `/platform:` | Target Syncfusion platform | `"WPF"`, `"Blazor"`, `"React"`, `"Angular"` |
| `/version:` | Syncfusion version being used | `"26.2.4"`, `"26.1.35"` |
| `/licensekey:` | Your actual license key | `"Mgo+DSMBMAY9C3t2..."` |

**Update the script:**
1. Replace `"WPF"` with your platform
2. Replace `"26.2.4"` with your Syncfusion version
3. Replace `"Your License Key"` with your actual license key

**Example for Blazor:**
```powershell
$result = & $PSScriptRoot"\LicenseKeyValidatorConsole.exe" `
    /platform:"Blazor" `
    /version:"26.2.4" `
    /licensekey:"Mgo+DSMBMAY9C3t2VFhhQlJBfV5AQmBIYVp..."

Write-Host $result
```

### Supported Versions

This feature is supported from Syncfusion version **16.2.0.41** and later.

## Azure Pipelines Integration

### Azure Pipelines (YAML)

Configure license validation in your Azure Pipelines YAML file:

#### Step 1: Create User-Defined Variable

In Azure DevOps:
1. Go to your pipeline
2. Click **Variables**
3. Add a new variable:
   - **Name:** `LICENSE_VALIDATION`
   - **Value:** `D:\LicenseKeyValidator\LicenseKeyValidation.ps1` (path to your script)

Or define in YAML:
```yaml
variables:
  LICENSE_VALIDATION: 'D:\LicenseKeyValidator\LicenseKeyValidation.ps1'
```

#### Step 2: Add PowerShell Task

```yaml
pool:
  vmImage: 'windows-latest'

variables:
  LICENSE_VALIDATION: 'D:\LicenseKeyValidator\LicenseKeyValidation.ps1'

steps:
  - task: PowerShell@2
    inputs:
      targetType: filePath
      filePath: $(LICENSE_VALIDATION)
    displayName: 'Syncfusion License Validation'
    
  # Your other build steps
  - task: DotNetCoreCLI@2
    inputs:
      command: 'build'
      projects: '**/*.csproj'
    displayName: 'Build Project'
```

**Complete Example:**

```yaml
trigger:
  branches:
    include:
      - main
      - develop

pool:
  vmImage: 'windows-latest'

variables:
  LICENSE_VALIDATION: '$(Build.SourcesDirectory)\LicenseKeyValidator\LicenseKeyValidation.ps1'
  buildConfiguration: 'Release'

steps:
  # Checkout code
  - checkout: self
  
  # Validate Syncfusion license
  - task: PowerShell@2
    inputs:
      targetType: filePath
      filePath: $(LICENSE_VALIDATION)
    displayName: 'Syncfusion License Validation'
    continueOnError: false  # Fail build if validation fails
  
  # Restore dependencies
  - task: DotNetCoreCLI@2
    inputs:
      command: 'restore'
      projects: '**/*.csproj'
    displayName: 'Restore NuGet Packages'
  
  # Build project
  - task: DotNetCoreCLI@2
    inputs:
      command: 'build'
      projects: '**/*.csproj'
      arguments: '--configuration $(buildConfiguration)'
    displayName: 'Build Project'
  
  # Run tests
  - task: DotNetCoreCLI@2
    inputs:
      command: 'test'
      projects: '**/*Tests.csproj'
      arguments: '--configuration $(buildConfiguration)'
    displayName: 'Run Tests'
```

### Azure Pipelines (Classic)

For classic (GUI-based) Azure Pipelines:

#### Step 1: Create Variable

1. Open your pipeline in Azure DevOps
2. Click **Variables** tab
3. Add variable:
   - **Name:** `LICENSE_VALIDATION`
   - **Value:** `D:\LicenseKeyValidator\LicenseKeyValidation.ps1`

#### Step 2: Add PowerShell Task

1. Click **+** to add a task
2. Search for **PowerShell**
3. Configure:
   - **Display name:** Syncfusion License Validation
   - **Type:** File Path
   - **Script Path:** `$(LICENSE_VALIDATION)`
4. Save the pipeline

**Task configuration screenshot guidance:**
- Set **Type** to **"File Path"**
- Set **Script Path** to **`$(LICENSE_VALIDATION)`**
- Set **Continue on error** to **false** (fail build on validation error)

## GitHub Actions Integration

Integrate license validation into GitHub Actions workflows:

### Workflow Configuration

```yaml
name: Build and Validate

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: windows-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x'
      
      - name: Syncfusion License Validation
        shell: pwsh
        run: |
          ./LicenseKeyValidator/LicenseKeyValidation.ps1
        continue-on-error: false
      
      - name: Restore dependencies
        run: dotnet restore
      
      - name: Build
        run: dotnet build --configuration Release --no-restore
      
      - name: Test
        run: dotnet test --no-build --verbosity normal
```

### Using Secrets for License Key

Store the license key as a GitHub secret:

#### Step 1: Add Secret

1. Go to repository **Settings**
2. Click **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `SYNCFUSION_LICENSE_KEY`
5. Value: Your license key

#### Step 2: Update Script to Use Secret

Create a modified validation script that accepts parameters:

**LicenseKeyValidation-GitHub.ps1:**
```powershell
param(
    [string]$Platform = "WPF",
    [string]$Version = "26.2.4",
    [string]$LicenseKey
)

if ([string]::IsNullOrEmpty($LicenseKey)) {
    Write-Host "##[error] License key is required"
    exit 1
}

$result = & $PSScriptRoot"\LicenseKeyValidatorConsole.exe" `
    /platform:$Platform `
    /version:$Version `
    /licensekey:$LicenseKey

Write-Host $result

if ($LASTEXITCODE -ne 0) {
    Write-Host "##[error] License validation failed"
    exit 1
}
```

**GitHub Actions workflow:**
```yaml
- name: Syncfusion License Validation
  shell: pwsh
  env:
    LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}
  run: |
    ./LicenseKeyValidator/LicenseKeyValidation-GitHub.ps1 `
      -Platform "WPF" `
      -Version "26.2.4" `
      -LicenseKey $env:LICENSE_KEY
```

## Jenkins Integration

Integrate license validation into Jenkins pipelines:

### Declarative Pipeline

```groovy
pipeline {
    agent any
    
    environment {
        LICENSE_VALIDATION = 'D:\\LicenseKeyValidator\\LicenseKeyValidation.ps1'
        DOTNET_CLI_HOME = '/tmp/dotnet'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Syncfusion License Validation') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'pwsh ${LICENSE_VALIDATION}'
                    } else {
                        powershell """
                            & '${LICENSE_VALIDATION}'
                        """
                    }
                }
            }
        }
        
        stage('Restore') {
            steps {
                bat 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                bat 'dotnet build --configuration Release'
            }
        }
        
        stage('Test') {
            steps {
                bat 'dotnet test --configuration Release'
            }
        }
    }
    
    post {
        failure {
            echo 'Build failed - check license validation'
        }
    }
}
```

### Using Jenkins Credentials

Store license key as Jenkins credential:

#### Step 1: Add Credential

1. Go to **Jenkins** → **Manage Jenkins** → **Manage Credentials**
2. Add a **Secret text** credential
3. ID: `syncfusion-license-key`
4. Secret: Your license key

#### Step 2: Use in Pipeline

```groovy
pipeline {
    agent any
    
    environment {
        LICENSE_VALIDATION = 'D:\\LicenseKeyValidator\\LicenseKeyValidation.ps1'
    }
    
    stages {
        stage('Syncfusion License Validation') {
            steps {
                withCredentials([string(credentialsId: 'syncfusion-license-key', variable: 'LICENSE_KEY')]) {
                    powershell """
                        \$result = & 'D:\\LicenseKeyValidator\\LicenseKeyValidatorConsole.exe' `
                            /platform:"WPF" `
                            /version:"26.2.4" `
                            /licensekey:"${env.LICENSE_KEY}"
                        Write-Host \$result
                        if (\$LASTEXITCODE -ne 0) {
                            throw "License validation failed"
                        }
                    """
                }
            }
        }
        
        // Other stages...
    }
}
```

## ValidateLicense Method

For programmatic validation within your application or tests, use the `ValidateLicense()` method.

### Method Signature

```csharp
bool ValidateLicense(Platform platform)
bool ValidateLicense(Platform platform, out string validationMessage)
```

### Basic Usage

```csharp
using Syncfusion.Licensing;

// Register license first
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

// Validate for specific platform
bool isValid = SyncfusionLicenseProvider.ValidateLicense(Platform.WPF);

if (isValid)
{
    Console.WriteLine("License is valid");
}
else
{
    Console.WriteLine("License validation failed");
}
```

### With Validation Message

```csharp
using Syncfusion.Licensing;

// Register license
SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

// Validate with detailed message
bool isValid = SyncfusionLicenseProvider.ValidateLicense(
    Platform.WPF, 
    out string validationMessage
);

if (isValid)
{
    Console.WriteLine($"Success: {validationMessage}");
}
else
{
    Console.WriteLine($"Validation failed: {validationMessage}");
}
```

### Platform Options

```csharp
// Available platforms
Platform.WPF
Platform.WindowsForms
Platform.Blazor
Platform.ASPNETCore
Platform.MAUI
Platform.Xamarin
Platform.React  // For JavaScript frameworks
Platform.Angular
Platform.Vue
```

### CI/CD Usage Example

```csharp
using System;
using Syncfusion.Licensing;

public class LicenseValidator
{
    public static int Main(string[] args)
    {
        // Get license key from environment variable
        var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
        
        if (string.IsNullOrEmpty(licenseKey))
        {
            Console.Error.WriteLine("ERROR: SYNCFUSION_LICENSE_KEY environment variable not set");
            return 1;
        }
        
        // Register license
        SyncfusionLicenseProvider.RegisterLicense(licenseKey);
        
        // Validate
        bool isValid = SyncfusionLicenseProvider.ValidateLicense(
            Platform.WPF, 
            out string message
        );
        
        if (isValid)
        {
            Console.WriteLine($"✓ License validation passed: {message}");
            return 0;
        }
        else
        {
            Console.Error.WriteLine($"✗ License validation failed: {message}");
            return 1;
        }
    }
}
```

## Unit Test Validation

Validate licenses using unit tests in your CI/CD pipeline.

### Creating Unit Test Project

#### Visual Studio

1. File → New → Project
2. Search for "Test"
3. Select test framework:
   - **MSTest** (recommended for .NET)
   - **NUnit**
   - **xUnit**
4. Create project

#### .NET CLI

```bash
# MSTest
dotnet new mstest -n YourProject.LicenseTests

# NUnit
dotnet new nunit -n YourProject.LicenseTests

# xUnit
dotnet new xunit -n YourProject.LicenseTests
```

### MSTest Example

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Syncfusion.Licensing;

namespace YourProject.LicenseTests
{
    [TestClass]
    public class SyncfusionLicenseTests
    {
        public TestContext TestContext { get; set; }
        
        [TestMethod]
        public void TestSyncfusionWPFLicense()
        {
            var platform = Platform.WPF;
            
            // Register the Syncfusion license key
            var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            
            // Validate license
            bool isValidLicense = SyncfusionLicenseProvider.ValidateLicense(
                platform, 
                out string validationMessage
            );
            
            // Assert validation passed
            Assert.IsTrue(
                isValidLicense, 
                $"Validation failed for {platform}. Message: {validationMessage}"
            );
            
            // Log success message
            if (isValidLicense)
            {
                TestContext.WriteLine(
                    $"Platform {platform} is correctly licensed for version " +
                    $"{typeof(SyncfusionLicenseProvider).Assembly.GetName().Version}"
                );
            }
        }
        
        [TestMethod]
        public void TestMultiplePlatforms()
        {
            var platforms = new[] { Platform.WPF, Platform.Blazor, Platform.ASPNETCore };
            
            var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            
            foreach (var platform in platforms)
            {
                bool isValid = SyncfusionLicenseProvider.ValidateLicense(
                    platform, 
                    out string message
                );
                
                TestContext.WriteLine($"{platform}: {(isValid ? "✓" : "✗")} - {message}");
            }
        }
    }
}
```

### NUnit Example

```csharp
using NUnit.Framework;
using Syncfusion.Licensing;

namespace YourProject.LicenseTests
{
    [TestFixture]
    public class SyncfusionLicenseTests
    {
        [Test]
        public void TestSyncfusionWPFLicense()
        {
            var platform = Platform.WPF;
            
            // Register license key
            var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
            SyncfusionLicenseProvider.RegisterLicense(licenseKey);
            
            // Validate license
            bool isValidLicense = SyncfusionLicenseProvider.ValidateLicense(
                platform, 
                out string validationMessage
            );
            
            // Assert
            Assert.That(isValidLicense, Is.True, 
                $"Validation failed for {platform}. Message: {validationMessage}");
            
            // Log on success
            if (isValidLicense)
            {
                TestContext.Out.WriteLine(
                    $"Platform {platform} is correctly licensed for version " +
                    $"{typeof(SyncfusionLicenseProvider).Assembly.GetName().Version}"
                );
            }
        }
    }
}
```

### Integration with CI/CD

#### Azure Pipelines

```yaml
- task: DotNetCoreCLI@2
  inputs:
    command: 'test'
    projects: '**/*LicenseTests.csproj'
  env:
    SYNCFUSION_LICENSE_KEY: $(SyncfusionLicenseKey)
  displayName: 'Validate Syncfusion License'
```

#### GitHub Actions

```yaml
- name: Validate Syncfusion License
  run: dotnet test **/YourProject.LicenseTests.csproj
  env:
    SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}
```

#### Jenkins

```groovy
stage('Validate License') {
    steps {
        withCredentials([string(credentialsId: 'syncfusion-license-key', variable: 'LICENSE_KEY')]) {
            bat """
                set SYNCFUSION_LICENSE_KEY=${LICENSE_KEY}
                dotnet test **\\*LicenseTests.csproj
            """
        }
    }
}
```

## Best Practices

### 1. Fail Fast

Configure CI to fail immediately on license validation errors:

```yaml
# Azure Pipelines
continueOnError: false

# GitHub Actions
continue-on-error: false

# Jenkins - throws exception on failure
```

### 2. Secure Key Storage

Never hardcode license keys in scripts:

- ✅ Use CI/CD secrets/variables
- ✅ Use environment variables
- ✅ Use credential management systems
- ❌ Don't commit keys to repositories
- ❌ Don't hardcode in pipeline files

### 3. Validate Early

Place license validation as early as possible in the pipeline:

```yaml
steps:
  1. Checkout code
  2. Validate license  ← Do this early
  3. Restore dependencies
  4. Build
  5. Test
```

### 4. Clear Error Messages

Provide clear failure messages:

```powershell
if ($LASTEXITCODE -ne 0) {
    Write-Host "##[error] Syncfusion license validation failed"
    Write-Host "##[error] Check platform, version, and license key"
    exit 1
}
```

### 5. Version-Specific Validation

Validate against the exact version being built:

```yaml
variables:
  SYNCFUSION_VERSION: '26.2.4'

steps:
  - script: |
      ./validate.ps1 -Version $(SYNCFUSION_VERSION)
```

### 6. Multi-Platform Projects

For projects using multiple Syncfusion platforms:

```csharp
[Test]
public void ValidateAllPlatforms()
{
    var platforms = new[] 
    { 
        Platform.WPF, 
        Platform.Blazor, 
        Platform.React 
    };
    
    foreach (var platform in platforms)
    {
        bool isValid = SyncfusionLicenseProvider.ValidateLicense(
            platform, 
            out string message
        );
        
        Assert.IsTrue(isValid, $"{platform}: {message}");
    }
}
```

### 7. Logging and Reporting

Log validation results for troubleshooting:

```csharp
if (isValid)
{
    Console.WriteLine($"✓ {platform} license valid for v{version}");
}
else
{
    Console.Error.WriteLine($"✗ {platform} validation failed");
    Console.Error.WriteLine($"  Message: {validationMessage}");
    Console.Error.WriteLine($"  Version: {version}");
}
```

## Troubleshooting CI Validation

### Issue: Validation Script Not Found

**Error:** `The system cannot find the path specified`

**Solutions:**
- Verify script path in CI configuration
- Use `$(Build.SourcesDirectory)` or equivalent for relative paths
- Check that LicenseKeyValidator was extracted correctly
- Ensure script is checked into repository or downloaded in CI

### Issue: PowerShell Execution Policy

**Error:** `running scripts is disabled on this system`

**Solution:**
```yaml
- script: |
    powershell -ExecutionPolicy Bypass -File ./LicenseKeyValidation.ps1
```

### Issue: License Key Not Available

**Error:** License validation fails but works locally

**Solutions:**
- Verify environment variable/secret is set in CI
- Check secret name matches code reference
- Ensure secret is available in the pipeline scope
- Test with `echo $env:VARIABLE_NAME` (don't log actual key!)

### Issue: Exit Code Issues

**Problem:** Build doesn't fail on validation error

**Solution:**
```powershell
if ($LASTEXITCODE -ne 0) {
    exit $LASTEXITCODE  # Propagate exit code
}
```

## Summary

CI/CD license validation ensures:
- ✅ License errors caught before deployment
- ✅ Automated validation in every build
- ✅ Consistent licensing across environments
- ✅ Early detection of version/platform mismatches
- ✅ Secure key management via CI secrets

Choose the validation method that best fits your workflow:
- **LicenseKeyValidator:** Standalone utility for any CI system
- **ValidateLicense():** Programmatic validation in custom scripts
- **Unit Tests:** Test-based validation integrated with test pipelines
