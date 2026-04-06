# Events and Integration

This reference covers handling color change events and integrating ColorPicker with other WPF controls and workflows.

## Overview of Events

ColorPicker provides two main events to handle user interactions:

| Event | Triggered When | Use Case |
|-------|---|---|
| `ColorChanged` | Selected solid color changes | React to solid color selection |
| `SelectedBrushChanged` | Selected brush (gradient) changes | React to gradient or brush changes |

**When to use ColorChanged:**
- Updating UI element backgrounds/foregrounds in real-time
- Tracking color history for undo/redo
- Validating color selections against constraints
- Updating color-dependent properties

**When to use SelectedBrushChanged:**
- Handling gradient changes specifically
- Managing complex brush compositions
- Detecting mode switches (solid ↔ gradient)

## ColorChanged Event

Use `ColorChanged` to react when the user selects or modifies a solid color value. Fires for any color property modification.

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

Subscribe to track brush changes (both solid and gradient). Use this event to:
- Detect when users switch between solid and gradient modes
- Apply gradients to shapes or UI elements in real-time
- Differentiate between SolidColorBrush, LinearGradientBrush, and RadialGradientBrush types

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

**Pattern 1: Two-Way Binding to ViewModel**
Bind Color property directly to XAML: `Color="{Binding SelectedColor, Mode=TwoWay}"` then implement INotifyPropertyChanged in your ViewModel.

**Pattern 2: Validation and Constraints**
In ColorChanged event, validate the new color. If invalid, revert: `colorPicker.Color = (Color)e.OldValue;` and show user feedback.

**Pattern 3: Color History Tracking**
Maintain a List<Color> in ColorChanged event. Track previously selected colors for undo/redo or display color history UI.

**Pattern 4: Export/Import Color Configuration**
In button click handlers, serialize color settings to JSON or XML: `Color.ToString()` and `Brush.ToString()`. On import, parse and set: `colorPicker.Color = (Color)ColorConverter.ConvertFromString(...)`.

**Pattern 5: Real-Time Preview with Multiple Shapes**
Bind multiple shapes (Rectangle, Ellipse, etc.) to the same `SelectedBrushChanged` event. Update all preview elements: `rect.Fill = selectedBrush;` to show brush changes across different geometries.

**Pattern 6: Validate Before Applying**
Track valid state in ColorChanged handler. Disable action buttons until validation passes. On valid selection, apply color and provide user feedback.

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
