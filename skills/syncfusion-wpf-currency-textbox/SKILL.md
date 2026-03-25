---
name: syncfusion-wpf-currency-textbox
description: Implements the Syncfusion WPF CurrencyTextBox control for formatted currency input with culture-specific formatting. Use this when adding currency input fields, financial data entry controls, or decimal value inputs with currency symbols in WPF applications. Covers currency formatting, culture support, min/max validation, NumberFormatInfo customization, and value binding.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# WPF Currency TextBox

The Syncfusion WPF CurrencyTextBox is a specialized input control that restricts entry to decimal values and displays them in currency format. It provides comprehensive support for data binding, culture-specific formatting, value validation, watermarks, null values, and extensive customization options.

## When to Use This Skill

Use this skill when implementing WPF applications that require:

- **Currency input fields** for financial applications, invoicing, pricing, or e-commerce
- **Formatted decimal entry** with automatic currency symbols and separators
- **Culture-specific formatting** for international applications
- **Value validation** with minimum/maximum restrictions
- **Custom number formatting** with specific decimal digits, group separators, or currency symbols
- **Data-bound currency fields** in MVVM applications
- **Styled financial inputs** with foreground colors for positive/negative/zero values
- **Increment/decrement controls** using keyboard arrows, mouse wheel, or spin buttons
- **Read-only currency displays** with formatted values
- **Visual progress indicators** for currency values within a range

## Component Overview

**Key Capabilities:**
- Restricts input to decimal values only
- Automatic currency formatting with culture support
- Min/Max value validation with configurable validation modes
- Data binding support for MVVM patterns
- Custom formatting via Culture, NumberFormatInfo, or dedicated properties
- Step interval for keyboard/mouse wheel navigation
- Foreground colors for positive, negative, and zero values
- Watermark and null value support
- Range adorner for visual progress indication
- Spin buttons for value increment/decrement
- Paste mode with advanced insertion behavior

**Assembly:** `Syncfusion.Shared.WPF`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet package installation
- Adding CurrencyTextBox via designer, XAML, or C#
- Setting and binding the Value property
- Value changed notification and event handling
- Basic min/max value restrictions
- Introduction to step interval and culture support

### Formatting and Culture
📄 **Read:** [references/formatting-culture.md](references/formatting-culture.md)
- Culture-based formatting for international applications
- NumberFormatInfo configuration (separators, symbols, decimal digits)
- Dedicated formatting properties (CurrencySymbol, CurrencyGroupSeparator, etc.)
- Currency group sizes and patterns
- Positive and negative value patterns
- Format property precedence and priority rules

### Validation and Restrictions
📄 **Read:** [references/validation-restrictions.md](references/validation-restrictions.md)
- Min/Max value restrictions and validation modes
- OnKeyPress vs OnLostFocus validation
- MaxValueOnExceedMaxDigit and MinValueOnExceedMinDigit behavior
- Decimal digit restrictions (min/max decimal places)
- Read-only mode with caret visibility options

### Step Interval and Scrolling
📄 **Read:** [references/step-interval-scrolling.md](references/step-interval-scrolling.md)
- ScrollInterval property for increment/decrement amounts
- Arrow key navigation (up/down)
- Mouse wheel scrolling with IsScrollingOnCircle
- Click-and-drag scrolling with EnableExtendedScrolling
- Text selection on focus behavior

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)
- Foreground colors for positive, negative, and zero values
- Background customization
- Corner radius for rounded borders
- Selection brush and opacity
- Text alignment (left, center, right)
- Tooltip configuration

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Null values with UseNullOption and NullValue
- Watermark text, templates, and styling
- Paste mode (Default vs Advanced)
- Spin buttons (ShowSpinButton) for UpDown controls
- Range adorner for visual progress indicators
- Value changed event handling

## Quick Start Example

### XAML Implementation

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <StackPanel Margin="20">
        <!-- Basic currency input -->
        <syncfusion:CurrencyTextBox 
            x:Name="currencyTextBox" 
            Width="200" 
            Height="30"
            Value="1234.56" />
        
        <!-- With min/max validation -->
        <syncfusion:CurrencyTextBox 
            Width="200" 
            Height="30"
            Value="500"
            MinValue="0"
            MaxValue="10000"
            ScrollInterval="50" />
    </StackPanel>
