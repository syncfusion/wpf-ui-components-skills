# CI/CD License Validation

## Overview

Syncfusion license key validation in CI/CD services ensures that components are properly licensed during continuous integration processes. Validating the license key at the CI level prevents licensing errors during deployment.

**⚠️ SECURITY ALERT:**
Avoid downloading and executing external utilities from remote URLs in CI/CD pipelines. This introduces supply chain risks. Use programmatic validation instead.
**NOTE (SECURITY):** Legacy instructions that directed CI systems to download and execute an external `LicenseKeyValidator` binary have been removed from this document. Do NOT fetch or run unvetted third-party binaries in CI pipelines. Use the programmatic `ValidateLicense()` method, unit-test validation, or an internally-hosted, versioned artifact with integrity checks. See `SECURITY_ANALYSIS.md` for details.


**Recommended Validation Methods (Secure):**
1. ✅ **ValidateLicense() Method** - Programmatic validation in code (RECOMMENDED)
2. ✅ **Unit Test Validation** - Test-based license verification (RECOMMENDED)
3. ✅ **Internal Artifact Host** - If external tool needed, host internally
4. ⚠️ **Checksum Verification** - If download required, verify integrity

**Legacy/Not Recommended:**
- ❌ **LicenseKeyValidator Utility (External Download)** - Security risk, use programmatic validation instead

## LicenseKeyValidator Utility (DEPRECATED - Security Risk)

⚠️ **DEPRECATION NOTICE:** The external LicenseKeyValidator utility introduces security risks:
- Downloads executable from external URL
- No integrity verification
- Subject to supply chain attacks
- Requires MITM-safe network conditions

**Recommendation:** Use the programmatic `ValidateLicense()` method instead (see section below). If you must use the external utility, refer to [SECURITY_ANALYSIS.md](../SECURITY_ANALYSIS.md) for secure implementation patterns with checksum verification.

### Why This Approach Is Risky

1. **External Download:** Utility is fetched from external URL at build time
2. **No Verification:** No checksums or signatures to verify authenticity
3. **MITM Vulnerability:** Network traffic could be intercepted
4. **Supply Chain Risk:** Compromised S3 bucket = compromised builds
5. **Execution Privilege:** Runs with full pipeline credentials

