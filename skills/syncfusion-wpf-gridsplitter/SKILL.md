---
name: syncfusion-wpf-gridsplitter
description: Guide for implementing Syncfusion WPF GridSplitter (SfGridSplitter) control for dynamic layout resizing. Use this skill when implementing WPF GridSplitter, split panels, or resizable grid layouts. Covers splitting grid rows or columns with movable splitters, collapse/expand functionality, resizable panels, drag behavior configuration, and splitter appearance customization in WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF GridSplitter

This skill guides you in implementing the Syncfusion® WPF GridSplitter (SfGridSplitter) control, a container control that splits available space horizontally or vertically with a movable splitter to dynamically resize visual elements.

## When to Use This Skill

Use this skill when you need to:
- **Create resizable layouts** with movable splitters between panels
- **Implement split panels** in WPF applications (horizontal or vertical)
- **Add collapse/expand functionality** for grid sections
- **Enable dynamic resizing** of grid rows or columns with user interaction
- **Configure drag behavior** for splitter movement and preview
- **Customize splitter appearance** with templates and styles
- **Support keyboard-based resizing** with increment controls
- **Create multi-pane layouts** with independent resizable sections

This skill covers complete GridSplitter implementation from basic setup to advanced customization.

## Component Overview

**SfGridSplitter** is a layout control that divides available space in a Grid with an interactive splitter bar. Key capabilities:

- **Dynamic Resizing:** Movable splitter redistributes space between grid rows/columns on demand
- **Orientation Support:** Horizontal or vertical splitting based on grid placement
- **Collapse/Expand:** Built-in buttons to hide/show panels interactively
- **Preview Mode:** Optional preview of resize operation before applying
- **Customizable Templates:** Full control over gripper, collapse buttons, and preview appearance
- **Keyboard Support:** Resize using keyboard with configurable increment values
- **Merged Cell Support:** Works with merged grid rows and columns

**Typical Use Cases:**
- Master-detail views with resizable sections
- Code editor with collapsible side panels
- Data explorer with adjustable tree/grid split
- Multi-document interfaces with resizable panes

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Learn the fundamentals of adding and configuring SfGridSplitter:
- Assembly deployment and NuGet package installation
- Adding control in XAML with namespace imports
- Adding control programmatically in C#
- Basic grid splitter setup in row/column layouts
- Initial configuration and alignment requirements

**Use this reference when:** Setting up GridSplitter for the first time, adding required assemblies, or creating basic split layouts.

### Resize and Layout Operations
📄 **Read:** [references/resize-and-layout.md](references/resize-and-layout.md)

Master grid resizing capabilities and layout configuration:
- Resizing grid rows with horizontal splitters
- Resizing grid columns with vertical splitters
- ResizeBehavior property (PreviousAndNext, PreviousAndCurrent, etc.)
- DragIncrement and KeyboardIncrement for pixel-precise resizing
- MoveSplitter() method for programmatic resizing
- Working with merged columns and rows
- Orientation handling and alignment strategies

**Use this reference when:** Configuring resize behavior, implementing programmatic resizing, working with merged cells, or fine-tuning drag/keyboard increments.

### Collapse and Expand Features
📄 **Read:** [references/collapse-expand-features.md](references/collapse-expand-features.md)

Implement interactive collapse/expand functionality:
- EnableCollapseButton property to show/hide buttons
- Collapsing and expanding grid rows and columns
- Custom collapse button templates (Up, Down, Left, Right)
- UpButtonTemplate and DownButtonTemplate for horizontal splitters
- LeftButtonTemplate and RightButtonTemplate for vertical splitters
- User interaction patterns for panel visibility

**Use this reference when:** Adding collapse/expand buttons, customizing button appearance, or implementing panel hide/show features.

### Customization and Styling
📄 **Read:** [references/customization-styling.md](references/customization-styling.md)

Customize visual appearance and behavior:
- Background property for splitter color
- HorizontalGripperTemplate for horizontal splitter gripper
- VerticalGripperTemplate for vertical splitter gripper
- PreviewStyle for deferred resizing preview
- ShowsPreview property for preview mode
- Custom drag preview UI with templates
- Advanced styling scenarios and theme integration

**Use this reference when:** Customizing splitter appearance, creating custom gripper designs, implementing preview styles, or matching application themes.

