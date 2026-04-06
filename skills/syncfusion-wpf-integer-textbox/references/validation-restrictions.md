# Validation and Restrictions

## Overview

IntegerTextBox provides validation and restriction capabilities to ensure data integrity. Restrict values using `MinValue`/`MaxValue`, choose when validation occurs via `MinValidation`/`MaxValidation`, and control exceed behavior.

## Min/Max Value Restrictions

| Property | Default |
|----------|---------|
| `MinValue` | `long.MinValue` |
| `MaxValue` | `long.MaxValue` |

```xml
<syncfusion:IntegerTextBox Value="50" MinValue="0" MaxValue="100" Width="150" Height="30"/>
```

## Validation Modes

| Mode | Behavior |
|------|----------|
| `OnKeyPress` | Validates immediately on each keystroke |
| `OnLostFocus` | Validates when control loses focus (default) |

```xml
<syncfusion:IntegerTextBox MinValue="10" MaxValue="100" MinValidation="OnKeyPress" MaxValidation="OnLostFocus" Width="150" Height="30"/>
```

## Exceed Digit Behavior

Only applies when validation mode is `OnKeyPress`.

| Property | When `true` | When `false` |
|----------|-------------|--------------|
| `MaxValueOnExceedMaxDigit` | Resets to `MaxValue` | Retains last valid value |
| `MinValueOnExceedMinDigit` | Resets to `MinValue` | Retains last valid value |

```xml
<syncfusion:IntegerTextBox MinValue="10" MaxValue="100" MinValidation="OnKeyPress" MaxValidation="OnKeyPress" MinValueOnExceedMinDigit="True" MaxValueOnExceedMaxDigit="True" Width="150" Height="30"/>
```

## Read-Only Mode

| Property | Default | Purpose |
|----------|---------|---------|
| `IsReadOnly` | `false` | Disable user input |
| `IsReadOnlyCaretVisible` | `false` | Show cursor in read-only mode |

```xml
<syncfusion:IntegerTextBox Value="78" IsReadOnly="True" IsReadOnlyCaretVisible="True" Width="150" Height="30"/>
```

## Custom Value Validation

| Property | Description |
|----------|-------------|
| `ValidationValue` | Expected value string |
| `InvalidValueBehavior` | `None` / `DisplayErrorMessage` / `ResetValue` |

```xml
<syncfusion:IntegerTextBox InvalidValueBehavior="DisplayErrorMessage" ValidationValue="1234" Width="150" Height="30"/>
```

## ValueChanged Event Handler

```csharp
private void IntegerTextBox_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        long value = (long)e.NewValue;
        // respond to value change
    }
}
```

## Best Practices

- Use `OnKeyPress` for critical fields (age, percentage) where invalid input must be prevented immediately.
- Use `OnLostFocus` when user experience flexibility is preferred.
- Combine with `EnableRangeAdorner="True"` for visual progress feedback.
