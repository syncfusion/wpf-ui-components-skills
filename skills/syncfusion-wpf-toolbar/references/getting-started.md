# Getting Started with ToolBarAdv

This guide covers the installation, basic setup, and initial implementation of Syncfusion WPF ToolBarAdv.

## Assembly and Package Installation

### Required Assembly
- **Assembly Name:** `Syncfusion.Shared.WPF`
- **Namespace:** `Syncfusion.Windows.Tools.Controls`

### NuGet Package Installation

Install via Package Manager Console:

```powershell
Install-Package Syncfusion.Shared.WPF
```

Or via NuGet Package Manager UI:
1. Right-click on your project → Manage NuGet Packages
2. Search for "Syncfusion.Shared.WPF"
3. Install the latest version

### License Key Registration

For versions 16.2.0.x and later, register your license key in `App.xaml.cs`:

```csharp
using Syncfusion.Licensing;

protected override void OnStartup(StartupEventArgs e)
{
    // Register Syncfusion license key
    SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    base.OnStartup(e);
}
```

## Creating ToolBarAdv in XAML

### Basic XAML Setup

Add namespace declaration:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
```

### Minimal Implementation

```xaml
<syncfusion:ToolBarTrayAdv>
    <syncfusion:ToolBarAdv ToolBarName="Standard" Height="40">
        <Button Content="New"/>
        <Button Content="Open"/>
        <Button Content="Save"/>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

### With Buttons and Icons

```xaml
<syncfusion:ToolBarTrayAdv>
    <syncfusion:ToolBarAdv ToolBarName="File Operations" Height="40">
        <!-- New Document -->
        <Button Width="22" Height="22" ToolTip="New Document">
            <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
        </Button>
        
        <!-- Open Document -->
        <Button Width="22" Height="22" ToolTip="Open Document">
            <Image Source="Images/openHS.png" Width="16" Height="16"/>
        </Button>
        
        <!-- Separator -->
        <syncfusion:ToolBarItemSeparator />
        
        <!-- Save Document -->
        <Button Width="22" Height="22" ToolTip="Save Document">
            <Image Source="Images/saveHS.png" Width="16" Height="16"/>
        </Button>
        
        <!-- Save As -->
        <Button Width="22" Height="22" ToolTip="Save As">
            <Image Source="Images/SaveAsHS.png" Width="16" Height="16"/>
        </Button>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

## Creating ToolBarAdv in C#

### Basic C# Implementation

```csharp
using Syncfusion.Windows.Tools.Controls;
using System.Windows;
using System.Windows.Controls;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        CreateToolBar();
    }

    private void CreateToolBar()
    {
        // Create ToolBarTray
        ToolBarTrayAdv tray = new ToolBarTrayAdv();
        
        // Create ToolBar
        ToolBarAdv toolBar = new ToolBarAdv();
        toolBar.ToolBarName = "Standard";
        toolBar.Height = 40;
        
        // Add buttons
        toolBar.Items.Add(CreateButton("New", "Images/NewDocumentHS.png"));
        toolBar.Items.Add(CreateButton("Open", "Images/openHS.png"));
        toolBar.Items.Add(new ToolBarItemSeparator());
        toolBar.Items.Add(CreateButton("Save", "Images/saveHS.png"));
        
        // Add toolbar to tray
        tray.ToolBars.Add(toolBar);
        
        // Set as window content
        this.Content = tray;
    }

    private Button CreateButton(string tooltip, string imagePath)
    {
        Button button = new Button
        {
            Width = 22,
            Height = 22,
            ToolTip = tooltip
        };
        
        Image image = new Image
        {
            Source = new System.Windows.Media.Imaging.BitmapImage(
                new System.Uri(imagePath, System.UriKind.Relative)),
            Width = 16,
            Height = 16
        };
        
        button.Content = image;
        return button;
    }
}
```

## ToolBarAdv Structure Overview

### Visual Components

A ToolBarAdv consists of:

1. **Gripper** - Handle for dragging and repositioning the toolbar
2. **Toolbar Items** - Buttons, controls, and separators
3. **Overflow Button** - Chevron (>>) that shows items that don't fit
4. **Overflow Panel** - Popup showing overflow items

### Structure Hierarchy

```
ToolBarTrayAdv (Container)
└── ToolBarAdv (Toolbar)
    ├── Gripper (Drag handle)
    ├── Items Panel (Visible items)
    │   ├── Button
    │   ├── ToolBarItemSeparator
    │   ├── Button
    │   └── ...
    └── Overflow Button + Panel (Hidden items)
```

## Adding Different Control Types

### Buttons

```xaml
<Button Width="22" Height="22" ToolTip="Action">
    <Image Source="Images/icon.png" Width="16" Height="16"/>
</Button>
```

### Separators

```xaml
<syncfusion:ToolBarItemSeparator />
```

### ComboBoxes

```xaml
<ComboBox Width="100" Height="22" SelectedIndex="0">
    <ComboBoxItem Content="Option 1"/>
    <ComboBoxItem Content="Option 2"/>
    <ComboBoxItem Content="Option 3"/>
</ComboBox>
```

### TextBoxes

```xaml
<TextBox Width="120" Height="22" Text="Search..."/>
```

### CheckBoxes

```xaml
<CheckBox Content="Show Gridlines" Margin="5,0"/>
```

## Icon Templates with Path Data

For scalable vector icons, use `Path` with `Fill` inside button content:

```xaml
<syncfusion:ToolBarAdv>
    <Button Width="22" Height="22" ToolTip="New">
        <Path Data="M19,13H13V19H11V13H5V11H11V5H13V11H19V13Z"
              Fill="Black" Stretch="Uniform" Width="16" Height="16"/>
    </Button>
</syncfusion:ToolBarAdv>
```

## Themes

Apply a Syncfusion theme using `SfSkinManager` — see [customization-theming.md](customization-theming.md) for all options:

```csharp
SfSkinManager.SetTheme(this, new Theme("FluentLight"));
```

## Commands (MVVM)

Bind toolbar buttons to ViewModel commands:

```xaml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>

<syncfusion:ToolBarAdv>
    <Button Command="{Binding NewCommand}" ToolTip="New">
        <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
    </Button>
</syncfusion:ToolBarAdv>
```
