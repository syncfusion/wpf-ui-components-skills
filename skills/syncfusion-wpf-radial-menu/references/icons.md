# Icons in WPF Radial Menu

This guide covers how to customize icons in the Syncfusion WPF Radial Menu, including the center icon and individual menu item icons.

---

## Overview

The SfRadialMenu control supports icon customization at two levels:

1. **Center Icon:** The icon displayed at the center of the radial menu
2. **Menu Item Icons:** Individual icons for each `SfRadialMenuItem`

Both support any `UIElement`, providing maximum flexibility for icon content.

---

## Center Icon

The `Icon` property of `SfRadialMenu` customizes the icon displayed in the center of the radial menu circle. This typically represents the overall purpose or category of the menu.

### Basic Center Icon with Image

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenu.Icon>
        <Grid Background="White">
            <Image Source="/Assets/text.png" 
                   Width="20"
                   Stretch="Uniform"/>
        </Grid>
    </navigation:SfRadialMenu.Icon>
    
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Cut"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

### Code-Behind Implementation

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.IsOpen = true;

// Create icon grid
Grid iconGrid = new Grid();
iconGrid.Background = new SolidColorBrush(Colors.White);

// Create image
Image image = new Image();
image.Source = new BitmapImage(new Uri("/Assets/text.png", UriKind.Relative));
image.Width = 20;
image.Stretch = Stretch.Uniform;

iconGrid.Children.Add(image);
radialMenu.Icon = iconGrid;
```

### Center Icon with Path Geometry

For vector graphics, use `Path` with geometry data:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenu.Icon>
        <Image Source="ms-appx:///Assets/text.png" Width="20"  

 	 	 	                Stretch="Uniform"/>
    </navigation:SfRadialMenu.Icon>
    
    <navigation:SfRadialMenuItem Header="Option 1"/>
    <navigation:SfRadialMenuItem Header="Option 2"/>
    <navigation:SfRadialMenuItem Header="Option 3"/>
</navigation:SfRadialMenu>
```

### Center Icon with Custom Content

Use any `UIElement` for advanced scenarios:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenu.Icon>
        <Grid Width="40" Height="40">
            <Ellipse Fill="OrangeRed" Width="40" Height="40"/>
            <TextBlock Text="T" 
                      FontSize="20" 
                      FontWeight="Bold"
                      Foreground="White"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center"/>
        </Grid>
    </navigation:SfRadialMenu.Icon>
    
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Italic"/>
</navigation:SfRadialMenu>
```

### Center Icon with Icon Font

Using icon fonts like FontAwesome or Segoe MDL2 Assets:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenu.Icon>
        <TextBlock Text="&#xE70F;" 
                   FontFamily="Segoe MDL2 Assets"
                   FontSize="20"
                   Foreground="DodgerBlue"/>
    </navigation:SfRadialMenu.Icon>
    
    <navigation:SfRadialMenuItem Header="Edit"/>
    <navigation:SfRadialMenuItem Header="View"/>
</navigation:SfRadialMenu>
```

---

## SfRadialMenuItem Icon

The `Icon` property of `SfRadialMenuItem` sets a custom icon for individual menu items. This provides visual identification for each menu option.

### Basic Menu Item Icons

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold">
        <navigation:SfRadialMenuItem.Icon>
            <Image Source="/Assets/bold.png" 
                   Width="16" 
                   Height="16" 
                   Stretch="Uniform"/>
        </navigation:SfRadialMenuItem.Icon>
    </navigation:SfRadialMenuItem>
    
    <navigation:SfRadialMenuItem Header="Copy">
        <navigation:SfRadialMenuItem.Icon>
            <Image Source="/Assets/copy.png" 
                   Width="16" 
                   Height="16" 
                   Stretch="Uniform"/>
        </navigation:SfRadialMenuItem.Icon>
    </navigation:SfRadialMenuItem>
    
    <navigation:SfRadialMenuItem Header="Paste">
        <navigation:SfRadialMenuItem.Icon>
            <Image Source="/Assets/paste.png" 
                   Width="16" 
                   Height="16" 
                   Stretch="Uniform"/>
        </navigation:SfRadialMenuItem.Icon>
    </navigation:SfRadialMenuItem>
    
    <navigation:SfRadialMenuItem Header="Cut">
        <navigation:SfRadialMenuItem.Icon>
            <Image Source="/Assets/cut.png" 
                   Width="16" 
                   Height="16" 
                   Stretch="Uniform"/>
        </navigation:SfRadialMenuItem.Icon>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

