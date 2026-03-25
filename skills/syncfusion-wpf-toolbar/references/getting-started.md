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

For scalable vector icons, use IconTemplate:

```xaml
<syncfusion:ToolBarAdv>
    <Button Width="22" Height="22" ToolTip="New">
        <Button.Content>
            <Path Data="M1,1 L9,1 L9,9 L1,9 Z" 
                  Fill="Black" 
                  Stretch="Uniform"
                  Width="16" 
                  Height="16"/>
        </Button.Content>
    </Button>
</syncfusion:ToolBarAdv>
```

### Using Resources for Reusable Icons

**Define in Resources:**

```xaml
<Window.Resources>
    <PathGeometry x:Key="NewIcon">
        M19,13H13V19H11V13H5V11H11V5H13V11H19V13Z
    </PathGeometry>
    
    <PathGeometry x:Key="SaveIcon">
        M15,9H5V5H15M12,19A3,3 0 0,1 9,16A3,3 0 0,1 12,13A3,3 0 0,1 15,16A3,3 0 0,1 12,19M17,3H5C3.89,3 3,3.9 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V7L17,3Z
    </PathGeometry>
</Window.Resources>
```

**Use in Buttons:**

```xaml
<syncfusion:ToolBarAdv>
    <Button Width="22" Height="22" ToolTip="New">
        <Path Data="{StaticResource NewIcon}" 
              Fill="Black" 
              Stretch="Uniform"
              Width="16" 
              Height="16"/>
    </Button>
    
    <Button Width="22" Height="22" ToolTip="Save">
        <Path Data="{StaticResource SaveIcon}" 
              Fill="Black" 
              Stretch="Uniform"
              Width="16" 
              Height="16"/>
    </Button>
</syncfusion:ToolBarAdv>
```

## CSS Imports and Themes

### Basic Styling

ToolBarAdv automatically inherits system theme by default. For custom appearance:

```xaml
<syncfusion:ToolBarAdv Background="LightGray" Foreground="Black">
    <!-- Items -->
</syncfusion:ToolBarAdv>
```

### Theme Integration

For consistent theming, use SfSkinManager (see [customization-theming.md](customization-theming.md)):

```csharp
SfSkinManager.SetTheme(this, new Theme("FluentLight"));
```

## Click Handlers and Events

### XAML with Event Handlers

```xaml
<syncfusion:ToolBarAdv>
    <Button Click="NewButton_Click" ToolTip="New">
        <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
    </Button>
    
    <Button Click="OpenButton_Click" ToolTip="Open">
        <Image Source="Images/openHS.png" Width="16" Height="16"/>
    </Button>
</syncfusion:ToolBarAdv>
```

**Code-behind:**

```csharp
private void NewButton_Click(object sender, RoutedEventArgs e)
{
    // Create new document
    MessageBox.Show("Creating new document...");
}

private void OpenButton_Click(object sender, RoutedEventArgs e)
{
    // Open document dialog
    Microsoft.Win32.OpenFileDialog dialog = new Microsoft.Win32.OpenFileDialog();
    dialog.Filter = "All Files (*.*)|*.*";
    if (dialog.ShowDialog() == true)
    {
        // Load document
        string fileName = dialog.FileName;
    }
}
```

### Commands (MVVM Pattern)

```xaml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>

<syncfusion:ToolBarAdv>
    <Button Command="{Binding NewCommand}" ToolTip="New">
        <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
    </Button>
    
    <Button Command="{Binding OpenCommand}" ToolTip="Open">
        <Image Source="Images/openHS.png" Width="16" Height="16"/>
    </Button>
</syncfusion:ToolBarAdv>
```

**ViewModel:**

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    public ICommand NewCommand { get; }
    public ICommand OpenCommand { get; }

    public MainViewModel()
    {
        NewCommand = new RelayCommand(ExecuteNew);
        OpenCommand = new RelayCommand(ExecuteOpen);
    }

    private void ExecuteNew()
    {
        // Create new document logic
    }

    private void ExecuteOpen()
    {
        // Open document logic
    }
}
```

## Complete Working Example

```xaml
<Window x:Class="ToolBarDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="ToolBarAdv Demo" Height="450" Width="800">
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <!-- Toolbar -->
        <syncfusion:ToolBarTrayAdv Grid.Row="0">
            <syncfusion:ToolBarAdv ToolBarName="Standard" Height="40">
                <Button Width="22" Height="22" ToolTip="New" Click="New_Click">
                    <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
                </Button>
                <Button Width="22" Height="22" ToolTip="Open" Click="Open_Click">
                    <Image Source="Images/openHS.png" Width="16" Height="16"/>
                </Button>
                <syncfusion:ToolBarItemSeparator />
                <Button Width="22" Height="22" ToolTip="Save" Click="Save_Click">
                    <Image Source="Images/saveHS.png" Width="16" Height="16"/>
                </Button>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
        
        <!-- Content Area -->
        <TextBox Grid.Row="1" 
                 AcceptsReturn="True" 
                 TextWrapping="Wrap"
                 VerticalScrollBarVisibility="Auto"/>
    </Grid>
</Window>
```

## Next Steps

- **Positioning and Layout**: Learn about Band and BandIndex properties → [positioning-and-layout.md](positioning-and-layout.md)
- **Add/Remove Buttons**: Enable user customization → [add-remove-buttons.md](add-remove-buttons.md)
- **Toolbar States**: Implement floating and docked states → [toolbar-states.md](toolbar-states.md)
- **ToolBarManager**: Organize toolbars at multiple positions → [toolbar-manager.md](toolbar-manager.md)
- **Customization**: Apply themes and styling → [customization-theming.md](customization-theming.md)
