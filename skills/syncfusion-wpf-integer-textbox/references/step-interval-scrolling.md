# Step Interval and Scrolling

## Overview

IntegerTextBox supports multiple interaction methods for adjusting values. The `ScrollInterval` property controls step size for all methods.

## ScrollInterval

**Type:** `int`  **Default:** `1`

```xml
<syncfusion:IntegerTextBox Value="10" ScrollInterval="5" ShowSpinButton="True" Width="150" Height="30"/>
```

Each arrow key press, mouse wheel tick, spin button click, or drag gesture changes the value by `ScrollInterval`. Respects `MinValue`/`MaxValue` boundaries.

## Keyboard Arrow Keys

- **Up ↑** — increments by `ScrollInterval`
- **Down ↓** — decrements by `ScrollInterval`

## Mouse Wheel

| Property | Default | Purpose |
|----------|---------|---------|
| `IsScrollingOnCircle` | `true` | Enable mouse wheel scrolling |

```xml
<syncfusion:IntegerTextBox Value="34" IsScrollingOnCircle="True" ScrollInterval="3" Width="150" Height="25"/>
```

## Click-and-Drag

| Property | Default | Purpose |
|----------|---------|---------|
| `EnableExtendedScrolling` | `false` | Enable click-and-drag adjustment |

Control must be **unfocused** for drag to work. Drag right/up = increment; left/down = decrement.

```xml
<syncfusion:IntegerTextBox Value="50" ScrollInterval="5" EnableExtendedScrolling="True" Width="150" Height="30"/>
```

## Text Selection on Focus

| Property | Default | Behavior |
|----------|---------|---------|
| `TextSelectionOnFocus` | `true` | Selects all text when focused |

```xml
<syncfusion:IntegerTextBox Value="12345" TextSelectionOnFocus="True" Width="150" Height="30"/>
```

Set to `false` when users edit portions of a value rather than replacing it.

## Combined Example

```xml
<syncfusion:IntegerTextBox Value="50" MinValue="0" MaxValue="100" ScrollInterval="5" ShowSpinButton="True" IsScrollingOnCircle="True" EnableExtendedScrolling="True" TextSelectionOnFocus="True" Width="200" Height="35"/>
```
