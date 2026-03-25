---
name: syncfusion-wpf-hub-tile
description: Implement Syncfusion WPF Tile Controls (SfHubTile and SfPulsingTile) for animated tile UIs. Use this when building Windows-style live tiles, tile transitions, tile animations, or grouped tile layouts in WPF. Covers Slide/Fade transitions, pulsing/zooming content, tile grouping, freeze/unfreeze animations, and desktop-style live tile experiences.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF Tile Controls

Guide for implementing Syncfusion® WPF Tile Controls — **SfHubTile** and **SfPulsingTile** — which create Windows Desktop-style live tile UIs with animated transitions, content cycling, grouping, and freeze support.

## When to Use This Skill

Use this skill when you need to:
- Create **live tile UIs** similar to Windows Desktop or Windows Phone start screens
- Implement **SfHubTile** — tiles that cycle between primary and secondary content with Slide/Fade transitions
- Implement **SfPulsingTile** — tiles that zoom/pan content continuously (music/video tile style)
- Configure tile **header, title, and image** display
- Set up **transition animations** and intervals
- **Group tiles** and freeze/unfreeze animations programmatically or via `HubTileService`
- **Customize appearance** with styles, templates, and themes

## Component Overview

| Control | Style | Key Capability |
|---|---|---|
| `SfHubTile` | Live tile | Cycles between primary + secondary content via Slide/Fade transitions |
| `SfPulsingTile` | Music/video tile | Continuously zooms and pans its content using `PulseScale`, `RadiusX`, `RadiusY` |

Both inherit from `HubTileBase` and share: `Header`, `Title`, `ImageSource`, `IsFrozen`, `GroupName`, `ScaleDepth`, `TilePressDuration`, `Click`, `Command`.

**Required assemblies:**
- `Syncfusion.SfHubTile.WPF`
- `Syncfusion.SfShared.WPF`

**XAML namespace:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
xmlns:shared="clr-namespace:Syncfusion.Windows.Controls;assembly=Syncfusion.SfShared.Wpf"
```

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly references and NuGet setup
- Adding tiles via Designer, XAML, and C#
- Setting Header, Title, and ImageSource
- Basic click events and MVVM command binding
- Applying built-in themes

### Hub Tile Features
📄 **Read:** [references/hub-tile-features.md](references/hub-tile-features.md)
- Secondary content (image, text, or control)
- Slide and Fade transitions with `HubTileTransitions`
- Transition interval and `HubTileTransitionCompleted` event
- Press animation (`ScaleDepth`, `TilePressDuration`)
- MVVM command binding

### Pulsing Tile Features
📄 **Read:** [references/pulsing-tile-features.md](references/pulsing-tile-features.md)
- Scaling animation (`PulseScale`, `PulseDuration`)
- Horizontal/vertical translation (`RadiusX`, `RadiusY`, `TranslateDuration`)
- Press animation (`ScaleDepth`, `TilePressDuration`)
- `OverrideDefaultStates` note
- MVVM command binding

### Grouping & Freeze/Unfreeze
📄 **Read:** [references/grouping-and-freeze.md](references/grouping-and-freeze.md)
- `GroupName` property for tile grouping
- Data binding grouping with ListView + ItemTemplate (MVVM)
- `IsFrozen` property to freeze/unfreeze a single tile
- `HubTileService.Freeze` / `UnFreeze` by instance or group name
- `System.Windows.Interactivity` dependency notes

### Appearance & Themes
📄 **Read:** [references/appearance-and-themes.md](references/appearance-and-themes.md)
- `HeaderStyle` and `HeaderTemplate` customization
- `TitleStyle` customization
- `SecondaryContent` and `SecondaryContentTemplate` (HubTile)
- Applying built-in themes via `SfSkinManager`
- Creating custom themes with ThemeStudio

## Quick Start

### SfHubTile (live tile with transitions)
```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:shared="clr-namespace:Syncfusion.Windows.Controls;assembly=Syncfusion.SfShared.Wpf">
  <Grid>
    <syncfusion:SfHubTile Header="Mail"
                          Title="You have 5 new messages"
                          ImageSource="/Assets/mail.png"
                          Foreground="White"
                          Interval="00:00:03">
      <syncfusion:SfHubTile.HubTileTransitions>
        <shared:SlideTransition/>
      </syncfusion:SfHubTile.HubTileTransitions>
      <syncfusion:SfHubTile.SecondaryContent>
        <Image Source="/Assets/mail-preview.png" Stretch="UniformToFill" Margin="-1"/>
      </syncfusion:SfHubTile.SecondaryContent>
    </syncfusion:SfHubTile>
  </Grid>
</Window>
```

### SfPulsingTile (zoom/pan animation)
```xml
<syncfusion:SfPulsingTile Header="Music"
                           Title="Now Playing - Song Name"
                           ImageSource="/Assets/album.jpg"
                           Foreground="White"
                           PulseScale="2"
                           PulseDuration="00:00:03"
                           RadiusX="50"
                           RadiusY="50">
  <Image Source="/Assets/album.jpg" Stretch="UniformToFill"/>
</syncfusion:SfPulsingTile>
```

### C# code-behind
```csharp
using Syncfusion.Windows.Controls.Notification;

SfHubTile hubTile = new SfHubTile();
hubTile.Header = "Mail";
hubTile.Title = "You have 5 new messages";
hubTile.Foreground = Brushes.White;
hubTile.ImageSource = new BitmapImage(new Uri(@"/Assets/mail.png", UriKind.RelativeOrAbsolute));
hubTile.HubTileTransitions.Add(new SlideTransition());
hubTile.Interval = TimeSpan.FromSeconds(3.0);
grid.Children.Add(hubTile);
```

## Common Patterns

### Choose the right tile type
- **User wants live tile that cycles content** → use `SfHubTile` with `SecondaryContent` + transitions
- **User wants continuously animated background** (music/video style) → use `SfPulsingTile` with `PulseScale` + `RadiusX`/`RadiusY`
- **User wants both** → each is independent; can mix in same layout

### Freeze tile on demand
```csharp
// Freeze single tile
hubTile.IsFrozen = true;

// Freeze entire group
HubTileService.Freeze("GroupName");

// Unfreeze entire group
HubTileService.UnFreeze("GroupName");
```

### MVVM: bind tiles from a collection
Bind an `ObservableCollection<Model>` to a `ListView` and use `ItemTemplate` with `SfHubTile`. Each model exposes `Header`, `ImageSource`, and `Interval` properties. See `references/grouping-and-freeze.md` for the full MVVM example.

## Key Properties

| Property | Control | Purpose |
|---|---|---|
| `Header` | Both | Label displayed at tile bottom |
| `Title` | Both | Notification text at tile top |
| `ImageSource` | Both | Primary image path |
| `Foreground` | Both | Text color (set to White for dark tiles) |
| `IsFrozen` | Both | Stop/start animation |
| `GroupName` | Both | Group tiles for bulk freeze/unfreeze |
| `Interval` | SfHubTile | Time between transitions |
| `SecondaryContent` | SfHubTile | Content shown during transition |
| `HubTileTransitions` | SfHubTile | Transition effects (Slide, Fade) |
| `PulseScale` | SfPulsingTile | Zoom magnitude |
| `PulseDuration` | SfPulsingTile | Duration of one zoom cycle |
| `RadiusX` / `RadiusY` | SfPulsingTile | Translation range on X/Y axes |
| `TranslateDuration` | SfPulsingTile | Duration of one translation cycle |
| `ScaleDepth` | Both | Press animation zoom depth |
| `TilePressDuration` | Both | Press animation duration |
