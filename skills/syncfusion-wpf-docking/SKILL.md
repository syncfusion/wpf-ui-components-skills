---
name: syncfusion-wpf-docking
description: Implements the Syncfusion WPF DockingManager control for Visual Studio-like docking interfaces with MDI/TDI support, floating windows, and auto-hide panels. Use this when creating docking layouts, window management systems, tabbed document interfaces, or IDE-style layouts in WPF applications. Covers dock panels, floating windows, auto-hide functionality, state persistence, and window management.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF DockingManager

The Syncfusion WPF DockingManager control provides a comprehensive window management system similar to Visual Studio, allowing users to create sophisticated docking interfaces with multiple window states. It enables child controls to be docked, floated, auto-hidden, or displayed as documents, with full support for drag-and-drop repositioning, state persistence, and extensive customization options.

## When to Use This Skill

Use this skill when implementing:
- **IDE-like Applications**: Creating Visual Studio-style interfaces with tool windows and document areas
- **Document Management**: MDI (Multiple Document Interface) or TDI (Tabbed Document Interface) applications
- **Complex Layouts**: Applications requiring flexible window arrangements with docking, floating, and auto-hide capabilities
- **Dashboard Applications**: Data analysis tools with repositionable panels and persistent layouts
- **Design Applications**: CAD, image editors, or other design tools requiring customizable workspace layouts
- **Multi-Panel Interfaces**: Any application where users need to arrange multiple views and save their layout preferences

## Component Overview

DockingManager is a container control that manages child windows in four distinct states:
- **Dock State**: Windows anchored to sides of the container (left, right, top, bottom, or tabbed)
- **Float State**: Windows that can be positioned anywhere, including on secondary monitors
- **AutoHidden State**: Windows collapsed to screen edges that expand on hover
- **Document State**: Windows displayed as MDI or TDI documents in a central area

Key capabilities include interactive drag-and-drop for repositioning, layout serialization for saving/loading window arrangements, extensive theming and styling support, keyboard navigation between windows, nested and linked manager support, and comprehensive customization through properties, events, and templates.

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and configure DockingManager
- Add DockingManager via Designer, XAML, or C#
- Set up basic window structure with child controls
- Configure headers, initial states, and sides
- Create your first working docking interface

### Understanding Window States

📄 **Read:** [references/window-states.md](references/window-states.md)

When you need to:
- Understand the four dock states (Dock, Float, AutoHidden, Document)
- Set and change window states programmatically or in XAML
- Handle state transitions and events
- Configure UseDocumentContainer for Document state

📄 **Read:** [references/autohide-windows.md](references/autohide-windows.md)

When you need to:
- Implement auto-hide functionality
- AutoHide all docked windows
- Unpin auto-hidden windows
- Customize auto-hide behavior
- Retrieve auto-hidden window dimensions

### Configuring Docking Windows

📄 **Read:** [references/docking-windows.md](references/docking-windows.md)

When you need to:
- Position windows on different sides (Left, Right, Top, Bottom)
- Dock windows relative to target windows (TargetNameInDockedMode)
- Enable maximize/minimize for docked windows
- Limit maximized window size
- Enable full-screen maximization
- Configure hot tracking and header visibility
- Add custom context menus for docked windows
- Control docking functionality per window

### Managing Floating Windows

📄 **Read:** [references/floating-windows.md](references/floating-windows.md)

When you need to:
- Create and configure native float windows
- Enable float windows in taskbar
- Support multi-monitor scenarios
- Position float windows at specific locations
- Enable maximize/minimize for float windows
- Configure double-click behavior (dock vs. maximize)
- Implement snapping between float windows
- Add custom context menus for float windows
- Enable rolling up float windows

### MDI/TDI Document Management

📄 **Read:** [references/mdi-tdi-functionalities.md](references/mdi-tdi-functionalities.md)

When you need to:
- Choose between MDI and TDI modes
- Configure container mode (DocumentContainerMode)
- Manage MDI window states (minimized, maximized, normal)
- Set MDI layouts (horizontal, vertical, cascade)
- Enable TDI drag and drop for tab reordering
- Create horizontal and vertical tab groups
- Close tabs on middle-click
- Pin and unpin document tabs
- Handle document tab order events
- Customize TDI headers and close menus
- Add custom context menus for tabs
- Control tab list context menus

### Saving and Loading Layouts

📄 **Read:** [references/state-persistence.md](references/state-persistence.md)

When you need to:
- Enable automatic layout persistence
- Save dock state to XML or binary format
- Load previously saved layouts
- Clear persisted state entries
- Handle window detection and closing events
- Manage serialization for specific windows

### Styling and Customization

📄 **Read:** [references/styling-and-customization.md](references/styling-and-customization.md)

When you need to:
- Apply visual styles and themes (VisualStudio2013, Office2016, Metro)
- Use SfSkinManager for theming
- Customize window appearance and headers
- Set header icons for MDI windows
- Configure tooltips for child windows
- Customize splitter appearance
- Modify context menu items
- Remove specific menu items

