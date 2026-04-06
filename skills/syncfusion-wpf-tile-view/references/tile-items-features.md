# TileViewItem Features

## Overview

[TileViewItem](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Shared.TileViewItem.html) is the fundamental container within a TileViewControl. Each tile represents a visual unit that can display content, be rearranged, maximized, minimized, and closed.

A TileViewItem consists of a header (title bar) and content area:

```xaml
<syncfusion:TileViewItem Header="Tile Title">
    <Grid>
        <!-- Your content goes here -->
    </Grid>
</syncfusion:TileViewItem>
```

## Headers

Customize headers with text, icons, and rich content.

**Simple text header:**
```xaml
<syncfusion:TileViewItem Header="My Tile Title">
    <TextBlock Text="Tile Content"/>
</syncfusion:TileViewItem>
```

**Header with icon and dynamic content:**
```xaml
<syncfusion:TileViewItem>
    <syncfusion:TileViewItem.Header>
        <StackPanel Orientation="Horizontal" Background="#0078D4" Padding="8">
            <Image Source="icon.png" Width="20" Height="20" Margin="0,0,8,0"/>
            <TextBlock Text="Dashboard" Foreground="White" FontWeight="Bold" VerticalAlignment="Center"/>
        </StackPanel>
    </syncfusion:TileViewItem.Header>
    <Grid>
        <TextBlock Text="Dashboard Content"/>
    </Grid>
</syncfusion:TileViewItem>
```

## Closable Tiles

Enable users to close tiles by setting `CanClose="True"`. A close button appears in the header.

**Enable and handle close:**
```xaml
<syncfusion:TileViewItem Header="Closable Tile"
                         CanClose="True"
                         Close="OnTileClose">
    <TextBlock Text="This tile can be closed"/>
</syncfusion:TileViewItem>
```

**Code-behind close handler:**
```csharp
private void OnTileClose(object sender, RoutedEventArgs e)
{
    var tileItem = sender as TileViewItem;
    tileViewControl.Items.Remove(tileItem);
}
```

**Programmatic close:**
```csharp
// Remove a tile
tileViewControl.Items.Remove(tileItem);

// Or trigger close event
TileViewItem tile = (TileViewItem)tileViewControl.Items[0];
tile.RaiseEvent(new RoutedEventArgs(TileViewItem.CloseEvent, tile));
```

## Tile States (Maximize/Minimize)

Tiles support maximize and minimize operations with automatic animations.

**Enable both states:**
```xaml
<syncfusion:TileViewItem Header="Multi-State Tile"
                         CanMaximize="True"
                         CanMinimize="True"
                         MaximizedHeight="400"
                         MaximizedWidth="600"
                         MinimizedHeight="100"
                         MinimizedWidth="100">
    <Grid>
        <TextBlock Text="This tile supports all operations"/>
    </Grid>
</syncfusion:TileViewItem>
```

**State properties and methods:**

| Property | Purpose |
|----------|---------|
| `CanMaximize` | Enable/disable maximize functionality |
| `CanMinimize` | Enable/disable minimize functionality |
| `MaximizedHeight` | Height when maximized (pixels or auto) |
| `MaximizedWidth` | Width when maximized (pixels or auto) |
| `MinimizedHeight` | Height when minimized (pixels) |
| `MinimizedWidth` | Width when minimized (pixels) |
| `IsMaximized` | Current maximization state (read/write) |
| `IsMinimized` | Current minimization state (read/write) |

**Programmatic state control:**
```csharp
// Check and change state
TileViewItem tile = (TileViewItem)tileViewControl.Items[0];
bool isMaximized = tile.IsMaximized;
tile.IsMaximized = true;  // Maximize
tile.IsMinimized = true;  // Minimize

// React to state changes
tile.MaximizeChanged += (s, e) => { /* Handle maximize */ };
tile.MinimizeChanged += (s, e) => { /* Handle minimize */ };
```

## Content

Display content ranging from simple text to complex data-bound layouts.

**Simple text content:**
```xaml
<syncfusion:TileViewItem Header="Simple">
    <TextBlock Text="Simple text content"/>
</syncfusion:TileViewItem>
```

**Rich UI with controls:**
```xaml
<syncfusion:TileViewItem Header="Dashboard">
    <Grid Background="White">
        <StackPanel Margin="10">
            <TextBlock Text="Welcome" FontSize="16" FontWeight="Bold"/>
            <TextBlock Text="Last updated: Today" Foreground="Gray" Margin="0,5,0,0"/>
            <Button Content="Refresh" Margin="0,10,0,0"/>
        </StackPanel>
    </Grid>
</syncfusion:TileViewItem>
```

**Data-bound content using templates:**
```xaml
<syncfusion:TileViewItem Header="Data Tile">
    <syncfusion:TileViewItem.ContentTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock Text="{Binding Title}" FontWeight="Bold"/>
                <TextBlock Text="{Binding Description}" TextWrapping="Wrap"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:TileViewItem.ContentTemplate>
</syncfusion:TileViewItem>
```

## Complete Example

```xaml
<syncfusion:TileViewControl AllowDragDrop="True">
    <!-- Simple static tile -->
    <syncfusion:TileViewItem Header="Dashboard">
        <TextBlock Text="Dashboard Content"/>
    </syncfusion:TileViewItem>
    
    <!-- Tile with all features combined -->
    <syncfusion:TileViewItem Header="Settings"
                             CanMaximize="True"
                             CanMinimize="True"
                             CanClose="True"
                             MaximizedHeight="300"
                             MaximizedWidth="400"
                             MinimizedHeight="100"
                             MinimizedWidth="100"
                             Close="OnSettingsClose">
        <Grid Background="#F5F5F5">
            <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
                <TextBlock Text="Settings Panel"/>
            </StackPanel>
        </Grid>
    </syncfusion:TileViewItem>
    
    <!-- Tile with rich header -->
    <syncfusion:TileViewItem>
        <syncfusion:TileViewItem.Header>
            <StackPanel Orientation="Horizontal">
                <Image Source="reports.png" Width="16" Height="16"/>
                <TextBlock Text="Reports" Margin="5,0"/>
            </StackPanel>
        </syncfusion:TileViewItem.Header>
        <TextBlock Text="Reports Content" VerticalAlignment="Center" HorizontalAlignment="Center"/>
    </syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

All features work seamlessly together to create interactive, customizable tile layouts.
