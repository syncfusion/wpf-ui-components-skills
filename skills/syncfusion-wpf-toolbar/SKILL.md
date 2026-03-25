---
name: syncfusion-wpf-toolbar
description: Implement Syncfusion WPF ToolBarAdv controls for customizable toolbar layouts. Use this when creating toolbars with custom positioning, handling overflow items, implementing floating or docked states, or integrating ToolBarManager for multi-position layouts. Covers band positioning, gripper controls, add/remove button functionality, and dynamic state management.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF ToolBarAdv

## When to Use This Skill

Reference this skill when users need to:

- **Implement ToolBarAdv controls** in WPF applications with Syncfusion.Shared.WPF assembly
- **Create command toolbars** with buttons, separators, and custom controls
- **Handle overflow items** when toolbar content exceeds available space
- **Implement floating toolbars** that can detach and dock at different positions
- **Use ToolBarManager** to organize toolbars at top, bottom, left, or right positions
- **Configure band positioning** to control toolbar layout within ToolBarTrayAdv
- **Add/remove button functionality** allowing users to show/hide toolbar items
- **Customize toolbar appearance** with themes, icon templates, and styling
- **Manage toolbar states** (Docked, Floating, Hidden) dynamically
- **Implement gripper controls** for toolbar drag-and-drop repositioning

## Component Overview

**ToolBarAdv** is a container control for grouping related commands or controls, typically consisting of buttons that invoke actions. It provides:

- **Flexible positioning** with Band and BandIndex properties
- **Overflow panel** for items that don't fit in visible space
- **Multiple states**: Docked, Floating, or Hidden
- **ToolBarManager integration** for multi-position docking
- **Gripper control** for user-driven repositioning
- **Icon templates** supporting path data and font icons
- **Add/Remove buttons** for user-customizable item visibility
- **Theme support** via SfSkinManager

**Key Controls:**
- `ToolBarAdv` - Main toolbar container
- `ToolBarTrayAdv` - Container for organizing multiple toolbars in bands
- `ToolBarManager` - Layout manager for positioning toolbars at Top/Bottom/Left/Right
- `ToolBarItemSeparator` - Visual separator between toolbar items

**Assembly:** Syncfusion.Shared.WPF  
**Namespace:** `Syncfusion.Windows.Tools.Controls`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly and NuGet package installation
- Creating ToolBarAdv via XAML and C#
- Basic structure (Gripper, Overflow button)
- Adding buttons and controls to toolbar
- Icon template setup with path data
- Initial appearance and styling
- License key registration (v16.2.0.x+)

### Positioning and Layout
📄 **Read:** [references/positioning-and-layout.md](references/positioning-and-layout.md)
- Band and BandIndex properties for toolbar placement
- Overflow handling (OverflowMode.Always, Never, AsNeeded)
- Gripper visibility control
- ToolBarTrayAdv orientation (Horizontal/Vertical)
- Multiple toolbar arrangement in bands
- Overflow panel behavior

### Add/Remove Button Functionality
📄 **Read:** [references/add-remove-buttons.md](references/add-remove-buttons.md)
- EnableAddRemoveButton feature
- Adding items with Icon and Label properties
- Hiding toolbar items with IsAvailable attached property
- ToolBarItemInfoCollection management
- User-driven item visibility customization

### Toolbar States and Behavior
📄 **Read:** [references/toolbar-states.md](references/toolbar-states.md)
- ToolBarState property (Docked, Floating, Hidden)
- FloatingBarLocation configuration
- Docking restrictions (CanDockAtTop/Bottom/Left/Right)
- ToolBarStateChanged event handling
- Floating toolbar lifecycle
- State transitions and management

### ToolBarManager Integration
📄 **Read:** [references/toolbar-manager.md](references/toolbar-manager.md)
- TopToolBarTray, BottomToolBarTray, LeftToolBarTray, RightToolBarTray
- Multi-position toolbar layout
- Content placement in remaining space
- IsLocked property to prevent repositioning
- Floating toolbar requirements

### Customization and Theming
📄 **Read:** [references/customization-theming.md](references/customization-theming.md)
- FloatingToolBarStyle property
- Foreground color customization
- ControlsResourceDictionary for FrameworkElement styles
- SfSkinManager theme application
- ThemeStudio for custom themes
- Built-in theme options

## Quick Start Example

### Basic ToolBarAdv with Buttons

