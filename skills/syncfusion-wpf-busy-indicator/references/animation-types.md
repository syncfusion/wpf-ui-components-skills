# Animation Types and Speed Configuration

The SfBusyIndicator control provides over 37 built-in animations to match different use cases and application styles. This guide covers the AnimationType and AnimationSpeed properties.

## AnimationType Property

The `AnimationType` property allows you to select from a rich collection of pre-built animations. Each animation provides a unique visual style for indicating busy states.

### Available Animation Types

The control includes these animation types (partial list of most commonly used):

- **Ball** - Bouncing ball animation
- **Battery** - Battery charging animation
- **Box** - Rotating box animation
- **DoubleCircle** - Double circular rotation
- **ECG** - Heartbeat/ECG line animation
- **Flight** - Flying plane animation
- **Gear** - Rotating gear animation
- **Globe** - Rotating globe animation
- **Hurricane** - Spinning hurricane animation
- **Movie Timer** - Film reel animation
- **Print** - Printer animation
- **Rain** - Falling rain drops
- **Rectangle** - Rotating rectangles
- **RollingBall** - Rolling ball animation
- **SingleCircle** - Single circular rotation
- **Slider** - Sliding animation
- **Snowflakes** - Falling snowflakes
- **Spin** - Classic spinner animation
- **Sun** - Rotating sun animation
- **Cupertino** - iOS-style spinner
- **Fluent** - Fluent design animation
- And many more...

### Setting Animation Type in XAML

```xml
<!-- Flight animation -->
<syncfusion:SfBusyIndicator IsBusy="True" 
                             AnimationType="Flight"
                             Background="CornflowerBlue"/>

<!-- DoubleCircle animation -->
<syncfusion:SfBusyIndicator IsBusy="True"
                             AnimationType="DoubleCircle"
                             Foreground="DarkBlue"/>

<!-- Spin animation -->
<syncfusion:SfBusyIndicator IsBusy="True"
                             AnimationType="Spin"/>
```

### Setting Animation Type in C#

```csharp
using Syncfusion.Windows.Controls.Notification;

// Create and configure busy indicator
SfBusyIndicator busyIndicator = new SfBusyIndicator();
busyIndicator.IsBusy = true;
busyIndicator.AnimationType = AnimationTypes.Flight;

// Programmatically change animation type
busyIndicator.AnimationType = AnimationTypes.DoubleCircle;
```

### Setting Animation Type in VB.NET

```vb
Imports Syncfusion.Windows.Controls.Notification

' Create and configure busy indicator
Dim busyIndicator As New SfBusyIndicator()
busyIndicator.IsBusy = True
busyIndicator.AnimationType = AnimationTypes.Flight

' Programmatically change animation type
busyIndicator.AnimationType = AnimationTypes.DoubleCircle
```

## AnimationSpeed Property

The `AnimationSpeed` property controls how fast the selected animation runs. It accepts a numeric multiplier where:
- **1.0** = Normal speed (default)
- **> 1.0** = Faster animation
- **< 1.0** = Slower animation

**Important:** AnimationSpeed does NOT apply to the `Fluent` animation type.

### Speed Configuration Examples

#### XAML Speed Configuration

```xml
<!-- Normal speed (default) -->
<syncfusion:SfBusyIndicator AnimationType="Spin" 
                             AnimationSpeed="1.0"/>

<!-- Faster animation (2x speed) -->
<syncfusion:SfBusyIndicator AnimationType="DoubleCircle"
                             AnimationSpeed="2.0"/>

<!-- Slower animation (half speed) -->
<syncfusion:SfBusyIndicator AnimationType="Ball"
                             AnimationSpeed="0.5"/>

<!-- Very fast animation -->
<syncfusion:SfBusyIndicator AnimationType="ECG"
                             AnimationSpeed="3.0"/>
```

#### C# Speed Configuration

```csharp
SfBusyIndicator busyIndicator = new SfBusyIndicator();
busyIndicator.AnimationType = AnimationTypes.Spin;
busyIndicator.AnimationSpeed = 1.5; // 50% faster

// For quick operations, use faster animations
busyIndicator.AnimationSpeed = 2.5;

// For long operations, use slower animations
busyIndicator.AnimationSpeed = 0.75;
```

#### VB.NET Speed Configuration

```vb
Dim busyIndicator As New SfBusyIndicator()
busyIndicator.AnimationType = AnimationTypes.Spin
busyIndicator.AnimationSpeed = 1.5 ' 50% faster

' For quick operations, use faster animations
busyIndicator.AnimationSpeed = 2.5

' For long operations, use slower animations
busyIndicator.AnimationSpeed = 0.75
```

## Complete Animation Gallery Example

This example creates a selection UI to preview different animations:

