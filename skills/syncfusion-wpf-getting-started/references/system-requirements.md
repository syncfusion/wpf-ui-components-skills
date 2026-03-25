# System Requirements

This reference covers the system requirements needed to develop and run applications using Syncfusion® Essential® WPF components.

## Operating Systems

Syncfusion WPF controls support the following Windows operating systems:

- **Windows XP**
- **Windows Vista SP2**
- **Windows 7 SP1**
- **Windows 8, 8.1**
- **Windows 10**
- **Windows 11**

All versions support both 32-bit (x86) and 64-bit (x64) architectures.

## Hardware Requirements

### Minimum Requirements

- **Processor**: x86 or x64 architecture
- **RAM**: 512 MB minimum
- **Hard Disk Space**: Up to 3 GB of free space may be required for full installation

### Recommended Requirements

- **RAM**: 1 GB or more (recommended for better performance)
- **Hard Disk Space**: 5 GB or more for complete installation with samples and documentation
- **Processor**: Multi-core processor for better compilation and runtime performance

## Development Environment

### Visual Studio Versions

Syncfusion WPF controls support the following Visual Studio versions:

- **Visual Studio 2015**
- **Visual Studio 2017**
- **Visual Studio 2019**
- **Visual Studio 2022**

All editions are supported (Community, Professional, Enterprise).

### .NET Framework Versions

Syncfusion WPF controls are compatible with multiple .NET Framework versions. The compatibility has evolved over time:

#### Current Support (.NET Framework)

From version **28.1 (2024 Vol4)** onwards:
- **.NET Framework 4.6.2** and above

From version **25.1 (2024 Vol1)** to **27.4**:
- **.NET Framework 4.0**
- **.NET Framework 4.6.2** and above

Earlier versions supported:
- **.NET Framework 3.5**
- **.NET Framework 4.0**
- **.NET Framework 4.5**
- **.NET Framework 4.6**

> **Note**: Syncfusion has dropped support for .NET Framework 3.5 from version 21.1 (2023 Vol1). From version 28.1 (2024 Vol4), only .NET Framework 4.6.2 and above are supported.

### .NET Core and .NET Support

Syncfusion WPF controls support modern .NET versions (.NET Core 3.1, .NET 5, .NET 6, .NET 7, .NET 8, .NET 9, and .NET 10):

#### Version Compatibility Table

| Syncfusion Version | .NET 6.0 | .NET 7.0 | .NET 8.0 | .NET 9.0 | .NET 10.0 |
|-------------------|----------|----------|----------|----------|-----------|
| Earlier versions | No | No | No | No | No |
| From 20.4 (2022 Vol4) | Yes | No | No | No | No |
| From 21.1 (2023 Vol1) | Yes | Yes | No | No | No |
| From 23.2 (2023 Vol3 SP) | Yes | Yes | Yes | No | No |
| From 27.2 (2024 Vol3 SP) | Yes | Yes | Yes | Yes | No |
| From 29.1 (2025 Vol1) | No | No | Yes | Yes | No |
| From 31.2 (2025 Vol3 SP) | No | No | Yes | Yes | Yes |

> **Important**: All Syncfusion WPF controls support . Core and .NET except controls labeled as "classic" (legacy controls).

## Installation Formats

Syncfusion provides multiple installation options:

### Full Installers

- **Web Installer** - Downloads components during installation (requires internet connection)
- **Offline Installer** - Complete package for offline installation (EXE and ZIP formats available)

### NuGet Packages

- Available from [nuget.org](https://www.nuget.org/packages?q=Tags%3A%22Wpf%22+syncfusion)
- Can be used without installing full Essential Studio setup
- Lightweight, control-by-control installation

## Additional Software

### Optional Components

Depending on your development needs, you may require:

- **SQL Server** - For data-bound controls with SQL data sources
- **.NET SDK** - Required for .NET Core/.NET development
- **IIS** - For web-based reporting or dashboard scenarios

## Verifying Your System

### Checking Visual Studio Version

1. Open Visual Studio
2. Go to **Help** → **About Microsoft Visual Studio**
3. Version number will be displayed

### Checking .NET Framework Version

**Option 1: Through Registry Editor**
1. Press `Win + R`, type `regedit`
2. Navigate to: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full`
3. Check the `Release` DWORD value

**Option 2: Through PowerShell**
```powershell
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse |
Get-ItemProperty -Name version -EA 0 |
Where { $_.PSChildName -Match '^(?!S)\p{L}'} |
Select PSChildName, version
```

### Checking Windows Version

1. Press `Win + R`, type `winver`
2. Click OK to see Windows version details

## Compatibility Notes

### Windows XP and Vista

- Limited support for latest Syncfusion versions
- Consider upgrading to Windows 7 SP1 or later for optimal experience
- Some modern features may not be available

### 32-bit vs 64-bit

- Syncfusion assemblies support both x86 and x64 architectures
- AnyCPU projects will work on both architectures
- For optimal performance on 64-bit systems, target x64 specifically

### Visual Studio 2015

- Still supported but consider upgrading to Visual Studio 2019 or 2022
- Some newer project templates may not be available in VS2015

## Recommendations

### For New Projects

- **OS**: Windows 10 or Windows 11
- **Visual Studio**: Visual Studio 2022
- **Framework**: .NET 8.0 or .NET 9.0 (or .NET Framework 4.6.2+)
- **RAM**: 8 GB or more
- **Storage**: SSD with at least 10 GB free space

### For Enterprise Development

- **Visual Studio Enterprise** with additional productivity features
- **Multi-core processor** (4+ cores) for faster builds
- **16 GB+ RAM** for working with large solutions
- **Version control integration** (Git, Azure DevOps)

## Troubleshooting System Requirement Issues

### "Target framework not available"

**Problem**: Project templates show unsupported framework versions.

**Solution**: Install the appropriate .NET SDK or .NET Framework Developer Pack from Microsoft.

### "Visual Studio version not supported"

**Problem**: Installer warns about Visual Studio version.

**Solution**: Update Visual Studio to the latest service release or install a supported version.

### Performance Issues

**Problem**: Slow compilation or designer performance.

**Solution**: 
- Increase RAM to 8 GB or more
- Use SSD instead of HDD
- Disable unnecessary Visual Studio extensions
- Close other applications during development

## Next Steps

After verifying your system meets the requirements:

1. **Choose installation method**: Proceed to [installation-methods.md](installation-methods.md)
2. **Set up NuGet packages**: See [nuget-package-management.md](nuget-package-management.md)
3. **Add controls to project**: Refer to [adding-controls.md](adding-controls.md)

## Additional Resources

- [Syncfusion WPF System Requirements (Official)](https://help.syncfusion.com/wpf/installation-and-upgrade/system-requirements)
- [Visual Studio System Requirements](https://docs.microsoft.com/en-us/visualstudio/releases/2022/system-requirements)
- [.NET Framework System Requirements](https://docs.microsoft.com/en-us/dotnet/framework/get-started/system-requirements)
