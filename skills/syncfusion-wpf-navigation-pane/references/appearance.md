# Appearance

## Table of Contents
- [GroupBar Header Style](#groupbar-header-style)
- [Collapse Button Customization](#collapse-button-customization)
- [ItemContainerStyle](#itemcontainerstyle)
- [ItemContainerStyleSelector](#itemcontainerstyleselector)
- [Drag Marker Color](#drag-marker-color)
- [Themes with SfSkinManager](#themes-with-sfskinmanager)
- [Custom Themes with ThemeStudio](#custom-themes-with-themestudio)
- [Gotchas](#gotchas)

---

## GroupBar Header Style

Customize the header border of the GroupBar using `GroupBarHeaderStyle`. The target type is `Border`:

```xaml
<Window.Resources>
    <Style x:Key="customHeader" TargetType="{x:Type Border}">
        <Setter Property="CornerRadius" Value="7"/>
        <Setter Property="Background"   Value="Pink"/>
        <Setter Property="Height"       Value="30"/>
    </Style>
</Window.Resources>

<syncfusion:GroupBar GroupBarHeaderStyle="{StaticResource customHeader}"
                     Height="300" Width="230" VisualMode="StackMode">
    <syncfusion:GroupBarItem Header="Mailbox"/>
    <syncfusion:GroupBarItem Header="Contacts"/>
</syncfusion:GroupBar>
```

---

## Collapse Button Customization

### Template

Replace the collapse button with a custom `ControlTemplate` (target type `ToggleButton`):

```xaml
<syncfusion:GroupBar AllowCollapse="True" VisualMode="StackMode" Height="300" Width="230">
    <syncfusion:GroupBar.CollapseButtonTemplate>
        <ControlTemplate TargetType="{x:Type ToggleButton}">
            <Border Height="16" Width="16">
                <Path Name="arrow" Data="M0,0 L3.5,4 7,0z"
                      Fill="DarkBlue"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center"/>
            </Border>
            <ControlTemplate.Triggers>
                <Trigger Property="IsChecked" Value="True">
                    <Setter TargetName="arrow" Property="Data" Value="M3,1 L7,5 3,9z"/>
                </Trigger>
            </ControlTemplate.Triggers>
        </ControlTemplate>
    </syncfusion:GroupBar.CollapseButtonTemplate>
    <syncfusion:GroupBarItem Header="Mailbox"/>
</syncfusion:GroupBar>
```

### Background Colors

```xaml
<syncfusion:GroupBar AllowCollapse="True"
                     CollapseButtonBackground="AliceBlue"
                     CollapseButtonMouseOverBackground="LightSteelBlue"
                     VisualMode="StackMode"
                     Height="300" Width="230">
    <syncfusion:GroupBarItem Header="Mailbox"/>
</syncfusion:GroupBar>
```

```csharp
groupBar.CollapseButtonBackground          = Brushes.AliceBlue;
groupBar.CollapseButtonMouseOverBackground = Brushes.LightSteelBlue;
```

---

## ItemContainerStyle

Apply a consistent style to all `GroupBarItem` elements using `ItemContainerStyle`. Most useful with data binding to customize `HeaderTemplate` and `ContentTemplate`:

```xaml
<Window.Resources>
    <Style TargetType="{x:Type syncfusion:GroupBarItem}" x:Key="GroupBarItemStyle">
        <Setter Property="HeaderTemplate">
            <Setter.Value>
                <DataTemplate>
                    <TextBlock Text="{Binding XPath=@Name}"
                               Foreground="DarkGreen"
                               FontWeight="Bold"
                               FontFamily="Segoe UI"/>
                </DataTemplate>
            </Setter.Value>
        </Setter>
        <Setter Property="ContentTemplate">
            <Setter.Value>
                <DataTemplate>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="4*"/>
                            <ColumnDefinition Width="6*"/>
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding XPath=@ImagePath}"/>
                        <TextBlock Text="{Binding XPath=@Description}"
                                   TextWrapping="Wrap"
                                   Grid.Column="1"/>
                    </Grid>
                </DataTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<syncfusion:GroupBar ItemContainerStyle="{StaticResource GroupBarItemStyle}"
                     ItemsSource="{Binding Source={StaticResource xmlSource}, XPath=Book}"
                     AllowCollapse="True"
                     VisualMode="StackMode"/>
```

---

## ItemContainerStyleSelector

Apply different styles to different items based on data at runtime:

```csharp
public class GroupBarStyleSelector : StyleSelector
{
    public Style WpfStyle { get; set; }
    public Style CsStyle  { get; set; }

    public override Style SelectStyle(object item, DependencyObject container)
    {
        var element = item as System.Xml.XmlElement;
        string name = element?.GetAttribute("Name").ToLower() ?? "";
        return name.Contains("wpf") ? WpfStyle : CsStyle;
    }
}
```

```xaml
<local:GroupBarStyleSelector x:Key="groupBarStyleSelector"
    WpfStyle="{StaticResource WpfItemStyle}"
    CsStyle="{StaticResource CsItemStyle}"/>

<syncfusion:GroupBar ItemContainerStyleSelector="{StaticResource groupBarStyleSelector}"
                     ItemsSource="{Binding Source={StaticResource xmlSource}, XPath=Book}"
                     VisualMode="StackMode"/>
```

---

## Drag Marker Color

Customize the insertion marker shown while dragging items:

```xaml
<syncfusion:GroupBar DragItemVisibility="True"
                     DragMarkerBrush="OrangeRed"
                     Height="200" Width="230">
    <syncfusion:GroupBarItem Header="Item 1"/>
    <syncfusion:GroupBarItem Header="Item 2"/>
    <syncfusion:GroupBarItem Header="Item 3"/>
</syncfusion:GroupBar>
```

```csharp
groupBar.DragMarkerBrush = Brushes.OrangeRed;
```

---

## Themes with SfSkinManager

Apply built-in Syncfusion themes:

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<syncfusion:GroupBar
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentLight}"
    VisualMode="StackMode"
    Height="300" Width="230">
    <syncfusion:GroupBarItem Header="Mailbox"/>
</syncfusion:GroupBar>
```

Apply to entire window:
```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
```

**Available themes:**

| Theme Name | Style |
|---|---|
| `FluentLight` | Windows 11 Fluent light |
| `FluentDark` | Windows 11 Fluent dark |
| `MaterialLight` | Material Design light |
| `MaterialDark` | Material Design dark |
| `Office2019Colorful` | Office 2019 colorful |
| `Office2019Black` | Office 2019 black |
| `Windows11Light` | Windows 11 light |
| `Windows11Dark` | Windows 11 dark |
| `SystemTheme` | Matches OS system theme |

> ⚠️ **StackMode layout changes by theme:** ThemeStudio themes arrange StackMode items **horizontally**; classic themes arrange them **vertically**.

---

## Custom Themes with ThemeStudio

1. Open [Syncfusion ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)
2. Select a base theme and customize colors, fonts, borders
3. Export the generated resource dictionary
4. Reference in `App.xaml` or window resources

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `GroupBarHeaderStyle` has no effect | `TargetType` is wrong type | Ensure `TargetType="{x:Type Border}"` |
| Collapse button not visible | `AllowCollapse` is `false` | Set `AllowCollapse="True"` |
| `ItemContainerStyle` overrides binding | Style `Content` setter conflicts with model binding | Remove the `Content` setter if `ContentTemplate` is used |
| Theme has no effect | `SfSkinManager` assembly missing | Add `Syncfusion.SfSkinManager.WPF` reference |
| StackMode items suddenly horizontal | Switched to ThemeStudio theme | Expected behavior — ThemeStudio uses horizontal StackMode layout |
