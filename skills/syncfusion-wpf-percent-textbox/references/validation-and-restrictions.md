# Validation and Restrictions

## Table of Contents
- [Restrict Values with Min/Max](#restrict-values-with-minmax)
- [Validation Timing](#validation-timing)
- [Exceed Limit Behavior](#exceed-limit-behavior)
- [Decimal Digit Restrictions](#decimal-digit-restrictions)
- [Read-Only Mode](#read-only-mode)

## Restrict Values with Min/Max

Constrain user input within specific boundaries using `MinValue` and `MaxValue` properties.

### Basic Min/Max Setup

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="50"
                           MinValue="0"
                           MaxValue="100"/>
```

**Behavior:**
- User cannot enter values below MinValue (0)
- User cannot enter values above MaxValue (100)
- Validation enforces these limits based on validation mode

### Programmatic Setup

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 50;
percentTextBox.MinValue = 0;
percentTextBox.MaxValue = 100;

// Later, retrieve limits
double min = percentTextBox.MinValue;
double max = percentTextBox.MaxValue;
```

### Negative Value Ranges

```xml
<syncfusion:PercentTextBox PercentValue="-50"
                           MinValue="-100"
                           MaxValue="100"/>
```

**Use case:** Profit/loss percentages, temperature changes, growth/decline rates

### Decimal Ranges

```xml
<syncfusion:PercentTextBox PercentValue="5.5"
                           MinValue="0.01"
                           MaxValue="99.99"/>
```

**Use case:** Precise percentage inputs like interest rates

## Validation Timing

Control **when** validation occurs using `MinValidation` and `MaxValidation` properties.

### OnKeyPress Validation

Validates immediately as user types - prevents invalid input:

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"/>
```

**Behavior:**
- Each keystroke is validated in real-time
- User cannot type digits that would exceed limits
- Provides immediate feedback
- Prevents invalid states entirely

**Example:** 
- MaxValue = 100, current value = 95
- User tries to type "9" → Allowed (would make 959, but...)
- User tries to type next digit → Blocked (would exceed 100)

### OnLostFocus Validation

Validates only when control loses focus - allows temporary invalid input:

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           MinValidation="OnLostFocus"
                           MaxValidation="OnLostFocus"/>
```

**Behavior:**
- User can type any value while focused
- Validation occurs when user tabs away or clicks elsewhere
- If value exceeds limits, it's corrected to MinValue or MaxValue
- More flexible for user input flow

**Example:**
- MaxValue = 100
- User types "250" → Allowed while typing
- User tabs to next field → Value automatically corrected to "100"

### Mixed Validation Modes

Combine different validation timings for min and max:

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           MinValue="10"
                           MaxValue="90"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnLostFocus"/>
```

**Behavior:**
- Minimum enforced immediately (can't type below 10)
- Maximum validated on focus lost (can temporarily type above 90)

**Use case:** Ensure minimum requirement while allowing temporary high values during input

### Choosing Validation Mode

| Use OnKeyPress When | Use OnLostFocus When |
|---------------------|---------------------|
| Strict data entry required | User needs input flexibility |
| Prevent invalid states entirely | Complex value composition |
| Immediate feedback is critical | Validating calculated values |
| Simple numeric ranges | Copy-paste workflows |

## Exceed Limit Behavior

Control what happens when user attempts to exceed Min/Max limits.

### MaxValueOnExceedMaxDigit

Determines behavior when user tries to exceed MaxValue:

**True (Reset to MaxValue):**
```xml
<syncfusion:PercentTextBox MinValue="0"
                           MaxValue="100"
                           MaxValidation="OnKeyPress"
                           MaxValueOnExceedMaxDigit="True"/>
```

**Behavior:**
- MaxValue = 100
- User types "200" → Value resets to "100" (MaxValue)
- Last digit that would exceed limit triggers reset

**False (Retain Partial Value):**
```xml
<syncfusion:PercentTextBox MinValue="0"
                           MaxValue="100"
                           MaxValidation="OnKeyPress"
                           MaxValueOnExceedMaxDigit="False"/>
```

**Behavior:**
- MaxValue = 100
- User types "200" → Value stays at "20"
- Last digit "0" is ignored/blocked

**Important:** `MaxValueOnExceedMaxDigit` only works when `MaxValidation="OnKeyPress"`

### MinValueOnExceedMinDigit

Determines behavior when user tries to go below MinValue:

**True (Reset to MinValue):**
```xml
<syncfusion:PercentTextBox MinValue="200"
                           MaxValue="999"
                           PercentValue="205"
                           MinValidation="OnKeyPress"
                           MinValueOnExceedMinDigit="True"/>
```

**Behavior:**
- MinValue = 200, current = 205
- User tries to change to "20" → Value resets to "200" (MinValue)

**False (Retain Old Value):**
```xml
<syncfusion:PercentTextBox MinValue="200"
                           MaxValue="999"
                           PercentValue="205"
                           MinValidation="OnKeyPress"
                           MinValueOnExceedMinDigit="False"/>
```

**Behavior:**
- MinValue = 200, current = 205
- User tries to change to "20" → Value stays at "205" (old value retained)

**Important:** `MinValueOnExceedMinDigit` only works when `MinValidation="OnKeyPress"`

### Complete Exceed Behavior Example

```xml
<syncfusion:PercentTextBox Width="150"
                           MinValue="10"
                           MaxValue="100"
                           PercentValue="50"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"
                           MinValueOnExceedMinDigit="True"
                           MaxValueOnExceedMaxDigit="True"/>
```

**Result:**
- Immediate validation on every keystroke
- Attempting to exceed 100 → Resets to 100
- Attempting to go below 10 → Resets to 10

## Decimal Digit Restrictions

Control the number of decimal places displayed and accepted.

### PercentDecimalDigits

Set exact number of decimal places:

```xml
<syncfusion:PercentTextBox PercentValue="123.456789"
                           PercentDecimalDigits="2"/>
```

**Result:** Displays "123.46 %" (rounded to 2 decimal places)

```csharp
percentTextBox.PercentDecimalDigits = 3;
percentTextBox.PercentValue = 45.6789; // Displays "45.679 %"
```

### MinPercentDecimalDigits

Set minimum number of decimal places:

```xml
<syncfusion:PercentTextBox PercentValue="100"
                           MinPercentDecimalDigits="2"/>
```

**Result:** Displays "100.00 %" (forced 2 decimals)

**Use case:** Financial applications requiring consistent decimal display

### MaxPercentDecimalDigits

Set maximum number of decimal places:

```xml
<syncfusion:PercentTextBox PercentValue="123.456789"
                           MaxPercentDecimalDigits="3"/>
```

**Result:** Displays "123.457 %" (limited to 3 decimals)

### Combining Min and Max Decimal Digits

```xml
<syncfusion:PercentTextBox PercentValue="125.32545"
                           MinPercentDecimalDigits="1"
                           MaxPercentDecimalDigits="4"/>
```

**Behavior:**
- At least 1 decimal place shown
- At most 4 decimal places shown
- Value determines actual decimals within range

**Examples:**
- Input: 100 → Display: "100.0 %" (min enforced)
- Input: 100.5 → Display: "100.5 %"
- Input: 100.123456 → Display: "100.1235 %" (max enforced, rounded)

### Property Precedence

When multiple decimal properties are set, precedence matters:

**Highest Priority: PercentDecimalDigits**
```xml
<syncfusion:PercentTextBox PercentValue="125.32545"
                           MinPercentDecimalDigits="1"
                           MaxPercentDecimalDigits="4"
                           PercentDecimalDigits="3"/>
```

**Result:** Displays exactly 3 decimals (PercentDecimalDigits overrides min/max)

**Order of Precedence:**
1. `PercentDecimalDigits` (highest)
2. `MinPercentDecimalDigits` / `MaxPercentDecimalDigits`
3. Culture defaults (lowest)

### Edge Case: MinDecimal > MaxDecimal

```xml
<syncfusion:PercentTextBox PercentValue="100.5"
                           MinPercentDecimalDigits="3"
                           MaxPercentDecimalDigits="1"/>
```

**Result:** MinPercentDecimalDigits wins - displays "100.500 %"

### Default Value

Default value for all decimal digit properties is **-1** (use culture defaults).

```xml
<!-- Uses culture default decimal places -->
<syncfusion:PercentTextBox PercentValue="50.5"
                           PercentDecimalDigits="-1"/>
```

## Read-Only Mode

Prevent user editing while allowing programmatic changes.

### Basic Read-Only

```xml
<syncfusion:PercentTextBox PercentValue="75"
                           IsReadOnly="True"/>
```

**Behavior:**
- User cannot type or edit
- User cannot paste
- Value can be changed programmatically
- Text can still be selected (by default)
- Cursor appears on click (by default)

### Hide Caret in Read-Only Mode

```xml
<syncfusion:PercentTextBox PercentValue="75"
                           IsReadOnly="True"
                           IsReadOnlyCaretVisible="False"/>
```

**Behavior:**
- User cannot edit
- User cannot see cursor/caret
- Text can still be selected

### Allow Selection in Read-Only

```xml
<syncfusion:PercentTextBox PercentValue="75"
                           IsReadOnly="True"
                           IsReadOnlyCaretVisible="True"/>
```

**Behavior:**
- User can select text (e.g., for copying)
- Cursor/caret visible
- Cannot edit or type

**Use case:** Display values that users need to copy but not modify

### Programmatic Changes in Read-Only

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.IsReadOnly = true;
percentTextBox.PercentValue = 50;

// Later, update value programmatically (works even in read-only)
percentTextBox.PercentValue = 75; // ✅ Allowed

// User cannot type or edit directly ❌
```

### Conditional Read-Only

```csharp
// Enable/disable based on conditions
if (userRole == "Admin")
{
    percentTextBox.IsReadOnly = false;
}
else
{
    percentTextBox.IsReadOnly = true;
    percentTextBox.IsReadOnlyCaretVisible = false;
}
```

### Read-Only with Data Binding

```xml
<syncfusion:PercentTextBox PercentValue="{Binding TaxRate}"
                           IsReadOnly="{Binding IsUserAdmin, Converter={StaticResource InverseBoolConverter}}"/>
```

**Behavior:** Read-only state controlled by ViewModel property

## Validation Patterns

### Pattern 1: Strict Percentage (0-100%)

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"
                           MaxValueOnExceedMaxDigit="True"
                           PercentDecimalDigits="2"/>
```

### Pattern 2: Flexible Input with Correction

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           MinValidation="OnLostFocus"
                           MaxValidation="OnLostFocus"
                           MaxPercentDecimalDigits="4"/>
```

### Pattern 3: Precision Financial Percentage

```xml
<syncfusion:PercentTextBox PercentValue="5.125"
                           MinValue="0.01"
                           MaxValue="25.00"
                           MinPercentDecimalDigits="2"
                           MaxPercentDecimalDigits="4"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"/>
```

### Pattern 4: Display-Only with Copy

```xml
<syncfusion:PercentTextBox PercentValue="{Binding CalculatedRate}"
                           IsReadOnly="True"
                           IsReadOnlyCaretVisible="True"
                           PercentDecimalDigits="2"/>
```

## Best Practices

### 1. Match Validation Mode to UX Requirements

- Use OnKeyPress for strict data entry
- Use OnLostFocus for flexible workflows

### 2. Always Set Exceed Behavior with OnKeyPress

```xml
<syncfusion:PercentTextBox MaxValidation="OnKeyPress"
                           MaxValueOnExceedMaxDigit="True"/>
```

### 3. Consider User Experience for Decimal Precision

```xml
<!-- Good: Reasonable precision -->
<syncfusion:PercentTextBox MaxPercentDecimalDigits="2"/>

<!-- Avoid: Too many decimals for typical use -->
<syncfusion:PercentTextBox MaxPercentDecimalDigits="10"/>
```

### 4. Use Read-Only for Calculated Values

```xml
<syncfusion:PercentTextBox PercentValue="{Binding EffectiveRate}"
                           IsReadOnly="True"
                           IsReadOnlyCaretVisible="False"/>
```

### 5. Provide Clear Min/Max Boundaries

```xml
<StackPanel>
    <TextBlock Text="Enter percentage (0-100):"/>
    <syncfusion:PercentTextBox MinValue="0" MaxValue="100"/>
</StackPanel>
```
