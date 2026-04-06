# Customization and Styling

## Table of Contents
````markdown
# Customization and Styling

This guide focuses on visual customization patterns for `SfGridSplitter`. Keep basic layout examples in `references/getting-started.md`; here we show focused styling and template snippets.

## Background Customization

The `Background` property controls the splitter bar color. Use a concise splitter-only example instead of full layout wrappers.

### Basic Background

```xml
<syncfusion:SfGridSplitter Background="Green"
                           HorizontalAlignment="Stretch"
                           Height="5"
                           ResizeBehavior="PreviousAndNext"/>
```

### Gradient Background

```xml
<syncfusion:SfGridSplitter HorizontalAlignment="Stretch"
                           Height="8">
    <syncfusion:SfGridSplitter.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#FF4A90E2" Offset="0"/>
            <GradientStop Color="#FF2D6FA8" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:SfGridSplitter.Background>
</syncfusion:SfGridSplitter>
```

## Gripper Templates

Customize the gripper visuals with minimal templates. Avoid duplicating full layout containers.

### Horizontal Gripper Template (minimal)

```xml
<syncfusion:SfGridSplitter.HorizontalGripperTemplate>
    <DataTemplate>
        <TextBlock Background="Blue" Foreground="White" Text="⋮⋮⋮" Padding="4"/>
    </DataTemplate>
</syncfusion:SfGridSplitter.HorizontalGripperTemplate>
```

### Vertical Gripper Template (minimal)

```xml
<syncfusion:SfGridSplitter.VerticalGripperTemplate>
    <DataTemplate>
        <TextBlock Background="Red" Foreground="White" Text="⋮" LayoutTransform="{StaticResource Rotate90}" Padding="4"/>
    </DataTemplate>
</syncfusion:SfGridSplitter.VerticalGripperTemplate>
```

### Icon-Based Gripper

```xml
<syncfusion:SfGridSplitter.HorizontalGripperTemplate>
    <DataTemplate>
        <Grid Width="50" Height="16" Background="#FF007ACC">
            <Path Data="M 0,4 L 50,4 M 0,8 L 50,8 M 0,12 L 50,12" Stroke="White" StrokeThickness="2" StrokeDashArray="2,2"/>
        </Grid>
    </DataTemplate>
</syncfusion:SfGridSplitter.HorizontalGripperTemplate>
```

## Preview Style Configuration

Use `PreviewStyle` to change the deferred-resize indicator. Keep templates concise and focused on the visual element.

### Custom Preview Style

```xml
<syncfusion:SfGridSplitter ShowsPreview="True">
    <syncfusion:SfGridSplitter.PreviewStyle>
        <Style TargetType="Control">
            <Setter Property="Background" Value="#FF0078D7"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="Control">
                        <Grid Opacity="0.7">
                            <Rectangle Fill="{TemplateBinding Background}" Height="3"/>
                        </Grid>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfGridSplitter.PreviewStyle>
</syncfusion:SfGridSplitter>
```

### Dashed Line Preview

```xml
<syncfusion:SfGridSplitter.PreviewStyle>
    <Style TargetType="Control">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="Control">
                    <Rectangle Stroke="DarkBlue" StrokeThickness="2" Fill="Transparent" StrokeDashArray="4,2"/>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</syncfusion:SfGridSplitter.PreviewStyle>
```

## ShowsPreview Property

Control whether resizing is applied in real-time or via a preview indicator. See `references/getting-started.md` for placement examples.

```xml
<syncfusion:SfGridSplitter ShowsPreview="True" HorizontalAlignment="Stretch"/>
```

## Best Practices and Accessibility

- Use `ShowsPreview` for performance-sensitive layouts.
- Keep gripper templates simple (avoid heavy effects).
- Use resource brushes for theme integration.
- Ensure keyboard accessibility and visible focus states.

## Troubleshooting

- If a custom `PreviewStyle` is not visible, verify `ShowsPreview="True"` and that template visuals are not collapsed.
- If gripper templates do not render, ensure splitter size accommodates the template and test with simple shapes first.

````
                               Width="90"

                               Height="20"/>
