# Placement and Animations

## Table of Contents
- [Overview](#overview)
- [Toast Placement](#toast-placement)
- [Animation Types](#animation-types)
- [Combining Placement and Animations](#combining-placement-and-animations)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

Toast notifications can be positioned at specific locations on the screen/window and animated with smooth transitions. These features enhance the user experience by providing predictable positioning and visually appealing effects.

**Key Concepts:**
- **Placement:** Determines where the toast appears (8 positions available)
- **ShowAnimationType:** Animation when toast appears
- **CloseAnimationType:** Animation when toast disappears

---

## Toast Placement

### Available Placement Options

The `Placement` property controls where toasts appear on the screen or within the window.

| Placement | Description | Best For |
|-----------|-------------|----------|
| **TopLeft** | Top-left corner | Less intrusive, secondary notifications |
| **TopCenter** | Centered at top | Important alerts, validation messages |
| **TopRight** | Top-right corner | Default, general notifications |
| **LeftCenter** | Vertically centered on left | Navigation-related messages |
| **RightCenter** | Vertically centered on right | Status updates, progress indicators |
| **BottomLeft** | Bottom-left corner | Non-intrusive updates |
| **BottomCenter** | Centered at bottom | Form validation, input feedback |
| **BottomRight** | Bottom-right corner | Chat messages, system notifications |

### Top Placements

```csharp
// Top-Left corner
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Top Left",
    Message = "Notification at top-left corner.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopLeft
});

// Top-Center
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Important Alert",
    Message = "This is centered at the top for maximum visibility.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopCenter,
    Severity = ToastSeverity.Warning
});

// Top-Right corner (default)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Top Right",
    Message = "Default notification position.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopRight
});
```

### Center Placements

```csharp
// Left-Center
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Navigation",
    Message = "Menu opened.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.LeftCenter
});

// Right-Center
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Status Update",
    Message = "Synchronization in progress...",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.RightCenter
});
```

### Bottom Placements

```csharp
// Bottom-Left corner
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Low Priority",
    Message = "Background task completed.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.BottomLeft
});

// Bottom-Center
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Form Validation",
    Message = "Please correct the errors before submitting.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.BottomCenter,
    Severity = ToastSeverity.Error
});

// Bottom-Right corner
SfToastNotification.Show(this, new ToastOptions
{
    Title = "New Message",
    Message = "You have a new chat message.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.BottomRight
});
```

### Placement Recommendations by Use Case

| Use Case | Recommended Placement | Reason |
|----------|----------------------|---------|
| **Form validation** | BottomCenter or TopCenter | Near submit button or form |
| **Success confirmations** | TopRight or BottomRight | Traditional notification position |
| **Critical errors** | TopCenter | Maximum visibility |
| **Chat messages** | BottomRight | Familiar pattern |
| **Status updates** | RightCenter | Non-intrusive, visible |
| **Navigation feedback** | LeftCenter | Near navigation area |

---

## Animation Types

### Available Animations

| Animation | In Version | Out Version | Effect |
|-----------|-----------|-------------|--------|
| **None** | None | None | No animation, instant appear/disappear |
| **Fade** | FadeIn | FadeOut | Gradual opacity change |
| **Zoom** | FadeZoomIn | FadeZoomOut | Fade + scale effect |
| **Slide Bottom** | SlideBottomIn | SlideBottomOut | Slide from/to bottom |
| **Flip Left Down** | FlipLeftDownIn | FlipLeftDownOut | 3D flip effect from left-down |
| **Flip Left Up** | FlipLeftUpIn | FlipLeftUpOut | 3D flip effect from left-up |
| **Flip Right Down** | FlipRightDownIn | FlipRightDownOut | 3D flip effect from right-down |

### Fade Animations

```csharp
// Simple fade in/out
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Fade Effect",
    Message = "This toast fades in and out.",
    Mode = ToastMode.Screen,
    ShowAnimationType = ToastAnimation.FadeIn,
    CloseAnimationType = ToastAnimation.FadeOut
});
```

**Characteristics:**
- Smooth, subtle effect
- Good for non-intrusive notifications
- Universal, works well in all placements

### Zoom Animations

```csharp
// Fade + Zoom effect
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Zoom Effect",
    Message = "This toast zooms in with fade.",
    Mode = ToastMode.Screen,
    ShowAnimationType = ToastAnimation.FadeZoomIn,
    CloseAnimationType = ToastAnimation.FadeZoomOut,
    Placement = ToastPlacement.TopCenter
});
```

**Characteristics:**
- More attention-grabbing than fade
- Scales from center while fading
- Good for important notifications

### Slide Animations

```csharp
// Slide from bottom
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Slide Effect",
    Message = "This toast slides from the bottom.",
    Mode = ToastMode.Screen,
    ShowAnimationType = ToastAnimation.SlideBottomIn,
    CloseAnimationType = ToastAnimation.SlideBottomOut,
    Placement = ToastPlacement.BottomRight
});
```

**Characteristics:**
- Directional movement
- Natural for bottom placements
- Mobile-friendly pattern

### Flip Animations

```csharp
// Flip Left Down
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Flip Effect",
    Message = "This toast flips in from left-down.",
    Mode = ToastMode.Screen,
    ShowAnimationType = ToastAnimation.FlipLeftDownIn,
    CloseAnimationType = ToastAnimation.FlipLeftDownOut
});

// Flip Left Up
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Flip Effect",
    Message = "This toast flips in from left-up.",
    Mode = ToastMode.Screen,
    ShowAnimationType = ToastAnimation.FlipLeftUpIn,
    CloseAnimationType = ToastAnimation.FlipLeftUpOut
});

// Flip Right Down
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Flip Effect",
    Message = "This toast flips in from right-down.",
    Mode = ToastMode.Screen,
    ShowAnimationType = ToastAnimation.FlipRightDownIn,
    CloseAnimationType = ToastAnimation.FlipRightDownOut
});
```

**Characteristics:**
- 3D rotation effect
- More dramatic, playful
- Use sparingly for special notifications

### No Animation

```csharp
// Instant appear/disappear
SfToastNotification.Show(this, new ToastOptions
{
    Title = "No Animation",
    Message = "This toast appears instantly.",
    Mode = ToastMode.Screen,
    ShowAnimationType = ToastAnimation.None,
    CloseAnimationType = ToastAnimation.None
});
```

**When to Use:**
- Performance-critical scenarios
- Rapid successive notifications
- Accessibility concerns (some users prefer reduced motion)

---

## Combining Placement and Animations

### Pattern 1: Top-Center with Slide

```csharp
// Slide down from top for validation errors
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Validation Error",
    Message = "Please fill in all required fields.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopCenter,
    Severity = ToastSeverity.Error,
    ShowAnimationType = ToastAnimation.SlideBottomIn,  // Slide from top
    CloseAnimationType = ToastAnimation.SlideBottomOut
});
```

### Pattern 2: Bottom-Right with Fade

```csharp
// Fade in at bottom-right for status updates
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Update",
    Message = "New version available.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.BottomRight,
    Severity = ToastSeverity.Info,
    ShowAnimationType = ToastAnimation.FadeIn,
    CloseAnimationType = ToastAnimation.FadeOut
});
```

### Pattern 3: Top-Center with Zoom (Attention-Grabbing)

```csharp
// Zoom in at center for critical alerts
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Critical Warning",
    Message = "Your session will expire in 2 minutes.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopCenter,
    Severity = ToastSeverity.Warning,
    Variant = ToastVariant.Filled,
    ShowAnimationType = ToastAnimation.FadeZoomIn,
    CloseAnimationType = ToastAnimation.FadeZoomOut,
    Duration = TimeSpan.FromSeconds(10)
});
```

### Pattern 4: Contextual Placement with Flip

```csharp
// Flip in from corner for playful notifications
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Achievement Unlocked!",
    Message = "You've completed 100 tasks.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.BottomLeft,
    Severity = ToastSeverity.Success,
    ShowAnimationType = ToastAnimation.FlipLeftUpIn,
    CloseAnimationType = ToastAnimation.FlipLeftUpOut
});
```

### Animation Recommendations by Placement

| Placement | Recommended Animation | Reason |
|-----------|----------------------|---------|
| **TopCenter** | SlideBottomIn/Out or FadeZoomIn/Out | Natural downward motion |
| **BottomCenter** | SlideBottomIn/Out | Slides up from bottom |
| **TopRight/TopLeft** | FadeIn/Out | Subtle corner appearance |
| **BottomRight/BottomLeft** | FadeIn/Out or FlipIn/Out | Smooth corner entry |
| **LeftCenter/RightCenter** | FadeIn/Out | Balanced side appearance |

---

## Edge Cases and Troubleshooting

### Issue: Placement Not Working

**Problem:** Toast appears at default position despite setting Placement.

**Cause:** Mode is set to Default (native OS notifications don't support custom placement).

**Solution:**
```csharp
// WRONG: Placement with Default mode (ignored)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Test",
    Mode = ToastMode.Default,              // ❌ Native mode
    Placement = ToastPlacement.TopLeft     // Ignored
});

// CORRECT: Use Window or Screen mode
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Test",
    Mode = ToastMode.Screen,               // ✅ Screen mode
    Placement = ToastPlacement.TopLeft     // Applied
});
```

### Issue: Animations Not Visible

**Problem:** Toast appears/disappears instantly despite animation settings.

**Possible Causes:**
1. Mode is set to Default
2. System has animations disabled
3. ShowAnimationType/CloseAnimationType set to None

**Solution:**
```csharp
// Ensure proper mode and animation types
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Animated",
    Mode = ToastMode.Screen,                    // ✅ Use Screen/Window mode
    ShowAnimationType = ToastAnimation.FadeIn,  // ✅ Not None
    CloseAnimationType = ToastAnimation.FadeOut // ✅ Not None
});
```

### Issue: Toast Blocked by UI Elements

**Problem:** Toast appears but is hidden behind other UI elements.

**Solution:** Choose appropriate placement that doesn't overlap critical UI:

```csharp
// If top-right is blocked by toolbar, use bottom-right
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Alternative Position",
    Message = "Positioned to avoid UI overlap.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.BottomRight  // Alternative position
});
```

### Issue: Multiple Toasts Overlapping

**Problem:** Multiple toasts stack on top of each other at same position.

**Solution:** Use different placements or stagger timing:

```csharp
// Different placements for multiple toasts
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Notification 1",
    Placement = ToastPlacement.TopRight
});

SfToastNotification.Show(this, new ToastOptions
{
    Title = "Notification 2",
    Placement = ToastPlacement.BottomRight  // Different position
});

// OR: Close previous before showing new
SfToastNotification.CloseAll();
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Latest Notification"
});
```

### Issue: Animation Too Fast/Slow

**Problem:** Animation duration doesn't match expectations.

**Note:** Animation speed is controlled by the control's internal timing. You can't directly adjust it, but you can:

**Workaround:**
```csharp
// Use different animation types for perceived speed difference
// Fade feels faster than Slide
// Zoom feels faster than Flip

// Faster feel: Fade
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Quick",
    ShowAnimationType = ToastAnimation.FadeIn
});

// Slower feel: Flip
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Slower",
    ShowAnimationType = ToastAnimation.FlipRightDownIn
});
```

## Best Practices

1. **Match animation to severity:**
   - Errors/Warnings: Zoom or Fade (attention-grabbing)
   - Info/Success: Fade or Slide (subtle)

2. **Consider placement context:**
   - Bottom placements: Use Slide animations
   - Corner placements: Use Fade or Flip
   - Center placements: Use Zoom for emphasis

3. **Be consistent:**
   - Use same placement for similar notification types
   - Use same animations throughout app for consistency

4. **Accessibility:**
   - Provide option to disable animations for users sensitive to motion
   - Don't rely solely on animation for critical information

5. **Performance:**
   - Use None animation for rapid, successive notifications
   - Limit number of simultaneous animated toasts

6. **Test visibility:**
   - Ensure toasts don't overlap critical UI elements
   - Test all placements in your application layout
