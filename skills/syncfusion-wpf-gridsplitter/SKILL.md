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

## Quick Start

For horizontal splitters: Create Grid with RowDefinitions (*, Auto, *), place SfGridSplitter in middle row with `Height="5"` and `ResizeBehavior="PreviousAndNext"`. For vertical splitters: Create ColumnDefinitions (*, Auto, *) and set Width instead. See [Getting Started](references/getting-started.md) for complete examples.

## Common Patterns

Key patterns include collapse buttons (EnableCollapseButton), preview mode (ShowsPreview), programmatic resizing (MoveSplitter), multi-splitter layouts, and constrained resizing with DragIncrement/KeyboardIncrement. See the reference files for complete implementation examples and when-to-use guidance.

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

- **Code Editor:** IDE-style layouts with collapsible side panels using EnableCollapseButton and custom button templates.
- **Data Explorer:** Database tools with adjustable query/result views using ShowsPreview and ResizeBehavior.
- **Master-Detail:** List-detail interfaces with variable sizing using basic resize and DragIncrement.
- **Multi-Document Interface:** Multiple resizable document panes using multiple splitters and merged cell support.
- **Dashboard Widgets:** Analytics dashboards with adjustable widget containers using DragIncrement and preset configurations.

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
