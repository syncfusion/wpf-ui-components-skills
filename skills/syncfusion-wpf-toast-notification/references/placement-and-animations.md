# Placement and Animations

## Table of Contents
- [Overview](#overview)
- [Toast Placement](#toast-placement)
- [Animation Types](#animation-types)
- [Combining Placement and Animations](#combining-placement-and-animations)

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

### Placement Example

```csharp
// Set Placement property to position toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Notification",
    Message = "Positioned at specific location.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopCenter  // or other positions
});
```

### Placement Recommendations

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

### Flip Animations

Use FlipLeftDownIn/Out, FlipLeftUpIn/Out, or FlipRightDownIn/Out for 3D flip effects.

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

Combine `Placement` and animation properties for customized appearance. Match animations to placement (SlideBottomIn/Out for top/bottom, FadeIn/Out for sides).



### Animation Recommendations by Placement

| Placement | Recommended Animation | Reason |
|-----------|----------------------|---------|
| **TopCenter** | SlideBottomIn/Out or FadeZoomIn/Out | Natural downward motion |
| **BottomCenter** | SlideBottomIn/Out | Slides up from bottom |
| **TopRight/TopLeft** | FadeIn/Out | Subtle corner appearance |
| **BottomRight/BottomLeft** | FadeIn/Out or FlipIn/Out | Smooth corner entry |
| **LeftCenter/RightCenter** | FadeIn/Out | Balanced side appearance |

---

## Best Practices

1. **Match animation to severity:** Errors/Warnings use attention-grabbing animations (Zoom, Fade); Info/Success use subtle (Fade, Slide)
2. **Be consistent:** Use same placement and animations throughout app for predictable UX
3. **Test visibility:** Ensure toasts don't overlap critical UI elements
4. **Performance:** Use None animation for rapid successive notifications; limit simultaneous animated toasts
