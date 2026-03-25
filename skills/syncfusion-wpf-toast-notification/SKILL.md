---
name: syncfusion-wpf-toast-notification
description: Guide for implementing Syncfusion WPF Toast Notification (SfToastNotification) control for displaying temporary notification messages. Use this skill when implementing toast notifications, pop-up alerts, status messages, or non-intrusive notifications in WPF applications. Covers showing success/error/warning messages, notification systems, and temporary UI feedback without blocking the main window.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Toast Notifications in WPF

## When to Use This Skill

Use this skill whenever the user needs to:
- Display **temporary, non-intrusive notifications** in WPF applications
- Show **status messages** (save successful, operation completed, errors)
- Implement **notification alerts** that auto-dismiss after a timeout
- Create **native Windows OS notifications** or custom in-app toasts
- Display **severity-based messages** (Info, Success, Warning, Error)
- Add **interactive action buttons** to notifications (Reply, Undo, OK)
- Show **transient feedback** that doesn't block user workflow
- Implement **global or window-scoped notifications**

## Component Overview

**SfToastNotification** is a **non-UI control** (no XAML required) that displays toast notifications in WPF applications. It supports three display modes:
- **Default Mode:** Native Windows OS notifications (Windows 10+)
- **Window Mode:** In-app notifications constrained to window boundaries
- **Screen Mode:** Global overlay notifications across the screen

**Key Characteristics:**
- Code-only implementation (no XAML markup needed)
- Static API for showing/closing toasts
- Multiple severity levels with built-in visual styling
- Customizable animations, placement, and templates
- Interactive action buttons with callbacks
- Full template customization support

---

## Documentation and Navigation Guide

### Getting Started & Core Concepts
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet installation
- Non-UI control architecture
- Basic toast with `SfToastNotification.Show()` method
- Title, Message, and Header properties
- Duration configuration and auto-close behavior
- Toast lifecycle management (Close, CloseAll)
- Action buttons and close button visibility
- When to read: User needs basic setup or first-time implementation

### Display Modes
📄 **Read:** [references/display-modes.md](references/display-modes.md)
- Default Mode (Native OS notifications) with WindowsToastBootstrapper setup
- Window Mode (window-constrained notifications)
- Screen Mode (global overlay notifications)
- Mode comparison and when to use each
- Application startup configuration (App.xaml.cs)
- Customization limitations by mode
- When to read: User needs to choose between native OS vs in-app notifications, or configure application startup

### Severity & Visual Styling
📄 **Read:** [references/severity-and-variants.md](references/severity-and-variants.md)
- Severity levels (None, Info, Success, Warning, Error)
- Visual variants (Text, Outlined, Filled)
- AccentBrush customization
- Severity + Variant combinations
- Visual styling rules and applicability
- When to read: User needs to style toasts with colors, severity levels, or custom branding

### Placement & Animations
📄 **Read:** [references/placement-and-animations.md](references/placement-and-animations.md)
- 8 placement positions (TopLeft, TopCenter, TopRight, etc.)
- Animation types (Fade, Zoom, Slide, Flip)
- ShowAnimationType and CloseAnimationType
- Animation reference table
- When to read: User needs to position toasts or add animation effects

### Action Buttons
📄 **Read:** [references/action-buttons.md](references/action-buttons.md)
- Simple action buttons
- Action buttons with callbacks
- CloseOnClick behavior
- Custom ActionTemplate with DataTemplate
- Multiple actions handling
- When to read: User needs interactive buttons, callbacks, or custom button styling

### Template Customization
📄 **Read:** [references/template-customization.md](references/template-customization.md)
- TitleTemplate customization
- ContentTemplate customization
- CloseButtonTemplate customization
- DataTemplate structure and resource binding
- Advanced template scenarios
- When to read: User needs to fully customize toast appearance beyond built-in options

---

## Quick Start Example

### Basic Information Toast

```csharp
using System.Windows;
using Syncfusion.UI.Xaml.SfToastNotification;

namespace ToastDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void ShowToast_Click(object sender, RoutedEventArgs e)
        {
            // Simple toast with title and message
            SfToastNotification.Show(this, new ToastOptions
            {
                Mode = ToastMode.Screen,
                Title = "Welcome",
                Message = "Hello! This is your first toast notification."
            });
        }
    }
}
```

### Success Toast with Custom Duration

```csharp
// Success toast with 8-second duration
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Save Successful",
    Message = "Your document has been saved successfully.",
    Severity = ToastSeverity.Success,
    Mode = ToastMode.Screen,
    Duration = TimeSpan.FromSeconds(8)
});
```

### Error Toast with Action Buttons

```csharp
// Error toast with Retry and Dismiss actions
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Operation Failed",
    Message = "Unable to save your changes. Please try again.",
    Severity = ToastSeverity.Error,
    Mode = ToastMode.Screen,
    Actions = new List<ToastAction>
    {
        new ToastAction("Retry")
        {
            Callback = () => RetryOperation(),
            CloseOnClick = true
        },
        new ToastAction("Dismiss")
        {
            CloseOnClick = true
        }
    }
});
```

---

## Common Patterns

### Pattern 1: Native OS Notification

