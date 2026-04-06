# Arrangement and Layout

TileViewControl arranges items in a matrix layout where tiles automatically position themselves in rows and columns, with support for drag-drop reordering, scrolling, and position control.

## Matrix Arrangement

TileViewControl automatically arranges tiles in a matrix order based on minimized tile sizes. Tiles are positioned row-by-row, left-to-right.

**Basic matrix layout:**
```xaml
<syncfusion:TileViewControl Height="400" Width="600">
    <syncfusion:TileViewItem Header="Tile 1"><TextBlock Text="Content"/></syncfusion:TileViewItem>
    <syncfusion:TileViewItem Header="Tile 2"><TextBlock Text="Content"/></syncfusion:TileViewItem>
    <syncfusion:TileViewItem Header="Tile 3"><TextBlock Text="Content"/></syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

**Mixed sizes:** Larger tiles occupy more matrix space:
```xaml
<syncfusion:TileViewItem Header="Small" MinimizedHeight="100" MinimizedWidth="100"/>
<syncfusion:TileViewItem Header="Large" MinimizedHeight="200" MinimizedWidth="200"/>
```

## Drag-Drop Support

Enable drag-drop to allow users to rearrange tiles interactively.

**Enable in XAML:**
```xaml
<syncfusion:TileViewControl AllowDragDrop="True">
    <!-- TileViewItems -->
</syncfusion:TileViewControl>
```

**Enable in code:**
```csharp
tileViewControl.AllowDragDrop = true;
```

**Behavior:** When enabled, users can click and drag tiles to new positions. Tiles automatically reorder in the matrix, and other tiles shift to accommodate the new position. Disable drag-drop by setting `AllowDragDrop="False"` to create a fixed layout.

## Position Management

Programmatically control tile order and positioning.

**Get and change position:**
```csharp
// Get a specific tile
TileViewItem tile = (TileViewItem)tileViewControl.Items[0];

// Get current position
int index = tileViewControl.Items.IndexOf(tile);

// Move to a different position
tileViewControl.Items.Remove(tile);
tileViewControl.Items.Insert(2, tile);
```

**Rearrange tiles by sorting:**
```csharp
// Sort tiles by header text
var sortedItems = tileViewControl.Items.OfType<TileViewItem>()
    .OrderBy(t => t.Header.ToString())
    .ToList();

tileViewControl.Items.Clear();
foreach (var item in sortedItems)
{
    tileViewControl.Items.Add(item);
}
```

## Visibility and Navigation

Ensure tiles are visible in the viewport using `BringIntoView()`.

**Bring tile into view:**
```csharp
// Bring a specific tile into the visible area
TileViewItem tile = (TileViewItem)tileViewControl.Items[5];
tile.BringIntoView();

// Use cases: programmatic selection, search result navigation, new items
void SelectAndNavigateTo(int index)
{
    TileViewItem tile = (TileViewItem)tileViewControl.Items[index];
    tileViewControl.SelectedItem = tile;
    tile.BringIntoView();
}
```

## Scrolling

Scroll bars appear automatically when tiles exceed the control size. Access and control scrolling programmatically:

```csharp
ScrollViewer scrollViewer = tileViewControl.FindScrollViewer();
if (scrollViewer != null)
{
    // Scroll to home/end
    scrollViewer.ScrollToHome();
    scrollViewer.ScrollToEnd();
    
    // Get current position
    double verticalOffset = scrollViewer.VerticalOffset;
}
```

## Layout Patterns

**Fixed grid (same-sized tiles):**
```xaml
<syncfusion:TileViewControl AllowDragDrop="True">
    <syncfusion:TileViewItem Header="Sales" MinimizedHeight="150" MinimizedWidth="150"/>
    <syncfusion:TileViewItem Header="Marketing" MinimizedHeight="150" MinimizedWidth="150"/>
    <syncfusion:TileViewItem Header="Finance" MinimizedHeight="150" MinimizedWidth="150"/>
</syncfusion:TileViewControl>
```

**Mixed sizes (featured + quick stats):**
```xaml
<syncfusion:TileViewControl>
    <syncfusion:TileViewItem Header="Featured" MinimizedHeight="200" MinimizedWidth="200"/>
    <syncfusion:TileViewItem Header="Stat 1" MinimizedHeight="100" MinimizedWidth="100"/>
    <syncfusion:TileViewItem Header="Stat 2" MinimizedHeight="100" MinimizedWidth="100"/>
</syncfusion:TileViewControl>
```

**Responsive layout (adjust by screen width):**
```csharp
void AdjustLayout(double controlWidth)
{
    double size = controlWidth < 600 ? 80 : controlWidth < 1200 ? 120 : 150;
    foreach (var item in tileViewControl.Items.OfType<TileViewItem>())
    {
        item.MinimizedHeight = size;
        item.MinimizedWidth = size;
    }
}
```

**Dynamic content (many tiles with auto-scroll):**
```csharp
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
