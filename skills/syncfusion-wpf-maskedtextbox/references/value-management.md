# Value Management

This guide covers setting and retrieving values from SfMaskedEdit with different formatting options.

<!-- Table of contents removed for cross-environment compatibility; see section headings below -->

## Setting the Value Property

The `Value` property holds the current input of the SfMaskedEdit control. When you set a value, it's automatically formatted according to the mask.

### Basic Value Assignment

```xml
<syncfusion:SfMaskedEdit 
    Value="4553456789"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"
    x:Name="phoneInput"/>
```

```csharp
SfMaskedEdit phoneInput = new SfMaskedEdit();
phoneInput.Value = "4553456789";
phoneInput.MaskType = MaskType.RegEx;
phoneInput.Mask = @"\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}";
```

**Result:**
- **Input:** `"4553456789"`
- **Displayed:** `(455) 345-6789`
- **Value property:** Contains formatted text based on `ValueMaskFormat`

### Setting Value After Initialization

Set the `Value` property programmatically to update the control; the control reformats the value according to the mask (see examples above).

### Data Binding in MVVM

```xml
<syncfusion:SfMaskedEdit 
    Value="{Binding PhoneNumber, Mode=TwoWay}"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"/>
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private string _phoneNumber;
    public string PhoneNumber
    {
        get => _phoneNumber;
        set
        {
            _phoneNumber = value;
            OnPropertyChanged(nameof(PhoneNumber));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Value Formatting Options

The `ValueMaskFormat` property controls what the `Value` property contains:
- User-entered characters only
- User-entered characters + literals
- User-entered characters + prompt characters
- User-entered characters + both literals and prompts

**Default:** `IncludePromptAndLiterals`

### ValueMaskFormat Enum Values

| Value | Description |
|-------|-------------|
| `ExcludePromptAndLiterals` | Only user-entered characters |
| `IncludeLiterals` | User characters + literals (no prompts) |
| `IncludePrompt` | User characters + prompts (no literals) |
| `IncludePromptAndLiterals` | User characters + literals + prompts (default) |

## ExcludePromptAndLiterals Format

Returns only the characters the user actually typed, excluding prompts and literals.

### Example: Phone Number

```xml
<syncfusion:SfMaskedEdit 
    ValueMaskFormat="ExcludePromptAndLiterals"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"
    Value="4553456789"
    x:Name="phoneInput"/>
```

```csharp
SfMaskedEdit phoneInput = new SfMaskedEdit();
phoneInput.ValueMaskFormat = MaskFormat.ExcludePromptAndLiterals;
phoneInput.MaskType = MaskType.RegEx;
phoneInput.Mask = @"\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}";
phoneInput.Value = "4553456789";

// Display: (455) 345-6789
// Value property: "4553456789"
```

**What's included:**
- ✅ User-entered digits: `4553456789`
- ❌ Parentheses: `(` `)`
- ❌ Spaces
- ❌ Hyphen: `-`

### Use Cases

**Database Storage:**
```csharp
// Store clean data without formatting
private void SavePhone()
{
    phoneInput.ValueMaskFormat = MaskFormat.ExcludePromptAndLiterals;
    string cleanPhone = phoneInput.Value; // "4553456789"
    
    // Save to database without literals
    database.SavePhoneNumber(cleanPhone);
}
```

**API Submission:**
```csharp
// Send only digits to API
var request = new PhoneVerificationRequest
{
    PhoneNumber = phoneInput.Value // "4553456789" without formatting
};
```

## IncludeLiterals Format

Returns user-entered characters plus literal characters defined in the mask. Excludes prompt characters for missing input.

### Example: Phone Number

```xml
<syncfusion:SfMaskedEdit 
    ValueMaskFormat="IncludeLiterals"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"
    Value="4553456789"
    x:Name="phoneInput"/>
```

```csharp
phoneInput.ValueMaskFormat = MaskFormat.IncludeLiterals;

