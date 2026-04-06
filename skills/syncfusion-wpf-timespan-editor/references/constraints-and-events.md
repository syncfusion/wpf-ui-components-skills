# Constraints & Events

## Table of Contents
- [Min/Max Value Constraints](#minmax-value-constraints)
- [ValueChanged Event](#valuechanged-event)
- [Event Binding in XAML](#event-binding-in-xaml)
- [Event Handling in C#](#event-handling-in-c)
- [Validation Patterns](#validation-patterns)

---

## Min/Max Value Constraints

Set `MinValue` and `MaxValue` to restrict user input. The control automatically clamps values at boundaries.

**Quick Setup:**
```xml
<!-- XAML: Range 2 days - 10 days -->
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         MinValue="2.00:00:00"
                         MaxValue="10.00:00:00" />
```

```csharp
// C#: Same constraints
TimeSpanEdit editor = new TimeSpanEdit {
    Value = new TimeSpan(5, 12, 30, 45),
    MinValue = new TimeSpan(2, 0, 0, 0),
    MaxValue = new TimeSpan(10, 0, 0, 0)
};
```

**Behavior:** Values automatically clamp to min/max boundaries. If user tries to exceed 10 days max, value becomes 10 days.

**Common Scenarios:**

| Use Case | Min | Max | Example |
|----------|-----|-----|---------|
| Work Day | 6h | 10h | `MinValue="0.06:00:00"` `MaxValue="0.10:00:00"` |
| Project | 1d | 30d | `MinValue="1.00:00:00"` `MaxValue="30.00:00:00"` |
| Timer | 0s | 60s | `MinValue="0.00:00:00"` `MaxValue="0.00:01:00"` |

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

Attach a `ValueChanged` event handler to respond to value changes:

```xml
<!-- Simple binding -->
<syncfusion:TimeSpanEdit x:Name="timeSpanEdit"
                         Value="5.12:30:45"
                         ValueChanged="TimeSpanEdit_ValueChanged" />
```

**With Status Display:**
```xml
<StackPanel>
    <syncfusion:TimeSpanEdit x:Name="durationEditor"
                             Value="5.00:00:00"
                             Width="150" Height="35"
                             ValueChanged="DurationEditor_ValueChanged" />
    <TextBlock x:Name="statusText" Text="Change the duration value" FontSize="12" />
</StackPanel>
```

---

## Event Handling in C#

**Signature:** All ValueChanged handlers use `DependencyPropertyChangedEventArgs`:

```csharp
private void TimeSpanEdit_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    TimeSpan? oldValue = (TimeSpan?)e.OldValue;
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    // Handle change...
}
```

**Display Status Example:**
```csharp
private void DurationEditor_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    if (newValue.HasValue) {
        statusText.Text = $"Duration: {newValue.Value.Days}d {newValue.Value.Hours}h {newValue.Value.Minutes}m";
    } else {
        statusText.Text = "Duration cleared";
    }
}
```

**Programmatic Subscription:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit { Value = new TimeSpan(5, 12, 30, 0) };
editor.ValueChanged += (d, e) => Debug.WriteLine($"New: {e.NewValue}");
```

---

## Validation Patterns

| Pattern | Use Case | Key Method |
|---------|----------|-----------|
| **Logging** | Track all value changes in diagnostics | `Debug.WriteLine($"[{DateTime.Now:HH:mm:ss}] Duration changed: {oldValue} → {newValue}")` |
| **Error Display** | Show validation feedback inline | Check bounds in `ValueChanged`, update TextBlock with error text |
| **Cascading Updates** | Update dependent UI when value changes | Calculate derived values (e.g., hourly cost) on change |
| **Form Validation** | Require valid value before submission | Check `Value.HasValue` and validate constraints in click handler |
| **MVVM Binding** | Two-way binding with validation in ViewModel | Implement `INotifyPropertyChanged`, validate in property setter |

**Basic Validation Example:**
```csharp
private void Duration_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    
    if (!newValue.HasValue) {
        errorMessage.Text = "Please enter a duration";
        return;
    }
    
    if (newValue.Value.TotalHours < 0.5) {
        errorMessage.Text = "Minimum 30 minutes required";
    }
    else if (newValue.Value.TotalHours > 12) {
        errorMessage.Text = "Maximum 12 hours allowed";
    }
    else {
        errorMessage.Text = "";  // Valid
    }
}
```

**Form Submission Validation:**
```csharp
private void SubmitButton_Click(object sender, RoutedEventArgs e) {
    if (durationEditor.Value == null) {
        MessageBox.Show("Please enter a duration", "Validation Error");
        return;
    }
    ProcessTimeEntry(durationEditor.Value.Value);  // Valid - process
}
```

**MVVM Pattern with ViewModel Validation:**
```csharp
public class TimeEntryViewModel : INotifyPropertyChanged {
    private TimeSpan? _duration;
    private string _validationError = "";
    
    public TimeSpan? Duration {
        get { return _duration; }
        set {
            if (_duration != value) {
                _duration = value;
                ValidateDuration();
                OnPropertyChanged(nameof(Duration));
            }
        }
    }
    
    public string ValidationError {
        get { return _validationError; }
        private set { _validationError = value; OnPropertyChanged(nameof(ValidationError)); }
    }
    
    private void ValidateDuration() {
        if (Duration == null) {
            ValidationError = "Duration is required";
        }
        else if (Duration.Value.TotalHours < 0.5) {
            ValidationError = "Minimum 30 minutes";
        }
        else if (Duration.Value.TotalHours > 12) {
            ValidationError = "Maximum 12 hours";
        }
        else {
            ValidationError = "";
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName) => 
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```

---

## Common Event Scenarios

| Scenario | Behavior | Handler Logic |
|----------|----------|---------------|
| **Duration Accumulation** | Add each entered value to total | Maintain `TimeSpan totalTime` field; add `newValue` on each change; display accumulated result |
| **Auto-save** | Persist to database immediately | Call async `SaveDurationAsync(newValue.Value)` in handler; show confirmation status |
| **Conditional UI** | Enable/disable controls based on value | If null: disable button, show warning; if > 8h: show alert; else enable normally |

**Duration Accumulator Example:**
```csharp
private TimeSpan totalTime = TimeSpan.Zero;

private void Duration_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    if (newValue.HasValue) {
        totalTime = totalTime.Add(newValue.Value);
        totalDisplay.Text = $"Total: {totalTime.TotalHours:F1} hours";
    }
}
```

**Conditional UI Activation:**
```csharp
private void Duration_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    TimeSpan? newValue = (TimeSpan?)e.NewValue;
    submitButton.IsEnabled = newValue.HasValue && newValue.Value.TotalHours <= 8;
    warningPanel.Visibility = (newValue?.TotalHours > 8) ? Visibility.Visible : Visibility.Collapsed;
}
```

