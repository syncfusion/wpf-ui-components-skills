# Value Management — Advanced

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

```csharp
// Copy with formatting
phoneInput.ValueMaskFormat = MaskFormat.IncludeLiterals;
phoneInput.Focus();
phoneInput.SelectAll();
ApplicationCommands.Copy.Execute(null, phoneInput);
// Clipboard contains: "(455) 345-6789"

// Copy without formatting
phoneInput.ValueMaskFormat = MaskFormat.ExcludePromptAndLiterals;
phoneInput.SelectAll();
ApplicationCommands.Copy.Execute(null, phoneInput);
// Clipboard contains: "4553456789"
```

### Paste Operations

```csharp
// Paste phone number (with or without formatting)
Clipboard.SetText("4553456789");
phoneInput.Focus();
ApplicationCommands.Paste.Execute(null, phoneInput);
// Result: (455) 345-6789

// Paste with literals (they're stripped automatically)
Clipboard.SetText("(455) 345-6789");
phoneInput.Focus();
ApplicationCommands.Paste.Execute(null, phoneInput);
// Result: (455) 345-6789 (correctly parsed)
```

### Custom Copy/Paste

```csharp
// Custom copy button
private void CopyButton_Click(object sender, RoutedEventArgs e)
{
    if (!phoneInput.HasError && !string.IsNullOrWhiteSpace(phoneInput.Value))
    {
        Clipboard.SetText(phoneInput.Value);
        MessageBox.Show("Phone number copied!");
    }
}

// Custom paste button
private void PasteButton_Click(object sender, RoutedEventArgs e)
{
    if (Clipboard.ContainsText())
    {
        string text = Clipboard.GetText();
        phoneInput.Value = text;
    }
}
```

## Best Practices

1. **Set ValueMaskFormat early:** Configure before accessing `Value` to avoid confusion
2. **Use ExcludePromptAndLiterals for storage:** Clean data for databases and APIs
3. **Use IncludeLiterals for display:** Show formatted values to users
4. **Handle ValueChanged appropriately:** Consider ValidationMode when implementing event handlers
5. **Check HasError before using Value:** Ensure data is valid before processing
6. **Document format choice:** Comment why you chose a specific ValueMaskFormat
7. **Test partial input:** Verify behavior when user hasn't completed input
8. **Consider MVVM binding:** Use two-way binding for better architecture