// Display: (455) 345-6789
// Value property: "(455) 345-6789"
```

**What's included:**
- ✅ User-entered digits: `4553456789`
- ✅ Parentheses: `(` `)`
- ✅ Spaces
- ✅ Hyphen: `-`
- ❌ Prompt characters (if input incomplete)

### Example: Partial Input

```csharp
// User enters: 455
// Display: (455) ___-____
// Value with IncludeLiterals: "(455) " (literals included, no prompts)
```

### Use Cases

**Display Formatted Value:**
```csharp
// Show formatted phone in UI
phoneInput.ValueMaskFormat = MaskFormat.IncludeLiterals;
string displayPhone = phoneInput.Value; // "(455) 345-6789"
messageTextBlock.Text = $"Your phone: {displayPhone}";
```

**Copy to Clipboard:**
```csharp
// Copy formatted value
Clipboard.SetText(phoneInput.Value); // "(455) 345-6789"
```

## IncludePrompt Format

Returns user-entered characters plus prompt characters for missing input. Excludes literals.

### Example: Phone Number

```xml
<syncfusion:SfMaskedEdit 
    ValueMaskFormat="IncludePrompt"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"
    PromptChar="_"
    x:Name="phoneInput"/>
```

```csharp
phoneInput.ValueMaskFormat = MaskFormat.IncludePrompt;
phoneInput.PromptChar = '_';

// User enters: 455
// Display: (455) ___-____
// Value property: "455______" (no literals, includes prompts)
```

**What's included:**
- ✅ User-entered digits: `455`
- ✅ Prompt characters: `______` (for missing positions)
- ❌ Parentheses: `(` `)`
- ❌ Spaces
- ❌ Hyphen: `-`

### Example: Full Input

```csharp
// User enters: 4553456789
// Display: (455) 345-6789
// Value with IncludePrompt: "4553456789" (no literals, no prompts needed)
```

### Use Cases

**Check Completeness:**
```csharp
// Verify if all positions filled
phoneInput.ValueMaskFormat = MaskFormat.IncludePrompt;
bool isComplete = !phoneInput.Value.Contains('_');

if (isComplete)
{
    // Process complete input
}
```

## IncludePromptAndLiterals Format

Returns everything: user-entered characters, literals, and prompt characters. This is the **default** format.

### Example: Phone Number

```xml
<syncfusion:SfMaskedEdit 
    ValueMaskFormat="IncludePromptAndLiterals"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"
    PromptChar="_"
    x:Name="phoneInput"/>
```

```csharp
phoneInput.ValueMaskFormat = MaskFormat.IncludePromptAndLiterals; // Default

// User enters: 455
// Display: (455) ___-____
// Value property: "(455) ___-____" (everything included)
```

**What's included:**
- ✅ User-entered digits: `455`
- ✅ Parentheses: `(` `)`
- ✅ Spaces
- ✅ Hyphen: `-`
- ✅ Prompt characters: `___` (for missing positions)

### Example: Full Input

```csharp
// User enters: 4553456789
// Display: (455) 345-6789
// Value with IncludePromptAndLiterals: "(455) 345-6789"
```

### Use Cases

**Exact Display Representation:**
```csharp
// Get exactly what's shown in the control
string exactDisplay = phoneInput.Value; // "(455) ___-____"
```

## Format Comparison Table

Given mask: `\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}` and input: `455`

| Format | Value Property | Use Case |
|--------|----------------|----------|
| ExcludePromptAndLiterals | `"455"` | Database storage, API calls |
| IncludeLiterals | `"(455) "` | Display formatting, partial input |
| IncludePrompt | `"455______"` | Completeness checking |
| IncludePromptAndLiterals | `"(455) ___-____"` | Exact display representation |

Given complete input: `4553456789`

| Format | Value Property |
|--------|----------------|
| ExcludePromptAndLiterals | `"4553456789"` |
| IncludeLiterals | `"(455) 345-6789"` |
| IncludePrompt | `"4553456789"` |
| IncludePromptAndLiterals | `"(455) 345-6789"` |

## Value Change Notifications

Subscribe to the `ValueChanged` event to respond when the value changes.

### Basic Event Subscription

```xml
<syncfusion:SfMaskedEdit 
    ValueChanged="SfMaskedEdit_ValueChanged"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"
    x:Name="phoneInput"/>
