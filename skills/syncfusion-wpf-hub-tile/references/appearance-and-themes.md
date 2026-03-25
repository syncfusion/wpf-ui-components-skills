# Appearance and Themes

Customization options for `SfHubTile` and `SfPulsingTile` — header, title, secondary content styling, and theme support.

## Table of Contents
- [Customizing Header](#customizing-header)
- [Customizing Title](#customizing-title)
- [Customizing Secondary Content (Hub Tile)](#customizing-secondary-content-hub-tile)
- [Applying Built-in Themes](#applying-built-in-themes)
- [Custom Themes with ThemeStudio](#custom-themes-with-themestudio)

---

## Customizing Header

The `Header` area (bottom of tile) can be styled two ways:

### HeaderStyle — change font, size, alignment

```xml
<syncfusion:SfHubTile Header="Mail"
                      ImageSource="/Assets/mail.png"
                      Foreground="White">
  <syncfusion:SfHubTile.HeaderStyle>
    <Style TargetType="ContentControl">
      <Setter Property="VerticalAlignment" Value="Bottom"/>
      <Setter Property="FontSize" Value="18"/>
      <Setter Property="FontFamily" Value="ALGERIAN"/>
    </Style>
  </syncfusion:SfHubTile.HeaderStyle>
</syncfusion:SfHubTile>
```

### HeaderTemplate — full custom layout

Use `HeaderTemplate` when you need a logo + text combination or any complex layout in the header area:

```xml
<syncfusion:SfHubTile ImageSource="/Assets/mail.png"
                      Foreground="White">
  <syncfusion:SfHubTile.HeaderTemplate>
    <DataTemplate>
      <Grid>
        <Grid.ColumnDefinitions>
          <ColumnDefinition Width="Auto"/>
          <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Source="/Assets/logo.png" HorizontalAlignment="Left" Stretch="None"/>
        <TextBlock Text="COMPANY" Foreground="White" Grid.Column="1" FontSize="17"/>
      </Grid>
    </DataTemplate>
  </syncfusion:SfHubTile.HeaderTemplate>
</syncfusion:SfHubTile>
```

> Both `HeaderStyle` and `HeaderTemplate` work identically on `SfPulsingTile`. Replace `SfHubTile` with `SfPulsingTile` in the snippets above.

---

## Customizing Title

`TitleStyle` customizes the title area (top of tile) appearance. It supports a `ContentTemplate` setter for complex layouts:

```xml
<syncfusion:SfHubTile ImageSource="/Assets/mail.png"
                      Title="You have new mail"
                      Foreground="White">
  <syncfusion:SfHubTile.TitleStyle>
    <Style TargetType="ContentControl">
      <Setter Property="Foreground" Value="White"/>
      <Setter Property="FontSize" Value="15"/>
      <Setter Property="ContentTemplate">
        <Setter.Value>
          <DataTemplate>
            <Grid>
              <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="Auto"/>
              </Grid.ColumnDefinitions>
              <Image Source="/Assets/logo.png"
                     HorizontalAlignment="Left"
                     VerticalAlignment="Top"
                     Stretch="None"/>
              <TextBlock Text="COMPANY" FontSize="17" Grid.Column="1"/>
            </Grid>
          </DataTemplate>
        </Setter.Value>
      </Setter>
    </Style>
  </syncfusion:SfHubTile.TitleStyle>
</syncfusion:SfHubTile>
```

> `TitleStyle` works the same way on `SfPulsingTile`.

---

## Customizing Secondary Content (Hub Tile)

`SfHubTile` offers two ways to customize the secondary content view.

### SecondaryContent — set directly

Assign any UIElement to `SecondaryContent` for the alternate view shown during transitions:

```xml
<syncfusion:SfHubTile Header="Mail"
                      ImageSource="/Assets/mail.png"
                      Foreground="White"
                      Interval="00:00:03">
  <syncfusion:SfHubTile.HubTileTransitions>
    <shared:SlideTransition/>
  </syncfusion:SfHubTile.HubTileTransitions>
  <syncfusion:SfHubTile.SecondaryContent>
    <Image Source="/Assets/preview.png" Stretch="Fill" ToolTip="Preview" Margin="-1"/>
  </syncfusion:SfHubTile.SecondaryContent>
</syncfusion:SfHubTile>
```

### SecondaryContentTemplate — DataTemplate approach

Use `SecondaryContentTemplate` when you need a more complex secondary view with multiple elements:

```xml
<syncfusion:SfHubTile Header="Mail"
                      ImageSource="/Assets/mail.png"
                      Foreground="White"
                      Height="200"
                      Interval="00:00:03">
  <syncfusion:SfHubTile.HubTileTransitions>
    <shared:SlideTransition/>
  </syncfusion:SfHubTile.HubTileTransitions>
  <syncfusion:SfHubTile.SecondaryContentTemplate>
    <DataTemplate>
      <Grid>
        <Grid.RowDefinitions>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Image Source="/Assets/preview.png"/>
        <CheckBox Grid.Row="1"
                  Foreground="White"
                  Content="Freeze"
                  IsChecked="{Binding ElementName=hubTile, Path=IsFrozen}"/>
        <TextBlock Grid.Row="2"
                   Text="Secondary content description"
                   Foreground="White"
                   FontSize="12"
                   TextWrapping="Wrap"
                   FontStyle="Italic"/>
      </Grid>
    </DataTemplate>
  </syncfusion:SfHubTile.SecondaryContentTemplate>
</syncfusion:SfHubTile>
```

> `SecondaryContent` and `SecondaryContentTemplate` are **SfHubTile-only** features. `SfPulsingTile` has no secondary view.

---

## Applying Built-in Themes

Both tile controls support Syncfusion's `SfSkinManager` for built-in themes.

### Apply a theme to the whole window

```xml
<Window xmlns:skin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        skin:SfSkinManager.Theme="{skin:SkinManagerExtension ThemeName=FluentDark}">
  <Grid>
    <syncfusion:SfHubTile Header="Mail" Foreground="White"/>
  </Grid>
</Window>
```

### Apply a theme to a specific control

```xml
<syncfusion:SfHubTile Header="Mail"
                      Foreground="White"
                      skin:SfSkinManager.Theme="{skin:SkinManagerExtension ThemeName=MaterialDark}"/>
```

### Available themes (examples)

- `FluentLight` / `FluentDark`
- `MaterialLight` / `MaterialDark`
- `Office2019White` / `Office2019Black` / `Office2019Colorful`
- `Windows11Light` / `Windows11Dark`

For the complete list, see the [SfSkinManager documentation](https://help.syncfusion.com/wpf/themes/skin-manager).

---

## Custom Themes with ThemeStudio

Syncfusion ThemeStudio lets you generate a custom theme package with your brand colors.

1. Open [ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio)
2. Customize colors, typography, and shapes
3. Export the theme package
4. Reference it in your app via `SfSkinManager`

```csharp
// Apply custom theme programmatically
SfSkinManager.SetTheme(this, new Theme("CustomThemeName;assembly=CustomThemeAssembly"));
```

For full ThemeStudio usage, see the [creating custom theme guide](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme).
