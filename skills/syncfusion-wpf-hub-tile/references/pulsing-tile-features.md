# Pulsing Tile Features

`SfPulsingTile` creates a continuously animated tile — content zooms in/out and pans across X/Y axes, similar to music and video tiles in Windows Phone. Unlike `SfHubTile`, it does not flip between two views; instead it animates a single piece of content endlessly.

## Table of Contents
- [Scaling Animation](#scaling-animation)
- [Horizontal and Vertical Translation](#horizontal-and-vertical-translation)
- [Translation Duration](#translation-duration)
- [Press Animation](#press-animation)
- [OverrideDefaultStates Note](#overridedefaultstates-note)
- [MVVM Command Binding](#mvvm-command-binding)

---

## Scaling Animation

`PulseScale` sets how much the content zooms in/out during animation. A value of `1` means no visible zoom; `2` doubles the content size at peak; `3` triples it.

`PulseDuration` sets how long one full zoom cycle (in → out) takes.

```xml
<syncfusion:SfPulsingTile Header="Music"
                           Title="Now Playing"
                           Foreground="White"
                           PulseScale="2"
                           PulseDuration="00:00:03">
  <Image Source="/Assets/album.jpg"
         Stretch="None"
         VerticalAlignment="Center"
         HorizontalAlignment="Center"/>
</syncfusion:SfPulsingTile>
```

```csharp
SfPulsingTile pulsingTile = new SfPulsingTile();
pulsingTile.Header = "Music";
pulsingTile.Title = "Now Playing";
pulsingTile.Foreground = Brushes.White;

Image image = new Image
{
    Source = new BitmapImage(new Uri(@"/Assets/album.jpg", UriKind.RelativeOrAbsolute))
};
pulsingTile.Content = image;

pulsingTile.PulseScale = 2;
pulsingTile.PulseDuration = TimeSpan.FromSeconds(3.0);
grid.Children.Add(pulsingTile);
```

> **Tip:** Set content via the `Content` property in C# for `SfPulsingTile` (not `ImageSource`). `ImageSource` sets the background image, while `Content` sets the foreground element that gets animated.

---

## Horizontal and Vertical Translation

In addition to scaling, the content can also translate (pan) along the X and Y axes simultaneously.

| Property | Description |
|---|---|
| `RadiusX` | Translation range along the X-axis (pixels). Content moves left ↔ right. |
| `RadiusY` | Translation range along the Y-axis (pixels). Content moves up ↕ down. |

### Horizontal translation only

```xml
<syncfusion:SfPulsingTile Header="Music"
                           Title="Now Playing"
                           Foreground="White"
                           RadiusX="100">
  <Image Source="/Assets/album.jpg" VerticalAlignment="Center" HorizontalAlignment="Center"/>
</syncfusion:SfPulsingTile>
```

### Vertical translation only

```xml
<syncfusion:SfPulsingTile Header="Music"
                           Title="Now Playing"
                           Foreground="White"
                           RadiusY="100">
  <Image Source="/Assets/album.jpg" VerticalAlignment="Center" HorizontalAlignment="Center"/>
</syncfusion:SfPulsingTile>
```

### Combined scaling + translation (most realistic effect)

```xml
<syncfusion:SfPulsingTile Header="Music"
                           Title="Now Playing - Song Name"
                           Foreground="White"
                           PulseScale="3"
                           PulseDuration="00:00:03"
                           RadiusX="100"
                           RadiusY="100"
                           TranslateDuration="00:00:03">
  <Image Source="/Assets/album.jpg" Stretch="UniformToFill"
         HorizontalAlignment="Center" VerticalAlignment="Center"/>
</syncfusion:SfPulsingTile>
```

```csharp
pulsingTile.PulseScale = 3;
pulsingTile.PulseDuration = TimeSpan.FromSeconds(3.0);
pulsingTile.RadiusX = 100;
pulsingTile.RadiusY = 100;
pulsingTile.TranslateDuration = TimeSpan.FromSeconds(3.0);
```

---

## Translation Duration

`TranslateDuration` controls the time for one full translation cycle (left → right → left for `RadiusX`, or up → down → up for `RadiusY`).

```xml
<syncfusion:SfPulsingTile RadiusX="100"
                           RadiusY="100"
                           TranslateDuration="00:00:03"
                           Header="Music"
                           Foreground="White">
  <Image Source="/Assets/album.jpg"/>
</syncfusion:SfPulsingTile>
```

```csharp
pulsingTile.TranslateDuration = TimeSpan.FromSeconds(3.0);
```

> **Gotcha:** `TranslateDuration` affects both X and Y translation speed. There is no separate duration for each axis.

---

## Press Animation

When the user presses the tile, it zooms in/out briefly. This works identically to `SfHubTile`.

| Property | Description |
|---|---|
| `ScaleDepth` | Zoom depth at press (higher = more zoom) |
| `TilePressDuration` | Duration of the press animation |

```xml
<syncfusion:SfPulsingTile Header="Music"
                           Foreground="White"
                           PulseScale="3"
                           PulseDuration="00:00:03"
                           ScaleDepth="2"
                           TilePressDuration="00:00:03">
  <Image Source="/Assets/album.jpg" Stretch="None"/>
</syncfusion:SfPulsingTile>
```

```csharp
pulsingTile.ScaleDepth = 2;
pulsingTile.TilePressDuration = TimeSpan.FromSeconds(3.0);
```

---

## OverrideDefaultStates Note

`OverrideDefaultStates` (inherited from `HubTileBase`) must be `false` for the press animation to work. This is the default value — only change it if you are implementing fully custom visual states.

```xml
<!-- Press animation active (default) -->
<syncfusion:SfPulsingTile OverrideDefaultStates="False" .../>

<!-- Press animation disabled (custom visual states) -->
<syncfusion:SfPulsingTile OverrideDefaultStates="True" .../>
```

---

## MVVM Command Binding

`SfPulsingTile` supports `Command` and `CommandParameter` identically to `SfHubTile`.

```xml
<Window.DataContext>
  <local:ViewModel/>
</Window.DataContext>
<Grid>
  <syncfusion:SfPulsingTile x:Name="pulsingTile"
                             Header="Music"
                             Foreground="White"
                             PulseScale="3"
                             PulseDuration="00:00:03"
                             Command="{Binding PulsingTileCommand}"
                             CommandParameter="{Binding ElementName=pulsingTile}">
    <Image Source="/Assets/album.jpg" Stretch="None"/>
  </syncfusion:SfPulsingTile>
</Grid>
```

```csharp
public class ViewModel
{
    public ICommand PulsingTileCommand { get; }

    public ViewModel()
    {
        PulsingTileCommand = new RelayCommand(OnPulsingTileClicked);
    }

    private void OnPulsingTileClicked(object parameter)
    {
        MessageBox.Show("Pulsing Tile clicked!");
    }
}
```