```

```csharp
private void SfMaskedEdit_ValueChanged(object sender, EventArgs e)
{
    var maskedEdit = sender as SfMaskedEdit;
    string currentValue = maskedEdit.Value;
    
    MessageBox.Show($"Value changed to: {currentValue}");
}
```

### When Does ValueChanged Fire?

The timing depends on the `ValidationMode` property:

#### With KeyPress Validation (Default)

```xml
<syncfusion:SfMaskedEdit 
    ValidationMode="KeyPress"
    ValueChanged="OnValueChanged"/>
```

- Fires after **each valid keystroke**
- Value is updated immediately
- Invalid keystrokes don't trigger the event

#### With LostFocus Validation

```xml
<syncfusion:SfMaskedEdit 
    ValidationMode="LostFocus"
    ValueChanged="OnValueChanged"/>
```

- Fires when **control loses focus**
- Only fires if validation succeeds
- Value is updated only after focus loss

### Example: Real-time Validation

```xml
<StackPanel>
    <syncfusion:SfMaskedEdit 
        x:Name="phoneInput"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"
        ValueMaskFormat="ExcludePromptAndLiterals"
        ValueChanged="PhoneInput_ValueChanged"/>
    
    <TextBlock x:Name="statusText" Margin="0,5,0,0"/>
</StackPanel>
```

```csharp
private void PhoneInput_ValueChanged(object sender, EventArgs e)
{
    var maskedEdit = sender as SfMaskedEdit;
    
    if (!maskedEdit.HasError && maskedEdit.Value.Length == 10)
    {
        statusText.Text = "✓ Valid phone number";
        statusText.Foreground = Brushes.Green;
    }
    else
    {
        statusText.Text = "Enter 10 digits";
        statusText.Foreground = Brushes.Gray;
    }
}
```

### Example: Sync Two Controls

```xml
<StackPanel>
    <TextBlock Text="Enter phone number:"/>
    <syncfusion:SfMaskedEdit 
        x:Name="phoneInput"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"
        ValueMaskFormat="IncludeLiterals"
        ValueChanged="PhoneInput_ValueChanged"/>
    
    <TextBlock Text="Formatted preview:" Margin="0,10,0,0"/>
    <TextBox x:Name="previewText" IsReadOnly="True"/>
</StackPanel>
```

```csharp
private void PhoneInput_ValueChanged(object sender, EventArgs e)
{
    previewText.Text = phoneInput.Value;
}
```

### Example: Multi-field Form Validation

```xml
<StackPanel>
    <syncfusion:SfMaskedEdit 
        x:Name="phoneInput"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"
        ValueChanged="ValidateForm"/>
    
    <syncfusion:SfMaskedEdit 
        x:Name="zipInput"
        Mask="\d{5}-\d{4}"
        MaskType="RegEx"
        ValueChanged="ValidateForm"
        Margin="0,10,0,0"/>
    
    <Button x:Name="submitButton" Content="Submit" IsEnabled="False" Margin="0,10,0,0"/>
</StackPanel>
```

```csharp
private void ValidateForm(object sender, EventArgs e)
{
    bool phoneValid = !phoneInput.HasError && !string.IsNullOrWhiteSpace(phoneInput.Value);
    bool zipValid = !zipInput.HasError && !string.IsNullOrWhiteSpace(zipInput.Value);
    
    submitButton.IsEnabled = phoneValid && zipValid;
}
```

## Clipboard Operations

### Copy Formatted Value

The clipboard behavior is affected by `ValueMaskFormat`:
<!-- Advanced value-change and clipboard examples moved to value-management-advanced.md -->
```csharp
