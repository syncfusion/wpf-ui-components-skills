# Auto-Hide Windows in DockingManager

Auto-hide windows collapse to the container edge and expand on hover or click, saving screen space for infrequently used panels.

## Overview

Auto-hidden windows are displayed as tabs on the edges of the DockingManager. When users hover over or click these tabs, the window temporarily expands. This behavior is similar to Visual Studio's auto-hide panels.

**Key Features:**
- Saves screen space
- Quick access on hover/click
- Customizable animation
- Side panel placement control
- Drag restrictions available

## Basic Configuration

### Setting Auto-Hide State

```xaml
<syncfusion:DockingManager x:Name="dockingManager">
    <ContentControl syncfusion:DockingManager.Header="Toolbox"
                    syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

```csharp
DockingManager.SetState(toolbox, DockState.AutoHidden);
```

## Positioning Auto-Hidden Windows

Control which edge the collapsed tab appears on using `SideInDockedMode`:

```xaml
<syncfusion:DockingManager>
    <!-- Left edge -->
    <ContentControl syncfusion:DockingManager.Header="Toolbox"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Left"/>
    
    <!-- Right edge -->
    <ContentControl syncfusion:DockingManager.Header="Properties"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Right"/>
    
    <!-- Top edge -->
    <ContentControl syncfusion:DockingManager.Header="Search"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Top"/>
    
    <!-- Bottom edge -->
    <ContentControl syncfusion:DockingManager.Header="Output"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Bottom"/>
</syncfusion:DockingManager>
```

### Target-Based Auto-Hide Positioning

Auto-hidden windows inherit the side from their target window:

```xaml
<syncfusion:DockingManager>
    <!-- Target window docked on left -->
    <ContentControl x:Name="SolutionExplorer"
                    syncfusion:DockingManager.Header="Solution Explorer"
                    syncfusion:DockingManager.SideInDockedMode="Left"/>
    
    <!-- This window auto-hides on left (inherited from target) -->
    <ContentControl x:Name="Output"
                    syncfusion:DockingManager.Header="Output"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.TargetNameInDockedMode="SolutionExplorer"
                    syncfusion:DockingManager.SideInDockedMode="Bottom"/>
</syncfusion:DockingManager>
```

The Output window will auto-hide on the left edge because SolutionExplorer is on the left.

## Animation Modes

DockingManager supports three animation styles:

### Fade Animation

```xaml
<syncfusion:DockingManager AutoHideAnimationMode="Fade">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

### Scale Animation

```xaml
<syncfusion:DockingManager AutoHideAnimationMode="Scale">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

### Slide Animation (Default)

```xaml
<syncfusion:DockingManager AutoHideAnimationMode="Slide">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

```csharp
dockingManager.AutoHideAnimationMode = AutoHideAnimationMode.Fade;
// or Scale, Slide
```

## Animation Duration

Control how fast windows expand/collapse:

```xaml
<ContentControl syncfusion:DockingManager.Header="Toolbox"
                syncfusion:DockingManager.State="AutoHidden"
                syncfusion:DockingManager.AnimationDelay="100"/>
```

```csharp
// Fast animation (100ms)
DockingManager.SetAnimationDelay(toolbox, new Duration(TimeSpan.FromMilliseconds(100)));

// Slow animation (500ms)
DockingManager.SetAnimationDelay(toolbox, new Duration(TimeSpan.FromMilliseconds(500)));
```

## Mouse-Over Behavior

### Enable/Disable Hover Expansion

By default, auto-hidden windows expand on hover. Disable to require click:

```xaml
<!-- Require click to expand (no hover) -->
<syncfusion:DockingManager IsAnimationEnabledOnMouseOver="False">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

```csharp
dockingManager.IsAnimationEnabledOnMouseOver = false;
```

### Visual Studio 2013 Behavior

VS2013-style: click to open, click again to close:

```xaml
<syncfusion:DockingManager IsAnimationEnabledOnMouseOver="False"
                           IsVS2013SidePanelEnable="True">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

```csharp
dockingManager.IsAnimationEnabledOnMouseOver = false;
dockingManager.IsVS2013SidePanelEnable = true;
```

## Side Panel Customization

### Side Panel Appearance

```xaml
<syncfusion:DockingManager SidePanelBackground="LightGray"
                           SidePanelBorderBrush="DarkGray"
                           SidePanelBorderThickness="2"
                           SidePanelSize="40">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

```csharp
dockingManager.SidePanelBackground = new SolidColorBrush(Colors.LightGray);
dockingManager.SidePanelBorderBrush = new SolidColorBrush(Colors.DarkGray);
dockingManager.SidePanelBorderThickness = new Thickness(2);
dockingManager.SidePanelSize = 40;
```

### Individual Tab Customization

```xaml
<ContentControl syncfusion:DockingManager.Header="Toolbox"
                syncfusion:DockingManager.State="AutoHidden"
                syncfusion:DockingManager.SideTabItemBackground="Blue"
                syncfusion:DockingManager.SideTabItemForeground="White"/>
```

```csharp
DockingManager.SetSideTabItemBackground(toolbox, new SolidColorBrush(Colors.Blue));
DockingManager.SetSideTabItemForeground(toolbox, new SolidColorBrush(Colors.White));
```

## Scrollable Side Panel

Enable scrolling when too many tabs overflow:

```xaml
<syncfusion:DockingManager EnableScrollableSidePanel="True">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.Header="Tab 1"/>
    <ContentControl syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.Header="Tab 2"/>
    <!-- Many more tabs... -->
