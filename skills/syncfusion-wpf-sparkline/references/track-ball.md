# Track Ball

## Table of Contents
- [Overview](#overview)
- [Enabling Track Ball](#enabling-track-ball)
- [Track Ball Customization](#track-ball-customization)
- [Adding Labels to Track Ball](#adding-labels-to-track-ball)
- [Track Ball Event Handling](#track-ball-event-handling)
- [Styling Track Ball Appearance](#styling-track-ball-appearance)
- [Best Practices](#best-practices)

## Overview

The track ball feature provides interactive data inspection by displaying a marker and vertical line at the mouse cursor position, allowing users to view exact data values on hover without cluttering the sparkline with permanent labels.

**Track Ball Availability:**
- ✓ **Line Sparkline** (SfLineSparkline)
- ✓ **Area Sparkline** (SfAreaSparkline)
- ✗ **Column Sparkline** (not supported)
- ✗ **WinLoss Sparkline** (not supported)

Track ball is ideal for:
- Inspecting exact values in dense data
- Comparing values across multiple sparklines
- Interactive dashboards and reports
- Hover-based data exploration

## Enabling Track Ball

Enable the track ball feature by setting the `ShowTrackBall` property to `True`.

### XAML Implementation

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowTrackBall="True">
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
using Syncfusion.UI.Xaml.Charts;

SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    ShowTrackBall = true
};
```

### Default Behavior

When enabled, the track ball displays:
- **Vertical line** tracking mouse position
- **Circular marker** at the nearest data point
- Automatic snapping to closest data point

### Track Ball with Area Sparkline

```xml
<syncfusion:SfAreaSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowTrackBall="True"
    Interior="LightBlue">
</syncfusion:SfAreaSparkline>
```

Track ball works identically for both Line and Area sparklines.

## Track Ball Customization

Customize the track ball's appearance using `TrackBallStyle` (for the marker) and `LineStyle` (for the vertical line).

### Customizing Track Ball Marker

Define a style for the track ball marker (Ellipse):

```xml
<Window.Resources>
    <Style TargetType="Ellipse" x:Key="trackBallMarkerStyle">
        <Setter Property="Fill" Value="Blue"/>
        <Setter Property="Height" Value="12"/>
        <Setter Property="Width" Value="12"/>
        <Setter Property="Stroke" Value="White"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
</Window.Resources>

<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowTrackBall="True"
    TrackBallStyle="{StaticResource trackBallMarkerStyle}">
</syncfusion:SfLineSparkline>
```

### Customizing Track Ball Line

Define a style for the vertical tracking line:

```xml
<Window.Resources>
    <Style TargetType="Line" x:Key="trackBallLineStyle">
        <Setter Property="Stroke" Value="Blue"/>
        <Setter Property="StrokeThickness" Value="2"/>
        <Setter Property="StrokeDashArray" Value="1,2"/>
    </Style>
</Window.Resources>

<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowTrackBall="True"
    LineStyle="{StaticResource trackBallLineStyle}">
</syncfusion:SfLineSparkline>
```

### Complete Customization Example

```xml
<Window.Resources>
    <!-- Track ball marker style -->
    <Style TargetType="Ellipse" x:Key="customMarker">
        <Setter Property="Fill" Value="Red"/>
        <Setter Property="Height" Value="14"/>
        <Setter Property="Width" Value="14"/>
        <Setter Property="Stroke" Value="DarkRed"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
    
    <!-- Track ball line style -->
    <Style TargetType="Line" x:Key="customLine">
        <Setter Property="Stroke" Value="Red"/>
        <Setter Property="StrokeThickness" Value="2"/>
        <Setter Property="StrokeDashArray" Value="2,2"/>
    </Style>
</Window.Resources>

<syncfusion:SfLineSparkline 
    Height="250" 
    Width="350" 
    Interior="#4a4a4a"   
    BorderBrush="DarkGray" 
    BorderThickness="1"
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowTrackBall="True" 
    TrackBallStyle="{StaticResource customMarker}" 
    LineStyle="{StaticResource customLine}">
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    ShowTrackBall = true,
    Interior = new SolidColorBrush(Color.FromRgb(0x4a, 0x4a, 0x4a)),
    BorderBrush = new SolidColorBrush(Colors.DarkGray),
    BorderThickness = new Thickness(1),
    Height = 250,
    Width = 350,
    TrackBallStyle = this.Resources["customMarker"] as Style,
    LineStyle = this.Resources["customLine"] as Style
};
```

## Adding Labels to Track Ball

To display data values alongside the track ball, handle the `OnSparklineMouseMove` event and add custom labels dynamically.

### Step 1: Subscribe to Mouse Move Event

```xml
<syncfusion:SfLineSparkline 
    x:Name="sparkline"
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowTrackBall="True"
    OnSparklineMouseMove="SfLineSparkline_OnSparklineMouseMove">
</syncfusion:SfLineSparkline>
```

### Step 2: Event Handler Implementation

```csharp
using Syncfusion.UI.Xaml.Charts;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;

private ContentPresenter info;

private void SfLineSparkline_OnSparklineMouseMove(object src, SparklineMouseMoveEventArgs args)
{
    // Create label on first use
    if (!args.RootPanel.Children.Contains(info))
    {
        info = new ContentPresenter();
        args.RootPanel.Children.Add(info);
        
        // Style the label
        TextBlock.SetForeground(info, new SolidColorBrush(Colors.Red));
        TextBlock.SetFontSize(info, 25);
        TextBlock.SetFontWeight(info, FontWeights.Bold);
    }
    
    // Update label content with Y-value
    info.Content = args.Value.Y;
    
    // Position label near track ball
    info.Arrange(new Rect(
        args.Coordinate.X, 
        args.Coordinate.Y, 
        info.ActualWidth, 
        info.ActualHeight));
}
```

### Event Arguments

The `SparklineMouseMoveEventArgs` provides:

| Property | Type | Description |
|----------|------|-------------|
| **RootPanel** | Panel | Container panel for adding custom elements |
| **Value** | Point | Data point value (X and Y coordinates) |
| **Coordinate** | Point | Screen coordinates for positioning |

### Advanced Label Customization

**Formatted Value Display:**
```csharp
private void SfLineSparkline_OnSparklineMouseMove(object src, SparklineMouseMoveEventArgs args)
{
    if (!args.RootPanel.Children.Contains(info))
    {
        info = new ContentPresenter();
        args.RootPanel.Children.Add(info);
        
        TextBlock.SetForeground(info, new SolidColorBrush(Colors.White));
        TextBlock.SetFontSize(info, 14);
        TextBlock.SetBackground(info, new SolidColorBrush(Color.FromArgb(200, 0, 0, 0)));
        TextBlock.SetPadding(info, new Thickness(5));
    }
    
    // Format as currency
    info.Content = string.Format("${0:N0}", args.Value.Y);
    
    // Position above track ball
    double xPos = args.Coordinate.X - (info.ActualWidth / 2);
    double yPos = args.Coordinate.Y - info.ActualHeight - 10;
    
    info.Arrange(new Rect(xPos, yPos, info.ActualWidth, info.ActualHeight));
}
```

**Multi-Line Label:**
```csharp
private void SfLineSparkline_OnSparklineMouseMove(object src, SparklineMouseMoveEventArgs args)
{
    if (!args.RootPanel.Children.Contains(info))
    {
        info = new ContentPresenter();
        args.RootPanel.Children.Add(info);
    }
    
    // Create multi-line label
    TextBlock textBlock = new TextBlock
    {
        Foreground = new SolidColorBrush(Colors.White),
        Background = new SolidColorBrush(Color.FromArgb(230, 0, 0, 0)),
        Padding = new Thickness(8),
        FontSize = 12
    };
    
    textBlock.Inlines.Add(new Run { Text = "Value: ", FontWeight = FontWeights.Bold });
    textBlock.Inlines.Add(new Run { Text = args.Value.Y.ToString("N2") });
    textBlock.Inlines.Add(new LineBreak());
    textBlock.Inlines.Add(new Run { Text = "Index: ", FontWeight = FontWeights.Bold });
    textBlock.Inlines.Add(new Run { Text = args.Value.X.ToString("F0") });
    
    info.Content = textBlock;
    
    double xPos = args.Coordinate.X + 15;
    double yPos = args.Coordinate.Y - 10;
    
    info.Arrange(new Rect(xPos, yPos, info.ActualWidth, info.ActualHeight));
}
```

## Track Ball Event Handling

The `OnSparklineMouseMove` event fires continuously as the mouse moves over the sparkline.

### Event Lifecycle

1. **Mouse enters sparkline** - Event starts firing
2. **Mouse moves over sparkline** - Event fires for each position change
3. **Track ball snaps to nearest data point** - Event provides point data
4. **Mouse exits sparkline** - Event stops firing (track ball disappears)

### Handling Multiple Sparklines

Track the same data point across multiple sparklines:

```csharp
private ContentPresenter label1, label2, label3;

private void Sparkline1_OnSparklineMouseMove(object src, SparklineMouseMoveEventArgs args)
{
    UpdateLabel(ref label1, args, Colors.Red);
}

private void Sparkline2_OnSparklineMouseMove(object src, SparklineMouseMoveEventArgs args)
{
    UpdateLabel(ref label2, args, Colors.Blue);
}

private void Sparkline3_OnSparklineMouseMove(object src, SparklineMouseMoveEventArgs args)
{
    UpdateLabel(ref label3, args, Colors.Green);
}

private void UpdateLabel(ref ContentPresenter label, SparklineMouseMoveEventArgs args, Color color)
{
    if (!args.RootPanel.Children.Contains(label))
    {
        label = new ContentPresenter();
        args.RootPanel.Children.Add(label);
        TextBlock.SetForeground(label, new SolidColorBrush(color));
        TextBlock.SetFontSize(label, 16);
    }
    
    label.Content = args.Value.Y.ToString("N0");
    label.Arrange(new Rect(args.Coordinate.X, args.Coordinate.Y, 
                           label.ActualWidth, label.ActualHeight));
}
```

## Styling Track Ball Appearance

### Solid Line Style

```xml
<Style TargetType="Line" x:Key="solidLine">
    <Setter Property="Stroke" Value="Orange"/>
    <Setter Property="StrokeThickness" Value="3"/>
</Style>
```

### Dashed Line Style

```xml
<Style TargetType="Line" x:Key="dashedLine">
    <Setter Property="Stroke" Value="Purple"/>
    <Setter Property="StrokeThickness" Value="2"/>
    <Setter Property="StrokeDashArray" Value="4,2"/>
</Style>
```

### Dotted Line Style

```xml
<Style TargetType="Line" x:Key="dottedLine">
    <Setter Property="Stroke" Value="Green"/>
    <Setter Property="StrokeThickness" Value="2"/>
    <Setter Property="StrokeDashArray" Value="1,2"/>
</Style>
```

### Custom Marker Shapes

**Square Marker:**
```xml
<!-- Note: Track ball uses Ellipse, so use Fill and Stroke -->
<Style TargetType="Ellipse" x:Key="squareMarker">
    <Setter Property="Fill" Value="Red"/>
    <Setter Property="Height" Value="10"/>
    <Setter Property="Width" Value="10"/>
    <Setter Property="Stroke" Value="White"/>
    <Setter Property="StrokeThickness" Value="2"/>
</Style>
```

**Large Highlighted Marker:**
```xml
<Style TargetType="Ellipse" x:Key="largeMarker">
    <Setter Property="Fill" Value="Yellow"/>
    <Setter Property="Height" Value="18"/>
    <Setter Property="Width" Value="18"/>
    <Setter Property="Stroke" Value="Orange"/>
    <Setter Property="StrokeThickness" Value="3"/>
    <Setter Property="Opacity" Value="0.8"/>
</Style>
```

## Best Practices

### When to Use Track Ball

**Use track ball when:**
- Users need to inspect exact values
- Multiple sparklines need coordinated inspection
- Data density is high (markers would clutter)
- Interactive exploration is a priority
- Hover-based UI patterns are appropriate

**Avoid track ball when:**
- Using Column or WinLoss sparklines (not supported)
- Touch-based interfaces (hover doesn't work)
- Printing or static reports (track ball won't appear)
- Performance is critical with many sparklines

### Performance Considerations

**Event Handler Optimization:**
```csharp
private void SfLineSparkline_OnSparklineMouseMove(object src, SparklineMouseMoveEventArgs args)
{
    // Cache the label creation (already shown above)
    // Avoid heavy computations in this event
    // Use string formatting sparingly
    
    // Good: Simple display
    info.Content = args.Value.Y;
    
    // Acceptable: Light formatting
    info.Content = args.Value.Y.ToString("N0");
    
    // Avoid: Heavy processing per mouse move
    // Don't query databases, make API calls, or perform complex calculations
}
```

### Label Positioning

**Avoid Label Clipping:**
```csharp
private void SfLineSparkline_OnSparklineMouseMove(object src, SparklineMouseMoveEventArgs args)
{
    if (!args.RootPanel.Children.Contains(info))
    {
        info = new ContentPresenter();
        args.RootPanel.Children.Add(info);
        TextBlock.SetForeground(info, new SolidColorBrush(Colors.White));
        TextBlock.SetBackground(info, new SolidColorBrush(Color.FromArgb(200, 0, 0, 0)));
        TextBlock.SetPadding(info, new Thickness(5));
    }
    
    info.Content = args.Value.Y.ToString("N0");
    
    // Ensure label stays within bounds
    double xPos = args.Coordinate.X + 10;
    double yPos = args.Coordinate.Y - 10;
    
    // Prevent right edge clipping
    if (xPos + info.ActualWidth > args.RootPanel.ActualWidth)
        xPos = args.Coordinate.X - info.ActualWidth - 10;
    
    // Prevent top edge clipping
    if (yPos < 0)
        yPos = args.Coordinate.Y + 10;
    
    info.Arrange(new Rect(xPos, yPos, info.ActualWidth, info.ActualHeight));
}
```

### Combining with Markers

Track ball and markers complement each other:

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    MarkerVisibility="Visible"
    ShowTrackBall="True">
    
    <!-- Permanent markers for special points -->
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            HighPointBrush="Red"
            LowPointBrush="Blue"
            MarkerHeight="10" 
            MarkerWidth="10"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

**Use case:** Markers permanently highlight high/low points, track ball shows all values interactively.

### Accessibility

- Track ball is mouse-dependent; provide alternative value display for keyboard/screen reader users
- Use sufficient contrast for track ball line and marker
- Consider adding tooltip as fallback for touch devices

### Theming

Track ball styles respect WPF themes. To ensure consistent appearance across themes:

```xml
<Style TargetType="Ellipse" x:Key="themedMarker">
    <Setter Property="Fill" Value="{DynamicResource AccentColorBrush}"/>
    <Setter Property="Height" Value="12"/>
    <Setter Property="Width" Value="12"/>
    <Setter Property="Stroke" Value="{DynamicResource ForegroundBrush}"/>
    <Setter Property="StrokeThickness" Value="2"/>
</Style>
```

## Next Steps

- **[Markers](markers.md)** - Combine track ball with permanent markers
- **[Segment Customization](segment-customization.md)** - Color-code data points
- **[Range Band](range-band.md)** - Highlight target ranges
