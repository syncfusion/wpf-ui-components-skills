# Advanced Features and API Reference for WPF TextInputLayout

## Overview

This guide covers advanced features of the WPF TextInputLayout control, including Right-to-Left (RTL) support for internationalization, and provides a comprehensive API reference for all public properties, enumerations, and supported input views.

## Table of Contents

1. [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
2. [Complete API Reference](#complete-api-reference)
3. [Properties Reference](#properties-reference)
4. [Enumerations](#enumerations)
5. [Supported Input Views](#supported-input-views)
6. [Advanced Usage Patterns](#advanced-usage-patterns)

---

## Right-to-Left (RTL) Support

### Overview

The TextInputLayout control provides full support for right-to-left languages such as Arabic, Hebrew, Persian, and Urdu. RTL support ensures proper text flow, alignment, and visual arrangement for international applications.

### Basic RTL Implementation

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    x:Name="textinputlayout" 
    FlowDirection="RightToLeft"
    ContainerType="Outlined"
    Hint="نام"  
    HelperText="اپنا نام درج کریں">
    <TextBox Text="جانسن" />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
textinputlayout.FlowDirection = FlowDirection.RightToLeft;
textinputlayout.ContainerType = ContainerType.Outlined;
textinputlayout.Hint = "نام";  // Name in Urdu
textinputlayout.HelperText = "اپنا نام درج کریں";  // Enter your name in Urdu
textinputlayout.InputView = new TextBox() { Text = "جانسن" };  // Johnson in Urdu
```

**Visual Result:**
```
Right-to-Left Layout:
┌────────────────────────────────┐
│                           نام  │
├────────────────────────────────┤
│                          جانسن │
├────────────────────────────────┤
│                 اپنا نام درج کریں │
└────────────────────────────────┘
```

### RTL Behavior

When `FlowDirection="RightToLeft"` is set:

1. **Text alignment**: All text aligns to the right
2. **Reading order**: Content flows from right to left
3. **Icons**: Leading/trailing views swap positions
4. **Hint position**: Floats to the right side
5. **Helper/Error text**: Aligns to the right

**Visual Comparison:**
```
Left-to-Right (LTR):              Right-to-Left (RTL):
┌──────────────────────┐         ┌──────────────────────┐
│ [L] Name         [T] │         │ [T]         نام [L]  │
├──────────────────────┤         ├──────────────────────┤
│ John Doe             │         │             جان ڈو   │
├──────────────────────┤         ├──────────────────────┤
│ Enter your name      │         │     اپنا نام درج کریں │
└──────────────────────┘         └──────────────────────┘

[L] = Leading View                [L] = Leading View (now on right)
[T] = Trailing View               [T] = Trailing View (now on left)
```

### Automatic Language Detection

```csharp
public static class RTLHelper
{
    public static bool IsRTLLanguage(string text)
    {
        if (string.IsNullOrEmpty(text)) return false;
        
        // Check for RTL Unicode ranges
        foreach (char c in text)
        {
            // Arabic: U+0600 to U+06FF
            // Hebrew: U+0590 to U+05FF
            // Persian/Urdu extensions: U+FB50 to U+FDFF
            if ((c >= 0x0590 && c <= 0x06FF) || 
                (c >= 0xFB50 && c <= 0xFDFF))
            {
                return true;
            }
        }
        return false;
    }
    
    public static void ApplyAutoRTL(SfTextInputLayout layout, string content)
    {
        layout.FlowDirection = IsRTLLanguage(content) 
            ? FlowDirection.RightToLeft 
            : FlowDirection.LeftToRight;
    }
}
```

### Culture-Based RTL

```csharp
public static class CultureHelper
{
    private static readonly string[] RTLCultures = 
    {
        "ar",    // Arabic
        "ar-SA", // Arabic (Saudi Arabia)
        "ar-EG", // Arabic (Egypt)
        "he",    // Hebrew
        "he-IL", // Hebrew (Israel)
        "fa",    // Persian
        "fa-IR", // Persian (Iran)
        "ur",    // Urdu
        "ur-PK"  // Urdu (Pakistan)
    };
    
    public static FlowDirection GetFlowDirection(CultureInfo culture)
    {
        string cultureName = culture.TwoLetterISOLanguageName;
        return RTLCultures.Any(c => c.StartsWith(cultureName))
            ? FlowDirection.RightToLeft
            : FlowDirection.LeftToRight;
    }
    
    public static void ApplyCurrentCulture(SfTextInputLayout layout)
    {
        layout.FlowDirection = GetFlowDirection(
            CultureInfo.CurrentCulture);
    }
}
```

### RTL Form Example

```xaml
<Window xmlns:inputLayout="clr-namespace:Syncfusion.UI.Xaml.TextInputLayout;assembly=Syncfusion.SfInput.WPF"
        FlowDirection="RightToLeft">
    <StackPanel Width="350" Margin="20">
        
        <TextBlock Text="تفصیلات درج کریں" 
                   FontSize="24" 
                   FontWeight="Bold"
                   TextAlignment="Right"
                   Margin="0,0,0,20"/>
        
        <!-- Name Field -->
        <inputLayout:SfTextInputLayout
            Hint="نام"
            HelperText="اپنا پورا نام"
            ContainerType="Outlined"
            Margin="0,0,0,16">
            <TextBox />
        </inputLayout:SfTextInputLayout>
        
        <!-- Email Field -->
        <inputLayout:SfTextInputLayout
            Hint="ای میل"
            HelperText="مثال: user@example.com"
            ContainerType="Outlined"
            Margin="0,0,0,16">
            <TextBox />
        </inputLayout:SfTextInputLayout>
        
        <!-- Phone Field -->
        <inputLayout:SfTextInputLayout
            Hint="فون نمبر"
            HelperText="بغیر ڈیش کے"
            ContainerType="Outlined">
            <TextBox />
        </inputLayout:SfTextInputLayout>
        
    </StackPanel>
</Window>
```

### Mixed Content Handling

```csharp
public class BiDirectionalFormBuilder
{
    public void CreateMixedLanguageForm()
    {
        var formPanel = new StackPanel();
        
        // English field
        var englishField = new SfTextInputLayout
        {
            Hint = "English Name",
            HelperText = "Enter name in English",
            FlowDirection = FlowDirection.LeftToRight,
            ContainerType = ContainerType.Outlined
        };
        formPanel.Children.Add(englishField);
        
        // Arabic field
        var arabicField = new SfTextInputLayout
        {
            Hint = "الاسم بالعربية",
            HelperText = "أدخل الاسم بالعربية",
            FlowDirection = FlowDirection.RightToLeft,
            ContainerType = ContainerType.Outlined
        };
        formPanel.Children.Add(arabicField);
        
        // Hebrew field
        var hebrewField = new SfTextInputLayout
        {
            Hint = "שם בעברית",
            HelperText = "הזן שם בעברית",
            FlowDirection = FlowDirection.RightToLeft,
            ContainerType = ContainerType.Outlined
        };
        formPanel.Children.Add(hebrewField);
    }
}
```

### RTL Best Practices

1. **Consistent Direction**: Set `FlowDirection` on the parent container when entire form is RTL
2. **Font Selection**: Use fonts that support RTL languages properly
3. **Icon Positioning**: Remember that leading/trailing positions swap in RTL
4. **Testing**: Test with actual RTL content, not transliteration
5. **Culture Awareness**: Respect user's system culture settings

---

## Complete API Reference

This section provides a comprehensive reference of all public APIs available in the SfTextInputLayout control.

### Quick Reference Table

| Category | Property Count | Key Features |
|----------|----------------|--------------|
| Core Properties | 4 | InputView, Hint, Visibility, Float Mode |
| Container Properties | 3 | Type, Background, Corner Radius |
| Color Properties | 5 | Focused, Unfocused, Error, Active, Border |
| Border Properties | 2 | Stroke Thickness (Focused/Unfocused) |
| Assistive Labels | 5 | Helper Text, Error Text, Visibility, HasError |
| Character Counter | 2 | Max Length, Counter Visibility |
| Icon Properties | 4 | Leading/Trailing View and Position |
| Layout Properties | 2 | Input Padding, Flow Direction |
| Dropdown Properties | 1 | Dropdown Border |

---

## Properties Reference

### Core Properties

#### InputView
- **Type**: `Object`
- **Description**: The input control (TextBox, PasswordBox, ComboBox, ComboBoxAdv, or SfTextBoxExt) for the text input layout.
- **Usage**:
```csharp
inputLayout.InputView = new TextBox();
inputLayout.InputView = new PasswordBox();
inputLayout.InputView = new ComboBox { ItemsSource = countries };
```

#### Hint
- **Type**: `String`
- **Description**: The floating label text that appears in the text input layout.
- **Usage**:
```csharp
inputLayout.Hint = "Email Address";
```

#### HintVisibility
- **Type**: `Visibility`
- **Description**: Controls the visibility of the hint label.
- **Default**: `Visible`
- **Usage**:
```csharp
inputLayout.HintVisibility = Visibility.Collapsed;
```

#### HintFloatMode
- **Type**: `HintFloatMode` (enum)
- **Description**: Determines how the hint label will be displayed.
- **Options**: `Float`, `AlwaysFloat`, `None`
- **Default**: `Float`
- **Usage**:
```csharp
inputLayout.HintFloatMode = HintFloatMode.AlwaysFloat;
```

---

### Container Properties

#### ContainerType
- **Type**: `ContainerType` (enum)
- **Description**: The style of the input container.
- **Options**: `Outlined`, `Filled`, `None`
- **Default**: `Outlined`
- **Usage**:
```csharp
inputLayout.ContainerType = ContainerType.Filled;
```

#### ContainerBackground
- **Type**: `Brush`
- **Description**: Background color of the container (not applicable for `None` type).
- **Usage**:
```csharp
inputLayout.ContainerBackground = Brushes.LightBlue;
inputLayout.ContainerBackground = new SolidColorBrush(Color.FromRgb(240, 240, 240));
```

#### OutlineCornerRadius
- **Type**: `Double`
- **Description**: Corner radius of the outlined border (only for `Outlined` type).
- **Default**: `4`
- **Usage**:
```csharp
inputLayout.OutlineCornerRadius = 8;
```

---

### Color and Styling Properties

#### FocusedForeground
- **Type**: `Brush`
- **Description**: Foreground color for hint, base line, and border when focused.
- **Usage**:
```csharp
inputLayout.FocusedForeground = Brushes.DodgerBlue;
```

#### Foreground
- **Type**: `Brush`
- **Description**: Foreground color when unfocused.
- **Usage**:
```csharp
inputLayout.Foreground = Brushes.Gray;
```

#### ErrorForeground
- **Type**: `Brush`
- **Description**: Color applied when `HasError` is true.
- **Usage**:
```csharp
inputLayout.ErrorForeground = Brushes.Red;
```

#### ActiveForeground
- **Type**: `Brush` (Read-only)
- **Description**: Gets the current active foreground color based on state.
- **Note**: Error color is NOT set to ActiveForeground when HasError is true.
- **Usage**:
```csharp
var currentColor = inputLayout.ActiveForeground;
```

#### FocusedBorderBrush
- **Type**: `Brush`
- **Description**: Border color when focused (overrides FocusedForeground for borders).
- **Usage**:
```csharp
inputLayout.FocusedBorderBrush = Brushes.Purple;
```

---

### Border and Stroke Properties

#### StrokeThickness
- **Type**: `Double`
- **Description**: Thickness of border/base line when unfocused.
- **Default**: `1`
- **Usage**:
```csharp
inputLayout.StrokeThickness = 2;
```

#### FocusedStrokeThickness
- **Type**: `Double`
- **Description**: Thickness of border/base line when focused.
- **Default**: `2`
- **Usage**:
```csharp
inputLayout.FocusedStrokeThickness = 3;
```

---

### Assistive Label Properties

#### HelperText
- **Type**: `String`
- **Description**: Guidance text displayed below the input field.
- **Usage**:
```csharp
inputLayout.HelperText = "Enter your email address";
```

#### HelperTextVisibility
- **Type**: `Visibility`
- **Description**: Controls helper text visibility.
- **Default**: `Visible`
- **Usage**:
```csharp
inputLayout.HelperTextVisibility = Visibility.Collapsed;
```

#### ErrorText
- **Type**: `String`
- **Description**: Error message displayed when validation fails.
- **Usage**:
```csharp
inputLayout.ErrorText = "Invalid email format";
```

#### HasError
- **Type**: `Boolean`
- **Description**: Indicates whether the control is in error state.
- **Default**: `false`
- **Usage**:
```csharp
inputLayout.HasError = true;
```

---

### Character Counter Properties

#### CharMaxLength
- **Type**: `Int32`
- **Description**: Maximum number of characters allowed.
- **Usage**:
```csharp
inputLayout.CharMaxLength = 50;
```

#### CharCountVisibility
- **Type**: `Visibility`
- **Description**: Controls character counter visibility.
- **Default**: `Collapsed`
- **Usage**:
```csharp
inputLayout.CharCountVisibility = Visibility.Visible;
```

---

### Icon Properties

#### LeadingView
- **Type**: `Object`
- **Description**: View (typically Label or Icon) at the leading edge.
- **Usage**:
```csharp
inputLayout.LeadingView = new Label { Text = "🔍", FontSize = 18 };
```

#### LeadingViewPosition
- **Type**: `ViewPosition` (enum)
- **Description**: Position of leading view.
- **Options**: `Inside`, `Outside`
- **Default**: `Outside`
- **Usage**:
```csharp
inputLayout.LeadingViewPosition = ViewPosition.Inside;
```

#### TrailingView
- **Type**: `Object`
- **Description**: View at the trailing edge.
- **Usage**:
```csharp
inputLayout.TrailingView = new Label { Text = "✓", FontSize = 18 };
```

#### TrailingViewPosition
- **Type**: `ViewPosition` (enum)
- **Description**: Position of trailing view.
- **Options**: `Inside`, `Outside`
- **Default**: `Inside`
- **Usage**:
```csharp
inputLayout.TrailingViewPosition = ViewPosition.Outside;
```

---

### Layout and Spacing Properties

#### InputViewPadding
- **Type**: `Thickness`
- **Description**: Padding around the input view.
- **Usage**:
```csharp
inputLayout.InputViewPadding = new Thickness(16, 8, 16, 8);
inputLayout.InputViewPadding = new Thickness(12); // All sides equal
```

#### FlowDirection
- **Type**: `FlowDirection` (enum)
- **Description**: Direction of text flow (LTR or RTL).
- **Options**: `LeftToRight`, `RightToLeft`
- **Default**: `LeftToRight`
- **Usage**:
```csharp
inputLayout.FlowDirection = FlowDirection.RightToLeft;
```

---

### Dropdown Properties

#### DropDownBorder
- **Type**: `Border`
- **Description**: Custom border for dropdown popup (ComboBox/ComboBoxAdv only).
- **Usage**:
```csharp
inputLayout.DropDownBorder = new Border
{
    BorderBrush = Brushes.Blue,
    BorderThickness = new Thickness(2),
    CornerRadius = new CornerRadius(5)
};
```

---

## Enumerations

### HintFloatMode

Defines how the hint label will be displayed.

```csharp
public enum HintFloatMode
{
    /// <summary>
    /// The hint label floats to the top when the input view is focused or has content.
    /// </summary>
    Float,
    
    /// <summary>
    /// The hint label is always positioned at the top of the input view.
    /// </summary>
    AlwaysFloat,
    
    /// <summary>
    /// The hint label is hidden when the input view is focused.
    /// </summary>
    None
}
```

**Usage Example**:
```csharp
// Material Design standard behavior
inputLayout.HintFloatMode = HintFloatMode.Float;

// Always show label (good for edit forms)
inputLayout.HintFloatMode = HintFloatMode.AlwaysFloat;

// Minimal design
inputLayout.HintFloatMode = HintFloatMode.None;
```

---

### ContainerType

Defines the style of the input container.

```csharp
public enum ContainerType
{
    /// <summary>
    /// The container is covered with a rounded border.
    /// </summary>
    Outlined,
    
    /// <summary>
    /// The background of the input view is filled with container color,
    /// with a base line at the bottom.
    /// </summary>
    Filled,
    
    /// <summary>
    /// The container has an empty background with minimal styling.
    /// </summary>
    None
}
```

**Visual Representation**:
```
Outlined:
╔════════════════════╗
║ Hint               ║
╠────────────────────╢
║ Input text         ║
╚════════════════════╝

Filled:
┌────────────────────┐
│ Hint               │
├────────────────────┤
│▒Input text▒▒▒▒▒▒▒▒▒│
└════════════════════┘

None:
Hint
─────────────────────
Input text
─────────────────────
```

---

### ViewPosition

Defines the position of leading or trailing views.

```csharp
public enum ViewPosition
{
    /// <summary>
    /// The view is positioned inside the container.
    /// </summary>
    Inside,
    
    /// <summary>
    /// The view is positioned outside the container.
    /// </summary>
    Outside
}
```

**Usage Example**:
```csharp
// Icon inside the input container
inputLayout.LeadingViewPosition = ViewPosition.Inside;
inputLayout.LeadingView = new Label { Text = "🔍" };

// Icon outside the input container
inputLayout.TrailingViewPosition = ViewPosition.Outside;
inputLayout.TrailingView = new Label { Text = "ℹ" };
```

---

## Supported Input Views

### Complete List

| Input View | Purpose | Key Features |
|------------|---------|--------------|
| **TextBox** | General text input | Single/multi-line, most common use |
| **PasswordBox** | Secure password entry | Masked characters, secure string |
| **ComboBox** | Dropdown selection | Standard WPF control, simple lists |
| **ComboBoxAdv** | Advanced dropdown | Syncfusion control, better performance |
| **SfTextBoxExt** | Autocomplete input | Syncfusion control, search suggestions |

### TextBox
```csharp
// Single line
inputLayout.InputView = new TextBox();

// Multi-line
inputLayout.InputView = new TextBox 
{ 
    AcceptsReturn = true,
    TextWrapping = TextWrapping.Wrap,
    MinHeight = 80
};
```

### PasswordBox
```csharp
inputLayout.InputView = new PasswordBox();

// Access password
var password = (inputLayout.InputView as PasswordBox).Password;
```

### ComboBox
```csharp
inputLayout.InputView = new ComboBox
{
    ItemsSource = new List<string> { "Option 1", "Option 2", "Option 3" },
    SelectedIndex = 0
};
```

### ComboBoxAdv
```csharp
inputLayout.InputView = new ComboBoxAdv
{
    ItemsSource = largeDataset,
    EnableVirtualization = true,
    AllowMultiSelect = false
};
```

### SfTextBoxExt
```csharp
inputLayout.InputView = new SfTextBoxExt
{
    AutoCompleteMode = AutoCompleteMode.Suggest,
    AutoCompleteSource = countries
};
```

---

## Advanced Usage Patterns

### Pattern 1: Validation Framework

```csharp
public class ValidationManager
{
    private Dictionary<SfTextInputLayout, Func<string, (bool isValid, string error)>> 
        validators = new Dictionary<SfTextInputLayout, Func<string, (bool, string)>>();
    
    public void RegisterValidator(
        SfTextInputLayout layout,
        Func<string, (bool, string)> validator)
    {
        validators[layout] = validator;
        
        // Hook into input view's text changed event
        if (layout.InputView is TextBox textBox)
        {
            textBox.TextChanged += (s, e) => ValidateField(layout, textBox.Text);
        }
    }
    
    private void ValidateField(SfTextInputLayout layout, string value)
    {
        if (validators.TryGetValue(layout, out var validator))
        {
            var (isValid, error) = validator(value);
            layout.HasError = !isValid;
            layout.ErrorText = error;
        }
    }
    
    public bool ValidateAll()
    {
        bool allValid = true;
        foreach (var kvp in validators)
        {
            var layout = kvp.Key;
            var text = GetTextFromInputView(layout.InputView);
            var (isValid, error) = kvp.Value(text);
            
            layout.HasError = !isValid;
            layout.ErrorText = error;
            allValid &= isValid;
        }
        return allValid;
    }
    
    private string GetTextFromInputView(object inputView)
    {
        return inputView switch
        {
            TextBox tb => tb.Text,
            PasswordBox pb => pb.Password,
            ComboBox cb => cb.SelectedItem?.ToString() ?? string.Empty,
            _ => string.Empty
        };
    }
}

// Usage
var manager = new ValidationManager();
manager.RegisterValidator(emailLayout, email => 
{
    bool isValid = Regex.IsMatch(email, @"^[^@\s]+@[^@\s]+\.[^@\s]+$");
    return (isValid, isValid ? string.Empty : "Invalid email format");
});
```

### Pattern 2: Form State Manager

```csharp
public class FormStateManager
{
    private List<SfTextInputLayout> layouts = new List<SfTextInputLayout>();
    
    public void RegisterField(SfTextInputLayout layout)
    {
        layouts.Add(layout);
    }
    
    public void SetReadOnly(bool isReadOnly)
    {
        foreach (var layout in layouts)
        {
            if (layout.InputView is TextBox textBox)
                textBox.IsReadOnly = isReadOnly;
            else if (layout.InputView is ComboBox comboBox)
                comboBox.IsEnabled = !isReadOnly;
        }
    }
    
    public Dictionary<string, string> GetFormData()
    {
        var data = new Dictionary<string, string>();
        foreach (var layout in layouts)
        {
            var key = layout.Hint;
            var value = GetValue(layout.InputView);
            data[key] = value;
        }
        return data;
    }
    
    public void SetFormData(Dictionary<string, string> data)
    {
        foreach (var layout in layouts)
        {
            if (data.TryGetValue(layout.Hint, out var value))
            {
                SetValue(layout.InputView, value);
            }
        }
    }
    
    private string GetValue(object inputView)
    {
        return inputView switch
        {
            TextBox tb => tb.Text,
            PasswordBox pb => pb.Password,
            ComboBox cb => cb.SelectedItem?.ToString() ?? string.Empty,
            _ => string.Empty
        };
    }
    
    private void SetValue(object inputView, string value)
    {
        switch (inputView)
        {
            case TextBox tb:
                tb.Text = value;
                break;
            case PasswordBox pb:
                pb.Password = value;
                break;
            case ComboBox cb:
                cb.SelectedItem = value;
                break;
        }
    }
}
```

### Pattern 3: Theme Manager

```csharp
public class ThemeManager
{
    public enum Theme { Light, Dark, HighContrast }
    
    public static void ApplyTheme(SfTextInputLayout layout, Theme theme)
    {
        switch (theme)
        {
            case Theme.Light:
                layout.FocusedForeground = new SolidColorBrush(Color.FromRgb(33, 150, 243));
                layout.Foreground = new SolidColorBrush(Color.FromRgb(117, 117, 117));
                layout.ErrorForeground = new SolidColorBrush(Color.FromRgb(244, 67, 54));
                layout.ContainerBackground = Brushes.White;
                break;
                
            case Theme.Dark:
                layout.FocusedForeground = new SolidColorBrush(Color.FromRgb(100, 181, 246));
                layout.Foreground = new SolidColorBrush(Color.FromRgb(189, 189, 189));
                layout.ErrorForeground = new SolidColorBrush(Color.FromRgb(239, 83, 80));
                layout.ContainerBackground = new SolidColorBrush(Color.FromRgb(30, 30, 30));
                break;
                
            case Theme.HighContrast:
                layout.FocusedForeground = Brushes.Yellow;
                layout.Foreground = Brushes.White;
                layout.ErrorForeground = Brushes.Red;
                layout.ContainerBackground = Brushes.Black;
                layout.FocusedStrokeThickness = 4;
                layout.StrokeThickness = 2;
                break;
        }
    }
}
```

---

## Related Topics

- [Customization](customization.md) - Detailed styling and color options
- [Icons and Views](icons-and-views.md) - Adding custom icons and views
- [Hint Configuration](hint-configuration.md) - Hint positioning and float modes
- [Assistive Labels](assistive-labels.md) - Helper text and validation messages
- [Container Types](container-type.md) - Understanding container styles

## Usage Notes and Important Considerations

1. **Character Counter**: Applies `ErrorForeground` when count exceeds `CharMaxLength`
2. **Error Validation**: Must be implemented at application level
3. **FocusedBorderBrush**: Overrides `FocusedForeground` for border color
4. **DropDownBorder**: Only applicable to ComboBox/ComboBoxAdv
5. **OutlineCornerRadius**: Only applicable to Outlined container type
6. **ContainerBackground**: Not applicable for None container type
7. **ActiveForeground**: Read-only, does not include error state
8. **RTL Support**: Automatically flips leading/trailing view positions

## Summary

The WPF TextInputLayout control provides:
- **Comprehensive RTL support** for international applications
- **50+ properties** for complete customization
- **3 hint float modes** for different UX patterns
- **3 container types** for various design needs
- **5 supported input views** for diverse scenarios
- **Advanced patterns** for validation, state management, and theming

This makes it a powerful, flexible control for building modern, accessible, and internationalized WPF applications.
