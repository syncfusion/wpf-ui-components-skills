# Window States in DockingManager

## Table of Contents
- [Overview](#overview)
- [Dock State](#dock-state)
- [Float State](#float-state)
- [AutoHidden State](#autohidden-state)
- [Document State](#document-state)
- [Changing States](#changing-states)
- [State Transitions](#state-transitions)

## Overview

DockingManager manages child windows in four distinct states, each serving different UI purposes:

| State | Description | Use Case |
|-------|-------------|----------|
| **Dock** | Windows anchored to container edges | Tool panels, side bars |
| **Float** | Independent floating windows | Secondary views, detached panels |
| **AutoHidden** | Collapsed to edges, expand on hover | Infrequently used tools |
| **Document** | Central document area (MDI/TDI) | Main content, editors |

The `State` property (or `SetState` method) controls which state a window occupies.

## Dock State

The default state where windows are docked to the sides of the container.

### Basic Dock Configuration

```xaml
<syncfusion:DockingManager x:Name="dockingManager">
    <!-- Dock is the default state - no State property needed -->
    <ContentControl syncfusion:DockingManager.Header="Docked Window"/>
</syncfusion:DockingManager>
```

```csharp
// Explicitly set Dock state
DockingManager.SetState(window, DockState.Dock);
```

### Positioning Docked Windows

Use `SideInDockedMode` to specify where windows dock:

```xaml
<syncfusion:DockingManager>
    <ContentControl syncfusion:DockingManager.Header="Left Panel"
                    syncfusion:DockingManager.SideInDockedMode="Left"/>
    
    <ContentControl syncfusion:DockingManager.Header="Right Panel"
                    syncfusion:DockingManager.SideInDockedMode="Right"/>
    
    <ContentControl syncfusion:DockingManager.Header="Top Panel"
                    syncfusion:DockingManager.SideInDockedMode="Top"/>
    
    <ContentControl syncfusion:DockingManager.Header="Bottom Panel"
                    syncfusion:DockingManager.SideInDockedMode="Bottom"/>
</syncfusion:DockingManager>
```

**Available Dock Sides:**
- `Left` (default)
- `Right`
- `Top`
- `Bottom`
- `Tabbed` (creates tabs with target window)

### Controlling Dock Ability

Prevent specific windows from being docked:

```xaml
<ContentControl syncfusion:DockingManager.Header="Float Only"
                syncfusion:DockingManager.CanDock="False"
                syncfusion:DockingManager.State="Float"/>
```

```csharp
DockingManager.SetCanDock(window, false);
```

## Float State

Windows become independent floating windows, similar to dialog boxes.

### Basic Float Configuration

```xaml
<syncfusion:DockingManager>
    <ContentControl syncfusion:DockingManager.Header="Floating Window"
                    syncfusion:DockingManager.State="Float"/>
</syncfusion:DockingManager>
```

```csharp
DockingManager.SetState(window, DockState.Float);
```

### Native Float Windows

Use native WPF windows for better multi-monitor support and resizing:

```xaml
<syncfusion:DockingManager UseNativeFloatWindow="True">
    <ContentControl syncfusion:DockingManager.Header="Native Float"
                    syncfusion:DockingManager.State="Float"/>
</syncfusion:DockingManager>
```

```csharp
dockingManager.UseNativeFloatWindow = true;
```

**Benefits of Native Float Windows:**
- Better multi-monitor support
- Improved resizing behavior
- Can appear in taskbar
- Better performance across displays

### Controlling Float Ability

Prevent windows from floating:

```xaml
<ContentControl syncfusion:DockingManager.Header="Dock Only"
                syncfusion:DockingManager.CanFloat="False"/>
```

```csharp
DockingManager.SetCanFloat(window, false);
```

## AutoHidden State

Windows collapse to the container edge and expand on hover/click, similar to Visual Studio's auto-hide panels. This state saves screen space while keeping frequently accessed tools within quick reach.

**Key Features:**
- Saves screen space by collapsing to edge tabs
- Quick access on hover or click
- Customizable animation (fade, scale, slide)
- Per-edge panel placement
- Scrollable side panels for many tabs
- Drag and reposition between edges

### Basic AutoHidden Configuration

```xaml
<syncfusion:DockingManager>
    <ContentControl syncfusion:DockingManager.Header="Auto-Hidden"
                    syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

```csharp
DockingManager.SetState(window, DockState.AutoHidden);
```

### Positioning AutoHidden Windows

Specify which edge the collapsed window appears on:

```xaml
<syncfusion:DockingManager>
    <!-- Auto-hide on left edge -->
    <ContentControl syncfusion:DockingManager.Header="Left Auto"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Left"/>
    
    <!-- Auto-hide on right edge -->
    <ContentControl syncfusion:DockingManager.Header="Right Auto"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Right"/>
    
    <!-- Auto-hide on top edge -->
    <ContentControl syncfusion:DockingManager.Header="Top Auto"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Top"/>
    
    <!-- Auto-hide on bottom edge -->
    <ContentControl syncfusion:DockingManager.Header="Bottom Auto"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Bottom"/>
</syncfusion:DockingManager>
```

### Target-Based Auto-Hide Positioning

Auto-hidden windows can inherit the side from their target docked window:

```xaml
<syncfusion:DockingManager>
    <!-- Target window docked on left -->
    <ContentControl x:Name="SolutionExplorer"
                    syncfusion:DockingManager.Header="Solution Explorer"
                    syncfusion:DockingManager.SideInDockedMode="Left"/>
    
    <!-- This window auto-hides on left (inherited from target) -->
    <ContentControl syncfusion:DockingManager.Header="Output"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.TargetNameInDockedMode="SolutionExplorer"/>
</syncfusion:DockingManager>
```

### Animation Modes

Control how auto-hidden windows animate when expanding/collapsing:

```xaml
<!-- Fade animation -->
<syncfusion:DockingManager AutoHideAnimationMode="Fade">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>

<!-- Scale animation -->
<syncfusion:DockingManager AutoHideAnimationMode="Scale">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>

<!-- Slide animation (default) -->
<syncfusion:DockingManager AutoHideAnimationMode="Slide">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

```csharp
dockingManager.AutoHideAnimationMode = AutoHideAnimationMode.Fade;
```

### Animation Duration

Control expansion/collapse speed:

```xaml
<ContentControl syncfusion:DockingManager.Header="Toolbox"
                syncfusion:DockingManager.State="AutoHidden"
                syncfusion:DockingManager.AnimationDelay="200"/>
```

```csharp
DockingManager.SetAnimationDelay(toolbox, new Duration(TimeSpan.FromMilliseconds(200)));
```

### Mouse-Over Behavior

#### Hover Expansion (Default)

```xaml
<!-- Expand on hover (default) -->
<syncfusion:DockingManager IsAnimationEnabledOnMouseOver="True">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

#### Click-Only Expansion

```xaml
<!-- Require click to expand -->
<syncfusion:DockingManager IsAnimationEnabledOnMouseOver="False">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

#### Visual Studio 2013 Behavior

Click to open, click again to close:

```xaml
<syncfusion:DockingManager IsAnimationEnabledOnMouseOver="False"
                           IsVS2013SidePanelEnable="True">
```

### Side Panel Customization

#### Side Panel Appearance

```xaml
<syncfusion:DockingManager SidePanelBackground="LightGray"
                           SidePanelBorderBrush="DarkGray"
                           SidePanelBorderThickness="2"
                           SidePanelSize="40">
```

```csharp
dockingManager.SidePanelBackground = new SolidColorBrush(Colors.LightGray);
dockingManager.SidePanelSize = 40;
```

#### Per-Tab Customization

```xaml
<ContentControl syncfusion:DockingManager.Header="Toolbox"
                syncfusion:DockingManager.State="AutoHidden"
                syncfusion:DockingManager.SideTabItemBackground="Blue"
                syncfusion:DockingManager.SideTabItemForeground="White"/>
```

#### Scrollable Side Panel

Enable scrolling when tabs exceed panel width:

```xaml
<syncfusion:DockingManager EnableScrollableSidePanel="True">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.Header="Tab 1"/>
    <!-- Many more tabs... -->
</syncfusion:DockingManager>
```

### Auto-Hide Grouping Modes

Control whether individual tabs or entire groups auto-hide:

```xaml
<!-- Only active tab auto-hides -->
<syncfusion:DockingManager AutoHideTabsMode="AutoHideActive">
</syncfusion:DockingManager>

<!-- All tabs in group auto-hide (default) -->
<syncfusion:DockingManager AutoHideTabsMode="AutoHideGroup">
</syncfusion:DockingManager>
```

### Controlling AutoHide Ability

#### Per-Window Control

```xaml
<!-- Prevent auto-hide for this window -->
<ContentControl syncfusion:DockingManager.Header="No AutoHide"
                syncfusion:DockingManager.CanAutoHide="False"/>

<!-- Allow auto-hide (default) -->
<ContentControl syncfusion:DockingManager.Header="Can AutoHide"
                syncfusion:DockingManager.CanAutoHide="True"/>
```

```csharp
DockingManager.SetCanAutoHide(window, false);
```

#### Global Control

```xaml
<!-- Hide pin button for all windows -->
<syncfusion:DockingManager AutoHideVisibility="False">
```

#### Dragging Auto-Hidden Windows

```xaml
<!-- Allow dragging between edges -->
<ContentControl syncfusion:DockingManager.Header="Draggable"
                syncfusion:DockingManager.State="AutoHidden"
                syncfusion:DockingManager.CanDragAutoHidden="True"/>

<!-- Prevent dragging -->
<ContentControl syncfusion:DockingManager.Header="Fixed"
                syncfusion:DockingManager.State="AutoHidden"
                syncfusion:DockingManager.CanDragAutoHidden="False"/>
```

### Programmatic Operations

```csharp
// Auto-hide all currently docked windows
dockingManager.AutoHideAllDockWindow();

// Convert all auto-hidden windows back to docked
dockingManager.UnPinAllAutoHide();

// Get auto-hidden window height
double height = DockingManager.GetAutoHideHeight(toolbox);
```

## Document State

Windows appear in a central document area using MDI or TDI mode.

### Enabling Document State

**IMPORTANT:** Must set `UseDocumentContainer="True"` first.

```xaml
<syncfusion:DockingManager UseDocumentContainer="True">
    <ContentControl syncfusion:DockingManager.Header="Document 1"
                    syncfusion:DockingManager.State="Document"/>
    
    <ContentControl syncfusion:DockingManager.Header="Document 2"
                    syncfusion:DockingManager.State="Document"/>
</syncfusion:DockingManager>
```

```csharp
// Enable document container
dockingManager.UseDocumentContainer = true;

// Set document state
DockingManager.SetState(document1, DockState.Document);
DockingManager.SetState(document2, DockState.Document);
```

### MDI vs TDI Mode

Choose between Multiple Document Interface (MDI) or Tabbed Document Interface (TDI):

```xaml
<!-- TDI Mode (default) - Tabbed documents -->
<syncfusion:DockingManager UseDocumentContainer="True" 
                           ContainerMode="TDI">
    <ContentControl syncfusion:DockingManager.State="Document"/>
</syncfusion:DockingManager>

<!-- MDI Mode - Multiple overlapping windows -->
<syncfusion:DockingManager UseDocumentContainer="True" 
                           ContainerMode="MDI">
    <ContentControl syncfusion:DockingManager.State="Document"/>
</syncfusion:DockingManager>
```

### Controlling Document Ability

Prevent windows from becoming documents:

```xaml
<ContentControl syncfusion:DockingManager.Header="No Document"
                syncfusion:DockingManager.CanDocument="False"/>
```

```csharp
DockingManager.SetCanDocument(window, false);
```

## Changing States

### Programmatically Change State

```csharp
// Get current state
DockState currentState = DockingManager.GetState(window);

// Change to different states
DockingManager.SetState(window, DockState.Dock);
DockingManager.SetState(window, DockState.Float);
DockingManager.SetState(window, DockState.AutoHidden);
DockingManager.SetState(window, DockState.Document);
```

### User-Initiated State Changes

Users can change states through:
- **Drag and drop**: Drag window headers to dock/float
- **Double-click header**: Float ↔ Dock (configurable)
- **Pin button**: Dock ↔ AutoHidden
- **Context menu**: Right-click header for state options

### Disabling State Transitions

Control which state changes are allowed:

```xaml
<ContentControl syncfusion:DockingManager.Header="Limited States"
                syncfusion:DockingManager.CanFloat="False"
                syncfusion:DockingManager.CanAutoHide="False"
                syncfusion:DockingManager.CanDocument="False"/>
```

This window can only be docked.

## State Transitions

### Double-Click Behavior

Configure what happens when user double-clicks a docked window header:

```xaml
<!-- Default: Dock ↔ Float -->
<syncfusion:DockingManager DoubleClickAction="DockOrFloat">
```

```xaml
<!-- Alternative: Maximize ↔ Restore (for native float windows only) -->
<syncfusion:DockingManager UseNativeFloatWindow="True"
                           DoubleClickAction="MaximizeOrRestore">
```

### Restricting Double-Click Dock

Prevent float windows from docking on double-click:

```xaml
<ContentControl syncfusion:DockingManager.Header="Float Only"
                syncfusion:DockingManager.State="Float"
                syncfusion:DockingManager.NoDock="True"/>
```

```csharp
DockingManager.SetNoDock(window, true);
```

### Context Menu State Options

Customize which state options appear in context menus:

```xaml
<ContentControl syncfusion:DockingManager.Header="Custom Menu"
                syncfusion:DockingManager.ShowDockableMenuItem="True"
                syncfusion:DockingManager.ShowFloatingMenuItem="True"
                syncfusion:DockingManager.ShowAutoHideMenuItem="False"/>
```

## State Detection and Events

### Detecting Current State

```csharp
DockState state = DockingManager.GetState(window);

if (state == DockState.Dock)
{
    // Window is docked
}
else if (state == DockState.Float)
{
    // Window is floating
}
else if (state == DockState.AutoHidden)
{
    // Window is auto-hidden
}
else if (state == DockState.Document)
{
    // Window is a document
}
```

### State Change Events

React to state changes:

```csharp
dockingManager.DockStateChanged += (sender, e) =>
{
    string header = DockingManager.GetHeader(e.TargetItem).ToString();
    DockState oldState = e.OldState;
    DockState newState = e.NewState;
    
    MessageBox.Show($"{header} changed from {oldState} to {newState}");
};
```

### Preventing State Changes

Use `DockStateChanging` event to cancel state changes:

```csharp
dockingManager.DockStateChanging += (sender, e) =>
{
    // Prevent this specific window from floating
    if (DockingManager.GetHeader(e.TargetItem).ToString() == "Solution Explorer" &&
        e.NewState == DockState.Float)
    {
        e.Cancel = true;
    }
};
```

## Common State Patterns

**IDE Layout (Visual Studio-style):**
```xaml
<syncfusion:DockingManager UseDocumentContainer="True">
    <ContentControl Header="Solution Explorer" SideInDockedMode="Right"/>
    <ContentControl Header="Toolbox" State="AutoHidden" SideInDockedMode="Left"/>
    <ContentControl Header="Document" State="Document"/>
</syncfusion:DockingManager>
```

Other patterns: Dashboard with floating panels (use `State="Float"`), fixed layouts (disable `CanFloat`, `CanAutoHide`, `CanClose`), dual-monitor setups (enable `UseNativeFloatWindow="True"`).

## Troubleshooting

### Document State Not Working

**Problem:** Document state has no effect.

**Solution:** Enable UseDocumentContainer:
```xaml
<syncfusion:DockingManager UseDocumentContainer="True">
```

### Float Windows Don't Show in Taskbar

**Problem:** Floating windows missing from taskbar on multi-monitor setups.

**Solution:** Enable native float windows:
```xaml
<syncfusion:DockingManager UseNativeFloatWindow="True"
                           ShowFloatWindowInTaskbar="True">
```

### AutoHidden Window Doesn't Expand

**Problem:** Hovering over auto-hidden tab doesn't expand window.

**Solution:** Check `IsAnimationEnabledOnMouseOver`:
```xaml
<!-- Ensure mouse-over animation is enabled (default=true) -->
<syncfusion:DockingManager IsAnimationEnabledOnMouseOver="True">
```

### State Changes Ignored

**Problem:** SetState calls don't change window state.

**Solution:** Ensure ability flags are not disabled:
```csharp
// Check and enable abilities
DockingManager.SetCanDock(window, true);
DockingManager.SetCanFloat(window, true);
DockingManager.SetCanAutoHide(window, true);
DockingManager.SetCanDocument(window, true);
```
