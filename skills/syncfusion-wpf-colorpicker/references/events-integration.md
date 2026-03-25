# Events and Integration

This reference covers handling color change events and integrating ColorPicker with other WPF controls and workflows.

## Overview of Events

ColorPicker provides two main events to handle user interactions:

| Event | Triggered When | Use Case |
|-------|---|---|
| `ColorChanged` | Selected solid color changes | React to solid color selection |
| `SelectedBrushChanged` | Selected brush (gradient) changes | React to gradient or brush changes |

## ColorChanged Event

Fires when the user selects or modifies a solid color value.

### XAML Event Binding
```xaml
<syncfusion:ColorPicker ColorChanged="ColorPicker_ColorChanged"
                        Name="colorPicker"/>
```

### C# Event Handler
```csharp
using Syncfusion.Windows.Shared;

ColorPicker colorPicker = new ColorPicker();
colorPicker.ColorChanged += ColorPicker_ColorChanged;

private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    Color oldColor = (Color)e.OldValue;
    Color newColor = (Color)e.NewValue;
    
    // Handle color change
    MessageBox.Show($"Color changed from {oldColor} to {newColor}");
}
```

### Practical Example: Display Hex Code
```csharp
private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    Color selectedColor = (Color)e.NewValue;
    
    // Convert to hex
    string hexColor = String.Format("#{0:X2}{1:X2}{2:X2}{3:X2}", 
                                    selectedColor.A, 
                                    selectedColor.R, 
                                    selectedColor.G, 
                                    selectedColor.B);
    
    hexCodeTextBlock.Text = hexColor;
}
```

### Practical Example: Update Background
```xaml
<StackPanel>
    <syncfusion:ColorPicker ColorChanged="ColorPicker_ColorChanged"
                            Name="colorPicker"/>
    <Rectangle Name="previewRectangle"
               Width="200"
               Height="100"
               Margin="10"/>
</StackPanel>
```

```csharp
private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    Color selectedColor = (Color)e.NewValue;
    previewRectangle.Fill = new SolidColorBrush(selectedColor);
}
```

## SelectedBrushChanged Event

Fires when the selected brush (gradient or solid) changes. This event is useful for gradient changes or when you need to track both solid and gradient selections.

### XAML Event Binding
```xaml
<syncfusion:ColorPicker SelectedBrushChanged="ColorPicker_SelectedBrushChanged"
                        Name="colorPicker"/>
```

### C# Event Handler
```csharp
private void ColorPicker_SelectedBrushChanged(DependencyObject d, 
                                              DependencyPropertyChangedEventArgs e)
{
    Brush oldBrush = (Brush)e.OldValue;
    Brush newBrush = (Brush)e.NewValue;
    
    // Handle brush change
    MessageBox.Show($"Brush changed");
}
```

### Detecting Brush Type
```csharp
private void ColorPicker_SelectedBrushChanged(DependencyObject d, 
                                              DependencyPropertyChangedEventArgs e)
{
    Brush selectedBrush = (Brush)e.NewValue;
    
    if (selectedBrush is SolidColorBrush solidBrush)
    {
        MessageBox.Show($"Solid color selected: {solidBrush.Color}");
    }
    else if (selectedBrush is LinearGradientBrush linearGradient)
    {
        MessageBox.Show($"Linear gradient selected with {linearGradient.GradientStops.Count} stops");
    }
    else if (selectedBrush is RadialGradientBrush radialGradient)
    {
        MessageBox.Show($"Radial gradient selected");
    }
}
```

### Practical Example: Update Shape with Gradient
```xaml
<StackPanel>
    <syncfusion:ColorPicker SelectedBrushChanged="ColorPicker_SelectedBrushChanged"
                            EnableSolidToGradientSwitch="True"
                            Name="colorPicker"/>
    <Ellipse Name="previewEllipse"
             Width="200"
             Height="200"
             Margin="10"/>
</StackPanel>
```

