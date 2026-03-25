# Hub Tile Features

## Table of Contents
- [Secondary Content](#secondary-content)
- [Transitions](#transitions)
- [Transition Interval](#transition-interval)
- [Transition Completed Event](#transition-completed-event)
- [Press Animation](#press-animation)
- [MVVM Command Binding](#mvvm-command-binding)

---

## Secondary Content

`SecondaryContent` is the content displayed when the tile transitions away from its primary view. It can be an image, text string, or any control.

When the `Interval` elapses, the tile switches from the primary view (showing `ImageSource` + `Title`) to the secondary content using the configured transition.

### Image as secondary content

```xml
<syncfusion:SfHubTile Header="Mail"
                      Title="This is title area."
                      ImageSource="/Assets/mail.png"
                      Foreground="White"
                      Interval="00:00:03">
  <syncfusion:SfHubTile.HubTileTransitions>
    <shared:SlideTransition/>
  </syncfusion:SfHubTile.HubTileTransitions>
  <syncfusion:SfHubTile.SecondaryContent>
    <Image Source="/Assets/preview.png" Stretch="UniformToFill" Margin="-1"/>
  </syncfusion:SfHubTile.SecondaryContent>
</syncfusion:SfHubTile>
```

### Text as secondary content

```xml
<syncfusion:SfHubTile Header="Mail"
                      ImageSource="/Assets/mail.png"
                      Foreground="White"
                      Interval="00:00:03"
                      SecondaryContent="This is the secondary content.">
  <syncfusion:SfHubTile.HubTileTransitions>
    <shared:SlideTransition/>
  </syncfusion:SfHubTile.HubTileTransitions>
</syncfusion:SfHubTile>
```

### Control as secondary content

```xml
<syncfusion:SfHubTile.SecondaryContent>
  <TextBlock Text="New notification arrived"
             TextWrapping="Wrap"
             FontSize="15"
             Foreground="White"/>
</syncfusion:SfHubTile.SecondaryContent>
```

### C# equivalent

```csharp
SfHubTile hubTile = new SfHubTile();
hubTile.Header = "Mail";
hubTile.Title = "This is title area.";
hubTile.Foreground = Brushes.White;
hubTile.ImageSource = new BitmapImage(new Uri(@"/Assets/mail.png", UriKind.RelativeOrAbsolute));
hubTile.HubTileTransitions.Add(new SlideTransition());
hubTile.Interval = TimeSpan.FromSeconds(3.0);

// Image secondary content
Image secondaryImage = new Image
{
    Source = new BitmapImage(new Uri(@"/Assets/preview.png", UriKind.RelativeOrAbsolute))
};
hubTile.SecondaryContent = secondaryImage;

// OR: text secondary content
hubTile.SecondaryContent = "This is the secondary content.";

grid.Children.Add(hubTile);
```

> **Note:** `Syncfusion.SfShared.Wpf` assembly must be referenced for `SlideTransition` and `FadeTransition` in XAML. In C#, add `using Syncfusion.Windows.Controls;`.

---

## Transitions

The `HubTileTransitions` collection defines how the tile animates between primary and secondary content. Two built-in transitions are available.

### SlideTransition — content slides in from the side

```xml
<syncfusion:SfHubTile.HubTileTransitions>
  <shared:SlideTransition/>
</syncfusion:SfHubTile.HubTileTransitions>
```

```csharp
hubTile.HubTileTransitions.Add(new SlideTransition());
```

### FadeTransition — content fades between views

```xml
<syncfusion:SfHubTile.HubTileTransitions>
  <shared:FadeTransition/>
</syncfusion:SfHubTile.HubTileTransitions>
```

```csharp
hubTile.HubTileTransitions.Add(new FadeTransition());
```

### Multiple transitions

You can add both to the collection; they will alternate:

```xml
<syncfusion:SfHubTile.HubTileTransitions>
  <shared:SlideTransition/>
  <shared:FadeTransition/>
</syncfusion:SfHubTile.HubTileTransitions>
```

> **Gotcha:** If you don't add any transition to `HubTileTransitions`, the tile will still cycle content but without animation. Always add at least one transition for visible effect.

---

## Transition Interval

`Interval` controls how often the tile cycles between primary and secondary content. It accepts a `TimeSpan` value.

```xml
<!-- Transition every 3 seconds -->
<syncfusion:SfHubTile Interval="00:00:03" .../>

<!-- Transition every 5 seconds -->
<syncfusion:SfHubTile Interval="00:00:05" .../>
```

```csharp
hubTile.Interval = TimeSpan.FromSeconds(3.0);
```

---

## Transition Completed Event

`HubTileTransitionCompleted` fires after each transition cycle completes. Use it to update content dynamically or respond to each flip.

```xml
<syncfusion:SfHubTile x:Name="hubTile"
                      Header="Mail"
                      Foreground="White"
                      Interval="00:00:03">
  <syncfusion:SfHubTile.HubTileTransitions>
    <shared:SlideTransition/>
  </syncfusion:SfHubTile.HubTileTransitions>
  <i:Interaction.Triggers>
    <i:EventTrigger EventName="HubTileTransitionCompleted">
      <local:TransitionCompletedAction/>
    </i:EventTrigger>
  </i:Interaction.Triggers>
</syncfusion:SfHubTile>
```

```csharp
// Example: change background color on each transition
public class TransitionCompletedAction : TargetedTriggerAction<SfHubTile>
{
    protected override void Invoke(object parameter)
    {
        var hubTile = this.AssociatedObject as SfHubTile;
        if (hubTile != null)
        {
            hubTile.Background = Brushes.Green;
        }
    }
}
```

---

## Press Animation

When a user presses the center of the tile, it zooms in/out briefly — a tactile feedback effect. Two properties control this:

| Property | Type | Description |
|---|---|---|
| `ScaleDepth` | `double` | Depth of the zoom effect when pressed (higher = more zoom) |
| `TilePressDuration` | `TimeSpan` | Duration of the press animation |

```xml
<syncfusion:SfHubTile Header="Mail"
                      Foreground="White"
                      ImageSource="/Assets/mail.png"
                      Interval="00:00:03"
                      ScaleDepth="2"
                      TilePressDuration="00:00:02">
  <syncfusion:SfHubTile.HubTileTransitions>
    <shared:SlideTransition/>
  </syncfusion:SfHubTile.HubTileTransitions>
  <syncfusion:SfHubTile.SecondaryContent>
    <Image Source="/Assets/preview.png" Stretch="UniformToFill"/>
  </syncfusion:SfHubTile.SecondaryContent>
</syncfusion:SfHubTile>
```

```csharp
hubTile.ScaleDepth = 2;
hubTile.TilePressDuration = TimeSpan.FromSeconds(2.0);
```

> **Important:** Press animation only works when `OverrideDefaultStates` is `false` (the default). If you've set it to `true` for custom visual states, press animation will not play.

---

## MVVM Command Binding

Use `Command` and `CommandParameter` instead of the `Click` event in MVVM applications. `CommandParameter` is typically bound to the tile itself for context.

```xml
<Window.DataContext>
  <local:ViewModel/>
</Window.DataContext>
<Grid>
  <syncfusion:SfHubTile x:Name="hubTile"
                        Header="Mail"
                        Foreground="White"
                        Title="Inbox"
                        ImageSource="/Assets/mail.png"
                        Command="{Binding HubTileCommand}"
                        CommandParameter="{Binding ElementName=hubTile}"/>
</Grid>
```

```csharp
public class ViewModel
{
    public ICommand HubTileCommand { get; }

    public ViewModel()
    {
        HubTileCommand = new RelayCommand<SfHubTile>(OnHubTileClicked);
    }

    private void OnHubTileClicked(SfHubTile tile)
    {
        MessageBox.Show($"Hub Tile '{tile?.Header}' clicked");
    }
}
```