**For secure alternatives, see:**
- ✅ [Programmatic Validation (Recommended)](#validatelicense-method)
- ✅ [Unit Test Validation (Recommended)](#unit-test-validation)
- ✅ [Internal Artifact Hosting](#best-practices)
- ⚠️ [Checksum Verification (Fallback)](#best-practices)

## Azure Pipelines Integration (Secure - Programmatic Validation)

Configure license validation in your Azure Pipelines YAML file using programmatic validation:

### Option 1: Validation During Build (RECOMMENDED)

```yaml
trigger:
  branches:
    include:
      - main
      - develop

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'

steps:
  # Checkout code
  - checkout: self
  
  # Setup .NET
  - task: UseDotNet@2
    inputs:
      version: '8.0.x'
    displayName: 'Setup .NET'
  
  # Restore dependencies
  - task: DotNetCoreCLI@2
    inputs:
      command: 'restore'
      projects: '**/*.csproj'
    displayName: 'Restore NuGet Packages'
  
  # Build project (license registration happens in Program.cs)
  - task: DotNetCoreCLI@2
    inputs:
      command: 'build'
      projects: '**/*.csproj'
      arguments: '--configuration $(buildConfiguration) --no-restore'
    env:
      SYNCFUSION_LICENSE_KEY: $(SyncfusionLicenseKey)  # From pipeline secret
    displayName: 'Build Project with License Registration'
    continueOnError: false  # Fail if license registration fails
```

### Option 2: Validation with Unit Tests (RECOMMENDED)

```yaml
trigger:
  branches:
    include:
      - main
      - develop

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'

steps:
  - checkout: self
  
  - task: UseDotNet@2
    inputs:
      version: '8.0.x'
    displayName: 'Setup .NET'
  
  # License validation test
  - task: DotNetCoreCLI@2
    inputs:
      command: 'test'
      projects: '**/*LicenseValidation.Tests.csproj'
      arguments: '--configuration $(buildConfiguration)'
    env:
      SYNCFUSION_LICENSE_KEY: $(SyncfusionLicenseKey)
    displayName: 'Validate Syncfusion License'
    continueOnError: false  # Fail build if validation fails
  
  # Build project
  - task: DotNetCoreCLI@2
    inputs:
      command: 'build'
      projects: '**/*.csproj'
      arguments: '--configuration $(buildConfiguration)'
    env:
      SYNCFUSION_LICENSE_KEY: $(SyncfusionLicenseKey)
    displayName: 'Build Project'
  
  # Run all tests
  - task: DotNetCoreCLI@2
    inputs:
      command: 'test'
      projects: '**/*Tests.csproj'
      arguments: '--configuration $(buildConfiguration)'
    env:
      SYNCFUSION_LICENSE_KEY: $(SyncfusionLicenseKey)
    displayName: 'Run All Tests'
```

### Setup Pipeline Secret

1. Go to your pipeline in Azure DevOps
2. Click **Edit** → **Variables**
3. Click **New variable**
   - **Name:** `SyncfusionLicenseKey`
   - **Value:** Your license key
   - **Keep this value secret:** ✅ (checked)
4. Click **OK** and **Save**

### Complete Example with All Stages

```yaml
trigger:
  branches:
    include:
      - main
      - develop

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'
  dotnetVersion: '8.0.x'

stages:
  - stage: Build
    displayName: 'Build and Validate'
    jobs:
      - job: ValidateAndBuild
        displayName: 'Validate License and Build'
        steps:
          - checkout: self
          
          - task: UseDotNet@2
            inputs:
              version: $(dotnetVersion)
            displayName: 'Setup .NET'
          
          # Validate license first (fail fast)
          - task: DotNetCoreCLI@2
            inputs:
              command: 'test'
              projects: '**/LicenseValidation.Tests.csproj'
            env:
              SYNCFUSION_LICENSE_KEY: $(SyncfusionLicenseKey)
            displayName: 'Validate License'
            continueOnError: false
          
          - task: DotNetCoreCLI@2
            inputs:
              command: 'restore'
              projects: '**/*.csproj'
            displayName: 'Restore'
          
          - task: DotNetCoreCLI@2
            inputs:
              command: 'build'
              projects: '**/*.csproj'
              arguments: '--configuration $(buildConfiguration) --no-restore'
            env:
              SYNCFUSION_LICENSE_KEY: $(SyncfusionLicenseKey)
            displayName: 'Build'
          
          - task: DotNetCoreCLI@2
            inputs:
              command: 'test'
              projects: '**/*Tests.csproj'
              arguments: '--configuration $(buildConfiguration)'
            env:
              SYNCFUSION_LICENSE_KEY: $(SyncfusionLicenseKey)
            displayName: 'Test'

  - stage: Deploy
    displayName: 'Deploy'
    dependsOn: Build
    condition: succeeded()
    jobs:
      - job: DeployJob
        displayName: 'Deploy Application'
        steps:
          - checkout: self
          - script: echo Deploying application...
            displayName: 'Deploy'
```

**Key Points:**
- ✅ No external downloads
- ✅ No executable execution
- ✅ License key from secure pipeline variable
- ✅ Validation happens at build time
- ✅ Build fails if validation fails

## GitHub Actions Integration (Secure - Programmatic Validation)

Integrate license validation into GitHub Actions workflows using programmatic validation:

### Basic Workflow

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
      
      - name: Validate Syncfusion License
        run: dotnet test **/LicenseValidation.Tests.csproj
        env:
          SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}
      
      - name: Restore dependencies
        run: dotnet restore
      
      - name: Build
        run: dotnet build --configuration Release --no-restore
        env:
          SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}
      
      - name: Run tests
        run: dotnet test --no-build --verbosity normal
        env:
          SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}
```

### Setup GitHub Secret

#### Step 1: Add Secret

1. Go to repository **Settings**
2. Click **Secrets and variables** → **Actions**
3. Click **New repository secret**
   - Name: `SYNCFUSION_LICENSE_KEY`
   - Value: Your actual license key
4. Click **Add secret**

#### Step 2: Use Secret in Workflow

The secret is automatically available as `${{ secrets.SYNCFUSION_LICENSE_KEY }}`

### Advanced Workflow with Multiple Jobs

```yaml
name: Build and Validate

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    name: Validate License
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x'
      
      - name: Validate License
        run: dotnet test **/LicenseValidation.Tests.csproj
        env:
          SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}

  build:
    name: Build and Test
    needs: validate  # Only run after license validation passes
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x'
      
      - name: Restore
        run: dotnet restore
      
      - name: Build
        run: dotnet build --configuration Release --no-restore
        env:
          SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}
      
      - name: Test
        run: dotnet test --no-build --verbosity normal
        env:
          SYNCFUSION_LICENSE_KEY: ${{ secrets.SYNCFUSION_LICENSE_KEY }}

  deploy:
    name: Deploy
    needs: build  # Only run after build succeeds
    runs-on: windows-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - name: Deploy Application
        run: echo "Deploying..."
