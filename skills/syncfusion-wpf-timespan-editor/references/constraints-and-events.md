# Constraints & Events

## Table of Contents
- [Min/Max Value Constraints](#minmax-value-constraints)
- [ValueChanged Event](#valuechanged-event)
- [Event Binding in XAML](#event-binding-in-xaml)
- [Event Handling in C#](#event-handling-in-c)
- [Validation Patterns](#validation-patterns)

---

## Min/Max Value Constraints

Restrict the time values that users can enter by setting minimum and maximum boundaries.

**Default Behavior:**
- No minimum constraint (can be zero or negative)
- No maximum constraint (can be very large)

**Setting Constraints:**

**XAML Example:**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         MinValue="2.00:00:00"
                         MaxValue="10.00:00:00" />
```

This allows values between:
- **Minimum:** 2 days, 0 hours, 0 minutes, 0 seconds
- **Maximum:** 10 days, 0 hours, 0 minutes, 0 seconds

**C# Code Example:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.Value = new TimeSpan(5, 12, 30, 45);
editor.MinValue = new TimeSpan(2, 0, 0, 0);   // 2 days minimum
editor.MaxValue = new TimeSpan(10, 0, 0, 0);  // 10 days maximum
```

**Constraint Enforcement:**

When users try to exceed the limits:
- The value stops at the boundary (cannot go beyond min/max)
- Users see the boundary value as a "ceiling" or "floor"

**Example Workflow:**
```
MinValue: 2.00:00:00, MaxValue: 10.00:00:00, Current: 5.00:00:00
User attempts to increase to 11 days → Value becomes 10.00:00:00 (capped at max)
User attempts to decrease to 1 day  → Value becomes 2.00:00:00 (capped at min)
```

**Practical Constraint Scenarios:**

*Scenario 1: Work Day Duration (6-10 hours)*
```xml
<syncfusion:TimeSpanEdit Value="0.08:00:00"
                         MinValue="0.06:00:00"
                         MaxValue="0.10:00:00"
                         Format="h 'hours' m 'minutes'"
                         StepInterval="0.00:30:00" />
```

*Scenario 2: Project Task Time (1-30 days)*
```xml
<syncfusion:TimeSpanEdit Value="5.00:00:00"
                         MinValue="1.00:00:00"
                         MaxValue="30.00:00:00"
                         Format="d 'days'" />
```

*Scenario 3: Timer with Seconds Precision (0-60 seconds)*
```xml
<syncfusion:TimeSpanEdit Value="0.00:00:30"
                         MinValue="0.00:00:00"
                         MaxValue="0.00:01:00"
                         Format="m ':' s"
                         StepInterval="0.00:00:01" />
```

*Scenario 4: Meeting Duration (30 minutes - 4 hours)*
```xml
<syncfusion:TimeSpanEdit Value="0.01:00:00"
                         MinValue="0.00:30:00"
                         MaxValue="0.04:00:00"
                         Format="h ':' mm 'minutes'"
                         StepInterval="0.00:15:00" />
```

**C# Validation Pattern:**
```csharp
bool IsWithinConstraints(TimeSpan value, TimeSpan? min, TimeSpan? max) {
    if (min.HasValue && value < min.Value) return false;
    if (max.HasValue && value > max.Value) return false;
    return true;
}

// Usage
TimeSpanEdit editor = new TimeSpanEdit();
editor.MinValue = new TimeSpan(1, 0, 0, 0);
editor.MaxValue = new TimeSpan(10, 0, 0, 0);

TimeSpan testValue = new TimeSpan(5, 12, 30, 0);
if (IsWithinConstraints(testValue, editor.MinValue, editor.MaxValue)) {
    editor.Value = testValue;  // Valid
}
```

---

## ValueChanged Event

The `ValueChanged` event fires whenever the time value is modified by the user or programmatically.

**Event Details:**
- **Parameter Type:** `DependencyPropertyChangedEventArgs`
- **OldValue:** Previous time value
- **NewValue:** New time value
- **Sender:** The TimeSpanEdit control that raised the event

---

## Event Binding in XAML

**Basic Event Binding:**
```xml
<syncfusion:TimeSpanEdit x:Name="timeSpanEdit"
                         Value="5.12:30:45"
                         ValueChanged="TimeSpanEdit_ValueChanged" />
```

**Complete XAML Example:**
```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="TimeSpanEditDemo.MainWindow"
        Title="TimeSpanEdit Event Demo"
        Height="300"
        Width="400">
    <Grid>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <TextBlock Text="Duration:" FontSize="14" Margin="0,0,0,10" />
            
            <syncfusion:TimeSpanEdit x:Name="durationEditor"
                                     Value="5.00:00:00"
                                     Width="150"
                                     Height="35"
                                     ValueChanged="DurationEditor_ValueChanged"
                                     Margin="0,0,0,15" />
            
            <TextBlock x:Name="statusText"
                       Text="Change the duration value"
                       FontSize="12"
                       Foreground="#666" />
        </StackPanel>
    </Grid>
</Window>
```

---

## Event Handling in C#

**Basic Event Handler:**

```csharp
private void TimeSpanEdit_ValueChanged(DependencyObject d, 
                                       DependencyPropertyChangedEventArgs e) {
    TimeSpan? oldValue = (TimeSpan?)e.OldValue;
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    
    MessageBox.Show($"Value changed from {oldValue} to {newValue}");
}
```

**Code-Behind Example:**
```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
    }
    
    private void DurationEditor_ValueChanged(DependencyObject d, 
                                             DependencyPropertyChangedEventArgs e) {
        TimeSpan? oldValue = (TimeSpan?)e.OldValue;
        TimeSpan? newValue = (TimeSpan?)e.NewValue;
        
        // Get the editor that triggered the event
        TimeSpanEdit editor = d as TimeSpanEdit;
        
        if (newValue.HasValue) {
            TimeSpan ts = newValue.Value;
            statusText.Text = $"Duration: {ts.Days}d {ts.Hours}h {ts.Minutes}m {ts.Seconds}s";
        }
        else {
            statusText.Text = "Duration cleared";
        }
    }
}
```

**Programmatic Event Subscription:**

```csharp
public MainWindow() {
    InitializeComponent();
    
    // Create TimeSpanEdit
    TimeSpanEdit editor = new TimeSpanEdit();
    editor.Value = new TimeSpan(5, 12, 30, 0);
    
    // Subscribe to ValueChanged
    editor.ValueChanged += Editor_ValueChanged;
    
    this.Content = editor;
}

private void Editor_ValueChanged(DependencyObject d, 
                                 DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    Debug.WriteLine($"New value: {newValue}");
}
```

---

## Validation Patterns

**Pattern 1: Log Value Changes**
```csharp
private void TimeSpanEdit_ValueChanged(DependencyObject d, 
                                       DependencyPropertyChangedEventArgs e) {
    TimeSpan? oldValue = (TimeSpan?)e.OldValue;
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    
    Debug.WriteLine($"[{DateTime.Now:HH:mm:ss}] Duration changed: {oldValue} → {newValue}");
}
```

**Pattern 2: Validation with Error Display**
```xml
<Grid>
    <StackPanel>
        <syncfusion:TimeSpanEdit x:Name="taskDuration"
                                 Value="0.08:00:00"
                                 MinValue="0.00:30:00"
                                 MaxValue="0.12:00:00"
                                 ValueChanged="TaskDuration_ValueChanged"
                                 Width="150"
                                 Height="35" />
        
        <TextBlock x:Name="errorMessage"
                   Foreground="Red"
                   FontSize="11"
                   Margin="0,5,0,0" />
    </StackPanel>
</Grid>
```

```csharp
private void TaskDuration_ValueChanged(DependencyObject d, 
                                       DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    
    if (!newValue.HasValue) {
        errorMessage.Text = "Please enter a duration";
        return;
    }
    
    TimeSpan duration = newValue.Value;
    
    // Validate bounds (redundant since control enforces, but good for feedback)
    if (duration.TotalHours < 0.5) {
        errorMessage.Text = "Minimum 30 minutes required";
    }
    else if (duration.TotalHours > 12) {
        errorMessage.Text = "Maximum 12 hours allowed";
    }
    else {
        errorMessage.Text = "";  // Clear error
    }
}
```

**Pattern 3: Cascading Updates**

```csharp
private void DurationChanged_ValueChanged(DependencyObject d, 
                                          DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    
    if (!newValue.HasValue) return;
    
    // Update dependent fields when duration changes
    TimeSpan duration = newValue.Value;
    decimal hourlyRate = 50;  // Example: $50/hour
    decimal totalCost = (decimal)duration.TotalHours * hourlyRate;
    
    costDisplay.Text = $"Estimated Cost: ${totalCost:F2}";
}
```

**Pattern 4: Form Submission Validation**

```csharp
private void SubmitButton_Click(object sender, RoutedEventArgs e) {
    // Validate before submission
    if (durationEditor.Value == null) {
        MessageBox.Show("Please enter a duration", "Validation Error");
        return;
    }
    
    TimeSpan duration = durationEditor.Value.Value;
    
    // Additional business logic validation
    if (duration.TotalMinutes < 30) {
        MessageBox.Show("Minimum duration is 30 minutes", "Validation Error");
        return;
    }
    
    // Valid - process the form
    ProcessTimeEntry(duration);
}

private void ProcessTimeEntry(TimeSpan duration) {
    MessageBox.Show($"Time entry recorded: {duration}");
    // Save to database, etc.
}
```

**Pattern 5: Two-Way Binding with Validation**

```csharp
// ViewModel class
public class TimeEntryViewModel : INotifyPropertyChanged {
    private TimeSpan? _duration;
    
    public TimeSpan? Duration {
        get { return _duration; }
        set {
            if (_duration != value) {
                _duration = value;
                OnPropertyChanged(nameof(Duration));
                ValidateDuration();
            }
        }
    }
    
    private string _validationError = "";
    
    public string ValidationError {
        get { return _validationError; }
        set {
            if (_validationError != value) {
                _validationError = value;
                OnPropertyChanged(nameof(ValidationError));
            }
        }
    }
    
    private void ValidateDuration() {
        if (Duration == null) {
            ValidationError = "Duration is required";
            return;
        }
        
        TimeSpan duration = Duration.Value;
        
        if (duration.TotalHours < 0.5) {
            ValidationError = "Minimum 30 minutes";
        }
        else if (duration.TotalHours > 12) {
            ValidationError = "Maximum 12 hours";
        }
        else {
            ValidationError = "";
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName) {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML with ViewModel Binding:**
```xml
<Window DataContext="{StaticResource TimeEntryViewModel}">
    <Grid>
        <StackPanel>
            <syncfusion:TimeSpanEdit Value="{Binding Duration, Mode=TwoWay}"
                                     MinValue="0.00:30:00"
                                     MaxValue="0.12:00:00"
                                     Width="150"
                                     Height="35" />
            
            <TextBlock Text="{Binding ValidationError}"
                       Foreground="Red"
                       FontSize="11"
                       Margin="0,5,0,0" />
        </StackPanel>
    </Grid>
</Window>
```

---

## Common Event Scenarios

**Scenario 1: Time Duration Tracker**
```csharp
private TimeSpan totalTime = TimeSpan.Zero;

private void ActivityDuration_ValueChanged(DependencyObject d, 
                                           DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    
    if (newValue.HasValue) {
        totalTime = totalTime.Add(newValue.Value);
        totalTimeDisplay.Text = $"Total: {totalTime.TotalHours:F1} hours";
    }
}
```

**Scenario 2: Auto-save on Change**
```csharp
private async void Duration_ValueChanged(DependencyObject d, 
                                         DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    
    if (newValue.HasValue) {
        // Auto-save to database
        await SaveDurationAsync(newValue.Value);
        statusLabel.Text = "Saved";
    }
}
```

**Scenario 3: Conditional UI Updates**
```csharp
private void Duration_ValueChanged(DependencyObject d, 
                                   DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    
    if (!newValue.HasValue) {
        warningPanel.Visibility = Visibility.Visible;
        submitButton.IsEnabled = false;
    }
    else if (newValue.Value.TotalHours > 8) {
        warningPanel.Visibility = Visibility.Visible;
        warningText.Text = "Duration exceeds standard work day";
        submitButton.IsEnabled = true;
    }
    else {
        warningPanel.Visibility = Visibility.Collapsed;
        submitButton.IsEnabled = true;
    }
}
```

---

## Next Steps

1. **Add interactions** — Review [user-interactions.md](user-interactions.md) for keyboard/mouse controls
2. **Style it** — Apply colors and themes in [appearance-and-theming.md](appearance-and-theming.md)
3. **Reference formats** — Check [value-and-formats.md](value-and-formats.md) for custom display formats