</Window>
```

### C# Implementation

```csharp
using Syncfusion.Windows.Shared;

// Create instance
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 200;
currencyTextBox.Height = 30;

// Set value
currencyTextBox.Value = 1234.56;

// Configure validation
currencyTextBox.MinValue = 0;
currencyTextBox.MaxValue = 10000;

// Set culture for formatting
currencyTextBox.Culture = new System.Globalization.CultureInfo("en-US");

// Handle value changes
currencyTextBox.ValueChanged += (d, e) =>
{
    var oldValue = e.OldValue;
    var newValue = e.NewValue;
};

// Add to layout
this.Content = currencyTextBox;
```

## Common Patterns

### Basic Currency Input with Validation

```xml
<syncfusion:CurrencyTextBox 
    x:Name="priceInput"
    Width="150" 
    Height="30"
    Value="99.99"
    MinValue="0"
    MaxValue="9999.99"
    ScrollInterval="10"
    ToolTip="Enter product price" />
```

### Formatted Currency with Culture

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 1234567.89;
currencyTextBox.Culture = new CultureInfo("fr-FR"); // French formatting
// Result: 1 234 567,89 €
```

### Custom Currency Symbol and Formatting

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Value = 123456.789;
currencyTextBox.CurrencySymbol = "£";
currencyTextBox.CurrencyDecimalDigits = 2;
currencyTextBox.CurrencyGroupSeparator = ",";
currencyTextBox.CurrencyDecimalSeparator = ".";
currencyTextBox.GroupSeperatorEnabled = true;
// Result: £123,456.79
```

### Data Binding with MVVM

**XAML:**
```xml
<syncfusion:CurrencyTextBox 
    Value="{Binding Price, UpdateSourceTrigger=PropertyChanged}"
    MinValue="0"
    MaxValue="100000"
    Width="150" 
    Height="30" />
