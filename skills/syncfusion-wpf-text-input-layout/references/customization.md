# Customizing WPF TextInputLayout Appearance

## Overview

This guide covers all customization options available in the WPF TextInputLayout control, enabling you to create visually consistent and branded input forms. The control provides extensive styling capabilities that adapt to different states (focused, unfocused, error) and container types.

## Table of Contents

1. [Color Customization](#color-customization)
2. [Container Styling](#container-styling)
3. [Border Customization](#border-customization)
4. [Stroke Thickness Control](#stroke-thickness-control)
5. [Layout Spacing](#layout-spacing)
6. [Best Practices](#best-practices)

---

## Color Customization

### Understanding State-Based Colors

The TextInputLayout applies different colors based on its current state. Understanding this state-color relationship is crucial for effective customization.

```
State Hierarchy:
┌─────────────────────────────────────┐
│ Error State (highest priority)     │
│   → ErrorForeground                 │
├─────────────────────────────────────┤
│ Focused State                       │
│   → FocusedForeground               │
├─────────────────────────────────────┤
│ Unfocused State (default)           │
│   → Foreground                      │
└─────────────────────────────────────┘
```

### Focused Color

The `FocusedForeground` property controls the color of the hint label, base line, and border when the input view has focus.

**When to use:**
- To highlight active input fields
- To match your application's accent color
- To create visual feedback for user interaction

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="User name" 
    FocusedForeground="Green"
    HelperText="Enter your name">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "User name";
inputLayout.FocusedForeground = Brushes.Green;
inputLayout.InputView = new TextBox() { Text = "John" };
```

**Visual Result:**
```
Focused State:
┌────────────────────────────┐
│ User name (Green)          │
├────────────────────────────┤
│ John                       │
├════════════════════════════┤ ← Green border/baseline
│ Enter your name            │
└────────────────────────────┘
```

> **Important:** The cursor color in the input view matches the application's Accent color, not the `FocusedForeground`.

> **Note:** The current active color can be retrieved from the read-only `ActiveForeground` property. However, when `HasError="True"`, the error color is NOT set to `ActiveForeground` since error is not a state but a condition.

### Unfocused Color

The `Foreground` property determines the color when the input view is not focused.

**When to use:**
- To create subtle, non-distracting input fields
- To maintain visual hierarchy in forms
- To indicate inactive/default state

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="User name" 
    Foreground="Gray"
    HelperText="Enter your name">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "User name";
inputLayout.Foreground = Brushes.Gray;
inputLayout.InputView = new TextBox() { Text = "John" };
```

**Visual Result:**
```
Unfocused State:
┌────────────────────────────┐
│ User name (Gray)           │
├────────────────────────────┤
│ John                       │
├────────────────────────────┤ ← Gray border/baseline
│ Enter your name            │
└────────────────────────────┘
```

### Error Color

The `ErrorForeground` property applies to the hint label, base line, border, and error text when `HasError="True"`.

**When to use:**
- For validation failures
- To draw attention to required corrections
- To maintain consistent error indication across forms

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ErrorForeground="Red"
    ErrorText="Should not contain special characters"
    HasError="True">
    <TextBox Text="John/" />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ErrorForeground = Brushes.Red;
inputLayout.ErrorText = "Should not contain special characters";
inputLayout.HasError = true;
inputLayout.InputView = new TextBox() { Text = "John/" };
```

**Visual Result:**
```
Error State:
┌────────────────────────────────────┐
│ Name (Red)                         │
├────────────────────────────────────┤
│ John/                              │
├════════════════════════════════════┤ ← Red border/baseline
│ ⚠ Should not contain special chars │ ← Red error text
└────────────────────────────────────┘
```

**Pattern: Dynamic Error Validation**
```csharp
private void ValidateInput(SfTextInputLayout inputLayout, TextBox textBox)
{
    // Check for special characters
    bool hasSpecialChars = Regex.IsMatch(textBox.Text, @"[^a-zA-Z0-9\s]");
    
    if (hasSpecialChars)
    {
        inputLayout.HasError = true;
        inputLayout.ErrorText = "Should not contain special characters";
    }
    else
    {
        inputLayout.HasError = false;
        inputLayout.ErrorText = string.Empty;
    }
}
```

---

## Container Styling

### Container Background Color

The `ContainerBackground` property customizes the container's background color for `Filled` and `Outlined` container types.

**When to use:**
- To match form design with app theme
- To create visual grouping of related inputs
- To improve contrast and readability

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    FocusedForeground="Blue"
    ContainerType="Outlined"
    ContainerBackground="LightBlue">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.FocusedForeground = Brushes.Blue;
inputLayout.ContainerBackground = Brushes.LightBlue;
inputLayout.ContainerType = ContainerType.Outlined;
inputLayout.InputView = new TextBox() { Text = "John" };
```

**Visual Comparison:**
```
Outlined with Background:
╔════════════════════════╗
║ Name (Blue)            ║ ← Light blue background
╠────────────────────────╢
║ John                   ║
╚════════════════════════╝

Filled with Background:
┌────────────────────────┐
│ Name (Blue)            │ ← Light blue background
├────────────────────────┤
│ John                   │
└════════════════════════┘ ← Base line at bottom
```

> **Important:** Container background is NOT applicable for `ContainerType.None`.

### Outline Corner Radius

The `OutlineCornerRadius` property controls the border curvature for outlined containers.

**When to use:**
- To match material design guidelines
- To create softer, modern UI elements
- To align with your app's design language

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Outlined"
    OutlineCornerRadius="8">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ContainerType = ContainerType.Outlined;
inputLayout.OutlineCornerRadius = 8;
inputLayout.InputView = new TextBox();
```

**Visual Comparison:**
```
OutlineCornerRadius="0" (square):
┏━━━━━━━━━━━━━━━━━━━━━━┓
┃ Name                 ┃
┣━━━━━━━━━━━━━━━━━━━━━━┫
┃                      ┃
┗━━━━━━━━━━━━━━━━━━━━━━┛

OutlineCornerRadius="8" (rounded):
╭──────────────────────╮
│ Name                 │
├──────────────────────┤
│                      │
╰──────────────────────╯

OutlineCornerRadius="16" (very rounded):
╭────────────────────────╮
│ Name                   │
├────────────────────────┤
│                        │
╰────────────────────────╯
```

> **Restriction:** Only applicable to `ContainerType.Outlined`.

**Pattern: Consistent Radius Across Forms**
```csharp
// Create a style for consistent corner radius
public static class InputLayoutStyles
{
    public static double StandardCornerRadius = 8;
    public static double CardCornerRadius = 12;
    
    public static void ApplyStandardStyle(SfTextInputLayout layout)
    {
        layout.ContainerType = ContainerType.Outlined;
        layout.OutlineCornerRadius = StandardCornerRadius;
        layout.FocusedForeground = Brushes.DodgerBlue;
        layout.Foreground = Brushes.Gray;
    }
}
```

---

## Border Customization

### Focused Border Brush

The `FocusedBorderBrush` property allows you to set a different border color independent of the foreground color when focused.

**When to use:**
- When border and text colors should differ
- To create subtle accent effects
- To maintain brand consistency

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Outlined"
    FocusedBorderBrush="Purple"
    FocusedForeground="Blue">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ContainerType = ContainerType.Outlined;
inputLayout.FocusedBorderBrush = Brushes.Purple;
inputLayout.FocusedForeground = Brushes.Blue;
inputLayout.InputView = new TextBox();
```

**Visual Result:**
```
Focused State:
╔══════════════════════╗ ← Purple border
║ Name (Blue hint)     ║
╠──────────────────────╢
║ [cursor here]        ║
╚══════════════════════╝
```

> **Priority:** When `FocusedBorderBrush` is set, it overrides the border color that would normally be set by `FocusedForeground`.

**Pattern: Dual-Tone Focus Effect**
```csharp
// Hint label and text use one color, border uses another
public void ApplyDualToneFocus(SfTextInputLayout layout)
{
    layout.FocusedForeground = new SolidColorBrush(Colors.DodgerBlue);
    layout.FocusedBorderBrush = new SolidColorBrush(
        Color.FromRgb(138, 43, 226)); // Purple
}
```

---

## Stroke Thickness Control

### Focused Stroke Thickness

The `FocusedStrokeThickness` property controls the border/baseline thickness when focused.

**When to use:**
- To create emphasis on active fields
- To follow material design specifications
- To improve visual feedback

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Outlined"
    FocusedStrokeThickness="3"
    FocusedBorderBrush="Blue">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ContainerType = ContainerType.Outlined;
inputLayout.FocusedStrokeThickness = 3;
inputLayout.FocusedBorderBrush = Brushes.Blue;
inputLayout.InputView = new TextBox();
```

**Visual Comparison:**
```
FocusedStrokeThickness="1":
┌────────────────────┐ ← Thin border
│ Name               │
├────────────────────┤
│                    │
└────────────────────┘

FocusedStrokeThickness="2" (default):
╔════════════════════╗ ← Medium border
║ Name               ║
╠════════════════════╣
║                    ║
╚════════════════════╝

FocusedStrokeThickness="3":
╔═════════════════════╗ ← Thick border
║ Name                ║
╠═════════════════════╣
║                     ║
╚═════════════════════╝
```

> **Default Value:** 2

### Unfocused Stroke Thickness

The `StrokeThickness` property controls the border/baseline thickness when unfocused.

**When to use:**
- To create subtle inactive state
- To maintain visual hierarchy
- To reduce clutter in dense forms

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Filled"
    StrokeThickness="2"
    Foreground="Gray">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ContainerType = ContainerType.Filled;
inputLayout.StrokeThickness = 2;
inputLayout.Foreground = Brushes.Gray;
inputLayout.InputView = new TextBox();
```

> **Default Value:** 1

**Pattern: Material Design Stroke Behavior**
```csharp
// Material Design typically uses 1px unfocused, 2px focused
public static void ApplyMaterialDesignStrokes(SfTextInputLayout layout)
{
    layout.StrokeThickness = 1;
    layout.FocusedStrokeThickness = 2;
}
```

**Pattern: Prominent Focus Indicator**
```csharp
// Create strong visual distinction between states
public static void ApplyProminentFocus(SfTextInputLayout layout)
{
    layout.StrokeThickness = 1;
    layout.FocusedStrokeThickness = 4;
    layout.FocusedBorderBrush = Brushes.DodgerBlue;
}
```

---

## Layout Spacing

### Input View Padding

The `InputViewPadding` property controls spacing between the input view and container boundaries.

**When to use:**
- To improve touch target size
- To match design specifications
- To create spacious or compact layouts

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Outlined"
    InputViewPadding="16,8,16,8">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ContainerType = ContainerType.Outlined;
inputLayout.InputViewPadding = new Thickness(16, 8, 16, 8);
inputLayout.InputView = new TextBox();
```

**Visual Comparison:**
```
Compact (8,4,8,4):
╔═══════════════════╗
║ Name              ║
╠═══════════════════╣
║▓Text▓             ║ ← Small padding
╚═══════════════════╝

Standard (16,8,16,8):
╔═══════════════════╗
║ Name              ║
╠═══════════════════╣
║                   ║
║  ▓Text▓           ║ ← Medium padding
║                   ║
╚═══════════════════╝

Spacious (24,16,24,16):
╔═══════════════════╗
║ Name              ║
╠═══════════════════╣
║                   ║
║                   ║
║   ▓Text▓          ║ ← Large padding
║                   ║
║                   ║
╚═══════════════════╝
```

**Pattern: Padding Presets**
```csharp
public static class PaddingPresets
{
    // Compact for dense forms
    public static Thickness Compact => new Thickness(8, 4, 8, 4);
    
    // Standard for general use
    public static Thickness Standard => new Thickness(16, 8, 16, 8);
    
    // Spacious for touch-friendly interfaces
    public static Thickness Spacious => new Thickness(24, 16, 24, 16);
    
    // Asymmetric for icons
    public static Thickness WithLeadingIcon => new Thickness(8, 8, 16, 8);
}
```

**Pattern: Touch-Optimized Forms**
```csharp
// Ensure minimum 44x44 touch target
public static void OptimizeForTouch(SfTextInputLayout layout)
{
    layout.InputViewPadding = PaddingPresets.Spacious;
    layout.MinHeight = 56; // Material Design minimum
}
```

---

## Best Practices

### 1. **Maintain Consistent Color Schemes**

```csharp
public static class AppColors
{
    public static Brush Primary = new SolidColorBrush(Colors.DodgerBlue);
    public static Brush OnSurface = new SolidColorBrush(Colors.Black);
    public static Brush OnSurfaceVariant = new SolidColorBrush(Colors.Gray);
    public static Brush Error = new SolidColorBrush(Colors.Red);
}

public static void ApplyTheme(SfTextInputLayout layout)
{
    layout.FocusedForeground = AppColors.Primary;
    layout.Foreground = AppColors.OnSurfaceVariant;
    layout.ErrorForeground = AppColors.Error;
}
```

### 2. **Container Type Selection Guide**

| Container Type | Use Case | Best For |
|----------------|----------|----------|
| **Outlined** | Clear boundaries needed | Forms with white backgrounds, multiple adjacent fields |
| **Filled** | Subtle, modern look | Forms on colored backgrounds, standalone fields |
| **None** | Minimal design | Simple forms, reading-focused UIs |

### 3. **Stroke Thickness Guidelines**

```csharp
// Accessibility: Ensure minimum 1px for visibility
// Focus indicator should be 2x or more of unfocused
public static class StrokeGuidelines
{
    // Standard desktop
    public static void ApplyDesktopStyle(SfTextInputLayout layout)
    {
        layout.StrokeThickness = 1;
        layout.FocusedStrokeThickness = 2;
    }
    
    // High-contrast accessibility
    public static void ApplyHighContrastStyle(SfTextInputLayout layout)
    {
        layout.StrokeThickness = 2;
        layout.FocusedStrokeThickness = 4;
    }
}
```

### 4. **Performance Optimization**

```csharp
// Reuse brush instances rather than creating new ones
private static readonly SolidColorBrush FocusedBrush = 
    new SolidColorBrush(Colors.DodgerBlue);
private static readonly SolidColorBrush UnfocusedBrush = 
    new SolidColorBrush(Colors.Gray);

public void OptimizedStyling(SfTextInputLayout layout)
{
    // More efficient than: layout.FocusedForeground = Brushes.DodgerBlue
    layout.FocusedForeground = FocusedBrush;
    layout.Foreground = UnfocusedBrush;
}
```

### 5. **Responsive Design Patterns**

```csharp
public void ApplyResponsiveStyling(SfTextInputLayout layout, double windowWidth)
{
    if (windowWidth < 600) // Mobile
    {
        layout.InputViewPadding = PaddingPresets.Spacious;
        layout.FocusedStrokeThickness = 3;
    }
    else if (windowWidth < 1200) // Tablet
    {
        layout.InputViewPadding = PaddingPresets.Standard;
        layout.FocusedStrokeThickness = 2;
    }
    else // Desktop
    {
        layout.InputViewPadding = PaddingPresets.Compact;
        layout.FocusedStrokeThickness = 2;
    }
}
```

### 6. **Theme-Aware Customization**

```csharp
public void ApplySystemTheme(SfTextInputLayout layout)
{
    // Detect system theme
    var isDarkMode = SystemParameters.HighContrast || 
                     Application.Current.RequestedTheme == ApplicationTheme.Dark;
    
    if (isDarkMode)
    {
        layout.Foreground = new SolidColorBrush(Colors.LightGray);
        layout.FocusedForeground = new SolidColorBrush(Colors.SkyBlue);
        layout.ContainerBackground = new SolidColorBrush(Color.FromRgb(30, 30, 30));
    }
    else
    {
        layout.Foreground = new SolidColorBrush(Colors.DarkGray);
        layout.FocusedForeground = new SolidColorBrush(Colors.DodgerBlue);
        layout.ContainerBackground = new SolidColorBrush(Colors.WhiteSmoke);
    }
}
```

---

## Complete Example: Styled Login Form

```xaml
<StackPanel Width="300" VerticalAlignment="Center">
    <!-- Username Field -->
    <inputLayout:SfTextInputLayout
        Hint="Username"
        ContainerType="Outlined"
        OutlineCornerRadius="8"
        FocusedForeground="#2196F3"
        FocusedBorderBrush="#2196F3"
        FocusedStrokeThickness="2"
        Foreground="#757575"
        StrokeThickness="1"
        InputViewPadding="16,12,16,12"
        HelperText="Enter your username"
        Margin="0,0,0,16">
        <TextBox />
    </inputLayout:SfTextInputLayout>
    
    <!-- Password Field -->
    <inputLayout:SfTextInputLayout
        Hint="Password"
        ContainerType="Outlined"
        OutlineCornerRadius="8"
        FocusedForeground="#2196F3"
        FocusedBorderBrush="#2196F3"
        FocusedStrokeThickness="2"
        Foreground="#757575"
        StrokeThickness="1"
        InputViewPadding="16,12,16,12"
        HelperText="At least 8 characters"
        Margin="0,0,0,16">
        <PasswordBox />
    </inputLayout:SfTextInputLayout>
    
    <!-- Email with Error State -->
    <inputLayout:SfTextInputLayout
        Hint="Email"
        ContainerType="Outlined"
        OutlineCornerRadius="8"
        FocusedForeground="#2196F3"
        ErrorForeground="#F44336"
        FocusedStrokeThickness="2"
        Foreground="#757575"
        StrokeThickness="1"
        InputViewPadding="16,12,16,12"
        ErrorText="Invalid email format"
        HasError="True">
        <TextBox Text="invalid@" />
    </inputLayout:SfTextInputLayout>
</StackPanel>
```

---

## Related Topics

- [Container Types](container-type.md) - Learn about Outlined, Filled, and None container types
- [Assistive Labels](assistive-labels.md) - Helper text, error text, and character counter
- [Icons and Views](icons-and-views.md) - Adding leading and trailing icons
- [Hint Configuration](hint-configuration.md) - Float modes and hint positioning
- [Advanced Features](advanced-features.md) - RTL support and API reference

## Common Pitfalls to Avoid

1. **Don't mix conflicting styles**: Setting both `FocusedBorderBrush` and relying on `FocusedForeground` for borders can be confusing.
2. **Container background on None type**: This has no effect and should be avoided.
3. **OutlineCornerRadius on non-Outlined types**: Only works with `ContainerType.Outlined`.
4. **Overly thick strokes**: Values above 4 can look unprofessional on desktop applications.
5. **Insufficient padding for touch**: Mobile interfaces need at least 16px padding for comfortable interaction.

## Summary

The WPF TextInputLayout provides comprehensive customization through:
- **State-based colors** (Focused, Unfocused, Error)
- **Container styling** (Background, corner radius)
- **Border control** (Separate brush, stroke thickness)
- **Layout spacing** (Input view padding)

By combining these features thoughtfully, you can create forms that are both beautiful and functional.
