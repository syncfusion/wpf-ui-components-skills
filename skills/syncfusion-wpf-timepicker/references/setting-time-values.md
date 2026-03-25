# Setting Time Values in SfTimePicker

## Setting Time Using the Value Property

The primary way to set or retrieve the selected time is through the **Value** property, which accepts a `TimeSpan` object:

### In XAML

```xaml
<syncfusion:SfTimePicker Value="04:45:00" 
                         Name="sfTimePicker" />
```

### In C#

```csharp
SfTimePicker sfTimePicker = new SfTimePicker();
sfTimePicker.Value = new TimeSpan(04, 45, 00);
```

### Getting the Current Value

```csharp
TimeSpan selectedTime = sfTimePicker.Value;
Console.WriteLine($"Selected: {selectedTime.Hours}:{selectedTime.Minutes}");
```

## Default Behavior

If no value is assigned, SfTimePicker automatically uses the **current system time**.

```xaml
<!-- Will display current system time -->
<syncfusion:SfTimePicker Name="timePicker"/>
```

## Allowing Null Values

By default, SfTimePicker does not allow null values. To enable optional time selection:

### Enable Null Value Support

```xaml
<syncfusion:SfTimePicker AllowNull="True" 
                         Value="{x:Null}"
                         Name="sfTimePicker" />
```

```csharp
SfTimePicker sfTimePicker = new SfTimePicker();
sfTimePicker.AllowNull = true;
sfTimePicker.Value = null;
```

### Checking for Null

```csharp
if (sfTimePicker.Value == null && sfTimePicker.AllowNull)
{
    Console.WriteLine("No time selected");
}
```

### Important: AllowNull = False Behavior

When `AllowNull` is `false` and you try to set the value to `null`, the control automatically reverts to the **current system time**:

```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.AllowNull = false;
timePicker.Value = null; // Automatically resets to current time
```

## Watermark Text for Prompts

Show placeholder text when the value is null to guide users:

```xaml
<syncfusion:SfTimePicker AllowNull="True" 
                         Value="{x:Null}"
                         Watermark="Select a time"
                         Name="sfTimePicker" />
```

```csharp
SfTimePicker sfTimePicker = new SfTimePicker();
sfTimePicker.AllowNull = true;
sfTimePicker.Value = null;
sfTimePicker.Watermark = "Select a time";
```

**Note:** Watermark is only displayed when `AllowNull` is `true` and the value is `null`. If `AllowNull` is `false`, the current system time is displayed instead.

## Setting Value on Lost Focus

By default, when you change time in the time selector dropdown, the value is updated only when you click the **OK button**. To update the value automatically when focus is lost:

### Enable SetValueOnLostFocus

```xaml
<syncfusion:SfTimePicker SetValueOnLostFocus="True" 
                         Name="sfTimePicker" />
```

```csharp
SfTimePicker sfTimePicker = new SfTimePicker();
sfTimePicker.SetValueOnLostFocus = true;
```

**Behavior:**
- **SetValueOnLostFocus = true:** Value updates when focus leaves the time selector
- **SetValueOnLostFocus = false:** Value updates only on OK button click

## Handling Value Changes with Events

React to time value changes using the `ValueChanged` event:

```xaml
<syncfusion:SfTimePicker ValueChanged="TimePicker_ValueChanged" 
                         Name="sfTimePicker"/>
```

```csharp
private void TimePicker_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) 
{
    TimeSpan oldTime = e.OldValue as TimeSpan? ?? TimeSpan.Zero;
    TimeSpan newTime = e.NewValue as TimeSpan? ?? TimeSpan.Zero;
    
    Console.WriteLine($"Changed from {oldTime:hh\\:mm\\ tt} to {newTime:hh\\:mm\\ tt}");
    
    // Perform validation or updates here
    ValidateBusinessHours(newTime);
}
```

### Event Details

The `ValueChanged` event provides via `DependencyPropertyChangedEventArgs`:
- **OldValue** - Previously selected time
- **NewValue** - Currently selected time
- **Property** - The Value property that changed

## Common Patterns

### Pattern 1: Business Hours Validation

```csharp
private void TimePicker_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) 
{
    TimeSpan newTime = (TimeSpan)e.NewValue;
    
    // Validate business hours (9 AM to 5 PM)
    if (newTime.Hours < 9 || newTime.Hours >= 17)
    {
        MessageBox.Show("Please select a time within business hours (9 AM - 5 PM)");
        sfTimePicker.Value = (TimeSpan)e.OldValue; // Revert to previous value
    }
}
```

### Pattern 2: Time Range Selection

```csharp
SfTimePicker startTime = new SfTimePicker { Value = new TimeSpan(09, 00, 00) };
SfTimePicker endTime = new SfTimePicker { Value = new TimeSpan(17, 00, 00) };

startTime.ValueChanged += (d, e) => 
{
    TimeSpan start = (TimeSpan)e.NewValue;
    if (endTime.Value <= start)
        endTime.Value = start.Add(TimeSpan.FromHours(1));
};
```

### Pattern 3: Optional Time with Watermark

```csharp
SfTimePicker optionalTime = new SfTimePicker 
{
    AllowNull = true,
    Value = null,
    Watermark = "Optional: Select a time",
    Width = 200
};

optionalTime.ValueChanged += (d, e) => 
{
    if (e.NewValue != null)
        Console.WriteLine($"Time selected: {e.NewValue}");
    else
        Console.WriteLine("Time cleared");
};
```

## Binding Time Values

For MVVM scenarios, use WPF binding:

```xaml
<syncfusion:SfTimePicker Value="{Binding SelectedTime, Mode=TwoWay}" 
                         Name="timePicker"/>
```

```csharp
public class TimeViewModel : INotifyPropertyChanged
{
    private TimeSpan? _selectedTime = new TimeSpan(14, 30, 00);
    
    public TimeSpan? SelectedTime
    {
        get => _selectedTime;
        set
        {
            if (_selectedTime != value)
            {
                _selectedTime = value;
                OnPropertyChanged();
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    private void OnPropertyChanged([CallerMemberName] string name = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

## Next Steps

- Learn **[Time Formatting](time-formatting.md)** to display time in different formats
- Explore **[Time Selector Customization](time-selector-customization.md)** for UI customization