```

**ViewModel:**
```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private double _price;
    public double Price
    {
        get => _price;
        set
        {
            _price = value;
            OnPropertyChanged(nameof(Price));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Styled with Color-Coded Values

```xml
<syncfusion:CurrencyTextBox 
    x:Name="profitLoss"
    Width="150" 
    Height="30"
    Value="-250.00"
    PositiveForeground="Green"
    NegativeForeground="Red"
    ApplyNegativeForeground="True"
    ZeroColor="Gray"
    ApplyZeroColor="True" />
```

### Currency Input with Spin Buttons

```xml
<syncfusion:CurrencyTextBox 
    Width="150" 
    Height="30"
    Value="100"
    MinValue="0"
    MaxValue="1000"
    ScrollInterval="25"
    ShowSpinButton="True" />
```

### With Watermark for Empty State

```xml
<syncfusion:CurrencyTextBox 
    Width="150" 
    Height="30"
    UseNullOption="True"
    WatermarkText="Enter amount"
    WatermarkTextIsVisible="True"
    WatermarkTextForeground="Gray" />
```

## Key Properties Reference

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| `Value` | `double` | Gets or sets the currency value. Use this instead of `Text` property. |
| `MinValue` | `double` | Minimum allowed value. |
| `MaxValue` | `double` | Maximum allowed value. |
| `Culture` | `CultureInfo` | Culture for formatting decimal and group separators. |
| `ScrollInterval` | `double` | Increment/decrement amount for arrow keys and mouse wheel. Default: 1. |

### Formatting Properties

| Property | Type | Description |
|----------|------|-------------|
| `CurrencySymbol` | `string` | Currency symbol to display (e.g., "$", "€", "£"). |
| `CurrencyDecimalDigits` | `int` | Number of decimal places. Default: -1 (culture default). |
| `CurrencyDecimalSeparator` | `string` | Decimal separator character (e.g., ".", ","). |
| `CurrencyGroupSeparator` | `string` | Group separator for thousands (e.g., ",", " "). |
| `CurrencyGroupSizes` | `Int32Collection` | Sizes of digit groups (e.g., {3} for thousands). |
| `GroupSeperatorEnabled` | `bool` | Enables display of group separator. |
| `NumberFormat` | `NumberFormatInfo` | Complete number format configuration object. |
| `CurrencyPositivePattern` | `int` | Pattern for positive values (0-3). See formatting reference. |
| `CurrencyNegativePattern` | `int` | Pattern for negative values (0-15). See formatting reference. |

### Validation Properties

| Property | Type | Description |
|----------|------|-------------|
| `MinValidation` | `MinValidation` | When to validate minimum: `OnKeyPress` or `OnLostFocus`. |
| `MaxValidation` | `MaxValidation` | When to validate maximum: `OnKeyPress` or `OnLostFocus`. |
| `MaxValueOnExceedMaxDigit` | `bool` | Set to MaxValue when input exceeds maximum. |
| `MinValueOnExceedMinDigit` | `bool` | Set to MinValue when input is below minimum. |
| `MinimumCurrencyDecimalDigits` | `int` | Minimum decimal places to display. |
| `MaximumCurrencyDecimalDigits` | `int` | Maximum decimal places allowed. |
| `IsReadOnly` | `bool` | Prevents user input when true. |
| `IsReadOnlyCaretVisible` | `bool` | Shows caret in read-only mode. |

### Appearance Properties

| Property | Type | Description |
|----------|------|-------------|
| `PositiveForeground` | `Brush` | Foreground color for positive values. Default: Black. |
| `NegativeForeground` | `Brush` | Foreground color for negative values. Default: Red. |
| `ApplyNegativeForeground` | `bool` | Enables negative foreground color. |
| `ZeroColor` | `Brush` | Foreground color when value is zero. Default: Green. |
| `ApplyZeroColor` | `bool` | Enables zero color. |
| `Background` | `Brush` | Background color of the control. |
| `CornerRadius` | `CornerRadius` | Corner radius for rounded borders. Default: 1. |
| `TextAlignment` | `TextAlignment` | Text alignment: Left, Center, or Right. |
| `SelectionBrush` | `Brush` | Background color for selected text. |
| `SelectionOpacity` | `double` | Opacity of selection brush. |

### Scrolling Properties

| Property | Type | Description |
|----------|------|-------------|
| `IsScrollingOnCircle` | `bool` | Enables mouse wheel scrolling. Default: true. |
| `EnableExtendedScrolling` | `bool` | Enables click-and-drag scrolling. |
| `TextSelectionOnFocus` | `bool` | Auto-selects text when control receives focus. Default: true. |

### Advanced Properties

| Property | Type | Description |
|----------|------|-------------|
| `UseNullOption` | `bool` | Enables null value and watermark support. |
| `NullValue` | `double?` | Value to display when control is null. |
| `WatermarkText` | `string` | Text to display when value is null/empty. |
| `WatermarkTextIsVisible` | `bool` | Shows/hides watermark. |
| `WatermarkTextForeground` | `Brush` | Foreground color for watermark. |
| `WatermarkTemplate` | `DataTemplate` | Custom template for watermark appearance. |
| `PasteMode` | `PasteMode` | Paste behavior: `Default` or `Advanced`. |
| `ShowSpinButton` | `bool` | Shows UpDown spin buttons. |
| `EnableRangeAdorner` | `bool` | Shows visual progress indicator based on min/max. |
| `RangeAdornerBackground` | `Brush` | Background color of range adorner. |

### Events

| Event | Description |
|-------|-------------|
| `ValueChanged` | Raised when the Value property changes. Provides OldValue and NewValue. |

## Common Use Cases

1. **E-commerce Product Pricing** - Currency input with validation for product prices
2. **Financial Calculators** - Loan amounts, interest rates, payment calculations
3. **Invoicing Systems** - Line items, subtotals, tax amounts, totals
4. **Budgeting Applications** - Expense tracking, budget allocation, balance display
5. **Point of Sale Systems** - Transaction amounts, discounts, change calculation
6. **International Applications** - Multi-currency support with culture-specific formatting
7. **Accounting Software** - General ledger entries, balance sheets, financial reports
8. **Payroll Systems** - Salary entry, deductions, net pay calculation
9. **Inventory Management** - Cost tracking, pricing, valuation
10. **Donation Forms** - Contribution amounts with preset increments

## Notes

- **Always use `Value` property**, not `Text`, to get/set the currency amount
- **Format priority**: Dedicated properties > NumberFormat > Culture
- **Validation modes**: OnKeyPress prevents invalid input; OnLostFocus validates after entry
- **Range adorner** only displays when both MinValue and MaxValue are set
- **Watermark** only appears when UseNullOption is true and value is null/empty
