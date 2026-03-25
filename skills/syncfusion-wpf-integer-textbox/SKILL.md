---
name: syncfusion-wpf-integer-textbox
description: Comprehensive guide for implementing Syncfusion WPF IntegerTextBox control for integer value input with validation, formatting, and customization. Use this skill when implementing integer input controls, numeric validation for integers, or integer value entry in WPF. Covers integer data entry forms, min/max value restrictions, number formatting with culture support, integer value scrolling, range adorner progress visualization, and integer-specific input control implementation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing IntegerTextBox

Comprehensive guide for implementing the Syncfusion® WPF IntegerTextBox control in Windows Presentation Foundation applications. The IntegerTextBox restricts text box input to only integer values with support for data binding, watermark, null values, validation, and extensive customization options.

## When to Use This Skill

Use this skill when you need to:

- **Implement integer-only input controls** in WPF applications
- **Restrict user input to integer values** with validation
- **Set up min/max value constraints** for integer inputs
- **Configure number formatting** with culture and grouping support
- **Customize visual appearance** with foreground colors for positive/negative/zero values
- **Add interactive value adjustment** via arrow keys, mouse wheel, or spin buttons
- **Implement range adorner** for visual progress indication
- **Handle null values and watermarks** in integer input fields
- **Create data-bound integer inputs** with MVVM pattern support
- **Validate integer input** with multiple validation modes

This skill covers the complete IntegerTextBox control with Int64 support, providing up to 19-digit integer value handling.

## Component Overview

The IntegerTextBox control is a specialized numeric input control from Syncfusion's WPF library that provides:

- **Int64 data type support** (up to 19 digits)
- **Min/Max value validation** with multiple validation modes
- **Culture-based number formatting** with customizable separators
- **Visual customization** including foreground colors for different value states
- **Interactive value adjustment** via keyboard, mouse wheel, and spin buttons
- **Range adorner** for visual progress bar-like indication
- **Watermark support** for empty state guidance
- **Null value handling** with configurable display
- **MVVM-ready** with full data binding support
- **Built-in themes** and styling options

**Assembly Required:** `Syncfusion.Shared.WPF`  
**Namespace:** `Syncfusion.Windows.Shared`

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this first for initial setup and basic implementation:
- Assembly deployment and NuGet package installation
- Adding IntegerTextBox via designer, XAML, or C#
- Basic property configuration
- Data binding setup with ViewModel pattern
- Initial min/max value restrictions
- Basic formatting examples

### Value Management

📄 **Read:** [references/value-management.md](references/value-management.md)

For managing integer values and user interactions:
- Value property usage and binding
- Paste mode configuration (Default vs Advanced)
- Spin button implementation for increment/decrement
- ValueChanged event handling
- Null value display with NullValue property
- Watermark text configuration and customization
- Watermark templates for visual enhancement

### Validation and Restrictions

📄 **Read:** [references/validation-restrictions.md](references/validation-restrictions.md)

For input validation and value constraints:
- MinValue and MaxValue property configuration
- Validation modes (OnKeyPress vs OnLostFocus)
- MaxValueOnExceedMaxDigit and MinValueOnExceedMinDigit behavior
- Read-only mode with caret visibility
- InvalidValueBehavior options (DisplayErrorMessage, ResetValue, None)
- ValidationValue property for custom validation
- Complete validation workflow examples

### Formatting and Culture

📄 **Read:** [references/formatting-culture.md](references/formatting-culture.md)

For number formatting and internationalization:
- Culture property for globalization support
- NumberFormat with NumberFormatInfo customization
- NumberGroupSeparator configuration
- NumberGroupSizes for custom grouping patterns
- GroupSeperatorEnabled property
- Priority rules (dedicated properties vs NumberFormat vs Culture)
- Multiple culture examples (US, Latin, custom)

### Appearance and Customization

📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

