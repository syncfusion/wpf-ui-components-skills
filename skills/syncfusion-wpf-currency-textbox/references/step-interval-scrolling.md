# Step Interval and Scrolling

## Table of Contents
- [Overview](#overview)
- [ScrollInterval Property](#scrollinterval-property)
- [Arrow Key Navigation](#arrow-key-navigation)
- [Mouse Wheel Scrolling](#mouse-wheel-scrolling)
- [Click-and-Drag Scrolling](#click-and-drag-scrolling)
- [Text Selection on Focus](#text-selection-on-focus)
- [Combined Scrolling Example](#combined-scrolling-example)

## Overview

CurrencyTextBox provides multiple ways to increment or decrement values without manual typing:

- **Arrow keys** (Up/Down) - Keyboard-based value changes
- **Mouse wheel** - Scroll over the control to adjust value
- **Click-and-drag** - Drag mouse to increase/decrease value
- **Spin buttons** - Optional UpDown buttons (see advanced-features.md)

All methods use the `ScrollInterval` property to determine the increment/decrement amount.

## ScrollInterval Property

The `ScrollInterval` property specifies how much the value changes with each increment or decrement action.

**Property:** `ScrollInterval`  
**Type:** `double`  
**Default:** `1`

### Basic Usage

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="150"
    Height="25" 
    Value="8" 
    ScrollInterval="4"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 150;
currencyTextBox.Height = 25;
currencyTextBox.Value = 8;
currencyTextBox.ScrollInterval = 4;
```

**Behavior:** Each increment/decrement action changes the value by 4.

### Common ScrollInterval Values

| Use Case | ScrollInterval | Example Increments |
|----------|----------------|-------------------|
| Fine-tuned input | 0.01 | 1.00 → 1.01 → 1.02 |
| Standard currency | 1 | 10 → 11 → 12 |
| Quick adjustments | 10 | 100 → 110 → 120 |
| Large steps | 100 | 1000 → 1100 → 1200 |
| Custom increments | 25 | 50 → 75 → 100 |

## Arrow Key Navigation

Press Up or Down arrow keys to change the value by `ScrollInterval`.

### How It Works

- **Up Arrow:** Increases value by ScrollInterval
- **Down Arrow:** Decreases value by ScrollInterval
- **Respects Min/Max:** Won't exceed MinValue or MaxValue

### Example

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="150"
    Height="25" 
    Value="34" 
    ScrollInterval="3"
    MinValue="0"
    MaxValue="100"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 150;
currencyTextBox.Height = 25;
currencyTextBox.Value = 34;
currencyTextBox.ScrollInterval = 3;
currencyTextBox.MinValue = 0;
currencyTextBox.MaxValue = 100;
```

**Interaction:**
- Initial value: 34
- Press Up Arrow: 37
- Press Up Arrow again: 40
- Press Down Arrow: 37
- At value 99, Up Arrow goes to 100 (respects MaxValue, not 102)

### Arrow Key Use Cases

1. **Numeric spinners** - Alternative to spin button UI
2. **Quick adjustments** - Fine-tune values without mouse
3. **Keyboard-centric workflows** - Keep hands on keyboard
4. **Accessibility** - Keyboard-only navigation support

## Mouse Wheel Scrolling

Scroll the mouse wheel over the control to increment/decrement the value.

### IsScrollingOnCircle Property

**Property:** `IsScrollingOnCircle`  
**Type:** `bool`  
**Default:** `true`

**When `true`:** Mouse wheel scrolling is enabled  
**When `false`:** Mouse wheel scrolling is disabled

### Example

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="150" 
    Height="25" 
    Value="10" 
    IsScrollingOnCircle="True" 
    ScrollInterval="2"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 150;
currencyTextBox.Height = 25;
currencyTextBox.Value = 10;
currencyTextBox.IsScrollingOnCircle = true;
currencyTextBox.ScrollInterval = 2;
```

**Interaction:**
- Hover mouse over the control
- Scroll wheel up: Value increases by 2
- Scroll wheel down: Value decreases by 2

### Behavior Details

- **Focus not required** - Works even if control doesn't have focus
- **Hover activation** - Just hover over the control
- **Respects bounds** - Won't exceed MinValue/MaxValue
- **Smooth scrolling** - Each wheel click = one ScrollInterval change

### Disabling Mouse Wheel

To prevent accidental changes via mouse wheel:

```csharp
currencyTextBox.IsScrollingOnCircle = false;
```

**Use case:** Forms where mouse wheel is used for page scrolling, not value changes.

## Click-and-Drag Scrolling

Click and drag the mouse to increase or decrease the value dynamically.

### EnableExtendedScrolling Property

**Property:** `EnableExtendedScrolling`  
**Type:** `bool`  
**Default:** `false`

**When `true`:** Click-and-drag scrolling is enabled  
**When `false`:** Click-and-drag scrolling is disabled

### How It Works

1. **Control must be unfocused** - Click-and-drag only works when control doesn't have focus
2. **Click on control** - Click and hold the mouse button
3. **Drag right or up** - Value increases
4. **Drag left or down** - Value decreases
5. **Release mouse** - Value is set

### Example

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="120" 
    Height="25" 
    Value="10" 
    ScrollInterval="5" 
    EnableExtendedScrolling="True"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 120;
currencyTextBox.Height = 25;
currencyTextBox.Value = 10;
currencyTextBox.ScrollInterval = 5;
currencyTextBox.EnableExtendedScrolling = true;
```

**Interaction:**
1. Click on the control (but don't give it focus)
2. Hold mouse button down
3. Drag to the right → value increases by ScrollInterval for each movement increment
4. Drag to the left → value decreases

### Direction Mapping

| Mouse Movement | Value Change |
|----------------|--------------|
| Right | Increase |
| Left | Decrease |
| Up | Increase |
| Down | Decrease |

### Important Requirements

**⚠️ Control must be unfocused:**
- If control has focus (showing text cursor), click-and-drag won't work
- Click somewhere else first to remove focus
- Then click-and-drag will work

**Example scenario:**
```csharp
// User clicks in textbox (focus gained) - can type
// User clicks outside textbox (focus lost)
// User clicks and drags on textbox (value changes)
```

### Use Cases

1. **Quick value scrubbing** - Rapidly adjust values like volume/brightness
2. **Visual feedback** - See value change in real-time while dragging
3. **Slider alternative** - Provides slider-like behavior without slider UI
4. **Touch interfaces** - Works well with touch screens

## Text Selection on Focus

Control whether text is automatically selected when the control receives focus.

### TextSelectionOnFocus Property

**Property:** `TextSelectionOnFocus`  
**Type:** `bool`  
**Default:** `true`

**When `true`:**
- Text is automatically selected when control gets focus
- User can immediately start typing to replace value
- Common UX pattern for numeric inputs

**When `false`:**
- Text is not selected on focus
- Cursor is placed in text
- User can edit specific digits

### Example

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    TextSelectionOnFocus="False"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.TextSelectionOnFocus = true;
```

### Behavior Comparison

**TextSelectionOnFocus = true (default):**
```
User tabs to control
→ Entire value "$1,234.56" is highlighted
→ Typing "5" replaces with "$5.00"
```

**TextSelectionOnFocus = false:**
```
User tabs to control
→ Cursor appears at end: "$1,234.56|"
→ Typing "7" appends: "$1,234.567" (then formatted)
```

### Use Cases

**Enable (true) when:**
- Quick value replacement is common
- Users typically enter completely new values
- Standard numeric input fields
- Form filling workflows

**Disable (false) when:**
- Users need to edit specific digits
- Partial edits are common
- Calculator-style input
- Append-only scenarios

## Combined Scrolling Example

Here's a complete example combining all scrolling features:

**XAML:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <StackPanel Margin="20">
        <TextBlock Text="Full-Featured Currency Input:" Margin="0,0,0,5"/>
        <TextBlock Text="• Arrow keys: ±10" FontSize="10" Foreground="Gray"/>
        <TextBlock Text="• Mouse wheel: ±10" FontSize="10" Foreground="Gray"/>
        <TextBlock Text="• Click-drag: ±10" FontSize="10" Foreground="Gray"/>
        
        <syncfusion:CurrencyTextBox 
            x:Name="currencyTextBox"
            Width="200"
            Height="35"
            Value="100"
            MinValue="0"
            MaxValue="10000"
            ScrollInterval="10"
            IsScrollingOnCircle="True"
            EnableExtendedScrolling="True"
            TextSelectionOnFocus="True"
            FontSize="14"
            Margin="0,10,0,0"/>
        
        <TextBlock x:Name="infoText" 
                   Margin="0,10,0,0"
                   Text="Try arrow keys, mouse wheel, or click-and-drag!"/>
    </StackPanel>
</Window>
```

**Code-behind with value change tracking:**

```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

namespace ScrollingDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            currencyTextBox.ValueChanged += (d, e) =>
            {
                infoText.Text = $"Value changed from {e.OldValue:C2} to {e.NewValue:C2}";
            };
        }
    }
}
```

## Best Practices

1. **Choose appropriate ScrollInterval** - Match your data precision
   - Currency: 1, 10, or 100
   - Percentages: 1 or 5
   - Decimals: 0.1 or 0.01

2. **Set Min/Max bounds** - Prevent invalid values from scrolling

3. **Enable mouse wheel selectively** - Disable if conflicts with page scrolling

4. **Consider click-and-drag carefully** - May confuse users unfamiliar with the feature

5. **Default to TextSelectionOnFocus=true** - Most intuitive for numeric inputs

6. **Provide visual feedback** - Show value changes in real-time

7. **Test with keyboard-only users** - Ensure arrow keys work properly

## Troubleshooting

**Issue:** Mouse wheel not working  
**Solution:** Ensure `IsScrollingOnCircle = true`

**Issue:** Click-and-drag not working  
**Solution:** Control must not have focus. Click elsewhere first, then click-and-drag

**Issue:** Value changes too much/little  
**Solution:** Adjust `ScrollInterval` to appropriate increment

**Issue:** Text not selected on focus  
**Solution:** Set `TextSelectionOnFocus = true`

**Issue:** Value exceeds bounds  
**Solution:** Set `MinValue` and `MaxValue` properties

**Issue:** Arrow keys don't work  
**Solution:** Ensure control has keyboard focus and `ScrollInterval > 0`

## Related Features

- **Spin Buttons** - See [advanced-features.md](advanced-features.md) for ShowSpinButton property
- **Validation** - See [validation-restrictions.md](validation-restrictions.md) for Min/Max enforcement
- **Value Binding** - See [getting-started.md](getting-started.md) for data binding
