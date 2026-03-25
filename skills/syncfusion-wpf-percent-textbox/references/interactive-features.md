# Interactive Features

## Table of Contents
- [Scroll Interval](#scroll-interval)
- [Keyboard Navigation](#keyboard-navigation)
- [Mouse Wheel Scrolling](#mouse-wheel-scrolling)
- [Click and Drag Scrolling](#click-and-drag-scrolling)
- [Text Selection on Focus](#text-selection-on-focus)
- [Spin Buttons](#spin-buttons)
- [Range Adorner](#range-adorner)

## Scroll Interval

The `ScrollInterval` property defines the step size for incrementing or decrementing the value.

### Basic ScrollInterval Setup

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           ScrollInterval="5"/>
```

**Behavior:**
- Up arrow key: Increases value by 5 (50 → 55)
- Down arrow key: Decreases value by 5 (50 → 45)
- Mouse wheel: Changes value by 5 per scroll

**Default:** ScrollInterval = 1

### Programmatic Setup

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 50;
percentTextBox.ScrollInterval = 5;
```

### Different Interval Sizes

**Fine adjustments (decimal intervals):**
```xml
<syncfusion:PercentTextBox PercentValue="10.0"
                           ScrollInterval="0.5"/>
```
**Result:** Each interaction changes by 0.5 (10.0 → 10.5 → 11.0)

**Coarse adjustments:**
```xml
<syncfusion:PercentTextBox PercentValue="50"
                           ScrollInterval="10"/>
```
**Result:** Each interaction changes by 10 (50 → 60 → 70)

### ScrollInterval with Min/Max Limits

```xml
<syncfusion:PercentTextBox PercentValue="95"
                           MinValue="0"
                           MaxValue="100"
                           ScrollInterval="10"/>
```

**Behavior:**
- At 95, pressing Up arrow goes to 100 (stops at MaxValue, not 105)
- At 5, pressing Down arrow goes to 0 (stops at MinValue, not -5)

### Use Cases by Interval Size

| Interval | Use Case | Example |
|----------|----------|---------|
| 0.1 | Precision adjustments | Interest rates: 5.1%, 5.2%, 5.3% |
| 0.5 | Half-percent steps | Ratings: 3.5%, 4.0%, 4.5% |
| 1 | Standard increments | General percentages: 50%, 51%, 52% |
| 5 | Quick adjustments | Discount tiers: 10%, 15%, 20% |
| 10 | Coarse steps | Completion: 0%, 10%, 20%, 30% |
| 25 | Quarter steps | Progress: 0%, 25%, 50%, 75%, 100% |

## Keyboard Navigation

Change values using keyboard arrow keys.

### Up Arrow Key

Increases value by ScrollInterval:

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           ScrollInterval="2"/>
```

**Action:** Press Up arrow
**Result:** 50 → 52 → 54 → 56...

### Down Arrow Key

Decreases value by ScrollInterval:

**Action:** Press Down arrow
**Result:** 50 → 48 → 46 → 44...

### Rapid Key Presses

Hold down arrow keys for continuous changes:

```csharp
// Current value: 50
// Hold Up arrow for 2 seconds
// Result: 50 → 52 → 54 → ... → 74 (with ScrollInterval=2)
```

### Keyboard Navigation Example

```xml
<StackPanel>
    <TextBlock Text="Use arrow keys to adjust:"/>
    <syncfusion:PercentTextBox x:Name="percentTextBox"
                               Width="150"
                               Height="30"
                               PercentValue="50"
                               MinValue="0"
                               MaxValue="100"
                               ScrollInterval="5"/>
    <TextBlock Text="↑ Increase by 5% | ↓ Decrease by 5%" 
               Foreground="Gray"
               FontSize="10"/>
</StackPanel>
```

### Keyboard with Validation

```xml
<syncfusion:PercentTextBox PercentValue="98"
                           MaxValue="100"
                           ScrollInterval="5"
                           MaxValidation="OnKeyPress"/>
```

**Behavior:**
- At 98, press Up arrow → Goes to 100 (not 103)
- MaxValue enforced during keyboard navigation

## Mouse Wheel Scrolling

Change values by scrolling the mouse wheel over the control.

### Enable Mouse Wheel Scrolling

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           ScrollInterval="3"
                           IsScrollingOnCircle="True"/>
```

**Behavior:**
- Scroll up: Increases by ScrollInterval (50 → 53)
- Scroll down: Decreases by ScrollInterval (50 → 47)
- Control must have focus or cursor over it

**Default:** IsScrollingOnCircle = True (enabled by default)

### Disable Mouse Wheel Scrolling

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           IsScrollingOnCircle="False"/>
```

**Result:** Mouse wheel has no effect on value

**Use case:** Prevent accidental changes when scrolling page

### Mouse Wheel with Different Intervals

**Fine control:**
```xml
<syncfusion:PercentTextBox PercentValue="10.0"
                           ScrollInterval="0.1"
                           IsScrollingOnCircle="True"/>
```

**Coarse control:**
```xml
<syncfusion:PercentTextBox PercentValue="50"
                           ScrollInterval="25"
                           IsScrollingOnCircle="True"/>
```

### Complete Mouse Wheel Example

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           ScrollInterval="5"
                           IsScrollingOnCircle="True"/>
```

**Usage:**
1. Click on control or hover cursor over it
2. Scroll mouse wheel up/down
3. Value changes by 5 per scroll notch

### Mouse Wheel Best Practices

**When to enable (IsScrollingOnCircle=True):**
- Numeric configuration controls
- Dashboard value adjustments
- Interactive sliders alternative
- Quick data entry scenarios

**When to disable (IsScrollingOnCircle=False):**
- Forms within scrollable pages
- Readonly/display values
- Mobile/touch-only applications
- Prevent accidental changes

## Click and Drag Scrolling

Change values by clicking and dragging the mouse.

### Enable Click and Drag

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           ScrollInterval="5"
                           EnableExtendedScrolling="True"/>
```

**Default:** EnableExtendedScrolling = False

### How Click and Drag Works

**Requirements:**
1. Control must NOT have focus
2. Click on control
3. Drag mouse while holding button down

**Behavior:**
- Drag right or up: Increases value
- Drag left or down: Decreases value
- Change rate depends on ScrollInterval

### Click and Drag Example

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           ScrollInterval="2"
                           EnableExtendedScrolling="True"/>
```

**Usage:**
1. Ensure control is unfocused (click elsewhere first)
2. Click on the PercentTextBox
3. Drag mouse right → Value increases (50 → 52 → 54...)
4. Drag mouse left → Value decreases (50 → 48 → 46...)

### Directional Behavior

**Horizontal drag:**
- Right: Increases value
- Left: Decreases value

**Vertical drag:**
- Up: Increases value
- Down: Decreases value

Both directions work simultaneously - diagonal drag also changes value.

### Click and Drag with ScrollInterval

```xml
<!-- Fast changes -->
<syncfusion:PercentTextBox ScrollInterval="10"
                           EnableExtendedScrolling="True"/>

<!-- Slow/precise changes -->
<syncfusion:PercentTextBox ScrollInterval="0.5"
                           EnableExtendedScrolling="True"/>
```

### Important Notes

**Unfocused State Required:**
```csharp
// Before drag, ensure unfocused
anotherControl.Focus(); // Move focus away

// OR click on control border/background first
// Then drag will work
```

**Focus Behavior:**
- If control has focus, dragging selects text instead
- Must lose focus before drag scrolling works

## Text Selection on Focus

Control whether text is automatically selected when the control receives focus.

### Enable Auto-Selection (Default)

```xml
<syncfusion:PercentTextBox PercentValue="123456"
                           TextSelectionOnFocus="True"/>
```

**Behavior:**
- Click or tab to control → All text selected automatically
- Easy to overwrite entire value
- Standard behavior for numeric inputs

**Default:** TextSelectionOnFocus = True

### Disable Auto-Selection

```xml
<syncfusion:PercentTextBox PercentValue="123456"
                           TextSelectionOnFocus="False"/>
```

**Behavior:**
- Click or tab to control → Cursor appears at click position
- Must manually select text
- Allows editing within value

### Comparison

**With TextSelectionOnFocus="True":**
```xml
<syncfusion:PercentTextBox PercentValue="50.25"
                           TextSelectionOnFocus="True"/>
```
- Focus → "50.25" all selected
- Type "75" → Replaces with "75"

**With TextSelectionOnFocus="False":**
```xml
<syncfusion:PercentTextBox PercentValue="50.25"
                           TextSelectionOnFocus="False"/>
```
- Focus → Cursor at click position
- Type "75" → Inserts at cursor

### Use Cases

**Enable auto-selection when:**
- Values are typically replaced entirely
- Quick data entry workflows
- Calculator-style inputs
- Default system behavior expected

**Disable auto-selection when:**
- Users edit within values (e.g., change "50.25" to "50.75")
- Precise cursor positioning needed
- Non-standard input behavior desired

## Spin Buttons

Add up/down buttons for incrementing/decrementing values.

### Enable Spin Buttons

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           ShowSpinButton="True"
                           ScrollInterval="5"/>
```

**Result:** Small up/down arrow buttons appear on the right side

**Default:** ShowSpinButton = False

### Spin Button Behavior

**Up Button (▲):**
- Click: Increases value by ScrollInterval
- Hold: Continuously increases value

**Down Button (▼):**
- Click: Decreases value by ScrollInterval
- Hold: Continuously decreases value

### Complete Spin Button Example

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           ScrollInterval="10"
                           ShowSpinButton="True"/>
```

**Usage:**
- Click ▲ → 50 → 60 → 70...
- Click ▼ → 50 → 40 → 30...

### Spin Buttons with Validation

```xml
<syncfusion:PercentTextBox PercentValue="95"
                           MaxValue="100"
                           ScrollInterval="10"
                           ShowSpinButton="True"
                           MaxValidation="OnKeyPress"/>
```

**Behavior:**
- At 95, click ▲ → Goes to 100 (stops at MaxValue)
- Cannot exceed limits via spin buttons

### Programmatic Setup

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.ShowSpinButton = true;
percentTextBox.ScrollInterval = 5;
percentTextBox.MinValue = 0;
percentTextBox.MaxValue = 100;
```

## Range Adorner

Visual progress indicator showing value relative to min/max range.

### Enable Range Adorner

```xml
<syncfusion:PercentTextBox PercentValue="65"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"/>
```

**Result:** Background fills proportionally based on value (65% filled)

**Default:** EnableRangeAdorner = False

**Important:** Range adorner requires both `MinValue` and `MaxValue` to be set.

### How Range Adorner Works

The adorner fills the control background from left to right based on the percentage of (Value - MinValue) / (MaxValue - MinValue):

**Examples:**
- MinValue=0, MaxValue=100, PercentValue=50 → 50% filled
- MinValue=0, MaxValue=100, PercentValue=75 → 75% filled
- MinValue=0, MaxValue=200, PercentValue=100 → 50% filled

### Range Adorner Appearance

```xml
<syncfusion:PercentTextBox PercentValue="70"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           Width="200"
                           Height="30"/>
```

**Visual result:** 70% of control width filled with default adorner color

### Customize Range Adorner Background

```xml
<syncfusion:PercentTextBox PercentValue="65"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightGreen"/>
```

**Result:** Fill color is light green instead of default

**C#:**
```csharp
percentTextBox.EnableRangeAdorner = true;
percentTextBox.RangeAdornerBackground = Brushes.LightGreen;
percentTextBox.MinValue = 0;
percentTextBox.MaxValue = 100;
percentTextBox.PercentValue = 65;
```

### Range Adorner with Custom Colors

**Status indication:**
```xml
<syncfusion:PercentTextBox PercentValue="85"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightBlue"
                           Background="White"/>
```

**Gradient adorner:**
```xml
<syncfusion:PercentTextBox PercentValue="60"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True">
    <syncfusion:PercentTextBox.RangeAdornerBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="Green" Offset="0"/>
            <GradientStop Color="Yellow" Offset="0.5"/>
            <GradientStop Color="Red" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:PercentTextBox.RangeAdornerBackground>
</syncfusion:PercentTextBox>
```

### Dynamic Range Adorner

```csharp
// Change adorner color based on value
private void PercentTextBox_PercentValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    double value = (double)e.NewValue;
    
    if (value >= 80)
        percentTextBox.RangeAdornerBackground = Brushes.Green;
    else if (value >= 50)
        percentTextBox.RangeAdornerBackground = Brushes.Yellow;
    else
        percentTextBox.RangeAdornerBackground = Brushes.Red;
}
```

### Range Adorner Requirements

**Must have:**
- `EnableRangeAdorner="True"`
- `MinValue` set
- `MaxValue` set

**Won't display if:**
- MinValue or MaxValue not set
- EnableRangeAdorner is False
- Value is outside MinValue-MaxValue range

### Range Adorner Not Displaying

```xml
<!-- ❌ Won't work - missing MinValue/MaxValue -->
<syncfusion:PercentTextBox EnableRangeAdorner="True"
                           PercentValue="50"/>

<!-- ✅ Works - all required properties set -->
<syncfusion:PercentTextBox EnableRangeAdorner="True"
                           MinValue="0"
                           MaxValue="100"
                           PercentValue="50"/>
```

### Range Adorner Use Cases

**Progress tracking:**
```xml
<syncfusion:PercentTextBox PercentValue="{Binding CompletionPercent}"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightBlue"
                           IsReadOnly="True"/>
```

**Goal tracking:**
```xml
<syncfusion:PercentTextBox PercentValue="{Binding SalesTarget}"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightGreen"/>
```

## Complete Interactive Example

Combining all interactive features:

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="200"
                           Height="35"
                           
                           <!-- Value and limits -->
                           PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           
                           <!-- Scroll settings -->
                           ScrollInterval="5"
                           IsScrollingOnCircle="True"
                           EnableExtendedScrolling="True"
                           
                           <!-- UI features -->
                           ShowSpinButton="True"
                           TextSelectionOnFocus="True"
                           
                           <!-- Range adorner -->
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightBlue"/>
```

**Available interactions:**
- ⌨️ Up/Down arrow keys (±5)
- 🖱️ Mouse wheel scrolling (±5)
- 🖱️ Click and drag (±5)
- 🔼🔽 Spin buttons (±5)
- 📊 Visual progress indicator

## Best Practices

### 1. Choose Appropriate ScrollInterval

```xml
<!-- Precision work -->
<syncfusion:PercentTextBox ScrollInterval="0.1"/>

<!-- General use -->
<syncfusion:PercentTextBox ScrollInterval="1"/>

<!-- Quick adjustments -->
<syncfusion:PercentTextBox ScrollInterval="10"/>
```

### 2. Disable Mouse Wheel in Scrollable Forms

```xml
<ScrollViewer>
    <StackPanel>
        <!-- Prevent accidental changes while scrolling form -->
        <syncfusion:PercentTextBox IsScrollingOnCircle="False"/>
    </StackPanel>
</ScrollViewer>
```

### 3. Use Range Adorner for Progress Indication

```xml
<syncfusion:PercentTextBox EnableRangeAdorner="True"
                           IsReadOnly="True"
                           MinValue="0"
                           MaxValue="100"/>
```

### 4. Enable Spin Buttons for Touch Interfaces

```xml
<syncfusion:PercentTextBox ShowSpinButton="True"
                           ScrollInterval="5"/>
```

### 5. Keep TextSelectionOnFocus=True for Quick Entry

```xml
<syncfusion:PercentTextBox TextSelectionOnFocus="True"/>
```
