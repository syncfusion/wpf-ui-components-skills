# Advanced Theme Features

## Overview

Syncfusion WPF themes include advanced visual and interaction features that enhance the user experience. This document covers reveal animations, acrylic effects, keyboard focus indicators, ScrollBar modes, and touch support.

## Fluent Theme Reveal Animations

### What are Reveal Animations?

Reveal animations are interactive visual effects that respond to user input. They create visual feedback when users hover over or press controls, adding polish and interactivity to the UI.

Fluent theme includes two types of reveal effects:
1. **Hover Effect:** Activates when mouse hovers over a control
2. **Pressed Effect:** Activates when control is clicked or touched

### Hover Effect Configuration

#### HoverEffectMode Property

Control which parts of a control get the hover reveal effect using the `HoverEffectMode` property:

```csharp
using Syncfusion.SfSkinManager;

// In code-behind
var theme = new FluentTheme 
{ 
    ThemeName = "FluentDark",
    HoverEffectMode = HoverEffect.BackgroundAndBorder  // Default
};
SfSkinManager.SetTheme(this, theme);
```

#### HoverEffect Enum Values

| Value | Effect | Usage |
|-------|--------|-------|
| `HoverEffect.Background` | Reveals only the background area | When you want subtle hover feedback |
| `HoverEffect.BackgroundAndBorder` | Reveals both background and border (default) | Most common; provides clear feedback |
| `HoverEffect.Border` | Reveals only the border edge | For controls with prominent borders |
| `HoverEffect.None` | Disables hover reveal effect | To disable animations for performance |

#### XAML Configuration

Configure hover effect in XAML:

```xaml
<syncfusion:ChromelessWindow 
    xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentDark, FluentHoverEffectMode=Border}">
    <!-- Window content -->
</syncfusion:ChromelessWindow>
```

#### Code-Behind Configuration

Apply hover effect to a specific window:

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Create Fluent theme with custom hover effect
    var fluentTheme = new FluentTheme 
    { 
        ThemeName = "FluentDark",
        HoverEffectMode = HoverEffect.Border  // Only border reveals on hover
    };
    
    SfSkinManager.SetTheme(this, fluentTheme);
}
```

#### Visual Examples

**BackgroundAndBorder (Default):**
```
Normal state:       Hover state:
┌─────────┐        ┌═══════════┐
│ Button  │   →    ║ Button  ║
└─────────┘        └═══════════┘
(Subtle glow/light effect on background and border)
```

**Border Only:**
```
Normal state:       Hover state:
┌─────────┐        ═══════════
│ Button  │   →    ║ Button  ║
└─────────┘        ═══════════
(Only border glows; background unchanged)
```

**Background Only:**
```
Normal state:       Hover state:
┌─────────┐        ┌─────────┐
│ Button  │   →    │ Button  │  (subtle background glow)
└─────────┘        └─────────┘
```

### Pressed Effect Configuration

#### PressedEffectMode Property

Control the animation when a control is clicked or touched:

```csharp
using Syncfusion.SfSkinManager;

// In code-behind
var theme = new FluentTheme 
{ 
    ThemeName = "FluentDark",
    PressedEffectMode = PressedEffect.Reveal  // Default
};
SfSkinManager.SetTheme(this, theme);
```

#### PressedEffect Enum Values

| Value | Effect | When to Use |
|-------|--------|------------|
| `PressedEffect.Glow` | Creates a glowing effect on press | For subtle press feedback |
| `PressedEffect.Reveal` | Reveals light/highlight on press (default) | Most common; provides clear feedback |
| `PressedEffect.None` | Disables pressed effect | To disable animations or for custom behavior |

#### XAML Configuration

```xaml
<syncfusion:ChromelessWindow 
    xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentDark, FluentPressedEffectMode=Glow}">
    <!-- Window content -->
</syncfusion:ChromelessWindow>
```

#### Code-Behind Configuration

```csharp
public MainWindow()
{
    InitializeComponent();
    
    var fluentTheme = new FluentTheme 
    { 
        ThemeName = "FluentDark",
        PressedEffectMode = PressedEffect.Glow
    };
    
    SfSkinManager.SetTheme(this, fluentTheme);
}
```

#### Visual Examples

**Reveal (Default):**
```
Click:
┌─────────┐
║ Button  ║  (Light reveal/highlight effect)
└─────────┘
```

**Glow:**
```
Click:
◈═════════◈
║ Button  ║  (Outer glow effect around control)
◈═════════◈
```

### Complete Reveal Animation Example

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        SfSkinManager.ApplyThemeAsDefaultStyle = true;
        
        // Configure Fluent theme with specific animation effects
        var fluentTheme = new FluentTheme 
        { 
            ThemeName = "FluentDark",
            HoverEffectMode = HoverEffect.BackgroundAndBorder,  // Full reveal on hover
            PressedEffectMode = PressedEffect.Reveal,           // Reveal on press
            ShowAcrylicBackground = true                        // Enable acrylic effect
        };
        
        SfSkinManager.SetTheme(this, fluentTheme);
    }
}
```

