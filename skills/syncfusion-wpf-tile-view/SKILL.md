---
name: syncfusion-wpf-tile-view
description: Create and configure WPF TileView controls for organizing and displaying content in tile layouts. Use this skill whenever users need to implement tile-based layouts, arrange tiles in matrix positions, add drag-drop functionality to tiles, maximize/minimize tile items, customize tile headers and appearance, bind data to tile views, or create responsive dashboard-like layouts. Essential for building WPF applications with interactive tiled interfaces.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# WPF TileView Implementation

The [TileViewControl](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Shared.TileViewControl.html) is a WPF container that hosts [TileViewItems](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Shared.TileViewItem.html) in a customizable matrix layout. It supports auto-arrangement, drag-drop reordering, maximize/minimize functionality, and rich customization options.

## Use Cases

- **Dashboards:** Organize information in flexible tile grids
- **Drag-drop interfaces:** Allow users to rearrange tiles
- **Expandable views:** Tiles with maximize/minimize functionality
- **Rich customization:** Branded headers, content, and styling
- **Data binding:** Dynamic tile population from collections
- **Interactive apps:** Modern, responsive WPF interfaces

## Documentation Navigation

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly installation and NuGet setup
- Adding TileViewControl via designer, XAML, and C#
- Basic configuration and initial setup
- Creating your first TileView

### TileViewItem Features
📄 **Read:** [references/tile-items-features.md](references/tile-items-features.md)
- TileViewItem structure and properties
- Header customization and styling
- Closable items with close buttons
- Maximizable and minimizable items
- TileViewItem states and behavior

### Arrangement and Layout
📄 **Read:** [references/arrangement-and-layout.md](references/arrangement-and-layout.md)
- Auto-arrangement in matrix positions
- Drag-drop support and item reordering
- BringIntoView and scroll functionality
- Positioning and layout management
- Matrix-based tile placement

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding TileView to data sources
- Item templates and data templates
- Dynamic tile creation from collections
- Master-detail binding patterns

### Appearance and Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)
- Styling and theming TileView
- Custom appearance for minimized/maximized states
- Content area customization
- Scroll bar and border styling
- Visual customization patterns

## Quick Start

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <syncfusion:TileViewControl Height="400" Width="600">
        <syncfusion:TileViewItem Header="Tile 1"><TextBlock>Content 1</TextBlock></syncfusion:TileViewItem>
        <syncfusion:TileViewItem Header="Tile 2"><TextBlock>Content 2</TextBlock></syncfusion:TileViewItem>
        <syncfusion:TileViewItem Header="Tile 3"><TextBlock>Content 3</TextBlock></syncfusion:TileViewItem>
    </syncfusion:TileViewControl>
</Window>
```

## Common Patterns

- **Basic grid:** Matrix layout with auto-arranged tiles of uniform size
- **Maximizable/minimizable:** Tiles with `CanMaximize="True"` and `CanMinimize="True"` that expand on selection
- **Closable tiles:** Set `CanClose="True"` to show close button with `Close="Handler"` event
- **Drag-drop reordering:** Set `AllowDragDrop="True"` to enable tile repositioning

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Header` | Object | Tile header content/title |
| `Content` | Object | Main tile content area |
| `CanMaximize` | bool | Enable maximize functionality |
| `CanMinimize` | bool | Enable minimize functionality |
| `CanClose` | bool | Show close button |
| `MinimizedHeight` / `MinimizedWidth` | double | Size when minimized |
| `MaximizedHeight` / `MaximizedWidth` | double | Size when maximized |
| `AllowDragDrop` | bool | Allow tile reordering |

## Next Steps

- **New to TileView?** Start with [Getting Started](references/getting-started.md)
- **Need specific item features?** See [TileViewItem Features](references/tile-items-features.md)
- **Building custom layouts?** Read [Arrangement and Layout](references/arrangement-and-layout.md)
- **Working with data?** Check [Data Binding](references/data-binding.md)
- **Styling tiles?** Explore [Appearance and Customization](references/appearance-customization.md)
