# Validation and Restrictions

## Table of Contents
- [Overview](#overview)
- [Min and Max Value Restrictions](#min-and-max-value-restrictions)
- [Validation Modes](#validation-modes)
- [OnKeyPress Validation](#onkeypress-validation)
- [OnLostFocus Validation](#onlostfocus-validation)
- [Exceed Digit Behavior](#exceed-digit-behavior)
- [Complete Validation Examples](#complete-validation-examples)
- [Read-Only Mode](#read-only-mode)
- [Custom Value Validation](#custom-value-validation)
- [Best Practices](#best-practices)

## Overview

IntegerTextBox provides comprehensive validation and restriction capabilities to ensure data integrity. You can restrict values within min/max ranges, choose when validation occurs, control behavior when limits are exceeded, and implement custom validation logic.

## Min and Max Value Restrictions

Restrict the IntegerTextBox value within specific boundaries using `MinValue` and `MaxValue` properties.

**Properties:**
- `MinValue` - Minimum allowed value (default: `int.MinValue`)
- `MaxValue` - Maximum allowed value (default: `int.MaxValue`)

### Basic Min/Max Configuration

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="150" 
                           Height="30"
                           Value="50"
                           MinValue="0" 
                           MaxValue="100"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Width = 150;
integerTextBox.Height = 30;
integerTextBox.Value = 50;
integerTextBox.MinValue = 0;
integerTextBox.MaxValue = 100;
```

The control prevents users from entering values outside the 0-100 range based on the validation mode.

### Negative Range Example

```xml
<syncfusion:IntegerTextBox Value="-50"
                           MinValue="-100"
                           MaxValue="100"
                           Width="150"
                           Height="30"/>
```

### Large Number Range

```xml
<syncfusion:IntegerTextBox Value="5000000"
                           MinValue="0"
                           MaxValue="999999999"
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```

## Validation Modes

Control **when** validation occurs using `MinValidation` and `MaxValidation` properties.

**Properties:**
- `MinValidation` - When to validate minimum value (default: `MinValidation.OnLostFocus`)
- `MaxValidation` - When to validate maximum value (default: `MaxValidation.OnLostFocus`)

**Available Modes:**
- `OnKeyPress` - Validates immediately as user types
- `OnLostFocus` - Validates only when control loses focus

### Validation Mode Configuration

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="150" 
                           Height="30"
                           MinValue="10" 
                           MaxValue="100"
                           MinValidation="OnKeyPress" 
                           MaxValidation="OnLostFocus"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.MinValue = 10;
integerTextBox.MaxValue = 100;
integerTextBox.MinValidation = MinValidation.OnKeyPress;
integerTextBox.MaxValidation = MaxValidation.OnLostFocus;
```

## OnKeyPress Validation

When set to `OnKeyPress`, validation occurs immediately after each keystroke, preventing invalid input in real-time.

### Characteristics

- **Immediate feedback** - User cannot type invalid values
- **Strict enforcement** - Impossible to enter values outside limits
- **Best for** - Critical fields where invalid values must be prevented

### OnKeyPress Example

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"
                           Width="150"
                           Height="30"/>
```

**Behavior:**
- User tries to type "150" in a field with MaxValue="100"
- After typing "1", "5", the next "0" is prevented
- Control stops accepting input that would exceed the limit

### OnKeyPress with Min Validation

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="20"
                           MaxValue="200"
                           MinValidation="OnKeyPress"
                           Width="150"
                           Height="30"/>
```

If user tries to enter "5" (less than MinValue 20):
- The control prevents entering values digit-by-digit that would result in a final value below 20
- Or applies exceed behavior (see next section)

## OnLostFocus Validation

When set to `OnLostFocus`, validation occurs only when the control loses focus, allowing temporary invalid input.

### Characteristics

- **Deferred validation** - User can type anything temporarily
- **Flexible input** - Allows users to clear and retype
- **Best for** - Better user experience in most scenarios

### OnLostFocus Example

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="10"
                           MaxValue="100"
                           MinValidation="OnLostFocus"
                           MaxValidation="OnLostFocus"
                           Width="150"
                           Height="30"/>
```

**Behavior:**
- User can type "500" even though MaxValue is 100
- When user tabs out or clicks elsewhere, value automatically corrects to 100
- Same applies for values below MinValue

### Mixed Validation Modes

You can use different modes for min and max:

```xml
<syncfusion:IntegerTextBox MinValue="0"
                           MaxValue="1000"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnLostFocus"
                           Width="150"
                           Height="30"/>
```

This configuration:
- Prevents negative values immediately (OnKeyPress for min)
- Allows typing values over 1000 but corrects on focus loss (OnLostFocus for max)

## Exceed Digit Behavior

Control what happens when user attempts to enter values exceeding limits during `OnKeyPress` validation.

**Properties:**
- `MaxValueOnExceedMaxDigit` - Behavior when exceeding max (default: `false`)
- `MinValueOnExceedMinDigit` - Behavior when exceeding min (default: `false`)

**⚠️ Important:** These properties only work when validation mode is `OnKeyPress`.

### MaxValueOnExceedMaxDigit

**When `true`:** Resets to MaxValue when digit would exceed limit  
**When `false`:** Retains previous valid value

```xml
<syncfusion:IntegerTextBox MinValue="0"
                           MaxValue="100"
                           MaxValidation="OnKeyPress"
                           MaxValueOnExceedMaxDigit="True"
                           Width="150"
                           Height="30"/>
```

**Example Scenario:**
- MaxValue is 100
- Current value is 50
- User tries to type "200"
- After typing "2", "0", next "0" would create 200
- **If `MaxValueOnExceedMaxDigit="True"`:** Value becomes 100
- **If `MaxValueOnExceedMaxDigit="False"`:** Value remains 20 (keeps what was typed so far)

### MinValueOnExceedMinDigit

**When `true`:** Resets to MinValue when digit would go below limit  
**When `false`:** Retains previous valid value

```xml
<syncfusion:IntegerTextBox MinValue="200"
                           MaxValue="1000"
                           Value="500"
                           MinValidation="OnKeyPress"
                           MinValueOnExceedMinDigit="True"
                           Width="150"
                           Height="30"/>
```

**Example Scenario:**
- MinValue is 200
- Current value is 500
- User tries to change to "50"
- After typing "5", then "0", value would be 50 (below MinValue)
- **If `MinValueOnExceedMinDigit="True"`:** Value becomes 200
- **If `MinValueOnExceedMinDigit="False"`:** Value remains 500 (retains old value)

### Combined Exceed Behavior

```xml
<syncfusion:IntegerTextBox MinValue="10"
                           MaxValue="100"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"
                           MinValueOnExceedMinDigit="True"
                           MaxValueOnExceedMaxDigit="True"
                           Value="50"
                           Width="150"
                           Height="30"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.MinValue = 10;
integerTextBox.MaxValue = 100;
integerTextBox.Value = 50;
integerTextBox.MinValidation = MinValidation.OnKeyPress;
integerTextBox.MaxValidation = MaxValidation.OnKeyPress;
integerTextBox.MinValueOnExceedMinDigit = true;
integerTextBox.MaxValueOnExceedMaxDigit = true;
```

## Complete Validation Examples

### Strict Age Validation

Prevent invalid ages immediately:

```xml
<StackPanel Margin="20">
    <TextBlock Text="Enter Age (18-120):" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox x:Name="ageTextBox"
                               Value="25"
                               MinValue="18"
                               MaxValue="120"
                               MinValidation="OnKeyPress"
                               MaxValidation="OnKeyPress"
                               MaxValueOnExceedMaxDigit="True"
                               Width="150"
                               Height="30"/>
</StackPanel>
```

### Percentage Input (0-100)

```xml
<syncfusion:IntegerTextBox Value="75"
                           MinValue="0"
                           MaxValue="100"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"
                           MaxValueOnExceedMaxDigit="True"
                           EnableRangeAdorner="True"
                           Width="150"
                           Height="30"/>
```

### Flexible Quantity Input

Allow temporary invalid input, correct on focus loss:

```xml
<syncfusion:IntegerTextBox Value="1"
                           MinValue="1"
                           MaxValue="999"
                           MinValidation="OnLostFocus"
                           MaxValidation="OnLostFocus"
                           Width="150"
                           Height="30"/>
```

### Temperature Range (-50 to 50)

```xml
<syncfusion:IntegerTextBox Value="20"
                           MinValue="-50"
                           MaxValue="50"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"
                           PositiveForeground="Red"
                           NegativeForeground="Blue"
                           ApplyNegativeForeground="True"
                           Width="150"
                           Height="30"/>
```

## Read-Only Mode

Prevent user input while still allowing programmatic updates and value display.

**Properties:**
- `IsReadOnly` - Disable user input (default: `false`)
- `IsReadOnlyCaretVisible` - Show cursor in read-only mode (default: `false`)

### Basic Read-Only

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Value="78" 
                           IsReadOnly="True"
                           Width="150"
                           Height="30"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Value = 78;
integerTextBox.IsReadOnly = true;
```

**Behavior:**
- User cannot type or edit the value
- Value can still be changed programmatically
- Text can be selected and copied

### Read-Only with Caret Visible

```xml
<syncfusion:IntegerTextBox Value="78" 
                           IsReadOnly="True"
                           IsReadOnlyCaretVisible="True"
                           Width="150"
                           Height="30"/>
```

```csharp
integerTextBox.IsReadOnly = true;
integerTextBox.IsReadOnlyCaretVisible = true;
```

**Behavior:**
- Cursor appears in the control
- User can navigate with arrow keys
- Still cannot edit the value
- Better for accessibility

### Calculated Field Example

```xml
<StackPanel Margin="20">
    <TextBlock Text="Unit Price:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox x:Name="priceTextBox"
                               Value="10"
                               MinValue="1"
                               ValueChanged="Calculate"
                               Width="150"
                               Height="30"/>
    
    <TextBlock Text="Quantity:" Margin="0,15,0,5"/>
    <syncfusion:IntegerTextBox x:Name="quantityTextBox"
                               Value="5"
                               MinValue="1"
                               ValueChanged="Calculate"
                               Width="150"
                               Height="30"/>
    
    <TextBlock Text="Total (Read-Only):" Margin="0,15,0,5"/>
    <syncfusion:IntegerTextBox x:Name="totalTextBox"
                               Value="50"
                               IsReadOnly="True"
                               IsReadOnlyCaretVisible="True"
                               Background="LightGray"
                               Width="150"
                               Height="30"/>
</StackPanel>
```

```csharp
private void Calculate(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (priceTextBox.Value != null && quantityTextBox.Value != null)
    {
        int total = priceTextBox.Value.Value * quantityTextBox.Value.Value;
        totalTextBox.Value = total;
    }
}
```

## Custom Value Validation

Implement custom validation logic using the `ValidationValue` and `InvalidValueBehavior` properties.

**Properties:**
- `ValidationValue` - Expected value for validation (default: `string.Empty`)
- `InvalidValueBehavior` - Action when value doesn't match (default: `None`)

**InvalidValueBehavior Options:**
- `None` - No validation occurs
- `DisplayErrorMessage` - Shows MessageBox with error
- `ResetValue` - Resets value to 0

### Display Error Message

```xml
<syncfusion:IntegerTextBox Width="150" 
                           Height="30"
                           InvalidValueBehavior="DisplayErrorMessage"
                           ValidationValue="1234"
                           VerticalAlignment="Center"
                           HorizontalAlignment="Center"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox
{
    Width = 150,
    Height = 30,
    InvalidValueBehavior = InvalidInputBehavior.DisplayErrorMessage,
    ValidationValue = "1234",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};
```

**Behavior:**
- User enters any value other than 1234
- On focus loss, MessageBox appears: "String validation failed"
- Value remains unchanged

### Reset Value on Invalid Input

```xml
<syncfusion:IntegerTextBox Width="150" 
                           Height="30"
                           InvalidValueBehavior="ResetValue"
                           ValidationValue="1000"/>
```

**Behavior:**
- User enters any value other than 1000
- On focus loss, value resets to 0
- No error message displayed

### PIN Code Validation Example

```xml
<StackPanel Margin="20">
    <TextBlock Text="Enter PIN (1234):" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox x:Name="pinTextBox"
                               Width="150"
                               Height="30"
                               InvalidValueBehavior="DisplayErrorMessage"
                               ValidationValue="1234"
                               MaxValue="9999"/>
    
    <Button Content="Submit" 
            Click="Submit_Click" 
            Width="150"
            Height="30"
            Margin="0,10,0,0"/>
</StackPanel>
```

## Best Practices

### Choose Appropriate Validation Mode

**Use OnKeyPress when:**
- Data integrity is critical (age, percentage)
- Invalid values would cause errors downstream
- User should never see invalid states

**Use OnLostFocus when:**
- Better user experience is priority
- Users need flexibility to clear and retype
- Invalid temporary states are acceptable

### Combine with Visual Feedback

```xml
<syncfusion:IntegerTextBox Value="75"
                           MinValue="0"
                           MaxValue="100"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightGreen"
                           Width="200"
                           Height="30"/>
```

### Provide Clear Labels and Instructions

```xml
<StackPanel>
    <TextBlock Text="Enter Age (Must be 18-120):" 
               Margin="0,0,0,5"
               FontWeight="Bold"/>
    <syncfusion:IntegerTextBox MinValue="18"
                               MaxValue="120"
                               MinValidation="OnKeyPress"
                               MaxValidation="OnKeyPress"
                               WatermarkText="Age"
                               WatermarkTextIsVisible="True"
                               UseNullOption="True"
                               Width="150"
                               Height="30"/>
    <TextBlock Text="* Required field" 
               Foreground="Red" 
               FontSize="10"
               Margin="0,5,0,0"/>
</StackPanel>
```

### Handle Validation Events

```csharp
private void IntegerTextBox_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    IntegerTextBox textBox = d as IntegerTextBox;
    
    if (e.NewValue != null)
    {
        int value = (int)e.NewValue;
        
        // Provide feedback based on value
        if (value == textBox.MaxValue)
        {
            MessageBox.Show("Maximum value reached!", "Warning", 
                          MessageBoxButton.OK, MessageBoxImage.Warning);
        }
        else if (value == textBox.MinValue)
        {
            MessageBox.Show("Minimum value reached!", "Warning", 
                          MessageBoxButton.OK, MessageBoxImage.Warning);
        }
    }
}
```

### Document Validation Rules

```xml
<GroupBox Header="Account Balance Configuration" Padding="10">
    <StackPanel>
        <TextBlock Text="Minimum Balance:" Margin="0,0,0,5"/>
        <syncfusion:IntegerTextBox Value="1000"
                                   MinValue="0"
                                   MaxValue="1000000"
                                   MinValidation="OnKeyPress"
                                   MaxValidation="OnLostFocus"
                                   GroupSeperatorEnabled="True"
                                   Width="200"
                                   Height="30"/>
        <TextBlock Text="Range: $0 - $1,000,000" 
                   FontSize="10" 
                   Foreground="Gray"
                   Margin="0,5,0,0"/>
    </StackPanel>
</GroupBox>
```

## Common Validation Scenarios

### Form Validation

```xml
<Grid Margin="20">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <StackPanel Grid.Row="0" Margin="0,0,0,10">
        <TextBlock Text="Age (18-100):"/>
        <syncfusion:IntegerTextBox MinValue="18" 
                                   MaxValue="100"
                                   MinValidation="OnKeyPress"
                                   MaxValidation="OnKeyPress"
                                   Width="200" 
                                   Height="30"/>
    </StackPanel>
    
    <StackPanel Grid.Row="1" Margin="0,0,0,10">
        <TextBlock Text="Years of Experience (0-50):"/>
        <syncfusion:IntegerTextBox MinValue="0" 
                                   MaxValue="50"
                                   MinValidation="OnLostFocus"
                                   MaxValidation="OnLostFocus"
                                   Width="200" 
                                   Height="30"/>
    </StackPanel>
    
    <Button Grid.Row="2" 
            Content="Submit" 
            Width="200" 
            Height="35"
            HorizontalAlignment="Left"/>
</Grid>
```

### Configuration Settings

```xml
<syncfusion:IntegerTextBox Value="30"
                           MinValue="1"
                           MaxValue="3600"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnLostFocus"
                           ToolTip="Timeout in seconds (1-3600)"
                           Width="150"
                           Height="30"/>
```

This comprehensive validation system ensures data integrity while providing flexibility for different user experience requirements.
