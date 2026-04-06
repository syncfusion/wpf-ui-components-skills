# Getting Started with WPF TileView

## Overview
This guide covers the essential steps to set up and create your first WPF TileViewControl in a Visual Studio project.

## Installation

**Required assembly:** `Syncfusion.Shared.WPF`

**Install via NuGet:**
```
Install-Package Syncfusion.Shared.WPF
```

## Adding TileViewControl

**Via XAML (recommended):**
1. Add assembly reference
2. Import schema: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`
3. Add control to Grid:

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:TileViewControl Name="tileViewControl" Height="400" Width="600"/>
    </Grid>
</Window>
```

**Via Designer:**
- Drag TileViewControl from Toolbox to designer surface
- Assembly and schema automatically added

**Via C# (programmatic):**
```csharp
using Syncfusion.Windows.Shared;

TileViewControl tileViewControl = new TileViewControl { Height = 400, Width = 600 };
grid.Children.Add(tileViewControl);
```

## Basic Configuration

**Configure control properties:**
- `Height`, `Width` - Control size
- `Margin` - Spacing around control
- `Background`, `Foreground` - Colors

**Add tiles with headers and content:**
```xaml
<syncfusion:TileViewControl Height="400" Width="600" Margin="10" Background="White">
    <syncfusion:TileViewItem Header="Dashboard">
        <Grid Background="#F0F0F0">
            <TextBlock Text="Dashboard Content" VerticalAlignment="Center" HorizontalAlignment="Center"/>
        </Grid>
    </syncfusion:TileViewItem>
    
    <syncfusion:TileViewItem Header="Settings">
        <TextBlock Text="Settings Content" VerticalAlignment="Center" HorizontalAlignment="Center"/>
    </syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

**TileViewItem properties:**
- `Header` - Tile title/label
- `Content` - Main display area (any UIElement)
- Default minimized size - Auto-calculated

## Minimal Example

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf" Title="TileView" Height="500" Width="700">
    <syncfusion:TileViewControl>
        <syncfusion:TileViewItem Header="Tile 1">
            <TextBlock Text="Content 1" VerticalAlignment="Center" HorizontalAlignment="Center"/>
        </syncfusion:TileViewItem>
        <syncfusion:TileViewItem Header="Tile 2">
            <TextBlock Text="Content 2" VerticalAlignment="Center" HorizontalAlignment="Center"/>
        </syncfusion:TileViewItem>
        <syncfusion:TileViewItem Header="Tile 3">
            <TextBlock Text="Content 3" VerticalAlignment="Center" HorizontalAlignment="Center"/>
        </syncfusion:TileViewItem>
    </syncfusion:TileViewControl>
</Window>
```

## Next Steps

- **TileViewItem Features:** Customize headers, maximize/minimize, close buttons
- **Arrangement and Layout:** Enable drag-drop, matrix positioning
- **Appearance and Customization:** Styling, themes, visual effects
- **Data Binding:** Bind to collections, templates, MVVM patterns

## Troubleshooting

**Assembly not found:** Verify `Syncfusion.Shared.WPF` installed via NuGet and project references updated.

**Tiles not appearing:** Ensure TileViewControl has `Height` and `Width` set, TileViewItems have content, and schema imported correctly (`xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`).

**Designer issues:** Rebuild solution, close/reopen XAML designer, verify Visual Studio compatibility.
