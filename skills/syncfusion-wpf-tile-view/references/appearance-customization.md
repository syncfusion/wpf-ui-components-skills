# Appearance and Customization

## Overview

TileViewControl offers extensive customization options for appearance, including styling headers, content areas, borders, backgrounds, and state-specific presentations for minimized and maximized tiles.

## Styling Tiles

Use the following properties to customize tile appearance:

| Property | Purpose |
|----------|---------|
| `Background` | Tile background color |
| `Foreground` | Text color |
| `BorderBrush` | Border line color |
| `BorderThickness` | Border width (all sides or individual) |
| `Margin` | Space outside the tile |
| `Padding` | Space inside the tile |
| `CornerRadius` | Rounded corners (via Style) |
| `Opacity` | Tile transparency (0.0-1.0) |

**Example with multiple colors:**

```xaml
<!-- Set background and border -->
<syncfusion:TileViewItem Header="Styled Tile"
                         Background="White"
                         BorderBrush="#0078D4"
                         BorderThickness="2"
                         Foreground="#333333">
    <TextBlock Text="Tile content"/>
</syncfusion:TileViewItem>
```

For colored tiles, apply semantic colors (blue for primary, green for success, red for alerts, orange for warnings).

## Header Customization

Headers can display simple text or complex UI elements with icons and dynamic content.

**Simple text header:**
```xaml
<syncfusion:TileViewItem Header="My Tile Title">
    <TextBlock Text="Tile Content"/>
</syncfusion:TileViewItem>
```

**Rich header with icon:**
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

Set header background color using the tile's `Background` property or use a styled `Border` wrapper for fine-grained control.

## Content Styling

Structure content with rich layouts combining text, images, and data visualizations.

**Dashboard metric display:**
```xaml
<syncfusion:TileViewItem Header="Dashboard">
    <Grid Background="White">
        <StackPanel Margin="15">
            <TextBlock Text="Total Revenue" FontSize="14" FontWeight="Bold" Foreground="#333"/>
            <TextBlock Text="$125,430" FontSize="32" FontWeight="Bold" Foreground="#0078D4" Margin="0,5,0,0"/>
            <TextBlock Text="↑ 12% from last month" FontSize="12" Foreground="#2E7D32" Margin="0,3,0,0"/>
        </StackPanel>
    </Grid>
</syncfusion:TileViewItem>
```

**Content with background images:**
Use `Image` elements with `Stretch="UniformToFill"` and overlay content using `StackPanel` with semi-transparent backgrounds for text readability.

## State-Specific Styling

Apply different appearance for minimized and maximized states. Tiles automatically animate when transitioning between states.

**Minimized appearance:**
```xaml
<syncfusion:TileViewItem Header="Compact View"
                         MinimizedHeight="120"
                         MinimizedWidth="150">
    <Grid Background="#F5F5F5">
        <TextBlock Text="Summary" VerticalAlignment="Center" HorizontalAlignment="Center"/>
    </Grid>
</syncfusion:TileViewItem>
```

**Maximized appearance:**
```xaml
<syncfusion:TileViewItem Header="Expanded View"
                         CanMaximize="True"
                         MaximizedHeight="400"
                         MaximizedWidth="600">
    <Grid Background="White">
        <StackPanel Margin="20">
            <TextBlock Text="Detailed Content" FontSize="18" FontWeight="Bold"/>
            <TextBlock Text="Full details shown when maximized." Margin="0,10,0,0" TextWrapping="Wrap"/>
        </StackPanel>
    </Grid>
</syncfusion:TileViewItem>
```

## Themes

Apply consistent theming across all tiles using color palettes and styles.

**Define a color palette:**
```csharp
public static class TileTheme
{
    public static readonly Color PrimaryBlue = Color.FromArgb(255, 0, 120, 212);
    public static readonly Color SuccessGreen = Color.FromArgb(255, 46, 125, 50);
    public static readonly Color WarningOrange = Color.FromArgb(255, 255, 152, 0);
    public static readonly Color AlertRed = Color.FromArgb(255, 198, 40, 40);
}
```

**Light theme (white backgrounds):**
```xaml
<syncfusion:TileViewControl Background="#FFFFFF">
    <syncfusion:TileViewItem Header="Light Tile"
                             Background="#F5F5F5"
                             Foreground="#333333">
        <TextBlock Text="Content on light background"/>
    </syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

**Dark theme (dark backgrounds):**
```xaml
<syncfusion:TileViewControl Background="#1E1E1E">
    <syncfusion:TileViewItem Header="Dark Tile"
                             Background="#2D2D2D"
                             Foreground="#FFFFFF">
        <TextBlock Text="Content on dark background"/>
    </syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

## Visual Effects

Enhance tiles with gradients, transparency, and custom shapes.

**Gradient background:**
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

## ScrollBar Customization

Customize or hide scroll bars within the TileViewControl.

**Style scroll bars:**
```xaml
<Window.Resources>
    <Style TargetType="ScrollBar">
        <Setter Property="Background" Value="#F5F5F5"/>
        <Setter Property="Foreground" Value="#0078D4"/>
    </Style>
</Window.Resources>
```

**Hide scroll bars programmatically:**
```csharp
ScrollViewer scrollViewer = tileViewControl.FindScrollViewer();
if (scrollViewer != null)
{
    scrollViewer.HorizontalScrollBarVisibility = ScrollBarVisibility.Hidden;
    scrollViewer.VerticalScrollBarVisibility = ScrollBarVisibility.Hidden;
}
```

## Example

```xaml
<syncfusion:TileViewControl AllowDragDrop="True" Margin="10" Background="#FAFAFA">
    <syncfusion:TileViewItem Header="Dashboard" Background="White" Foreground="#333" MinimizedHeight="150" MinimizedWidth="150" CanMaximize="True" MaximizedHeight="400" MaximizedWidth="600">
        <StackPanel Margin="15">
            <TextBlock Text="$45,230" FontSize="28" FontWeight="Bold" Foreground="#0078D4"/>
            <TextBlock Text="Total Revenue" Margin="0,5,0,0" Foreground="#999"/>
        </StackPanel>
    </syncfusion:TileViewItem>
    
    <syncfusion:TileViewItem Header="Alerts" Background="#FFEBEE" Foreground="#C62828" MinimizedHeight="150" MinimizedWidth="150">
        <TextBlock Text="3 Active Alerts" FontSize="18" FontWeight="Bold" Margin="15"/>
    </syncfusion:TileViewItem>
</syncfusion:TileViewControl>
```

## Best Practices

- **Maintain consistency:** Use a unified color scheme and font sizing across all tiles
- **Ensure readability:** Verify sufficient contrast between text and background colors
- **Use semantic colors:** Red for alerts, green for success, blue for primary, orange for warnings
- **Responsive design:** Test tile layouts at different window sizes
- **Optimize performance:** Keep custom templates lean and minimize complex visual effects
