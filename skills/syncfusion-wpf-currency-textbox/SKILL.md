---
name: syncfusion-wpf-currency-textbox
description: Implements the Syncfusion WPF CurrencyTextBox control for formatted currency input with culture-specific formatting and validation. Use this when adding currency input fields, financial data entry controls, or decimal value inputs with currency symbols in WPF applications. Covers currency formatting, culture support, min/max validation, NumberFormatInfo customization, and value binding.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# WPF CurrencyTextBox -- Quick Reference

This skill documents core usage patterns and links to concise reference pages for formatting, validation, styling, and advanced features.

## Quick Start

Add the Syncfusion WPF package and drop a `CurrencyTextBox` in XAML:

```xml
<syncfusion:CurrencyTextBox Width="200" Value="1234.56" MinValue="0" MaxValue="10000" />
```

Bind in MVVM:

```xml
<syncfusion:CurrencyTextBox Value="{Binding Amount, UpdateSourceTrigger=PropertyChanged}" />
```

## References

- Getting started: [references/getting-started.md](references/getting-started.md)
- Formatting & culture: [references/formatting-culture.md](references/formatting-culture.md)
- Validation & restrictions: [references/validation-restrictions.md](references/validation-restrictions.md)
- Appearance & styling: [references/appearance-styling.md](references/appearance-styling.md)
- Advanced features: [references/advanced-features.md](references/advanced-features.md)
- Step interval & scrolling: [references/step-interval-scrolling.md](references/step-interval-scrolling.md)

## Notes

- Always use `Value` (not `Text`) to get/set the currency amount.
- Format priority: dedicated properties > NumberFormat > Culture.
- Range adorner requires both MinValue and MaxValue to be set.
- Watermark requires UseNullOption = true.
