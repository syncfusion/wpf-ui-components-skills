# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Linked DockingManager](#linked-dockingmanager)
- [Nested Docking](#nested-docking)
- [Full-Screen Mode](#full-screen-mode)
- [Keyboard Navigation](#keyboard-navigation)
- [Window Activation](#window-activation)
- [VS2010 Dragging](#vs2010-dragging)
- [Dock Hints Control](#dock-hints-control)
- [Pattern and Practices](#pattern-and-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Advanced features in DockingManager enable sophisticated layouts, cross-manager interactions, enhanced navigation, and fine-grained control over drag-and-drop behaviors. These features support complex application scenarios like multi-window applications, linked workspaces, and advanced keyboard workflows.

## Linked DockingManager

### Concept

Linked DockingManager allows dragging windows between separate DockingManager instances. By default, windows are confined to their parent DockingManager, but linking enables cross-manager drag-and-drop.

### Basic Setup

Create two linked DockingManagers:

**Window 1:**
```xml
<Window x:Class="MainWindow" x:Name="Window1">
    <syncfusion:DockingManager x:Name="DockingManager1" UseDocumentContainer="True">
        <ContentControl syncfusion:DockingManager.Header="Window1"
                        syncfusion:DockingManager.State="Document" />
        
        <ContentControl syncfusion:DockingManager.Header="Window2"
                        syncfusion:DockingManager.State="Dock"
                        syncfusion:DockingManager.SideInDockedMode="Left" />
    </syncfusion:DockingManager>
</Window>
```

**Window 2:**
```xml
<Window x:Class="SecondWindow" x:Name="Window2">
    <syncfusion:DockingManager x:Name="DockingManager2" UseDocumentContainer="True">
        <ContentControl syncfusion:DockingManager.Header="WindowA"
                        syncfusion:DockingManager.State="Dock"
                        syncfusion:DockingManager.SideInDockedMode="Right" />
    </syncfusion:DockingManager>
</Window>
```

**C# - Link Managers:**
```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Create second window
    SecondWindow secondWindow = new SecondWindow();
    secondWindow.Title = "Second DockingManager";
    secondWindow.Show();
    
    // Link bidirectionally
    DockingManager1.AddToTargetManagersList(secondWindow.DockingManager2);
    secondWindow.DockingManager2.AddToTargetManagersList(DockingManager1);
}
```

### One-Way Linking

Allow dragging from Manager1 to Manager2 only:

**C#:**
```csharp
// One-way: DockingManager1 → DockingManager2
DockingManager1.AddToTargetManagersList(DockingManager2);

// Windows can be dragged from Manager1 to Manager2
// But not from Manager2 back to Manager1
```

### Two-Way Linking

Enable bidirectional dragging:

**C#:**
```csharp
// Two-way: DockingManager1 ↔ DockingManager2
DockingManager1.AddToTargetManagersList(DockingManager2);
DockingManager2.AddToTargetManagersList(DockingManager1);
```

### Remove Linking

Unlink DockingManagers:

**C#:**
```csharp
// Remove link
DockingManager1.RemoveFromTargetManagersList(DockingManager2);
DockingManager2.RemoveFromTargetManagersList(DockingManager1);
```

### Use Cases

**Multi-Monitor Scenarios:**
```csharp
// Create DockingManager on secondary monitor
Window secondMonitor = new Window();
DockingManager secondDockManager = new DockingManager();
secondMonitor.Content = secondDockManager;

// Position on second screen
var secondScreen = System.Windows.Forms.Screen.AllScreens[1];
secondMonitor.Left = secondScreen.Bounds.Left;
secondMonitor.Top = secondScreen.Bounds.Top;
secondMonitor.Show();

// Link for cross-monitor drag-drop
mainDockManager.AddToTargetManagersList(secondDockManager);
secondDockManager.AddToTargetManagersList(mainDockManager);
```

## Nested Docking

### Concept

Nested Docking allows embedding a DockingManager as a child window within another DockingManager. The entire nested manager can be docked, floated, or auto-hidden as a single unit.

### Basic Nested Layout

**XAML:**
```xml
<syncfusion:DockingManager x:Name="ParentDockManager" 
                          UseDocumentContainer="True">
    
    <!-- Regular child window -->
    <ContentControl x:Name="Content1"
                    syncfusion:DockingManager.Header="Regular Window"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Top" />
    
    <!-- Nested DockingManager -->
    <syncfusion:DockingManager x:Name="NestedDockManager"
                              UseDocumentContainer="True"
                              syncfusion:DockingManager.Header="Nested Manager"
                              syncfusion:DockingManager.State="Dock"
                              syncfusion:DockingManager.SideInDockedMode="Left"
                              syncfusion:DockingManager.DesiredWidthInDockedMode="400">
        
        <!-- Children of nested manager -->
        <ContentControl syncfusion:DockingManager.Header="Nested Child 1"
                        syncfusion:DockingManager.State="Dock"
                        syncfusion:DockingManager.SideInDockedMode="Top" />
        
        <ContentControl syncfusion:DockingManager.Header="Nested Child 2"
                        syncfusion:DockingManager.State="Document" />
    </syncfusion:DockingManager>
    
    <!-- Another nested manager -->
    <syncfusion:DockingManager x:Name="NestedDockManager2"
                              UseDocumentContainer="True"
                              syncfusion:DockingManager.Header="Second Nested"
                              syncfusion:DockingManager.State="Dock"
                              syncfusion:DockingManager.SideInDockedMode="Bottom"
                              syncfusion:DockingManager.DesiredHeightInDockedMode="250">
        
        <ContentControl syncfusion:DockingManager.Header="Nested Child 3"
                        syncfusion:DockingManager.State="Dock" />
    </syncfusion:DockingManager>
    
</syncfusion:DockingManager>
```

### Nested Behavior

**Characteristics:**
- Entire nested DockingManager moves as one unit
- Children inside nested manager cannot be dragged to parent
- Parent can dock/float/auto-hide the nested manager
- Each manager maintains independent state

**Use Cases:**
- Group related tools into sub-workspaces
- Create modular layouts
- Isolate tool categories

### Programmatic Nested Creation

**C#:**
```csharp
// Create nested DockingManager
DockingManager nestedManager = new DockingManager
{
    UseDocumentContainer = true
};

// Set as child of parent
DockingManager.SetHeader(nestedManager, "Tools Section");
DockingManager.SetState(nestedManager, DockState.Dock);
DockingManager.SetSideInDockedMode(nestedManager, DockSide.Left);
DockingManager.SetDesiredWidthInDockedMode(nestedManager, 350);

// Add children to nested manager
ContentControl child1 = new ContentControl();
DockingManager.SetHeader(child1, "Tool 1");
DockingManager.SetState(child1, DockState.Dock);
nestedManager.Children.Add(child1);

ContentControl child2 = new ContentControl();
DockingManager.SetHeader(child2, "Tool 2");
DockingManager.SetState(child2, DockState.Document);
nestedManager.Children.Add(child2);

// Add nested manager to parent
parentDockManager.Children.Add(nestedManager);
```

## Full-Screen Mode

### TDI Full-Screen

Hide tab panel for distraction-free experience (TDI mode only):

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          UseDocumentContainer="True"
                          ContainerMode="TDI"
                          TDIFullScreenMode="WindowMode">
    <!-- TDIFullScreenMode options:
         - None: Normal mode (default)
         - WindowMode: Full window, tab panel hidden, shows on mouse-over
         - ControlMode: Normal window size, tab panel hidden, shows on mouse-over -->
</syncfusion:DockingManager>
```

**C#:**
```csharp
// Enable full-screen mode
dockManager.TDIFullScreenMode = FullScreenMode.WindowMode;

// Control mode (tabs hidden but not full-screen)
dockManager.TDIFullScreenMode = FullScreenMode.ControlMode;

// Disable
dockManager.TDIFullScreenMode = FullScreenMode.None;
```

### Toggle Full-Screen

**C#:**
```csharp
private FullScreenMode _previousMode = FullScreenMode.None;

public void ToggleFullScreen()
{
    if (dockManager.TDIFullScreenMode == FullScreenMode.None)
    {
        dockManager.TDIFullScreenMode = FullScreenMode.WindowMode;
    }
    else
    {
        dockManager.TDIFullScreenMode = FullScreenMode.None;
    }
}

// Bind to F11 key
private void Window_KeyDown(object sender, KeyEventArgs e)
{
    if (e.Key == Key.F11)
    {
        ToggleFullScreen();
        e.Handled = true;
    }
}
```

## Keyboard Navigation

DockingManager supports multiple keyboard navigation modes (set via `SwitchMode` property):

| Mode | Behavior | Default Shortcut |
|------|----------|------------------|
| **Immediate** | Switch instantly on key press | Ctrl+Tab / Ctrl+Shift+Tab |
| **List** | Show window list popup | Ctrl+Tab (hold to select) |
| **QuickTabs** | VS-style tab switcher | Ctrl+Tab |
| **VS2005** | Visual Studio 2005 navigation | - |
| **VistaFlip** | Vista-style flip 3D | - |

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager" SwitchMode="QuickTabs">
</syncfusion:DockingManager>
```

**C#:**
```csharp
// Custom keyboard navigation for Ctrl+1-9 window activation
private void Window_KeyDown(object sender, KeyEventArgs e)
{
    if (Keyboard.Modifiers == ModifierKeys.Control && e.Key >= Key.D1 && e.Key <= Key.D9)
    {
        int index = (int)e.Key - (int)Key.D1;
        var window = dockManager.Children.OfType<FrameworkElement>().Skip(index).FirstOrDefault();
        if (window != null)
            dockManager.ActiveWindow = window;
    }
}
```

## Window Activation

### Get Active Window

**C#:**
```csharp
// Get currently active window
FrameworkElement activeWindow = dockManager.ActiveWindow;

if (activeWindow != null)
{
    string header = DockingManager.GetHeader(activeWindow).ToString();
    Console.WriteLine($"Active: {header}");
}
```

### Set Active Window

**C#:**
```csharp
// Activate specific window
dockManager.ActiveWindow = myWindow;

// Or use SetNewFocusedElement for focus control
DockingManager.SetNewFocusedElement(dockManager, myWindow);
```

### Active Window Events

**C#:**
```csharp
dockManager.ActiveWindowChanged += (s, e) =>
{
    var oldWindow = e.OldValue as FrameworkElement;
    var newWindow = e.NewValue as FrameworkElement;
    
    if (oldWindow != null)
    {
        Console.WriteLine($"Deactivated: {DockingManager.GetHeader(oldWindow)}");
    }
    
    if (newWindow != null)
    {
        Console.WriteLine($"Activated: {DockingManager.GetHeader(newWindow)}");
        
        // Update UI based on active window
        UpdateToolbarsForWindow(newWindow);
    }
};
```

### Cancel Window Activation

**C#:**
```csharp
dockManager.ActiveWindowChanging += (s, e) =>
{
    var targetWindow = e.TargetElement as FrameworkElement;
    
    // Prevent activation if unsaved changes
    if (HasUnsavedChanges(dockManager.ActiveWindow))
    {
        var result = MessageBox.Show(
            "Save changes before switching?",
            "Unsaved Changes",
            MessageBoxButton.YesNoCancel
        );
        
        if (result == MessageBoxResult.Cancel)
        {
            e.Cancel = true; // Prevent activation
        }
        else if (result == MessageBoxResult.Yes)
        {
            SaveChanges(dockManager.ActiveWindow);
        }
    }
};
```

## VS2010 Dragging

### Enable VS2010-Style Drag Hints

Modern Visual Studio 2010+ drag-and-drop experience:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          IsVS2010DraggingEnabled="True">
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.IsVS2010DraggingEnabled = true;
```

**Features:**
- Enhanced drag hints with preview
- Better visual feedback
- Smoother animations
- Modern look and feel

## Dock Hints Control

### DockAbility Property

Control which dock hints are available per window:

**XAML:**
```xml
<!-- All docking allowed -->
<ContentControl syncfusion:DockingManager.Header="Unrestricted"
                syncfusion:DockingManager.DockAbility="All" />

<!-- Specific directions only -->
<ContentControl syncfusion:DockingManager.Header="Horizontal"
                syncfusion:DockingManager.DockAbility="Left, Right" />

<!-- No docking -->
<ContentControl syncfusion:DockingManager.Header="Locked"
                syncfusion:DockingManager.DockAbility="None" />
```

**DockAbility Values:** `All`, `None`, `Left`, `Right`, `Top`, `Bottom`, `Tabbed`, `DockAll`, `DocumentAll`, `Horizontal`, `Vertical` (and combinations)

### Outer Dock Ability

Control outer (edge) docking separately:

**XAML:**
```xml
<syncfusion:DockingManager UseOuterDockAbility="True">
    <ContentControl syncfusion:DockingManager.Header="Limited Outer"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.OuterDockAbility="Top, Bottom" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.UseOuterDockAbility = true;
DockingManager.SetOuterDockAbility(window, OuterDockAbility.Top | OuterDockAbility.Bottom);
```

### Dynamic Hint Control

Control hints at runtime:

**C#:**
```csharp
dockManager.PreviewDockHints += (s, e) =>
{
    // Disable specific hints based on conditions
    if (e.DraggingSource == lockedWindow)
    {
        e.DockAbility = DockAbility.None;
        e.Cancel = true;
    }
    
    // Allow document hints only for certain windows
    if (e.DraggingTarget is FrameworkElement target &&
        DockingManager.GetState(target) == DockState.Document)
    {
        e.DockAbility = DockAbility.DocumentAll;
    }
};
```

## Pattern and Practices

### Detect If Window is Hosted

Check if element is in DockingManager:

**C#:**
```csharp
public bool IsInDockingManager(FrameworkElement element)
{
    return DockingManager.GetDockingManager(element) != null;
}

// Get parent DockingManager
DockingManager parentManager = DockingManager.GetDockingManager(myWindow);
```

### Detect Window Closing

**C#:**
```csharp
dockManager.WindowClosing += (s, e) =>
{
    var header = DockingManager.GetHeader(e.TargetElement);
    Console.WriteLine($"Closing: {header}");
    
    // Confirm close
    if (MessageBox.Show($"Close {header}?", "Confirm", 
        MessageBoxButton.YesNo) == MessageBoxResult.No)
    {
        e.Cancel = true;
    }
};
```

### IsDragging Property

Detect if window is currently being dragged:

**C#:**
```csharp
bool isDragging = DockingManager.GetIsDragging(window);

// React to drag state changes
DockingManager.IsDraggingChanged += (s, e) =>
{
    if (e.NewValue)
    {
        Console.WriteLine("Drag started");
    }
    else
    {
        Console.WriteLine("Drag ended");
    }
};
```

### Host Under Mouse

Get window currently under mouse cursor:

**C#:**
```csharp
dockManager.MouseMove += (s, e) =>
{
    Point mousePos = e.GetPosition(dockManager);
    
    // Find window under mouse
    var hitElement = dockManager.InputHitTest(mousePos) as FrameworkElement;
    
    if (hitElement != null)
    {
        var parentWindow = FindParentWindow(hitElement);
        if (parentWindow != null)
        {
            string header = DockingManager.GetHeader(parentWindow).ToString();
            Console.WriteLine($"Mouse over: {header}");
        }
    }
};

private FrameworkElement FindParentWindow(DependencyObject element)
{
    while (element != null)
    {
        if (DockingManager.GetHeader(element as FrameworkElement) != null)
        {
            return element as FrameworkElement;
        }
        element = VisualTreeHelper.GetParent(element);
    }
    return null;
}
```

## Troubleshooting

### Linked Managers Not Working

❌ **Problem:** Cannot drag between DockingManagers.

✅ **Solution:**
```csharp
// Ensure bidirectional linking
Manager1.AddToTargetManagersList(Manager2);
Manager2.AddToTargetManagersList(Manager1);

// Verify managers are in separate windows
// (Cannot link managers in same window)
```

### Nested Manager Not Docking

❌ **Problem:** Nested DockingManager won't dock properly.

✅ **Check:**
```xml
<!-- Ensure nested manager has State and Side set -->
<syncfusion:DockingManager syncfusion:DockingManager.Header="Nested"
                          syncfusion:DockingManager.State="Dock"
                          syncfusion:DockingManager.SideInDockedMode="Left">
</syncfusion:DockingManager>
```

### Full-Screen Not Working

❌ **Problem:** TDIFullScreenMode has no effect.

✅ **Solution:**
```csharp
// Requires TDI mode, not MDI
dockManager.ContainerMode = DocumentContainerMode.TDI;

// Then enable full-screen
dockManager.TDIFullScreenMode = FullScreenMode.WindowMode;
```

### Keyboard Navigation Not Responding

❌ **Problem:** Ctrl+Tab doesn't switch windows.

✅ **Check:**
```csharp
// Ensure SwitchMode is not None
dockManager.SwitchMode = SwitchMode.List;

// Verify focus is on DockingManager
dockManager.Focus();

// Check if keyboard shortcuts are handled elsewhere
```

### DockAbility Not Restricting

❌ **Problem:** DockAbility setting ignored.

✅ **Solution:**
```csharp
// Ensure VS2010 dragging is enabled
dockManager.IsVS2010DraggingEnabled = true;

// Set DockAbility on the window being dragged
DockingManager.SetDockAbility(window, DockAbility.Left | DockAbility.Right);

// For outer docking, enable UseOuterDockAbility
dockManager.UseOuterDockAbility = true;
```

### Active Window Not Updating

❌ **Problem:** ActiveWindow property doesn't reflect current window.

✅ **Check:**
```csharp
// Manually set active window
dockManager.ActiveWindow = targetWindow;

// Or use SetNewFocusedElement
DockingManager.SetNewFocusedElement(dockManager, targetWindow);

// Verify window is actually in Children collection
if (!dockManager.Children.Contains(targetWindow))
{
    dockManager.Children.Add(targetWindow);
}
```