### Advanced Features

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When you need to:
- Create linked DockingManagers
- Implement nested layouts (DockingManager within DockingManager)
- Enable full-screen mode
- Configure keyboard navigation modes (Immediate, List, QuickTabs, VS2005, VistaFlip)
- Programmatically activate windows
- Handle ActiveWindow change events
- Detect window hosting in DockingManager
- Customize dock hints
- Configure VS2010 dragging behavior
- Disable drag and drop for TDI items
- Use advanced properties (FlipItems, IsDragging)

## Quick Start

### Basic Docking Layout

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:DockingManager x:Name="dockingManager" 
                                   UseDocumentContainer="True">
            
            <!-- Docked Window - Right Side -->
            <ContentControl x:Name="SolutionExplorer"
                          syncfusion:DockingManager.Header="Solution Explorer"
                          syncfusion:DockingManager.SideInDockedMode="Right"
                          syncfusion:DockingManager.DesiredWidthInDockedMode="250">
                <TextBlock Text="Solution Explorer Content" />
            </ContentControl>
            
            <!-- Auto-Hidden Window -->
            <ContentControl x:Name="Toolbox"
                          syncfusion:DockingManager.Header="Toolbox"
                          syncfusion:DockingManager.State="AutoHidden">
                <TextBlock Text="Toolbox Content" />
            </ContentControl>
            
            <!-- Float Window -->
            <ContentControl x:Name="Properties"
                          syncfusion:DockingManager.Header="Properties"
                          syncfusion:DockingManager.State="Float">
                <TextBlock Text="Properties Content" />
            </ContentControl>
            
            <!-- Document Window (TDI) -->
            <ContentControl x:Name="StartPage"
                          syncfusion:DockingManager.Header="Start Page"
                          syncfusion:DockingManager.State="Document">
                <TextBlock Text="Document Content" />
            </ContentControl>
        </syncfusion:DockingManager>
    </Grid>
</Window>
```

### Code-Behind Setup

```csharp
using Syncfusion.Windows.Tools.Controls;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Create DockingManager
        DockingManager dockingManager = new DockingManager();
        dockingManager.UseDocumentContainer = true;
        
        // Create windows
        ContentControl solutionExplorer = new ContentControl();
        ContentControl toolbox = new ContentControl();
        ContentControl properties = new ContentControl();
        ContentControl startPage = new ContentControl();
        
        // Configure Solution Explorer (Docked Right)
        DockingManager.SetHeader(solutionExplorer, "Solution Explorer");
        DockingManager.SetSideInDockedMode(solutionExplorer, DockSide.Right);
        DockingManager.SetDesiredWidthInDockedMode(solutionExplorer, 250);
        
        // Configure Toolbox (Auto-Hidden)
        DockingManager.SetHeader(toolbox, "Toolbox");
        DockingManager.SetState(toolbox, DockState.AutoHidden);
        
        // Configure Properties (Float)
        DockingManager.SetHeader(properties, "Properties");
        DockingManager.SetState(properties, DockState.Float);
        
        // Configure Start Page (Document)
        DockingManager.SetHeader(startPage, "Start Page");
        DockingManager.SetState(startPage, DockState.Document);
        
        // Add children
        dockingManager.Children.Add(solutionExplorer);
        dockingManager.Children.Add(toolbox);
        dockingManager.Children.Add(properties);
        dockingManager.Children.Add(startPage);
        
        this.Content = dockingManager;
    }
}
```

## Common Patterns

### Pattern 1: Tabbed Windows in Same Container

```xaml
<syncfusion:DockingManager x:Name="dockingManager">
    <!-- Target Window -->
    <ContentControl x:Name="MainPanel"
                    syncfusion:DockingManager.Header="Main Panel"
                    syncfusion:DockingManager.SideInDockedMode="Left"/>
    
    <!-- Tabbed with Main Panel -->
    <ContentControl syncfusion:DockingManager.Header="Secondary Panel"
                    syncfusion:DockingManager.SideInDockedMode="Tabbed"
                    syncfusion:DockingManager.TargetNameInDockedMode="MainPanel"/>
</syncfusion:DockingManager>
```

### Pattern 2: State Persistence

```xaml
<syncfusion:DockingManager x:Name="dockingManager" 
                           PersistState="True">
    <!-- Windows will automatically save/load state -->
</syncfusion:DockingManager>
```

```csharp
// Save state manually
dockingManager.SaveDockState();

// Load state manually
dockingManager.LoadDockState();
```

### Pattern 3: TDI with Tab Groups

```xaml
<syncfusion:DockingManager UseDocumentContainer="True" 
                           ContainerMode="TDI"
                           TabGroupEnabled="True">
    <ContentControl syncfusion:DockingManager.Header="Document 1"
                    syncfusion:DockingManager.State="Document"/>
    <ContentControl syncfusion:DockingManager.Header="Document 2"
                    syncfusion:DockingManager.State="Document"/>
</syncfusion:DockingManager>
```

```csharp
// Create vertical tab group programmatically
(dockingManager.DocContainer as DocumentContainer)
    .CreateVerticalTabGroup(document1);
