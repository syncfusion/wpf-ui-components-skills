# Value and Restrictions in WPF NumericUpdown

## Table of Contents
- [Managing Values](#managing-values)
- [Value Change Events](#value-change-events)
- [Minimum and Maximum Constraints](#minimum-and-maximum-constraints)
- [Validation Modes](#validation-modes)
- [Null Value Handling](#null-value-handling)
- [Controlling Edit Permissions](#controlling-edit-permissions)

## Managing Values

### Setting Initial Value

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" Height="25" Value="50" Width="100" />
```

**C#:**
```csharp
updown.Value = 50;
```

### Reading Current Value

```csharp
double currentValue = (double)updown.Value;
Console.WriteLine($"Current value: {currentValue}");
```

### Programmatic Value Changes

```csharp
// Increment by 5
updown.Value = (double)updown.Value + 5;

// Decrement by 10
updown.Value = (double)updown.Value - 10;

// Set to maximum
updown.Value = updown.MaxValue;
```

## Value Change Events

### ValueChanged Event

Fires **after** the value has been changed successfully.

```csharp
updown.ValueChanged += UpDown_ValueChanged;

private void UpDown_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    var newValue = e.NewValue;      // Updated value
    var oldValue = e.OldValue;      // Previous value
    
    Console.WriteLine($"Value changed from {oldValue} to {newValue}");
}
```

**Use Case:** Update dependent controls, refresh calculations, log changes.

### ValueChanging Event

Fires **before** the value change is applied. You can cancel the change.

```csharp
updown.ValueChanging += UpDown_ValueChanging;

private void UpDown_ValueChanging(object sender, ValueChangingEventArgs e)
{
    // Inspect the new value
    var proposedValue = e.NewValue;
    
    // Cancel the change if needed
    if ((double)proposedValue > 100)
    {
        e.Cancel = true;
        MessageBox.Show("Value cannot exceed 100");
    }
}
```

**Use Case:** Business rule validation, prevent invalid states, conditional changes.

### Complete Event Handler Example

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        updown.ValueChanging += (s, e) => 
        {
            // Validate before change
            if ((double)e.NewValue < 0)
                e.Cancel = true;
        };
        
        updown.ValueChanged += (d, e) => 
        {
            // React after successful change
            statusText.Text = $"Value is now {e.NewValue}";
        };
    }
}
```

## Minimum and Maximum Constraints

### Basic Min/Max Setup

Restrict the value to a specific range:

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  Height="25" 
                  Width="100"
                  MinValue="0"
                  MaxValue="100" />
```

**C#:**
```csharp
updown.MinValue = 0;
updown.MaxValue = 100;
```

### How Min/Max Work

- **Using spin buttons:** Stops incrementing/decrementing at boundaries
- **Keyboard input:** Validates against limits based on validation mode
- **Programmatic changes:** Applied after validation rules

### Example: Age Input (18-120)

```csharp
var ageUpDown = new UpDown();
ageUpDown.MinValue = 18;
ageUpDown.MaxValue = 120;
ageUpDown.Value = 25;
```

### Example: Percentage Input (0-100)

```csharp
var percentUpDown = new UpDown();
percentUpDown.MinValue = 0;
percentUpDown.MaxValue = 100;
percentUpDown.NumberDecimalDigits = 0;  // No decimals
```

## Validation Modes

### MinValidation and MaxValidation Properties

Control **when** values are validated against min/max limits.

#### OnKeyPress Validation

Validates on each keystroke. Invalid input is rejected immediately.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  MinValue="0" 
                  MaxValue="100"
                  MinValidation="OnKeyPress"
                  MaxValidation="OnKeyPress" />
```

**C#:**
```csharp
updown.MinValidation = MinValidation.OnKeyPress;
updown.MaxValidation = MaxValidation.OnKeyPress;
```

**Behavior:** User cannot type any value outside the range.

**Use Case:** Strict input validation, real-time feedback, preventing any invalid state.

#### OnLostFocus Validation

Validates when the control loses keyboard focus. Accepts any input initially, corrects on blur.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  MinValue="0" 
                  MaxValue="100"
                  MinValidation="OnLostFocus"
                  MaxValidation="OnLostFocus" />
```

**C#:**
```csharp
updown.MinValidation = MinValidation.OnLostFocus;
updown.MaxValidation = MaxValidation.OnLostFocus;
```

**Behavior:** 
- User can type any value
- When focus leaves the control, out-of-range values are clamped
- Value exceeding max becomes MaxValue
- Value below min becomes MinValue

**Use Case:** Flexible input, correction on blur, better UX for typos.

### Exceed Digit Behavior

Control what happens when user tries to exceed min/max by adding digits.

#### MaxValueOnExceedMaxDigit

When set to `True`, if the input exceeds MaxValue, the entire value is set to MaxValue instead of truncating the last digit.

**Example:**
- MaxValue = 100
- User types "200"
- Result with True: Value becomes 100 (full reset)
- Result with False: Value becomes 20 (last digit ignored)

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  MaxValue="100"
                  MaxValueOnExceedMaxDigit="True"
                  MaxValidation="OnKeyPress" />
```

**C#:**
```csharp
updown.MaxValueOnExceedMaxDigit = true;
```

#### MinValueOnExceedMinDigit

Similar to MaxValueOnExceedMaxDigit, but for minimum values.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  MinValue="10"
                  MinValueOnExceedMinDigit="True"
                  MinValidation="OnKeyPress" />
```

**C#:**
```csharp
updown.MinValueOnExceedMinDigit = true;
```

### Complete Validation Example

```csharp
var updown = new UpDown();
updown.MinValue = 0;
updown.MaxValue = 100;

// Strict on-keypress validation
updown.MinValidation = MinValidation.OnKeyPress;
updown.MaxValidation = MaxValidation.OnKeyPress;

// If user tries to exceed max, set to max
updown.MaxValueOnExceedMaxDigit = true;
updown.MinValueOnExceedMinDigit = true;
```

## Null Value Handling

### UseNullOption Property

Enables support for null values (no value set).

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  UseNullOption="True" 
                  Value="{x:Null}"
                  Height="25" 
                  Width="100" />
```

**C#:**
```csharp
updown.UseNullOption = true;
updown.Value = null;
```

### NullValue Property

Defines the numeric value to display when Value is null.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  UseNullOption="True"
                  Value="{x:Null}"
                  NullValue="0" />
```

**C#:**
```csharp
updown.UseNullOption = true;
updown.Value = null;
updown.NullValue = 0;  // Display 0 when null
```

### NullValueText Property (Watermark)

Display placeholder text instead of a number when Value is null.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  UseNullOption="True"
                  Value="{x:Null}"
                  NullValueText="Enter a value" />
```

**C#:**
```csharp
updown.UseNullOption = true;
updown.Value = null;
updown.NullValueText = "Enter a value";
```

**Behavior:** Displays "Enter a value" as placeholder text. When user enters a number, NullValueText disappears.

### Priority: NullValue vs NullValueText

If both are specified, **NullValue takes precedence** and NullValueText is ignored.

### Common Null Value Patterns

**Pattern 1: Optional field with watermark**
```csharp
updown.UseNullOption = true;
updown.Value = null;
updown.NullValueText = "Not specified";
```

**Pattern 2: Optional field with default display value**
```csharp
updown.UseNullOption = true;
updown.Value = null;
updown.NullValue = 0;  // Shows 0 but logically null
```

## Controlling Edit Permissions

### AllowEdit Property

Restrict or allow direct text editing in the control.

#### Allow Text Editing (Default)

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" AllowEdit="True" Height="25" Width="100" />
```

**C#:**
```csharp
updown.AllowEdit = true;  // Default
```

Users can click in the text area and type values directly.

#### Disable Text Editing

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  AllowEdit="False" 
                  MinValue="0"
                  MaxValue="100"
                  Height="25" 
                  Width="100" />
```

**C#:**
```csharp
updown.AllowEdit = false;
```

Users can **only** change values using spin buttons. Direct text input is prevented.

**Use Case:** 
- Strict control over input
- Force spin button usage
- Prevent accidental text entry
- Simple interfaces where buttons are preferred

### Complete Value Control Example

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        SetupUpDown();
    }
    
    private void SetupUpDown()
    {
        updown.MinValue = 0;
        updown.MaxValue = 100;
        updown.Value = 50;
        updown.Step = 10;
        
        // Strict validation
        updown.MinValidation = MinValidation.OnKeyPress;
        updown.MaxValidation = MaxValidation.OnKeyPress;
        updown.MaxValueOnExceedMaxDigit = true;
        
        // Allow null with watermark
        updown.UseNullOption = true;
        updown.NullValueText = "No value";
        
        // Event handling
        updown.ValueChanging += (s, e) => 
        {
            // Pre-validation business logic
        };
        
        updown.ValueChanged += (d, e) => 
        {
            statusLabel.Text = $"Value: {e.NewValue}";
        };
    }
}
```

---

**Next:** Read [number-formatting.md](number-formatting.md) to customize number display format.
