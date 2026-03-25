# Hint Position and Configuration in WPF TextInputLayout

## Overview

The hint label (also known as a floating label) is a key feature of the TextInputLayout control that improves user experience by providing context about the expected input. This guide covers all aspects of hint configuration, including float modes, positioning behavior, and best practices for creating intuitive form inputs.

## Table of Contents

1. [Understanding Hint Labels](#understanding-hint-labels)
2. [Hint Float Modes](#hint-float-modes)
3. [Float Mode (Default)](#float-mode-default)
4. [AlwaysFloat Mode](#alwaysfloat-mode)
5. [None Mode](#none-mode)
6. [Best Practices](#best-practices)

---

## Understanding Hint Labels

### What is a Hint Label?

A hint label is text that provides guidance about what should be entered into an input field. Unlike traditional static labels, hint labels in TextInputLayout can "float" or animate based on the input state.

### Hint Label States

The hint label can exist in different states depending on the float mode and input view state:

```
State Diagram:
┌──────────────────────────────────────────┐
│                                          │
│  Empty & Unfocused                      │
│  ┌────────────────────────┐             │
│  │ Name                   │ ← Large     │
│  │                        │   centered  │
│  └────────────────────────┘             │
│                                          │
├──────────────────────────────────────────┤
│                                          │
│  Focused or Has Content                 │
│  ┌────────────────────────┐             │
│  │ Name (small)           │ ← Floated   │
│  ├────────────────────────┤   to top    │
│  │ John                   │             │
│  └────────────────────────┘             │
│                                          │
└──────────────────────────────────────────┘
```

### Key Concepts

- **Hint**: The text displayed in the label
- **Float**: The animation/transition of the hint label
- **HintFloatMode**: Property that controls hint behavior
- **Position**: Where the hint appears relative to the input view

---

## Hint Float Modes

The `HintFloatMode` property controls how the floating label behaves. Three modes are available:

| Mode | Behavior | Use Case |
|------|----------|----------|
| **Float** (Default) | Floats to top when focused or has content | Standard material design behavior |
| **AlwaysFloat** | Always positioned at top | Pre-filled forms, constant context |
| **None** | Hidden when focused | Minimal design, external labels present |

> **Default Value:** The default value of `HintFloatMode` is `Float`.

---

## Float Mode (Default)

### Behavior

In Float mode, the hint label starts inside the input area and floats to the top when:
1. The input view receives focus
2. The input view contains text

**Animation Flow:**
```
Step 1 - Initial State (Empty & Unfocused):
┌─────────────────────────────────┐
│                                 │
│  Name (large, centered)         │
│                                 │
└─────────────────────────────────┘

Step 2 - User Clicks (Focused):
┌─────────────────────────────────┐
│ Name (small, at top)            │
├─────────────────────────────────┤
│ [cursor blinking]               │
└─────────────────────────────────┘

Step 3 - User Types:
┌─────────────────────────────────┐
│ Name (small, at top)            │
├─────────────────────────────────┤
│ John[cursor]                    │
└─────────────────────────────────┘

Step 4 - User Leaves (Blur with content):
┌─────────────────────────────────┐
│ Name (small, at top)            │
├─────────────────────────────────┤
│ John                            │
└─────────────────────────────────┘

Step 5 - Clear & Blur (Empty again):
┌─────────────────────────────────┐
│                                 │
│  Name (large, centered)         │
│                                 │
└─────────────────────────────────┘
```

### Implementation

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Name"
    HintFloatMode="Float" 
    HelperText="Enter your name">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.HintFloatMode = HintFloatMode.Float;
inputLayout.HelperText = "Enter your name";
inputLayout.InputView = new TextBox();
```

### When to Use Float Mode

✅ **Best for:**
- New data entry forms
- Empty fields requiring user input
- Standard material design implementations
- Most general-purpose input scenarios

❌ **Avoid when:**
- Forms are pre-filled with data
- Users need constant context (use AlwaysFloat)
- External labels already provide context

### Visual States in Float Mode

**Container Type Impact:**

```
Outlined Container:
Empty State:
┌─────────────────────────┐
│                         │
│  Email                  │
│                         │
└─────────────────────────┘

Floated State:
┌──Email──────────────────┐
│ user@example.com        │
│                         │
└─────────────────────────┘

Filled Container:
Empty State:
┌─────────────────────────┐
│  Email                  │
│▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒│
└─────────────────────────┘

Floated State:
┌─────────────────────────┐
│ Email                   │
│▒user@example.com▒▒▒▒▒▒▒▒│
└─────────────────────────┘
```

---

## AlwaysFloat Mode

### Behavior

In AlwaysFloat mode, the hint label is permanently positioned at the top of the input view, regardless of focus or content state.

**State Diagram:**
```
All States (Empty, Focused, Filled):
┌─────────────────────────────────┐
│ Name (always small, at top)     │
├─────────────────────────────────┤
│ [content or empty]              │
└─────────────────────────────────┘
```

### Implementation

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Name"
    HintFloatMode="AlwaysFloat" 
    HelperText="Enter your name">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.HintFloatMode = HintFloatMode.AlwaysFloat;
inputLayout.HelperText = "Enter your name";
inputLayout.InputView = new TextBox();
```

### When to Use AlwaysFloat Mode

✅ **Best for:**
- Pre-filled forms (user profiles, settings)
- Edit forms with existing data
- When users need constant field identification
- Read-only or disabled fields that still need labels
- Forms where animation might be distracting

❌ **Avoid when:**
- Space is limited (float saves vertical space when empty)
- Following strict material design guidelines
- Animation provides helpful feedback

### Visual Comparison

```
Float Mode vs AlwaysFloat Mode:

Float (Empty):                   AlwaysFloat (Empty):
┌─────────────────────────┐     ┌─────────────────────────┐
│                         │     │ Email                   │
│  Email                  │     ├─────────────────────────┤
│                         │     │                         │
└─────────────────────────┘     └─────────────────────────┘

Float (Filled):                  AlwaysFloat (Filled):
┌─────────────────────────┐     ┌─────────────────────────┐
│ Email                   │     │ Email                   │
├─────────────────────────┤     ├─────────────────────────┤
│ john@example.com        │     │ john@example.com        │
└─────────────────────────┘     └─────────────────────────┘
```

### Pattern: Pre-filled User Profile

```csharp
public void PopulateUserProfile(UserProfile profile)
{
    // Use AlwaysFloat for pre-filled data
    var nameLayout = new SfTextInputLayout
    {
        Hint = "Full Name",
        HintFloatMode = HintFloatMode.AlwaysFloat,
        InputView = new TextBox { Text = profile.FullName }
    };
    
    var emailLayout = new SfTextInputLayout
    {
        Hint = "Email Address",
        HintFloatMode = HintFloatMode.AlwaysFloat,
        InputView = new TextBox { Text = profile.Email }
    };
    
    var phoneLayout = new SfTextInputLayout
    {
        Hint = "Phone Number",
        HintFloatMode = HintFloatMode.AlwaysFloat,
        InputView = new TextBox { Text = profile.PhoneNumber }
    };
}
```

---

## None Mode

### Behavior

In None mode, the hint label is hidden when the input view receives focus. This creates a minimal design where the label only appears when the field is empty and unfocused.

**State Diagram:**
```
Empty & Unfocused:
┌─────────────────────────────────┐
│                                 │
│  Name (visible)                 │
│                                 │
└─────────────────────────────────┘

Focused (Empty or With Content):
┌─────────────────────────────────┐
│ [cursor or text]                │
│                                 │
└─────────────────────────────────┘
(Hint is hidden)

Unfocused with Content:
┌─────────────────────────────────┐
│ John                            │
│                                 │
└─────────────────────────────────┘
(Hint remains hidden)
```

### Implementation

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Name"
    HintFloatMode="None"
    HelperText="Enter your name">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.HintFloatMode = HintFloatMode.None;
inputLayout.HelperText = "Enter your name";
inputLayout.InputView = new TextBox();
```

### When to Use None Mode

✅ **Best for:**
- Minimalist design aesthetics
- When external labels are present
- Forms with very clear context
- Wizard-style interfaces with one input per page
- When helper text provides sufficient guidance

❌ **Avoid when:**
- Users need constant field identification
- Multiple similar fields are present
- Accessibility is a primary concern
- Forms are complex or lengthy

⚠️ **Accessibility Warning:** None mode can be problematic for users relying on screen readers or those with cognitive disabilities who need constant field identification.

### Visual Behavior

```
Timeline of User Interaction:

T0 - Page Load:
┌─────────────────────────┐
│                         │
│  Username               │
│                         │
└─────────────────────────┘
Helper: Enter your username

T1 - User Clicks:
┌─────────────────────────┐
│ [cursor]                │
│                         │
└─────────────────────────┘
Helper: Enter your username
(Hint disappears)

T2 - User Types:
┌─────────────────────────┐
│ john_doe                │
│                         │
└─────────────────────────┘
Helper: Enter your username
(Hint still hidden)

T3 - User Clears & Blurs:
┌─────────────────────────┐
│                         │
│  Username               │
│                         │
└─────────────────────────┘
Helper: Enter your username
(Hint reappears)
```

### Pattern: Single-Field Wizard

```csharp
public class WizardStep
{
    public void CreateStep(string question, string hint, string helperText)
    {
        var layout = new SfTextInputLayout
        {
            Hint = hint,
            HintFloatMode = HintFloatMode.None, // Clean, minimal look
            HelperText = helperText,
            ContainerType = ContainerType.None, // Extra minimal
            InputView = new TextBox()
        };
        
        // Large question text above
        var questionLabel = new TextBlock
        {
            Text = question,
            FontSize = 24,
            FontWeight = FontWeights.Bold,
            Margin = new Thickness(0, 0, 0, 20)
        };
        
        // Stack vertically
        var container = new StackPanel();
        container.Children.Add(questionLabel);
        container.Children.Add(layout);
    }
}
```

---

## Best Practices

### 1. **Mode Selection Matrix**

| Scenario | Recommended Mode | Reason |
|----------|-----------------|--------|
| New user registration | Float | Standard material design, empty fields |
| Edit profile form | AlwaysFloat | Pre-filled data needs constant labels |
| Single search box | None or Float | Minimal distraction, clear purpose |
| Password reset form | Float | Clear state transitions |
| Settings page | AlwaysFloat | Often has existing values |
| Quick action dialog | None | Single focused purpose |
| Multi-step wizard | None or Float | Clean, uncluttered interface |
| Data grid editing | AlwaysFloat | Constant context needed |

### 2. **Consistency Guidelines**

```csharp
public static class FormStyles
{
    // Use consistent float mode across related forms
    public static HintFloatMode GetModeForFormType(FormType type)
    {
        return type switch
        {
            FormType.CreateNew => HintFloatMode.Float,
            FormType.Edit => HintFloatMode.AlwaysFloat,
            FormType.Search => HintFloatMode.None,
            FormType.Filter => HintFloatMode.Float,
            _ => HintFloatMode.Float // Safe default
        };
    }
}
```

### 3. **Accessibility Considerations**

```csharp
public void EnsureAccessibility(SfTextInputLayout layout)
{
    // Always provide helper text when using None mode
    if (layout.HintFloatMode == HintFloatMode.None)
    {
        if (string.IsNullOrEmpty(layout.HelperText))
        {
            // Fallback to hint as helper text
            layout.HelperText = layout.Hint;
        }
    }
    
    // Set automation properties
    AutomationProperties.SetName(layout, layout.Hint);
    AutomationProperties.SetHelpText(layout, layout.HelperText);
}
```

### 4. **Animation Performance**

```csharp
// Float mode has animation overhead
// For forms with many fields, consider:
public void OptimizeFormPerformance(Panel formPanel, int fieldCount)
{
    if (fieldCount > 20)
    {
        // Use AlwaysFloat to reduce animations
        foreach (var child in formPanel.Children.OfType<SfTextInputLayout>())
        {
            child.HintFloatMode = HintFloatMode.AlwaysFloat;
        }
    }
}
```

### 5. **Helper Text Coordination**

```xaml
<!-- Good: Helper text complements the hint -->
<inputLayout:SfTextInputLayout 
    Hint="Password"
    HintFloatMode="Float"
    HelperText="Must be at least 8 characters">
    <PasswordBox />
</inputLayout:SfTextInputLayout>

<!-- Better: Dynamic helper text based on state -->
<inputLayout:SfTextInputLayout 
    x:Name="passwordLayout"
    Hint="Password"
    HintFloatMode="Float"
    HelperText="{Binding PasswordHelperText}">
    <PasswordBox PasswordChanged="OnPasswordChanged"/>
</inputLayout:SfTextInputLayout>
```

```csharp
private void OnPasswordChanged(object sender, RoutedEventArgs e)
{
    var password = (sender as PasswordBox).Password;
    
    if (string.IsNullOrEmpty(password))
        PasswordHelperText = "Must be at least 8 characters";
    else if (password.Length < 8)
        PasswordHelperText = $"{8 - password.Length} more characters needed";
    else
        PasswordHelperText = "Password meets requirements ✓";
}
```

### 6. **Container Type Compatibility**

```csharp
// Float mode works best with different container types
public static class HintContainerGuidelines
{
    public static void ApplyOptimalSettings(
        SfTextInputLayout layout, 
        ContainerType containerType)
    {
        layout.ContainerType = containerType;
        
        switch (containerType)
        {
            case ContainerType.Outlined:
                // Float works great, hint moves to border
                layout.HintFloatMode = HintFloatMode.Float;
                break;
                
            case ContainerType.Filled:
                // Float provides good visual feedback
                layout.HintFloatMode = HintFloatMode.Float;
                break;
                
            case ContainerType.None:
                // None mode matches minimal aesthetic
                layout.HintFloatMode = HintFloatMode.None;
                break;
        }
    }
}
```

---

## Complete Examples

### Example 1: Registration Form (Float Mode)

```xaml
<StackPanel Width="350" Margin="20">
    <TextBlock Text="Create Account" 
               FontSize="24" 
               FontWeight="Bold" 
               Margin="0,0,0,20"/>
    
    <inputLayout:SfTextInputLayout 
        Hint="Full Name"
        HintFloatMode="Float"
        HelperText="First and last name"
        ContainerType="Outlined"
        Margin="0,0,0,16">
        <TextBox />
    </inputLayout:SfTextInputLayout>
    
    <inputLayout:SfTextInputLayout 
        Hint="Email Address"
        HintFloatMode="Float"
        HelperText="We'll never share your email"
        ContainerType="Outlined"
        Margin="0,0,0,16">
        <TextBox />
    </inputLayout:SfTextInputLayout>
    
    <inputLayout:SfTextInputLayout 
        Hint="Password"
        HintFloatMode="Float"
        HelperText="At least 8 characters"
        ContainerType="Outlined"
        Margin="0,0,0,16">
        <PasswordBox />
    </inputLayout:SfTextInputLayout>
</StackPanel>
```

### Example 2: Profile Edit Form (AlwaysFloat Mode)

```csharp
public void LoadUserProfile(UserProfile profile)
{
    var formPanel = new StackPanel { Width = 350 };
    
    // Name field
    var nameLayout = new SfTextInputLayout
    {
        Hint = "Display Name",
        HintFloatMode = HintFloatMode.AlwaysFloat,
        ContainerType = ContainerType.Filled,
        InputView = new TextBox { Text = profile.DisplayName }
    };
    formPanel.Children.Add(nameLayout);
    
    // Email field (read-only)
    var emailLayout = new SfTextInputLayout
    {
        Hint = "Email Address",
        HintFloatMode = HintFloatMode.AlwaysFloat,
        ContainerType = ContainerType.Filled,
        HelperText = "Cannot be changed",
        InputView = new TextBox 
        { 
            Text = profile.Email, 
            IsReadOnly = true 
        }
    };
    formPanel.Children.Add(emailLayout);
    
    // Bio field
    var bioLayout = new SfTextInputLayout
    {
        Hint = "Bio",
        HintFloatMode = HintFloatMode.AlwaysFloat,
        ContainerType = ContainerType.Filled,
        HelperText = "Tell us about yourself",
        InputView = new TextBox 
        { 
            Text = profile.Bio,
            AcceptsReturn = true,
            TextWrapping = TextWrapping.Wrap,
            MinHeight = 80
        }
    };
    formPanel.Children.Add(bioLayout);
}
```

### Example 3: Search Interface (None Mode)

```xaml
<Grid Background="#F5F5F5" Height="600">
    <StackPanel VerticalAlignment="Center" 
                HorizontalAlignment="Center"
                Width="500">
        
        <TextBlock Text="Search Database" 
                   FontSize="32" 
                   FontWeight="Light"
                   HorizontalAlignment="Center"
                   Margin="0,0,0,40"/>
        
        <inputLayout:SfTextInputLayout 
            Hint="Type to search..."
            HintFloatMode="None"
            ContainerType="None"
            HelperText="Press Enter to search"
            LeadingViewPosition="Inside">
            
            <TextBox FontSize="18" Padding="8"/>
            
            <inputLayout:SfTextInputLayout.LeadingView>
                <Label Text="🔍" FontSize="20"/>
            </inputLayout:SfTextInputLayout.LeadingView>
        </inputLayout:SfTextInputLayout>
    </StackPanel>
</Grid>
```

### Example 4: Dynamic Mode Selection

```csharp
public class SmartFormBuilder
{
    public SfTextInputLayout CreateField(
        string hint, 
        string defaultValue = null,
        bool isEditing = false)
    {
        var layout = new SfTextInputLayout
        {
            Hint = hint,
            ContainerType = ContainerType.Outlined,
            InputView = new TextBox()
        };
        
        // Smart mode selection
        if (isEditing && !string.IsNullOrEmpty(defaultValue))
        {
            // Pre-filled edit form: keep label visible
            layout.HintFloatMode = HintFloatMode.AlwaysFloat;
            (layout.InputView as TextBox).Text = defaultValue;
        }
        else if (!isEditing)
        {
            // New entry form: standard float behavior
            layout.HintFloatMode = HintFloatMode.Float;
        }
        else
        {
            // Empty edit form: standard float
            layout.HintFloatMode = HintFloatMode.Float;
        }
        
        return layout;
    }
}
```

---

## Mode Comparison Table

| Feature | Float | AlwaysFloat | None |
|---------|-------|-------------|------|
| **Animation** | Yes | No | Partial |
| **Space Efficiency (Empty)** | High | Medium | High |
| **Space Efficiency (Filled)** | Medium | Medium | High |
| **Constant Context** | No | Yes | No |
| **Material Design** | ✓ | Variant | ✗ |
| **Accessibility** | Good | Excellent | Fair |
| **Performance** | Medium | High | High |
| **Best for New Data** | ✓ | ✗ | ~ |
| **Best for Editing** | ~ | ✓ | ✗ |
| **Best for Minimal UI** | ~ | ✗ | ✓ |

---

## Related Topics

- [Customization](customization.md) - Styling hints with colors and fonts
- [Container Types](container-type.md) - How hints interact with different containers
- [Assistive Labels](assistive-labels.md) - Combining hints with helper text
- [Icons and Views](icons-and-views.md) - Adding visual elements alongside hints
- [Advanced Features](advanced-features.md) - RTL support for hint labels

## Common Pitfalls to Avoid

1. **Mixing modes inconsistently**: Use the same mode across similar forms.
2. **None mode without helper text**: Always provide alternative context.
3. **AlwaysFloat for empty new forms**: Wastes vertical space unnecessarily.
4. **Ignoring accessibility**: Test with screen readers for all modes.
5. **Animation overload**: Too many Float animations can be distracting.
6. **Wrong mode for use case**: Edit forms should typically use AlwaysFloat.

## Summary

The `HintFloatMode` property provides three distinct behaviors:
- **Float**: Standard material design with smooth animations
- **AlwaysFloat**: Constant labels for editing and pre-filled forms
- **None**: Minimal design for focused, simple interfaces

Choose the mode that best fits your form's purpose, data state, and design philosophy. When in doubt, Float mode is a safe, widely-accepted default.