```

### Pattern 4: Native Float Windows with Multi-Monitor

```xaml
<syncfusion:DockingManager UseNativeFloatWindow="True"
                           ShowFloatWindowInTaskbar="True">
    <ContentControl syncfusion:DockingManager.Header="Float Window"
                    syncfusion:DockingManager.State="Float"
                    syncfusion:DockingManager.CanFloatMaximize="True"/>
</syncfusion:DockingManager>
```

### Pattern 5: Applying Themes

```xaml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <syncfusion:DockingManager 
        syncfusionskin:SfSkinManager.VisualStyle="VisualStudio2013">
        <!-- Windows -->
    </syncfusion:DockingManager>
</Window>
```

```csharp
// Apply theme in code
SfSkinManager.SetVisualStyle(dockingManager, VisualStyles.VisualStudio2013);
```

## Key Properties

### Essential DockingManager Properties

| Property | Type | Purpose | When to Use |
|----------|------|---------|-------------|
| **UseDocumentContainer** | bool | Enables Document state for windows | When you need MDI/TDI document areas |
| **ContainerMode** | DocumentContainerMode | Sets MDI or TDI mode | To choose between Multiple Document Interface (MDI) or Tabbed Document Interface (TDI) |
| **PersistState** | bool | Auto-saves layout on close | When users need to preserve window arrangements between sessions |
| **UseNativeFloatWindow** | bool | Uses WPF Window for float | For better multi-monitor support and resizing |
| **MaximizeButtonEnabled** | bool | Shows maximize button on docked windows | When users need to maximize docked panels |
| **MinimizeButtonEnabled** | bool | Shows minimize button on docked windows | When users need to minimize docked panels |
| **TabGroupEnabled** | bool | Allows creating horizontal/vertical tab groups | For advanced TDI document grouping |
| **IsVS2010DraggingEnabled** | bool | Enables VS2010-style dragging | For modern drag behavior where TDI tabs can float |

### Essential Attached Properties (Per Window)

| Property | Type | Purpose | When to Use |
|----------|------|---------|-------------|
| **Header** | object | Sets window title | Always - required for identifying windows |
| **State** | DockState | Sets window state (Dock/Float/AutoHidden/Document) | To control initial or runtime window state |
| **SideInDockedMode** | DockSide | Sets dock position (Left/Right/Top/Bottom/Tabbed) | When docking windows to specific sides |
| **TargetNameInDockedMode** | string | Specifies target window for relative docking | For tabbed or side-by-side arrangements |
| **DesiredWidthInDockedMode** | double | Sets width when docked | To control initial size of side-docked windows |
| **DesiredHeightInDockedMode** | double | Sets height when docked | To control initial size of top/bottom-docked windows |
| **CanDock** | bool | Enables/disables docking | To restrict certain windows from being docked |
| **CanFloat** | bool | Enables/disables floating | To restrict certain windows from floating |
| **CanClose** | bool | Enables/disables closing | To create mandatory windows that can't be closed |
| **NoHeader** | bool | Hides window header | For embedded content that doesn't need a title bar |

## Common Use Cases

### Use Case 1: IDE-Style Development Tool

Create a Visual Studio-like interface with:
- Solution Explorer docked on the right
- Toolbox auto-hidden on the left
- Properties panel floating or docked
- Code editor as TDI document
- Output window docked at bottom

**Approach:** Use `UseDocumentContainer="True"` with `ContainerMode="TDI"` for the main document area. Configure tool windows with appropriate states (Dock, AutoHidden) and enable `PersistState` to save user's layout preferences.

### Use Case 2: Multi-Document Data Analysis Dashboard

Create a data analysis application with:
- Multiple chart windows in TDI mode
- Data grid as a docked panel
- Filter panel that can float
- Tab groups for comparing charts side-by-side

**Approach:** Use TDI mode with `TabGroupEnabled="True"` to allow users to create horizontal/vertical tab groups. Enable `IsVS2010DraggingEnabled` for flexible document arrangement.

### Use Case 3: Design Application with Persistent Workspace

Create a CAD or image editing application where:
- Tools panel can be docked or floated
- Properties adjust based on selection
- Canvas is the central document
- Layout persists between sessions
- Support for multiple monitors

**Approach:** Use `UseNativeFloatWindow="True"` for multi-monitor support, enable `ShowFloatWindowInTaskbar="True"` for better taskbar integration, and use `PersistState="True"` with custom serialization for saving complete workspace state.

### Use Case 4: Document Editor with Context Panels

Create a document editor with:
- Document area using TDI with pinnable tabs
- Formatting panel that auto-hides
- Outline/navigation panel docked
- Comments panel that can be detached

**Approach:** Use TDI mode with pin/unpin functionality (`AllowPin="True"`, `ShowPin="True"`). Configure auto-hide for less frequently used panels. Allow floating for collaborative review scenarios.

### Use Case 5: Complex Data Entry Application

Create a data entry application with:
- Multiple forms in MDI windows
- Reference data in docked side panels
- Validation messages in bottom panel
- Customizable workspace per user role

**Approach:** Use `ContainerMode="MDI"` for traditional multiple document interface. Implement custom serialization to save role-specific layouts. Use `CanClose="False"` for mandatory reference panels.
