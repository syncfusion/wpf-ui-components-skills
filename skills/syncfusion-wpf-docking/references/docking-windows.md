# Docking Windows

## Table of Contents
- [Overview](#overview)
- [Positioning Windows](#positioning-windows)
- [Target-Based Docking](#target-based-docking)
- [Maximize and Minimize](#maximize-and-minimize)
- [Hot Tracking](#hot-tracking)
- [Header Visibility](#header-visibility)
- [Custom Context Menus](#custom-context-menus)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Docking windows are child elements positioned on the four sides (Left, Right, Top, Bottom) or tabbed together with other windows. This is the most common window state in DockingManager, providing flexible layout options for tool windows and panels.

## Positioning Windows

### Setting Side Position

Use `SideInDockedMode` attached property to position windows:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager">
    <!-- Left side -->
    <ContentControl syncfusion:DockingManager.Header="Solution Explorer"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Left" />
    
    <!-- Right side -->
    <ContentControl syncfusion:DockingManager.Header="Properties"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Right" />
    
    <!-- Top side -->
    <ContentControl syncfusion:DockingManager.Header="Bookmarks"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Top" />
    
    <!-- Bottom side -->
    <ContentControl syncfusion:DockingManager.Header="Output"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Bottom" />
    
    <!-- Tabbed (same group) -->
    <ContentControl syncfusion:DockingManager.Header="Error List"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Tabbed"
                    syncfusion:DockingManager.TargetNameInDockedMode="Output" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
ContentControl solutionExplorer = new ContentControl();
DockingManager.SetHeader(solutionExplorer, "Solution Explorer");
DockingManager.SetState(solutionExplorer, DockState.Dock);
DockingManager.SetSideInDockedMode(solutionExplorer, DockSide.Left);

ContentControl properties = new ContentControl();
DockingManager.SetHeader(properties, "Properties");
DockingManager.SetState(properties, DockState.Dock);
DockingManager.SetSideInDockedMode(properties, DockSide.Right);

dockManager.Children.Add(solutionExplorer);
dockManager.Children.Add(properties);
```

### Size Configuration

Control window dimensions with `DesiredWidthInDockedMode` and `DesiredHeightInDockedMode`:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Toolbox"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left"
                syncfusion:DockingManager.DesiredWidthInDockedMode="250"
                syncfusion:DockingManager.DesiredHeightInDockedMode="400" />
```

**C#:**
```csharp
DockingManager.SetDesiredWidthInDockedMode(toolbox, 250);
DockingManager.SetDesiredHeightInDockedMode(toolbox, 400);
```

## Target-Based Docking

Group windows together using `TargetNameInDockedMode`:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager">
    <!-- Base window -->
    <ContentControl x:Name="Output"
                    syncfusion:DockingManager.Header="Output"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Bottom" />
    
    <!-- Tab with Output -->
    <ContentControl syncfusion:DockingManager.Header="Error List"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Tabbed"
                    syncfusion:DockingManager.TargetNameInDockedMode="Output" />
    
    <!-- Dock to right of Output -->
    <ContentControl syncfusion:DockingManager.Header="Find Results"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Right"
                    syncfusion:DockingManager.TargetNameInDockedMode="Output" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
ContentControl errorList = new ContentControl();
DockingManager.SetHeader(errorList, "Error List");
DockingManager.SetState(errorList, DockState.Dock);
DockingManager.SetSideInDockedMode(errorList, DockSide.Tabbed);
DockingManager.SetTargetNameInDockedMode(errorList, "Output");
```

## Maximize and Minimize

### Enable Maximize/Minimize

Allow windows to be maximized or minimized within docked state:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Server Explorer"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left"
                syncfusion:DockingManager.CanMaximize="True"
                syncfusion:DockingManager.CanMinimize="True" />
```

**C#:**
```csharp
DockingManager.SetCanMaximize(serverExplorer, true);
DockingManager.SetCanMinimize(serverExplorer, true);
```

### Maximize Mode

Control how maximize behaves:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          MaximizeMode="FillParent">
    <!-- MaximizeMode values:
         - FillParent: Fills entire DockingManager
         - FillDockArea: Fills only dock area (not over other windows) -->
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.MaximizeMode = MaximizeMode.FillParent;
```

### Size Limits

Set minimum and maximum sizes when maximizing:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Console"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Bottom"
                syncfusion:DockingManager.CanMaximize="True"
                syncfusion:DockingManager.MinimumSizeInDockedMode="100,50"
                syncfusion:DockingManager.MaximumSizeInDockedMode="800,400" />
```

**C#:**
```csharp
DockingManager.SetMinimumSizeInDockedMode(console, new Size(100, 50));
DockingManager.SetMaximumSizeInDockedMode(console, new Size(800, 400));
```

## Hot Tracking

Enable visual feedback when hovering over window headers:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          IsEnableHotTracking="True">
    <ContentControl syncfusion:DockingManager.Header="Toolbox"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Left" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.IsEnableHotTracking = true;
```

When enabled, window headers highlight on mouse hover, providing better visual feedback.

## Header Visibility

### Hide Headers

Remove headers from windows using `NoHeader` property:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Main Panel"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left"
                syncfusion:DockingManager.NoHeader="True" />
```

**C#:**
```csharp
DockingManager.SetNoHeader(mainPanel, true);
```

### Custom Header Content

Use custom content for headers:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Right">
    <syncfusion:DockingManager.Header>
        <StackPanel Orientation="Horizontal">
            <Image Source="/Images/properties_icon.png" Width="16" Height="16" />
            <TextBlock Text="Properties" Margin="5,0,0,0" />
        </StackPanel>
    </syncfusion:DockingManager.Header>
</ContentControl>
```

## Custom Context Menus

### Default Context Menu

DockingManager provides a built-in context menu with options like:
- Float
- Dock
- Auto Hide
- Hide
- Close

### Customizing Context Menu

Modify or replace the default context menu:

**XAML:**
```xml
<ContentControl x:Name="customWindow"
                syncfusion:DockingManager.Header="Custom Menu"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left">
    <syncfusion:DockingManager.DockContextMenu>
        <ContextMenu>
            <MenuItem Header="Custom Action 1" Click="CustomAction1_Click" />
            <MenuItem Header="Custom Action 2" Click="CustomAction2_Click" />
            <Separator />
            <MenuItem Header="Close" Click="Close_Click" />
        </ContextMenu>
    </syncfusion:DockingManager.DockContextMenu>
</ContentControl>
```

**C#:**
```csharp
ContextMenu customMenu = new ContextMenu();

MenuItem item1 = new MenuItem() { Header = "Custom Action 1" };
item1.Click += CustomAction1_Click;

MenuItem item2 = new MenuItem() { Header = "Custom Action 2" };
item2.Click += CustomAction2_Click;

customMenu.Items.Add(item1);
customMenu.Items.Add(item2);
customMenu.Items.Add(new Separator());

MenuItem closeItem = new MenuItem() { Header = "Close" };
closeItem.Click += Close_Click;
customMenu.Items.Add(closeItem);

DockingManager.SetDockContextMenu(customWindow, customMenu);
```

### Remove Default Menu Items

Handle `ContextMenuOpening` event to remove specific items:

**C#:**
```csharp
dockManager.ContextMenuOpening += (s, e) =>
{
    if (e.Source is FrameworkElement element)
    {
        ContextMenu menu = DockingManager.GetDockContextMenu(element);
        if (menu != null)
        {
            // Remove specific menu items
            var itemsToRemove = menu.Items.OfType<MenuItem>()
                .Where(m => m.Header.ToString() == "Float" || 
                           m.Header.ToString() == "Auto Hide")
                .ToList();
            
            foreach (var item in itemsToRemove)
            {
                menu.Items.Remove(item);
            }
        }
    }
};
```

## Common Patterns

### Visual Studio-Style Layout

```xml
<syncfusion:DockingManager x:Name="dockManager" UseDocumentContainer="True">
    <!-- Left side tools -->
    <ContentControl x:Name="SolutionExplorer"
                    syncfusion:DockingManager.Header="Solution Explorer"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Left"
                    syncfusion:DockingManager.DesiredWidthInDockedMode="300" />
    
    <ContentControl syncfusion:DockingManager.Header="Class View"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Tabbed"
                    syncfusion:DockingManager.TargetNameInDockedMode="SolutionExplorer" />
    
    <!-- Right side tools -->
    <ContentControl x:Name="Properties"
                    syncfusion:DockingManager.Header="Properties"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Right"
                    syncfusion:DockingManager.DesiredWidthInDockedMode="300" />
    
    <!-- Bottom tools -->
    <ContentControl x:Name="Output"
                    syncfusion:DockingManager.Header="Output"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Bottom"
                    syncfusion:DockingManager.DesiredHeightInDockedMode="150" />
    
    <ContentControl syncfusion:DockingManager.Header="Error List"
                    syncfusion:DockingManager.State="Dock"
                    syncfusion:DockingManager.SideInDockedMode="Tabbed"
                    syncfusion:DockingManager.TargetNameInDockedMode="Output" />
    
    <!-- Documents in center -->
    <ContentControl syncfusion:DockingManager.Header="Document1.cs"
                    syncfusion:DockingManager.State="Document" />
</syncfusion:DockingManager>
```

### Programmatic Side Change

```csharp
// Move window to different side
DockingManager.SetSideInDockedMode(toolWindow, DockSide.Right);

// Get current side
DockSide currentSide = DockingManager.GetSideInDockedMode(toolWindow);
```

### Dynamic Window Addition

```csharp
public void AddNewToolWindow(string header, DockSide side)
{
    ContentControl newWindow = new ContentControl();
    DockingManager.SetHeader(newWindow, header);
    DockingManager.SetState(newWindow, DockState.Dock);
    DockingManager.SetSideInDockedMode(newWindow, side);
    DockingManager.SetDesiredWidthInDockedMode(newWindow, 250);
    
    dockManager.Children.Add(newWindow);
}
```

## Troubleshooting

### Window Not Appearing

❌ **Problem:** Window added but not visible.

✅ **Solutions:**
```csharp
// Ensure State is set
DockingManager.SetState(window, DockState.Dock);

// Verify SideInDockedMode is set
DockingManager.SetSideInDockedMode(window, DockSide.Left);

// Check if window is added to Children collection
if (!dockManager.Children.Contains(window))
{
    dockManager.Children.Add(window);
}
```

### Tabbed Window Not Grouping

❌ **Problem:** Windows not tabbing together.

✅ **Solution:**
```xml
<!-- Ensure TargetNameInDockedMode matches x:Name of target window -->
<ContentControl x:Name="Window1"
                syncfusion:DockingManager.Header="Window 1"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left" />

<ContentControl syncfusion:DockingManager.Header="Window 2"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Tabbed"
                syncfusion:DockingManager.TargetNameInDockedMode="Window1" />
```

### Size Not Applied

❌ **Problem:** DesiredWidthInDockedMode not working.

✅ **Check:**
- Width applies to Left/Right sides, Height to Top/Bottom sides
- Splitter may override size when dragged by user
- Size is initial desired size, not absolute constraint

```csharp
// For left/right sides, set width
DockingManager.SetDesiredWidthInDockedMode(leftWindow, 300);

// For top/bottom sides, set height
DockingManager.SetDesiredHeightInDockedMode(bottomWindow, 200);
```

### Maximize Button Not Showing

❌ **Problem:** Maximize button missing.

✅ **Solution:**
```csharp
// Enable both properties
DockingManager.SetCanMaximize(window, true);

// Ensure window is in Dock state (not Float or AutoHidden)
DockingManager.SetState(window, DockState.Dock);
```

### Context Menu Not Opening

❌ **Problem:** Right-click does nothing.

✅ **Check:**
- Context menu opens on window header, not content area
- Right-click on the header bar with window title
- If custom menu set, verify it's not null

```csharp
// Verify custom menu is assigned
ContextMenu menu = DockingManager.GetDockContextMenu(window);
if (menu == null)
{
    // Create and assign menu
    ContextMenu newMenu = new ContextMenu();
    // ... add items
    DockingManager.SetDockContextMenu(window, newMenu);
}
```