```xml
<Window x:Class="AnimationDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Controls.Notification;assembly=Syncfusion.SfBusyIndicator.WPF"
        Title="Animation Gallery" Height="500" Width="600">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <!-- Animation selector -->
        <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
            <TextBlock Text="Animation Type:" VerticalAlignment="Center" Margin="0,0,10,0"/>
            <ComboBox x:Name="animationSelector" Width="150" SelectedIndex="0"
                      SelectionChanged="OnAnimationChanged">
                <ComboBoxItem Content="Flight"/>
                <ComboBoxItem Content="DoubleCircle"/>
                <ComboBoxItem Content="Spin"/>
                <ComboBoxItem Content="Ball"/>
                <ComboBoxItem Content="ECG"/>
                <ComboBoxItem Content="Globe"/>
                <ComboBoxItem Content="Rain"/>
            </ComboBox>
        </StackPanel>
        
        <!-- Speed selector -->
        <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="10">
            <TextBlock Text="Animation Speed:" VerticalAlignment="Center" Margin="0,0,10,0"/>
            <Slider x:Name="speedSlider" Width="200" Minimum="0.5" Maximum="3.0" 
                    Value="1.0" TickFrequency="0.5" IsSnapToTickEnabled="True"
                    ValueChanged="OnSpeedChanged"/>
            <TextBlock Text="{Binding ElementName=speedSlider, Path=Value, StringFormat='N1'}" 
                       VerticalAlignment="Center" Margin="10,0,0,0"/>
        </StackPanel>
        
        <!-- Busy indicator display -->
        <Grid Grid.Row="2" Background="CornflowerBlue">
            <syncfusion:SfBusyIndicator x:Name="busyIndicator"
                                         IsBusy="True"
                                         AnimationType="Flight"
                                         Header="Preview Animation"
                                         Foreground="White"/>
        </Grid>
    </Grid>
</Window>
```

```csharp
private void OnAnimationChanged(object sender, SelectionChangedEventArgs e)
{
    if (busyIndicator != null && animationSelector.SelectedItem is ComboBoxItem item)
    {
        string animationType = item.Content.ToString();
        busyIndicator.AnimationType = (AnimationTypes)Enum.Parse(typeof(AnimationTypes), animationType);
    }
}

private void OnSpeedChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    if (busyIndicator != null)
    {
        busyIndicator.AnimationSpeed = e.NewValue;
    }
}
```

## Animation Selection Guidelines

### Choose animation based on context:

**Professional/Business Applications:**
- **DoubleCircle** - Clean, modern look
- **Spin** - Classic, familiar
- **SingleCircle** - Minimal, subtle

**Creative/Playful Applications:**
- **Flight** - Dynamic, engaging
- **Rain** - Atmospheric
- **Snowflakes** - Seasonal, friendly

**Technical/Industrial:**
- **Gear** - Mechanical, processing
- **Battery** - Power-related operations
- **ECG** - Monitoring, real-time data

**Quick Operations (< 2 seconds):**
- Use faster speeds (1.5 - 2.5)
- Simple animations (Spin, SingleCircle)

**Long Operations (> 10 seconds):**
- Use normal or slower speeds (0.75 - 1.0)
- Engaging animations (Flight, Globe, Rain)

## Platform-Specific Notes

### Fluent Animation Type

The **Fluent** animation type is unique:

```xml
<syncfusion:SfBusyIndicator AnimationType="Fluent" IsBusy="True"/>
```

**Important:** The `AnimationSpeed` property has NO EFFECT on the Fluent animation. It runs at a fixed speed optimized for the Fluent design system.

```csharp
// AnimationSpeed is ignored for Fluent
busyIndicator.AnimationType = AnimationTypes.Fluent;
busyIndicator.AnimationSpeed = 2.0; // This will be IGNORED
```

### Animation Performance

Different animations have different performance characteristics:

**Lightweight (best for frequent use):**
- Spin, SingleCircle, DoubleCircle

**Medium weight:**
- Ball, ECG, Rectangle

**Complex (use sparingly):**
- Flight, Rain, Snowflakes (particle-based)

## Dynamic Animation Switching

You can change animations dynamically based on operation type:

```csharp
public class SmartBusyIndicatorViewModel : ViewModelBase
{
    private AnimationTypes _currentAnimation;
    public AnimationTypes CurrentAnimation
    {
        get => _currentAnimation;
        set => SetProperty(ref _currentAnimation, value);
    }
    
    private double _animationSpeed;
    public double AnimationSpeed
    {
        get => _animationSpeed;
        set => SetProperty(ref _animationSpeed, value);
    }
    
    public async Task PerformOperationAsync(OperationType type)
    {
        // Select animation based on operation
        switch (type)
        {
            case OperationType.Loading:
                CurrentAnimation = AnimationTypes.DoubleCircle;
                AnimationSpeed = 1.5;
                break;
            case OperationType.Processing:
                CurrentAnimation = AnimationTypes.Gear;
                AnimationSpeed = 1.0;
                break;
            case OperationType.Uploading:
                CurrentAnimation = AnimationTypes.Flight;
                AnimationSpeed = 1.2;
                break;
        }
        
        await ExecuteOperationAsync();
    }
}
```

## Visual Animation Reference

For a complete visual reference of all 37+ animations, see the animated GIF in the official documentation or run the sample application:

**GitHub Sample:** https://github.com/SyncfusionExamples/wpf-BusyIndicator-examples/tree/master/Samples/AnimationType

## Best Practices

1. **Match animation to duration** - Use faster animations for quick operations, slower for long ones
2. **Be consistent** - Use the same animation type throughout your application for similar operations
3. **Consider context** - Choose animations that match your application's tone and industry
4. **Test performance** - Complex animations may impact performance on older hardware
5. **Avoid Fluent speed adjustments** - Remember AnimationSpeed doesn't work with Fluent type
6. **User preferences** - Consider allowing users to choose animation types in settings

## Troubleshooting

**Issue:** Animation appears too fast or slow
- **Solution:** Adjust AnimationSpeed property (except for Fluent type)

**Issue:** Fluent animation doesn't respond to speed changes
- **Solution:** This is expected behavior - Fluent runs at fixed speed

**Issue:** Animation appears choppy
- **Solution:** Try a simpler animation type or check system performance

**Issue:** Animation doesn't change when AnimationType is set
- **Solution:** Ensure IsBusy is True and the property change is triggering correctly
