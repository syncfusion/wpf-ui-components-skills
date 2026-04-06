---
name: syncfusion-wpf-maskedtextbox
description: Comprehensive guide for implementing Syncfusion WPF MaskedTextBox (SfMaskedEdit) control for restricted input with mask patterns. Use this skill when implementing masked input, input masks, or input restrictions with predefined patterns in WPF. Covers phone number input, email validation, credit card input, formatted input with prompt characters, RegEx masks, and custom patterns for dates, currency, product keys, and zip codes.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"


# Implementing Syncfusion WPF MaskedTextBox (SfMaskedEdit)

The WPF MaskedTextBox (SfMaskedEdit) is an advanced input control that restricts user input to specific types using mask patterns. It's ideal for creating templates for phone numbers, IP addresses, product IDs, dates, and other formatted data entry.

## When to Use This Skill

Use this skill when you need to:
- **Restrict input to specific formats** without custom validation
- **Implement formatted input fields** for phone numbers, emails, dates, currency
- **Create mask patterns** using Simple or RegEx mask types
- **Validate user input** on key press or lost focus
- **Format values** with or without literals and prompt characters
- **Display error indicators** for invalid input
- **Add watermarks** to guide user input
- **Customize appearance** including colors, borders, and themes
- **Support RTL layouts** for international applications
- **Handle value changes** with event notifications

## Component Overview

**SfMaskedEdit** provides:
- **Multiple mask types:** Simple and RegEx patterns
- **Flexible validation:** KeyPress or LostFocus validation modes
- **Value formatting:** Include/exclude prompts and literals
- **Visual feedback:** Error borders, prompt characters, watermarks
- **Full customization:** Colors, themes, templates
- **MVVM support:** Data binding and property change notifications

**Assembly:** `Syncfusion.SfInput.WPF`, `Syncfusion.SfShared.WPF`  
**Namespace:** `Syncfusion.Windows.Controls.Input`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet installation
- Adding SfMaskedEdit via designer, XAML, and C#
- Basic configuration and setup
- First masked textbox example
- Handling ValueChanged events

### Mask Patterns and Input Restriction
📄 **Read:** [references/mask-patterns-input-restriction.md](references/mask-patterns-input-restriction.md), [references/mask-patterns-advanced.md](references/mask-patterns-advanced.md)
- Mask element reference ([], \d, \w, {n}, +, *, ?, etc.)
- Creating custom mask patterns
- Allow/restrict specific values with (?=) and (?!)
- Extensive mask examples (phone, email, currency, dates, etc.)
- Input validation modes (KeyPress vs LostFocus)
- Validation results and HasError property
- PromptChar and ShowPromptOnFocus configuration

### Value Management
📄 **Read:** [references/value-management.md](references/value-management.md), [references/value-management-advanced.md](references/value-management-advanced.md)
- Setting and getting Value property
- ValueMaskFormat options:
  - ExcludePromptAndLiterals
  - IncludeLiterals
  - IncludePrompt
  - IncludePromptAndLiterals
- Value change notifications
- Clipboard operations with formatted values

### Appearance and Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md), [references/appearance-customization-themes.md](references/appearance-customization-themes.md)
- Background, Foreground, and SelectionBrush
- CaretBrush and BorderBrush customization
- ErrorBorderBrush for validation feedback
- FlowDirection for RTL support
- Watermark and WatermarkTemplate
- Theme integration and styling

## Quick Start Example

### Phone Number Mask

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Grid>
        <syncfusion:SfMaskedEdit 
            x:Name="phoneNumber"
            Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
            MaskType="RegEx"
            Value="4553456789"
            PromptChar="_"
            Width="200" 
            Height="30"
            ValueChanged="PhoneNumber_ValueChanged"/>
    </Grid>
</Window>
```

```csharp
using Syncfusion.Windows.Controls.Input;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }

    private void PhoneNumber_ValueChanged(object sender, EventArgs e)
    {
        var maskedEdit = sender as SfMaskedEdit;
        if (!maskedEdit.HasError)
        {
            // Use formatted value: (455) 345-6789
            string formattedPhone = maskedEdit.Value;
        }
    }
}
```

## Common Patterns

Key patterns include Email, Currency, Date, Product Key, and Value Formatting. See [Mask Patterns and Input Restriction](references/mask-patterns-input-restriction.md) for 25+ detailed pattern examples with complete XAML and code samples.

## Key Properties

Essential properties include **Mask** (pattern), **MaskType** (Simple/RegEx), **Value** (formatted output), **ValueMaskFormat** (format control), **ValidationMode** (KeyPress/LostFocus), **HasError** (validation status), **PromptChar** (missing input indicator), **ErrorBorderBrush** (error feedback), and **Watermark** (placeholder text). See [Getting Started](references/getting-started.md) and [Mask Patterns](references/mask-patterns-input-restriction.md) for property details and 25+ mask pattern examples.

## Common Use Cases

- **Data Entry Forms:** Contact info, financial data, ID numbers with automatic formatting
- **Phone/Finance:** Consistent formatting across app, currency/percent enforcement
- **Product Registration:** Validate product keys and serial numbers
- **Date/Time:** Custom formats beyond DatePicker with RegEx validation
- **International:** RTL support via FlowDirection, regional date/phone formats
- **Real-time Validation:** KeyPress for instant feedback or LostFocus for less intrusive

## Events

| Event | Description |
|-------|-------------|
| **ValueChanged** | Fires when Value property changes (based on ValidationMode) |
| **LostFocus** | Standard WPF event, useful with LostFocus validation mode |

## Best Practices

1. **Choose appropriate MaskType:** Use RegEx for complex patterns, Simple for basic scenarios
2. **Set ValueMaskFormat early:** Determine format before accessing Value property
3. **Provide visual feedback:** Use ErrorBorderBrush and Watermark for better UX
4. **Match validation mode to UX:** KeyPress for instant feedback, LostFocus for less intrusive validation
5. **Document custom patterns:** Comment complex RegEx masks for maintainability
6. **Test edge cases:** Verify behavior with empty input, partial input, and boundary values
7. **Use ShowPromptOnFocus:** Enable for better visibility of expected input format

---

**Next Steps:** Choose a reference file above based on your needs, or start with getting-started.md for initial setup.