## Acrylic Background Effect

### What is Acrylic Background?

The acrylic background creates a **transparent, blurred effect** behind windows, showing the content behind while applying a frosted-glass appearance. This is a signature Fluent Design element.

### Enabling Acrylic Background

#### Code-Behind

```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    
    var fluentTheme = new FluentTheme 
    { 
        ThemeName = "FluentDark",
        ShowAcrylicBackground = true  // Enable acrylic effect
    };
    
    SfSkinManager.SetTheme(this, fluentTheme);
}
```

#### XAML Configuration

Unfortunately, `ShowAcrylicBackground` cannot be set in XAML directly. Use code-behind approach.

### Visual Characteristics

**With Acrylic Off (Default):**
```
Window: Solid background color
┌──────────────────┐
│ Normal background│
│                  │
└──────────────────┘
```

**With Acrylic On:**
```
Window: Blurred transparent background
┌──────────────────┐
│ ≈≈≈ Blurred ≈≈≈  │  (Shows content behind with frosted effect)
│ ≈ desktop ≈      │
└──────────────────┘
```

### Acrylic Effect Considerations

- **Performance:** Acrylic effect uses GPU resources; may impact performance on lower-end systems
- **Visual Style:** Creates modern, premium appearance
- **Desktop Integration:** Blurs whatever is behind the window
- **Windows Requirement:** Acrylic is optimized for Windows 10 and later
- **Accessibility:** Ensure text contrast is sufficient when acrylic is enabled

### Combined Effects Example

```csharp
public MainWindow()
{
    InitializeComponent();
    
    SfSkinManager.ApplyThemeAsDefaultStyle = true;
    
    // Full Fluent experience: animations + acrylic
    var fluentTheme = new FluentTheme 
    { 
        ThemeName = "FluentDark",
        HoverEffectMode = HoverEffect.BackgroundAndBorder,
        PressedEffectMode = PressedEffect.Reveal,
        ShowAcrylicBackground = true
    };
    
    SfSkinManager.SetTheme(this, fluentTheme);
}
```

## Keyboard Focus Visuals

### Overview

Keyboard focus visuals provide **visual indicators** showing which control currently has focus when using keyboard navigation. This is critical for accessibility.

### High Visibility Keyboard Focus

The `HighVisibilityKeyboardFocusTheme` property controls the prominence of keyboard focus indicators:

```csharp
var theme = new FluentTheme 
{ 
    ThemeName = "FluentDark",
    HighVisibilityKeyboardFocusTheme = true  // Enable high visibility
};
```

### Visual Effect

**Standard Focus:**
```
┌─────────────────┐
│ ◯ Button        │
└─────────────────┘
(Subtle focus indicator, may be hard to see)
```

**High Visibility Focus:**
```
┌─────────────────┐
│ ⚈ Button ⚈      │  (Bold, colored focus rectangle)
└─────────────────┘
(Clear, obvious focus indicator)
```

### Best Practices

- Enable high visibility for accessibility-critical applications
- Test keyboard navigation with screen readers
- Ensure focus indicators have sufficient contrast (WCAG AA requirement: 4.5:1)
- Make sure focus indicators don't obscure critical information

## ScrollBar Modes

### ScrollBar Configuration

Different theme variants support different ScrollBar appearances.

### ScrollBarMode Options

```csharp
// Example: Windows 11 theme with specific ScrollBar mode
var theme = new Windows11LightTheme
{
    ScrollBarMode = ScrollBarMode.Native  // or Compact
};
```

### Available Modes

| Mode | Appearance | Use Case |
|------|-----------|----------|
| `ScrollBarMode.Native` | Full-size native-looking scrollbar | Traditional, familiar look |
| `ScrollBarMode.Compact` | Compact, minimal scrollbar | Modern, space-saving design |

### Visual Examples

**Native Mode:**
```
┌─────────┐
│ Content │
│ Content │
│ Content │
│ ──────│◇│  (Full-width scrollbar thumb)
│ Content │
└─────────┘
```

**Compact Mode:**
```
┌─────────┐
│ Content │
│ Content │  ↓
│ Content │─────  (Thin, minimal scrollbar)
│ Content │  ↓
│ Content │
└─────────┘
```

## Touch Support Configuration

### Overview

