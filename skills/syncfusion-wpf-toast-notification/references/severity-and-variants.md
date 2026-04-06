# Severity Levels and Visual Variants

## Table of Contents
- [Overview](#overview)
- [Severity Levels](#severity-levels)
- [Visual Variants](#visual-variants)
- [AccentBrush Customization](#accentbrush-customization)
- [Applicability Rules](#applicability-rules)

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

### Severity Options

- **None:** Neutral styling (Variants/AccentBrush not applicable)
- **Info:** Blue, for status updates and tips
- **Success:** Green, for successful operations
- **Warning:** Orange/Yellow, for cautions and potential issues
- **Error:** Red, for failures and critical problems

Set the `Severity` property when showing toasts.

---

## Visual Variants

Variants control the visual styling intensity of toast notifications. They only apply when a severity level (Info, Success, Warning, Error) is set.

### Variant Options

- **Text:** Minimal styling, subtle, least intrusive (default)
- **Outlined:** Colored border with light background, moderate emphasis
- **Filled:** Full background color, maximum visibility for critical notifications

Set the `Variant` property. Only applies when Severity is Info/Success/Warning/Error.

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

### AccentBrush Behavior

**Applied when:** Severity is Info/Success/Warning/Error AND Mode is Window/Screen.

**Ignored when:** Severity = None OR Mode = Default.

```csharp
// Applied: Severity = Success + Mode = Screen
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Applied",
    Severity = ToastSeverity.Success,
    Mode = ToastMode.Screen,
    AccentBrush = new SolidColorBrush(Colors.DarkGreen)  // ✅ Applied
});
```

---

## Applicability Rules

### Complete Customization Reference

| Feature | Default Mode | Window/Screen + Severity=None | Window/Screen + Severity Levels |
|---------|--------------|-------------------------------|--------------------------------|
| **Severity** | ❌ NO | ⚠️ Accepted (no color effect) | ✅ YES |
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

## Best Practices

1. **Match severity to message type:** Use appropriate severity for the notification context
2. **Choose variants based on importance:** Text (low), Outlined (medium), Filled (high priority/errors)
3. **Use AccentBrush for branding:** Override default colors to match app theme
4. **Test visibility:** Ensure text is readable against custom accent colors
5. **Be consistent:** Use same severity + variant combinations throughout app