For visual styling and appearance control:
- PositiveForeground, NegativeForeground, and ZeroColor properties
- ApplyNegativeForeground and ApplyZeroColor configuration
- Background and CornerRadius customization
- SelectionBrush and SelectionOpacity for text selection
- TextAlignment options (Left, Center, Right)
- ToolTip configuration
- Complete visual customization examples

### Step Interval and Scrolling

📄 **Read:** [references/step-interval-scrolling.md](references/step-interval-scrolling.md)

For interactive value adjustment features:
- ScrollInterval property for increment/decrement steps
- Up/Down arrow key behavior
- Mouse wheel scrolling with IsScrollingOnCircle
- Click and drag scrolling with EnableExtendedScrolling
- TextSelectionOnFocus for auto-selection behavior
- Complete interactive input examples

### Range Adorner

📄 **Read:** [references/range-adorner.md](references/range-adorner.md)

For visual progress indication:
- EnableRangeAdorner property for progress bar-like display
- RangeAdornerBackground for custom adorner colors
- Value visualization between min/max range
- Use cases for visual feedback

## Quick Start Example

### Basic XAML Implementation

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="IntegerTextBoxSample.MainWindow"
        Title="IntegerTextBox Sample" Height="350" Width="525">
    <StackPanel Margin="20">
        <!-- Basic IntegerTextBox -->
        <syncfusion:IntegerTextBox x:Name="integerTextBox" 
                                   Width="200" 
                                   Height="30"
                                   Value="100"
                                   MinValue="0"
                                   MaxValue="1000"
                                   VerticalAlignment="Center"/>
    </StackPanel>
</Window>
```

### Basic C# Implementation

```csharp
using Syncfusion.Windows.Shared;
using System.Windows;

namespace IntegerTextBoxSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create IntegerTextBox
            IntegerTextBox integerTextBox = new IntegerTextBox();
            integerTextBox.Width = 200;
            integerTextBox.Height = 30;
            integerTextBox.Value = 100;
            integerTextBox.MinValue = 0;
            integerTextBox.MaxValue = 1000;
            
            // Add to layout
            this.Content = integerTextBox;
        }
    }
}
```

## Common Patterns

### MVVM Data Binding Pattern

Bind IntegerTextBox to ViewModel properties for reactive UI:

```xml
<syncfusion:IntegerTextBox Value="{Binding Quantity, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                           MinValue="1"
                           MaxValue="999"
                           Width="150"
                           Height="30"/>
```

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class ProductViewModel : INotifyPropertyChanged
{
    private int quantity;
    
    public int Quantity
    {
        get => quantity;
        set
        {
            if (quantity != value)
            {
                quantity = value;
                OnPropertyChanged();
                // Trigger calculations or validation
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Formatted Input with Culture

Configure number formatting with group separators:

```xml
<syncfusion:IntegerTextBox Value="123456789"
                           Culture="en-US"
                           GroupSeperatorEnabled="True"
                           NumberGroupSeparator=","
                           Width="200"
                           Height="30"/>
```

### Interactive Spinner Pattern

Enable spin buttons for easy value adjustment:

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           ScrollInterval="5"
                           ShowSpinButton="True"
                           IsScrollingOnCircle="True"
                           Width="150"
                           Height="30"/>
```

### Range Adorner Visualization

Show visual progress indication based on value:

```xml
<syncfusion:IntegerTextBox Value="60"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightBlue"
                           Width="200"
                           Height="30"/>
```

### Validation with Color Feedback

Visual feedback for positive, negative, and zero values:

```xml
<syncfusion:IntegerTextBox Value="-50"
                           MinValue="-100"
                           MaxValue="100"
                           PositiveForeground="Green"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"
                           ZeroColor="Gray"
                           ApplyZeroColor="True"
                           Width="150"
                           Height="30"/>
```

