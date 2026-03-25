# Getting Started with WPF Tile Controls

Setup and basic implementation guide for `SfHubTile` and `SfPulsingTile`.

## Table of Contents
- [Assembly Setup](#assembly-setup)
- [Adding Controls](#adding-controls)
- [Setting Header, Title, and Image](#setting-header-title-and-image)
- [Click Events and Commands](#click-events-and-commands)
- [Applying Themes](#applying-themes)

---

## Assembly Setup

### Required Assemblies
Add these references to your WPF project:
- `Syncfusion.SfHubTile.WPF`
- `Syncfusion.SfShared.WPF`

### NuGet Package
```
Install-Package Syncfusion.SfHubTile.WPF
```

### XAML Namespaces
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
<!-- Required for built-in transitions (SlideTransition, FadeTransition) -->
xmlns:shared="clr-namespace:Syncfusion.Windows.Controls;assembly=Syncfusion.SfShared.Wpf"
```

### C# Namespace
```csharp
using Syncfusion.Windows.Controls.Notification;
```

---

## Adding Controls

### Via Designer
Drag `SfHubTile` or `SfPulsingTile` from the Toolbox onto the designer canvas. The required assemblies are added automatically and this XAML is generated:

```xml
<!-- Hub Tile -->
<syncfusion:SfHubTile HorizontalAlignment="Left" VerticalAlignment="Top"/>
<!-- Pulsing Tile -->
<syncfusion:SfPulsingTile Content="SfPulsingTile" HorizontalAlignment="Left" VerticalAlignment="Top"/>
```

### Via XAML (Manual)

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:shared="clr-namespace:Syncfusion.Windows.Controls;assembly=Syncfusion.SfShared.Wpf"
        x:Class="MyApp.MainWindow"
        Title="MainWindow" Height="350" Width="525">
  <Grid>
    <!-- Hub Tile -->
    <syncfusion:SfHubTile Content="Hub Tile"/>
    <!-- Pulsing Tile -->
    <syncfusion:SfPulsingTile Content="Pulsing Tile"/>
  </Grid>
</Window>
```

### Via C#

```csharp
using Syncfusion.Windows.Controls.Notification;

// Hub Tile
SfHubTile hubTile = new SfHubTile();
grid.Children.Add(hubTile);

// Pulsing Tile
SfPulsingTile pulsingTile = new SfPulsingTile();
grid.Children.Add(pulsingTile);
```

---

## Setting Header, Title, and Image

The three core display properties shared by both tile types:

| Property | Location | Description |
|---|---|---|
| `Header` | Bottom of tile | Name/label of the tile; accepts text, image, or control |
| `Title` | Top of tile | Notification/status text; accepts text, image, or control |
| `ImageSource` | Center of tile | Primary background image path |

### Text values (most common)

```xml
<syncfusion:SfHubTile Header="Mail"
                      Title="You have 10 unread mails."
                      ImageSource="/Assets/mail.png"
                      Foreground="White"/>
```

```csharp
SfHubTile hubTile = new SfHubTile();
hubTile.Header = "Mail";
hubTile.Title = "You have 10 unread mails.";
hubTile.Foreground = Brushes.White;
hubTile.ImageSource = new BitmapImage(new Uri(@"/Assets/mail.png", UriKind.RelativeOrAbsolute));
grid.Children.Add(hubTile);
```

### Image as Header

```xml
<syncfusion:SfHubTile>
  <syncfusion:SfHubTile.Header>
    <Image Source="/Assets/logo.png" Stretch="None"/>
  </syncfusion:SfHubTile.Header>
</syncfusion:SfHubTile>
```

### Control as Header

```xml
<syncfusion:SfHubTile>
  <syncfusion:SfHubTile.Header>
    <TextBlock Text="SYNCFUSION" Foreground="White" FontSize="13"/>
  </syncfusion:SfHubTile.Header>
</syncfusion:SfHubTile>
```

> **Note:** `Title` and `Header` accept the same content types (string, image, control) for both `SfHubTile` and `SfPulsingTile`. Replace `SfHubTile` with `SfPulsingTile` in the snippets above for identical behavior.

---

## Click Events and Commands

### Click Event

The `Click` event fires whenever a tile is pressed. Use it for code-behind scenarios:

```xml
<syncfusion:SfHubTile x:Name="hubTile"
                      Header="Mail"
                      Foreground="White"
                      Title="Click me">
  <i:Interaction.Triggers>
    <i:EventTrigger EventName="Click">
      <local:TileClickAction/>
    </i:EventTrigger>
  </i:Interaction.Triggers>
</syncfusion:SfHubTile>
```

> Requires `System.Windows.Interactivity` assembly and namespace `xmlns:i="http://schemas.microsoft.com/expression/2010/interactivity"`.

### Command Binding (MVVM)

Use `Command` and `CommandParameter` instead of the `Click` event in MVVM applications:

```xml
<Window.DataContext>
  <local:ViewModel/>
</Window.DataContext>
<Grid>
  <syncfusion:SfHubTile x:Name="hubTile"
                        Header="Mail"
                        Foreground="White"
                        Command="{Binding TileClickCommand}"
                        CommandParameter="{Binding ElementName=hubTile}"/>
</Grid>
```

```csharp
public class ViewModel
{
    public ICommand TileClickCommand { get; }

    public ViewModel()
    {
        TileClickCommand = new RelayCommand(OnTileClicked);
    }

    private void OnTileClicked(object parameter)
    {
        MessageBox.Show("Tile clicked!");
    }
}
```

---

## Applying Themes

Both tile controls support Syncfusion's built-in theme system.

### Using SfSkinManager

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:skin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        skin:SfSkinManager.Theme="{skin:SkinManagerExtension ThemeName=FluentDark}">
  <Grid>
    <syncfusion:SfHubTile Header="Mail" Foreground="White"/>
  </Grid>
</Window>
```

For more details on applying themes, refer to the [Syncfusion SfSkinManager documentation](https://help.syncfusion.com/wpf/themes/skin-manager).

### Custom Themes via ThemeStudio

Use Syncfusion ThemeStudio to generate a custom theme package and apply it via `SfSkinManager`. See [ThemeStudio docs](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme).

For full appearance customization (HeaderStyle, TitleStyle, SecondaryContentTemplate), see [`references/appearance-and-themes.md`](appearance-and-themes.md).
