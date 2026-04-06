# Toolbar States and Behavior

This guide covers toolbar state management including Docked, Floating, and Hidden states, along with floating toolbar configuration and state change events.

## Table of Contents
- [Overview of Toolbar States](#overview-of-toolbar-states)
- [ToolBarState Property](#toolbarstate-property)
- [Floating Toolbar Configuration](#floating-toolbar-configuration)
- [Docking Restrictions](#docking-restrictions)
- [ToolBarStateChanged Event](#toolbarstatechanged-event)
- [State Transitions](#state-transitions)

## Overview of Toolbar States

ToolBarAdv supports three distinct states:

| State | Description | Visual Appearance | When to Use |
|-------|-------------|-------------------|-------------|
| **Docked** | Attached to ToolBarTrayAdv or ToolBarManager | Within main window layout | Default state for most toolbars |
| **Floating** | Detached window that can be positioned anywhere | Separate window with title bar | User preference, tool palettes |
| **Hidden** | Not visible at all | Not displayed | Temporarily hide unused toolbars |

### State Diagram

```
Docked ←→ Floating
  ↓         ↓
Hidden ←→ Hidden
  ↓         ↓
Docked ←→ Floating
```

Users can transition between states by:
- **Dragging gripper** outside main window → Floating
- **Dragging floating toolbar** to dock area → Docked
- **Closing floating toolbar** → Hidden
- **Using context menu** (if implemented)

## ToolBarState Property

The `ToolBarState` attached property (set via ToolBarManager) controls the current state.

### ToolBarState Enumeration

```csharp
public enum ToolBarState
{
    Docked,   // Attached to ToolBarTrayAdv
    Floating, // Separate window
    Hidden    // Not visible
}
```

### Setting Initial State

**XAML (requires ToolBarManager):**

```xaml
<syncfusion:ToolBarManager>
    <syncfusion:ToolBarManager.TopToolBarTray>
        <syncfusion:ToolBarTrayAdv>
            <!-- Docked state (default) -->
            <syncfusion:ToolBarAdv 
                ToolBarName="File"
                syncfusion:ToolBarManager.ToolBarState="Docked">
                <Button Content="New"/>
                <Button Content="Open"/>
            </syncfusion:ToolBarAdv>
            
            <!-- Floating state -->
            <syncfusion:ToolBarAdv 
                ToolBarName="Tools"
                syncfusion:ToolBarManager.ToolBarState="Floating"
                FloatingBarLocation="500,300">
                <Button Content="Tool 1"/>
                <Button Content="Tool 2"/>
            </syncfusion:ToolBarAdv>
            
            <!-- Hidden state -->
            <syncfusion:ToolBarAdv 
                ToolBarName="Advanced"
                syncfusion:ToolBarManager.ToolBarState="Hidden">
                <Button Content="Advanced 1"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.TopToolBarTray>
    
    <syncfusion:ToolBarManager.Content>
        <TextBox Text="Main content"/>
    </syncfusion:ToolBarManager.Content>
</syncfusion:ToolBarManager>
```

### Getting Current State in C#

```csharp
// Get current state
ToolBarState state = ToolBarManager.GetToolBarState(toolBar);

if (state == ToolBarState.Docked)
{
    Console.WriteLine("Toolbar is docked");
}
else if (state == ToolBarState.Floating)
{
    Console.WriteLine("Toolbar is floating");
}
else if (state == ToolBarState.Hidden)
{
    Console.WriteLine("Toolbar is hidden");
}
```

### Setting State Programmatically

```csharp
// Change to floating
ToolBarManager.SetToolBarState(toolBar, ToolBarState.Floating);

// Change to docked
ToolBarManager.SetToolBarState(toolBar, ToolBarState.Docked);

// Hide toolbar
ToolBarManager.SetToolBarState(toolBar, ToolBarState.Hidden);
```

### Important Notes

- **ToolBarState** is an attached property of `ToolBarManager`
- **ToolBarManager required** for state management to work
- **Cannot set on standalone** ToolBarTrayAdv (without ToolBarManager)

## Floating Toolbar Configuration

### FloatingBarLocation Property

**Type:** `Point`  
**Purpose:** Specifies screen coordinates where floating toolbar appears

**XAML:**

```xaml
<syncfusion:ToolBarAdv 
    ToolBarName="Floating Tools"
    syncfusion:ToolBarManager.ToolBarState="Floating"
    FloatingBarLocation="500,300">
    <Button Content="Tool 1"/>
    <Button Content="Tool 2"/>
</syncfusion:ToolBarAdv>
```

**C#:**

```csharp
toolBar.FloatingBarLocation = new Point(500, 300);
ToolBarManager.SetToolBarState(toolBar, ToolBarState.Floating);
```

### Floating Window Behavior

When a toolbar is floating:

1. **Separate window** with its own title bar
2. **Always on top** of main application window
3. **Can be moved** anywhere on screen
4. **Retains toolbar content** and functionality
5. **Close button** hides toolbar (sets state to Hidden)

### Floating Window Properties

**Title Bar Text:**
Uses `ToolBarName` property as window title:

```xaml
<syncfusion:ToolBarAdv 
    ToolBarName="Drawing Tools"
    syncfusion:ToolBarManager.ToolBarState="Floating">
    <!-- Title bar shows "Drawing Tools" -->
</syncfusion:ToolBarAdv>
```

### Customizing Floating Window

**FloatingToolBarStyle** property customizes floating window appearance (see [customization-theming.md](customization-theming.md)):

```xaml
<syncfusion:ToolBarAdv>
    <syncfusion:ToolBarAdv.FloatingToolBarStyle>
        <Style TargetType="Window">
            <Setter Property="Background" Value="LightBlue"/>
            <Setter Property="BorderBrush" Value="DarkBlue"/>
            <Setter Property="BorderThickness" Value="2"/>
        </Style>
    </syncfusion:ToolBarAdv.FloatingToolBarStyle>
</syncfusion:ToolBarAdv>
```

### Detecting Floating State

```csharp
if (ToolBarManager.GetToolBarState(toolBar) == ToolBarState.Floating)
{
    // Toolbar is currently floating
    Point location = toolBar.FloatingBarLocation;
    Console.WriteLine($"Floating at: {location.X}, {location.Y}");
}
```

## Docking Restrictions

Control which sides of the window allow toolbar docking.

### CanDock Properties

**Properties:**
- `CanDockAtTop` (bool)
- `CanDockAtBottom` (bool)
- `CanDockAtLeft` (bool)
- `CanDockAtRight` (bool)

**Default:** All `true`

### Restricting Docking Positions

**XAML:**

```xaml
<syncfusion:ToolBarManager>
    <syncfusion:ToolBarManager.TopToolBarTray>
        <syncfusion:ToolBarTrayAdv>
            <!-- Only allow docking at top -->
            <syncfusion:ToolBarAdv 
                ToolBarName="Main Menu"
                CanDockAtTop="True"
                CanDockAtBottom="False"
                CanDockAtLeft="False"
                CanDockAtRight="False">
                <Button Content="File"/>
            </syncfusion:ToolBarAdv>
            
            <!-- Allow left or right only -->
            <syncfusion:ToolBarAdv 
                ToolBarName="Side Tools"
                CanDockAtTop="False"
                CanDockAtBottom="False"
                CanDockAtLeft="True"
                CanDockAtRight="True">
                <Button Content="Tool"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.TopToolBarTray>
    
    <syncfusion:ToolBarManager.Content>
        <TextBox/>
    </syncfusion:ToolBarManager.Content>
</syncfusion:ToolBarManager>
```

**C#:**

```csharp
// Restrict to top only
toolBar.CanDockAtTop = true;
toolBar.CanDockAtBottom = false;
toolBar.CanDockAtLeft = false;
toolBar.CanDockAtRight = false;
```

### Use Cases

**Top-only docking:**
- Main menu bar
- Primary command toolbar
- Preventing vertical orientation

**Side-only docking:**
- Tool palettes
- Vertical toolbars
- Navigation panels

**No docking (floating only):**
- Tool windows
- Inspectors
- Temporary dialogs

```csharp
// Floating only - cannot dock
toolBar.CanDockAtTop = false;
toolBar.CanDockAtBottom = false;
toolBar.CanDockAtLeft = false;
toolBar.CanDockAtRight = false;
ToolBarManager.SetToolBarState(toolBar, ToolBarState.Floating);
```

## ToolBarStateChanged Event

Detect when toolbar state changes (Docked ↔ Floating ↔ Hidden).

### Event Signature

```csharp
public event EventHandler<ToolBarStateChangedEventArgs> ToolBarStateChanged
```

### ToolBarStateChangedEventArgs

```csharp
public class ToolBarStateChangedEventArgs : EventArgs
{
    public ToolBarState OldState { get; }  // Previous state
    public ToolBarState NewState { get; }  // New state
}
```

### Subscribing to Event

**XAML:**

```xaml
<syncfusion:ToolBarAdv 
    ToolBarName="File"
    ToolBarStateChanged="ToolBar_StateChanged">
    <Button Content="New"/>
</syncfusion:ToolBarAdv>
```

**Code-behind:**

```csharp
private void ToolBar_StateChanged(object sender, ToolBarStateChangedEventArgs e)
{
    ToolBarAdv toolBar = sender as ToolBarAdv;
    
    Console.WriteLine($"State changed: {e.OldState} → {e.NewState}");
    
    if (e.NewState == ToolBarState.Floating)
    {
        Console.WriteLine($"Floating at: {toolBar.FloatingBarLocation}");
    }
}
```

### C# Subscription

```csharp
toolBar.ToolBarStateChanged += (sender, e) =>
{
    MessageBox.Show($"Toolbar state changed from {e.OldState} to {e.NewState}");
};
```

### Common Event Handling Scenarios

**Track floating toolbars:**

```csharp
private List<ToolBarAdv> floatingToolBars = new List<ToolBarAdv>();

private void ToolBar_StateChanged(object sender, ToolBarStateChangedEventArgs e)
{
    ToolBarAdv toolBar = sender as ToolBarAdv;
    
    if (e.NewState == ToolBarState.Floating)
    {
        floatingToolBars.Add(toolBar);
    }
    else if (e.OldState == ToolBarState.Floating)
    {
        floatingToolBars.Remove(toolBar);
    }
    
    UpdateStatusBar($"{floatingToolBars.Count} floating toolbars");
}
```

**Prevent unwanted state changes:**

```csharp
private void ToolBar_StateChanged(object sender, ToolBarStateChangedEventArgs e)
{
    ToolBarAdv toolBar = sender as ToolBarAdv;
    
    // Prevent hiding critical toolbar
    if (toolBar.ToolBarName == "Main Menu" && e.NewState == ToolBarState.Hidden)
    {
        MessageBox.Show("Main menu cannot be hidden");
        ToolBarManager.SetToolBarState(toolBar, ToolBarState.Docked);
    }
}
```

## State Transitions

### User-Initiated Transitions

**Docked → Floating:**
1. User drags toolbar by gripper
2. Drags outside docking area
3. Releases mouse button
4. Toolbar becomes floating window

**Floating → Docked:**
1. User drags floating window by title bar
2. Drags over valid docking area (based on CanDock settings)
3. Docking preview appears
4. Releases mouse button
5. Toolbar docks at that position

**Any State → Hidden:**
1. User closes floating toolbar window (X button)
2. Or uses View menu / context menu to hide

### Programmatic Transitions

```csharp
// Show hidden toolbar at specific position
if (ToolBarManager.GetToolBarState(toolBar) == ToolBarState.Hidden)
{
    toolBar.FloatingBarLocation = new Point(600, 400);
    ToolBarManager.SetToolBarState(toolBar, ToolBarState.Floating);
}

// Dock floating toolbar
if (ToolBarManager.GetToolBarState(toolBar) == ToolBarState.Floating)
{
    ToolBarManager.SetToolBarState(toolBar, ToolBarState.Docked);
    // Will dock at last known docked position
}

// Hide toolbar temporarily
ToolBarManager.SetToolBarState(toolBar, ToolBarState.Hidden);

// Restore to previous state
ToolBarManager.SetToolBarState(toolBar, previousState);
```

### Managing Toolbar Visibility

Toggle toolbar visibility via a View menu using `ToolBarManager.SetToolBarState`:

```csharp
// Show toolbar
ToolBarManager.SetToolBarState(toolBar, ToolBarState.Docked);

// Hide toolbar
ToolBarManager.SetToolBarState(toolBar, ToolBarState.Hidden);
```
