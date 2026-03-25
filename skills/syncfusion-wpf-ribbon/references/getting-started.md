# Getting Started with Syncfusion WPF Ribbon

## Table of Contents
- [Installation](#installation)
- [Namespace Setup](#namespace-setup)
- [Basic Ribbon Creation](#basic-ribbon-creation)
- [Adding Tabs and Groups](#adding-tabs-and-groups)
- [Adding Buttons](#adding-buttons)
- [Event Handling](#event-handling)
- [Complete Example](#complete-example)
- [Troubleshooting](#troubleshooting)

## Installation

### Step 1: Add NuGet Package

Install the Syncfusion Tools package (contains Ribbon) from NuGet:

```powershell
Install-Package Syncfusion.Tools.Wpf
```

Or via NuGet Package Manager:
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Wpf.Ribbon"
3. Click Install

### Step 2: Add License (if required)

If using Syncfusion trial or licensed version, register the license in your application startup:

```csharp
using Syncfusion.Licensing;

// In App.xaml.cs or Program.cs
public partial class App : Application
{
    public App()
    {
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    }
}
```

## Namespace Setup

Add required namespaces to your XAML and C# files:

**XAML:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

**C# (if needed):**
```csharp
using Syncfusion.Windows.Tools.Controls;
```

## Basic Ribbon Creation

### Step 1: Create RibbonWindow

The ribbon must be hosted in a `RibbonWindow` instead of regular `Window`:

```xml
<!-- MainWindow.xaml -->
<syncfusion:RibbonWindow x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="My Ribbon Application" 
        Height="600" Width="1000">
    <!-- Ribbon and content go here -->
</syncfusion:RibbonWindow>
```

**Update the C# code-behind:**
```csharp
public partial class MainWindow : RibbonWindow
{
    public MainWindow()
    {
        InitializeComponent();
    }
}
```

### Step 2: Add Ribbon Control

Place a `Ribbon` control inside the `RibbonWindow`:

```xml
<syncfusion:Ribbon>
    <!-- Tabs will go here -->
</syncfusion:Ribbon>

<!-- Main content area -->
<Grid>
    <TextBlock Text="Main Application Content" />
</Grid>
```

## Adding Tabs and Groups

Ribbons are organized into tabs, and each tab contains groups of related commands.

```xml
<syncfusion:Ribbon>
    <!-- Home Tab -->
    <syncfusion:RibbonTab Caption="Home">
        <!-- Bars go inside tabs -->
    </syncfusion:RibbonTab>
    
    <!-- View Tab -->
    <syncfusion:RibbonTab Caption="View">
        <!-- More bars -->
    </syncfusion:RibbonTab>
</syncfusion:Ribbon>
```

### Adding Groups

Groups organize related commands within a tab:

```xml
<syncfusion:RibbonTab Caption="Home">
    <!-- File Operations Bar -->
    <syncfusion:RibbonBar Header="File">
        <!-- Buttons will go here -->
    </syncfusion:RibbonBar>
    
    <!-- Formatting Bar -->
    <syncfusion:RibbonBar Header="Formatting">
        <!-- More buttons -->
    </syncfusion:RibbonBar>
</syncfusion:RibbonTab>
```

## Adding Buttons

### Basic RibbonButton

```xml
<syncfusion:RibbonBar Header="File">
    <syncfusion:RibbonButton Label="New" />
    <syncfusion:RibbonButton Label="Open" />
    <syncfusion:RibbonButton Label="Save" />
</syncfusion:RibbonBar>
```

### Button with Icon

```xml
<syncfusion:RibbonButton Label="New" 
                         SmallIcon="pack://application:,,,/Resources/new.png"
                         SizeForm="Large" />
```

### Button Sizing

Control button appearance with `SizeForm`:

```xml
<!-- Large button (prominent) -->
<syncfusion:RibbonButton Label="New" SizeForm="Large" />

<!-- Normal button (standard) -->
<syncfusion:RibbonButton Label="Open" SizeForm="Normal" />

<!-- Small button (compact) -->
<syncfusion:RibbonButton Label="Save" SizeForm="Small" />
```

## Event Handling

### Click Event

Handle button clicks in C#:

```xml
<!-- XAML -->
<syncfusion:RibbonButton Label="Save" Click="OnSaveClick" />
```

```csharp
// Code-behind
private void OnSaveClick(object sender, EventArgs e)
{
    MessageBox.Show("Save command executed!");
}
```

### Command Binding (MVVM Pattern)

For MVVM applications, use command binding:

```xml
<!-- XAML -->
<syncfusion:RibbonButton Label="Save" 
                         Command="{Binding SaveCommand}" />
```

```csharp
// ViewModel
public ICommand SaveCommand { get; }

public MyViewModel()
{
    SaveCommand = new RelayCommand(() =>
    {
        // Handle save logic
        MessageBox.Show("Saved!");
    });
}
```

## Complete Example

Here's a complete working example:

```xml
<!-- MainWindow.xaml -->
<syncfusion:RibbonWindow x:Class="RibbonQuickStart.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Quick Start - Ribbon" 
        Height="500" Width="800">
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="300" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        
        <!-- Ribbon Bar -->
        <syncfusion:Ribbon Grid.Row="0">
            <syncfusion:RibbonTab Caption="Home">
                <syncfusion:RibbonBar Header="Clipboard">
                    <syncfusion:RibbonButton Label="Cut" Click="OnCutClick" />
                    <syncfusion:RibbonButton Label="Copy" Click="OnCopyClick" />
                    <syncfusion:RibbonButton Label="Paste" Click="OnPasteClick" />
                </syncfusion:RibbonBar>
            
                <syncfusion:RibbonBar Header="Styles">
                    <syncfusion:RibbonButton Label="Bold" />
                    <syncfusion:RibbonButton Label="Italic" />
                    <syncfusion:RibbonButton Label="Underline" />
                </syncfusion:RibbonBar>
            </syncfusion:RibbonTab>
        
            <syncfusion:RibbonTab Caption="Insert">
                <syncfusion:RibbonBar Header="Media">
                    <syncfusion:RibbonButton Label="Image" />
                    <syncfusion:RibbonButton Label="Table" />
                </syncfusion:RibbonBar>
            </syncfusion:RibbonTab>
        </syncfusion:Ribbon>
        
        <!-- Main Content -->
        <TextBox Grid.Row="1"
                 AcceptsReturn="True"
                 TextWrapping="Wrap"
                 Padding="10" />
        
        <!-- Status Bar -->
        <StatusBar Grid.Row="2">
            <TextBlock Text="Ready" Padding="10,0" />
        </StatusBar>
    </Grid>
</syncfusion:RibbonWindow>
```

```csharp
// MainWindow.xaml.cs
public partial class MainWindow : RibbonWindow
{
    public MainWindow()
    {
        InitializeComponent();
    }

    private void OnCutClick(object sender, EventArgs e)
    {
        Clipboard.SetText("Cut operation");
    }

    private void OnCopyClick(object sender, EventArgs e)
    {
        Clipboard.SetText("Copy operation");
    }

    private void OnPasteClick(object sender, EventArgs e)
    {
        var text = Clipboard.GetText();
        MessageBox.Show($"Pasting: {text}");
    }
}
```

## Troubleshooting

### Issue: Namespace not found
**Solution:** Make sure you've added `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"` in XAML and installed the NuGet package.

### Issue: RibbonWindow not recognized
**Solution:** Change your window base class from `Window` to `RibbonWindow` and update code-behind.

### Issue: Ribbon appears empty
**Solution:** Ensure you have `RibbonTab` elements inside the `Ribbon`, and `RibbonBar` elements inside tabs with actual controls.

### Issue: Icons not showing
**Solution:** Verify the image path is correct. Use `pack://application:,,,/YourAssembly;component/Resources/image.png` format or embed images as resources.

### Issue: Click events not firing
**Solution:** For event binding, ensure you're using the correct event name (`Click`). For commands, verify your view model command is properly initialized.