```csharp
// App.xaml.cs - Initialize at startup
private void Application_Startup(object sender, StartupEventArgs e)
{
    WindowsToastBootstrapper.RemoveShortcutOnUnload = true;
    WindowsToastBootstrapper.Initialize("MyApp.App", "MyApplication");
}

// MainWindow.xaml.cs - Show native toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "New Message",
    Message = "You have received a new message.",
    Mode = ToastMode.Default  // Native OS notification
});
```

### Pattern 2: Custom Styled Toast

```csharp
// Filled variant with custom accent color
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Custom Toast",
    Message = "This toast uses custom styling.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,
    Variant = ToastVariant.Filled,
    AccentBrush = new SolidColorBrush(Colors.Purple),
    Placement = ToastPlacement.BottomRight,
    ShowAnimationType = ToastAnimation.SlideBottomIn,
    CloseAnimationType = ToastAnimation.SlideBottomOut
});
```

### Pattern 3: Persistent Toast (No Auto-Close)

```csharp
// Toast remains until user closes it
var options = new ToastOptions
{
    Id = "important-notification",
    Title = "Important Update",
    Message = "Please review this notification carefully.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Warning,
    PreventAutoClose = true  // Disable auto-close
};

SfToastNotification.Show(this, options);

// Manually close later by ID
SfToastNotification.Close("important-notification");
```

### Pattern 4: Multiple Action Buttons

```csharp
SfToastNotification.Show(this, new ToastOptions
{
    Title = "New Email",
    Message = "You have a new message from John Doe.",
    Mode = ToastMode.Screen,
    Actions = new List<ToastAction>
    {
        new ToastAction("Reply")
        {
            Arguments = "Reply",
            Callback = () => OpenReplyDialog(),
            CloseOnClick = true
        },
        new ToastAction("Mark as Read")
        {
            Callback = () => MarkAsRead(),
            CloseOnClick = true
        },
        new ToastAction("Later")
        {
            CloseOnClick = true
        }
    }
});
```

---

## Key Properties

| Property | Type | Purpose | Default |
|----------|------|---------|---------|
| **Title** | string | Bold text at top of toast | null |
| **Message** | string | Main body text | null |
| **Header** | string | Additional header (Window/Screen only) | null |
| **Mode** | ToastMode | Default, Window, or Screen | Default |
| **Severity** | ToastSeverity | None, Info, Success, Warning, Error | None |
| **Variant** | ToastVariant | Text, Outlined, Filled | Text |
| **Duration** | TimeSpan | How long toast stays visible | 6 seconds |
| **PreventAutoClose** | bool | Disable auto-close | false |
| **Placement** | ToastPlacement | Screen position (8 options) | TopRight |
| **AccentBrush** | Brush | Custom color accent | null |
| **ShowAnimationType** | ToastAnimation | Show animation effect | FadeIn |
| **CloseAnimationType** | ToastAnimation | Hide animation effect | FadeOut |
| **Actions** | List\<ToastAction\> | Interactive action buttons | null |
| **ShowActionButtons** | bool | Display action button row | true |
| **ShowCloseButton** | bool | Display close button | true |
| **Id** | string | Unique identifier for management | null |

---

## Common Use Cases

### Use Case 1: Save Operation Feedback
Show success confirmation when user saves a document or form.

```csharp
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Saved",
    Message = "Your changes have been saved successfully.",
    Severity = ToastSeverity.Success,
    Mode = ToastMode.Screen,
    Duration = TimeSpan.FromSeconds(4)
});
```

### Use Case 2: Validation Errors
Display validation errors without blocking the UI.

```csharp
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Validation Error",
    Message = "Please fill in all required fields before submitting.",
    Severity = ToastSeverity.Error,
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopCenter
});
```

### Use Case 3: Background Task Completion
Notify users when long-running background tasks complete.

```csharp
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Export Complete",
    Message = "Your data has been exported to Excel.",
    Severity = ToastSeverity.Info,
    Mode = ToastMode.Default,  // Native OS notification
    Actions = new List<ToastAction>
    {
        new ToastAction("Open File")
        {
            Callback = () => OpenExportedFile(),
            CloseOnClick = true
        }
    }
});
```

### Use Case 4: Warning Messages
Show warnings about unsaved changes or pending operations.

```csharp
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Unsaved Changes",
    Message = "You have unsaved changes. Don't forget to save before closing.",
    Severity = ToastSeverity.Warning,
    Mode = ToastMode.Screen,
    Duration = TimeSpan.FromSeconds(10),
    Placement = ToastPlacement.BottomCenter
});
```

### Use Case 5: Real-Time Notifications
Display real-time updates like new messages or system alerts.

```csharp
SfToastNotification.Show(this, new ToastOptions
{
    Id = "notification-" + DateTime.Now.Ticks,
    Title = "New Message",
    Message = $"From: {senderName}\n{messagePreview}",
    Severity = ToastSeverity.Info,
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.BottomRight,
    PreventAutoClose = false,
    Duration = TimeSpan.FromSeconds(7)
});
```

---

## Additional Resources

- **Syncfusion Documentation:** [Toast Notification Overview](https://help.syncfusion.com/wpf/toast-notification/overview)
- **API Reference:** [SfToastNotification Class](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.SfToastNotification.html)
