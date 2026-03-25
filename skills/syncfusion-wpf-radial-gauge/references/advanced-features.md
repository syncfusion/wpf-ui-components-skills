# Advanced Features

## Table of Contents
- [Pointer Animation](#pointer-animation)
- [Pointer Dragging](#pointer-dragging)
- [Events](#events)
- [MVVM Pattern](#mvvm-pattern)
- [Real-Time Updates](#real-time-updates)
- [Performance Optimization](#performance-optimization)

## Pointer Animation

Animate pointer movements for smooth visual transitions.

### Enabling Animation

**XAML:**
```xml
<gauge:CircularPointer PointerType="NeedlePointer"
                       Value="60"
                       EnableAnimation="True"
                       AnimationDuration="500"/>
```

**C#:**
```csharp
pointer.EnableAnimation = true;
pointer.AnimationDuration = 500;  // milliseconds
```

### AnimationDuration Property

Controls animation speed in milliseconds.

**Values:**
- `200-500` - Quick animations
- `500-1000` - Standard speed
- `1000-2000` - Slow, smooth animations

### Animation for Different Pointer Types

**Needle Pointer:**
```xml
<gauge:CircularPointer PointerType="NeedlePointer"
                       EnableAnimation="True"
                       AnimationDuration="600"
                       Value="85"/>
```

**Range Pointer:**
```xml
<gauge:CircularPointer PointerType="RangePointer"
                       EnableAnimation="True"
                       AnimationDuration="1000"
                       Value="75"
                       RangePointerStroke="DeepSkyBlue"/>
```

**Symbol Pointer:**
```xml
<gauge:CircularPointer PointerType="SymbolPointer"
                       EnableAnimation="True"
                       AnimationDuration="400"
                       Value="50"/>
```

### Complete Animation Example

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Pointers>
        <!-- Animated needle -->
        <gauge:CircularPointer PointerType="NeedlePointer"
                               EnableAnimation="True"
                               AnimationDuration="500"
                               NeedleLengthFactor="0.6"
                               Value="60"/>
        
        <!-- Animated range -->
        <gauge:CircularPointer PointerType="RangePointer"
                               EnableAnimation="True"
                               AnimationDuration="1000"
                               RangePointerStroke="LightBlue"
                               Value="100"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularPointer needle = new CircularPointer
{
    PointerType = PointerType.NeedlePointer,
    EnableAnimation = true,
    AnimationDuration = 500,
    NeedleLengthFactor = 0.6,
    Value = 60
};

CircularPointer range = new CircularPointer
{
    PointerType = PointerType.RangePointer,
    EnableAnimation = true,
    AnimationDuration = 1000,
    RangePointerStroke = new SolidColorBrush(Colors.LightBlue),
    Value = 100
};
```

### Updating Animated Values

When you change the `Value` property, the pointer animates to the new position:

**C#:**
```csharp
// Pointer animates from current value to 85
myPointer.Value = 85;
```

## Pointer Dragging

Enable interactive value selection by dragging pointers.

### Enabling Dragging

**XAML:**
```xml
<gauge:CircularPointer PointerType="SymbolPointer"
                       Symbol="InvertedTriangle"
                       EnableDragging="True"
                       EnableAnimation="False"
                       Value="50"
                       ValueChanged="Pointer_ValueChanged"/>
```

**C#:**
```csharp
pointer.EnableDragging = true;
pointer.EnableAnimation = false;  // Disable for smoother dragging
pointer.ValueChanged += Pointer_ValueChanged;
```

**Note:** Typically disable animation when dragging is enabled for immediate feedback.

### Drag with Range Update

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange x:Name="dynamicRange"
                             StartValue="0"
                             EndValue="25"
                             Stroke="DeepSkyBlue"
                             StrokeThickness="30"/>
    </gauge:CircularScale.Ranges>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="SymbolPointer"
                               Symbol="InvertedTriangle"
                               EnableDragging="True"
                               Value="25"
                               ValueChanged="Pointer_ValueChanged"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
private void Pointer_ValueChanged(object sender, ValueChangedEventArgs e)
{
    dynamicRange.EndValue = e.Value;
}
```

### Complete Dragging Example

**XAML:**
```xml
<Window x:Class="GaugeApp.MainWindow">
    <Grid>
        <gauge:SfCircularGauge>
            <gauge:SfCircularGauge.Scales>
                <gauge:CircularScale StartValue="0" 
                                     EndValue="100"
                                     RimStroke="LightGray"
                                     RimStrokeThickness="30"
                                     ShowRim="True">
                    <gauge:CircularScale.Ranges>
                        <gauge:CircularRange x:Name="progressRange"
                                             StartValue="0"
                                             EndValue="50"
                                             Stroke="DeepSkyBlue"
                                             StrokeThickness="30"/>
                    </gauge:CircularScale.Ranges>
                    <gauge:CircularScale.Pointers>
                        <gauge:CircularPointer PointerType="SymbolPointer"
                                               Symbol="InvertedTriangle"
                                               EnableDragging="True"
                                               EnableAnimation="False"
                                               Value="50"
                                               SymbolPointerHeight="18"
                                               SymbolPointerWidth="18"
                                               SymbolPointerStroke="DarkBlue"
                                               ValueChanged="Pointer_ValueChanged"/>
                    </gauge:CircularScale.Pointers>
                </gauge:CircularScale>
            </gauge:SfCircularGauge.Scales>
        </gauge:SfCircularGauge>
    </Grid>
</Window>
```

**C#:**
```csharp
private void Pointer_ValueChanged(object sender, ValueChangedEventArgs e)
{
    progressRange.EndValue = e.Value;
}
```

## Events

Circular gauge provides several events for pointer interactions.

### ValueChangeStarted

Fired when user begins dragging a pointer.

**XAML:**
```xml
<gauge:CircularPointer EnableDragging="True"
                       ValueChangeStarted="Pointer_ValueChangeStarted"/>
```

**C#:**
```csharp
private void Pointer_ValueChangeStarted(object sender, ValueChangedEventArgs e)
{
    Debug.WriteLine($"Drag started at value: {e.Value}");
    // e.Value contains the pointer's value when drag began
}
```

### ValueChanging

Fired continuously during dragging. Can be canceled.

**XAML:**
```xml
<gauge:CircularPointer EnableDragging="True"
                       ValueChanging="Pointer_ValueChanging"/>
```

**C#:**
```csharp
private void Pointer_ValueChanging(object sender, ValueChangingEventArgs e)
{
    Debug.WriteLine($"Changing from {e.OldValue} to {e.NewValue}");
    
    // Restrict to range 30-70
    if (e.NewValue > 30 && e.NewValue < 70)
    {
        e.Cancel = true;  // Prevent update in this range
    }
}
```

**Properties:**
- `OldValue` - Previous pointer value
- `NewValue` - New pointer value being set
- `Cancel` - Set to true to prevent the value change

### ValueChanged

Fired after pointer value changes (drag or programmatic).

**XAML:**
```xml
<gauge:CircularPointer Value="50"
                       ValueChanged="Pointer_ValueChanged"/>
```

**C#:**
```csharp
private void Pointer_ValueChanged(object sender, ValueChangedEventArgs e)
{
    Debug.WriteLine($"Value changed to: {e.Value}");
    
    // Update UI or data model
    CurrentValueTextBlock.Text = $"{e.Value:N0}";
}
```

### ValueChangeCompleted

Fired when user finishes dragging.

**XAML:**
```xml
<gauge:CircularPointer EnableDragging="True"
                       ValueChangeCompleted="Pointer_ValueChangeCompleted"/>
```

**C#:**
```csharp
private void Pointer_ValueChangeCompleted(object sender, ValueChangedEventArgs e)
{
    Debug.WriteLine($"Drag completed at value: {e.Value}");
    
    // Save final value to database or perform action
    SaveValue(e.Value);
}
```

### PointerPositionChanged

Fired when pointer position changes, providing detailed position information.

**XAML:**
```xml
<gauge:CircularPointer PointerPositionChanged="Pointer_PointerPositionChanged"/>
```

**C#:**
```csharp
private void Pointer_PointerPositionChanged(object sender, PointerPosition pointerPosition)
{
    var pointerValue = pointerPosition.PointerValue;
    var pointerValuePos = pointerPosition.PointerValuePosition;
    var rangePointerPos = pointerPosition.RangePointerStartPosition;
    var rangeStartValue = pointerPosition.RangeStartValue;
    
    Debug.WriteLine($"Pointer at {pointerValue}, position {pointerValuePos}");
}
```

### Event Workflow

**Complete event sequence during drag:**

```csharp
// 1. User starts dragging
private void Pointer_ValueChangeStarted(object sender, ValueChangedEventArgs e)
{
    Console.WriteLine("1. Drag started");
}

// 2. Fires repeatedly during drag (can cancel)
private void Pointer_ValueChanging(object sender, ValueChangingEventArgs e)
{
    Console.WriteLine($"2. Changing to {e.NewValue}");
    // Optionally: e.Cancel = true;
}

// 3. Fires after each successful change
private void Pointer_ValueChanged(object sender, ValueChangedEventArgs e)
{
    Console.WriteLine($"3. Changed to {e.Value}");
}

// 4. User releases pointer
private void Pointer_ValueChangeCompleted(object sender, ValueChangedEventArgs e)
{
    Console.WriteLine($"4. Completed at {e.Value}");
}
```

## MVVM Pattern

Implement gauges using the MVVM pattern for clean separation of concerns.

### ViewModel

**C#:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class GaugeViewModel : INotifyPropertyChanged
{
    private double _currentSpeed;
    private double _maxSpeed = 160;
    private string _speedUnit = "mph";
    
    public double CurrentSpeed
    {
        get => _currentSpeed;
        set
        {
            _currentSpeed = value;
            OnPropertyChanged();
        }
    }
    
    public double MaxSpeed
    {
        get => _maxSpeed;
        set
        {
            _maxSpeed = value;
            OnPropertyChanged();
        }
    }
    
    public string SpeedUnit
    {
        get => _speedUnit;
        set
        {
            _speedUnit = value;
            OnPropertyChanged();
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### View (XAML)

**XAML:**
```xml
<Window x:Class="GaugeApp.MainWindow"
        xmlns:local="clr-namespace:GaugeApp">
    <Window.DataContext>
        <local:GaugeViewModel/>
    </Window.DataContext>
    
    <gauge:SfCircularGauge>
        <gauge:SfCircularGauge.Annotations>
            <gauge:GaugeAnnotation Angle="270" Offset="0">
                <StackPanel>
                    <TextBlock Text="{Binding CurrentSpeed, StringFormat='{}{0:N0}'}"
                               FontSize="32"
                               FontWeight="Bold"
                               HorizontalAlignment="Center"/>
                    <TextBlock Text="{Binding SpeedUnit}"
                               FontSize="14"
                               Foreground="Gray"
                               HorizontalAlignment="Center"/>
                </StackPanel>
            </gauge:GaugeAnnotation>
        </gauge:SfCircularGauge.Annotations>
        
        <gauge:SfCircularGauge.Scales>
            <gauge:CircularScale StartValue="0"
                                 EndValue="{Binding MaxSpeed}"
                                 Interval="20">
                <gauge:CircularScale.Pointers>
                    <gauge:CircularPointer Value="{Binding CurrentSpeed, Mode=TwoWay}"
                                           EnableAnimation="True"
                                           AnimationDuration="600"/>
                </gauge:CircularScale.Pointers>
            </gauge:CircularScale>
        </gauge:SfCircularGauge.Scales>
    </gauge:SfCircularGauge>
</Window>
```

### Updating Values

**C#:**
```csharp
// In ViewModel or Code-Behind
public void UpdateSpeed(double newSpeed)
{
    // This automatically updates the gauge via data binding
    CurrentSpeed = newSpeed;
}

// Example: Simulate speed changes
private async Task SimulateSpeedChanges()
{
    while (true)
    {
        CurrentSpeed = Random.Shared.Next(0, 161);
        await Task.Delay(1000);
    }
}
```

## Real-Time Updates

### Timer-Based Updates

**C#:**
```csharp
using System.Windows.Threading;

public partial class MainWindow : Window
{
    private DispatcherTimer _timer;
    private Random _random = new Random();
    
    public MainWindow()
    {
        InitializeComponent();
        StartRealTimeUpdates();
    }
    
    private void StartRealTimeUpdates()
    {
        _timer = new DispatcherTimer
        {
            Interval = TimeSpan.FromSeconds(1)
        };
        _timer.Tick += Timer_Tick;
        _timer.Start();
    }
    
    private void Timer_Tick(object sender, EventArgs e)
    {
        // Update pointer value (assuming pointer is named 'myPointer')
        double newValue = _random.Next(0, 101);
        myPointer.Value = newValue;
    }
}
```

### Real-Time Sensor Data

**C#:**
```csharp
public class SensorMonitor
{
    private GaugeViewModel _viewModel;
    
    public async Task MonitorSensorAsync()
    {
        while (true)
        {
            // Read from sensor/API
            double sensorValue = await ReadSensorAsync();
            
            // Update UI (ensure on UI thread)
            Application.Current.Dispatcher.Invoke(() =>
            {
                _viewModel.CurrentValue = sensorValue;
            });
            
            await Task.Delay(500);  // Update every 500ms
        }
    }
    
    private async Task<double> ReadSensorAsync()
    {
        // Simulate sensor reading
        await Task.Delay(100);
        return Random.Shared.NextDouble() * 100;
    }
}
```

## Performance Optimization

### Best Practices

**1. Minimize Pointer Count**
```csharp
// Good: One pointer per metric
scale.Pointers.Add(new CircularPointer { Value = 75 });

// Avoid: Too many pointers
// (>5 pointers may impact performance)
```

**2. Disable Animation for High-Frequency Updates**
```xml
<!-- For rapid updates (>10 per second) -->
<gauge:CircularPointer EnableAnimation="False" Value="{Binding FastChangingValue}"/>
```

**3. Use Simple Ranges**
```xml
<!-- Prefer solid colors over gradients for performance -->
<gauge:CircularRange Stroke="Green" StrokeThickness="10"/>
```

**4. Optimize Annotations**
```csharp
// Reuse annotation content instead of recreating
annotationText.Text = newValue.ToString();

// Instead of:
// annotation.Content = new TextBlock { Text = newValue.ToString() };
```

**5. Batch Updates**
```csharp
// Update multiple properties together
pointer.BeginInit();
pointer.Value = 85;
pointer.NeedlePointerStroke = new SolidColorBrush(Colors.Red);
pointer.EndInit();
```

### Memory Management

**Dispose timers and event handlers:**
```csharp
public partial class GaugeWindow : Window
{
    private DispatcherTimer _timer;
    
    protected override void OnClosed(EventArgs e)
    {
        _timer?.Stop();
        _timer = null;
        
        // Unsubscribe from events
        myPointer.ValueChanged -= Pointer_ValueChanged;
        
        base.OnClosed(e);
    }
}
```

### Async Loading for Multiple Gauges

**C#:**
```csharp
public async Task LoadDashboardAsync()
{
    var tasks = new List<Task>
    {
        LoadGaugeDataAsync(gauge1),
        LoadGaugeDataAsync(gauge2),
        LoadGaugeDataAsync(gauge3)
    };
    
    await Task.WhenAll(tasks);
}

private async Task LoadGaugeDataAsync(SfCircularGauge gauge)
{
    var data = await FetchDataAsync();
    
    Application.Current.Dispatcher.Invoke(() =>
    {
        ((CircularPointer)gauge.Scales[0].Pointers[0]).Value = data;
    });
}
```

## Complete Advanced Example

**XAML:**
```xml
<Window x:Class="GaugeApp.AdvancedGauge">
    <gauge:SfCircularGauge x:Name="gauge">
        <gauge:SfCircularGauge.Annotations>
            <gauge:GaugeAnnotation Angle="270" Offset="0">
                <TextBlock x:Name="valueText" 
                           FontSize="32" 
                           FontWeight="Bold"/>
            </gauge:GaugeAnnotation>
        </gauge:SfCircularGauge.Annotations>
        
        <gauge:SfCircularGauge.Scales>
            <gauge:CircularScale StartValue="0" EndValue="100">
                <gauge:CircularScale.Ranges>
                    <gauge:CircularRange x:Name="activeRange"
                                         StartValue="0"
                                         EndValue="0"
                                         Stroke="LimeGreen"
                                         StrokeThickness="20"/>
                </gauge:CircularScale.Ranges>
                <gauge:CircularScale.Pointers>
                    <gauge:CircularPointer x:Name="pointer"
                                           PointerType="SymbolPointer"
                                           Symbol="InvertedTriangle"
                                           EnableDragging="True"
                                           EnableAnimation="False"
                                           Value="50"
                                           ValueChanged="Pointer_ValueChanged"
                                           ValueChangeCompleted="Pointer_ValueChangeCompleted"/>
                </gauge:CircularScale.Pointers>
            </gauge:CircularScale>
        </gauge:SfCircularGauge.Scales>
    </gauge:SfCircularGauge>
</Window>
```

**C#:**
```csharp
public partial class AdvancedGauge : Window
{
    public AdvancedGauge()
    {
        InitializeComponent();
    }
    
    private void Pointer_ValueChanged(object sender, ValueChangedEventArgs e)
    {
        // Update annotation
        valueText.Text = $"{e.Value:N0}%";
        
        // Update range
        activeRange.EndValue = e.Value;
    }
    
    private void Pointer_ValueChangeCompleted(object sender, ValueChangedEventArgs e)
    {
        // Save to database or perform action
        SaveUserSelection(e.Value);
    }
    
    private void SaveUserSelection(double value)
    {
        Debug.WriteLine($"User selected: {value}");
        // Implement save logic
    }
}
```

## Next Steps

- **Review Complete Examples:** See SKILL.md for full patterns
- **Explore Documentation:** https://help.syncfusion.com/wpf/radial-gauge/overview
- **Build Dashboard:** Combine multiple gauges with all features
