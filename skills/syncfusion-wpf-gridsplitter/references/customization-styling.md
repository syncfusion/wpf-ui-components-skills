# Customization and Styling

## Table of Contents
- [Background Customization](#background-customization)
- [Gripper Templates](#gripper-templates)
- [Preview Style Configuration](#preview-style-configuration)
- [ShowsPreview Property](#showspreview-property)
- [Custom Drag Preview](#custom-drag-preview)
- [Advanced Styling Scenarios](#advanced-styling-scenarios)
- [Theme Integration](#theme-integration)

This guide covers all visual customization options for SfGridSplitter, from simple color changes to advanced template customization.

## Background Customization

The Background property controls the splitter bar color.

### Basic Background Color

Set a solid color background:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <TextBlock Grid.Row="0" 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   Text="Panel 1"/>
        
        <TextBlock Grid.Row="2"
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 2"/>
        
        <!-- Grid Splitter with custom background -->
        <syncfusion:SfGridSplitter Background="Green"
                                   HorizontalAlignment="Stretch"
                                   Height="5"
                                   Grid.Row="1"
                                   ResizeBehavior="PreviousAndNext"/>
    </Grid>
</Border>
```

**Default:** Light Gray

### Gradient Background

Create visual depth with gradients:

```xml
<syncfusion:SfGridSplitter HorizontalAlignment="Stretch"
                           Height="8"
                           Grid.Row="1">
    <syncfusion:SfGridSplitter.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#FF4A90E2" Offset="0"/>
            <GradientStop Color="#FF2D6FA8" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:SfGridSplitter.Background>
</syncfusion:SfGridSplitter>
```

### Image Background

Use patterns or textures:

```xml
<syncfusion:SfGridSplitter VerticalAlignment="Stretch"
                           Width="10"
                           Grid.Column="1">
    <syncfusion:SfGridSplitter.Background>
        <ImageBrush ImageSource="Assets/splitter-texture.png"
                    Stretch="Fill"/>
    </syncfusion:SfGridSplitter.Background>
</syncfusion:SfGridSplitter>
```

### Transparent/Invisible Splitter

Create invisible splitter (gripper only):

```xml
<syncfusion:SfGridSplitter Background="Transparent"
                           HorizontalAlignment="Stretch"
                           Height="10"
                           Grid.Row="1"/>
```

**Use case:** Clean minimal design where only the gripper indicates the splitter.

## Gripper Templates

Grippers are the visual indicators on the splitter bar that suggest draggability.

### HorizontalGripperTemplate

Customize the gripper for horizontal splitters:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <Border Grid.Row="0" Background="LightBlue"/>
        <Border Grid.Row="2" Background="LightGreen"/>

        <syncfusion:SfGridSplitter Grid.Row="1"
                                   Height="20"
                                   HorizontalAlignment="Stretch"
                                   Background="DarkGray"
                                   ResizeBehavior="PreviousAndNext">
            
            <!-- Custom Horizontal Gripper -->
            <syncfusion:SfGridSplitter.HorizontalGripperTemplate>
                <DataTemplate>
                    <TextBlock Background="Blue" 
                               Foreground="White"
                               TextAlignment="Center"
                               VerticalAlignment="Center" 
                               HorizontalAlignment="Center"
                               Text="⋮⋮⋮ Drag ⋮⋮⋮" 
                               FontSize="10"
                               Width="90"
                               Height="20"/>
                </DataTemplate>
            </syncfusion:SfGridSplitter.HorizontalGripperTemplate>
        </syncfusion:SfGridSplitter>
    </Grid>
</Border>
```

### VerticalGripperTemplate

Customize the gripper for vertical splitters:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <Border Grid.Column="0" Background="LightCoral"/>
        <Border Grid.Column="2" Background="LightYellow"/>

        <syncfusion:SfGridSplitter Grid.Column="1"
                                   Width="20"
                                   VerticalAlignment="Stretch"
                                   Background="DarkGray"
                                   ResizeBehavior="PreviousAndNext">
            
            <!-- Custom Vertical Gripper -->
            <syncfusion:SfGridSplitter.VerticalGripperTemplate>
                <DataTemplate>
                    <TextBlock Background="Red" 
                               Foreground="White"
                               TextAlignment="Center"
                               VerticalAlignment="Center"
                               HorizontalAlignment="Center" 
                               Text="⋮⋮⋮ Drag ⋮⋮⋮" 
                               FontSize="10"
                               Width="90" 
                               Height="20">
                        <TextBlock.LayoutTransform>
                            <RotateTransform Angle="-90"/>
                        </TextBlock.LayoutTransform>
                    </TextBlock>
                </DataTemplate>
            </syncfusion:SfGridSplitter.VerticalGripperTemplate>
        </syncfusion:SfGridSplitter>
    </Grid>
</Border>
```

**Note:** Use `LayoutTransform` with `RotateTransform` to rotate content for vertical orientation.

### Icon-Based Gripper

Use paths or geometry for professional grippers:

```xml
<syncfusion:SfGridSplitter.HorizontalGripperTemplate>
    <DataTemplate>
        <Grid Width="50" Height="16" Background="#FF007ACC">
            <Path Data="M 0,4 L 50,4 M 0,8 L 50,8 M 0,12 L 50,12"
                  Stroke="White"
                  StrokeThickness="2"
                  StrokeDashArray="2,2"/>
        </Grid>
    </DataTemplate>
</syncfusion:SfGridSplitter.HorizontalGripperTemplate>
```

### Minimalist Dot Gripper

Subtle dot pattern:

```xml
<syncfusion:SfGridSplitter.HorizontalGripperTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center"
                    VerticalAlignment="Center">
            <Ellipse Width="4" Height="4" Fill="Gray" Margin="2"/>
            <Ellipse Width="4" Height="4" Fill="Gray" Margin="2"/>
            <Ellipse Width="4" Height="4" Fill="Gray" Margin="2"/>
            <Ellipse Width="4" Height="4" Fill="Gray" Margin="2"/>
            <Ellipse Width="4" Height="4" Fill="Gray" Margin="2"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:SfGridSplitter.HorizontalGripperTemplate>
```

## Preview Style Configuration

The PreviewStyle property customizes the appearance of the preview indicator during deferred resizing.

### Understanding Preview Mode

When `ShowsPreview="True"`:
1. User starts dragging splitter
2. Preview indicator appears (instead of real-time resize)
3. User sees where splitter will move
4. On mouse release, actual resize occurs

**Benefits:**
- Better performance for complex layouts
- Visual confirmation before committing
- Reduces layout thrashing during drag

### Default Preview Behavior

Basic preview mode:

```xml
<syncfusion:SfGridSplitter ShowsPreview="True"
                           HorizontalAlignment="Stretch"
                           Grid.Row="1"
                           ResizeBehavior="PreviousAndNext"/>
```

**Default preview:** Semi-transparent gray line follows cursor.

### Custom Preview Style

Create custom preview appearance:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <Border Grid.Row="0" Background="LightBlue"/>
        <Border Grid.Row="2" Background="LightGreen"/>

        <syncfusion:SfGridSplitter ShowsPreview="True"
                                   HorizontalAlignment="Stretch"
                                   Height="5"
                                   Grid.Row="1"
                                   ResizeBehavior="PreviousAndNext">
            
            <!-- Custom Preview Style -->
            <syncfusion:SfGridSplitter.PreviewStyle>
                <Style TargetType="Control">
                    <Setter Property="Background" Value="Red"/>
                    <Setter Property="Opacity" Value="0.5"/>
                    <Setter Property="Template">
                        <Setter.Value>
                            <ControlTemplate TargetType="Control">
                                <Grid x:Name="Root">
                                    <Rectangle Fill="{TemplateBinding Background}"/>
                                </Grid>
                            </ControlTemplate>
                        </Setter.Value>
                    </Setter>
                </Style>
            </syncfusion:SfGridSplitter.PreviewStyle>
        </syncfusion:SfGridSplitter>
    </Grid>
</Border>
```

**Key Properties in PreviewStyle:**
- **Background:** Color of preview indicator
- **Opacity:** Transparency (0.0 to 1.0)
- **Template:** Complete visual structure

### Animated Preview Style

Add visual effects to preview:

```xml
<syncfusion:SfGridSplitter.PreviewStyle>
    <Style TargetType="Control">
        <Setter Property="Background" Value="#FF007ACC"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="Control">
                    <Grid x:Name="Root" Opacity="0.6">
                        <Rectangle Fill="{TemplateBinding Background}">
                            <Rectangle.Effect>
                                <DropShadowEffect BlurRadius="10" 
                                                  ShadowDepth="0"
                                                  Color="#FF007ACC"/>
                            </Rectangle.Effect>
                        </Rectangle>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</syncfusion:SfGridSplitter.PreviewStyle>
```

### Dashed Line Preview

Professional preview style:

```xml
<syncfusion:SfGridSplitter.PreviewStyle>
    <Style TargetType="Control">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="Control">
                    <Border BorderBrush="DarkBlue"
                            BorderThickness="2"
                            Background="Transparent">
                        <Border.BorderDashArray>
                            <DoubleCollection>4,2</DoubleCollection>
                        </Border.BorderDashArray>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</syncfusion:SfGridSplitter.PreviewStyle>
```

## ShowsPreview Property

Controls whether splitter uses preview mode or real-time resizing.

### Real-Time Resizing (Default)

```xml
<syncfusion:SfGridSplitter ShowsPreview="False"
                           HorizontalAlignment="Stretch"
                           Grid.Row="1"/>
```

**Behavior:** Panels resize immediately as splitter is dragged.

**Best for:**
- Simple layouts with few controls
- Fast rendering scenarios
- When immediate feedback is important

### Preview Mode

```xml
<syncfusion:SfGridSplitter ShowsPreview="True"
                           HorizontalAlignment="Stretch"
                           Grid.Row="1"/>
```

**Behavior:** Shows preview line; resizes on mouse release.

**Best for:**
- Complex layouts with many controls
- Data-heavy panels (grids, charts)
- Performance-sensitive scenarios
- Large applications with multiple nested controls

### Performance Comparison

**Real-Time Resizing:**
- ✅ Immediate visual feedback
- ✅ Intuitive for users
- ❌ Can lag with complex content
- ❌ Causes continuous layout recalculation

**Preview Mode:**
- ✅ Smooth dragging even with heavy content
- ✅ Better performance
- ✅ Single layout recalculation on release
- ❌ Less immediate feedback
- ❌ Requires mouse release to see result

## Custom Drag Preview

Combine preview style customization with functional requirements.

### Complete Custom Preview Example

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <!-- Heavy content panels -->
    <DataGrid Grid.Row="0" ItemsSource="{Binding TopData}"/>
    <DataGrid Grid.Row="2" ItemsSource="{Binding BottomData}"/>
    
    <!-- Splitter with custom preview -->
    <syncfusion:SfGridSplitter Grid.Row="1"
                               Height="8"
                               HorizontalAlignment="Stretch"
                               ShowsPreview="True"
                               Background="#FF333333"
                               ResizeBehavior="PreviousAndNext">
        
        <!-- Custom gripper -->
        <syncfusion:SfGridSplitter.HorizontalGripperTemplate>
            <DataTemplate>
                <Border Background="#FF555555" 
                        Width="80" 
                        Height="8"
                        CornerRadius="4">
                    <StackPanel Orientation="Horizontal" 
                                HorizontalAlignment="Center">
                        <Ellipse Width="3" Height="3" 
                                 Fill="White" Margin="1"/>
                        <Ellipse Width="3" Height="3" 
                                 Fill="White" Margin="1"/>
                        <Ellipse Width="3" Height="3" 
                                 Fill="White" Margin="1"/>
                    </StackPanel>
                </Border>
            </DataTemplate>
        </syncfusion:SfGridSplitter.HorizontalGripperTemplate>
        
        <!-- Custom preview style -->
        <syncfusion:SfGridSplitter.PreviewStyle>
            <Style TargetType="Control">
                <Setter Property="Background" Value="#FF0078D7"/>
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Control">
                            <Grid Opacity="0.7">
                                <Rectangle Fill="{TemplateBinding Background}"
                                           Height="3"/>
                            </Grid>
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </syncfusion:SfGridSplitter.PreviewStyle>
    </syncfusion:SfGridSplitter>
</Grid>
```

## Advanced Styling Scenarios

### Hover Effect on Splitter

Add visual feedback on mouse hover:

```xml
<syncfusion:SfGridSplitter Grid.Row="1"
                           HorizontalAlignment="Stretch"
                           Height="8"
                           Background="#FFCCCCCC">
    <syncfusion:SfGridSplitter.Style>
        <Style TargetType="syncfusion:SfGridSplitter">
            <Style.Triggers>
                <Trigger Property="IsMouseOver" Value="True">
                    <Setter Property="Background" Value="#FF0078D7"/>
                    <Setter Property="Cursor" Value="SizeNS"/>
                </Trigger>
            </Style.Triggers>
        </Style>
    </syncfusion:SfGridSplitter.Style>
</syncfusion:SfGridSplitter>
```

### Combined Gripper and Collapse Buttons

Full custom styling:

```xml
<syncfusion:SfGridSplitter Grid.Row="1"
                           Height="30"
                           HorizontalAlignment="Stretch"
                           EnableCollapseButton="True"
                           Background="LightGray"
                           ResizeBehavior="PreviousAndNext">
    
    <!-- Custom gripper -->
    <syncfusion:SfGridSplitter.HorizontalGripperTemplate>
        <DataTemplate>
            <Border Background="#FF007ACC"
                    Width="100"
                    Height="6"
                    CornerRadius="3">
                <Path Data="M 0,0 L 100,0 M 0,3 L 100,3"
                      Stroke="White"
                      StrokeThickness="1"
                      StrokeDashArray="2,2"/>
            </Border>
        </DataTemplate>
    </syncfusion:SfGridSplitter.HorizontalGripperTemplate>
    
    <!-- Custom up button -->
    <syncfusion:SfGridSplitter.UpButtonTemplate>
        <DataTemplate>
            <Border Background="#FF4CAF50"
                    CornerRadius="12"
                    Width="24"
                    Height="24"
                    BorderBrush="White"
                    BorderThickness="2">
                <Path Data="M 8,12 L 12,8 L 16,12"
                      Stroke="White"
                      StrokeThickness="2"/>
            </Border>
        </DataTemplate>
    </syncfusion:SfGridSplitter.UpButtonTemplate>
    
    <!-- Custom down button -->
    <syncfusion:SfGridSplitter.DownButtonTemplate>
        <DataTemplate>
            <Border Background="#FFF44336"
                    CornerRadius="12"
                    Width="24"
                    Height="24"
                    BorderBrush="White"
                    BorderThickness="2">
                <Path Data="M 8,8 L 12,12 L 16,8"
                      Stroke="White"
                      StrokeThickness="2"/>
            </Border>
        </DataTemplate>
    </syncfusion:SfGridSplitter.DownButtonTemplate>
</syncfusion:SfGridSplitter>
```

### Multi-Splitter with Consistent Styling

Apply consistent style across multiple splitters:

```xml
<Window.Resources>
    <!-- Define shared splitter style -->
    <Style x:Key="CustomSplitterStyle" 
           TargetType="syncfusion:SfGridSplitter">
        <Setter Property="Background" Value="#FF2D2D30"/>
        <Setter Property="ResizeBehavior" Value="PreviousAndNext"/>
        <Setter Property="ShowsPreview" Value="True"/>
        <Setter Property="PreviewStyle">
            <Setter.Value>
                <Style TargetType="Control">
                    <Setter Property="Background" Value="#FF007ACC"/>
                    <Setter Property="Opacity" Value="0.6"/>
                </Style>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="Auto"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    
    <!-- Content panels -->
    <Border Grid.Row="0" Grid.Column="0" Background="LightBlue"/>
    <Border Grid.Row="2" Grid.Column="0" Background="LightGreen"/>
    <Border Grid.RowSpan="3" Grid.Column="2" Background="LightYellow"/>
    
    <!-- Horizontal splitter with style -->
    <syncfusion:SfGridSplitter Grid.Row="1" Grid.Column="0"
                               HorizontalAlignment="Stretch"
                               Height="5"
                               Style="{StaticResource CustomSplitterStyle}"/>
    
    <!-- Vertical splitter with style -->
    <syncfusion:SfGridSplitter Grid.RowSpan="3" Grid.Column="1"
                               VerticalAlignment="Stretch"
                               Width="5"
                               Style="{StaticResource CustomSplitterStyle}"/>
</Grid>
```

## Theme Integration

### Matching Application Theme

Adapt splitter to match your app's theme:

```xml
<!-- Light theme -->
<syncfusion:SfGridSplitter Background="#FFE1E1E1"
                           Grid.Row="1">
    <syncfusion:SfGridSplitter.HorizontalGripperTemplate>
        <DataTemplate>
            <Border Background="#FF999999" 
                    Width="60" 
                    Height="4"
                    CornerRadius="2"/>
        </DataTemplate>
    </syncfusion:SfGridSplitter.HorizontalGripperTemplate>
</syncfusion:SfGridSplitter>

<!-- Dark theme -->
<syncfusion:SfGridSplitter Background="#FF2D2D30"
                           Grid.Row="1">
    <syncfusion:SfGridSplitter.HorizontalGripperTemplate>
        <DataTemplate>
            <Border Background="#FF555555" 
                    Width="60" 
                    Height="4"
                    CornerRadius="2"/>
        </DataTemplate>
    </syncfusion:SfGridSplitter.HorizontalGripperTemplate>
</syncfusion:SfGridSplitter>
```

### Using Theme Resources

Reference application-wide theme resources:

```xml
<Window.Resources>
    <SolidColorBrush x:Key="AccentColor" Color="#FF0078D7"/>
    <SolidColorBrush x:Key="BackgroundColor" Color="#FF2D2D30"/>
</Window.Resources>

<syncfusion:SfGridSplitter Background="{StaticResource BackgroundColor}"
                           Grid.Row="1">
    <syncfusion:SfGridSplitter.PreviewStyle>
        <Style TargetType="Control">
            <Setter Property="Background" 
                    Value="{StaticResource AccentColor}"/>
        </Style>
    </syncfusion:SfGridSplitter.PreviewStyle>
</syncfusion:SfGridSplitter>
```

## Best Practices

### Performance Optimization

1. **Use ShowsPreview for Heavy Layouts:**
   ```xml
   <syncfusion:SfGridSplitter ShowsPreview="True"/>
   ```

2. **Simplify Real-Time Layouts:**
   - Minimize number of controls in resizable panels
   - Use virtualization for lists/grids
   - Avoid complex nested layouts

### Visual Design

1. **Size Appropriately:**
   - 3-5 pixels: Minimal splitters
   - 5-8 pixels: Standard splitters
   - 8-12 pixels: With collapse buttons
   - 15-30 pixels: Prominent custom designs

2. **Color Selection:**
   - Match application theme
   - Ensure sufficient contrast for visibility
   - Use hover effects for discoverability

3. **Gripper Design:**
   - Keep grippers simple and recognizable
   - Use established patterns (dots, lines)
   - Size appropriately for splitter thickness

### Accessibility

1. **Ensure Visibility:**
   - Don't rely solely on color
   - Provide tactile feedback (cursor change)
   - Use sufficient contrast ratios

2. **Keyboard Support:**
   - SfGridSplitter has built-in keyboard support
   - Arrow keys move splitter by KeyboardIncrement
   - Ensure focus is visible

## Troubleshooting

### Preview Style Not Showing

**Problem:** Custom PreviewStyle defined but not visible during drag.

**Solutions:**
- Verify `ShowsPreview="True"` is set
- Check template's visual elements are not collapsed
- Ensure Opacity is > 0
- Test with simple solid color first

### Gripper Template Not Visible

**Problem:** Custom gripper template defined but not appearing.

**Solutions:**
- Ensure template matches orientation (Horizontal vs Vertical)
- Verify splitter size accommodates gripper dimensions
- Check template elements have explicit sizes
- Test with simple shapes first (Rectangle, Ellipse)

### Performance Issues with Custom Templates

**Problem:** Lag when dragging splitter with custom templates.

**Solutions:**
- Enable ShowsPreview mode
- Simplify template complexity (fewer effects, gradients)
- Use static resources instead of dynamic
- Avoid animations in gripper templates

## Summary

Key customization capabilities:
- **Background:** Simple color changes to match themes
- **Gripper Templates:** Custom drag indicators for horizontal and vertical splitters
- **Preview Style:** Custom appearance for deferred resize preview
- **ShowsPreview:** Toggle between real-time and preview modes
- **Combined Styling:** Mix grippers, collapse buttons, and previews

Choose customization level based on:
- Application design requirements
- Performance constraints
- User experience goals
- Development time available
