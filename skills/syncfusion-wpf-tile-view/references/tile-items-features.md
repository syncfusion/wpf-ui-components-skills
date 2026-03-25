# TileViewItem Features

## Table of Contents
- [Overview](#overview)
- [TileViewItem Structure](#tileviewitem-structure)
- [Header Customization](#header-customization)
- [Closable Items](#closable-items)
- [Maximizable Items](#maximizable-items)
- [Minimizable Items](#minimizable-items)
- [Item States and Events](#item-states-and-events)
- [Content Customization](#content-customization)

## Overview

[TileViewItem](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Shared.TileViewItem.html) is the fundamental container within a TileViewControl. Each tile represents a visual unit that can display content, be rearranged, maximized, minimized, and closed. Understanding TileViewItem properties is essential for creating interactive tile layouts.

## TileViewItem Structure

A TileViewItem consists of two main areas:

```
┌─────────────────────────┐
│ Header (Title Bar)      │  ← Customizable header with optional buttons
├─────────────────────────┤
│                         │
│   Content Area          │  ← Main content display area
│                         │
└─────────────────────────┘
```

### Basic Structure

```xaml
<syncfusion:TileViewItem Header="Tile Title">
    <Grid>
        <!-- Your content goes here -->
    </Grid>
</syncfusion:TileViewItem>
```

## Header Customization

The header is the title bar of each tile. You can customize it extensively.

### Simple Text Header

```xaml
<syncfusion:TileViewItem Header="My Tile Title">
    <TextBlock Text="Tile Content"/>
</syncfusion:TileViewItem>
```

### Complex Header with Template

For richer header content, use a `HeaderTemplate`:

```xaml
<syncfusion:TileViewItem>
    <syncfusion:TileViewItem.Header>
        <StackPanel Orientation="Horizontal">
            <Image Source="icon.png" Width="16" Height="16" Margin="5,0"/>
            <TextBlock Text="Dashboard" VerticalAlignment="Center"/>
        </StackPanel>
    </syncfusion:TileViewItem.Header>
    
    <Grid>
        <TextBlock Text="Dashboard Content"/>
    </Grid>
</syncfusion:TileViewItem>
```

### Header Styling

Customize header appearance with properties:

```xaml
<syncfusion:TileViewItem Header="Styled Header"
                         Foreground="White"
                         Background="#0078D4">
    <TextBlock Text="Content"/>
</syncfusion:TileViewItem>
```

## Closable Items

Allow users to close/remove tiles by clicking a close button.

### Basic Closable Tile

```xaml
<syncfusion:TileViewItem Header="Closable Tile"
                         CanClose="True"
                         Close="OnTileClose">
    <TextBlock Text="This tile can be closed"/>
</syncfusion:TileViewItem>
```

### Handle Close Event

```csharp
private void OnTileClose(object sender, RoutedEventArgs e)
{
    // Handle tile close
    var tileItem = sender as TileViewItem;
    tileViewControl.Items.Remove(tileItem);
}
```

### Close Button Styling

When `CanClose="True"`, a close button appears in the header. The button's appearance respects the header styling.

### Programmatic Close

```csharp
// Remove a tile from the control
tileViewControl.Items.Remove(tileItem);

// Or close it via the Close event
TileViewItem tile = (TileViewItem)tileViewControl.Items[0];
tile.RaiseEvent(new RoutedEventArgs(TileViewItem.CloseEvent, tile));
```

## Maximizable Items

Allow tiles to expand to fill the available space.

### Enable Maximize

```xaml
<syncfusion:TileViewItem Header="Expandable Tile"
                         CanMaximize="True"
                         MaximizedHeight="400"
                         MaximizedWidth="600">
    <Grid>
        <TextBlock Text="This tile can be maximized"/>
    </Grid>
</syncfusion:TileViewItem>
```

### Properties

| Property | Purpose |
|----------|---------|
| `CanMaximize` | Enable/disable maximize functionality |
| `MaximizedHeight` | Height when maximized (in pixels or auto) |
| `MaximizedWidth` | Width when maximized (in pixels or auto) |
| `IsMaximized` | Current maximization state |

### Programmatic Maximize

```csharp
// Maximize a tile
TileViewItem tile = (TileViewItem)tileViewControl.Items[0];
tile.IsMaximized = true;

// Restore to minimized
tile.IsMaximized = false;
```

### Maximize with Animation

Tiles automatically animate when transitioning between states when maximized/minimized.

```xaml
<syncfusion:TileViewItem Header="Animated"
                         CanMaximize="True"
                         MaximizedHeight="500"
                         MaximizedWidth="700">
    <!-- Content automatically animates during state change -->
</syncfusion:TileViewItem>
```

## Minimizable Items

Allow tiles to collapse to a smaller size.

### Enable Minimize

```xaml
<syncfusion:TileViewItem Header="Collapsible Tile"
                         CanMinimize="True"
                         MinimizedHeight="100"
                         MinimizedWidth="100">
    <Grid>
        <TextBlock Text="This tile can be minimized"/>
    </Grid>
</syncfusion:TileViewItem>
```

### Properties

| Property | Purpose |
|----------|---------|
| `CanMinimize` | Enable/disable minimize functionality |
| `MinimizedHeight` | Height when minimized (in pixels) |
| `MinimizedWidth` | Width when minimized (in pixels) |
| `IsMinimized` | Current minimization state |

### Programmatic Minimize

```csharp
// Minimize a tile
TileViewItem tile = (TileViewItem)tileViewControl.Items[0];
tile.IsMinimized = true;

// Expand back
tile.IsMinimized = false;
```

## Item States and Events

### State Management

```xaml
<syncfusion:TileViewItem x:Name="flexTile"
                         Header="Multi-State Tile"
                         CanMaximize="True"
                         CanMinimize="True"
                         CanClose="True">
    <TextBlock Text="This tile supports all operations"/>
</syncfusion:TileViewItem>
```

### Monitoring State Changes

```csharp
// Check current state
bool isMaximized = flexTile.IsMaximized;
bool isMinimized = flexTile.IsMinimized;

// React to state changes
flexTile.MaximizeChanged += (s, e) => 
{
    if (flexTile.IsMaximized)
    {
        // Handle maximize
    }
};

flexTile.MinimizeChanged += (s, e) => 
{
    if (flexTile.IsMinimized)
    {
        // Handle minimize
    }
};
```

## Content Customization

### Simple Text Content

```xaml
<syncfusion:TileViewItem Header="Simple">
    <TextBlock Text="Simple text content"/>
</syncfusion:TileViewItem>
```

### Rich UI Content

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

### Data Templating

Use templates to bind content to data:

```xaml
<syncfusion:TileViewItem>
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
<syncfusion:TileViewControl>
    <!-- Simple static tile -->
    <syncfusion:TileViewItem Header="Dashboard">
        <TextBlock Text="Dashboard Content"/>
    </syncfusion:TileViewItem>
    
    <!-- Expandable tile with close button -->
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

## Combining Features

You can combine multiple features on a single tile:

```xaml
<syncfusion:TileViewItem Header="Advanced Tile"
                         CanMaximize="True"
                         CanMinimize="True"
                         CanClose="True"
                         MaximizedHeight="500"
                         MaximizedWidth="700"
                         MinimizedHeight="120"
                         MinimizedWidth="150"
                         Foreground="White"
                         Background="#2196F3">
    <Grid>
        <!-- Feature-rich content -->
    </Grid>
</syncfusion:TileViewItem>
```

All features work seamlessly together, allowing comprehensive tile customization.
