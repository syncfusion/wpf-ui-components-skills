---
name: syncfusion-wpf-numericupdown
description: Learn to implement and configure the Syncfusion WPF NumericUpdown (UpDown) control for numeric value input. Use when creating numeric input fields with spin buttons, value restrictions, number formatting, and cultural support. Includes guidance for getting started, managing values, formatting numbers, styling, and advanced configurations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF NumericUpdown (UpDown)

## When to Use This Skill

Use this skill when you need to:
- Create numeric input fields with increment/decrement buttons
- Restrict values within minimum and maximum limits
- Format numbers with specific decimal places or grouping
- Apply localization and culture-specific formatting
- Customize appearance with colors based on value (positive, negative, zero)
- Implement keyboard and mouse interactions for numeric input
- Handle null values with watermark text

## Component Overview

The Syncfusion WPF NumericUpdown (UpDown) control displays and manages numeric values with built-in spin buttons for incrementing and decrementing. It provides features for value restrictions, number formatting, event handling, and extensive styling options.

**Key Components:**
- Text area: Displays the numeric value
- Increment button: Repeat button to increase value
- Decrement button: Repeat button to decrease value

## Documentation Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly references
- Adding UpDown via designer, XAML, and C# code
- Basic value and step properties
- Theme application and styling

### Value and Restrictions
📄 **Read:** [references/value-and-restrictions.md](references/value-and-restrictions.md)
- Setting and managing values
- Value change events (ValueChanged, ValueChanging)
- Minimum and maximum value constraints
- Validation modes and behaviors
- Null value handling and watermarks
- Controlling edit permissions

### Number Formatting
📄 **Read:** [references/number-formatting.md](references/number-formatting.md)
- Decimal digit formatting
- Group separator configuration
- NumberFormatInfo customization
- Culture and localization support
- Text alignment options

### Styling and Appearance
📄 **Read:** [references/styling-and-appearance.md](references/styling-and-appearance.md)
- Customizing positive, negative, and zero value colors
- Focused state styling
- Theme integration with SfSkinManager
- Custom theme creation with ThemeStudio

### Advanced Configuration
📄 **Read:** [references/advanced-configuration.md](references/advanced-configuration.md)
- Step interval management
- Keyboard and mouse interaction patterns
- Animation support
- Culture-specific behaviors
- Complex use cases and scenarios

## Quick Start Example

### Basic XAML Setup

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <!-- Simple UpDown with value and step -->
        <syncfusion:UpDown Name="upDown" 
                          Height="25" 
                          Width="100"
                          Value="10"
                          Step="5"
                          MinValue="0"
                          MaxValue="100" />
    </Grid>
</Window>
```

### Basic C# Setup

```csharp
// Create UpDown control
UpDown updown = new UpDown();
updown.Width = 100;
updown.Height = 25;
updown.Value = 10;
updown.Step = 5;
updown.MinValue = 0;
updown.MaxValue = 100;

// Add to container
grid.Children.Add(updown);
```

### Handling Value Changes

```csharp
updown.ValueChanged += (d, e) => 
{
    var newValue = e.NewValue;
    var oldValue = e.OldValue;
};

updown.ValueChanging += (s, e) => 
{
    // Can cancel the change
    if ((double)e.NewValue > 50)
        e.Cancel = true;
};
```

## Common Patterns

### Pattern 1: Validated Numeric Input with Restricted Range
When users need to validate input within a specific range (e.g., quantity 1-999):

```csharp
var updown = new UpDown();
updown.MinValue = 1;
updown.MaxValue = 999;
updown.MinValidation = MinValidation.OnKeyPress;
updown.MaxValidation = MaxValidation.OnKeyPress;
```

**When to use:** Product quantity, age validation, percentage fields.

### Pattern 2: Currency Input with Formatting
When users need formatted currency display with decimal places:

```csharp
updown.NumberDecimalDigits = 2;
updown.GroupSeperatorEnabled = true;
updown.Culture = new CultureInfo("en-US");
```

**When to use:** Price input, monetary values, financial calculations.

### Pattern 3: Value-Based Visual Feedback
When users need different colors for positive, negative, and zero values:

```csharp
updown.EnableNegativeColors = true;
updown.NegativeBackground = Brushes.LightCoral;
updown.NegativeForeground = Brushes.DarkRed;

updown.ApplyZeroColor = true;
updown.ZeroColor = Brushes.Gray;
```

**When to use:** Profit/loss display, balance sheets, variance analysis.

### Pattern 4: Null Value with Watermark
When users need to handle uninitialized or "no value" scenarios:

```csharp
updown.UseNullOption = true;
updown.Value = null;
updown.NullValueText = "Enter a value";
```

**When to use:** Optional numeric fields, initial empty states.

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| **Value** | double? | Current numeric value |
| **Step** | double | Increment/decrement interval |
| **MinValue** | double | Minimum allowed value |
| **MaxValue** | double | Maximum allowed value |
| **NumberDecimalDigits** | int | Decimal places to display |
| **GroupSeperatorEnabled** | bool | Show thousand separators |
| **Culture** | CultureInfo | Localization format |
| **AllowEdit** | bool | Allow direct text editing |
| **UseNullOption** | bool | Support null values |
| **NullValueText** | string | Watermark text when null |

## Common Use Cases

1. **Product Quantity Input**: Restrict to integers with min/max range
2. **Price Input**: Format with 2 decimal places and currency symbol
3. **Age Input**: Restrict to valid age range (18-120)
4. **Percentage Input**: Restrict to 0-100 with no decimals
5. **Temperature Control**: Allow negative values with culture-specific format
6. **Discount Calculator**: Show positive in green, negative in red
7. **Scoring System**: Restrict to specific range (1-5 for ratings)
8. **Time Duration**: Format with step intervals (15-minute increments)

---

**Next Steps:** Choose a reference file above based on your implementation needs, or read "Getting Started" for basic setup instructions.
