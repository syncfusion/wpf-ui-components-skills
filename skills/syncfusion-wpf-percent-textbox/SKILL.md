---
name: syncfusion-wpf-percent-textbox
description: Use this skill when implementing Syncfusion WPF PercentTextBox controls for percentage input handling. Provides comprehensive guidance on value binding, min/max validation, number formatting with culture support, appearance customization with positive/negative/zero foregrounds, interactive features like scroll intervals and range adorners, watermark text, and data binding patterns for percentage-based input controls with international format support.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF PercentTextBox

## When to Use This Skill

Use this skill when the user needs to:

- **Implement percentage input controls** in WPF applications with value restrictions and formatting
- **Configure value validation** with minimum/maximum limits and validation timing (OnKeyPress vs OnLostFocus)
- **Apply number formatting** with culture-specific separators, decimal digits, and group sizes
- **Customize appearance** with different foreground colors for positive, negative, and zero values
- **Enable interactive features** like scroll intervals, mouse wheel support, click-and-drag value changes
- **Display visual progress** using range adorner to show value relative to min/max range
- **Bind percentage data** from ViewModels with property change notifications
- **Handle null values** with custom null displays or watermark text
- **Implement international formats** with culture-based decimal/group separators and percent symbols

The PercentTextBox control restricts input to double values and displays them as percentages with extensive customization options for data binding, validation, formatting, and appearance.

## Component Overview

The **Syncfusion WPF PercentTextBox** is a specialized input control that:

