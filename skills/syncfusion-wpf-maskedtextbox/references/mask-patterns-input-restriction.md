# Mask Patterns and Input Restriction

This guide covers creating custom mask patterns, input validation, and restriction techniques for SfMaskedEdit.

## Table of Contents
- [Mask Types](#mask-types)
- [Mask Elements Reference](#mask-elements-reference)
- [Allow Specific Values](#allow-specific-values)
- [Restrict Specific Values](#restrict-specific-values)
- [Basic Mask Examples](#basic-mask-examples)
- [Input Validation Modes](#input-validation-modes)
- [Validation Results](#validation-results)
- [Error Indication](#error-indication)
- [Prompt Character Configuration](#prompt-character-configuration)

## Mask Types

SfMaskedEdit supports two mask types via the `MaskType` property:

### Simple Mask (Default)

Basic mask with predefined characters. Use for straightforward patterns.

```xml
<syncfusion:SfMaskedEdit MaskType="Simple" Mask="(000) 000-0000"/>
```

### RegEx Mask (Recommended)

Uses regular expression patterns for complex validation. Provides more flexibility and power.

```xml
<syncfusion:SfMaskedEdit MaskType="RegEx" Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"/>
```

**When to use:**
- **Simple:** Fixed-length patterns with repetitive characters
- **RegEx:** Complex patterns, conditional logic, advanced validation

## Mask Elements Reference

The following table lists RegEx mask elements for creating custom patterns:

| Element | Description | Example |
|---------|-------------|---------|
| `[]` | Defines a character set | `[ABC]` matches A, B, or C |
| `[ABC]` | Matches any single character in the set | `[0-9]` matches any digit |
| `[^ABC]` | Matches any character NOT in the set | `[^0-9]` matches non-digits |
| `[A-Z]` | Matches characters in range (inclusive) | `[A-Z]` matches A to Z |
| `\d` | Matches any digit (0-9) | Same as `[0-9]` |
| `\D` | Matches any non-digit | Same as `[^0-9]` |
| `\w` | Matches word character (a-zA-Z0-9_) | Same as `[a-zA-Z0-9_]` |
| `\W` | Matches non-word character | Same as `[^a-zA-Z0-9_]` |
| `\s` | Matches whitespace (space, tab, newline) | |
| `\S` | Matches non-whitespace | |
| `(?=subexpression)` | Positive lookahead (must match) | `(?=123)` allows only "123" |
| `(?!subexpression)` | Negative lookahead (must NOT match) | `(?!000)` disallows "000" |
| `{n}` | Exactly n occurrences | `\d{3}` matches 3 digits |
| `{n,}` | n or more occurrences | `\d{3,}` matches 3+ digits |
| `{n,m}` | Between n and m occurrences | `\d{3,5}` matches 3-5 digits |
| `+` | One or more occurrences | `\d+` matches 1+ digits |
| `*` | Zero or more occurrences | `\d*` matches 0+ digits |
| `?` | Zero or one occurrence (optional) | `-?` optional minus sign |
| `\|` | Boolean OR | `cat\|dog` matches "cat" or "dog" |
| `.` | Matches any character | Culture-dependent |
| `\` | Escape character | `\.` matches literal period |

### Special Characters in Patterns

To match literal special characters, escape them with `\`:

```xml
<!-- Match literal dot: $ 100.00 -->
<syncfusion:SfMaskedEdit Mask="$ \d+\.\d{2}" MaskType="RegEx"/>

<!-- Match literal parentheses: (123) -->
<syncfusion:SfMaskedEdit Mask="\(\d{3}\)" MaskType="RegEx"/>
```

## Allow Specific Values

Use positive lookahead `(?=subexpression)` to allow ONLY specific values.

### Example: Allow Only "123"

```xml
<syncfusion:SfMaskedEdit 
    Mask="(?=123)\d{3}" 
    MaskType="RegEx"/>
```

```csharp
SfMaskedEdit sfMaskedEdit = new SfMaskedEdit();
sfMaskedEdit.MaskType = MaskType.RegEx;
sfMaskedEdit.Mask = @"(?=123)\d{3}";
```

**How it works:**
- Allows entering first two digits normally
- When entering the third digit, validates against the pattern
- Only accepts if the complete input is "123"
- Rejects any other combination

### Example: Allow Multiple Specific Values

```xml
<!-- Allow only "100", "200", or "300" -->
<syncfusion:SfMaskedEdit 
    Mask="(?=100|200|300)\d{3}" 
    MaskType="RegEx"/>
```

## Restrict Specific Values

Use negative lookahead `(?!subexpression)` to disallow specific values.

### Example: Disallow "000" and "666"

```xml
<syncfusion:SfMaskedEdit 
    Mask="(?!000)(?!666)\d{3}" 
    MaskType="RegEx"/>
```

```csharp
SfMaskedEdit sfMaskedEdit = new SfMaskedEdit();
sfMaskedEdit.MaskType = MaskType.RegEx;
sfMaskedEdit.Mask = @"(?!000)(?!666)\d{3}";
```

**How it works:**
- `\d{3}` accepts 3 digits
- `(?!000)` rejects if value is "000"
- `(?!666)` rejects if value is "666"
- All other 3-digit combinations are accepted

### Example: Social Security Number Pattern

```xml
<!-- SSN: Cannot start with 000, 666, or 900-999 -->
<syncfusion:SfMaskedEdit 
    Mask="(?!000)(?!666)(?!9\d{2})\d{3}-(?!00)\d{2}-(?!0000)\d{4}" 
    MaskType="RegEx"/>
```

## Basic Mask Examples

### Numeric Patterns

#### Positive Whole Numbers Only

```xml
<syncfusion:SfMaskedEdit Mask="\d+" MaskType="RegEx"/>
```
**Accepts:** 1, 123, 9999  
**Rejects:** -5, 12.34, abc

#### Negative Whole Numbers Only

```xml
<syncfusion:SfMaskedEdit Mask="-\d+" MaskType="RegEx"/>
```
**Accepts:** -1, -123, -9999  
**Rejects:** 5, -12.34, abc

#### Positive or Negative Whole Numbers

```xml
<syncfusion:SfMaskedEdit Mask="-?\d+" MaskType="RegEx"/>
```
**Accepts:** 1, -5, 123, -9999  
**Rejects:** 12.34, abc

#### Positive or Negative Float Numbers

```xml
<syncfusion:SfMaskedEdit Mask="-?\d+\.?\d*" MaskType="RegEx"/>
```
**Accepts:** 1, -5, 12.34, -98.76  
**Rejects:** abc, 12.34.56

#### Fixed Decimal Places (Currency)

```xml
<syncfusion:SfMaskedEdit Mask="$ \d+\.\d{2}" MaskType="RegEx"/>
```
**Accepts:** $ 10.00, $ 1234.56  
**Format:** Dollar sign + digits + dot + 2 decimal places

#### Percentage with Decimals

```xml
<syncfusion:SfMaskedEdit Mask="\d+\.\d{2}%" MaskType="RegEx"/>
```
**Accepts:** 12.50%, 99.99%  
**Format:** Digits + dot + 2 decimals + percent sign

### Alphanumeric Patterns

#### Alphanumeric with Spaces

```xml
<syncfusion:SfMaskedEdit Mask="[a-zA-Z0-9 ]*" MaskType="RegEx"/>
```
**Accepts:** abc, ABC123, Hello World  
**Rejects:** @, #, special chars

#### Alphanumeric without Spaces

```xml
<syncfusion:SfMaskedEdit Mask="[a-zA-Z0-9]*" MaskType="RegEx"/>
```
**Accepts:** abc, ABC123, test123  
**Rejects:** Hello World, @#$

#### Letters Only (Any Case)

```xml
<syncfusion:SfMaskedEdit Mask="[a-zA-Z]+" MaskType="RegEx"/>
```
**Accepts:** abc, ABC, Hello  
**Rejects:** 123, test123

#### Uppercase Letters Only

```xml
<syncfusion:SfMaskedEdit Mask="[A-Z]+" MaskType="RegEx"/>
```
**Accepts:** ABC, HELLO  
**Rejects:** abc, Hello, 123

### Communication Patterns

#### Email Address

```xml
<syncfusion:SfMaskedEdit 
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
    MaskType="RegEx"/>
```
**Accepts:** user@domain.com, test.user@example.co  
**Format:** username + @ + domain + . + 2-3 letter extension

#### Phone Number (US Format)

```xml
<syncfusion:SfMaskedEdit 
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"/>
```
**Accepts:** (455) 345-6789  
**Format:** (###) ###-####

#### International Phone (with country code)

```xml
<syncfusion:SfMaskedEdit 
    Mask="\+1 [0-9]\d{2}-[0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"/>
```
**Accepts:** +1 455-345-6789  
**Format:** +1 ###-###-####

### Date and Time Patterns

#### Time (24-hour format)

```xml
<syncfusion:SfMaskedEdit 
    Mask="(0[0-9]|1[0-9]|2[0-3]):[0-5][0-9]"
    MaskType="RegEx"/>
```
**Accepts:** 00:00, 13:45, 23:59  
**Format:** HH:MM (00-23:00-59)

#### Date (MM/DD/YYYY)

```xml
<syncfusion:SfMaskedEdit 
    Mask="(0[1-9]|1[0-2])/(0[1-9]|[12][0-9]|3[01])/\d{4}"
    MaskType="RegEx"/>
```
**Accepts:** 01/15/2024, 12/31/2023  
**Format:** MM/DD/YYYY with validation

### Product and ID Patterns

#### Product Key (XXXXX-XXXXX-XXXXX)

```xml
<syncfusion:SfMaskedEdit 
    Mask="[A-Z\d]{5}-[A-Z\d]{5}-[A-Z\d]{5}-[A-Z\d]{5}-[A-Z\d]{5}"
    MaskType="RegEx"/>
```
**Accepts:** AB12C-3D45E-6F78G-9H01I-2J34K  
**Format:** Five groups of 5 alphanumeric characters

#### Credit Card Number

```xml
<syncfusion:SfMaskedEdit 
    Mask="\d{4} \d{4} \d{4} \d{4}"
    MaskType="RegEx"/>
```
**Accepts:** 1234 5678 9012 3456  
**Format:** #### #### #### ####

#### Zip Code (US)

```xml
<syncfusion:SfMaskedEdit Mask="\d{5}-\d{4}" MaskType="RegEx"/>
```
**Accepts:** 12345-6789  
**Format:** #####-####

#### Bank Account Number (8-12 digits)

```xml
<syncfusion:SfMaskedEdit Mask="\d{8,12}" MaskType="RegEx"/>
```
**Accepts:** 12345678, 123456789012  
**Length:** 8 to 12 digits

#### Bank SWIFT Code

```xml
<syncfusion:SfMaskedEdit 
    Mask="[A-Z]{6}[0-9]{2}[A-Z]{3}"
    MaskType="RegEx"/>
```
**Accepts:** ABCDEF12GHI  
**Format:** 6 letters + 2 digits + 3 letters

### Technical Patterns

#### Hexadecimal Number

```xml
<syncfusion:SfMaskedEdit 
    Mask="\\x[0-9A-Fa-f]{1,2}"
    MaskType="RegEx"/>
```
**Accepts:** \x0A, \xFF, \x1  
**Format:** \x followed by 1-2 hex digits

#### Octal Number

```xml
<syncfusion:SfMaskedEdit 
    Mask="\\0[0-7]{1,3}"
    MaskType="RegEx"/>
```
**Accepts:** \00, \077, \177  
**Format:** \0 followed by 1-3 octal digits

#### Hexadecimal Color Code

```xml
<syncfusion:SfMaskedEdit 
    Mask="#[A-Fa-f0-9]{6}"
    MaskType="RegEx"/>
```
**Accepts:** #FF5733, #00aaff  
**Format:** # followed by 6 hex digits

#### IP Address (Simple)

```xml
<syncfusion:SfMaskedEdit 
    Mask="\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}"
    MaskType="RegEx"/>
```
**Accepts:** 192.168.1.1, 10.0.0.1  
**Note:** Doesn't validate 0-255 range

## Input Validation Modes

Control when input validation occurs using the `ValidationMode` property.

### KeyPress Validation (Default)

Validates on each keystroke:

```xml
<syncfusion:SfMaskedEdit 
    ValidationMode="KeyPress"
    Mask="\+1 [0-9]\d{2}-[0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"/>
```

```csharp
sfMaskedEdit.ValidationMode = InputValidationMode.KeyPress;
```

**Behavior:**
- Validates immediately as user types
- Blocks invalid characters
- `Value` property updates after each valid keystroke
- `ValueChanged` event fires on each valid key press

**Use when:**
- You need real-time validation feedback
- Preventing invalid input is critical
- User needs immediate correction

### LostFocus Validation

Validates when control loses focus:

```xml
<syncfusion:SfMaskedEdit 
    ValidationMode="LostFocus"
    Mask="\+1 [0-9]\d{2}-[0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"/>
```

```csharp
sfMaskedEdit.ValidationMode = InputValidationMode.LostFocus;
```

**Behavior:**
- User can type freely
- Validation occurs when user tabs out or clicks elsewhere
- `Value` property updates only after validation succeeds
- `ValueChanged` event fires only on lost focus (if valid)

**Use when:**
- Less intrusive UX is desired
- Users need flexibility while typing
- Validation is complex and should wait for complete input

## Validation Results

### Checking Validation Status

Use the `HasError` property to check if validation failed:

```csharp
if (sfMaskedEdit.HasError)
{
    MessageBox.Show("Invalid input. Please correct and try again.");
}
else
{
    string validValue = sfMaskedEdit.Value;
    // Proceed with valid value
}
```

### Example: Form Validation on Submit

```xml
<StackPanel>
    <syncfusion:SfMaskedEdit 
        x:Name="phoneInput"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"
        ValidationMode="LostFocus"/>
    
    <Button Content="Submit" Click="Submit_Click"/>
</StackPanel>
```

```csharp
private void Submit_Click(object sender, RoutedEventArgs e)
{
    if (!phoneInput.HasError && !string.IsNullOrEmpty(phoneInput.Value))
    {
        string phone = phoneInput.Value;
        MessageBox.Show($"Valid phone: {phone}");
    }
    else
    {
        MessageBox.Show("Please enter a valid phone number");
        phoneInput.Focus();
    }
}
```

### Example: LostFocus Validation Handler

```xml
<syncfusion:SfMaskedEdit 
    x:Name="emailInput"
    ValidationMode="LostFocus"
    LostFocus="EmailInput_LostFocus"
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
    MaskType="RegEx"/>
```

```csharp
private void EmailInput_LostFocus(object sender, RoutedEventArgs e)
{
    var maskedEdit = sender as SfMaskedEdit;
    
    if (maskedEdit.HasError)
    {
        MessageBox.Show("Please enter a valid email address");
        maskedEdit.Focus(); // Return focus to fix error
    }
}
```

## Error Indication

### Error Border

When validation fails, an error border is displayed automatically. Customize the color:

```xml
<syncfusion:SfMaskedEdit 
    ErrorBorderBrush="Red"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"/>
```

```csharp
sfMaskedEdit.ErrorBorderBrush = Brushes.Red;
```

**Default:** Red border when `HasError` is true

### Custom Error Indication

```xml
<StackPanel>
    <syncfusion:SfMaskedEdit 
        x:Name="phoneInput"
        ErrorBorderBrush="Orange"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"
        LostFocus="PhoneInput_LostFocus"/>
    
    <TextBlock x:Name="errorText" 
               Foreground="Red" 
               Visibility="Collapsed"
               Text="Invalid phone number format"/>
</StackPanel>
```

```csharp
private void PhoneInput_LostFocus(object sender, RoutedEventArgs e)
{
    if (phoneInput.HasError)
    {
        errorText.Visibility = Visibility.Visible;
    }
    else
    {
        errorText.Visibility = Visibility.Collapsed;
    }
}
```

## Prompt Character Configuration

### Setting Prompt Character

The prompt character indicates missing input positions:

```xml
<syncfusion:SfMaskedEdit 
    PromptChar="_"
    Mask="\+1 [0-9]\d{2}-[0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"/>
```

```csharp
sfMaskedEdit.PromptChar = '_';
```

**Default:** `_` (underscore)

**Display:** `+1 ___-___-____`

### Custom Prompt Characters

```xml
<!-- Use 'X' as prompt -->
<syncfusion:SfMaskedEdit PromptChar="X" Mask="\d{4}-\d{4}" MaskType="RegEx"/>
<!-- Display: XXXX-XXXX -->

<!-- Use space as prompt (invisible) -->
<syncfusion:SfMaskedEdit PromptChar=" " Mask="\d{4}" MaskType="RegEx"/>
<!-- Display: (spaces) -->
```

### ShowPromptOnFocus

Control when prompt characters are visible:

```xml
<syncfusion:SfMaskedEdit 
    ShowPromptOnFocus="True"
    PromptChar="_"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"/>
```

```csharp
sfMaskedEdit.ShowPromptOnFocus = true;
```

**ShowPromptOnFocus = False (default):**
- Prompts appear only while typing
- Empty control shows watermark or nothing

**ShowPromptOnFocus = True:**
- Prompts appear when control gets focus
- User immediately sees expected format
- Better UX for complex masks

### Example: Comparing Prompt Display

```xml
<StackPanel>
    <!-- Prompts only while typing -->
    <syncfusion:SfMaskedEdit 
        ShowPromptOnFocus="False"
        PromptChar="_"
        Watermark="Phone Number"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"/>
    
    <!-- Prompts on focus -->
    <syncfusion:SfMaskedEdit 
        ShowPromptOnFocus="True"
        PromptChar="_"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"
        Margin="0,10,0,0"/>
</StackPanel>
```

## Best Practices

1. **Start simple:** Test basic patterns before adding complexity
2. **Use RegEx type:** More powerful and flexible than Simple masks
3. **Escape special characters:** Use `\` for literals like `.`, `(`, `)`
4. **Choose validation mode wisely:** KeyPress for strict validation, LostFocus for flexibility
5. **Provide visual cues:** Use PromptChar and ShowPromptOnFocus for better UX
6. **Test edge cases:** Empty input, partial input, invalid characters
7. **Document complex patterns:** Add comments explaining intricate RegEx masks
8. **Use lookaheads sparingly:** Can impact performance with very complex patterns
