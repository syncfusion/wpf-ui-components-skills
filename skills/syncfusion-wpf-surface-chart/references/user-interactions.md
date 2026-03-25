# User Interactions in WPF Surface Chart

## Table of Contents
- [Overview](#overview)
- [Zooming](#zooming)
- [Rotation](#rotation)
- [Tilt](#tilt)
- [Combining Interactions](#combining-interactions)
- [Programmatic Control](#programmatic-control)
- [Best Practices](#best-practices)

## Overview

The Syncfusion WPF Surface Chart allows users to interact with 3D visualizations through zooming and rotation, enabling better exploration and understanding of data from multiple perspectives.

**Available Interactions:**
- **Zooming** - Zoom in/out to see details or overview
- **Rotation** - Rotate around vertical axis to view from different angles
- **Tilt** - Adjust viewing angle (top-down to side view)

These interactions can be enabled for user control or set programmatically for specific viewpoints.

## Zooming

### Overview

Zooming allows users to zoom in for detailed views or zoom out for overview perspectives. It's controlled through the EnableZooming property.

**Zoom Methods:**
1. **Pinch Zooming** - Touch gesture support (on touch devices)
2. **Mouse Wheel** - Scroll to zoom (on desktop)
3. **Programmatic** - Set ZoomLevel property in code

### Enabling Zooming

**XAML:**
```xaml
<chart:SfSurfaceChart EnableZooming="True">
</chart:SfSurfaceChart>
```

**Code-Behind:**
```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.EnableZooming = true;
grid.Children.Add(chart);
```

**User Actions:**
- **Mouse Wheel Up** - Zoom in (closer)
- **Mouse Wheel Down** - Zoom out (farther)
- **Pinch Gesture** - Zoom in/out on touch devices

### Programmatic Zoom Level

Set the initial or current zoom level programmatically:

**XAML:**
```xaml
<chart:SfSurfaceChart EnableZooming="True" ZoomLevel="0.5">
</chart:SfSurfaceChart>
```

**Code-Behind:**
```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.EnableZooming = true;
chart.ZoomLevel = 0.5;  // Range: 0.0 (far) to 1.0 (close)
grid.Children.Add(chart);
```

**ZoomLevel Values:**
- `0.0` - Maximum zoom out (farthest view)
- `0.5` - Medium zoom (default comfortable view)
- `1.0` - Maximum zoom in (closest view)

### Complete Zooming Example

```xaml
<chart:SfSurfaceChart Type="Surface"
                     ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     EnableZooming="True"
                     ZoomLevel="0.3">
    <!-- Chart content -->
</chart:SfSurfaceChart>
```

### Zoom with UI Control

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
        <Label Content="Zoom Level:"/>
        <Slider Name="ZoomSlider"
                Width="200"
                Minimum="0"
                Maximum="1"
                Value="0.5"
                ValueChanged="ZoomSlider_ValueChanged"/>
    </StackPanel>
    
    <chart:SfSurfaceChart Name="chart"
                         Grid.Row="1"
                         EnableZooming="True"/>
</Grid>
```

```csharp
private void ZoomSlider_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    if (chart != null)
    {
        chart.ZoomLevel = e.NewValue;
    }
}
```

## Rotation

### Overview

Surface chart can be rotated around the vertical (Y) axis for better visualization of all axes and data points. Rotation is controlled through the EnableRotation property.

**Rotation Methods:**
1. **Mouse Drag** - Click and drag to rotate (when enabled)
2. **Programmatic** - Set Rotate property in code

### Enabling Rotation

**XAML:**
```xaml
<chart:SfSurfaceChart EnableRotation="True">
</chart:SfSurfaceChart>
```

**Code-Behind:**
```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.EnableRotation = true;
grid.Children.Add(chart);
```

**User Actions:**
- **Click and Drag** - Rotate chart horizontally

### Programmatic Rotation Angle

Set the rotation angle programmatically:

**XAML:**
```xaml
<chart:SfSurfaceChart EnableRotation="True" Rotate="50">
</chart:SfSurfaceChart>
```

**Code-Behind:**
```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.EnableRotation = true;
chart.Rotate = 50;  // 50 degrees rotation
grid.Children.Add(chart);
```

**Rotate Values:**
- `0°` - Front view (default)
- `90°` - Side view (left)
- `180°` - Back view
- `270°` - Side view (right)
- `360°` - Full rotation (returns to front)

### Common Rotation Angles

```csharp
// Front-right angle (common starting point)
chart.Rotate = 30;

// Side view
chart.Rotate = 90;

// Front-left angle
chart.Rotate = 330;  // or -30

// Isometric-like view
chart.Rotate = 45;
```

### Rotation with UI Control

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
        <Label Content="Rotation:"/>
        <Slider Name="RotationSlider"
                Width="200"
                Minimum="0"
                Maximum="360"
                Value="30"
                ValueChanged="RotationSlider_ValueChanged"/>
        <TextBlock Text="{Binding ElementName=RotationSlider, Path=Value, StringFormat={}{0:F0}°}"
                   VerticalAlignment="Center"
                   Margin="10,0,0,0"/>
    </StackPanel>
    
    <chart:SfSurfaceChart Name="chart"
                         Grid.Row="1"
                         EnableRotation="True"/>
</Grid>
```

```csharp
private void RotationSlider_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    if (chart != null)
    {
        chart.Rotate = e.NewValue;
    }
}
```

## Tilt

### Overview

The Tilt property adjusts the viewing angle from top-down to side view, providing different perspectives on the data.

**Tilt Range:**
- `0°` - Top-down view (bird's eye)
- `30°` - Slight angle (common for visualization)
- `60°` - Steep angle
- `90°` - Side view (horizontal)

### Setting Tilt

**XAML:**
```xaml
<chart:SfSurfaceChart Tilt="40">
</chart:SfSurfaceChart>
```

**Code-Behind:**
```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Tilt = 40;
grid.Children.Add(chart);
```

### Common Tilt Angles

```csharp
// Top-down view (for contours)
chart.Tilt = 0;

// Slight 3D effect
chart.Tilt = 15;

// Standard 3D view
chart.Tilt = 30;

// Dramatic 3D view
chart.Tilt = 60;

// Side view
chart.Tilt = 90;
```

### Tilt with UI Control

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
        <Label Content="Tilt:"/>
        <Slider Name="TiltSlider"
                Width="200"
                Minimum="0"
                Maximum="90"
                Value="20"
                ValueChanged="TiltSlider_ValueChanged"/>
        <TextBlock Text="{Binding ElementName=TiltSlider, Path=Value, StringFormat={}{0:F0}°}"
                   VerticalAlignment="Center"
                   Margin="10,0,0,0"/>
    </StackPanel>
    
    <chart:SfSurfaceChart Name="chart" Grid.Row="1"/>
</Grid>
```

```csharp
private void TiltSlider_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    if (chart != null)
    {
        chart.Tilt = e.NewValue;
    }
}
```

## Combining Interactions

### All Interactions Enabled

```xaml
<chart:SfSurfaceChart EnableRotation="True"
                     EnableZooming="True"
                     Rotate="30"
                     Tilt="20"
                     ZoomLevel="0.5">
</chart:SfSurfaceChart>
```

```csharp
SfSurfaceChart chart = new SfSurfaceChart();

// Enable user interactions
chart.EnableRotation = true;
chart.EnableZooming = true;

// Set initial view
chart.Rotate = 30;
chart.Tilt = 20;
chart.ZoomLevel = 0.5;

grid.Children.Add(chart);
```

**User Capabilities:**
- Mouse drag to rotate
- Mouse wheel to zoom
- Programmatic tilt control

### Interactive Exploration Interface

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <!-- Control Panel -->
    <StackPanel Grid.Row="0" Margin="10">
        <StackPanel Orientation="Horizontal" Margin="0,5">
            <Label Content="Rotation:" Width="80"/>
            <Slider Name="RotationSlider" Width="200" Minimum="0" Maximum="360" Value="30"/>
            <TextBlock Text="{Binding ElementName=RotationSlider, Path=Value, StringFormat={}{0:F0}°}" 
                      Width="50" VerticalAlignment="Center" Margin="10,0,0,0"/>
        </StackPanel>
        
        <StackPanel Orientation="Horizontal" Margin="0,5">
            <Label Content="Tilt:" Width="80"/>
            <Slider Name="TiltSlider" Width="200" Minimum="0" Maximum="90" Value="20"/>
            <TextBlock Text="{Binding ElementName=TiltSlider, Path=Value, StringFormat={}{0:F0}°}" 
                      Width="50" VerticalAlignment="Center" Margin="10,0,0,0"/>
        </StackPanel>
        
        <StackPanel Orientation="Horizontal" Margin="0,5">
            <Label Content="Zoom:" Width="80"/>
            <Slider Name="ZoomSlider" Width="200" Minimum="0" Maximum="1" Value="0.5"/>
            <TextBlock Text="{Binding ElementName=ZoomSlider, Path=Value, StringFormat={}{0:P0}}" 
                      Width="50" VerticalAlignment="Center" Margin="10,0,0,0"/>
        </StackPanel>
        
        <StackPanel Orientation="Horizontal" Margin="0,5">
            <CheckBox Name="EnableRotationCheckBox" Content="Enable Mouse Rotation" IsChecked="True" Margin="0,0,20,0"/>
            <CheckBox Name="EnableZoomingCheckBox" Content="Enable Mouse Zoom" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    
    <!-- Chart -->
    <chart:SfSurfaceChart Name="chart"
                         Grid.Row="1"
                         Rotate="{Binding ElementName=RotationSlider, Path=Value}"
                         Tilt="{Binding ElementName=TiltSlider, Path=Value}"
                         ZoomLevel="{Binding ElementName=ZoomSlider, Path=Value}"
                         EnableRotation="{Binding ElementName=EnableRotationCheckBox, Path=IsChecked}"
                         EnableZooming="{Binding ElementName=EnableZoomingCheckBox, Path=IsChecked}"/>
</Grid>
```

## Programmatic Control

### Preset View Buttons

```xaml
<StackPanel Orientation="Horizontal" Margin="10">
    <Button Content="Front View" Click="FrontView_Click" Margin="5"/>
    <Button Content="Side View" Click="SideView_Click" Margin="5"/>
    <Button Content="Top View" Click="TopView_Click" Margin="5"/>
    <Button Content="Isometric" Click="IsometricView_Click" Margin="5"/>
    <Button Content="Reset Zoom" Click="ResetZoom_Click" Margin="5"/>
</StackPanel>
```

```csharp
private void FrontView_Click(object sender, RoutedEventArgs e)
{
    chart.Rotate = 0;
    chart.Tilt = 20;
}

private void SideView_Click(object sender, RoutedEventArgs e)
{
    chart.Rotate = 90;
    chart.Tilt = 20;
}

private void TopView_Click(object sender, RoutedEventArgs e)
{
    chart.Rotate = 0;
    chart.Tilt = 0;
}

private void IsometricView_Click(object sender, RoutedEventArgs e)
{
    chart.Rotate = 45;
    chart.Tilt = 35;
}

private void ResetZoom_Click(object sender, RoutedEventArgs e)
{
    chart.ZoomLevel = 0.5;
}
```

### Animated View Changes

```csharp
using System.Windows.Media.Animation;

private void AnimateToView(double targetRotate, double targetTilt, double targetZoom)
{
    // Rotation animation
    DoubleAnimation rotateAnimation = new DoubleAnimation
    {
        To = targetRotate,
        Duration = TimeSpan.FromMilliseconds(500),
        EasingFunction = new QuadraticEase { EasingMode = EasingMode.EaseInOut }
    };
    
    // Tilt animation
    DoubleAnimation tiltAnimation = new DoubleAnimation
    {
        To = targetTilt,
        Duration = TimeSpan.FromMilliseconds(500),
        EasingFunction = new QuadraticEase { EasingMode = EasingMode.EaseInOut }
    };
    
    // Zoom animation
    DoubleAnimation zoomAnimation = new DoubleAnimation
    {
        To = targetZoom,
        Duration = TimeSpan.FromMilliseconds(500),
        EasingFunction = new QuadraticEase { EasingMode = EasingMode.EaseInOut }
    };
    
    chart.BeginAnimation(SfSurfaceChart.RotateProperty, rotateAnimation);
    chart.BeginAnimation(SfSurfaceChart.TiltProperty, tiltAnimation);
    chart.BeginAnimation(SfSurfaceChart.ZoomLevelProperty, zoomAnimation);
}

// Usage
private void IsometricView_Click(object sender, RoutedEventArgs e)
{
    AnimateToView(45, 35, 0.5);
}
```

## Best Practices

### 1. Enable Interactions for Exploration

```csharp
// For data exploration applications
chart.EnableRotation = true;
chart.EnableZooming = true;
```

### 2. Disable for Static Presentations

```csharp
// For presentations or reports
chart.EnableRotation = false;
chart.EnableZooming = false;
// Set specific viewpoint
chart.Rotate = 30;
chart.Tilt = 20;
```

### 3. Provide Good Initial View

```csharp
// Start with a clear, informative view
chart.Rotate = 30;   // Slightly rotated
chart.Tilt = 20;     // Slight tilt
chart.ZoomLevel = 0.4;  // Zoomed out for overview
```

### 4. Add View Presets

```csharp
// Provide common viewing angles
public enum ViewPreset
{
    Front,
    Side,
    Top,
    Isometric,
    Custom
}

public void ApplyViewPreset(ViewPreset preset)
{
    switch (preset)
    {
        case ViewPreset.Front:
            chart.Rotate = 0;
            chart.Tilt = 20;
            break;
        case ViewPreset.Side:
            chart.Rotate = 90;
            chart.Tilt = 20;
            break;
        case ViewPreset.Top:
            chart.Rotate = 0;
            chart.Tilt = 0;
            break;
        case ViewPreset.Isometric:
            chart.Rotate = 45;
            chart.Tilt = 35;
            break;
    }
}
```

### 5. Provide Visual Feedback

```xaml
<StatusBar DockPanel.Dock="Bottom">
    <StatusBarItem>
        <TextBlock>
            <Run Text="Rotation: "/>
            <Run Text="{Binding ElementName=chart, Path=Rotate, StringFormat={}{0:F1}°}"/>
            <Run Text=" | Tilt: "/>
            <Run Text="{Binding ElementName=chart, Path=Tilt, StringFormat={}{0:F1}°}"/>
            <Run Text=" | Zoom: "/>
            <Run Text="{Binding ElementName=chart, Path=ZoomLevel, StringFormat={}{0:P0}}"/>
        </TextBlock>
    </StatusBarItem>
</StatusBar>
```

### 6. Consider Touch Support

```csharp
// Ensure touch-friendly controls
if (SystemParameters.IsTabletPC)
{
    // Pinch zooming automatically supported
    chart.EnableZooming = true;
    // May want larger UI controls for touch
}
```

### 7. Limit Extreme Values

```csharp
// Validate user input
private void ValidateZoomLevel(double value)
{
    if (value < 0.1) value = 0.1;  // Minimum zoom
    if (value > 1.0) value = 1.0;  // Maximum zoom
    chart.ZoomLevel = value;
}
```

---