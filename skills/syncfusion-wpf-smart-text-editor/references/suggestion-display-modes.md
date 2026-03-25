# Suggestion Display Modes

## Overview

The WPF Smart Text Editor supports two display modes for showing AI-powered completions as you type: **Inline** and **Popup**. Each mode offers distinct advantages depending on the platform, user interaction pattern, and application context.

**Available Modes:**
- **Inline:** Renders the predicted text in place after the caret, matching your text style
- **Popup:** Shows a compact hint near the caret that you can tap or accept via key press

**Acceptance Methods:**
- **Tab key:** Accepts suggestions in both Inline and Popup modes 
- **Right Arrow key:** Accepts suggestions in both Inline and Popup modes
- **Tap/Click:** Accepts suggestions in Popup mode only

## Inline Suggestion Mode

### Overview

Inline mode displays the suggested text directly within the editor, seamlessly continuing your typing flow. The suggestion appears after the caret position, styled to differentiate it from user-entered text (typically lighter or grayed out).

**When to Use Inline Mode:**
- Desktop environments (Windows) where keyboard input is primary
- Scenarios requiring uninterrupted typing flow
- Applications where visual continuity is important
- Users comfortable with keyboard shortcuts
- Professional writing, coding, or content creation tools

**Advantages:**
- Natural typing experience
- No visual interruption or overlay
- Suggestions appear directly in context
- Ideal for rapid keyboard input

**Considerations:**
- May be less discoverable for users unfamiliar with the feature
- Tab key acceptance not supported on Android/iOS
- Works best with keyboard-driven workflows

### Implementation in XAML

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:smarttexteditor="clr-namespace:Syncfusion.UI.Xaml.SmartComponents;assembly=Syncfusion.SfSmartComponents.WPF">

    <smarttexteditor:SfSmartTextEditor
        Placeholder="Start typing..."
        UserRole="Email author responding to inquiries"
        SuggestionDisplayMode="Inline" />
</Window>
```

### Implementation in C#

```csharp
using Syncfusion.UI.Xaml.SmartComponents;

var smartTextEditor = new SfSmartTextEditor
{
    Placeholder = "Start typing...",
    UserRole = "Email author responding to inquiries",
    SuggestionDisplayMode = SuggestionDisplayMode.Inline
};
```

### Accepting Inline Suggestions

**Keyboard Methods:**
1. **Tab Key:** Press Tab to accept the entire suggestion
2. **Right Arrow Key:** Press Right Arrow to accept the entire suggestion

**Visual Appearance:**
- Suggestion text appears inline after the caret
- Typically styled with a lighter color or reduced opacity
- Seamlessly integrates with existing text

## Popup Suggestion Mode

### Overview

Popup mode displays the suggested text in a small, compact overlay near the caret position. The popup appears above or below the current line and can be accepted by tapping (touch) or using keyboard shortcuts.

**When to Use Popup Mode:**
- Touch-based devices (tablets, touch screens)
- Applications with mixed input methods (touch + keyboard)
- Scenarios where inline text might cause confusion
- Users who prefer explicit suggestion visibility

**Advantages:**
- More discoverable for users (clear visual indicator)
- Natural for touch interactions (tap to accept)
- Doesn't interfere with inline text appearance
- Works well on mobile and tablet devices

**Considerations:**
- Adds a visual overlay that may distract some users
- Requires additional screen space for the popup
- May feel less integrated than inline mode

### Implementation in XAML

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:smarttexteditor="clr-namespace:Syncfusion.UI.Xaml.SmartComponents;assembly=Syncfusion.SfSmartComponents.WPF">

    <smarttexteditor:SfSmartTextEditor
        Placeholder="Start typing..."
        UserRole="Email author responding to inquiries"
        SuggestionDisplayMode="Popup" />
</Window>
```

### Implementation in C#

```csharp
using Syncfusion.UI.Xaml.SmartComponents;

var smartTextEditor = new SfSmartTextEditor
{
    Placeholder = "Start typing...",
    UserRole = "Email author responding to inquiries",
    SuggestionDisplayMode = SuggestionDisplayMode.Popup
};
```

### Accepting Popup Suggestions

**Multiple Acceptance Methods:**
1. **Tab Key:** Press Tab to accept the suggestion
2. **Right Arrow Key:** Press Right Arrow to accept the suggestion
3. **Tap/Click:** Tap (touch) or click (mouse) directly on the popup to accept

**Visual Appearance:**
- Suggestion appears in a small popup/tooltip near the caret
- Typically positioned above or below the current line
- Contains the suggested text in a compact format
- May include visual indicators (arrows, borders)

## Choosing the Right Mode

### Decision Guide

| Factor | Inline Mode | Popup Mode |
|--------|-------------|------------|
| **Primary Input** | Keyboard | Touch/Mixed |
| **User Experience** | Seamless, integrated | Explicit, discoverable |
| **Visual Impact** | Minimal | Moderate (overlay) |
| **Discoverability** | Lower | Higher |
| **Best For** | Professional writers, developers | General users, mobile apps |

### Use Inline Mode When:
- Building desktop applications for Windows or Mac
- Users primarily interact via keyboard
- Uninterrupted typing flow is critical
- Application targets professional or power users
- Visual simplicity is preferred

### Use Popup Mode When:
- Building mobile or touch-first applications
- Users interact via touch or mixed input
- Suggestion visibility and discoverability are important
- Application targets general or casual users
- Need explicit visual feedback for suggestions

## Best Practices

### 1. Match Input Method to Display Mode
- Keyboard-heavy workflows → Inline mode
- Touch-heavy workflows → Popup mode

### 2. Consider User Familiarity
- First-time users → Popup mode (more discoverable)
- Expert users → Inline mode (faster workflow)

### 3. Test on Target Platforms
- Verify keyboard shortcuts work as expected
- Test touch interactions on actual devices
- Ensure popup positioning doesn't obscure content

### 4. Provide Visual Feedback
- Style suggestions to be clearly distinguishable
- Use appropriate colors and opacity
- Maintain readability and contrast

### 5. Allow User Preferences
- Consider letting users toggle between modes
- Provide settings or preferences UI
- Remember user's choice across sessions

---

**Related Topics:**
- Customize suggestion appearance: [customization.md](customization.md)
- Configure AI services: [ai-service-configuration.md](ai-service-configuration.md)
- Initial setup: [getting-started.md](getting-started.md)
