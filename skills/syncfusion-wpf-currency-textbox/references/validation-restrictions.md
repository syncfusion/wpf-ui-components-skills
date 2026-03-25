# Validation and Restrictions

## Table of Contents
- [Overview](#overview)
- [Min/Max Value Restrictions](#minmax-value-restrictions)
- [Validation Modes](#validation-modes)
- [Exceed Digit Behavior](#exceed-digit-behavior)
- [Decimal Digit Restrictions](#decimal-digit-restrictions)
- [Read-Only Mode](#read-only-mode)
- [Complete Validation Example](#complete-validation-example)

## Overview

CurrencyTextBox provides robust validation to ensure data integrity. You can restrict values within specific ranges, control validation timing, and manage decimal precision.

**Validation Features:**
- Min/Max value boundaries
- Validation timing control (OnKeyPress vs OnLostFocus)
- Automatic value correction on exceed
- Decimal digit restrictions (min/max)
- Read-only mode with optional caret visibility

## Min/Max Value Restrictions

Restrict the `Value` within maximum and minimum limits using `MinValue` and `MaxValue` properties.

### Setting MinValue and MaxValue

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="150" 
    Height="25"
    Value="455" 
    MaxValue="999.99" 
    MinValue="-999.99"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 150;
currencyTextBox.Height = 25;

// Setting minimum value
currencyTextBox.MinValue = -999.99;

// Setting maximum value
currencyTextBox.MaxValue = 999.99;

currencyTextBox.Value = 455;
```

**Behavior:** Once the value reaches the maximum or minimum limit, it cannot exceed those boundaries. The exact behavior depends on the validation mode.

## Validation Modes

Control WHEN validation occurs using `MinValidation` and `MaxValidation` properties.

### Available Modes

1. **OnKeyPress** - Validates immediately during typing
2. **OnLostFocus** - Validates when control loses focus

### OnKeyPress Mode

Validates immediately after each keystroke. Invalid input is prevented in real-time.

**Example:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.MinValue = 10;
currencyTextBox.MaxValue = 100;
currencyTextBox.MinValidation = MinValidation.OnKeyPress;
currencyTextBox.MaxValidation = MaxValidation.OnKeyPress;
```

**Behavior:**
- User cannot type values below MinValue or above MaxValue
- Invalid keys are simply ignored
- Provides immediate feedback to user
- Best for strict data entry scenarios

### OnLostFocus Mode

Validates only when the control loses keyboard focus.

**Example:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.MinValue = 10;
currencyTextBox.MaxValue = 100;
currencyTextBox.MinValidation = MinValidation.OnLostFocus;
currencyTextBox.MaxValidation = MaxValidation.OnLostFocus;
```

**Behavior:**
- User can type any value during entry
- When focus moves away, value is validated
- If invalid, value is automatically corrected to MinValue or MaxValue
- Best for user-friendly data entry

### Mode Comparison

| Aspect | OnKeyPress | OnLostFocus |
|--------|------------|-------------|
| **Validation timing** | During typing | After leaving control |
| **Invalid input** | Prevented | Allowed then corrected |
| **User experience** | Strict, immediate | Flexible, forgiving |
| **Use case** | Critical data entry | General data entry |

### Mixed Mode Example

```xml
<syncfusion:CurrencyTextBox 
    Width="150" 
    Height="25"
    MinValue="10" 
    MaxValue="100"
    MinValidation="OnKeyPress" 
    MaxValidation="OnLostFocus"/>
```

**Behavior:** Minimum is enforced strictly during typing, but maximum is only checked when leaving the field.

## Exceed Digit Behavior

Control what happens when user attempts to enter values beyond limits during OnKeyPress validation.

### MaxValueOnExceedMaxDigit

Determines behavior when input exceeds MaxValue during OnKeyPress validation.

**Property:** `MaxValueOnExceedMaxDigit` (bool)

**When `true`:**
- Value is set to MaxValue immediately
- Example: MaxValue=100, typing "200" → value becomes 100

**When `false`:**
- Previous valid digits are retained
- Example: MaxValue=100, typing "200" → value stays at "20", last "0" ignored

**Example:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.MaxValue = 100;
currencyTextBox.MaxValidation = MaxValidation.OnKeyPress;
currencyTextBox.MaxValueOnExceedMaxDigit = true;

// User types "200"
// Result: Value becomes 100 immediately
```

**⚠️ Important:** `MaxValueOnExceedMaxDigit` only works when `MaxValidation = OnKeyPress`.

### MinValueOnExceedMinDigit

Determines behavior when input falls below MinValue during OnKeyPress validation.

**Property:** `MinValueOnExceedMinDigit` (bool)

**When `true`:**
- Value is set to MinValue immediately
- Example: MinValue=200, Value=205, changing to "20" → becomes 200

**When `false`:**
- Old value is retained
- Example: MinValue=200, Value=205, changing to "20" → stays 205

**Example:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.MinValue = 200;
currencyTextBox.MinValidation = MinValidation.OnKeyPress;
currencyTextBox.MinValueOnExceedMinDigit = true;

// Current value: 205
// User tries to change to "20"
// Result: Value becomes 200 immediately
```

**⚠️ Important:** `MinValueOnExceedMinDigit` only works when `MinValidation = OnKeyPress`.

### Combined Exceed Behavior Example

```xml
<syncfusion:CurrencyTextBox 
    Width="150" 
    Height="25"
    MinValue="10" 
    MaxValue="100"
    MinValueOnExceedMinDigit="True" 
    MaxValueOnExceedMaxDigit="True"
    MinValidation="OnKeyPress" 
    MaxValidation="OnLostFocus"/>
```

**Behavior:**
- MinValidation is OnKeyPress with MinValueOnExceedMinDigit=True
  - Cannot enter values < 10; automatically becomes 10
- MaxValidation is OnLostFocus
  - Can type values > 100, but corrected to 100 when losing focus

## Decimal Digit Restrictions

Control the number of decimal places allowed in the value.

### Available Properties

- `CurrencyDecimalDigits` - Exact decimal places to display
- `MinimumCurrencyDecimalDigits` - Minimum decimal places
- `MaximumCurrencyDecimalDigits` - Maximum decimal places

**Default value for all:** `-1` (uses culture default)

### Basic Decimal Restriction

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    Value="125.32545" 
    MaximumCurrencyDecimalDigits="4"
    MinimumCurrencyDecimalDigits="1" />
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 125.32545;
currencyTextBox.MaximumCurrencyDecimalDigits = 4;
currencyTextBox.MinimumCurrencyDecimalDigits = 1;
```

**Result:** `125.3255` (rounded to 4 decimal places)

### Behavior Rules

**MinimumCurrencyDecimalDigits:**
- Pads with zeros if value has fewer decimals
- Example: Min=2, Value=125.3 → displays as 125.30

**MaximumCurrencyDecimalDigits:**
- Truncates or rounds if value has more decimals
- Example: Max=2, Value=125.789 → displays as 125.79

**Conflict resolution:**
- If `MinimumCurrencyDecimalDigits > MaximumCurrencyDecimalDigits`
- MinimumCurrencyDecimalDigits takes precedence

### CurrencyDecimalDigits Priority

When `CurrencyDecimalDigits` is set, it overrides both min and max.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    Value="125.32545"
    MaximumCurrencyDecimalDigits="4"
    MinimumCurrencyDecimalDigits="1" 
    CurrencyDecimalDigits="3" />
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 125.32545;
currencyTextBox.MaximumCurrencyDecimalDigits = 4;
currencyTextBox.MinimumCurrencyDecimalDigits = 1;
currencyTextBox.CurrencyDecimalDigits = 3; // This wins
```

**Result:** `125.325` (exactly 3 decimal places)

### Decimal Restriction Examples

**Example 1: No decimals (whole numbers only)**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234.56;
currencyTextBox.CurrencyDecimalDigits = 0;
// Result: 1235 (rounded)
```

**Example 2: High precision (8 decimals)**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 0.12345678901;
currencyTextBox.MinimumCurrencyDecimalDigits = 2;
currencyTextBox.MaximumCurrencyDecimalDigits = 8;
// Result: 0.12345679 (8 decimals, rounded)
```

**Example 3: Fixed 2 decimals (common currency format)**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 123.4;
currencyTextBox.CurrencyDecimalDigits = 2;
// Result: 123.40
```

## Read-Only Mode

Prevent user input while still displaying the value.

### IsReadOnly Property

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    IsReadOnly="True" 
    Value="10" 
    IsReadOnlyCaretVisible="True"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 10;
currencyTextBox.IsReadOnly = true;
currencyTextBox.IsReadOnlyCaretVisible = true;
```

### IsReadOnlyCaretVisible

Controls whether the caret (text cursor) is visible in read-only mode.

**When `true`:**
- User can see cursor
- Can select text
- Cannot edit

**When `false`:**
- No cursor visible
- Can still select text
- Cannot edit

### Programmatic Changes in Read-Only Mode

Read-only mode only prevents USER input. You can still change the value programmatically.

**Example:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.IsReadOnly = true;
currencyTextBox.Value = 100;

// This works even in read-only mode
currencyTextBox.Value = 200; // Successfully changed programmatically
```

### Read-Only Use Cases

1. **Display calculated totals** - Show sum/average without allowing edits
2. **Locked fields** - Temporarily disable input during processing
3. **View-only mode** - Display financial data in a read-only form
4. **Conditional editing** - Enable/disable based on user permissions

**Example: Conditional read-only**

```csharp
// Enable editing only for admins
currencyTextBox.IsReadOnly = !CurrentUser.IsAdmin;
```

## Complete Validation Example

Here's a comprehensive example combining all validation features:

**XAML:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <StackPanel Margin="20">
        <!-- Strict validation -->
        <TextBlock Text="Strict Entry (OnKeyPress):"/>
        <syncfusion:CurrencyTextBox 
            Width="200"
            Height="30"
            MinValue="10"
            MaxValue="100"
            MinValidation="OnKeyPress"
            MaxValidation="OnKeyPress"
            MaxValueOnExceedMaxDigit="True"
            MinValueOnExceedMinDigit="True"
            CurrencyDecimalDigits="2"
            Margin="0,5,0,15"/>
        
        <!-- Flexible validation -->
        <TextBlock Text="Flexible Entry (OnLostFocus):"/>
        <syncfusion:CurrencyTextBox 
            Width="200"
            Height="30"
            MinValue="0"
            MaxValue="10000"
            MinValidation="OnLostFocus"
            MaxValidation="OnLostFocus"
            MinimumCurrencyDecimalDigits="2"
            MaximumCurrencyDecimalDigits="4"
            Margin="0,5,0,15"/>
        
        <!-- Read-only display -->
        <TextBlock Text="Read-Only Total:"/>
        <syncfusion:CurrencyTextBox 
            Width="200"
            Height="30"
            Value="1234.56"
            IsReadOnly="True"
            IsReadOnlyCaretVisible="False"
            Background="LightGray"
            Margin="0,5,0,0"/>
    </StackPanel>
</Window>
```

## Best Practices

1. **Choose appropriate validation mode** - OnKeyPress for strict, OnLostFocus for user-friendly
2. **Set reasonable limits** - Don't use extreme values for min/max
3. **Match business rules** - Align decimal precision with domain requirements
4. **Provide user feedback** - Consider tooltips explaining restrictions
5. **Test edge cases** - Verify behavior at min/max boundaries
6. **Use read-only for calculated values** - Prevent accidental changes
7. **Consider exceed digit settings carefully** - Balance strictness with usability

## Troubleshooting

**Issue:** Validation not working  
**Solution:** Ensure MinValidation/MaxValidation properties are set

**Issue:** Exceed digit behavior not triggering  
**Solution:** Only works with OnKeyPress validation mode

**Issue:** Decimal places not restricted  
**Solution:** Check CurrencyDecimalDigits doesn't override min/max settings

**Issue:** Can't edit value  
**Solution:** Check if IsReadOnly is set to true

**Issue:** Value jumps to min/max unexpectedly  
**Solution:** Review exceed digit settings and validation modes