```csharp
private void ColorPicker_SelectedBrushChanged(DependencyObject d, 
                                              DependencyPropertyChangedEventArgs e)
{
    Brush selectedBrush = (Brush)e.NewValue;
    previewEllipse.Fill = selectedBrush;
}
```

## Combining Both Events

Use both events together to handle all color selection scenarios:

```csharp
ColorPicker colorPicker = new ColorPicker();
colorPicker.ColorChanged += ColorPicker_ColorChanged;
colorPicker.SelectedBrushChanged += ColorPicker_SelectedBrushChanged;

private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    Color newColor = (Color)e.NewValue;
    OnColorSelectionChanged($"Solid color: {newColor}");
}

private void ColorPicker_SelectedBrushChanged(DependencyObject d, 
                                              DependencyPropertyChangedEventArgs e)
{
    Brush newBrush = (Brush)e.NewValue;
    OnColorSelectionChanged($"Brush changed: {newBrush.GetType().Name}");
}

private void OnColorSelectionChanged(string message)
{
    // Central handler for all color changes
    statusTextBlock.Text = message;
    UpdatePreview();
}
```

## Integration Patterns

### Pattern 1: Two-Way Binding to View Model

```xaml
<syncfusion:ColorPicker Color="{Binding SelectedColor, Mode=TwoWay}"/>
```

```csharp
public class ColorViewModel : INotifyPropertyChanged
{
    private Color _selectedColor;
    
    public Color SelectedColor
    {
        get => _selectedColor;
        set
        {
            if (_selectedColor != value)
            {
                _selectedColor = value;
                OnPropertyChanged(nameof(SelectedColor));
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Pattern 2: Validation and Constraints

```csharp
private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    Color newColor = (Color)e.NewValue;
    
    // Only allow bright colors (luminosity > 128)
    int luminosity = (int)(0.299 * newColor.R + 0.587 * newColor.G + 0.114 * newColor.B);
    
    if (luminosity < 128)
    {
        MessageBox.Show("Please select a brighter color");
        // Reset to previous color
        colorPicker.Color = (Color)e.OldValue;
    }
}
```

### Pattern 3: Color History Tracking

```csharp
private List<Color> colorHistory = new List<Color>();

private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    Color newColor = (Color)e.NewValue;
    
    // Add to history if different from last
    if (colorHistory.Count == 0 || colorHistory.Last() != newColor)
    {
        colorHistory.Add(newColor);
    }
    
    // Limit history to 10 items
    if (colorHistory.Count > 10)
    {
        colorHistory.RemoveAt(0);
    }
    
    UpdateHistoryUI();
}

private void UpdateHistoryUI()
{
    // Display color history in UI
    historyStackPanel.Children.Clear();
    foreach (var color in colorHistory)
    {
        var rectangle = new Rectangle
        {
            Width = 20,
            Height = 20,
            Fill = new SolidColorBrush(color),
            Margin = new Thickness(2)
        };
        historyStackPanel.Children.Add(rectangle);
    }
}
```

### Pattern 4: Export Color Configuration

```csharp
private void ExportColorSettings_Click(object sender, RoutedEventArgs e)
{
    var colorSettings = new
    {
        Color = colorPicker.Color.ToString(),
        Brush = colorPicker.Brush.ToString(),
        Timestamp = DateTime.Now
    };
    
    string json = JsonConvert.SerializeObject(colorSettings, Formatting.Indented);
    
    // Save to file or database
    File.WriteAllText("color_settings.json", json);
    MessageBox.Show("Color settings exported");
}