## Key Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| **Value** | int? | 0 | Current integer value of the control |
| **MinValue** | int | int.MinValue | Minimum allowed value |
| **MaxValue** | int | int.MaxValue | Maximum allowed value |
| **ScrollInterval** | int | 1 | Increment/decrement step value |
| **ShowSpinButton** | bool | false | Show up/down spinner buttons |
| **EnableRangeAdorner** | bool | false | Show progress bar-like visual |
| **GroupSeperatorEnabled** | bool | false | Enable number group separators |
| **NumberGroupSeparator** | string | Culture-based | Custom group separator character |
| **Culture** | CultureInfo | CurrentCulture | Culture for number formatting |
| **UseNullOption** | bool | false | Allow null values |
| **NullValue** | int? | null | Value to display when null |
| **WatermarkText** | string | string.Empty | Placeholder text when empty |
| **PositiveForeground** | Brush | Black | Foreground for positive values |
| **NegativeForeground** | Brush | Red | Foreground for negative values |
| **ZeroColor** | Brush | Green | Foreground for zero value |
| **IsReadOnly** | bool | false | Prevent user input |
| **TextAlignment** | TextAlignment | Left | Horizontal text alignment |
| **CornerRadius** | CornerRadius | 1 | Border corner rounding |

## Common Use Cases

### Form Input with Validation

Use IntegerTextBox for forms requiring integer input with validation:

```xml
<StackPanel>
    <TextBlock Text="Age:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="{Binding Age}"
                               MinValue="0"
                               MaxValue="150"
                               MinValidation="OnKeyPress"
                               MaxValidation="OnLostFocus"
                               Width="150"
                               Height="30"/>
</StackPanel>
```

### Quantity Selectors

Perfect for e-commerce or inventory quantity selection:

```xml
<syncfusion:IntegerTextBox Value="{Binding OrderQuantity}"
                           MinValue="1"
                           MaxValue="999"
                           ScrollInterval="1"
                           ShowSpinButton="True"
                           Width="100"
                           Height="30"/>
```

### Settings and Configuration

Use for numeric settings with visual feedback:

```xml
<syncfusion:IntegerTextBox Value="{Binding TimeoutSeconds}"
                           MinValue="0"
                           MaxValue="3600"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightGreen"
                           WatermarkText="Enter timeout (seconds)"
                           Width="200"
                           Height="30"/>
```

### Financial Applications

Handle integer amounts with proper formatting:

```xml
<syncfusion:IntegerTextBox Value="{Binding Amount}"
                           Culture="en-US"
                           GroupSeperatorEnabled="True"
                           MinValue="0"
                           MinValidation="OnKeyPress"
                           Width="200"
                           Height="30"/>
```

### Progress Input

Allow users to input progress values with visual indication:

```xml
<syncfusion:IntegerTextBox Value="{Binding CompletionPercentage}"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="CornflowerBlue"
                           ScrollInterval="10"
                           IsScrollingOnCircle="True"
                           Width="250"
                           Height="30"/>
```

## Important Notes

### Value Property vs Text Property

**⚠️ Critical:** Do NOT use the `Text` property to set values. Always use the `Value` property for IntegerTextBox. The `Text` property is inherited from TextBox but should not be used as it may cause unexpected behavior.

```csharp
// ✅ CORRECT
integerTextBox.Value = 100;

// ❌ INCORRECT - Do not use
// integerTextBox.Text = "100";
```

### Null Value Display

To display null or custom values when the control is empty:
1. Set `UseNullOption` to `true`
2. Configure `NullValue` property (null or custom integer)
3. If both `NullValue` and `WatermarkText` are set, only `NullValue` displays

### Validation Modes

Choose validation timing based on user experience needs:
- **OnKeyPress** - Immediate validation, prevents invalid input (stricter)
- **OnLostFocus** - Validation on focus loss, allows temporary invalid input (more flexible)

---

**Next Steps:** Use the navigation guide above to read specific reference files based on your implementation needs. Start with `getting-started.md` for initial setup, then explore feature-specific documentation as needed.