WPF applications can be optimized for touch input (tablets, touch screens) with enhanced touch-friendly controls and animations.

### Enable Touch Support

```csharp
// Touch support is generally enabled by default in modern themes
// For explicit configuration:
var theme = new FluentDarkTheme();
SfSkinManager.SetTheme(this, theme);

// WPF automatically provides touch support for:
// - Tap gestures (like click)
// - Pan/swipe gestures (scrolling)
// - Pinch/zoom gestures (scaling)
```

### Touch-Friendly Considerations

1. **Control Size:** Ensure buttons and interactive elements are ≥44x44 pixels (touch target minimum)
2. **Spacing:** Add adequate padding around clickable elements to prevent accidental activation
3. **Feedback:** Ensure hover and pressed effects provide clear feedback for touch interactions
4. **Animations:** Modern themes include smooth animations that enhance touch feel

### Example: Touch-Optimized Layout

```xaml
<Window>
    <StackPanel Spacing="16">
        <!-- Large touch targets with spacing -->
        <Button Content="Accept" Height="48" Padding="16"/>
        <Button Content="Reject" Height="48" Padding="16"/>
        <Button Content="Settings" Height="48" Padding="16"/>
    </StackPanel>
</Window>
```

## Feature Combination Examples

### Example 1: Modern Fluent UI

Optimal settings for modern, interactive application:

```csharp
public MainWindow()
{
    InitializeComponent();
    
    SfSkinManager.ApplyThemeAsDefaultStyle = true;
    
    var fluentTheme = new FluentDarkTheme 
    { 
        HoverEffectMode = HoverEffect.BackgroundAndBorder,
        PressedEffectMode = PressedEffect.Reveal,
        ShowAcrylicBackground = true,
        HighVisibilityKeyboardFocusTheme = false  // Modern, subtle focus
    };
    
    SfSkinManager.SetTheme(this, fluentTheme);
}
```

### Example 2: Accessibility-Focused Application

Prioritize accessibility with high visibility and keyboard support:

```csharp
public MainWindow()
{
    InitializeComponent();
    
    SfSkinManager.ApplyThemeAsDefaultStyle = true;
    
    // Use high-contrast theme with accessible features
    var officeTheme = new Office2019HighContrastTheme();
    
    var fluentTheme = new FluentLightTheme 
    { 
        HoverEffectMode = HoverEffect.BackgroundAndBorder,
        PressedEffectMode = PressedEffect.Reveal,
        ShowAcrylicBackground = false,  // No acrylic for clarity
        HighVisibilityKeyboardFocusTheme = true  // Bold focus indicators
    };
    
    SfSkinManager.SetTheme(this, fluentTheme);
}
```

### Example 3: Enterprise Application

Professional appearance with traditional scrollbars:

```csharp
public MainWindow()
{
    InitializeComponent();
    
    SfSkinManager.ApplyThemeAsDefaultStyle = true;
    
    var officeTheme = new Office2019ColorfulTheme
    {
        ScrollBarMode = ScrollBarMode.Native  // Traditional scrollbars
    };
    
    SfSkinManager.SetTheme(this, officeTheme);
}
```

### Example 4: Touch-Optimized Application

Ideal for tablet or touch-screen applications:

```xaml
<!-- XAML layout optimized for touch -->
<Window>
    <StackPanel Spacing="20">
        <!-- Large touch targets -->
        <Button Content="Start" Height="56" FontSize="18" Margin="20"/>
        <Button Content="Settings" Height="56" FontSize="18" Margin="20"/>
        <Button Content="Exit" Height="56" FontSize="18" Margin="20"/>
    </StackPanel>
</Window>
```

```csharp
// Code-behind with Fluent animations
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        SfSkinManager.ApplyThemeAsDefaultStyle = true;
        
        var fluentTheme = new FluentLightTheme 
        { 
            HoverEffectMode = HoverEffect.Background,
            PressedEffectMode = PressedEffect.Reveal
        };
        
        SfSkinManager.SetTheme(this, fluentTheme);
    }
}
```

## Troubleshooting Advanced Features

| Issue | Solution |
|-------|----------|
| Acrylic background not visible | Ensure Fluent theme is used; older Windows versions may not support acrylic |
| Hover animations not working | Check that `ApplyThemeAsDefaultStyle = true` is set |
| Focus rectangle not visible | Set `HighVisibilityKeyboardFocusTheme = true` for high-contrast focus |
| Poor touch responsiveness | Increase control sizes to ≥44x44 pixels; add spacing between controls |
| ScrollBar appears wrong | Verify theme supports ScrollBarMode property; not all themes have this option |
| Performance issues with animations | Disable reveal effects by setting `HoverEffectMode = HoverEffect.None` |
