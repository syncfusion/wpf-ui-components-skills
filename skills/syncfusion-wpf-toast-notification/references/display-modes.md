# Display Modes

## Table of Contents
- [Overview](#overview)
- [Default Mode (Native OS Notifications)](#default-mode-native-os-notifications)
- [Window Mode](#window-mode)
- [Screen Mode](#screen-mode)
- [Mode Comparison](#mode-comparison)
- [Choosing the Right Mode](#choosing-the-right-mode)

## Overview

SfToastNotification supports three distinct display modes, each suited for different scenarios:

1. **Default Mode:** Native Windows OS notifications (system-level)
2. **Window Mode:** In-app notifications constrained to window boundaries
3. **Screen Mode:** Global overlay notifications across the screen

The mode determines where toasts appear, their level of customization, and how they interact with the application.

---

## Default Mode (Native OS Notifications)

Default mode uses the native Windows operating system toast notification system. These are the same notifications you see from Windows itself and other system applications.

### Application Startup Configuration

Initialize `WindowsToastBootstrapper` in App.xaml.cs Application_Startup:

```csharp
private void Application_Startup(object sender, StartupEventArgs e)
{
    WindowsToastBootstrapper.RemoveShortcutOnUnload = true;
    WindowsToastBootstrapper.Initialize("AppId", "DisplayName");  // Unique ID and display name
}
```

### Using Default Mode

```csharp
// Show native OS toast notification
SfToastNotification.Show(this, new ToastOptions
{
    Title = "System Notification",
    Message = "This appears as a native Windows notification.",
    Mode = ToastMode.Default  // Native OS notification
});
```

### Default Mode Characteristics

**Advantages:** OS integration, persists in Action Center, visible when minimized, system-level visibility.

**Limitations:** No customization (colors, animations, placement). Only Title and Message supported. Requires Windows 10+.

### Native Mode Action Buttons (Limited)

```csharp
// Native toast with basic action buttons
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Update Available",
    Message = "A new version is available for download.",
    Mode = ToastMode.Default,
    Actions = new List<ToastAction>
    {
        new ToastAction("Install"),
        new ToastAction("Later")
    }
});
```

**Note:** Action buttons in Default mode have limited styling and functionality compared to Window/Screen modes.

---

## Window Mode

Window mode displays toast notifications within the application window boundaries. Toasts are constrained to the window and respect its activation state.

### Basic Usage

```csharp
// Show toast within window bounds
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Window Toast",
    Message = "This notification appears within the window boundaries.",
    Mode = ToastMode.Window
});
```

### Window Mode Characteristics

**Best for:** Application-specific feedback with full customization (severity, variants, colors, animations). Constrained to window boundaries. Ideal for form validation and context-aware notifications.

### Window Mode with Full Customization

```csharp
// Fully customized window toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Save Successful",
    Header = "File Operations",
    Message = "Your document has been saved to the cloud.",
    Mode = ToastMode.Window,
    Severity = ToastSeverity.Success,
    Variant = ToastVariant.Filled,
    Placement = ToastPlacement.BottomRight,
    ShowAnimationType = ToastAnimation.SlideBottomIn,
    CloseAnimationType = ToastAnimation.SlideBottomOut,
    Duration = TimeSpan.FromSeconds(5)
});
```

---

## Screen Mode

Screen mode displays custom toast overlays globally across the screen, visible regardless of window state or focus.

### Basic Usage

```csharp
// Show global screen overlay toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Global Notification",
    Message = "This notification appears globally on screen.",
    Mode = ToastMode.Screen
});
```

### Screen Mode Characteristics

**Best for:** Critical application-wide events visible even when app is minimized. Supports full customization. Use for background task completions and important alerts requiring immediate attention.

### Screen Mode with Custom Styling

```csharp
// Global toast with custom appearance
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Background Task Complete",
    Header = "System Status",
    Message = "Data synchronization completed successfully.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,
    Variant = ToastVariant.Outlined,
    AccentBrush = new SolidColorBrush(Colors.DodgerBlue),
    Placement = ToastPlacement.TopCenter,
    ShowAnimationType = ToastAnimation.FadeZoomIn,
    CloseAnimationType = ToastAnimation.FadeZoomOut,
    Duration = TimeSpan.FromSeconds(8)
});
```

---

## Mode Comparison

| Feature | Default (Native) | Window | Screen |
|---------|------------------|--------|--------|
| **Display Location** | OS notification area | Within window bounds | Global screen overlay |
| **Visible When Minimized** | ✅ Yes | ❌ No | ✅ Yes |
| **Persists in Action Center** | ✅ Yes | ❌ No | ❌ No |
| **Severity Styling** | ❌ No | ✅ Yes | ✅ Yes |
| **Custom Variants** | ❌ No | ✅ Yes | ✅ Yes |
| **AccentBrush** | ❌ No | ✅ Yes | ✅ Yes |
| **Custom Placement** | ❌ No | ✅ Yes | ✅ Yes |
| **Custom Animations** | ❌ No | ✅ Yes | ✅ Yes |
| **Template Customization** | ❌ No | ✅ Yes | ✅ Yes |
| **Action Buttons** | ⚠️ Limited | ✅ Full support | ✅ Full support |
| **Header Property** | ❌ Ignored | ✅ Supported | ✅ Supported |
| **System Integration** | ✅ Full | ❌ No | ❌ No |
| **Requires Bootstrapper** | ✅ Yes | ❌ No | ❌ No |
| **Windows 10+ Only** | ✅ Yes | ❌ Any Windows | ❌ Any Windows |

---

## Choosing the Right Mode

| Requirement | Mode |
|---|---|
| Native OS integration + persistent | **Default** (Windows 10+) |
| Context-specific + fully customized | **Window** (constrained to window) |
| Application-wide + visible when minimized | **Screen** (global overlay) |

**Best Practices:** Default Mode requires WindowsToastBootstrapper initialization. Window Mode best for form validation. Screen Mode best for critical, time-sensitive notifications—use sparingly.
