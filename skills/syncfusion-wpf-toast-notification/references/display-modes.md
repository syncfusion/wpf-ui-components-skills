# Display Modes

## Table of Contents
- [Overview](#overview)
- [Default Mode (Native OS Notifications)](#default-mode-native-os-notifications)
- [Window Mode](#window-mode)
- [Screen Mode](#screen-mode)
- [Mode Comparison](#mode-comparison)
- [Choosing the Right Mode](#choosing-the-right-mode)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

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

**CRITICAL:** Default mode requires initialization at application startup using `WindowsToastBootstrapper`.

#### Step 1: Configure App.xaml

Add the `Startup` event handler in your `App.xaml`:

```xml
<Application x:Class="ToastDemo.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             StartupUri="MainWindow.xaml"
             Startup="Application_Startup">
    <Application.Resources>
    </Application.Resources>
</Application>
```

#### Step 2: Initialize in App.xaml.cs

Import the namespace and initialize the bootstrapper in the `Application_Startup` event:

```csharp
using System.Windows;
using Syncfusion.UI.Xaml.SfToastNotification;

namespace ToastDemo
{
    public partial class App : Application
    {
        private void Application_Startup(object sender, StartupEventArgs e)
        {
            // Initialize the Toast Notification bootstrapper
            WindowsToastBootstrapper.RemoveShortcutOnUnload = true;
            WindowsToastBootstrapper.Initialize("ToastDemo.App", "ToastDemo");
        }
    }
}
```

**Parameters Explained:**
- **First parameter:** Application ID (unique identifier for your app)
- **Second parameter:** Display name (shown in Windows notification settings)
- **RemoveShortcutOnUnload:** When `true`, removes the app shortcut when app closes

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

**Advantages:**
- Integrates with Windows notification center
- Persists in Action Center even after auto-dismiss
- System-wide visibility (visible even when app is minimized)
- Familiar user experience (consistent with OS)
- Can trigger even when app is not in focus

**Limitations:**
- ⚠️ **NO customization supported**
- No control over appearance (colors, styles, variants)
- No custom animations
- No custom placement
- Limited action button support
- Only Title and Message properties work (Header is ignored)

**System Requirements:**
- Windows 10 or higher
- Application must be installed or have a Start Menu shortcut

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

**Advantages:**
- Full customization support (severity, variants, colors)
- Stays within application context
- No system-level permissions needed
- Complete control over appearance and behavior
- Supports all template customizations

**Behavior:**
- Toast only visible when window is visible
- Constrained to window boundaries
- Respects window activation state
- Ideal for application-specific feedback
- Doesn't appear in Windows Action Center

**When to Use:**
- Application-specific status updates
- Form validation messages
- Context-aware notifications
- When you need full customization
- When notifications should stay within app context

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

**Advantages:**
- Global screen-level display
- Visible regardless of window state (minimized, background)
- Full customization capabilities
- Application-wide notifications
- Best for important events

**Behavior:**
- Appears as overlay on screen
- Not constrained by window boundaries
- Visible even when app is minimized
- Stays on top of other windows
- Supports all customization options

**When to Use:**
- Critical application-wide events
- Background task completions
- System-level app notifications
- Important alerts requiring immediate attention
- When notifications should be visible even if app is not focused

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

### Use Default Mode When:
- You want **native OS integration**
- Notifications should appear in **Windows Action Center**
- App should send notifications even when **minimized or closed**
- You need **system-level notifications**
- Customization is not important
- Example: Email client notifying about new messages

### Use Window Mode When:
- Notifications are **context-specific** to the window
- You need **full visual customization**
- Toasts should **stay within application boundaries**
- Application is always in focus when notifications appear
- Example: Form validation errors, in-app status updates

### Use Screen Mode When:
- Notifications are **application-wide but not system-level**
- You need toasts visible **even when window is minimized**
- **Full customization** is required
- Important events need immediate attention
- Example: Background task completion, critical alerts

### Decision Flow

```
Need native OS integration?
├─ YES → Use Default Mode
└─ NO → Need visible when minimized?
    ├─ YES → Use Screen Mode
    └─ NO → Use Window Mode
```

---

## Edge Cases and Troubleshooting

### Issue: Default Mode Toast Not Appearing

**Cause:** WindowsToastBootstrapper not initialized.

**Solution:**
```csharp
// Ensure bootstrapper is initialized in App.xaml.cs
private void Application_Startup(object sender, StartupEventArgs e)
{
    WindowsToastBootstrapper.RemoveShortcutOnUnload = true;
    WindowsToastBootstrapper.Initialize("YourApp.Id", "YourAppName");
}
```

### Issue: Default Mode Customization Not Working

**Problem:** User tries to set Severity, Variant, or AccentBrush in Default mode.

**Solution:** These properties are not supported in Default mode. Switch to Window or Screen mode:

```csharp
// WRONG: Customization in Default mode (ignored)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Test",
    Mode = ToastMode.Default,
    Severity = ToastSeverity.Success,  // ❌ Ignored
    Variant = ToastVariant.Filled      // ❌ Ignored
});

// CORRECT: Use Screen or Window mode for customization
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Test",
    Mode = ToastMode.Screen,           // ✅ Screen mode
    Severity = ToastSeverity.Success,  // ✅ Applied
    Variant = ToastVariant.Filled      // ✅ Applied
});
```

### Issue: Window Mode Toast Hidden Behind Other Windows

**Problem:** Toast not visible when window is minimized or behind other windows.

**Solution:** Use Screen mode instead:

```csharp
// Switch from Window to Screen mode
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Important",
    Message = "This needs to be visible even when app is in background.",
    Mode = ToastMode.Screen  // Use Screen instead of Window
});
```

### Issue: Screen Mode Toast Overlapping

**Problem:** Multiple Screen mode toasts overlap in the same position.

**Solution:** Use different placements or close previous toasts:

```csharp
// Option 1: Use different placement
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Notification 1",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopRight
});

SfToastNotification.Show(this, new ToastOptions
{
    Title = "Notification 2",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.BottomRight  // Different position
});

// Option 2: Close previous before showing new
SfToastNotification.CloseAll();
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Latest Notification",
    Mode = ToastMode.Screen
});
```

## Best Practices by Mode

### Default Mode Best Practices
1. Always initialize WindowsToastBootstrapper at app startup
2. Keep Title and Message concise (limited space)
3. Don't rely on advanced customization
4. Test on Windows 10+ only
5. Use for system-level, persistent notifications

### Window Mode Best Practices
1. Ensure window is visible and active
2. Use for context-specific feedback
3. Take advantage of full customization
4. Position appropriately within window (avoid corners if content is there)
5. Consider user's current focus area

### Screen Mode Best Practices
1. Use sparingly (can be intrusive)
2. Reserve for important, time-sensitive notifications
3. Choose appropriate placement (avoid blocking critical screen areas)
4. Customize appearance for better visibility
5. Provide clear actions or auto-close timing
