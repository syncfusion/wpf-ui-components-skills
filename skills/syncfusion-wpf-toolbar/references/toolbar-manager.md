# ToolBarManager Integration

This guide covers using ToolBarManager to organize toolbars at multiple positions (Top, Bottom, Left, Right) and manage floating toolbar behavior.

## Table of Contents
- [Overview](#overview)
- [Basic ToolBarManager Setup](#basic-toolbarmanager-setup)
- [Positioning Toolbars](#positioning-toolbars)
- [Content Placement](#content-placement)
- [IsLocked Property](#islocked-property)
- [Floating Toolbar Requirements](#floating-toolbar-requirements)

## Overview

**ToolBarManager** is a container control that provides:

- **Four docking positions**: Top, Bottom, Left, Right
- **ToolBarTrayAdv** at each position
- **Central content area** for main application UI
- **State management** for docked/floating/hidden toolbars
- **Drag-and-drop** repositioning between trays

### When to Use ToolBarManager

| Scenario | Use ToolBarManager | Use ToolBarTrayAdv |
|----------|-------------------|-------------------|
| Toolbars at multiple positions (top + left) | ✓ Yes | ✗ No |
| Need floating toolbar support | ✓ Yes | ✗ No |
| Single toolbar position (top only) | Optional | ✓ Yes |
| Toolbar state management | ✓ Yes | ✗ No |
| Application with main content area | ✓ Yes | Optional |

### Visual Structure

```
┌─────────────────────────────────┐
│     TopToolBarTray              │
├──┬────────────────────────────┬─┤
│L │                            │R│
│e │      Content Area          │i│
│f │                            │g│
│t │                            │h│
│  │                            │t│
├──┴────────────────────────────┴─┤
│     BottomToolBarTray           │
└─────────────────────────────────┘
```

## Basic ToolBarManager Setup

### Minimal XAML

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <syncfusion:ToolBarManager>
        <!-- Top Toolbar -->
        <syncfusion:ToolBarManager.TopToolBarTray>
            <syncfusion:ToolBarTrayAdv>
                <syncfusion:ToolBarAdv ToolBarName="File">
                    <Button Content="New"/>
                    <Button Content="Open"/>
                    <Button Content="Save"/>
                </syncfusion:ToolBarAdv>
            </syncfusion:ToolBarTrayAdv>
        </syncfusion:ToolBarManager.TopToolBarTray>
        
        <!-- Main Content -->
        <syncfusion:ToolBarManager.Content>
            <TextBox Text="Your application content here"/>
        </syncfusion:ToolBarManager.Content>
    </syncfusion:ToolBarManager>
</Window>
```

### C# Creation

```csharp
using Syncfusion.Windows.Tools.Controls;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        CreateToolBarLayout();
    }
    
    private void CreateToolBarLayout()
    {
        // Create ToolBarManager
        ToolBarManager manager = new ToolBarManager();
        
        // Create top toolbar
        ToolBarTrayAdv topTray = new ToolBarTrayAdv();
        ToolBarAdv fileToolBar = new ToolBarAdv { ToolBarName = "File" };
        fileToolBar.Items.Add(new Button { Content = "New" });
        fileToolBar.Items.Add(new Button { Content = "Open" });
        topTray.ToolBars.Add(fileToolBar);
        
        // Set top tray
        manager.TopToolBarTray = topTray;
        
        // Set content
        manager.Content = new TextBox { Text = "Main content" };
        
        // Set as window content
        this.Content = manager;
    }
}
```

## Positioning Toolbars

ToolBarManager provides four positioning properties, each containing a ToolBarTrayAdv.

### Tray Properties

| Property | Position | Default Orientation |
|----------|----------|---------------------|
| `TopToolBarTray` | Top of window | Horizontal |
| `BottomToolBarTray` | Bottom of window | Horizontal |
| `LeftToolBarTray` | Left side | Vertical |
| `RightToolBarTray` | Right side | Vertical |

### Multi-Position Example

```xaml
<syncfusion:ToolBarManager>
    <!-- Top: File and Edit toolbars -->
    <syncfusion:ToolBarManager.TopToolBarTray>
        <syncfusion:ToolBarTrayAdv>
            <syncfusion:ToolBarAdv ToolBarName="File">
                <Button Content="New"/>
                <Button Content="Open"/>
            </syncfusion:ToolBarAdv>
            <syncfusion:ToolBarAdv ToolBarName="Edit">
                <Button Content="Cut"/>
                <Button Content="Copy"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.TopToolBarTray>
    
    <!-- Left: Tool palette -->
    <syncfusion:ToolBarManager.LeftToolBarTray>
        <syncfusion:ToolBarTrayAdv Orientation="Vertical">
            <syncfusion:ToolBarAdv ToolBarName="Tools">
                <Button Width="32" Height="32" Content="📐"/>
                <Button Width="32" Height="32" Content="✏️"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.LeftToolBarTray>
    
    <!-- Right: Properties panel -->
    <syncfusion:ToolBarManager.RightToolBarTray>
        <syncfusion:ToolBarTrayAdv Orientation="Vertical">
            <syncfusion:ToolBarAdv ToolBarName="Properties">
                <Label Content="Options"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.RightToolBarTray>
    
    <!-- Bottom: Status toolbar -->
    <syncfusion:ToolBarManager.BottomToolBarTray>
        <syncfusion:ToolBarTrayAdv>
            <syncfusion:ToolBarAdv ToolBarName="Status">
                <Label Content="Ready"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.BottomToolBarTray>
    
    <!-- Center: Main content -->
    <syncfusion:ToolBarManager.Content>
        <Grid Background="White">
            <TextBox Text="Canvas or editor area"/>
        </Grid>
    </syncfusion:ToolBarManager.Content>
</syncfusion:ToolBarManager>
```

## Content Placement

The `Content` property specifies the main application UI displayed in the center area.

### Setting Content in XAML

```xaml
<syncfusion:ToolBarManager>
    <syncfusion:ToolBarManager.TopToolBarTray>
        <syncfusion:ToolBarTrayAdv>
            <syncfusion:ToolBarAdv ToolBarName="File">
                <Button Content="New"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.TopToolBarTray>
    
    <!-- Content: TextBox editor -->
    <syncfusion:ToolBarManager.Content>
        <TextBox AcceptsReturn="True" 
                 TextWrapping="Wrap"
                 VerticalScrollBarVisibility="Auto"/>
    </syncfusion:ToolBarManager.Content>
</syncfusion:ToolBarManager>
```

The `Content` area automatically fills remaining space after toolbar trays are laid out.

## IsLocked Property

The `IsLocked` property prevents users from repositioning toolbars.

### IsLocked on ToolBarAdv

**Type:** `bool`  
**Default:** `false`

**Purpose:** Lock individual toolbar to prevent dragging/floating

**XAML:**

```xaml
<syncfusion:ToolBarAdv ToolBarName="Main Menu" IsLocked="True">
    <!-- Users cannot move this toolbar -->
    <Button Content="File"/>
    <Button Content="Edit"/>
</syncfusion:ToolBarAdv>
```

**C#:**

```csharp
toolBar.IsLocked = true; // Prevent repositioning
```

### IsLocked on ToolBarTrayAdv

**Type:** `bool`  
**Default:** `false`

**Purpose:** Lock all toolbars within the tray

**XAML:**

```xaml
<syncfusion:ToolBarTrayAdv IsLocked="True">
    <!-- All toolbars in this tray are locked -->
    <syncfusion:ToolBarAdv ToolBarName="File">
        <Button Content="New"/>
    </syncfusion:ToolBarAdv>
    
    <syncfusion:ToolBarAdv ToolBarName="Edit">
        <Button Content="Cut"/>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

### Behavior When Locked

| Action | IsLocked="False" | IsLocked="True" |
|--------|------------------|----------------|
| Drag by gripper | Repositions toolbar | No effect |
| Float toolbar | Creates floating window | Blocked |
| Move between bands | Allowed | Blocked |
| User closes floating window | Hides toolbar | N/A (cannot float) |

### Use Cases

**Lock main menu:**
```xaml
<!-- Prevent accidental repositioning of critical toolbar -->
<syncfusion:ToolBarAdv ToolBarName="Main Menu" 
                       IsLocked="True"
                       GripperVisibility="Collapsed">
    <Button Content="File"/>
</syncfusion:ToolBarAdv>
```

**Lock during specific operations:**

```csharp
private void StartCriticalOperation()
{
    // Lock all toolbars during operation
    foreach (ToolBarAdv toolBar in allToolBars)
    {
        toolBar.IsLocked = true;
    }
}

private void EndCriticalOperation()
{
    // Unlock
    foreach (ToolBarAdv toolBar in allToolBars)
    {
        toolBar.IsLocked = false;
    }
}
```

### Combining IsLocked with GripperVisibility

```xaml
<!-- Lock AND hide gripper for cleaner appearance -->
<syncfusion:ToolBarAdv ToolBarName="Fixed Toolbar"
                       IsLocked="True"
                       GripperVisibility="Collapsed">
    <Button Content="Fixed"/>
</syncfusion:ToolBarAdv>
```

## Floating Toolbar Requirements

For toolbars to support floating state, they **must** be within a ToolBarManager.

### Requirements

1. **ToolBarAdv** must be in a **ToolBarTrayAdv**
2. **ToolBarTrayAdv** must be in a **ToolBarManager** property (Top/Bottom/Left/Right)
3. Set `ToolBarState` via `ToolBarManager.ToolBarState` attached property

### Correct Setup for Floating

```xaml
<syncfusion:ToolBarManager>
    <syncfusion:ToolBarManager.TopToolBarTray>
        <syncfusion:ToolBarTrayAdv>
            <!-- This toolbar CAN float -->
            <syncfusion:ToolBarAdv 
                ToolBarName="Floatable"
                syncfusion:ToolBarManager.ToolBarState="Floating"
                FloatingBarLocation="500,300">
                <Button Content="Tool"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.TopToolBarTray>
    
    <syncfusion:ToolBarManager.Content>
        <TextBox/>
    </syncfusion:ToolBarManager.Content>
</syncfusion:ToolBarManager>
```

### Incorrect Setup (Cannot Float)

```xaml
<!-- ❌ This CANNOT support floating - no ToolBarManager -->
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <syncfusion:ToolBarTrayAdv Grid.Row="0">
        <syncfusion:ToolBarAdv ToolBarName="Cannot Float">
            <Button Content="Tool"/>
        </syncfusion:ToolBarAdv>
    </syncfusion:ToolBarTrayAdv>
    
    <TextBox Grid.Row="1"/>
</Grid>
```

### Checking Floating Support

```csharp
// Check if toolbar is in ToolBarManager
bool canFloat = ToolBarManager.GetToolBarState(toolBar) != null;

if (canFloat)
{
    ToolBarManager.SetToolBarState(toolBar, ToolBarState.Floating);
}
else
{
    MessageBox.Show("Toolbar must be in ToolBarManager to support floating");
}
```

## Layout Convention

| Position | Tray Property | Recommended Orientation | Typical Use |
|----------|--------------|-------------------------|-------------|
| Top | `TopToolBarTray` | Horizontal | File, Edit, Format |
| Bottom | `BottomToolBarTray` | Horizontal | Status, zoom controls |
| Left | `LeftToolBarTray` | Vertical | Tool palettes |
| Right | `RightToolBarTray` | Vertical | Property panels |