## Quick Start Example

### Basic Horizontal Splitter (Resizes Rows)

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <!-- Top Panel -->
        <Border Grid.Row="0" Background="LightBlue">
            <TextBlock Text="Top Panel" 
                       VerticalAlignment="Center" 
                       HorizontalAlignment="Center"/>
        </Border>
        
        <!-- Grid Splitter -->
        <syncfusion:SfGridSplitter Grid.Row="1"
                                   HorizontalAlignment="Stretch"
                                   Height="5"
                                   Background="DarkGray"
                                   ResizeBehavior="PreviousAndNext"/>
        
        <!-- Bottom Panel -->
        <Border Grid.Row="2" Background="LightGreen">
            <TextBlock Text="Bottom Panel" 
                       VerticalAlignment="Center" 
                       HorizontalAlignment="Center"/>
        </Border>
    </Grid>
</Window>
```

### Basic Vertical Splitter (Resizes Columns)

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="*" />
    </Grid.ColumnDefinitions>
    
    <!-- Left Panel -->
    <Border Grid.Column="0" Background="LightCoral">
        <TextBlock Text="Left Panel" 
                   VerticalAlignment="Center" 
                   HorizontalAlignment="Center"/>
    </Border>
    
    <!-- Grid Splitter -->
    <syncfusion:SfGridSplitter Grid.Column="1"
                               VerticalAlignment="Stretch"
                               Width="5"
                               Background="DarkGray"
                               ResizeBehavior="PreviousAndNext"/>
    
    <!-- Right Panel -->
    <Border Grid.Column="2" Background="LightYellow">
        <TextBlock Text="Right Panel" 
                   VerticalAlignment="Center" 
                   HorizontalAlignment="Center"/>
    </Border>
</Grid>
```

## Common Patterns

### Pattern 1: Splitter with Collapse Buttons

Enable interactive panel collapse/expand:

```xml
<syncfusion:SfGridSplitter Grid.Row="1"
                           HorizontalAlignment="Stretch"
                           Height="8"
                           EnableCollapseButton="True"
                           ResizeBehavior="PreviousAndNext"/>
```

**When to use:** Users need to temporarily hide panels to maximize workspace.

### Pattern 2: Preview Mode for Deferred Resizing

Show preview before applying resize:

```xml
<syncfusion:SfGridSplitter Grid.Row="1"
                           HorizontalAlignment="Stretch"
                           ShowsPreview="True"
                           ResizeBehavior="PreviousAndNext"/>
```

**When to use:** Heavy layouts where real-time resizing may cause performance issues, or users need visual feedback before committing to resize.

### Pattern 3: Programmatic Resizing

Control splitter position via code:

```csharp
// Move splitter by 50 pixels
gridSplitter.MoveSplitter(50);

// Move splitter by -30 pixels (opposite direction)
gridSplitter.MoveSplitter(-30);
```

**When to use:** Implementing preset layouts, restoring saved positions, or animating layout changes.

### Pattern 4: Multi-Splitter Layout

Create complex resizable layouts:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition />
        <RowDefinition Height="Auto" />
        <RowDefinition />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition />
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>
    
    <!-- Panels in different grid positions -->
    <Border Grid.Row="0" Grid.Column="0" Background="LightBlue"/>
    <Border Grid.Row="2" Grid.Column="0" Background="LightGreen"/>
    <Border Grid.Row="0" Grid.Column="2" Grid.RowSpan="3" Background="LightYellow"/>
    
    <!-- Horizontal Splitter -->
    <syncfusion:SfGridSplitter Grid.Row="1" Grid.Column="0"
                               HorizontalAlignment="Stretch"
                               ResizeBehavior="PreviousAndNext"/>
    
    <!-- Vertical Splitter -->
    <syncfusion:SfGridSplitter Grid.RowSpan="3" Grid.Column="1"
                               VerticalAlignment="Stretch"
                               ResizeBehavior="PreviousAndNext"/>
</Grid>
```

**When to use:** Complex layouts with multiple independent resizable regions.

### Pattern 5: Constrained Resizing with Increments

Control resize granularity:

```xml
<syncfusion:SfGridSplitter DragIncrement="10"
                           KeyboardIncrement="20"
                           HorizontalAlignment="Stretch"
                           ResizeBehavior="PreviousAndNext"/>