### Code-Behind Implementation

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.IsOpen = true;

// Bold item with icon
SfRadialMenuItem bold = new SfRadialMenuItem() { Header = "Bold" };
bold.Icon = new Image()
{
    Source = new BitmapImage(new Uri("/Assets/bold.png", UriKind.Relative)),
    Width = 16,
    Height = 16,
    Stretch = Stretch.Uniform
};

// Copy item with icon
SfRadialMenuItem copy = new SfRadialMenuItem() { Header = "Copy" };
copy.Icon = new Image()
{
    Source = new BitmapImage(new Uri("/Assets/copy.png", UriKind.Relative)),
    Width = 16,
    Height = 16,
    Stretch = Stretch.Uniform
};

// Paste item with icon
SfRadialMenuItem paste = new SfRadialMenuItem() { Header = "Paste" };
paste.Icon = new Image()
{
    Source = new BitmapImage(new Uri("/Assets/paste.png", UriKind.Relative)),
    Width = 16,
    Height = 16,
    Stretch = Stretch.Uniform
};

radialMenu.Items.Add(bold);
radialMenu.Items.Add(copy);
radialMenu.Items.Add(paste);
```

## Icons with Data Binding

When using `ItemsSource` binding, configure icons through the business model and `ItemTemplate`.

### Business Model with Icon Path

```csharp
public class MenuCommand
{
    public string Name { get; set; }
    public string IconPath { get; set; }
    public ICommand Command { get; set; }
}

public class MenuViewModel
{
    public List<MenuCommand> Commands { get; set; }
    
    public MenuViewModel()
    {
        Commands = new List<MenuCommand>
        {
            new MenuCommand 
            { 
                Name = "Bold", 
                IconPath = "/Assets/bold.png" 
            },
            new MenuCommand 
            { 
                Name = "Italic", 
                IconPath = "/Assets/italic.png" 
            },
            new MenuCommand 
            { 
                Name = "Underline", 
                IconPath = "/Assets/underline.png" 
            }
        };
    }
}
```

### ItemTemplate with Icon Binding

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         ItemsSource="{Binding Commands}">
    <navigation:SfRadialMenu.ItemTemplate>
        <DataTemplate>
            <StackPanel>
                <Image Height="16" 
                       Width="16" 
                       Source="{Binding IconPath}"
                       Stretch="Uniform"/>
                <TextBlock Margin="0,3,0,0" 
                          Text="{Binding Name}"
                          FontSize="10"/>
            </StackPanel>
        </DataTemplate>
    </navigation:SfRadialMenu.ItemTemplate>
</navigation:SfRadialMenu>
```

---

## Icon Sizing and Stretching

### Recommended Sizes

| Icon Location | Recommended Size | Purpose |
|---------------|------------------|---------|
| Center Icon | 20-32px | Main menu identifier |
| Menu Item Icon | 16-20px | Individual item identification |
| Large Touch UI | 32-40px | Touch-friendly interfaces |

### Stretch Modes

```xaml
<!-- Uniform (Recommended): Maintains aspect ratio -->
<Image Source="icon.png" Stretch="Uniform"/>

<!-- Fill: May distort the image -->
<Image Source="icon.png" Stretch="Fill"/>

<!-- UniformToFill: Crops to fill space -->
<Image Source="icon.png" Stretch="UniformToFill"/>

<!-- None: Original size -->
<Image Source="icon.png" Stretch="None"/>
```

### Fixed Size Icons

```xaml
<navigation:SfRadialMenuItem Header="Export">
    <navigation:SfRadialMenuItem.Icon>
        <Image Source="/Assets/export.png" 
               Width="18" 
               Height="18" 
               Stretch="Uniform"/>
    </navigation:SfRadialMenuItem.Icon>
</navigation:SfRadialMenuItem>
```


