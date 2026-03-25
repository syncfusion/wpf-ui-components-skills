# Appearance and Customization

## Table of Contents
- [Overview](#overview)
- [Styling Tiles](#styling-tiles)
- [Header Styling](#header-styling)
- [Content Styling](#content-styling)
- [State-Specific Styling](#state-specific-styling)
- [Themes and Colors](#themes-and-colors)
- [Visual Effects](#visual-effects)
- [Scroll Bar Styling](#scroll-bar-styling)

## Overview

TileViewControl offers extensive customization options for appearance, including styling headers, content areas, borders, backgrounds, and state-specific presentations for minimized and maximized tiles.

## Styling Tiles

### Basic Tile Styling

```xaml
<syncfusion:TileViewItem Header="Styled Tile"
                         Background="White"
                         BorderBrush="#CCCCCC"
                         BorderThickness="1"
                         Foreground="#333333">
    <TextBlock Text="Tile content"/>
</syncfusion:TileViewItem>
```

### Tile Styling Properties

| Property | Effect |
|----------|--------|
| `Background` | Tile background color |
| `Foreground` | Text color |
| `BorderBrush` | Border line color |
| `BorderThickness` | Border width (all sides or individual) |
| `Margin` | Space outside the tile |
| `Padding` | Space inside the tile |
| `CornerRadius` | Rounded corners (via Style) |

### Colored Tiles

```xaml
<syncfusion:TileViewItem Header="Blue Tile"
                         Background="#E3F2FD"
                         Foreground="#1565C0">
    <TextBlock Text="Content"/>
</syncfusion:TileViewItem>

<syncfusion:TileViewItem Header="Green Tile"
                         Background="#E8F5E9"
                         Foreground="#2E7D32">
    <TextBlock Text="Content"/>
</syncfusion:TileViewItem>

<syncfusion:TileViewItem Header="Red Tile"
                         Background="#FFEBEE"
                         Foreground="#C62828">
    <TextBlock Text="Content"/>
</syncfusion:TileViewItem>
```

### Tile with Border and Shadow

```xaml
<syncfusion:TileViewItem Header="Bordered Tile"
                         Background="White"
                         BorderBrush="#0078D4"
                         BorderThickness="2">
    <TextBlock Text="Content" Margin="10"/>
</syncfusion:TileViewItem>
```

## Header Styling

### Basic Header Styling

```xaml
<syncfusion:TileViewItem Header="Custom Header"
                         Background="#0078D4"
                         Foreground="White">
    <TextBlock Text="Content"/>
</syncfusion:TileViewItem>
```

### Header with Icon and Text

```xaml
<syncfusion:TileViewItem>
    <syncfusion:TileViewItem.Header>
        <StackPanel Orientation="Horizontal" Background="#0078D4" Padding="8">
            <Image Source="icon.png" Width="20" Height="20" Margin="0,0,8,0"/>
            <TextBlock Text="Dashboard" 
                       Foreground="White" 
                       FontWeight="Bold"
                       VerticalAlignment="Center"/>
        </StackPanel>
    </syncfusion:TileViewItem.Header>
    
    <TextBlock Text="Content"/>
</syncfusion:TileViewItem>
```

### Styled Header Template

```xaml
<syncfusion:TileViewControl>
    <syncfusion:TileViewControl.ItemTemplate>
        <DataTemplate>
            <!-- Content template -->
        </DataTemplate>
    </syncfusion:TileViewControl.ItemTemplate>
    
    <syncfusion:TileViewItem>
        <syncfusion:TileViewItem.Header>
            <Border Background="#2196F3" Padding="10,5">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Text="{Binding Title}"
                               Foreground="White"
                               FontWeight="Bold"
                               FontSize="12"/>
                    <TextBlock Text="●"
                               Foreground="#90CAF9"
                               Margin="8,0,0,0"
                               FontSize="8"
                               VerticalAlignment="Center"/>
                </StackPanel>
            </Border>
        </syncfusion:TileViewItem.Header>
    </syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

## Content Styling

### Rich Content Area

```xaml
<syncfusion:TileViewItem Header="Dashboard">
    <Grid Background="White">
        <StackPanel Margin="15">
            <TextBlock Text="Key Metric"
                       FontSize="14"
                       FontWeight="Bold"
                       Foreground="#333"/>
            
            <TextBlock Text="$125,430"
                       FontSize="32"
                       FontWeight="Bold"
                       Foreground="#0078D4"
                       Margin="0,5,0,0"/>
            
            <TextBlock Text="↑ 12% from last month"
                       FontSize="12"
                       Foreground="#2E7D32"
                       Margin="0,3,0,0"/>
        </StackPanel>
    </Grid>
</syncfusion:TileViewItem>
```

### Content with Images

```xaml
<syncfusion:TileViewItem Header="Image Tile">
    <Grid>
        <Image Source="background.jpg" Stretch="UniformToFill"/>
        <StackPanel Background="#00000080" VerticalAlignment="Bottom">
            <TextBlock Text="Overlay Content"
                       Foreground="White"
                       Margin="10"/>
        </StackPanel>
    </Grid>
</syncfusion:TileViewItem>
```

## State-Specific Styling

### Minimized State Appearance

```xaml
<syncfusion:TileViewItem Header="Minimized View"
                         MinimizedHeight="120"
                         MinimizedWidth="150">
    <Grid Background="#F5F5F5">
        <TextBlock Text="Minimized" VerticalAlignment="Center" HorizontalAlignment="Center"/>
    </Grid>
</syncfusion:TileViewItem>
```

### Maximized State Appearance

```xaml
<syncfusion:TileViewItem Header="Maximized View"
                         CanMaximize="True"
                         MaximizedHeight="400"
                         MaximizedWidth="600">
    <Grid Background="White">
        <!-- Detailed content that shows when maximized -->
        <StackPanel Margin="20">
            <TextBlock Text="Expanded Content" FontSize="18" FontWeight="Bold"/>
            <TextBlock Text="This detailed view appears when the tile is maximized." Margin="0,10,0,0" TextWrapping="Wrap"/>
        </StackPanel>
    </Grid>
</syncfusion:TileViewItem>
```

### Transition States

Tiles automatically animate when transitioning between minimized and maximized states.

## Themes and Colors

### Color Palette

Create a consistent color scheme:

```csharp
public static class TileTheme
{
    public static readonly Color PrimaryBlue = Color.FromArgb(255, 0, 120, 212);
    public static readonly Color AccentGreen = Color.FromArgb(255, 46, 125, 50);
    public static readonly Color WarningOrange = Color.FromArgb(255, 255, 152, 0);
    public static readonly Color DangerRed = Color.FromArgb(255, 198, 40, 40);
    public static readonly Color LightGray = Color.FromArgb(255, 245, 245, 245);
    public static readonly Color DarkGray = Color.FromArgb(255, 51, 51, 51);
}
```

### Apply Theme

```xaml
<syncfusion:TileViewItem Header="Theme Applied"
                         Background="{Binding Source={x:Static local:TileTheme.LightGray}}"
                         Foreground="{Binding Source={x:Static local:TileTheme.DarkGray}}">
    <TextBlock Text="Content"/>
</syncfusion:TileViewItem>
```

### Dark Theme

```xaml
<syncfusion:TileViewControl Background="#1E1E1E">
    <syncfusion:TileViewItem Header="Dark Tile"
                             Background="#2D2D2D"
                             Foreground="#FFFFFF">
        <TextBlock Text="Content on dark background"/>
    </syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

### Light Theme

```xaml
<syncfusion:TileViewControl Background="#FFFFFF">
    <syncfusion:TileViewItem Header="Light Tile"
                             Background="#F5F5F5"
                             Foreground="#333333">
        <TextBlock Text="Content on light background"/>
    </syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

## Visual Effects

### Gradient Background

```xaml
<syncfusion:TileViewItem Header="Gradient Tile">
    <Grid>
        <Grid.Background>
            <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                <GradientStop Color="#0078D4" Offset="0"/>
                <GradientStop Color="#00B4D8" Offset="1"/>
            </LinearGradientBrush>
        </Grid.Background>
        <TextBlock Text="Gradient Content" Foreground="White" VerticalAlignment="Center" HorizontalAlignment="Center"/>
    </Grid>
</syncfusion:TileViewItem>
```

### Opacity and Transparency

```xaml
<syncfusion:TileViewItem Header="Transparent Tile"
                         Opacity="0.8">
    <TextBlock Text="Content with opacity"/>
</syncfusion:TileViewItem>
```

### Rounded Corners (via Style)

```xaml
<Window.Resources>
    <Style TargetType="syncfusion:TileViewItem">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="syncfusion:TileViewItem">
                    <Border CornerRadius="8" 
                            Background="{TemplateBinding Background}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}">
                        <ContentPresenter/>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<syncfusion:TileViewItem Header="Rounded Tile">
    <TextBlock Text="Content"/>
</syncfusion:TileViewItem>
```

## Scroll Bar Styling

### ScrollViewer Styling

```xaml
<Window.Resources>
    <Style TargetType="ScrollBar">
        <Setter Property="Background" Value="#F5F5F5"/>
        <Setter Property="Foreground" Value="#0078D4"/>
    </Style>
</Window.Resources>

<syncfusion:TileViewControl>
    <!-- Scroll bars use the styled appearance -->
</syncfusion:TileViewControl>
```

### Hide Scroll Bars

```csharp
// In code-behind
ScrollViewer scrollViewer = tileViewControl.FindScrollViewer();
if (scrollViewer != null)
{
    scrollViewer.HorizontalScrollBarVisibility = ScrollBarVisibility.Hidden;
    scrollViewer.VerticalScrollBarVisibility = ScrollBarVisibility.Hidden;
}
```

## Complete Customization Example

```xaml
<Window x:Class="CustomTileApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Customized TileView" Height="600" Width="900">
    
    <Window.Resources>
        <LinearGradientBrush x:Key="PrimaryGradient" StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#0078D4" Offset="0"/>
            <GradientStop Color="#00B4D8" Offset="1"/>
        </LinearGradientBrush>
    </Window.Resources>
    
    <Grid Background="#FAFAFA">
        <syncfusion:TileViewControl AllowDragDrop="True" Margin="10">
            
            <!-- Dashboard Tile -->
            <syncfusion:TileViewItem Header="Dashboard"
                                     Background="White"
                                     Foreground="#333"
                                     BorderBrush="#DDD"
                                     BorderThickness="1"
                                     MinimizedHeight="150"
                                     MinimizedWidth="150"
                                     CanMaximize="True"
                                     MaximizedHeight="400"
                                     MaximizedWidth="600">
                <Grid Margin="15">
                    <StackPanel>
                        <TextBlock Text="$45,230" FontSize="28" FontWeight="Bold" Foreground="#0078D4"/>
                        <TextBlock Text="Total Revenue" Margin="0,5,0,0" Foreground="#999"/>
                    </StackPanel>
                </Grid>
            </syncfusion:TileViewItem>
            
            <!-- Alerts Tile -->
            <syncfusion:TileViewItem Header="Alerts"
                                     Background="#FFEBEE"
                                     Foreground="#C62828"
                                     MinimizedHeight="150"
                                     MinimizedWidth="150">
                <Grid Margin="15">
                    <TextBlock Text="3 Active Alerts" FontSize="18" FontWeight="Bold"/>
                </Grid>
            </syncfusion:TileViewItem>
            
            <!-- Status Tile -->
            <syncfusion:TileViewItem Header="System Status"
                                     Background="White"
                                     Foreground="#333"
                                     BorderBrush="#DDD"
                                     BorderThickness="1"
                                     MinimizedHeight="150"
                                     MinimizedWidth="150">
                <Grid Margin="15">
                    <StackPanel>
                        <TextBlock Text="99.9%" FontSize="28" FontWeight="Bold" Foreground="#2E7D32"/>
                        <TextBlock Text="Uptime" Margin="0,5,0,0" Foreground="#999"/>
                    </StackPanel>
                </Grid>
            </syncfusion:TileViewItem>
        </syncfusion:TileViewControl>
    </Grid>
</Window>
```

## Best Practices

1. **Maintain consistency** - Use a unified color scheme across all tiles
2. **Ensure readability** - Sufficient contrast between text and background
3. **Use meaningful colors** - Red for alerts, green for success, etc.
4. **Responsive design** - Adjust styling for different screen sizes
5. **Test themes** - Verify appearance in both light and dark themes
6. **Performance** - Keep custom templates and effects optimized
