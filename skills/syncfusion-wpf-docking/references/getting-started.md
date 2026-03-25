# Getting Started with DockingManager

This guide covers installation, basic setup, and creating your first DockingManager implementation in WPF applications.

## Assembly Dependencies

The DockingManager control requires the following assemblies:

- **Syncfusion.Shared.Wpf** - Core shared controls
- **Syncfusion.Tools.Wpf** - DockingManager and related controls

### NuGet Package Installation

1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for `Syncfusion.Tools.WPF`
4. Install the package

### Manual Assembly References

If not using NuGet, add references to:
- `Syncfusion.Shared.Wpf.dll`
- `Syncfusion.Tools.Wpf.dll`

These DLLs are typically located in:
```
C:\Program Files (x86)\Syncfusion\Essential Studio\WPF\[Version]\Assemblies\[.NET Version]\
```

## Creating DockingManager

There are three ways to add DockingManager to your application:

### Method 1: Adding via Designer

1. Open your WPF Window/UserControl in the Visual Studio Designer
2. Open the Toolbox (View → Toolbox)
3. Locate **DockingManager** under Syncfusion WPF Toolbox
4. Drag and drop it onto your design surface

The required assemblies will be added automatically, and the namespace will be imported.

### Method 2: Adding in XAML

Add the Syncfusion namespace and declare the DockingManager:

```xaml
<Window x:Class="DockingApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="DockingManager Demo" Height="600" Width="800">
    <Grid>
        <syncfusion:DockingManager x:Name="dockingManager">
            <ContentControl x:Name="SolutionExplorer"/>
            <ContentControl x:Name="Toolbox"/>
            <ContentControl x:Name="Properties"/>
            <ContentControl x:Name="Output"/>
        </syncfusion:DockingManager>
    </Grid>
</Window>
```

**Key Points:**
- Import namespace: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`
- Always give DockingManager a `Name` (x:Name attribute)
- Child controls must have names for state persistence

### Method 3: Adding in C# Code-Behind

Create and configure DockingManager programmatically:

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Tools.Controls;

namespace DockingApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create DockingManager instance
            DockingManager dockingManager = new DockingManager();
            
            // Create child windows
            ContentControl solutionExplorer = new ContentControl();
            solutionExplorer.Name = "SolutionExplorer";
            
            ContentControl toolbox = new ContentControl();
            toolbox.Name = "Toolbox";
            
            ContentControl properties = new ContentControl();
            properties.Name = "Properties";
            
            ContentControl output = new ContentControl();
            output.Name = "Output";
            
            // Add children to DockingManager
            dockingManager.Children.Add(solutionExplorer);
            dockingManager.Children.Add(toolbox);
            dockingManager.Children.Add(properties);
            dockingManager.Children.Add(output);
            
            // Set as window content
            this.Content = dockingManager;
        }
    }
}
```

## Setting Window Headers

Each child window needs a header (title) displayed in its title bar:

### XAML Approach

```xaml
<syncfusion:DockingManager x:Name="dockingManager">
    <ContentControl x:Name="SolutionExplorer" 
                    syncfusion:DockingManager.Header="Solution Explorer"/>
    <ContentControl x:Name="Toolbox" 
                    syncfusion:DockingManager.Header="Toolbox"/>
    <ContentControl x:Name="Properties"
                    syncfusion:DockingManager.Header="Properties"/>
    <ContentControl x:Name="Output"
                    syncfusion:DockingManager.Header="Output"/>
</syncfusion:DockingManager>
```

### C# Approach

```csharp
DockingManager.SetHeader(solutionExplorer, "Solution Explorer");
DockingManager.SetHeader(toolbox, "Toolbox");
DockingManager.SetHeader(properties, "Properties");
DockingManager.SetHeader(output, "Output");
```

## Configuring Window States

DockingManager supports four window states:

- **Dock** (default): Docked to container sides
- **Float**: Free-floating window
- **AutoHidden**: Collapsed to edge, expands on hover
- **Document**: Displayed in document area (requires UseDocumentContainer)

### Setting States in XAML

