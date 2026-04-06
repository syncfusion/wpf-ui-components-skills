# Value Management

## Overview

IntegerTextBox supports direct value setting, data binding, paste modes, spin buttons, event handling, null values, and watermark text.

## Value Property

**Type:** `long?` (nullable)  **Default:** `0`

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" Value="10" Width="150" Height="25"/>
```

> **Important:** Use the `Value` property, not `Text`, to get/set integer values.

## Data Binding (MVVM)

```xml
<syncfusion:IntegerTextBox Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}" Width="150" Height="25"/>
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private long myValue;
    public long MyValue
    {
        get => myValue;
        set { if (myValue != value) { myValue = value; OnPropertyChanged(); } }
    }
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string p = null)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(p));
}
```

## Paste Mode

| Mode | Behavior |
|------|----------|
| `Default` | Replaces entire value with pasted content |
| `Advanced` | Context-sensitive insert at cursor; blocks decimals |

```xml
<syncfusion:IntegerTextBox PasteMode="Advanced" Value="12345" Width="150" Height="30"/>
```

## Spin Buttons

```xml
<syncfusion:IntegerTextBox Value="50" MinValue="0" MaxValue="100" ScrollInterval="5" ShowSpinButton="True" Width="150" Height="30"/>
```

## ValueChanged Event

```csharp
private void IntegerTextBox_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        long newVal = (long)e.NewValue;
        // handle change
    }
}
```

## Null Value Handling

| Property | Default | Purpose |
|----------|---------|---------|
| `UseNullOption` | `false` | Enable null value support |
| `NullValue` | `null` | Value shown when null |

```xml
<syncfusion:IntegerTextBox UseNullOption="True" NullValue="0" WatermarkText="Enter value" WatermarkTextIsVisible="True" Width="150" Height="25"/>
```

> When both `NullValue` and `WatermarkText` are set, `NullValue` takes priority.

## Watermark

```xml
<syncfusion:IntegerTextBox UseNullOption="True" WatermarkText="Enter age" WatermarkTextIsVisible="True" WatermarkTextForeground="Gray" WatermarkOpacity="0.6" Width="150" Height="25"/>
```
