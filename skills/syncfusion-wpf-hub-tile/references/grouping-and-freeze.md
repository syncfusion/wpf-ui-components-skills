# Grouping and Freeze/Unfreeze

## Table of Contents
- [Grouping with GroupName](#grouping-with-groupname)
- [Data Binding Grouping (MVVM)](#data-binding-grouping-mvvm)
- [Freezing via IsFrozen Property](#freezing-via-isfrozen-property)
- [Freeze and Unfreeze via HubTileService](#freeze-and-unfreeze-via-hubtileservice)
- [Interactivity Assembly Requirement](#interactivity-assembly-requirement)

---

## Grouping with GroupName

Assign the same `GroupName` string to multiple tiles to group them together. Groups are used when you want to freeze or unfreeze all tiles in a group at once via `HubTileService`.

```xml
<WrapPanel Orientation="Horizontal">
  <syncfusion:SfHubTile x:Name="tile1"
                        GroupName="Applications"
                        Header="Mail"
                        Foreground="White"
                        ImageSource="/Assets/mail.png"
                        Interval="00:00:03">
    <syncfusion:SfHubTile.SecondaryContent>
      <Image Source="/Assets/preview.png" Stretch="UniformToFill" Margin="-1"/>
    </syncfusion:SfHubTile.SecondaryContent>
    <syncfusion:SfHubTile.HubTileTransitions>
      <shared:SlideTransition/>
    </syncfusion:SfHubTile.HubTileTransitions>
  </syncfusion:SfHubTile>

  <syncfusion:SfHubTile x:Name="tile2"
                        GroupName="Applications"
                        Header="Word"
                        Foreground="White"
                        ImageSource="/Assets/word.png"
                        Interval="00:00:03"
                        Margin="10,0,0,0">
    <syncfusion:SfHubTile.SecondaryContent>
      <Image Source="/Assets/preview2.png" Stretch="UniformToFill" Margin="-1"/>
    </syncfusion:SfHubTile.SecondaryContent>
    <syncfusion:SfHubTile.HubTileTransitions>
      <shared:SlideTransition/>
    </syncfusion:SfHubTile.HubTileTransitions>
  </syncfusion:SfHubTile>
</WrapPanel>
```

```csharp
hubTile1.GroupName = "Applications";
hubTile2.GroupName = "Applications";
hubTile3.GroupName = "Applications";
```

---

## Data Binding Grouping (MVVM)

To populate tiles from a collection in MVVM, bind an `ObservableCollection<Model>` to a `ListView` and use an `ItemTemplate` containing `SfHubTile`.

### Model

```csharp
public class TileModel
{
    public string Header { get; set; }
    public string ImageSource { get; set; }
    public TimeSpan Interval { get; set; }
}
```

### ViewModel

```csharp
public class ViewModel
{
    public ObservableCollection<TileModel> Tiles { get; } = new ObservableCollection<TileModel>();

    public ViewModel()
    {
        Tiles.Add(new TileModel { Header = "Mail",      ImageSource = @"/Assets/mail.png",      Interval = TimeSpan.FromSeconds(3) });
        Tiles.Add(new TileModel { Header = "Word",      ImageSource = @"/Assets/word.png",      Interval = TimeSpan.FromSeconds(3) });
        Tiles.Add(new TileModel { Header = "Paint",     ImageSource = @"/Assets/paint.png",     Interval = TimeSpan.FromSeconds(3) });
        Tiles.Add(new TileModel { Header = "Calculator",ImageSource = @"/Assets/calc.png",      Interval = TimeSpan.FromSeconds(3) });
    }
}
```

### XAML — ListView with ItemTemplate

```xml
<Window.DataContext>
  <local:ViewModel/>
</Window.DataContext>
<Grid>
  <ListView ItemsSource="{Binding Tiles}"
            ScrollViewer.VerticalScrollBarVisibility="Disabled"
            BorderBrush="White">
    <ListView.ItemTemplate>
      <DataTemplate>
        <syncfusion:SfHubTile Header="{Binding Header}"
                              Foreground="White"
                              ImageSource="{Binding ImageSource}"
                              Interval="{Binding Interval}"/>
      </DataTemplate>
    </ListView.ItemTemplate>
    <ListBox.ItemsPanel>
      <ItemsPanelTemplate>
        <WrapPanel Orientation="Vertical" Margin="10"/>
      </ItemsPanelTemplate>
    </ListBox.ItemsPanel>
  </ListView>
</Grid>
```

> The same pattern works with `SfPulsingTile` — replace `SfHubTile` in the `DataTemplate` and add `PulseScale`/`PulseDuration` bindings to the model.

---

## Freezing via IsFrozen Property

Set `IsFrozen="True"` to stop animations on a single tile. Set it to `false` to resume.

### XAML

```xml
<!-- Freeze a single tile -->
<syncfusion:SfHubTile x:Name="hubTile"
                      Header="Mail"
                      Foreground="White"
                      IsFrozen="True"
                      ImageSource="/Assets/mail.png"
                      Interval="00:00:03">
  <syncfusion:SfHubTile.SecondaryContent>
    <Image Source="/Assets/preview.png" Stretch="UniformToFill" Margin="-1"/>
  </syncfusion:SfHubTile.SecondaryContent>
  <syncfusion:SfHubTile.HubTileTransitions>
    <shared:SlideTransition/>
  </syncfusion:SfHubTile.HubTileTransitions>
</syncfusion:SfHubTile>
```

```csharp
// Freeze
hubTile.IsFrozen = true;

// Unfreeze
hubTile.IsFrozen = false;
```

`IsFrozen` is bindable — you can drive it from a ViewModel property for MVVM toggle scenarios:

```xml
<syncfusion:SfHubTile IsFrozen="{Binding IsTileFrozen}" .../>
```

---

## Freeze and Unfreeze via HubTileService

`HubTileService` provides static methods to freeze/unfreeze tiles after they have been loaded — either by instance or by group name. This is the right approach when you need to control multiple tiles from a button click or a trigger.

> **Prerequisite:** `HubTileService` methods must be called after tiles are loaded in the visual tree (e.g., in a `Loaded` event or a triggered action).

### Freeze/unfreeze a single tile

```csharp
using Syncfusion.Windows.Controls.Notification;

// Freeze
HubTileService.Freeze(hubTileOne);

// Unfreeze
HubTileService.UnFreeze(hubTileOne);
```

### Freeze/unfreeze an entire group by GroupName

```csharp
// Freeze all tiles with GroupName = "Applications"
HubTileService.Freeze("Applications");

// Unfreeze all tiles with GroupName = "Applications"
HubTileService.UnFreeze("Applications");
```

### Using HubTileService from a Loaded trigger (XAML)

```xml
<syncfusion:SfHubTile x:Name="hubTileThree"
                      GroupName="Applications"
                      Header="Paint"
                      Foreground="White">
  <i:Interaction.Triggers>
    <i:EventTrigger EventName="Loaded">
      <local:FreezeGroupAction/>
    </i:EventTrigger>
  </i:Interaction.Triggers>
</syncfusion:SfHubTile>
```

```csharp
public class FreezeGroupAction : TargetedTriggerAction<SfHubTile>
{
    protected override void Invoke(object parameter)
    {
        var hubTile = this.AssociatedObject as SfHubTile;
        var window = VisualUtils.FindAncestor(hubTile, typeof(MainWindow)) as MainWindow;
        if (window != null && hubTile != null)
        {
            // Freeze a single tile by reference
            HubTileService.Freeze(window.hubTileOne);

            // Freeze all tiles in the "Applications" group
            HubTileService.Freeze("Applications");
        }
    }
}
```

> The same `Freeze`/`UnFreeze` methods work identically with `SfPulsingTile`.

---

## Interactivity Assembly Requirement

Using `HubTileService` from XAML triggers requires the **System.Windows.Interactivity** assembly:

1. Add reference: `System.Windows.Interactivity`
2. Add XAML namespace:

```xml
xmlns:i="http://schemas.microsoft.com/expression/2010/interactivity"
```

3. Add C# using (for `TargetedTriggerAction<T>`):

```csharp
using System.Windows.Interactivity;
```

Alternatively, call `HubTileService` directly from C# event handlers or ViewModel commands without any interactivity dependency.