```xaml
<syncfusion:DockingManager x:Name="dockingManager" UseDocumentContainer="True">
    <!-- Docked Window (default, no State needed) -->
    <ContentControl x:Name="SolutionExplorer"
                    syncfusion:DockingManager.Header="Solution Explorer"/>
    
    <!-- Auto-Hidden Window -->
    <ContentControl x:Name="Toolbox"
                    syncfusion:DockingManager.Header="Toolbox"
                    syncfusion:DockingManager.State="AutoHidden"/>
    
    <!-- Float Window -->
    <ContentControl x:Name="Properties"
                    syncfusion:DockingManager.Header="Properties"
                    syncfusion:DockingManager.State="Float"/>
    
    <!-- Document Window -->
    <ContentControl x:Name="StartPage"
                    syncfusion:DockingManager.Header="Start Page"
                    syncfusion:DockingManager.State="Document"/>
</syncfusion:DockingManager>
```

### Setting States in C#

```csharp
// Enable document container for Document state
dockingManager.UseDocumentContainer = true;

// Set states
DockingManager.SetState(solutionExplorer, DockState.Dock);
DockingManager.SetState(toolbox, DockState.AutoHidden);
DockingManager.SetState(properties, DockState.Float);
DockingManager.SetState(startPage, DockState.Document);
```

**Important:** Set `UseDocumentContainer="True"` to enable Document state.

## Positioning Docked Windows

Control where docked windows appear using `SideInDockedMode`:

### XAML Example

```xaml
<syncfusion:DockingManager x:Name="dockingManager">
    <!-- Dock to Right -->
    <ContentControl syncfusion:DockingManager.Header="Solution Explorer"
                    syncfusion:DockingManager.SideInDockedMode="Right"
                    syncfusion:DockingManager.DesiredWidthInDockedMode="250"/>
    
    <!-- Dock to Left -->
    <ContentControl syncfusion:DockingManager.Header="Toolbox"
                    syncfusion:DockingManager.SideInDockedMode="Left"
                    syncfusion:DockingManager.DesiredWidthInDockedMode="200"/>
    
    <!-- Dock to Bottom -->
    <ContentControl syncfusion:DockingManager.Header="Output"
                    syncfusion:DockingManager.SideInDockedMode="Bottom"
                    syncfusion:DockingManager.DesiredHeightInDockedMode="150"/>
</syncfusion:DockingManager>
```

### C# Example

```csharp
// Dock to right side with width
DockingManager.SetSideInDockedMode(solutionExplorer, DockSide.Right);
DockingManager.SetDesiredWidthInDockedMode(solutionExplorer, 250);

// Dock to left side with width
DockingManager.SetSideInDockedMode(toolbox, DockSide.Left);
DockingManager.SetDesiredWidthInDockedMode(toolbox, 200);

// Dock to bottom with height
DockingManager.SetSideInDockedMode(output, DockSide.Bottom);
DockingManager.SetDesiredHeightInDockedMode(output, 150);
```

**Available Sides:**
- `Left` - Dock to left edge (default)
- `Right` - Dock to right edge
- `Top` - Dock to top edge
- `Bottom` - Dock to bottom edge
- `Tabbed` - Create tabbed group (requires TargetNameInDockedMode)

## Complete Starter Example

Here's a complete working example combining all concepts:

### MainWindow.xaml