- Restricts input to **double values only** (no text allowed)
- Displays values as **percentages** with customizable symbols and patterns
- Supports **min/max value validation** with configurable validation timing
- Provides **data binding support** for MVVM patterns
- Offers **culture-specific formatting** with group separators and decimal digits
- Allows **different foreground colors** for positive, negative, and zero values
- Supports **interactive value changes** via keyboard arrows, mouse wheel, and click-drag
- Includes **range adorner** for visual progress indication
- Handles **null values** with watermark text support
- Provides **built-in visual styles** and themes

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Use this reference when you need to:
- Add PercentTextBox to a WPF project (via designer, XAML, or C#)
- Configure assembly deployment and NuGet packages (Syncfusion.Shared.WPF)
- Understand basic PercentValue property usage
- Set up initial control dimensions and placement
- Learn the basic control declaration patterns

### Value Management and Binding
📄 **Read:** [references/value-management.md](references/value-management.md)

Use this reference when you need to:
- Set and change percentage values using PercentValue property
- Implement data binding (unidirectional or bidirectional) with ViewModels
- Handle PercentValueChanged events to track value changes
- Configure null value behavior with NullValue and UseNullOption properties
- Display watermark text when control is empty or null
- Customize watermark appearance with foreground colors or templates
- Control paste behavior with PasteMode property (Default vs Advanced)
- Add spin buttons for increment/decrement operations

### Validation and Restrictions
📄 **Read:** [references/validation-and-restrictions.md](references/validation-and-restrictions.md)

Use this reference when you need to:
- Restrict values within minimum and maximum limits (MinValue, MaxValue)
- Configure validation timing (MinValidation, MaxValidation: OnKeyPress vs OnLostFocus)
- Control behavior when exceeding limits (MaxValueOnExceedMaxDigit, MinValueOnExceedMinDigit)
- Restrict decimal digit count with MinPercentDecimalDigits and MaxPercentDecimalDigits
- Understand property precedence for decimal digit settings
- Enable read-only mode while allowing text selection and caret visibility
- Prevent invalid input at key press vs validating on focus lost

### Number Formatting and Culture
📄 **Read:** [references/number-formatting.md](references/number-formatting.md)

Use this reference when you need to:
- Apply culture-specific formatting with Culture property
- Configure NumberFormatInfo for custom number patterns
- Set group separators (PercentGroupSeparator) and group sizes (PercentGroupSizes)
- Customize decimal separators and digit counts
- Change percentage symbol (PercentageSymbol)
- Enable group separator display with GroupSeperatorEnabled
- Define positive value patterns (PercentPositivePattern: 4 options)
- Define negative value patterns (PercentNegativePattern: 12 options)
- Understand property precedence (dedicated properties vs NumberFormat vs Culture)

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

Use this reference when you need to:
- Set different foreground colors for positive values (PositiveForeground)
- Apply negative foreground with ApplyNegativeForeground and NegativeForeground
- Display zero values with custom color (ApplyZeroColor, ZeroColor)
- Customize background brush for the control
- Apply corner radius for rounded borders
- Configure selection brush and opacity for highlighted text
- Align text (Left, Center, Right) using TextAlignment
- Add tooltips with custom information

### Interactive Features
📄 **Read:** [references/interactive-features.md](references/interactive-features.md)

Use this reference when you need to:
- Configure scroll interval for value increments/decrements
- Enable keyboard navigation with Up/Down arrow keys
- Allow mouse wheel scrolling with IsScrollingOnCircle
- Enable click-and-drag value changes with EnableExtendedScrolling
- Control text selection behavior on focus (TextSelectionOnFocus)
- Display spin buttons for manual increment/decrement
- Enable range adorner for visual progress indication (EnableRangeAdorner)
- Customize range adorner background color
- Understand min/max requirements for range adorner display

## Quick Start Example

### Basic PercentTextBox in XAML

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:PercentTextBox x:Name="percentTextBox" 
                                   Width="150" 
                                   Height="30"
                                   PercentValue="75.5"
                                   MinValue="0"
                                   MaxValue="100"/>
    </Grid>
</Window>
```

### Basic PercentTextBox in C#

```csharp
using Syncfusion.Windows.Shared;

// Create instance
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.Width = 150;
percentTextBox.Height = 30;

// Set value and limits
percentTextBox.PercentValue = 75.5;
percentTextBox.MinValue = 0;
percentTextBox.MaxValue = 100;

// Add to container
this.Content = percentTextBox;
```

### Data Binding Example

```xml
<!-- XAML -->
<syncfusion:PercentTextBox PercentValue="{Binding DiscountPercentage, UpdateSourceTrigger=PropertyChanged}"
                           Width="150" Height="30"
                           MinValue="0" MaxValue="100"/>
```

```csharp
// ViewModel.cs
public class ViewModel : INotifyPropertyChanged
{
    private double discountPercentage;
    public double DiscountPercentage
    {
        get { return discountPercentage; }
        set
        {
            discountPercentage = value;
            OnPropertyChanged(nameof(DiscountPercentage));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Common Patterns

### Pattern 1: Percentage Input with Validation

When user needs a percentage input with immediate validation on keypress:

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           MinValue="0"
                           MaxValue="100"
                           MinValidation="OnKeyPress"
                           MaxValidation="OnKeyPress"
                           MaxValueOnExceedMaxDigit="True"
                           MinValueOnExceedMinDigit="True"/>
```

**Why this works:** OnKeyPress validation prevents invalid input immediately, and the exceed properties reset to limits when boundaries are crossed.

### Pattern 2: Formatted Percentage with Culture

When user needs culture-specific percentage formatting:

```xml
<syncfusion:PercentTextBox PercentValue="1234.567"
                           Culture="de-DE"
                           GroupSeperatorEnabled="True"
                           PercentDecimalDigits="2"/>
```

**Why this works:** German culture uses "." for group separator and "," for decimal separator, automatically formatting the display.

### Pattern 3: Color-Coded Percentage Values

When user needs visual differentiation for positive/negative/zero values:

```xml
<syncfusion:PercentTextBox PercentValue="-15"
                           PositiveForeground="Green"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"
                           ZeroColor="Gray"
                           ApplyZeroColor="True"/>
```

**Why this works:** Different colors help users quickly identify value states without reading the number.

### Pattern 4: Interactive Percentage with Scroll

When user needs quick value adjustments via keyboard/mouse:

```xml
<syncfusion:PercentTextBox PercentValue="25"
                           ScrollInterval="5"
                           IsScrollingOnCircle="True"
                           EnableExtendedScrolling="True"
                           ShowSpinButton="True"/>
```

**Why this works:** Multiple interaction methods (arrow keys, mouse wheel, spin buttons, click-drag) provide flexibility for user preference.

### Pattern 5: Range Adorner for Visual Feedback

When user needs visual progress indication:

```xml
<syncfusion:PercentTextBox PercentValue="65"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightBlue"/>
```

**Why this works:** Range adorner fills the control background proportionally, showing progress at a glance like a progress bar.

### Pattern 6: Null Value with Watermark

When user needs placeholder text for empty state:

```xml
<syncfusion:PercentTextBox UseNullOption="True"
                           NullValue="{x:Null}"
                           WatermarkText="Enter discount %"
                           WatermarkTextIsVisible="True"
                           WatermarkTextForeground="Gray"/>
```

**Why this works:** Watermark provides user guidance when control is empty, improving UX.

## Key Properties Reference

### Value Properties
- **PercentValue** - Gets or sets the percentage value (double)
- **MinValue** - Minimum allowed value
- **MaxValue** - Maximum allowed value
- **NullValue** - Value to display when UseNullOption is true and value is null
- **UseNullOption** - Enables null value handling

### Validation Properties
- **MinValidation** - Validation timing for minimum (OnKeyPress, OnLostFocus)
- **MaxValidation** - Validation timing for maximum (OnKeyPress, OnLostFocus)
- **MinValueOnExceedMinDigit** - Reset to MinValue when exceeded (bool)
- **MaxValueOnExceedMaxDigit** - Reset to MaxValue when exceeded (bool)

### Formatting Properties
- **Culture** - Culture for number formatting (CultureInfo)
- **NumberFormat** - Custom NumberFormatInfo object
- **PercentGroupSeparator** - Character for grouping (e.g., ",")
- **PercentGroupSizes** - Array of group sizes (e.g., {3, 2})
- **PercentDecimalSeparator** - Decimal point character (e.g., ".")
- **PercentDecimalDigits** - Number of decimal places
- **MinPercentDecimalDigits** - Minimum decimal digits
- **MaxPercentDecimalDigits** - Maximum decimal digits
- **PercentageSymbol** - Percent symbol (default "%")
- **GroupSeperatorEnabled** - Show/hide group separator (bool)
- **PercentPositivePattern** - Format for positive values (0-3)
- **PercentNegativePattern** - Format for negative values (0-11)

### Appearance Properties
- **PositiveForeground** - Foreground brush for positive values
- **NegativeForeground** - Foreground brush for negative values
- **ApplyNegativeForeground** - Enable negative foreground (bool)
- **ZeroColor** - Foreground brush for zero value
- **ApplyZeroColor** - Enable zero color (bool)
- **Background** - Control background brush
- **CornerRadius** - Border corner radius
- **SelectionBrush** - Selected text background
- **SelectionOpacity** - Selected text opacity (0-1)
- **TextAlignment** - Text alignment (Left, Center, Right)

### Interactive Properties
- **ScrollInterval** - Increment/decrement step value
- **IsScrollingOnCircle** - Enable mouse wheel scrolling (bool)
- **EnableExtendedScrolling** - Enable click-drag scrolling (bool)
- **ShowSpinButton** - Display up/down buttons (bool)
- **TextSelectionOnFocus** - Auto-select text on focus (bool)
- **EnableRangeAdorner** - Show range adorner (bool)
- **RangeAdornerBackground** - Range adorner fill brush

### Watermark Properties
- **WatermarkText** - Placeholder text string
- **WatermarkTextIsVisible** - Show watermark (bool)
- **WatermarkTextForeground** - Watermark text color
- **WatermarkTemplate** - Custom DataTemplate for watermark

### Other Properties
- **IsReadOnly** - Prevent user editing (bool)
- **IsReadOnlyCaretVisible** - Show caret in read-only mode (bool)
- **PasteMode** - Paste behavior (Default, Advanced)
- **ToolTip** - Tooltip text or object

## Common Use Cases

### 1. Discount/Tax Calculator
Use PercentTextBox for discount or tax percentage inputs with 0-100% range, 2 decimal places, and immediate validation.

### 2. Progress/Completion Tracker
Display project completion percentages with range adorner for visual feedback, restricted to 0-100%.

### 3. Financial Calculations
Handle interest rates, ROI percentages, or growth rates with culture-specific formatting and multiple decimal places.

### 4. Survey/Rating Systems
Collect percentage-based ratings with color-coded foregrounds (green for high, red for low).

### 5. Resource Allocation
Input percentage allocations (e.g., budget distribution) with validation ensuring total doesn't exceed 100%.

### 6. Configuration Settings
Set performance thresholds, cache sizes, or buffer percentages with scroll intervals for quick adjustments.

### 7. Data Analysis Forms
Collect statistical percentages with group separators for large numbers and culture-specific formatting.

### 8. Dashboard Controls
Display KPI percentages with range adorners showing progress toward targets, bound to live data.

---

**Next Steps:**
1. Read the appropriate reference file based on your specific implementation need
2. Use the quick start example as a foundation
3. Apply common patterns that match your use case
4. Refer to key properties for fine-tuning behavior and appearance
