# Events Reference - WPF Range Slider (SfRangeSlider)

## Table of Contents

1. [Overview](#overview)
2. [LabelLoaded Event](#labelloaded-event)
   - [Event Arguments](#labelloaded-event-arguments)
   - [XAML Example](#labelloaded-xaml-example)
   - [C# Example](#labelloaded-csharp-example)
   - [Use Cases](#labelloaded-use-cases)
3. [RangeChanged Event](#rangechanged-event)
   - [Event Arguments](#rangechanged-event-arguments)
   - [XAML Example](#rangechanged-xaml-example)
   - [C# Example](#rangechanged-csharp-example)
   - [Use Cases](#rangechanged-use-cases)
4. [RangeStartChanged Event](#rangestartchanged-event)
   - [Event Arguments](#rangestartchanged-event-arguments)
   - [XAML Example](#rangestartchanged-xaml-example)
   - [C# Example](#rangestartchanged-csharp-example)
   - [Use Cases](#rangestartchanged-use-cases)
5. [RangeEndChanged Event](#rangeendchanged-event)
   - [Event Arguments](#rangeendchanged-event-arguments)
   - [XAML Example](#rangeendchanged-xaml-example)
   - [C# Example](#rangeendchanged-csharp-example)
   - [Use Cases](#rangeendchanged-use-cases)
6. [Event Handler Patterns](#event-handler-patterns)
7. [Common Scenarios](#common-scenarios)
8. [Best Practices](#best-practices)

---

## Overview

The WPF Range Slider (`SfRangeSlider`) provides several events that allow you to respond to user interactions and value changes. These events enable you to customize the behavior, update UI elements, perform validation, and integrate with your application logic.

**Available Events:**
- **LabelLoaded**: Triggered when slider labels are created
- **RangeChanged**: Triggered when either RangeStart or RangeEnd values change
- **RangeStartChanged**: Triggered specifically when RangeStart value changes
- **RangeEndChanged**: Triggered specifically when RangeEnd value changes

---

## LabelLoaded Event

The `LabelLoaded` event is triggered when the slider label is created. This event provides an opportunity to customize label content before it is displayed to the user.

### LabelLoaded Event Arguments

The event handler receives a `LabelLoadedEventArgs` parameter containing:

| Property | Type | Description |
|----------|------|-------------|
| `Content` | object | Gets or sets the content of the label. Use this to customize label text. |

### LabelLoaded XAML Example

```xml
<Window x:Class="RangeSliderEvents.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        Title="Range Slider Events" Height="300" Width="500">
    <Grid>
        <editors:SfRangeSlider
            x:Name="rangeSlider"
            Width="300"
            Height="50"
            LabelLoaded="SfRangeSlider_LabelLoaded"
            Maximum="200"
            Minimum="100"
            ShowValueLabels="True"
            TickFrequency="20"
            TickPlacement="Outside" />
    </Grid>
</Window>
```

### LabelLoaded C# Example

**Code-Behind (XAML Event Handler):**

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderEvents
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void SfRangeSlider_LabelLoaded(object sender, LabelLoadedEventArgs e)
        {
            // Add percentage symbol to label
            e.Content = e.Content + "%";
        }
    }
}
```

**Programmatic Creation and Event Registration:**

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderEvents
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();

            Grid parentGrid = new Grid();
            SfRangeSlider rangeSlider = new SfRangeSlider()
            {
                Width = 300,
                Height = 50,
                Maximum = 200,
                Minimum = 100,
                Value = 140,
                ShowValueLabels = true,
                TickFrequency = 20,
                TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.Outside
            };
            
            // Register event handler
            rangeSlider.LabelLoaded += SfRangeSlider_LabelLoaded;
            
            parentGrid.Children.Add(rangeSlider);
            this.Content = parentGrid;
        }

        private void SfRangeSlider_LabelLoaded(object sender, LabelLoadedEventArgs e)
        {
            // Add percentage symbol to label
            e.Content = e.Content + "%";
        }
    }
}
```

### LabelLoaded Use Cases

1. **Add Unit Symbols**: Append units like %, $, °C, etc. to labels
2. **Format Numbers**: Apply custom number formatting (e.g., thousands separators)
3. **Localization**: Translate or adapt label text for different cultures
4. **Conditional Styling**: Apply different formatting based on value ranges

**Advanced Example - Custom Formatting:**

```csharp
private void SfRangeSlider_LabelLoaded(object sender, LabelLoadedEventArgs e)
{
    if (e.Content is double value)
    {
        // Format as currency
        e.Content = string.Format("${0:N0}", value);
    }
}
```

---

## RangeChanged Event

The `RangeChanged` event is triggered when either the `RangeStart` or `RangeEnd` property values are changed. This is the most commonly used event for responding to range selection changes.

### RangeChanged Event Arguments

The event handler receives a `RangeChangedEventArgs` parameter containing:

| Property | Type | Description |
|----------|------|-------------|
| `NewStartValue` | double | Gets or sets the new start value of the range slider |
| `NewEndValue` | double | Gets or sets the new end value of the range slider |
| `OldStartValue` | double | Gets or sets the old start value of the range slider |
| `OldEndValue` | double | Gets or sets the old end value of the range slider |

### RangeChanged XAML Example

```xml
<Window x:Class="RangeSliderEvents.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        Title="Range Slider Events" Height="300" Width="500">
    <Grid>
        <StackPanel Margin="20">
            <TextBlock x:Name="statusText" 
                       Text="Range: 30 - 60"
                       FontSize="14"
                       Margin="0,0,0,20"/>
            
            <editors:SfRangeSlider
                x:Name="rangeSlider"
                Width="300"
                Height="50"
                RangeChanged="SfRangeSlider_RangeChanged"
                Maximum="100"
                Minimum="0"
                ShowRange="True"
                RangeStart="30"
                RangeEnd="60"/>
        </StackPanel>
    </Grid>
</Window>
```

### RangeChanged C# Example

**Code-Behind (XAML Event Handler):**

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderEvents
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void SfRangeSlider_RangeChanged(object sender, RangeChangedEventArgs e)
        {
            var newStartValue = e.NewStartValue;
            var newEndValue = e.NewEndValue;
            var oldStartValue = e.OldStartValue;
            var oldEndValue = e.OldEndValue;

            // Update UI with new range
            statusText.Text = $"Range: {newStartValue:F0} - {newEndValue:F0}";

            // Log changes
            System.Diagnostics.Debug.WriteLine(
                $"Range changed from [{oldStartValue}-{oldEndValue}] to [{newStartValue}-{newEndValue}]");
        }
    }
}
```

**Programmatic Creation and Event Registration:**

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderEvents
{
    public partial class MainWindow : Window
    {
        private TextBlock statusText;

        public MainWindow()
        {
            InitializeComponent();

            Grid parentGrid = new Grid();
            StackPanel stackPanel = new StackPanel() { Margin = new Thickness(20) };

            statusText = new TextBlock()
            {
                Text = "Range: 30 - 60",
                FontSize = 14,
                Margin = new Thickness(0, 0, 0, 20)
            };

            SfRangeSlider rangeSlider = new SfRangeSlider()
            {
                Width = 300,
                Height = 50,
                Maximum = 100,
                Minimum = 0,
                ShowRange = true,
                RangeStart = 30,
                RangeEnd = 60
            };

            // Register event handler
            rangeSlider.RangeChanged += SfRangeSlider_RangeChanged;

            stackPanel.Children.Add(statusText);
            stackPanel.Children.Add(rangeSlider);
            parentGrid.Children.Add(stackPanel);
            this.Content = parentGrid;
        }

        private void SfRangeSlider_RangeChanged(object sender, RangeChangedEventArgs e)
        {
            var newStartValue = e.NewStartValue;
            var newEndValue = e.NewEndValue;
            var oldStartValue = e.OldStartValue;
            var oldEndValue = e.OldEndValue;

            // Update UI with new range
            statusText.Text = $"Range: {newStartValue:F0} - {newEndValue:F0}";

            // Calculate range span
            double rangeSpan = newEndValue - newStartValue;
            System.Diagnostics.Debug.WriteLine($"Range span: {rangeSpan}");
        }
    }
}
```

### RangeChanged Use Cases

1. **Update Dependent UI**: Display selected range in other controls
2. **Data Filtering**: Filter data based on selected range
3. **Validation**: Ensure minimum/maximum range constraints
4. **Analytics**: Track user range selection behavior

**Advanced Example - Range Validation:**

```csharp
private void SfRangeSlider_RangeChanged(object sender, RangeChangedEventArgs e)
{
    double minRangeSpan = 10;
    double currentSpan = e.NewEndValue - e.NewStartValue;

    if (currentSpan < minRangeSpan)
    {
        // Enforce minimum range span
        var slider = sender as SfRangeSlider;
        if (slider != null)
        {
            // Restore previous values
            slider.RangeStart = e.OldStartValue;
            slider.RangeEnd = e.OldEndValue;
            MessageBox.Show($"Range must be at least {minRangeSpan} units wide.");
        }
    }
    else
    {
        // Update display
        statusText.Text = $"Selected Range: {e.NewStartValue:F0} - {e.NewEndValue:F0}";
    }
}
```

---

## RangeStartChanged Event

The `RangeStartChanged` event is triggered specifically when the `RangeStart` property value is changed. Use this event when you need to respond only to start value changes.

### RangeStartChanged Event Arguments

The event handler receives a `RangeStartChangedEventArgs` parameter containing:

| Property | Type | Description |
|----------|------|-------------|
| `NewStartValue` | double | Gets or sets the new start value of the range slider |
| `OldStartValue` | double | Gets or sets the old start value of the range slider |

### RangeStartChanged XAML Example

```xml
<Window x:Class="RangeSliderEvents.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        Title="Range Slider Events" Height="300" Width="500">
    <Grid>
        <StackPanel Margin="20">
            <TextBlock x:Name="startValueText" 
                       Text="Start Value: 30"
                       FontSize="14"
                       Margin="0,0,0,20"/>
            
            <editors:SfRangeSlider
                x:Name="rangeSlider"
                Width="300"
                Height="50"
                RangeStartChanged="SfRangeSlider_RangeStartChanged"
                Maximum="100"
                Minimum="0"
                ShowRange="True"
                RangeStart="30"
                RangeEnd="60"/>
        </StackPanel>
    </Grid>
</Window>
```

### RangeStartChanged C# Example

**Code-Behind (XAML Event Handler):**

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderEvents
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void SfRangeSlider_RangeStartChanged(object sender, RangeStartChangedEventArgs e)
        {
            var newStartValue = e.NewStartValue;
            var oldStartValue = e.OldStartValue;

            // Update UI with new start value
            startValueText.Text = $"Start Value: {newStartValue:F0}";

            // Log change
            System.Diagnostics.Debug.WriteLine(
                $"Start value changed from {oldStartValue} to {newStartValue}");
        }
    }
}
```

**Programmatic Creation and Event Registration:**

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderEvents
{
    public partial class MainWindow : Window
    {
        private TextBlock startValueText;

        public MainWindow()
        {
            InitializeComponent();

            Grid parentGrid = new Grid();
            StackPanel stackPanel = new StackPanel() { Margin = new Thickness(20) };

            startValueText = new TextBlock()
            {
                Text = "Start Value: 30",
                FontSize = 14,
                Margin = new Thickness(0, 0, 0, 20)
            };

            SfRangeSlider rangeSlider = new SfRangeSlider()
            {
                Width = 300,
                Height = 50,
                Maximum = 100,
                Minimum = 0,
                ShowRange = true,
                RangeStart = 30,
                RangeEnd = 60
            };

            // Register event handler
            rangeSlider.RangeStartChanged += SfRangeSlider_RangeStartChanged;

            stackPanel.Children.Add(startValueText);
            stackPanel.Children.Add(rangeSlider);
            parentGrid.Children.Add(stackPanel);
            this.Content = parentGrid;
        }

        private void SfRangeSlider_RangeStartChanged(object sender, RangeStartChangedEventArgs e)
        {
            var newStartValue = e.NewStartValue;
            var oldStartValue = e.OldStartValue;

            // Update UI with new start value
            startValueText.Text = $"Start Value: {newStartValue:F0}";

            // Calculate change delta
            double delta = newStartValue - oldStartValue;
            System.Diagnostics.Debug.WriteLine($"Start value delta: {delta}");
        }
    }
}
```

### RangeStartChanged Use Cases

1. **Start-Specific Validation**: Validate only the start value
2. **Asymmetric Behavior**: Apply different logic for start vs. end changes
3. **Performance**: Handle only start changes for optimization
4. **Independent Updates**: Update UI elements tied to start value only

**Advanced Example - Start Value Constraints:**

```csharp
private void SfRangeSlider_RangeStartChanged(object sender, RangeStartChangedEventArgs e)
{
    var slider = sender as SfRangeSlider;
    if (slider != null)
    {
        // Ensure minimum gap between start and end
        double minGap = 20;
        if (slider.RangeEnd - e.NewStartValue < minGap)
        {
            slider.RangeStart = e.OldStartValue;
            MessageBox.Show("Start value must be at least 20 units from end value.");
            return;
        }

        // Update dependent controls
        startValueText.Text = $"From: {e.NewStartValue:F0}";
    }
}
```

---

## RangeEndChanged Event

The `RangeEndChanged` event is triggered specifically when the `RangeEnd` property value is changed. Use this event when you need to respond only to end value changes.

### RangeEndChanged Event Arguments

The event handler receives a `RangeEndChangedEventArgs` parameter containing:

| Property | Type | Description |
|----------|------|-------------|
| `NewEndValue` | double | Gets or sets the new end value of the range slider |
| `OldEndValue` | double | Gets or sets the old end value of the range slider |

### RangeEndChanged XAML Example

```xml
<Window x:Class="RangeSliderEvents.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        Title="Range Slider Events" Height="300" Width="500">
    <Grid>
        <StackPanel Margin="20">
            <TextBlock x:Name="endValueText" 
                       Text="End Value: 60"
                       FontSize="14"
                       Margin="0,0,0,20"/>
            
            <editors:SfRangeSlider
                x:Name="rangeSlider"
                Width="300"
                Height="50"
                RangeEndChanged="SfRangeSlider_RangeEndChanged"
                Maximum="100"
                Minimum="0"
                ShowRange="True"
                RangeStart="30"
                RangeEnd="60"/>
        </StackPanel>
    </Grid>
</Window>
```

### RangeEndChanged C# Example

**Code-Behind (XAML Event Handler):**

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderEvents
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void SfRangeSlider_RangeEndChanged(object sender, RangeEndChangedEventArgs e)
        {
            var newEndValue = e.NewEndValue;
            var oldEndValue = e.OldEndValue;

            // Update UI with new end value
            endValueText.Text = $"End Value: {newEndValue:F0}";

            // Log change
            System.Diagnostics.Debug.WriteLine(
                $"End value changed from {oldEndValue} to {newEndValue}");
        }
    }
}
```

**Programmatic Creation and Event Registration:**

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderEvents
{
    public partial class MainWindow : Window
    {
        private TextBlock endValueText;

        public MainWindow()
        {
            InitializeComponent();

            Grid parentGrid = new Grid();
            StackPanel stackPanel = new StackPanel() { Margin = new Thickness(20) };

            endValueText = new TextBlock()
            {
                Text = "End Value: 60",
                FontSize = 14,
                Margin = new Thickness(0, 0, 0, 20)
            };

            SfRangeSlider rangeSlider = new SfRangeSlider()
            {
                Width = 300,
                Height = 50,
                Maximum = 100,
                Minimum = 0,
                ShowRange = true,
                RangeStart = 30,
                RangeEnd = 60
            };

            // Register event handler
            rangeSlider.RangeEndChanged += SfRangeSlider_RangeEndChanged;

            stackPanel.Children.Add(endValueText);
            stackPanel.Children.Add(rangeSlider);
            parentGrid.Children.Add(stackPanel);
            this.Content = parentGrid;
        }

        private void SfRangeSlider_RangeEndChanged(object sender, RangeEndChangedEventArgs e)
        {
            var newEndValue = e.NewEndValue;
            var oldEndValue = e.OldEndValue;

            // Update UI with new end value
            endValueText.Text = $"End Value: {newEndValue:F0}";

            // Calculate change delta
            double delta = newEndValue - oldEndValue;
            System.Diagnostics.Debug.WriteLine($"End value delta: {delta}");
        }
    }
}
```

### RangeEndChanged Use Cases

1. **End-Specific Validation**: Validate only the end value
2. **Upper Limit Monitoring**: Track changes to the upper boundary
3. **Asymmetric Updates**: Apply different logic for end vs. start changes
4. **Maximum Value Tracking**: Monitor approaches to maximum values

**Advanced Example - End Value Constraints:**

```csharp
private void SfRangeSlider_RangeEndChanged(object sender, RangeEndChangedEventArgs e)
{
    var slider = sender as SfRangeSlider;
    if (slider != null)
    {
        // Ensure minimum gap between start and end
        double minGap = 20;
        if (e.NewEndValue - slider.RangeStart < minGap)
        {
            slider.RangeEnd = e.OldEndValue;
            MessageBox.Show("End value must be at least 20 units from start value.");
            return;
        }

        // Update dependent controls
        endValueText.Text = $"To: {e.NewEndValue:F0}";
    }
}
```

---

## Event Handler Patterns

### Pattern 1: Combined Event Handling

Handle all range events to provide comprehensive feedback:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        rangeSlider.RangeChanged += OnRangeChanged;
        rangeSlider.RangeStartChanged += OnRangeStartChanged;
        rangeSlider.RangeEndChanged += OnRangeEndChanged;
        rangeSlider.LabelLoaded += OnLabelLoaded;
    }

    private void OnRangeChanged(object sender, RangeChangedEventArgs e)
    {
        // Handle overall range changes
        UpdateRangeDisplay(e.NewStartValue, e.NewEndValue);
    }

    private void OnRangeStartChanged(object sender, RangeStartChangedEventArgs e)
    {
        // Handle start-specific logic
        ValidateStartValue(e.NewStartValue);
    }

    private void OnRangeEndChanged(object sender, RangeEndChangedEventArgs e)
    {
        // Handle end-specific logic
        ValidateEndValue(e.NewEndValue);
    }

    private void OnLabelLoaded(object sender, LabelLoadedEventArgs e)
    {
        // Customize label display
        e.Content = FormatLabel(e.Content);
    }
}
```

### Pattern 2: Event Unsubscription

Properly unsubscribe from events to prevent memory leaks:

```csharp
public partial class MainWindow : Window
{
    private SfRangeSlider rangeSlider;

    public MainWindow()
    {
        InitializeComponent();
        SubscribeToEvents();
    }

    private void SubscribeToEvents()
    {
        rangeSlider.RangeChanged += OnRangeChanged;
    }

    private void UnsubscribeFromEvents()
    {
        rangeSlider.RangeChanged -= OnRangeChanged;
    }

    protected override void OnClosed(EventArgs e)
    {
        UnsubscribeFromEvents();
        base.OnClosed(e);
    }

    private void OnRangeChanged(object sender, RangeChangedEventArgs e)
    {
        // Handle event
    }
}
```

### Pattern 3: Conditional Event Handling

Use flags to prevent event cascades:

```csharp
public partial class MainWindow : Window
{
    private bool isUpdating = false;

    private void OnRangeChanged(object sender, RangeChangedEventArgs e)
    {
        if (isUpdating) return;

        try
        {
            isUpdating = true;
            
            // Perform updates that might trigger other events
            UpdateRelatedControls(e.NewStartValue, e.NewEndValue);
        }
        finally
        {
            isUpdating = false;
        }
    }
}
```

---

## Common Scenarios

### Scenario 1: Price Range Filter

```csharp
private List<Product> products;
private List<Product> filteredProducts;

private void OnRangeChanged(object sender, RangeChangedEventArgs e)
{
    // Filter products by price range
    filteredProducts = products
        .Where(p => p.Price >= e.NewStartValue && p.Price <= e.NewEndValue)
        .ToList();
    
    // Update data grid
    productGrid.ItemsSource = filteredProducts;
    
    // Update summary
    summaryText.Text = $"Showing {filteredProducts.Count} products (${ e.NewStartValue:F0} - ${e.NewEndValue:F0})";
}
```

### Scenario 2: Date Range Selection

```csharp
private DateTime minDate = new DateTime(2020, 1, 1);
private DateTime maxDate = new DateTime(2024, 12, 31);

private void OnRangeChanged(object sender, RangeChangedEventArgs e)
{
    // Convert slider values to dates
    DateTime startDate = minDate.AddDays(e.NewStartValue);
    DateTime endDate = minDate.AddDays(e.NewEndValue);
    
    // Update display
    dateRangeText.Text = $"{startDate:MMM dd, yyyy} - {endDate:MMM dd, yyyy}";
    
    // Filter data
    FilterDataByDateRange(startDate, endDate);
}

private void OnLabelLoaded(object sender, LabelLoadedEventArgs e)
{
    // Format labels as dates
    if (e.Content is double days)
    {
        DateTime date = minDate.AddDays(days);
        e.Content = date.ToString("MMM yyyy");
    }
}
```

### Scenario 3: Real-Time Chart Updates

```csharp
private void OnRangeChanged(object sender, RangeChangedEventArgs e)
{
    // Update chart axis range
    chart.PrimaryAxis.Minimum = e.NewStartValue;
    chart.PrimaryAxis.Maximum = e.NewEndValue;
    
    // Refresh chart
    chart.Refresh();
    
    // Update statistics for visible range
    UpdateStatistics(e.NewStartValue, e.NewEndValue);
}
```

---

## Best Practices

### 1. Performance Optimization

- **Throttle Updates**: Use timers to limit update frequency during rapid changes
- **Batch Operations**: Accumulate changes and process them together
- **Async Operations**: Move heavy processing off the UI thread

```csharp
private System.Windows.Threading.DispatcherTimer updateTimer;

private void InitializeUpdateTimer()
{
    updateTimer = new System.Windows.Threading.DispatcherTimer();
    updateTimer.Interval = TimeSpan.FromMilliseconds(300);
    updateTimer.Tick += (s, e) =>
    {
        updateTimer.Stop();
        PerformExpensiveUpdate();
    };
}

private void OnRangeChanged(object sender, RangeChangedEventArgs e)
{
    // Store latest values
    latestStart = e.NewStartValue;
    latestEnd = e.NewEndValue;
    
    // Restart timer
    updateTimer.Stop();
    updateTimer.Start();
}
```

### 2. Error Handling

Always validate event data and handle exceptions:

```csharp
private void OnRangeChanged(object sender, RangeChangedEventArgs e)
{
    try
    {
        // Validate values
        if (e.NewStartValue < 0 || e.NewEndValue > 100)
        {
            System.Diagnostics.Debug.WriteLine("Invalid range values");
            return;
        }
        
        // Perform operations
        UpdateUI(e.NewStartValue, e.NewEndValue);
    }
    catch (Exception ex)
    {
        System.Diagnostics.Debug.WriteLine($"Error in RangeChanged: {ex.Message}");
        // Show user-friendly message
        MessageBox.Show("An error occurred while updating the range.");
    }
}
```

### 3. Maintain State Consistency

Keep your application state synchronized:

```csharp
private class RangeState
{
    public double StartValue { get; set; }
    public double EndValue { get; set; }
    public DateTime LastModified { get; set; }
}

private RangeState currentState = new RangeState();

private void OnRangeChanged(object sender, RangeChangedEventArgs e)
{
    // Update state
    currentState.StartValue = e.NewStartValue;
    currentState.EndValue = e.NewEndValue;
    currentState.LastModified = DateTime.Now;
    
    // Notify other components
    OnRangeStateChanged();
}
```

### 4. User Feedback

Provide clear feedback for user actions:

```csharp
private void OnRangeStartChanged(object sender, RangeStartChangedEventArgs e)
{
    // Show tooltip with value
    startThumbTooltip.Content = $"{e.NewStartValue:F1}";
    startThumbTooltip.IsOpen = true;
    
    // Hide tooltip after delay
    var timer = new System.Windows.Threading.DispatcherTimer 
    { 
        Interval = TimeSpan.FromSeconds(2) 
    };
    timer.Tick += (s, args) =>
    {
        startThumbTooltip.IsOpen = false;
        timer.Stop();
    };
    timer.Start();
}
```

### 5. Testing Event Handlers

Create testable event handler logic:

```csharp
// Separate business logic from UI code
public class RangeValidator
{
    public bool IsValidRange(double start, double end, double minSpan)
    {
        return (end - start) >= minSpan;
    }
    
    public string FormatRangeDisplay(double start, double end)
    {
        return $"{start:F0} - {end:F0}";
    }
}

// Use in event handler
private RangeValidator validator = new RangeValidator();

private void OnRangeChanged(object sender, RangeChangedEventArgs e)
{
    if (!validator.IsValidRange(e.NewStartValue, e.NewEndValue, 10))
    {
        // Handle invalid range
        return;
    }
    
    statusText.Text = validator.FormatRangeDisplay(e.NewStartValue, e.NewEndValue);
}
```

---

## Summary

The WPF Range Slider events provide powerful hooks for creating interactive and responsive applications:

- Use **LabelLoaded** for customizing label display
- Use **RangeChanged** for general range selection handling
- Use **RangeStartChanged** and **RangeEndChanged** for granular control
- Follow best practices for performance, error handling, and maintainability
- Test event handlers thoroughly to ensure robust behavior

For more information, refer to the official Syncfusion documentation.