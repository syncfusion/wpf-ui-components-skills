# ToolBarManager Integration

This guide covers using ToolBarManager to organize toolbars at multiple positions (Top, Bottom, Left, Right) and manage floating toolbar behavior.

## Table of Contents
- [Overview](#overview)
- [Basic ToolBarManager Setup](#basic-toolbarmanager-setup)
- [Positioning Toolbars](#positioning-toolbars)
- [Content Placement](#content-placement)
- [IsLocked Property](#islocked-property)
- [Floating Toolbar Requirements](#floating-toolbar-requirements)
- [Best Practices](#best-practices)

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

### TopToolBarTray

**Position:** Horizontal toolbar area at top of window

**Default Orientation:** Horizontal (toolbars stack in rows)

**Example:**

```xaml
<syncfusion:ToolBarManager.TopToolBarTray>
    <syncfusion:ToolBarTrayAdv Orientation="Horizontal">
        <syncfusion:ToolBarAdv ToolBarName="File" Band="0">
            <Button Content="New"/>
            <Button Content="Open"/>
        </syncfusion:ToolBarAdv>
        
        <syncfusion:ToolBarAdv ToolBarName="Edit" Band="1">
            <Button Content="Cut"/>
            <Button Content="Copy"/>
        </syncfusion:ToolBarAdv>
    </syncfusion:ToolBarTrayAdv>
</syncfusion:ToolBarManager.TopToolBarTray>
```

### BottomToolBarTray

**Position:** Horizontal toolbar area at bottom of window

**Default Orientation:** Horizontal

**Example:**

```xaml
<syncfusion:ToolBarManager.BottomToolBarTray>
    <syncfusion:ToolBarTrayAdv Orientation="Horizontal">
        <syncfusion:ToolBarAdv ToolBarName="Status Tools">
            <ComboBox Width="100" SelectedIndex="0">
                <ComboBoxItem Content="100%"/>
                <ComboBoxItem Content="150%"/>
            </ComboBox>
            <Button Content="Refresh"/>
        </syncfusion:ToolBarAdv>
    </syncfusion:ToolBarTrayAdv>
</syncfusion:ToolBarManager.BottomToolBarTray>
```

### LeftToolBarTray

**Position:** Vertical toolbar area on left side

**Default Orientation:** Vertical (toolbars stack in columns)

**Example:**

```xaml
<syncfusion:ToolBarManager.LeftToolBarTray>
    <syncfusion:ToolBarTrayAdv Orientation="Vertical">
        <syncfusion:ToolBarAdv ToolBarName="Drawing Tools">
            <Button Width="32" Height="32" ToolTip="Line">
                <Image Source="Images/Line.png" Width="24" Height="24"/>
            </Button>
            <Button Width="32" Height="32" ToolTip="Rectangle">
                <Image Source="Images/Rectangle.png" Width="24" Height="24"/>
            </Button>
            <Button Width="32" Height="32" ToolTip="Circle">
                <Image Source="Images/Circle.png" Width="24" Height="24"/>
            </Button>
        </syncfusion:ToolBarAdv>
    </syncfusion:ToolBarTrayAdv>
</syncfusion:ToolBarManager.LeftToolBarTray>
```

### RightToolBarTray

**Position:** Vertical toolbar area on right side

**Default Orientation:** Vertical

**Example:**

```xaml
<syncfusion:ToolBarManager.RightToolBarTray>
    <syncfusion:ToolBarTrayAdv Orientation="Vertical">
        <syncfusion:ToolBarAdv ToolBarName="Properties">
            <Label Content="Width:"/>
            <TextBox Width="60" Text="100"/>
            <Label Content="Height:"/>
            <TextBox Width="60" Text="100"/>
        </syncfusion:ToolBarAdv>
    </syncfusion:ToolBarTrayAdv>
</syncfusion:ToolBarManager.RightToolBarTray>
```

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

### Complex Content Layout

```xaml
<syncfusion:ToolBarManager.Content>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        
        <!-- Main editor -->
        <RichTextBox Grid.Row="0" 
                     VerticalScrollBarVisibility="Auto"/>
        
        <!-- Status bar -->
        <StatusBar Grid.Row="1">
            <StatusBarItem Content="Line 1, Col 1"/>
            <StatusBarItem Content="Ready"/>
        </StatusBar>
    </Grid>
</syncfusion:ToolBarManager.Content>
```

### Setting Content in C#

```csharp
ToolBarManager manager = new ToolBarManager();

// Simple content
manager.Content = new TextBox 
{ 
    AcceptsReturn = true,
    TextWrapping = TextWrapping.Wrap
};

// Complex content
Grid contentGrid = new Grid();
contentGrid.Children.Add(new RichTextBox());
manager.Content = contentGrid;
```

### Content Sizing

Content area automatically fills remaining space after toolbar trays:

```
Width = Window.Width - LeftToolBarTray.Width - RightToolBarTray.Width
Height = Window.Height - TopToolBarTray.Height - BottomToolBarTray.Height
```

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

## Complete Example

```xaml
<Window x:Class="ToolBarManagerDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="ToolBarManager Demo" Height="600" Width="900">
    
    <syncfusion:ToolBarManager x:Name="toolBarManager">
        
        <!-- TOP: File and Edit Toolbars -->
        <syncfusion:ToolBarManager.TopToolBarTray>
            <syncfusion:ToolBarTrayAdv Orientation="Horizontal">
                <!-- Main menu - locked -->
                <syncfusion:ToolBarAdv ToolBarName="File" 
                                       Band="0"
                                       IsLocked="True"
                                       GripperVisibility="Collapsed">
                    <Button Content="New" Width="50"/>
                    <Button Content="Open" Width="50"/>
                    <Button Content="Save" Width="50"/>
                    <syncfusion:ToolBarItemSeparator/>
                    <Button Content="Exit" Width="50"/>
                </syncfusion:ToolBarAdv>
                
                <!-- Edit toolbar - movable -->
                <syncfusion:ToolBarAdv ToolBarName="Edit" Band="1">
                    <Button Content="Cut" Width="50"/>
                    <Button Content="Copy" Width="50"/>
                    <Button Content="Paste" Width="50"/>
                </syncfusion:ToolBarAdv>
            </syncfusion:ToolBarTrayAdv>
        </syncfusion:ToolBarManager.TopToolBarTray>
        
        <!-- LEFT: Tool Palette -->
        <syncfusion:ToolBarManager.LeftToolBarTray>
            <syncfusion:ToolBarTrayAdv Orientation="Vertical">
                <syncfusion:ToolBarAdv ToolBarName="Drawing Tools"
                                       syncfusion:ToolBarManager.ToolBarState="Docked">
                    <Button Width="40" Height="40" ToolTip="Select">
                        <Path Data="M0,0 L8,8 L0,16 Z" Fill="Black" Stretch="Uniform" Width="20" Height="20"/>
                    </Button>
                    <Button Width="40" Height="40" ToolTip="Line">
                        <Path Data="M0,20 L20,0" Stroke="Black" StrokeThickness="2" Stretch="Uniform" Width="20" Height="20"/>
                    </Button>
                    <Button Width="40" Height="40" ToolTip="Rectangle">
                        <Rectangle Width="20" Height="15" Stroke="Black" StrokeThickness="2"/>
                    </Button>
                </syncfusion:ToolBarAdv>
            </syncfusion:ToolBarTrayAdv>
        </syncfusion:ToolBarManager.LeftToolBarTray>
        
        <!-- RIGHT: Properties Panel -->
        <syncfusion:ToolBarManager.RightToolBarTray>
            <syncfusion:ToolBarTrayAdv Orientation="Vertical">
                <syncfusion:ToolBarAdv ToolBarName="Properties"
                                       syncfusion:ToolBarManager.ToolBarState="Docked">
                    <StackPanel Orientation="Vertical" Margin="5">
                        <Label Content="Width:" FontWeight="Bold"/>
                        <TextBox Width="80" Text="100" Margin="0,0,0,10"/>
                        
                        <Label Content="Height:" FontWeight="Bold"/>
                        <TextBox Width="80" Text="100" Margin="0,0,0,10"/>
                        
                        <Label Content="Color:" FontWeight="Bold"/>
                        <ComboBox Width="80" SelectedIndex="0">
                            <ComboBoxItem Content="Black"/>
                            <ComboBoxItem Content="Red"/>
                            <ComboBoxItem Content="Blue"/>
                        </ComboBox>
                    </StackPanel>
                </syncfusion:ToolBarAdv>
            </syncfusion:ToolBarTrayAdv>
        </syncfusion:ToolBarManager.RightToolBarTray>
        
        <!-- BOTTOM: Status Toolbar -->
        <syncfusion:ToolBarManager.BottomToolBarTray>
            <syncfusion:ToolBarTrayAdv Orientation="Horizontal">
                <syncfusion:ToolBarAdv ToolBarName="Status" IsLocked="True">
                    <Label Content="Zoom:"/>
                    <ComboBox Width="80" SelectedIndex="2">
                        <ComboBoxItem Content="50%"/>
                        <ComboBoxItem Content="75%"/>
                        <ComboBoxItem Content="100%"/>
                        <ComboBoxItem Content="150%"/>
                    </ComboBox>
                    <syncfusion:ToolBarItemSeparator/>
                    <Label Content="Ready" FontWeight="Bold"/>
                </syncfusion:ToolBarAdv>
            </syncfusion:ToolBarTrayAdv>
        </syncfusion:ToolBarManager.BottomToolBarTray>
        
        <!-- CENTER: Main Canvas -->
        <syncfusion:ToolBarManager.Content>
            <Grid Background="WhiteSmoke">
                <Canvas x:Name="drawingCanvas" Background="White" Margin="10">
                    <TextBlock Canvas.Left="100" Canvas.Top="100" 
                               Text="Drawing Area" 
                               FontSize="24" 
                               Foreground="Gray"/>
                </Canvas>
            </Grid>
        </syncfusion:ToolBarManager.Content>
        
    </syncfusion:ToolBarManager>
</Window>
```

## Best Practices

### Layout Organization

1. **Top**: Primary commands (File, Edit, Format)
2. **Left**: Tool palettes, vertical navigation
3. **Right**: Property panels, inspectors
4. **Bottom**: Status information, view controls

### Orientation Consistency

```xaml
<!-- ✓ GOOD - Horizontal orientation for Top/Bottom -->
<syncfusion:ToolBarManager.TopToolBarTray>
    <syncfusion:ToolBarTrayAdv Orientation="Horizontal">
        ...
    </syncfusion:ToolBarTrayAdv>
</syncfusion:ToolBarManager.TopToolBarTray>

<!-- ✓ GOOD - Vertical orientation for Left/Right -->
<syncfusion:ToolBarManager.LeftToolBarTray>
    <syncfusion:ToolBarTrayAdv Orientation="Vertical">
        ...
    </syncfusion:ToolBarTrayAdv>
</syncfusion:ToolBarManager.LeftToolBarTray>
```

### Locking Strategy

- **Lock main menu** (File, Edit) to prevent accidents
- **Allow customization** of secondary toolbars
- **Lock during critical operations** (save, export)

### Content Area Design

```xaml
<!-- ✓ GOOD - Content takes remaining space -->
<syncfusion:ToolBarManager.Content>
    <Grid>
        <!-- Your main UI here -->
    </Grid>
</syncfusion:ToolBarManager.Content>

<!-- ✗ BAD - Don't set explicit size on content -->
<syncfusion:ToolBarManager.Content>
    <Grid Width="800" Height="600"> <!-- Remove fixed sizes -->
        ...
    </Grid>
</syncfusion:ToolBarManager.Content>
```

### Multi-Position Guidelines

- **Limit toolbars**: Don't overcrowd all four positions
- **Logical grouping**: Related tools together
- **User preference**: Allow floating/hiding of optional toolbars
- **Consistent sizing**: Match button sizes within same toolbar

## Related Topics

- **Getting Started**: Basic toolbar setup → [getting-started.md](getting-started.md)
- **Toolbar States**: Docked/Floating/Hidden → [toolbar-states.md](toolbar-states.md)
- **Positioning**: Band and BandIndex → [positioning-and-layout.md](positioning-and-layout.md)
- **Customization**: Themes and styling → [customization-theming.md](customization-theming.md)
