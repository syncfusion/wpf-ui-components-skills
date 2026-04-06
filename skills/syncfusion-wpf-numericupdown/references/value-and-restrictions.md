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

Set the `Value` property in XAML (`Value="50"`) or C# (`updown.Value = 50;`). Read the current value by casting `updown.Value` to the expected numeric type (e.g., `(double)updown.Value`). Programmatic changes increment/decrement by modifying the `Value` property: `updown.Value = (double)updown.Value + 5;` or set directly to max/min using `updown.MaxValue` / `updown.MinValue`.

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

Set `MinValue` and `MaxValue` properties to restrict values to a specific range. Spin buttons stop at boundaries, keyboard input validates based on the validation mode, and programmatic changes are applied after validation. 

**Examples:**
- Age input: `MinValue="18"` and `MaxValue="120"`
- Percentage: `MinValue="0"`, `MaxValue="100"`, `NumberDecimalDigits="0"` (no decimals)

## Validation Modes

### MinValidation and MaxValidation Properties

Control **when** values are validated against min/max limits.

#### OnKeyPress Validation

Set `MinValidation="OnKeyPress"` and `MaxValidation="OnKeyPress"` to validate on each keystroke. Invalid input is rejected immediately; user cannot type values outside the range. **Use case:** Strict input validation, real-time feedback.

#### OnLostFocus Validation

Set `MinValidation="OnLostFocus"` and `MaxValidation="OnLostFocus"` to validate when focus leaves the control. User can type any value initially; out-of-range values are clamped on blur (exceeding max becomes MaxValue, below min becomes MinValue). **Use case:** Flexible input, correction on blur, better UX for typos.

### Exceed Digit Behavior

When user tries to exceed min/max by adding digits:

- **`MaxValueOnExceedMaxDigit="True"`** - If input exceeds MaxValue, entire value is set to MaxValue instead of truncating. Example: MaxValue=100, user types "200" → value becomes 100 (not 20).
- **`MinValueOnExceedMinDigit="True"`** - Similar for minimum values. If input goes below MinValue, entire value is set to MinValue.

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

### Null Value Handling

Set `UseNullOption="True"` and `Value="{x:Null}"` to enable null value support.

**`NullValue` property** - Defines numeric value to display when Value is null (e.g., `NullValue="0"` shows 0).

**`NullValueText` property** - Displays placeholder watermark text instead of a number when null (e.g., `NullValueText="Enter a value"`).

**Priority:** If both are specified, `NullValue` takes precedence.

**Patterns:**
- Optional field with watermark: `UseNullOption="True"`, `NullValueText="Not specified"`
- Optional field with default display: `UseNullOption="True"`, `NullValue="0"` (shows 0 but logically null)

## Controlling Edit Permissions

### AllowEdit Property

Control whether users can edit values directly by typing in the text area.

- **`AllowEdit="True"` (default)** - Users can click in text area and type values directly.
- **`AllowEdit="False"`** - Users can **only** change values using spin buttons; direct text input is prevented.

**Use cases for `AllowEdit="False"`:** Strict control over input, force spin button usage, prevent accidental text entry, simple interfaces where buttons are preferred.

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