```xaml
<Window x:Class="DockingApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="DockingManager Starter" Height="600" Width="900">
    <Grid>
        <syncfusion:DockingManager x:Name="dockingManager" 
                                   UseDocumentContainer="True"
                                   PersistState="True">
            
            <!-- Solution Explorer - Docked Right -->
            <ContentControl x:Name="SolutionExplorer"
                          syncfusion:DockingManager.Header="Solution Explorer"
                          syncfusion:DockingManager.SideInDockedMode="Right"
                          syncfusion:DockingManager.DesiredWidthInDockedMode="250">
                <TreeView>
                    <TreeViewItem Header="Project">
                        <TreeViewItem Header="MainWindow.xaml"/>
                        <TreeViewItem Header="App.xaml"/>
                    </TreeViewItem>
                </TreeView>
            </ContentControl>
            
            <!-- Toolbox - Auto-Hidden Left -->
            <ContentControl x:Name="Toolbox"
                          syncfusion:DockingManager.Header="Toolbox"
                          syncfusion:DockingManager.State="AutoHidden"
                          syncfusion:DockingManager.SideInDockedMode="Left">
                <ListBox>
                    <ListBoxItem Content="Button"/>
                    <ListBoxItem Content="TextBox"/>
                    <ListBoxItem Content="ListBox"/>
                </ListBox>
            </ContentControl>
            
            <!-- Properties - Float -->
            <ContentControl x:Name="Properties"
                          syncfusion:DockingManager.Header="Properties"
                          syncfusion:DockingManager.State="Float">
                <Grid>
                    <TextBlock Text="Properties Panel" 
                             VerticalAlignment="Center" 
                             HorizontalAlignment="Center"/>
                </Grid>
            </ContentControl>
            
            <!-- Output - Docked Bottom -->
            <ContentControl x:Name="Output"
                          syncfusion:DockingManager.Header="Output"
                          syncfusion:DockingManager.SideInDockedMode="Bottom"
                          syncfusion:DockingManager.DesiredHeightInDockedMode="120">
                <TextBox Text="Output window content..." 
                       TextWrapping="Wrap" 
                       AcceptsReturn="True"/>
            </ContentControl>
            
            <!-- Start Page - Document -->
            <ContentControl x:Name="StartPage"
                          syncfusion:DockingManager.Header="Start Page"
                          syncfusion:DockingManager.State="Document">
                <StackPanel Margin="20">
                    <TextBlock Text="Welcome to DockingManager" 
                             FontSize="24" 
                             FontWeight="Bold" 
                             Margin="0,0,0,20"/>
                    <TextBlock Text="This is a document window in the central area." 
                             TextWrapping="Wrap"/>
                </StackPanel>
            </ContentControl>
            
        </syncfusion:DockingManager>
    </Grid>
</Window>
```

### MainWindow.xaml.cs

```csharp
using System.Windows;
using Syncfusion.Windows.Tools.Controls;

namespace DockingApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Load saved layout on startup
            this.Loaded += MainWindow_Loaded;
        }

        private void MainWindow_Loaded(object sender, RoutedEventArgs e)
        {
            // Load previously saved dock state
            dockingManager.LoadDockState();
        }
    }
}
```

## Common Gotchas and Best Practices

### 1. Always Set Names
```csharp
// ❌ BAD - No name
ContentControl window1 = new ContentControl();

// ✅ GOOD - Named for state persistence
ContentControl window1 = new ContentControl();
window1.Name = "SolutionExplorer";
```

### 2. Enable UseDocumentContainer for Document State
```xaml
<!-- ❌ BAD - Document state won't work -->
<syncfusion:DockingManager>
    <ContentControl syncfusion:DockingManager.State="Document"/>
</syncfusion:DockingManager>

<!-- ✅ GOOD - UseDocumentContainer enabled -->
<syncfusion:DockingManager UseDocumentContainer="True">
    <ContentControl syncfusion:DockingManager.State="Document"/>
</syncfusion:DockingManager>
```

### 3. Set Desired Sizes for Docked Windows
```xaml
<!-- Without size, window may be too small/large -->
<ContentControl syncfusion:DockingManager.SideInDockedMode="Right"
                syncfusion:DockingManager.DesiredWidthInDockedMode="250"/>
```

### 4. Use PersistState for Auto-Save
```xaml
<!-- Enable automatic layout saving -->
<syncfusion:DockingManager PersistState="True">
```

## Next Steps

Now that you have a basic DockingManager running:

- **Explore Window States**: Learn about Dock, Float, AutoHidden, and Document states in detail
- **Configure Layouts**: Set up complex arrangements with tabbed windows and target positioning
- **Enable Persistence**: Save and load user layouts automatically
- **Apply Themes**: Customize appearance with built-in visual styles
- **Add Interactivity**: Handle drag-and-drop, context menus, and keyboard navigation