</syncfusion:DockingManager>
```

```csharp
dockingManager.EnableScrollableSidePanel = true;
```

## Auto-Hide Mode

Control whether single tabs or tab groups auto-hide:

### Auto-Hide Active Only

```xaml
<syncfusion:DockingManager AutoHideTabsMode="AutoHideActive">
    <!-- Only active tab in group auto-hides -->
</syncfusion:DockingManager>
```

### Auto-Hide Entire Group (Default)

```xaml
<syncfusion:DockingManager AutoHideTabsMode="AutoHideGroup">
    <!-- All tabs in group auto-hide together -->
</syncfusion:DockingManager>
```

## Controlling Auto-Hide Ability

### Globally Disable Auto-Hide

Hide pin button for all windows:

```xaml
<syncfusion:DockingManager AutoHideVisibility="False">
    <!-- No windows can auto-hide -->
</syncfusion:DockingManager>
```

### Per-Window Control

```xaml
<!-- This window cannot auto-hide -->
<ContentControl syncfusion:DockingManager.Header="Solution Explorer"
                syncfusion:DockingManager.CanAutoHide="False"/>

<!-- This window can auto-hide -->
<ContentControl syncfusion:DockingManager.Header="Toolbox"
                syncfusion:DockingManager.CanAutoHide="True"/>
```

```csharp
DockingManager.SetCanAutoHide(solutionExplorer, false);
DockingManager.SetCanAutoHide(toolbox, true);
```

## Dragging Auto-Hidden Windows

### Enable/Disable Dragging

```xaml
<!-- Allow dragging -->
<ContentControl syncfusion:DockingManager.Header="Toolbox"
                syncfusion:DockingManager.State="AutoHidden"
                syncfusion:DockingManager.CanDragAutoHidden="True"/>

<!-- Prevent dragging -->
<ContentControl syncfusion:DockingManager.Header="Properties"
                syncfusion:DockingManager.State="AutoHidden"
                syncfusion:DockingManager.CanDragAutoHidden="False"/>
```

```csharp
DockingManager.SetCanDragAutoHidden(toolbox, true);
DockingManager.SetCanDragAutoHidden(properties, false);
```

## Programmatic Operations

### Auto-Hide All Docked Windows

```csharp
// Auto-hide all currently docked windows
dockingManager.AutoHideAllDockWindow();
```

### Unpin All Auto-Hidden Windows

```csharp
// Convert all auto-hidden windows back to docked
dockingManager.UnPinAllAutoHide();
```

### Retrieve Auto-Hidden Window Height

```csharp
// Get height of specific auto-hidden window
double height = DockingManager.GetAutoHideHeight(toolbox);
```

## Common Patterns

### IDE Toolbox Pattern

```xaml
<syncfusion:DockingManager IsAnimationEnabledOnMouseOver="True"
                           AutoHideAnimationMode="Slide">
    <ContentControl syncfusion:DockingManager.Header="Toolbox"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Left"
                    syncfusion:DockingManager.AnimationDelay="200">
        <!-- Toolbox content -->
    </ContentControl>
</syncfusion:DockingManager>
```

### Quick Access Panel (Click to Toggle)

```xaml
<syncfusion:DockingManager IsAnimationEnabledOnMouseOver="False"
                           IsVS2013SidePanelEnable="True"
                           AutoHideAnimationMode="Fade">
    <ContentControl syncfusion:DockingManager.State="AutoHidden"/>
</syncfusion:DockingManager>
```

### Multiple Auto-Hidden Panels

```xaml
<syncfusion:DockingManager EnableScrollableSidePanel="True">
    <!-- Left side -->
    <ContentControl syncfusion:DockingManager.Header="Toolbox"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Left"/>
    
    <ContentControl syncfusion:DockingManager.Header="Server Explorer"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Left"/>
    
    <!-- Right side -->
    <ContentControl syncfusion:DockingManager.Header="Properties"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Right"/>
    
    <!-- Bottom side -->
    <ContentControl syncfusion:DockingManager.Header="Output"
                    syncfusion:DockingManager.State="AutoHidden"
                    syncfusion:DockingManager.SideInDockedMode="Bottom"/>
</syncfusion:DockingManager>
```

## Context Menu Integration

Auto-hidden windows support context menu state changes:

```csharp
// Context menu items enabled/disabled based on:
// - CanDock property
// - CanFloat property
// - CanDocument property (if UseDocumentContainer=true)
```

Users can right-click auto-hidden tabs to change states.

## Troubleshooting

### Auto-Hidden Window Doesn't Expand on Hover

**Problem:** Hovering over tab doesn't expand window.

**Solution:** Check `IsAnimationEnabledOnMouseOver`:
```csharp
dockingManager.IsAnimationEnabledOnMouseOver = true;
```

### Pin Button Not Visible

**Problem:** Can't auto-hide docked windows.

**Solution:** Enable auto-hide visibility:
```csharp
dockingManager.AutoHideVisibility = true;
DockingManager.SetCanAutoHide(window, true);
```

### Side Panel Too Narrow/Wide

**Problem:** Tab text cut off or too much space.

**Solution:** Adjust side panel size:
```csharp
dockingManager.SidePanelSize = 35; // Adjust as needed
```

### Animation Too Fast/Slow

**Problem:** Expansion feels abrupt or sluggish.

**Solution:** Tune animation delay:
```csharp
DockingManager.SetAnimationDelay(window, new Duration(TimeSpan.FromMilliseconds(250)));
```
