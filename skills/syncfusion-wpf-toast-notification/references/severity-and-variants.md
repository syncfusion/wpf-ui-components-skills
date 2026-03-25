# Severity Levels and Visual Variants

## Table of Contents
- [Overview](#overview)
- [Severity Levels](#severity-levels)
- [Visual Variants](#visual-variants)
- [AccentBrush Customization](#accentbrush-customization)
- [Severity and Variant Combinations](#severity-and-variant-combinations)
- [Applicability Rules](#applicability-rules)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

Toast notifications provide built-in visual styling through **severity levels** and **visual variants**. These work together to create appropriate visual feedback based on the importance and type of notification.

**Key Concepts:**
- **Severity:** Defines the notification type (Info, Success, Warning, Error)
- **Variant:** Controls the visual style (Text, Outlined, Filled)
- **AccentBrush:** Allows custom color overrides

---

## Severity Levels

### Available Severity Levels

| Severity | Purpose | Default Color | When to Use |
|----------|---------|---------------|-------------|
| **None** | Neutral/default | Gray | General information without specific urgency |
| **Info** | Informational | Blue | Status updates, informational messages |
| **Success** | Positive outcome | Green | Successful operations, confirmations |
| **Warning** | Caution | Orange/Yellow | Warnings, potential issues |
| **Error** | Critical issue | Red | Errors, failures, critical problems |

### None Severity

Default neutral styling without specific color association.

```csharp
// Neutral toast (no severity styling)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Notification",
    Message = "This is a neutral notification.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.None  // No severity styling
});
```

**Characteristics:**
- Minimal visual emphasis
- Gray/neutral color scheme
- Variants (Outlined, Filled) not applicable when Severity = None
- AccentBrush not applicable when Severity = None

### Info Severity

Used for informational messages that don't require immediate action.

```csharp
// Information toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "System Update",
    Message = "Your system has been updated to the latest version.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info
});
```

**Use Cases:**
- System status updates
- General information
- Tips and helpful messages
- Non-critical notifications

### Success Severity

Indicates successful completion of operations.

```csharp
// Success toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Save Successful",
    Message = "Your document has been saved successfully.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Success
});
```

**Use Cases:**
- Operation completion
- Successful form submissions
- File saved/uploaded
- Process completed

### Warning Severity

Alerts users about potential issues or important information.

```csharp
// Warning toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Unsaved Changes",
    Message = "You have unsaved changes. Save before closing to avoid data loss.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Warning
});
```

**Use Cases:**
- Potential data loss
- Approaching limits or deadlines
- Caution before destructive actions
- Non-critical errors

### Error Severity

Indicates failures or critical issues requiring attention.

```csharp
// Error toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Operation Failed",
    Message = "Unable to connect to the server. Please check your connection.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Error
});
```

**Use Cases:**
- Operation failures
- Connection errors
- Validation errors
- Critical system issues

---

## Visual Variants

Variants control the visual styling intensity of toast notifications. They only apply when a severity level (Info, Success, Warning, Error) is set.

### Text Variant

Minimal styling with text-based emphasis.

```csharp
// Text variant (subtle styling)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Information",
    Message = "New updates are available.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,
    Variant = ToastVariant.Text  // Minimal styling
});
```

**Characteristics:**
- Subtle color accents
- Text-based emphasis
- Least intrusive
- Best for non-critical notifications

### Outlined Variant

Border-based styling with more visual prominence.

```csharp
// Outlined variant (border emphasis)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Warning",
    Message = "Your session will expire in 5 minutes.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Warning,
    Variant = ToastVariant.Outlined  // Border styling
});
```

**Characteristics:**
- Colored border
- Moderate visual emphasis
- Background remains light
- Good balance between subtle and prominent

### Filled Variant

Full background color for maximum visibility.

```csharp
// Filled variant (full background color)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Error",
    Message = "Failed to save changes. Please try again.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Error,
    Variant = ToastVariant.Filled  // Full background color
});
```

**Characteristics:**
- Full background color
- Maximum visual impact
- Most prominent styling
- Best for critical notifications

---

## AccentBrush Customization

The `AccentBrush` property allows custom color overrides for toast notifications. It only applies when a severity level is set.

### Basic AccentBrush Usage

```csharp
// Custom purple accent for info toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Custom Styled",
    Message = "This toast uses a custom accent color.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,
    AccentBrush = new SolidColorBrush(Colors.Purple)
});
```

### AccentBrush with Different Severities

```csharp
// Custom color for success
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Success",
    Message = "Custom green shade for success.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Success,
    Variant = ToastVariant.Filled,
    AccentBrush = new SolidColorBrush(Color.FromRgb(34, 139, 34))  // Forest green
});

// Custom color for error
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Error",
    Message = "Custom red shade for errors.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Error,
    Variant = ToastVariant.Outlined,
    AccentBrush = new SolidColorBrush(Color.FromRgb(178, 34, 34))  // Firebrick red
});
```

### AccentBrush Behavior

**When AccentBrush IS Applied:**
- Severity is set to Info, Success, Warning, or Error
- Mode is Window or Screen
- Overrides default severity colors

**When AccentBrush IS NOT Applied:**
- Severity = None (not applicable)
- Mode = Default (native OS notifications)

```csharp
// AccentBrush applied (Severity = Success)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Applied",
    Severity = ToastSeverity.Success,
    Mode = ToastMode.Screen,
    AccentBrush = new SolidColorBrush(Colors.DarkGreen)  // ✅ Applied
});

// AccentBrush ignored (Severity = None)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Ignored",
    Severity = ToastSeverity.None,
    Mode = ToastMode.Screen,
    AccentBrush = new SolidColorBrush(Colors.DarkGreen)  // ❌ Ignored
});
```

---

## Severity and Variant Combinations

### Info + All Variants

```csharp
// Info + Text
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Info Text",
    Message = "Subtle information styling.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,
    Variant = ToastVariant.Text
});

// Info + Outlined
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Info Outlined",
    Message = "Information with border emphasis.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,
    Variant = ToastVariant.Outlined
});

// Info + Filled
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Info Filled",
    Message = "Information with full background.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,
    Variant = ToastVariant.Filled
});
```

### Success + All Variants

```csharp
// Success + Text
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Success",
    Message = "Operation completed.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Success,
    Variant = ToastVariant.Text
});

// Success + Outlined
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Success",
    Message = "Operation completed.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Success,
    Variant = ToastVariant.Outlined
});

// Success + Filled (most prominent for success)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Success",
    Message = "Operation completed.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Success,
    Variant = ToastVariant.Filled
});
```

### Warning and Error Combinations

```csharp
// Warning + Filled (recommended for warnings)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Warning",
    Message = "Action requires confirmation.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Warning,
    Variant = ToastVariant.Filled
});

// Error + Filled (recommended for errors)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Error",
    Message = "Operation failed. Please retry.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Error,
    Variant = ToastVariant.Filled
});
```

---

## Applicability Rules

### Complete Customization Reference

| Feature | Default Mode | Window/Screen + Severity=None | Window/Screen + Severity Levels |
|---------|--------------|-------------------------------|--------------------------------|
| **Severity** | ❌ NO | ✅ YES (but visual effect minimal) | ✅ YES |
| **Variant** | ❌ NO | ❌ NO (not applicable) | ✅ YES |
| **AccentBrush** | ❌ NO | ❌ NO (not applicable) | ✅ YES |
| **Custom Styling** | ❌ NO | ⚠️ LIMITED | ✅ FULL |

### Rule Summary

1. **Severity + Variant + AccentBrush require:**
   - Mode: Window or Screen (NOT Default)
   - Severity: Info, Success, Warning, or Error (NOT None)

2. **When Severity = None:**
   - Variant has no effect
   - AccentBrush has no effect
   - Minimal visual styling applied

3. **When Mode = Default:**
   - NO customization supported
   - Severity, Variant, AccentBrush all ignored
   - Native OS styling only

---

## Edge Cases and Troubleshooting

### Issue: Variant Not Showing Effect

**Problem:** Variant property set but no visual change.

**Cause:** Severity is set to None.

**Solution:**
```csharp
// WRONG: Variant with Severity = None (no effect)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Test",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.None,     // ❌ Severity = None
    Variant = ToastVariant.Filled      // No effect
});

// CORRECT: Variant with severity level
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Test",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,     // ✅ Severity set
    Variant = ToastVariant.Filled      // Works correctly
});
```

### Issue: AccentBrush Not Applied

**Problem:** Custom AccentBrush not visible.

**Causes:**
1. Severity = None
2. Mode = Default

**Solution:**
```csharp
// Ensure severity is set and mode is Window or Screen
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Custom Color",
    Mode = ToastMode.Screen,            // ✅ Screen mode
    Severity = ToastSeverity.Success,   // ✅ Severity set
    AccentBrush = new SolidColorBrush(Colors.Teal)  // ✅ Applied
});
```

### Issue: Colors Look Wrong

**Problem:** Severity colors don't match brand colors.

**Solution:** Use AccentBrush to override default colors:

```csharp
// Brand-specific success color
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Success",
    Message = "Action completed.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Success,
    Variant = ToastVariant.Filled,
    AccentBrush = new SolidColorBrush(Color.FromRgb(0, 123, 255))  // Brand blue
});
```

## Best Practices

1. **Match severity to message type:** Use appropriate severity for the notification context
2. **Choose variants based on importance:**
   - Text: Low priority, informational
   - Outlined: Medium priority, status updates
   - Filled: High priority, errors/critical warnings
3. **Use AccentBrush for branding:** Override default colors to match app theme
4. **Test visibility:** Ensure text is readable against custom accent colors
5. **Be consistent:** Use the same severity + variant combinations throughout the app
