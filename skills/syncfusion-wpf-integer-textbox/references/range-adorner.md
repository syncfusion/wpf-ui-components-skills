# Range Adorner

The Range Adorner displays a progress-bar fill inside the control, proportional to the current value between `MinValue` and `MaxValue`.

**Fill formula:** `(Value - MinValue) / (MaxValue - MinValue)`

> **Requires both `MinValue` and `MaxValue` to be set.**

## Basic Usage

```xml
<syncfusion:IntegerTextBox Value="63" MinValue="0" MaxValue="100" EnableRangeAdorner="True" RangeAdornerBackground="LightGreen" Width="200" Height="30"/>
```

```csharp
integerTextBox.MinValue = 0;
integerTextBox.MaxValue = 100;
integerTextBox.Value = 63;
integerTextBox.EnableRangeAdorner = true;
integerTextBox.RangeAdornerBackground = Brushes.LightGreen;
```

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `EnableRangeAdorner` | bool | false | Enable the adorner |
| `RangeAdornerBackground` | Brush | System default | Fill color |

## Notes

- Use semantic colors: green = healthy, yellow = warning, red = critical.
- Combine with `IsScrollingOnCircle="True"` for an interactive range slider experience.