**XAML:**
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <syncfusion:ToolBarTrayAdv>
        <syncfusion:ToolBarAdv ToolBarName="Standard" Height="40">
            <!-- New Document Button -->
            <Button Width="22" Height="22" ToolTip="New">
                <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
            </Button>
            
            <!-- Open Document Button -->
            <Button Width="22" Height="22" ToolTip="Open">
                <Image Source="Images/openHS.png" Width="16" Height="16"/>
            </Button>
            
            <!-- Separator -->
            <syncfusion:ToolBarItemSeparator />
            
            <!-- Save Button -->
            <Button Width="22" Height="22" ToolTip="Save">
                <Image Source="Images/saveHS.png" Width="16" Height="16"/>
            </Button>
        </syncfusion:ToolBarAdv>
    </syncfusion:ToolBarTrayAdv>
</Window>
```

**C#:**
```csharp
using Syncfusion.Windows.Tools.Controls;

// Create toolbar
ToolBarAdv toolBar = new ToolBarAdv();
toolBar.ToolBarName = "Standard";
toolBar.Height = 40;

// Add New button
Button newButton = new Button
{
    Width = 22,
    Height = 22,
    ToolTip = "New",
    Content = new Image 
    { 
        Source = new BitmapImage(new Uri("Images/NewDocumentHS.png", UriKind.Relative)),
        Width = 16,
        Height = 16
    }
};
toolBar.Items.Add(newButton);

// Add to ToolBarTray
ToolBarTrayAdv tray = new ToolBarTrayAdv();
tray.ToolBars.Add(toolBar);

// Add to window
this.Content = tray;
```

### ToolBarManager with Multiple Positions

**XAML:**
```xaml
<syncfusion:ToolBarManager x:Name="toolBarManager">
    <!-- Top Toolbar -->
    <syncfusion:ToolBarManager.TopToolBarTray>
        <syncfusion:ToolBarTrayAdv>
            <syncfusion:ToolBarAdv ToolBarName="File">
                <Button Width="22" Height="22" ToolTip="New">
                    <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
                </Button>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.TopToolBarTray>
    
    <!-- Left Toolbar -->
    <syncfusion:ToolBarManager.LeftToolBarTray>
        <syncfusion:ToolBarTrayAdv Orientation="Vertical">
            <syncfusion:ToolBarAdv ToolBarName="Tools">
                <Button Width="22" Height="22" ToolTip="Cut">
                    <Image Source="Images/CutHS.png" Width="16" Height="16"/>
                </Button>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.LeftToolBarTray>
    
    <!-- Main Content -->
    <syncfusion:ToolBarManager.Content>
        <TextBox Text="Your application content here"/>
    </syncfusion:ToolBarManager.Content>
</syncfusion:ToolBarManager>
```

## Common Patterns

### Pattern 1: Band Positioning for Multiple Toolbars

```xaml
<syncfusion:ToolBarTrayAdv>
    <!-- First toolbar on Band 0 -->
    <syncfusion:ToolBarAdv Band="0" BandIndex="0" ToolBarName="Standard">
        <Button Content="New"/>
        <Button Content="Open"/>
    </syncfusion:ToolBarAdv>
    
    <!-- Second toolbar on same band -->
    <syncfusion:ToolBarAdv Band="0" BandIndex="1" ToolBarName="Extras">
        <Button Content="Cut"/>
        <Button Content="Copy"/>
    </syncfusion:ToolBarAdv>
    
    <!-- Third toolbar on different band -->
    <syncfusion:ToolBarAdv Band="1" BandIndex="0" ToolBarName="Format">
        <Button Content="Bold"/>
        <Button Content="Italic"/>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

### Pattern 2: Overflow Mode Control

```xaml
<syncfusion:ToolBarAdv>
    <!-- Always visible -->
    <Button syncfusion:ToolBarAdv.OverflowMode="Never"
            Content="Important"/>
    
    <!-- Can overflow when needed -->
    <Button syncfusion:ToolBarAdv.OverflowMode="AsNeeded"
            Content="Normal"/>
    
    <!-- Always in overflow -->
    <Button syncfusion:ToolBarAdv.OverflowMode="Always"
            Content="Advanced"/>
</syncfusion:ToolBarAdv>
```

### Pattern 3: Add/Remove Buttons with Labels

