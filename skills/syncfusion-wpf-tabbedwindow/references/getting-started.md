# Getting Started with WPF Tabbed Window

## Table of Contents
- [Overview](#overview)
- [Installation and Setup](#installation-and-setup)
- [Assembly References](#assembly-references)
- [Namespace Imports](#namespace-imports)
- [Creating Your First Tabbed Window](#creating-your-first-tabbed-window)
- [Window Type Modes](#window-type-modes)
- [Basic Tab Creation](#basic-tab-creation)
- [Key Properties Overview](#key-properties-overview)
- [Common Setup Patterns](#common-setup-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion WPF Tabbed Window combines `SfChromelessWindow` with `SfTabControl` to create professional document-based applications. This guide walks through installation, basic setup, and your first implementation.

**What You'll Learn:**
- How to install and reference required assemblies
- Creating tabbed windows in XAML and code-behind
- Understanding Window Type Modes (Tabbed vs Normal)
- Basic tab creation and configuration
- Essential properties for tab management

## Installation and Setup

### Option 1: NuGet Package Manager

Install via Package Manager Console:

```powershell
Install-Package Syncfusion.SfChromelessWindow.WPF
Install-Package Syncfusion.Shared.WPF
```

Or use .NET CLI:

```bash
dotnet add package Syncfusion.SfChromelessWindow.WPF
dotnet add package Syncfusion.Shared.WPF
```

### Option 2: Visual Studio Package Manager UI

1. Right-click your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for `Syncfusion.SfChromelessWindow.WPF`
4. Click **Install**
5. Repeat for `Syncfusion.Shared.WPF`

### Option 3: Syncfusion Control Panel

For complete suite installation:
1. Download and install Syncfusion Essential Studio for WPF
2. Use Syncfusion Control Panel to manage installations
3. Add assemblies through **Add Reference** in Visual Studio

**Recommended:** Use NuGet for automatic dependency management and easy updates.

## Assembly References

Add these assemblies to your WPF project:

**Required Assemblies:**
- `Syncfusion.SfChromelessWindow.WPF` - Chromeless window functionality
- `Syncfusion.Shared.WPF` - Shared utilities and controls

**Location (if manually referencing):**
```
C:\Program Files (x86)\Syncfusion\Essential Studio\WPF\{version}\Assemblies\
```

**In Visual Studio:**
1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to Syncfusion assembly location
4. Select both required DLLs

## Namespace Imports

### XAML Namespace

Add this to your Window or UserControl:

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        ... >
```

**Full example:**

```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Tabbed Window" Height="600" Width="900">
    <!-- Content here -->
</Window>
```

### C# Namespace

Import in code-behind or ViewModels:

```csharp
using Syncfusion.Windows.Controls;
using Syncfusion.Windows.Shared;
using System.Windows.Controls;
```

## Creating Your First Tabbed Window

### Method 1: XAML-Based Approach (Recommended)

**Step 1: Create Window XAML**

```xml
<syncfusion:SfChromelessWindow 
    x:Class="TabbedWindowApp.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    Title="My Tabbed Application"
    WindowType="Tabbed"
    Height="450" 
    Width="800">
    
    <syncfusion:SfTabControl x:Name="MainTabControl">
        <syncfusion:SfTabItem Header="Home">
            <TextBlock Text="Welcome to Home Tab" 
                      Margin="20" 
                      FontSize="16"/>
        </syncfusion:SfTabItem>
        
        <syncfusion:SfTabItem Header="Documents">
            <TextBlock Text="Welcome to Documents Tab" 
                      Margin="20" 
                      FontSize="16"/>
        </syncfusion:SfTabItem>
        
        <syncfusion:SfTabItem Header="Settings">
            <TextBlock Text="Welcome to Settings Tab" 
                      Margin="20" 
                      FontSize="16"/>
        </syncfusion:SfTabItem>
    </syncfusion:SfTabControl>
    
</syncfusion:SfChromelessWindow>
```

**Step 2: Create Code-Behind**

```csharp
using System.Windows;
using Syncfusion.Windows.Controls;

namespace TabbedWindowApp
{
    public partial class MainWindow : SfChromelessWindow
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

**Key Points:**
- Window inherits from `SfChromelessWindow`, not `Window`
- `WindowType="Tabbed"` integrates tabs into window chrome
- `SfTabControl` contains `SfTabItem` elements

### Method 2: Code-Behind Approach

Create everything programmatically:

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Controls;

namespace TabbedWindowApp
{
    public class MainWindow : SfChromelessWindow
    {
        public MainWindow()
        {
            // Configure window
            this.Title = "My Tabbed Application";
            this.WindowType = WindowType.Tabbed;
            this.Height = 450;
            this.Width = 800;

            // Create tab control
            var tabControl = new SfTabControl();

            // Create tabs
            var homeTab = new SfTabItem
            {
                Header = "Home",
                Content = new TextBlock 
                { 
                    Text = "Welcome to Home Tab",
                    Margin = new Thickness(20),
                    FontSize = 16
                }
            };

            var docsTab = new SfTabItem
            {
                Header = "Documents",
                Content = new TextBlock 
                { 
                    Text = "Welcome to Documents Tab",
                    Margin = new Thickness(20),
                    FontSize = 16
                }
            };

            // Add tabs to control
            tabControl.Items.Add(homeTab);
            tabControl.Items.Add(docsTab);

            // Set as window content
            this.Content = tabControl;
        }
    }
}
```

## Window Type Modes

The `WindowType` property controls how tabs integrate with the window. Choose based on your application's needs.

### Tabbed Mode

Tabs are integrated into the window chrome (title bar area), similar to modern web browsers.

**Configuration:**

```xml
<syncfusion:SfChromelessWindow WindowType="Tabbed">
    <syncfusion:SfTabControl>
        <!-- Tabs here -->
    </syncfusion:SfTabControl>
</syncfusion:SfChromelessWindow>
```

**When to Use Tabbed Mode:**
- Building browser-style applications
- Document editors (like Visual Studio, VS Code)
- Tabs are the primary navigation method
- Want compact, integrated appearance
- Creating code editors or development tools

**Characteristics:**
- Tabs appear in the window chrome area
- Window title and tabs share the top area
- Compact, professional layout
- Browser-like user experience
- Maximizes content area

**Example:**

```xml
<syncfusion:SfChromelessWindow 
    x:Class="MyApp.MainWindow"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    Title="Code Editor"
    WindowType="Tabbed"
    Height="600" 
    Width="900">
    
    <syncfusion:SfTabControl AllowDragDrop="True" EnableNewTabButton="True">
        <syncfusion:SfTabItem Header="main.cs" CloseButtonVisibility="Visible">
            <TextBox AcceptsReturn="True" FontFamily="Consolas"/>
        </syncfusion:SfTabItem>
        <syncfusion:SfTabItem Header="utils.cs" CloseButtonVisibility="Visible">
            <TextBox AcceptsReturn="True" FontFamily="Consolas"/>
        </syncfusion:SfTabItem>
    </syncfusion:SfTabControl>
    
</syncfusion:SfChromelessWindow>
```

### Normal Mode

Tab control is displayed as regular content within the window. Tabs occupy the content area, not the chrome.

**Configuration:**

```xml
<syncfusion:SfChromelessWindow WindowType="Normal">
    <Grid>
        <!-- Additional UI elements above tabs -->
        <syncfusion:SfTabControl>
            <!-- Tabs here -->
        </syncfusion:SfTabControl>
    </Grid>
</syncfusion:SfChromelessWindow>
```

**When to Use Normal Mode:**
- Tabs are secondary navigation
- Need additional UI elements above tabs (toolbar, menu bar)
- Creating traditional MDI applications
- Want flexible layout control
- Tabs are one part of a larger UI

**Characteristics:**
- Tabs displayed as regular content
- Window title separate from tabs
- More layout flexibility
- Traditional application appearance
- Can combine with other UI elements

**Example:**

```xml
<syncfusion:SfChromelessWindow 
    x:Class="MyApp.MainWindow"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    Title="View Manager"
    WindowType="Normal"
    Height="600" 
    Width="900">
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <!-- Toolbar above tabs -->
        <ToolBar Grid.Row="0">
            <Button Content="New"/>
            <Button Content="Open"/>
            <Button Content="Save"/>
        </ToolBar>
        
        <!-- Tabs in content area -->
        <syncfusion:SfTabControl Grid.Row="1" AllowDragDrop="True">
            <syncfusion:SfTabItem Header="View 1" Content="Content 1"/>
            <syncfusion:SfTabItem Header="View 2" Content="Content 2"/>
        </syncfusion:SfTabControl>
    </Grid>
    
</syncfusion:SfChromelessWindow>
```

### Comparison Table

| Feature | Tabbed Mode | Normal Mode |
|---------|-------------|-------------|
| Tab Location | Window chrome | Content area |
| Layout | Compact | Flexible |
| Best For | Document-centric apps | Complex UIs |
| Chrome Integration | Yes | No |
| Additional UI Above Tabs | Limited | Full flexibility |
| User Experience | Browser-like | Traditional |

## Basic Tab Creation

### Static Tabs in XAML

Define tabs directly in markup:

```xml
<syncfusion:SfTabControl>
    <syncfusion:SfTabItem Header="Tab 1" Content="Simple text content"/>
    
    <syncfusion:SfTabItem Header="Tab 2">
        <StackPanel Margin="20">
            <TextBlock Text="Complex content" FontWeight="Bold"/>
            <Button Content="Click Me" Margin="0,10,0,0"/>
        </StackPanel>
    </syncfusion:SfTabItem>
    
    <syncfusion:SfTabItem Header="Tab 3">
        <Grid>
            <!-- Any XAML content -->
        </Grid>
    </syncfusion:SfTabItem>
</syncfusion:SfTabControl>
```

### Dynamic Tabs in Code

Add tabs programmatically:

```csharp
private void AddNewTab()
{
    var newTab = new SfTabItem
    {
        Header = $"Document {MainTabControl.Items.Count + 1}",
        Content = new TextBlock
        {
            Text = "New document content",
            Margin = new Thickness(20)
        }
    };
    
    MainTabControl.Items.Add(newTab);
    MainTabControl.SelectedItem = newTab; // Make it active
}
```

### Tabs with Icons

Add icons to tab headers:

```xml
<syncfusion:SfTabItem>
    <syncfusion:SfTabItem.Header>
        <StackPanel Orientation="Horizontal">
            <Image Source="/Images/document.png" Width="16" Height="16"/>
            <TextBlock Text="Document" Margin="4,0,0,0"/>
        </StackPanel>
    </syncfusion:SfTabItem.Header>
    <syncfusion:SfTabItem.Content>
        <!-- Tab content -->
    </syncfusion:SfTabItem.Content>
</syncfusion:SfTabItem>
```

## Key Properties Overview

### Window Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `WindowType` | WindowType | Normal | `Tabbed` or `Normal` mode |
| `Title` | string | "" | Window title text |
| `Height` | double | Auto | Window height |
| `Width` | double | Auto | Window width |

### Tab Control Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AllowDragDrop` | bool | false | Enable drag-drop reordering |
| `EnableNewTabButton` | bool | false | Show new tab (+) button |
| `SelectedItem` | object | null | Currently active tab |
| `SelectedIndex` | int | -1 | Index of active tab |
| `ItemsSource` | IEnumerable | null | Data source for tabs (MVVM) |

### Tab Item Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Header` | object | null | Tab header content |
| `Content` | object | null | Tab body content |
| `CloseButtonVisibility` | Visibility | Collapsed | Show/hide close button |
| `IsSelected` | bool | false | Whether tab is active |

## Common Setup Patterns

### Pattern 1: Minimal Tabbed Window

```xml
<syncfusion:SfChromelessWindow WindowType="Tabbed">
    <syncfusion:SfTabControl>
        <syncfusion:SfTabItem Header="Tab 1" Content="Content 1"/>
        <syncfusion:SfTabItem Header="Tab 2" Content="Content 2"/>
    </syncfusion:SfTabControl>
</syncfusion:SfChromelessWindow>
```

### Pattern 2: Document Editor Setup

```xml
<syncfusion:SfChromelessWindow WindowType="Tabbed">
    <syncfusion:SfTabControl 
        AllowDragDrop="True"
        EnableNewTabButton="True"
        NewTabRequested="OnNewTabRequested">
        <syncfusion:SfTabItem Header="Untitled" CloseButtonVisibility="Visible">
            <TextBox AcceptsReturn="True"/>
        </syncfusion:SfTabItem>
    </syncfusion:SfTabControl>
</syncfusion:SfChromelessWindow>
```

### Pattern 3: Traditional MDI with Toolbar

```xml
<syncfusion:SfChromelessWindow WindowType="Normal">
    <DockPanel>
        <Menu DockPanel.Dock="Top">
            <MenuItem Header="File"/>
            <MenuItem Header="Edit"/>
        </Menu>
        <syncfusion:SfTabControl AllowDragDrop="True">
            <!-- Tabs here -->
        </syncfusion:SfTabControl>
    </DockPanel>
</syncfusion:SfChromelessWindow>
```

## Troubleshooting

### Assembly Reference Errors

**Problem:** `The type 'SfChromelessWindow' is not found`

**Solutions:**
1. Verify NuGet packages are installed
2. Check assembly references in project
3. Rebuild solution
4. Clean and rebuild if issues persist

```powershell
# Reinstall packages
Install-Package Syncfusion.SfChromelessWindow.WPF -Force
Install-Package Syncfusion.Shared.WPF -Force
```

### Namespace Not Found

**Problem:** `The name "SfChromelessWindow" does not exist in namespace`

**Solution:** Verify XAML namespace declaration:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Window Doesn't Inherit from SfChromelessWindow

**Problem:** Code-behind shows errors with `SfChromelessWindow`

**Solution:** Update class declaration:

```csharp
// Change from:
public partial class MainWindow : Window

// To:
public partial class MainWindow : SfChromelessWindow
```

And update XAML root element:

```xml
<!-- Change from: -->
<Window ...>

<!-- To: -->
<syncfusion:SfChromelessWindow ...>
```

### Tabs Not Appearing

**Problem:** Window displays but tabs are missing

**Solutions:**
1. Verify `SfTabControl` is set as window content
2. Check that `SfTabItem` elements are added
3. Ensure content is not hidden by other elements
4. Verify no layout issues (check Height/Width)

### License Warning Messages

**Problem:** Syncfusion license popup appears

**Solution:** Register license key in `App.xaml.cs`:

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Register Syncfusion license
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR-LICENSE-KEY");
        
        base.OnStartup(e);
    }
}
```

Get your license key from Syncfusion website or use community license for qualifying users.

## Next Steps

Now that you have a basic tabbed window running:

1. **Add tab management features** - Read `tab-management.md` to implement close buttons, new tab button, and keyboard shortcuts
2. **Implement MVVM pattern** - Read `data-binding.md` for data-driven tab scenarios
3. **Enable floating windows** - Read `merge-tabs.md` for tear-off and tab merging functionality
4. **Customize appearance** - Read `customization.md` for styling and theming

Your foundation is ready - explore these advanced features to build powerful document-based applications!
