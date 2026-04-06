# Floating Windows

## Table of Contents
- [Overview](#overview)
- [Creating Float Windows](#creating-float-windows)
- [Native vs Popup Windows](#native-vs-popup-windows)
- [Taskbar Integration](#taskbar-integration)
- [Positioning and Sizing](#positioning-and-sizing)
- [Maximize Behavior](#maximize-behavior)
- [Snapping and Docking](#snapping-and-docking)
- [Rolling Up Windows](#rolling-up-windows)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Float windows are child elements that appear in separate movable windows outside the main DockingManager. They can exist anywhere on screen, support multi-monitor scenarios, and optionally appear in the Windows taskbar.

## Creating Float Windows

### Basic Float Window

Set `State` to `Float`:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager">
    <ContentControl syncfusion:DockingManager.Header="Floating Tool"
                    syncfusion:DockingManager.State="Float"
                    syncfusion:DockingManager.FloatingWindowRect="100,100,400,300" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
ContentControl floatWindow = new ContentControl();
DockingManager.SetHeader(floatWindow, "Floating Tool");
DockingManager.SetState(floatWindow, DockState.Float);
DockingManager.SetFloatingWindowRect(floatWindow, new Rect(100, 100, 400, 300));

dockManager.Children.Add(floatWindow);
```

### Convert Window to Float State

Programmatically change existing window to float:

**C#:**
```csharp
// Float a docked window
DockingManager.SetState(dockedWindow, DockState.Float);

// Set position and size
Rect floatRect = new Rect(200, 150, 500, 400);
DockingManager.SetFloatingWindowRect(dockedWindow, floatRect);
```

### Control Float Ability

Restrict whether windows can be floated:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Non-Floatable"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left"
                syncfusion:DockingManager.CanFloat="False" />
```

**C#:**
```csharp
DockingManager.SetCanFloat(window, false);
```

## Native vs Popup Windows

### Native Float Windows (Default)

Native windows are true WPF Windows with full OS integration:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          UseNativeFloatWindow="True">
    <ContentControl syncfusion:DockingManager.Header="Native Float"
                    syncfusion:DockingManager.State="Float" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.UseNativeFloatWindow = true;
```

**Benefits:**
- Appears in Windows taskbar (when enabled)
- Supports maximize/minimize
- Better multi-monitor support
- Full window chrome (title bar, borders)

### Popup Float Windows

Popup-based float windows are lightweight:

**C#:**
```csharp
dockManager.UseNativeFloatWindow = false;
```

**Benefits:**
- Lightweight (no separate Window instance)
- Always on top of parent window
- Faster creation

**Limitations:**
- No taskbar integration
- No maximize/minimize
- Limited multi-monitor support

## Taskbar Integration

### Enable Taskbar Buttons

Show float windows in Windows taskbar:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          UseNativeFloatWindow="True"
                          ShowInTaskbar="True">
    <ContentControl syncfusion:DockingManager.Header="Taskbar Window"
                    syncfusion:DockingManager.State="Float" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.UseNativeFloatWindow = true;
dockManager.ShowInTaskbar = true;
```

**Notes:**
- Requires `UseNativeFloatWindow="True"`
- Each float window gets separate taskbar button
- Users can Alt+Tab between windows

### Per-Window Taskbar Control

Control taskbar visibility per window:

**C#:**
```csharp
// Get native float window
var floatWindow = DockingManager.GetFloatWindow(childWindow);
if (floatWindow != null && floatWindow is Window nativeWindow)
{
    nativeWindow.ShowInTaskbar = true;
}
```

## Positioning and Sizing

### Set Float Window Position

Use `FloatingWindowRect` to set position and size:

**XAML:**
```xml
<!-- FloatingWindowRect: X,Y,Width,Height -->
<ContentControl syncfusion:DockingManager.Header="Positioned Float"
                syncfusion:DockingManager.State="Float"
                syncfusion:DockingManager.FloatingWindowRect="300,200,600,450" />
```

**C#:**
```csharp
// Position at (300, 200) with size 600x450
Rect position = new Rect(300, 200, 600, 450);
DockingManager.SetFloatingWindowRect(window, position);

// Get current position
Rect currentRect = DockingManager.GetFloatingWindowRect(window);
Console.WriteLine($"Position: {currentRect.X}, {currentRect.Y}");
Console.WriteLine($"Size: {currentRect.Width}x{currentRect.Height}");
```

### Dynamic Positioning

Position relative to screen or other windows:

**C#:**
```csharp
// Center on screen
double screenWidth = SystemParameters.PrimaryScreenWidth;
double screenHeight = SystemParameters.PrimaryScreenHeight;
double windowWidth = 500;
double windowHeight = 400;

Rect centered = new Rect(
    (screenWidth - windowWidth) / 2,
    (screenHeight - windowHeight) / 2,
    windowWidth,
    windowHeight
);

DockingManager.SetFloatingWindowRect(window, centered);
```

### Multi-Monitor Support

Position on specific monitor:

**C#:**
```csharp
// Get secondary screen bounds
System.Windows.Forms.Screen secondaryScreen = 
    System.Windows.Forms.Screen.AllScreens.FirstOrDefault(s => !s.Primary);

if (secondaryScreen != null)
{
    var bounds = secondaryScreen.Bounds;
    Rect floatRect = new Rect(
        bounds.X + 100,
        bounds.Y + 100,
        500,
        400
    );
    
    DockingManager.SetFloatingWindowRect(window, floatRect);
    DockingManager.SetState(window, DockState.Float);
}
```

## Maximize Behavior

### Enable Maximize

Allow float windows to be maximized:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Maximizable Float"
                syncfusion:DockingManager.State="Float"
                syncfusion:DockingManager.CanMaximize="True"
                syncfusion:DockingManager.FloatingWindowRect="200,150,500,400" />
```

**C#:**
```csharp
DockingManager.SetCanMaximize(window, true);
DockingManager.SetState(window, DockState.Float);
```

**Requirements:**
- Must use `UseNativeFloatWindow="True"`
- Maximize button appears in float window title bar

### Double-Click Title Bar Behavior

Configure what happens on double-click:

**C#:**
```csharp
dockManager.FloatWindowDoubleClickAction = FloatWindowDoubleClickAction.Maximize;
// Options: None, Maximize, Dock
```

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          FloatWindowDoubleClickAction="Maximize">
</syncfusion:DockingManager>
```

## Snapping and Docking

### Drag and Drop to Dock

Users can drag float windows back to docked positions:

1. Drag float window over DockingManager
2. Dock hints appear showing available positions
3. Drop on hint to dock window

### Disable Float Window Resize

Prevent users from resizing float windows:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Fixed Size Float"
                syncfusion:DockingManager.State="Float"
                syncfusion:DockingManager.FloatingWindowRect="100,100,400,300"
                syncfusion:DockingManager.CanResizeInFloatingState="False" />
```

**C#:**
```csharp
DockingManager.SetCanResizeInFloatingState(window, false);
```

### Programmatic Docking

Convert float window back to docked:

**C#:**
```csharp
// Change state from Float to Dock
DockingManager.SetState(floatWindow, DockState.Dock);
DockingManager.SetSideInDockedMode(floatWindow, DockSide.Left);
```

## Rolling Up Windows

Roll up float windows to show only title bar (minimize vertically):

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          AllowsRollUp="True">
    <ContentControl syncfusion:DockingManager.Header="Rollup Float"
                    syncfusion:DockingManager.State="Float"
                    syncfusion:DockingManager.IsRolledUp="False" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
// Enable roll-up feature globally
dockManager.AllowsRollUp = true;

// Roll up specific window programmatically
DockingManager.SetIsRolledUp(window, true);

// Expand back
DockingManager.SetIsRolledUp(window, false);
```

**Usage:**
- Double-click title bar to roll up/expand (if configured)
- Rolled-up windows show only title bar
- Click again to restore

## Common Patterns

**Dynamic Float Creation:** Create floating windows programmatically using `DockState.Float` and `SetFloatingWindowRect()` to position near cursor.

**Position Persistence:** Save float window positions (via `GetFloatingWindowRect()`) to application settings and restore on load.

**Float Window Events:** Listen to `WindowStateChanged` event to detect when windows become floating; access native `Window` object via `GetFloatWindow()`.

## Troubleshooting

### Float Window Not Appearing

❌ **Problem:** Window set to Float state but not visible.

✅ **Solutions:**
```csharp
// Ensure FloatingWindowRect is set and on-screen
Rect rect = DockingManager.GetFloatingWindowRect(window);
if (rect.IsEmpty || rect.Width == 0 || rect.Height == 0)
{
    // Set valid rect
    DockingManager.SetFloatingWindowRect(window, new Rect(100, 100, 400, 300));
}

// Verify state is set
DockingManager.SetState(window, DockState.Float);

// Check if window is off-screen (multi-monitor issue)
if (rect.X < SystemParameters.VirtualScreenLeft ||
    rect.Y < SystemParameters.VirtualScreenTop)
{
    // Reposition to primary screen
    DockingManager.SetFloatingWindowRect(window, new Rect(100, 100, rect.Width, rect.Height));
}
```

### Taskbar Button Not Showing

❌ **Problem:** Float window doesn't appear in taskbar.

✅ **Check:**
```csharp
// Ensure UseNativeFloatWindow is enabled
if (!dockManager.UseNativeFloatWindow)
{
    dockManager.UseNativeFloatWindow = true;
}

// Enable ShowInTaskbar
dockManager.ShowInTaskbar = true;

// For existing float windows, recreate them
DockingManager.SetState(window, DockState.Dock);
DockingManager.SetState(window, DockState.Float);
```

### Maximize Button Missing

❌ **Problem:** Can't maximize float window.

✅ **Solution:**
```csharp
// Use native float windows
dockManager.UseNativeFloatWindow = true;

// Enable maximize
DockingManager.SetCanMaximize(window, true);

// Verify window is in Float state
if (DockingManager.GetState(window) != DockState.Float)
{
    DockingManager.SetState(window, DockState.Float);
}
```

### Float Window Position Not Saving

❌ **Problem:** Window position resets on restart.

✅ **Solution:**
- Use State Persistence feature (see state-persistence.md)
- Or manually save/restore FloatingWindowRect as shown in patterns above
- Ensure LoadDockState is called after window initialization

### Can't Drag Float Window Back

❌ **Problem:** Unable to dock float window by dragging.

✅ **Check:**
```csharp
// Ensure CanDock is not set to false
DockingManager.SetCanDock(window, true);

// Verify IsVS2010DraggingEnabled for VS2010-style hints
dockManager.IsVS2010DraggingEnabled = true;

// Check DockAbility restrictions
var dockAbility = DockingManager.GetDockAbility(window);
if (dockAbility == DockAbility.None)
{
    DockingManager.SetDockAbility(window, DockAbility.All);
}
```

### Float Window Too Large/Small

❌ **Problem:** Float window has wrong size.

✅ **Solution:**
```csharp
// Set minimum/maximum sizes
DockingManager.SetMinimumSizeInFloatingState(window, new Size(200, 150));
DockingManager.SetMaximumSizeInFloatingState(window, new Size(1200, 800));

// Update FloatingWindowRect with correct size
Rect currentRect = DockingManager.GetFloatingWindowRect(window);
Rect newRect = new Rect(currentRect.X, currentRect.Y, 500, 400);
DockingManager.SetFloatingWindowRect(window, newRect);
```