```

### Security Best Practices

1. ✅ Secret only available to repository owner and maintainers
2. ✅ Secret automatically masked in logs
3. ✅ Secret not available to pull requests from forks (default)
4. ✅ Use `SYNCFUSION_LICENSE_KEY` environment variable in code
5. ❌ Don't hardcode secret in workflow file
6. ❌ Don't print actual secret value in logs

## Jenkins Integration (Secure - Programmatic Validation)

Integrate license validation into Jenkins pipelines using programmatic validation:

### Declarative Pipeline

```groovy
pipeline {
    agent any
    
    environment {
        SYNCFUSION_LICENSE_KEY = credentials('syncfusion-license-key')
        DOTNET_CLI_HOME = '/tmp/dotnet'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Validate License') {
            steps {
                bat 'dotnet test **\\*LicenseValidation.Tests.csproj'
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
        success {
            echo 'Build succeeded - Syncfusion license validated'
        }
    }
}
```

### Setup Jenkins Credential

#### Step 1: Add Credential

1. Go to **Jenkins** → **Manage Jenkins** → **Manage Credentials**
2. Click **Add Credentials**
3. Configure:
   - **Kind:** Secret text
   - **ID:** `syncfusion-license-key`
   - **Secret:** Your license key
4. Click **Create**

#### Step 2: Use in Pipeline

The credential is automatically available as `credentials('syncfusion-license-key')`

### Complete Example with Stages

```groovy
pipeline {
    agent any
    
    environment {
        SYNCFUSION_LICENSE_KEY = credentials('syncfusion-license-key')
        BUILD_CONFIGURATION = 'Release'
        DOTNET_VERSION = '8.0'
    }
    
    options {
        timeout(time: 1, unit: 'HOURS')
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }
    
    stages {
        stage('Prepare') {
            steps {
                script {
                    echo "Build started on ${env.BUILD_ID}"
                    echo ".NET version: ${DOTNET_VERSION}"
                }
            }
        }
        
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Validate License') {
            steps {
                script {
                    try {
                        bat 'dotnet test **\\*LicenseValidation.Tests.csproj --verbosity normal'
                        echo "✓ License validation passed"
                    } catch (Exception e) {
                        echo "✗ License validation failed"
                        throw e
                    }
                }
            }
        }
        
        stage('Restore Dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                bat "dotnet build --configuration ${BUILD_CONFIGURATION} --no-restore"
            }
        }
        
        stage('Test') {
            steps {
                bat "dotnet test --configuration ${BUILD_CONFIGURATION} --no-build --verbosity normal"
            }
        }
        
        stage('Package') {
            steps {
                bat "dotnet publish --configuration ${BUILD_CONFIGURATION} --output build/output"
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Build successful with valid license'
        }
        failure {
            echo 'Build failed - check logs above'
        }
    }
}
```

### Security Best Practices for Jenkins

1. ✅ Store license key as Jenkins credential (not in source)
2. ✅ Use `credentials('id')` to inject into pipeline
3. ✅ Credential automatically masked in console logs
4. ✅ Restrict credential access to specific jobs/pipelines
5. ✅ Use environment variables to pass credential to build
6. ❌ Don't hardcode secrets in pipeline script
7. ❌ Don't echo actual credential values

### Restrict Credential Access (Security)

```groovy
pipeline {
    agent any
    
    options {
        credentials {
            string(credentialsId: 'syncfusion-license-key', variable: 'LICENSE_KEY')
        }
    }
    
    stages {
        stage('Validate') {
            steps {
                // Credential automatically available as ${LICENSE_KEY}
                // and masked in console output
                withCredentials([
                    string(credentialsId: 'syncfusion-license-key', variable: 'SYNC_KEY')
                ]) {
                    bat 'dotnet test'
                }
            }
        }
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

### 1. Use Programmatic Validation (RECOMMENDED)

The `ValidateLicense()` method is the most secure approach:

✅ **Advantages:**
- No external downloads
- No executable execution
- Source code visible in Syncfusion assemblies
- Part of verified NuGet ecosystem
- Simple integration

```csharp
bool isValid = SyncfusionLicenseProvider.ValidateLicense(Platform.WPF);
if (!isValid) throw new InvalidOperationException("License validation failed");
```

### 2. Validate Early in Pipeline

Place license validation as early as possible to fail fast:

```yaml
steps:
  1. Checkout code
  2. Setup dependencies
  3. Validate license  ← Do this early
  4. Build
  5. Test
```

**Example:**
```csharp
// Program.cs - Validate immediately on startup
var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
SyncfusionLicenseProvider.RegisterLicense(licenseKey);

bool isValid = SyncfusionLicenseProvider.ValidateLicense(
    Platform.WPF, 
    out string message
);

if (!isValid)
{
    throw new InvalidOperationException($"License validation failed: {message}");
}
```

### 3. Fail Build on Validation Error

Configure CI to stop the build if validation fails:

```yaml
# Azure Pipelines
- task: DotNetCoreCLI@2
  inputs:
    command: 'test'
    projects: '**/LicenseValidation.Tests.csproj'
  continueOnError: false  # Fail build

# GitHub Actions
continue-on-error: false

# Jenkins - throws exception
withCredentials([...]) {
    bat 'dotnet test ...'  # Build stops on test failure
}
```

### 4. Secure License Key Storage

Never hardcode keys or expose them in logs:

- ✅ Use CI/CD secrets/variables (Azure DevOps, GitHub, Jenkins)
- ✅ Use environment variables
- ✅ Use secure credential stores (Azure Key Vault, AWS Secrets Manager)
- ✅ Secrets automatically masked in console logs
- ❌ Don't commit keys to repositories
- ❌ Don't hardcode in pipeline files
- ❌ Don't echo/print actual key values

**Setup:**
```bash
# Azure Pipelines - Create pipeline variable marked as secret
# GitHub Actions - Create repository secret
# Jenkins - Create secret text credential
```

### 5. Version-Specific Validation

Validate against the exact Syncfusion version being used:

```csharp
// Get version from environment or configuration
var syncfusionVersion = "26.2.4";

// Include in validation messages
bool isValid = SyncfusionLicenseProvider.ValidateLicense(
    Platform.WPF, 
    out string message
);

Console.WriteLine($"Syncfusion v{syncfusionVersion}: {(isValid ? "✓" : "✗")} {message}");
```

### 6. Multi-Platform Project Validation

For projects using multiple Syncfusion platforms:

```csharp
[TestMethod]
public void ValidateAllPlatforms()
{
    var platforms = new[] 
    { 
        (Platform.WPF, "WPF"),
        (Platform.Blazor, "Blazor"),
        (Platform.ASPNETCore, "ASP.NET Core")
    };
    
    var licenseKey = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
    SyncfusionLicenseProvider.RegisterLicense(licenseKey);
    
    foreach (var (platform, name) in platforms)
    {
        bool isValid = SyncfusionLicenseProvider.ValidateLicense(
            platform, 
            out string message
        );
        
        Assert.IsTrue(isValid, $"{name}: {message}");
        TestContext.WriteLine($"✓ {name} license valid: {message}");
    }
}
```

### 7. Logging and Reporting

Log validation results clearly for troubleshooting:

```csharp
bool isValid = SyncfusionLicenseProvider.ValidateLicense(
    Platform.WPF, 
    out string validationMessage
);

if (isValid)
{
    Console.WriteLine($"✓ Syncfusion license valid for v{version}");
    return 0;
}
else
{
    Console.Error.WriteLine("✗ Syncfusion license validation failed");
    Console.Error.WriteLine($"  Platform: WPF");
    Console.Error.WriteLine($"  Message: {validationMessage}");
    return 1;
}
```

### 8. Avoid External Downloads

❌ **DEPRECATED:** Don't download and execute external tools:
- No supply chain verification
- No integrity checking
- MITM attack risk
- Execution privileges risk

✅ **RECOMMENDED:** Use built-in validation methods:
- Programmatic `ValidateLicense()` method
- Unit test validation
- Integrated .NET/NuGet ecosystem

### 9. Document License Requirements

Add documentation about license setup in your CI/CD:

```yaml
# In your repository README or CI documentation:
# 
# License Setup for CI/CD:
# 1. Create pipeline secret "SyncfusionLicenseKey"
# 2. Set to your Syncfusion license key
# 3. License is validated automatically during build
# 4. Build fails if license is invalid or missing
```

### 10. Regular Key Rotation

Rotate license keys periodically:

- ✅ Update keys in CI/CD secrets
- ✅ Revoke old keys in Syncfusion account
- ✅ Update all deployments
- ✅ Monitor key access logs

## Troubleshooting CI Validation

### Issue: License Key Not Available in CI

**Error:** License validation fails but works locally

**Solutions:**

1. **Verify Secret/Variable is Set:**
   ```yaml
   # Azure Pipelines - check secret exists
   - script: echo "Secret is set" # Don't print actual value!
   ```

2. **Check Secret Name Matches Code:**
   ```csharp
   // Code expects SYNCFUSION_LICENSE_KEY
   var key = Environment.GetEnvironmentVariable("SYNCFUSION_LICENSE_KEY");
   
   // Azure DevOps secret must be: SyncfusionLicenseKey
   // (Reference as $(SyncfusionLicenseKey) or env variable)
   ```

3. **Ensure Credential Scope is Correct:**
   - In Jenkins: Verify credential is accessible to pipeline job
   - In GitHub: Verify secret is available to current repository
   - In Azure: Verify variable is in scope for current stage

### Issue: License Validation Fails During Build

**Error:** `License validation failed: Invalid key`

**Solutions:**

1. **Check Key Format:**
   - Verify key contains no extra spaces or line breaks
   - Ensure key hasn't been truncated
   - Verify key matches Syncfusion version

2. **Verify Version Match:**
   ```csharp
   // License key must match this version
   var syncfusionPackageVersion = "26.2.4";
   
   // Generate new key if version changed
   ```

3. **Check Platform Match:**
   ```csharp
   // Platform must match license
   var platform = Platform.WPF;  // Must match key platform
   ```

### Issue: Platform-Specific Build Failure

**Error:** `License validation failed for platform X`

**Solution:** Generate separate license keys for each platform:

```csharp
[TestMethod]
public void MultiPlatformValidation()
{
    // Each platform may need separate validation
    bool wpfValid = SyncfusionLicenseProvider.ValidateLicense(Platform.WPF);
    bool blazorValid = SyncfusionLicenseProvider.ValidateLicense(Platform.Blazor);
    
    Assert.IsTrue(wpfValid && blazorValid, "Platform validation failed");
}
```

### Issue: Build Passes Locally But Fails in CI

**Error:** Validation works locally but fails in CI pipeline

**Causes and Solutions:**

1. **Different .NET Version:**
   ```yaml
   # Ensure CI uses same .NET version as local
   - uses: actions/setup-dotnet@v3
     with:
       dotnet-version: '8.0.x'  # Match your local version
   ```

2. **Different NuGet Version:**
   ```bash
   # Update NuGet in CI to match local
   dotnet nuget locals all --clear
   dotnet restore  # Force fresh restore
   ```

3. **Key Environment Differences:**
   - CI system clock skew (clock must be within range of key validity)
   - Locale/culture differences
   - Architecture differences (x86 vs x64)

### Issue: Cannot Find LicenseValidation Assembly

**Error:** `Could not load file or assembly 'Syncfusion.Licensing'`

**Solutions:**

1. **Add NuGet Package:**
   ```bash
   dotnet add package Syncfusion.Licensing
   ```

2. **Verify Reference:**
   ```csharp
   using Syncfusion.Licensing;  // Required import
   ```

3. **Check .csproj:**
   ```xml
   <PackageReference Include="Syncfusion.Licensing" Version="26.2.4" />
   ```

## Summary

CI/CD license validation ensures:
- ✅ License errors caught before deployment
- ✅ Automated validation in every build
- ✅ Consistent licensing across environments
- ✅ Early detection of version/platform mismatches
- ✅ Secure key management via CI secrets

**Recommended Validation Methods:**
1. ✅ **Programmatic Validation** (`ValidateLicense()`) - No external tools
2. ✅ **Unit Test Validation** - Integrated with test pipeline
3. ✅ **Build-time Validation** - During project build in Program.cs

**NOT Recommended:**
- ❌ External tool downloads - Security and supply chain risk
- ❌ External PowerShell execution - Unverifiable code execution

See [SECURITY_ANALYSIS.md](../SECURITY_ANALYSIS.md) for more security details.