private void ImportColorSettings_Click(object sender, RoutedEventArgs e)
{
    string json = File.ReadAllText("color_settings.json");
    var colorSettings = JsonConvert.DeserializeObject(json);
    
    // Restore color
    colorPicker.Color = (Color)ColorConverter.ConvertFromString(colorSettings.Color);
    MessageBox.Show("Color settings imported");
}
```

### Pattern 5: Real-Time Preview with Multiple Shapes

```xaml
<Grid>
    <StackPanel Orientation="Vertical" Margin="10">
        <!-- Color Picker -->
        <syncfusion:ColorPicker SelectedBrushChanged="ColorPicker_SelectedBrushChanged"
                                Height="300"
                                Name="colorPicker"/>
        
        <!-- Preview Shapes -->
        <StackPanel Orientation="Horizontal" Margin="0,10,0,0">
            <Rectangle Name="rectPreview" Width="80" Height="80" Margin="5"/>
            <Ellipse Name="circlePreview" Width="80" Height="80" Margin="5"/>
            <Polygon Name="trianglePreview" Points="40,0 80,80 0,80" Width="80" Height="80" Margin="5"/>
        </StackPanel>
        
        <!-- Texture Sample -->
        <TextBlock Name="textPreview"
                   Text="Sample Text"
                   FontSize="24"
                   FontWeight="Bold"
                   Margin="0,10,0,0"
                   Padding="10"
                   Background="LightGray"/>
    </StackPanel>
</Grid>
```

```csharp
private void ColorPicker_SelectedBrushChanged(DependencyObject d, 
                                              DependencyPropertyChangedEventArgs e)
{
    Brush selectedBrush = (Brush)e.NewValue;
    
    // Update all preview shapes
    rectPreview.Fill = selectedBrush;
    circlePreview.Fill = selectedBrush;
    trianglePreview.Fill = selectedBrush;
    textPreview.Background = selectedBrush;
}
```

### Pattern 6: Disable Button Until Valid Selection

```xaml
<Grid>
    <StackPanel>
        <syncfusion:ColorPicker Name="colorPicker"/>
        <Button Name="applyButton" 
                Click="ApplyButton_Click"
                IsEnabled="False">
            Apply Color
        </Button>
    </StackPanel>
</Grid>
```

```csharp
private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    Color selectedColor = (Color)e.NewValue;
    
    // Enable button only if color is sufficiently different from default
    bool isValid = selectedColor != Colors.White;
    applyButton.IsEnabled = isValid;
}

private void ApplyButton_Click(object sender, RoutedEventArgs e)
{
    Color finalColor = colorPicker.Color;
    // Apply color to application
    MessageBox.Show($"Applying color: {finalColor}");
}
```

## Event Timing and Order

When users interact with ColorPicker, events fire in this sequence:

1. User changes color in picker region
2. `ColorChanged` event fires (if solid color mode)
3. **OR** `SelectedBrushChanged` event fires (if gradient mode or brush changed)
4. Property values updated
5. UI refreshed

## Error Handling in Event Handlers

```csharp
private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    try
    {
        Color newColor = (Color)e.NewValue;
        
        // Perform color operations
        ValidateAndApplyColor(newColor);
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error handling color change: {ex.Message}");
        // Optionally reset to previous value
        colorPicker.Color = (Color)e.OldValue;
    }
}

private void ValidateAndApplyColor(Color color)
{
    // Validation logic
    if (color.A == 0)
        throw new InvalidOperationException("Fully transparent colors not allowed");
    
    // Application logic
    ApplyColorToUI(color);
}
```

## Performance Considerations

If handling frequent color changes:

```csharp
private DispatcherTimer debounceTimer;
private Color pendingColor;

public ColorWindow()
{
    InitializeComponent();
    
    // Debounce timer to avoid rapid updates
    debounceTimer = new DispatcherTimer();
    debounceTimer.Interval = TimeSpan.FromMilliseconds(300);
    debounceTimer.Tick += DebounceTimer_Tick;
}

private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    pendingColor = (Color)e.NewValue;
    
    // Restart debounce timer
    debounceTimer.Stop();
    debounceTimer.Start();
}

private void DebounceTimer_Tick(object sender, EventArgs e)
{
    debounceTimer.Stop();
    
    // Perform expensive operation with debounced color value
    ApplyExpensiveColorOperation(pendingColor);
}
```