## Advanced Icon Patterns


### Pattern 1: Conditional Icon Display

Use data binding with converters to show different icons based on state:

```xaml
<navigation:SfRadialMenuItem Header="{Binding ItemName}">
    <navigation:SfRadialMenuItem.Icon>
        <Image Source="{Binding IsEnabled, 
                               Converter={StaticResource BoolToIconConverter}}"
               Width="16" 
               Height="16"/>
    </navigation:SfRadialMenuItem.Icon>
</navigation:SfRadialMenuItem>
```

```csharp
public class BoolToIconConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        bool isEnabled = (bool)value;
        string iconPath = isEnabled ? "/Assets/enabled.png" : "/Assets/disabled.png";
        return new BitmapImage(new Uri(iconPath, UriKind.Relative));
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Pattern 2: Animated Icons

Add subtle animations to icons:

```xaml
<navigation:SfRadialMenuItem Header="Refresh">
    <navigation:SfRadialMenuItem.Icon>
        <Grid Width="20" Height="20">
            <Path x:Name="RefreshIcon"
                  Data="M17.65,6.35C16.2,4.9 14.21,4 12,4A8,8 0 0,0 4,12A8,8 0 0,0 12,20C15.73,20 18.84,17.45 19.73,14H17.65C16.83,16.33 14.61,18 12,18A6,6 0 0,1 6,12A6,6 0 0,1 12,6C13.66,6 15.14,6.69 16.22,7.78L13,11H20V4L17.65,6.35Z"
                  Fill="DodgerBlue">
                <Path.RenderTransform>
                    <RotateTransform x:Name="RotateTransform" CenterX="10" CenterY="10"/>
                </Path.RenderTransform>
                <Path.Triggers>
                    <EventTrigger RoutedEvent="Path.Loaded">
                        <BeginStoryboard>
                            <Storyboard RepeatBehavior="Forever">
                                <DoubleAnimation Storyboard.TargetName="RotateTransform"
                                               Storyboard.TargetProperty="Angle"
                                               From="0" To="360"
                                               Duration="0:0:2"/>
                            </Storyboard>
                        </BeginStoryboard>
                    </EventTrigger>
                </Path.Triggers>
            </Path>
        </Grid>
    </navigation:SfRadialMenuItem.Icon>
</navigation:SfRadialMenuItem>
```

---

## Best Practices

### Icon Selection

1. **Consistency:** Use icons from the same icon family/style
2. **Clarity:** Choose icons with clear, recognizable shapes
3. **Simplicity:** Avoid overly detailed icons in small sizes
4. **Color:** Use colors that contrast with backgrounds

### Performance

1. **Vector vs Raster:** Prefer vector (Path) for scalability
2. **Caching:** Load icon resources once and reuse
3. **Size:** Don't use unnecessarily large image files

### Accessibility

1. **Meaningful Headers:** Always pair icons with text headers
2. **High Contrast:** Ensure icons are visible in high contrast mode
3. **Size:** Make icons large enough for touch targets (minimum 16px)

---

## Troubleshooting

### Issue: Icons Not Displaying

**Problem:** Icons are configured but don't appear.

**Solutions:**
1. Verify image path is correct: Use absolute paths or proper URI schemes
2. Check image Build Action: Should be "Resource" or "Content"
3. Ensure image file exists in the specified location

```xaml
<!-- Correct WPF path formats -->
<Image Source="/Assets/icon.png"/>                    <!-- Relative path -->
<Image Source="pack://application:,,,/Assets/icon.png"/> <!-- Pack URI -->
<Image Source="/ProjectName;component/Assets/icon.png"/> <!-- Component syntax -->
```

### Issue: Icons Appear Distorted

**Problem:** Icons are stretched or squashed.

**Solution:** Use `Stretch="Uniform"` to maintain aspect ratio:
```xaml
<Image Source="icon.png" Stretch="Uniform" Width="16" Height="16"/>
```

### Issue: Icons Too Small/Large

**Problem:** Icons don't scale properly for different DPI settings.

**Solution:** Use vector graphics (Path) instead of raster images, or provide multiple image sizes and use appropriate converters.
