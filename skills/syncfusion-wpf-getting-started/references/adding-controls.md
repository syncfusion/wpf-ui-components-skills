# Adding Syncfusion WPF Controls

## Table of Contents
- [Overview](#overview)
- [Using Designer](#using-designer)
- [Using XAML](#using-xaml)
- [Using Code-Behind](#using-code-behind)
- [Using Project Templates](#using-project-templates)
- [Assembly References](#assembly-references)
- [Common Scenarios](#common-scenarios)

## Overview

Syncfusion® WPF controls can be added to Visual Studio projects using four different approaches:

1. **Designer** - Drag and drop from Visual Studio Toolbox
2. **XAML** - Manually add namespace and control declarations
3. **Code-Behind** - Create controls programmatically in C# or VB.NET
4. **Project Templates** - Use Syncfusion project templates for new projects

Each approach has its advantages depending on your workflow and project requirements.

## Using Designer

The Designer approach provides a visual way to add controls through drag-and-drop.

### Prerequisites

- Syncfusion WPF must be installed via full installer (web or offline)
- Controls automatically appear in Visual Studio Toolbox after installation

### Steps

1. **Create or Open WPF Project**
   - Open your WPF project in Visual Studio
   - Open the XAML file where you want to add the control (e.g., `MainWindow.xaml`)

2. **Open Toolbox**
   - Go to **View** → **Toolbox** (or press `Ctrl+Alt+X`)
   - The Toolbox panel appears (typically on the left side)

3. **Find Syncfusion Control**
   - Scroll through the Toolbox to find Syncfusion controls
   - Or use the search box at the top of the Toolbox
   - Type the control name (e.g., "SfDataGrid", "SfChart", "ButtonAdv")

4. **Drag and Drop**
   - Click and drag the control onto the designer surface
   - Or double-click the control to add it to the current container

5. **Automatic Configuration**
   - Visual Studio automatically:
     - Adds required assembly references
     - Adds XML namespace declarations to XAML
     - Creates the control instance with default properties

### Example: Adding SfTextBoxExt via Designer

1. Create a WPF project
2. Search for "SfTextBoxExt" in Toolbox
3. Drag and drop onto the designer

**Result - Generated XAML:**

```xml
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:SfTextBoxExt HorizontalAlignment="Center" 
                                 Height="20" Width="120" 
                                 VerticalAlignment="Center"/>
    </Grid>
</Window>
```

**Required Assemblies (automatically added):**
- `Syncfusion.SfInput.WPF.dll`
- `Syncfusion.SfShared.WPF.dll`

### Advantages

- ✓ Visual, intuitive interface
- ✓ Automatic reference management
- ✓ Immediate visual feedback
- ✓ Properties can be set via Properties window
- ✓ No need to remember namespaces

### Limitations

- ✗ Requires full installer (not available with NuGet-only setup)
- ✗ Less control over exact placement
- ✗ Generates more verbose XAML

## Using XAML

The XAML approach provides manual control over control declaration and configuration.

### Steps

1. **Add Assembly References**
   - Right-click project → **Add** → **Reference**
   - Browse to Syncfusion installation folder or use NuGet
   - Add required assemblies for your control

2. **Add XML Namespace**
   - Open your XAML file
   - Add Syncfusion namespace to the Window or UserControl element

3. **Declare Control**
   - Use the namespace prefix to add the control in XAML

### Example: Adding SfTextBoxExt via XAML

**Step 1: Add Assembly References**

Either:
- Add references from installation folder (see [Assembly References](#assembly-references) section)
- Or install NuGet package:
  ```powershell
  Install-Package Syncfusion.SfInput.WPF
  ```

**Step 2: Add XML Namespace**

```xml
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
```

**Step 3: Declare Control**

```xml
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <syncfusion:SfTextBoxExt HorizontalAlignment="Center" 
                                 Name="textBoxExt1" 
                                 Margin="10" 
                                 Height="20" 
                                 Width="120" 
                                 Text="SfTextBoxExt" 
                                 VerticalAlignment="Center"/>
    </Grid>
</Window>
```

### Common Syncfusion Namespace

Most Syncfusion WPF controls use the standard namespace:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Advantages

- ✓ Works with NuGet-only installations
- ✓ Full control over XAML structure
- ✓ Easy to read and maintain
- ✓ Better for version control
- ✓ Supports MVVM pattern naturally

### Limitations

- ✗ Must manually add references
- ✗ Need to know correct namespace
- ✗ No immediate visual feedback

## Using Code-Behind

The Code-Behind approach creates controls programmatically in C# or VB.NET.

### When to Use Code-Behind

- Dynamic UI generation based on runtime data
- Creating controls in loops or conditional logic
- Building UI from external configuration
- Advanced scenarios requiring programmatic control

### Steps

1. **Add Assembly References**
   - Add required assemblies (same as XAML approach)

2. **Import Namespace**
   - Add `using` directive in C# (or `Imports` in VB.NET)

3. **Create Control Instance**
   - Instantiate the control with `new` keyword
   - Set properties
   - Add to parent container

### Example: Adding SfTextBoxExt via Code-Behind

**Step 1: Add Assembly References**

Add references to:
- `Syncfusion.SfInput.WPF.dll`
- `Syncfusion.SfShared.WPF.dll`

**Step 2: Import Namespace**

**C#:**
```csharp
using Syncfusion.Windows.Controls.Input;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Controls.Input
```

**Step 3: Create and Add Control**

**C#:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Create instance
        SfTextBoxExt textBoxExt1 = new SfTextBoxExt();
        
        // Set properties
        textBoxExt1.Height = 20;
        textBoxExt1.Width = 120;
        textBoxExt1.Margin = new Thickness(10, 10, 10, 10);
        textBoxExt1.VerticalAlignment = VerticalAlignment.Center;
        textBoxExt1.HorizontalAlignment = HorizontalAlignment.Center;
        textBoxExt1.Watermark = "Enter text...";
        
        // Add to parent (assuming Grid named "ROOT_Grid" exists)
        this.Content = textBoxExt1;
        // Or: ROOT_Grid.Children.Add(textBoxExt1);
    }
}
```

**VB.NET:**
```vb
Public Partial Class MainWindow
    Inherits Window
    
    Public Sub New()
        InitializeComponent()
        
        ' Create instance
        Dim textBoxExt1 As New SfTextBoxExt()
        
        ' Set properties
        textBoxExt1.Height = 20
        textBoxExt1.Width = 120
        textBoxExt1.Margin = New Thickness(10, 10, 10, 10)
        textBoxExt1.VerticalAlignment = VerticalAlignment.Center
        textBoxExt1.HorizontalAlignment = HorizontalAlignment.Center
        textBoxExt1.Watermark = "Enter text..."
        
        ' Add to parent
        Me.Content = textBoxExt1
        ' Or: ROOT_Grid.Children.Add(textBoxExt1)
    End Sub
End Class
```

### Advantages

- ✓ Full programmatic control
- ✓ Dynamic UI generation
- ✓ Easy to implement conditional logic
- ✓ Better for complex initialization

### Limitations

- ✗ More verbose than XAML
- ✗ Harder to visualize UI structure
- ✗ Less designer support

## Using Project Templates

Syncfusion provides Visual Studio project templates for quick project setup.

### Prerequisites

- Syncfusion WPF must be installed via full installer
- Templates available from v16.1.0.24 onwards
- Minimum target framework: .NET Framework 8.0

### Steps

1. **Create New Project**
   - Open Visual Studio
   - Click **File** → **New** → **Project**
   - Search for "Syncfusion"
   - Select **Syncfusion WPF Application**

2. **Configure Project**
   - Enter project name
   - Choose destination location
   - Set framework version (minimum 4.0)
   - Click **OK**

3. **Project Configuration Wizard**
   
   The wizard opens with following options:
   
   **Language**
   - Select **C#** or **VB.NET**
   
   **Choose Theme**
   - Select desired theme (MaterialLight, Office2019, etc.)
   
   **Assemblies From**
   - Choose assembly location: GAC, Installed Location, or NuGet
   
   **Select Controls**
   - Check controls you want to include in the project
   - Multiple controls can be selected
   
   Click **Create**

4. **Licensing Configuration**
   
   A licensing message box appears (for trial/NuGet installations):
   - Prompts for license key registration
   - See [License Key Generation](https://help.syncfusion.com/common/essential-studio/licensing/license-key#how-to-generate-syncfusion-license-key)
   
   > **Note**: Licensing was introduced in 2018 Volume 2 (v16.2.0.41)

### Result

The template creates a fully configured WPF project with:
- Selected controls and dependencies
- Theme resources
- Namespace declarations
- Sample XAML
- License key registration (if needed)

### Example Configuration

**Selected Controls**: SfDataGrid, SfChart  
**Theme**: MaterialLight  
**Language**: C#

**Generated App.xaml.cs:**

```csharp
public partial class App : Application
{
    public App()
    {
        // License registration
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    }
}
```

**Generated MainWindow.xaml:**

```xml
<Window x:Class="SyncfusionWpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:SfDataGrid x:Name="dataGrid"/>
    </Grid>
</Window>
```

### Advantages

- ✓ Fastest setup for new projects
- ✓ Automatic configuration
- ✓ Theme pre-configured
- ✓ License registration setup
- ✓ Best practices implemented

### Limitations

- ✗ Only for new projects (not existing ones)
- ✗ Requires full installer
- ✗ Limited customization during creation

## Assembly References

### Reference Locations

**If using full installer:**
- **.NET Framework 4.6.2+**: `C:\Program Files (x86)\Syncfusion\Essential Studio\WPF\{version}\Assemblies\4.6`
- **.NET 8.0+**: `C:\Program Files (x86)\Syncfusion\Essential Studio\WPF\{version}\precompiledassemblies\net8.0`

**If using NuGet:**
- Automatically managed by NuGet Package Manager
- No manual reference needed

### Common Assembly Mappings

| Control | Required Assemblies | NuGet Package |
|---------|-------------------|---------------|
| SfDataGrid | Syncfusion.SfGrid.WPF<br>Syncfusion.Data.WPF<br>Syncfusion.Shared.WPF | Syncfusion.SfGrid.WPF |
| SfChart | Syncfusion.SfChart.WPF | Syncfusion.SfChart.WPF |
| SfTextBoxExt | Syncfusion.SfInput.WPF<br>Syncfusion.SfShared.WPF | Syncfusion.SfInput.WPF |
| ButtonAdv | Syncfusion.Tools.WPF<br>Syncfusion.Shared.WPF | Syncfusion.Tools.WPF |
| Ribbon | Syncfusion.Tools.WPF<br>Syncfusion.Shared.WPF | Syncfusion.Tools.WPF |

For complete assembly dependencies, see [configuration.md](configuration.md).

### Adding References Manually

1. Right-click project → **Add** → **Reference**
2. Click **Browse** button
3. Navigate to Syncfusion installation folder
4. Select required DLL files
5. Click **Add** then **OK**

## Common Scenarios

### Scenario 1: Quick Prototype with Designer

**Best Approach**: Designer

1. Install Syncfusion via web installer
2. Create WPF project
3. Drag controls from Toolbox
4. Configure via Properties window

### Scenario 2: Existing Project, Specific Controls

**Best Approach**: XAML + NuGet

1. Install specific NuGet packages
   ```powershell
   Install-Package Syncfusion.SfGrid.WPF
   ```
2. Add namespace to XAML
3. Declare controls in XAML

### Scenario 3: Dynamic UI Generation

**Best Approach**: Code-Behind

1. Add assembly references
2. Create controls in C#/VB.NET code
3. Add event handlers programmatically
4. Build UI based on runtime data

### Scenario 4: New Enterprise Project

**Best Approach**: Project Templates

1. Use Syncfusion WPF Application template
2. Select required controls
3. Choose corporate theme
4. Configure for team standards

### Scenario 5: CI/CD Environment

**Best Approach**: XAML + NuGet

1. Use NuGet packages (no installer required)
2. Specify exact package versions in project file
3. Register license key in code
4. Build automatically restores packages

## Troubleshooting

### "Could not load file or assembly"

**Problem**: Assembly reference missing or version mismatch.

**Solution**:
- Verify assembly references are added
- Check assembly versions match
- Ensure all dependencies are included
- See [troubleshooting.md](troubleshooting.md)

### Control not in Toolbox

**Problem**: Syncfusion controls don't appear in Toolbox.

**Solution**:
- Verify full installer was used (not NuGet only)
- Reset Toolbox: Right-click Toolbox → Reset
- Check Visual Studio configuration in installer

### Namespace not recognized

**Problem**: XAML shows error on `syncfusion` namespace.

**Solution**:
- Verify assembly references added
- Check namespace URI: `http://schemas.syncfusion.com/wpf`
- Rebuild project

## Next Steps

After adding controls to your project:

1. **Configure themes**: See [configuration.md](configuration.md)
2. **Register license**: Check [upgrading.md](upgrading.md)
3. **Learn control-specific features**: Refer to component-specific skills
4. **Set up data binding**: Implement MVVM patterns
