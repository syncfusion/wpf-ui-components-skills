# Arrangement and Layout

## Table of Contents
- [Overview](#overview)
- [Matrix Arrangement](#matrix-arrangement)
- [Drag-Drop Support](#drag-drop-support)
- [Position Management](#position-management)
- [BringIntoView](#bringintoview)
- [Scroll Bar Functionality](#scroll-bar-functionality)
- [Layout Patterns](#layout-patterns)

## Overview

TileViewControl arranges items in a matrix-based layout where tiles automatically position themselves in rows and columns. The control supports drag-drop reordering, scroll functionality, and position control.

## Matrix Arrangement

### Auto-Arrangement

By default, TileViewControl automatically arranges tiles in a matrix order:

```
┌─────┬─────┬─────┐
│ T1  │ T2  │ T3  │
├─────┼─────┼─────┤
│ T4  │ T5  │ T6  │
├─────┼─────┼─────┤
│ T7  │ T8  │     │
└─────┴─────┴─────┘
```

### Basic Matrix Layout

```xaml
<syncfusion:TileViewControl Name="tileViewControl"
                            Height="400"
                            Width="600">
    <syncfusion:TileViewItem Header="Tile 1">
        <TextBlock Text="Position (0,0)"/>
    </syncfusion:TileViewItem>
    
    <syncfusion:TileViewItem Header="Tile 2">
        <TextBlock Text="Position (0,1)"/>
    </syncfusion:TileViewItem>
    
    <syncfusion:TileViewItem Header="Tile 3">
        <TextBlock Text="Position (0,2)"/>
    </syncfusion:TileViewItem>
    
    <!-- More tiles continue down and across -->
</syncfusion:TileViewControl>
```

### Matrix Properties

The control respects the size of minimized/maximized tiles to determine matrix positions:

```xaml
<syncfusion:TileViewItem Header="Tile 1"
                         MinimizedHeight="100"
                         MinimizedWidth="100"/>

<syncfusion:TileViewItem Header="Tile 2"
                         MinimizedHeight="200"
                         MinimizedWidth="100"/>
```

The larger Tile 2 occupies more space in the matrix.

## Drag-Drop Support

### Enable Drag-Drop

To allow users to rearrange tiles, enable drag-drop:

```csharp
tileViewControl.AllowDragDrop = true;
```

Or in XAML:

```xaml
<syncfusion:TileViewControl AllowDragDrop="True">
    <!-- TileViewItems -->
</syncfusion:TileViewControl>
```

### Drag-Drop Behavior

When `AllowDragDrop="True"`:
- Users can click and drag tiles to new positions
- Tiles automatically reorder in the matrix
- Other tiles shift to accommodate the new position
- Drop location is visual and immediate

### Example: Drag-Drop Dashboard

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:TileViewControl AllowDragDrop="True"
                                    Height="400"
                                    Width="600">
            <syncfusion:TileViewItem Header="Sales"
                                     MinimizedHeight="100"
                                     MinimizedWidth="100"
                                     CanMaximize="True">
                <TextBlock Text="Sales Metrics"/>
            </syncfusion:TileViewItem>
            
            <syncfusion:TileViewItem Header="Revenue"
                                     MinimizedHeight="100"
                                     MinimizedWidth="100"
                                     CanMaximize="True">
                <TextBlock Text="Revenue Data"/>
            </syncfusion:TileViewItem>
            
            <syncfusion:TileViewItem Header="Leads"
                                     MinimizedHeight="100"
                                     MinimizedWidth="100"
                                     CanMaximize="True">
                <TextBlock Text="Lead Information"/>
            </syncfusion:TileViewItem>
        </syncfusion:TileViewControl>
    </Grid>
</Window>
```

### Disabling Drag-Drop

To prevent rearrangement:

```csharp
tileViewControl.AllowDragDrop = false;
```

## Position Management

### Getting Item Position

```csharp
// Get a specific tile
TileViewItem tile = (TileViewItem)tileViewControl.Items[0];

// Check its current position
int index = tileViewControl.Items.IndexOf(tile);
```

### Changing Item Order

```csharp
// Move tile to a different position
TileViewItem tile = (TileViewItem)tileViewControl.Items[0];
tileViewControl.Items.Remove(tile);
tileViewControl.Items.Insert(2, tile); // Insert at index 2
```

### Swapping Tiles

```csharp
// Swap position of two tiles
TileViewItem tile1 = (TileViewItem)tileViewControl.Items[0];
TileViewItem tile2 = (TileViewItem)tileViewControl.Items[1];

int index1 = tileViewControl.Items.IndexOf(tile1);
int index2 = tileViewControl.Items.IndexOf(tile2);

tileViewControl.Items.RemoveAt(index1);
tileViewControl.Items.Insert(index1, tile2);
```

### Programmatic Arrangement

```csharp
// Create a custom arrangement
var sortedItems = tileViewControl.Items.OfType<TileViewItem>()
    .OrderBy(t => t.Header.ToString())
    .ToList();

tileViewControl.Items.Clear();
foreach (var item in sortedItems)
{
    tileViewControl.Items.Add(item);
}
```

## BringIntoView

The `BringIntoView` functionality ensures a specific tile is visible and scrolled into view.

### Bring Tile Into View

```csharp
// Get a tile and bring it into view
TileViewItem tile = (TileViewItem)tileViewControl.Items[5];
tile.BringIntoView();
```

### Use Cases

**Programmatic Selection:**
```csharp
// When a tile is selected programmatically, bring it into view
void SelectTile(int index)
{
    TileViewItem tile = (TileViewItem)tileViewControl.Items[index];
    tileViewControl.SelectedItem = tile;
    tile.BringIntoView();
}
```

**Search Result Navigation:**
```csharp
// Find a tile by header and navigate to it
void NavigateToTile(string headerText)
{
    var tile = tileViewControl.Items.OfType<TileViewItem>()
        .FirstOrDefault(t => t.Header.ToString() == headerText);
    
    if (tile != null)
    {
        tile.BringIntoView();
    }
}
```

**Auto-Scroll on New Item:**
```csharp
// When adding a new tile, bring it into view
void AddNewTile()
{
    var newTile = new TileViewItem { Header = "New Tile" };
    tileViewControl.Items.Add(newTile);
    newTile.BringIntoView();
}
```

## Scroll Bar Functionality

### ScrollBar Configuration

```xaml
<syncfusion:TileViewControl>
    <!-- Scroll bars appear automatically when content exceeds control size -->
</syncfusion:TileViewControl>
```

### Scroll Properties

```csharp
// Access scroll viewer for advanced scroll control
ScrollViewer scrollViewer = tileViewControl.FindScrollViewer();

if (scrollViewer != null)
{
    // Scroll to top
    scrollViewer.ScrollToHome();
    
    // Scroll to bottom
    scrollViewer.ScrollToEnd();
    
    // Get current scroll position
    double verticalOffset = scrollViewer.VerticalOffset;
    double horizontalOffset = scrollViewer.HorizontalOffset;
}
```

### Vertical vs Horizontal Scroll

```xaml
<!-- Control with more tiles will show vertical scroll -->
<syncfusion:TileViewControl Height="400" Width="600">
    <!-- Many tiles here -->
</syncfusion:TileViewControl>
```

## Layout Patterns

### Pattern 1: Fixed Grid Layout

Create a consistent grid with same-sized tiles:

```xaml
<syncfusion:TileViewControl AllowDragDrop="True">
    <syncfusion:TileViewItem Header="Sales"
                             MinimizedHeight="150"
                             MinimizedWidth="150"/>
    <syncfusion:TileViewItem Header="Marketing"
                             MinimizedHeight="150"
                             MinimizedWidth="150"/>
    <syncfusion:TileViewItem Header="Finance"
                             MinimizedHeight="150"
                             MinimizedWidth="150"/>
    <syncfusion:TileViewItem Header="Operations"
                             MinimizedHeight="150"
                             MinimizedWidth="150"/>
</syncfusion:TileViewControl>
```

### Pattern 2: Mixed Size Layout

Combine different-sized tiles:

```xaml
<syncfusion:TileViewControl>
    <!-- Large featured tile -->
    <syncfusion:TileViewItem Header="Featured"
                             MinimizedHeight="200"
                             MinimizedWidth="200"/>
    
    <!-- Small tiles -->
    <syncfusion:TileViewItem Header="Quick Stat 1"
                             MinimizedHeight="100"
                             MinimizedWidth="100"/>
    <syncfusion:TileViewItem Header="Quick Stat 2"
                             MinimizedHeight="100"
                             MinimizedWidth="100"/>
</syncfusion:TileViewControl>
```

### Pattern 3: Responsive Dashboard

Create a responsive layout that adapts:

```csharp
public class ResponsiveTileLayout
{
    public void AdjustLayout(double controlWidth)
    {
        if (controlWidth < 600)
        {
            // Mobile: Small tiles
            SetTileSize(80, 80);
        }
        else if (controlWidth < 1200)
        {
            // Tablet: Medium tiles
            SetTileSize(120, 120);
        }
        else
        {
            // Desktop: Large tiles
            SetTileSize(150, 150);
        }
    }
    
    private void SetTileSize(double height, double width)
    {
        foreach (var item in tileViewControl.Items.OfType<TileViewItem>())
        {
            item.MinimizedHeight = height;
            item.MinimizedWidth = width;
        }
    }
}
```

### Pattern 4: Dynamic Content with Scroll

Handle many tiles with automatic scrolling:

```xaml
<syncfusion:TileViewControl Height="400" Width="800">
    <!-- 20+ tiles will automatically enable scroll -->
</syncfusion:TileViewControl>
```

```csharp
// Add many tiles programmatically
for (int i = 0; i < 20; i++)
{
    var tile = new TileViewItem 
    { 
        Header = $"Tile {i + 1}",
        MinimizedHeight = 100,
        MinimizedWidth = 100
    };
    tileViewControl.Items.Add(tile);
}
```

## Best Practices

1. **Set minimized sizes explicitly** - Helps matrix calculation
2. **Enable drag-drop for dashboards** - Improves user experience
3. **Use BringIntoView for navigation** - Ensures visibility
4. **Test with many items** - Verify scroll behavior
5. **Combine with maximize/minimize** - Creates flexible layouts