```xaml
<syncfusion:ToolBarAdv EnableAddRemoveButton="True">
    <Button syncfusion:ToolBarAdv.Label="New Document"
            syncfusion:ToolBarAdv.Icon="Images/NewDocumentHS.png">
        <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
    </Button>
    
    <Button syncfusion:ToolBarAdv.Label="Open Document"
            syncfusion:ToolBarAdv.Icon="Images/openHS.png">
        <Image Source="Images/openHS.png" Width="16" Height="16"/>
    </Button>
</syncfusion:ToolBarAdv>
```

### Pattern 4: Floating Toolbar Configuration

```xaml
<syncfusion:ToolBarManager>
    <syncfusion:ToolBarManager.TopToolBarTray>
        <syncfusion:ToolBarTrayAdv>
            <syncfusion:ToolBarAdv 
                ToolBarName="FloatingTools"
                syncfusion:ToolBarManager.ToolBarState="Floating"
                FloatingBarLocation="500,300">
                <Button Content="Tool 1"/>
                <Button Content="Tool 2"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.TopToolBarTray>
</syncfusion:ToolBarManager>
```

## Key Properties

### ToolBarAdv Properties

| Property | Description | Type | When to Use |
|----------|-------------|------|-------------|
| `Band` | Band index where toolbar should be placed | int | Position toolbar in specific row/column |
| `BandIndex` | Position within the band | int | Order multiple toolbars in same band |
| `ToolBarName` | Display name for toolbar | string | Identify toolbar to users |
| `GripperVisibility` | Show/hide gripper handle | Visibility | Control user repositioning ability |
| `FloatingBarLocation` | Position for floating state | Point | Set custom floating position |
| `IsOverflowOpen` | Overflow popup open state | bool | Programmatically open/close overflow |
| `EnableAddRemoveButton` | Show add/remove menu | bool | Allow users to customize visible items |

### ToolBarManager Properties

| Property | Description | Type | When to Use |
|----------|-------------|------|-------------|
| `TopToolBarTray` | Toolbar tray at top | ToolBarTrayAdv | Position toolbars at top |
| `BottomToolBarTray` | Toolbar tray at bottom | ToolBarTrayAdv | Position toolbars at bottom |
| `LeftToolBarTray` | Toolbar tray at left | ToolBarTrayAdv | Position toolbars at left |
| `RightToolBarTray` | Toolbar tray at right | ToolBarTrayAdv | Position toolbars at right |
| `CanDockAtTop/Bottom/Left/Right` | Allow docking at position | bool | Restrict docking positions |
| `Content` | Main application content | UIElement | Content displayed in remaining space |

## Common Use Cases

### Use Case 1: Application Menu Bar
Create standard application toolbar with File, Edit, Format commands positioned at top.

**When:** Building document editors, IDEs, or productivity applications  
**Read:** [references/getting-started.md](references/getting-started.md), [references/toolbar-manager.md](references/toolbar-manager.md)

### Use Case 2: Contextual Toolbars
Multiple toolbars organized in bands, showing/hiding based on current context or selection.

**When:** Complex applications with mode-specific commands  
**Read:** [references/positioning-and-layout.md](references/positioning-and-layout.md), [references/add-remove-buttons.md](references/add-remove-buttons.md)

### Use Case 3: Floating Tool Palettes
Detachable toolbars that users can position anywhere on screen.

**When:** Graphics editors, design tools, or applications with detachable panels  
**Read:** [references/toolbar-states.md](references/toolbar-states.md), [references/toolbar-manager.md](references/toolbar-manager.md)

### Use Case 4: User-Customizable Toolbars
Allow users to show/hide toolbar items based on their preferences.

**When:** Applications with power users who want to customize their workspace  
**Read:** [references/add-remove-buttons.md](references/add-remove-buttons.md)

## Best Practices

1. **Use ToolBarManager** when you need toolbars at multiple positions (top/bottom/left/right)
2. **Set ToolBarName** for all toolbars to help users identify them
3. **Configure OverflowMode** strategically - keep critical buttons visible with `Never`
4. **Enable Add/Remove buttons** for power user scenarios with many optional commands
5. **Use Icon and Label** attached properties when EnableAddRemoveButton is true
6. **Set meaningful FloatingBarLocation** to ensure floating toolbars appear in useful positions
7. **Apply themes consistently** using SfSkinManager for professional appearance
8. **Use IconTemplate** for scalable vector icons instead of raster images
9. **Group related commands** in the same toolbar with separators
10. **Test toolbar behavior** at different window sizes to ensure overflow works correctly