```

**When to use:** Layouts that need to snap to specific sizes (e.g., aligning with grid cells, maintaining aspect ratios).

## Key Properties

| Property | Type | Purpose | When to Use |
|----------|------|---------|-------------|
| **ResizeBehavior** | ResizeBehavior | Controls which rows/columns are affected by resize | Set to PreviousAndNext (most common), PreviousAndCurrent, or CurrentAndNext based on which panels should resize |
| **EnableCollapseButton** | bool | Shows/hides collapse/expand buttons | Enable when users need to temporarily hide panels |
| **ShowsPreview** | bool | Shows resize preview before applying | Enable for heavy layouts or when users need visual confirmation |
| **DragIncrement** | double | Pixel interval for mouse-based resizing | Set to constrain resize to specific increments (default: 1) |
| **KeyboardIncrement** | double | Pixel interval for keyboard-based resizing | Set for arrow key resize granularity (default: 20) |
| **HorizontalAlignment** | HorizontalAlignment | Alignment for horizontal splitters | Use Stretch for row resizing |
| **VerticalAlignment** | VerticalAlignment | Alignment for vertical splitters | Use Stretch for column resizing |
| **Background** | Brush | Splitter bar color | Customize to match application theme |
| **Height** | double | Height of horizontal splitter bar | Set explicit height (e.g., 5-10 pixels) for horizontal splitters |
| **Width** | double | Width of vertical splitter bar | Set explicit width (e.g., 5-10 pixels) for vertical splitters |

## Common Use Cases

### Use Case 1: Code Editor with Collapsible Side Panel
**Scenario:** IDE-style layout with file tree, editor, and properties panel.

**Solution:** Vertical splitters with collapse buttons for side panels, allowing users to maximize editor space.

**Key Features:** EnableCollapseButton, custom button templates, programmatic restore.

---

### Use Case 2: Data Explorer with Adjustable Views
**Scenario:** Database tool with query editor and result grid requiring adjustable space.

**Solution:** Horizontal splitter between query and results with preview mode.

**Key Features:** ShowsPreview, ResizeBehavior, saved position restoration.

---

### Use Case 3: Master-Detail View
**Scenario:** List of items with detail panel that needs variable sizing.

**Solution:** Vertical or horizontal splitter based on layout orientation.

**Key Features:** Basic resize, DragIncrement for smooth resizing, saved preferences.

---

### Use Case 4: Multi-Document Interface
**Scenario:** Application with multiple resizable document views.

**Solution:** Grid with multiple splitters creating adjustable panes.

**Key Features:** Multiple splitters, merged cell support, complex layouts.

---

### Use Case 5: Dashboard with Resizable Widgets
**Scenario:** Analytics dashboard where users adjust widget sizes.

**Solution:** Grid layout with splitters between widget containers.

**Key Features:** DragIncrement for alignment, programmatic layouts, preset configurations.

## Implementation Checklist

Before implementing SfGridSplitter, ensure:

- [ ] **Grid Structure:** Parent Grid with appropriate row/column definitions
- [ ] **Splitter Placement:** Splitter in separate row (Height="Auto") or column (Width="Auto")
- [ ] **Alignment:** HorizontalAlignment="Stretch" for rows, VerticalAlignment="Stretch" for columns
- [ ] **Size:** Explicit Height for horizontal, Width for vertical splitters
- [ ] **ResizeBehavior:** Set based on which panels should resize
- [ ] **Assemblies:** Syncfusion.SfInput.WPF and Syncfusion.SfShared.WPF referenced
- [ ] **Namespace:** xmlns:syncfusion="http://schemas.syncfusion.com/wpf" declared

## Troubleshooting Quick Reference

**Splitter not resizing panels?**
- Verify Grid.Row/Grid.Column placement
- Check HorizontalAlignment/VerticalAlignment settings
- Ensure ResizeBehavior is set appropriately

**Collapse buttons not showing?**
- Set EnableCollapseButton="True"
- Verify splitter has sufficient size for buttons

**Preview not displaying?**
- Set ShowsPreview="True"
- Check PreviewStyle if using custom styles

**Jerky resizing?**
- Adjust DragIncrement for smoother movement
- Consider ShowsPreview for heavy layouts

Navigate to the detailed reference files above for comprehensive implementation guidance and advanced scenarios.
