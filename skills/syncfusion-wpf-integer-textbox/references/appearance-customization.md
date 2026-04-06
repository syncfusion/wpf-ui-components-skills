# Appearance and Customization

## Foreground Colors

Color text based on value state. Use `ApplyNegativeForeground` and `ApplyZeroColor` to enable their respective colors.

| Property | Default | Trigger |
|----------|---------|---------|
| `PositiveForeground` | Black | value > 0 (always active) |
| `NegativeForeground` | Red | value < 0, requires `ApplyNegativeForeground="True"` |
| `ZeroColor` | Green | value = 0, requires `ApplyZeroColor="True"` |

```xml
<syncfusion:IntegerTextBox Value="-25" MinValue="-100" MaxValue="100" PositiveForeground="Green" NegativeForeground="Red" ApplyNegativeForeground="True" ZeroColor="Gray" ApplyZeroColor="True" Width="150" Height="30"/>
```

## Background and Border

```xml
<syncfusion:IntegerTextBox Value="80" Background="Cyan" CornerRadius="5" BorderBrush="SteelBlue" BorderThickness="2" Width="150" Height="30"/>
```

## Selection Appearance

```xml
<syncfusion:IntegerTextBox Value="12345" SelectionBrush="Red" SelectionOpacity="0.5" Width="150" Height="30"/>
```

## Text Alignment

`TextAlignment` accepts `Left` (default), `Center`, or `Right`.

```xml
<syncfusion:IntegerTextBox Value="12345" TextAlignment="Right" Width="150" Height="30"/>
```

## ToolTip

```xml
<syncfusion:IntegerTextBox ToolTip="Enter integer value" Width="150" Height="30"/>
```

## Combined Example

```xml
<syncfusion:IntegerTextBox Value="1250000" TextAlignment="Right" PositiveForeground="DarkGreen" NegativeForeground="DarkRed" ApplyNegativeForeground="True" Background="Honeydew" CornerRadius="5" SelectionBrush="LightBlue" SelectionOpacity="0.4" GroupSeperatorEnabled="True" ToolTip="Portfolio value" Width="250" Height="35"/>
```
