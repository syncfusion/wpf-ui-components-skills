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

### Reference Files

- **[getting-started.md](references/getting-started.md):** Basic setup, NuGet installation, lifecycle management
- **[display-modes.md](references/display-modes.md):** Default/Window/Screen modes, bootstrapper setup, mode comparison
- **[severity-and-variants.md](references/severity-and-variants.md):** Severity levels, visual variants, AccentBrush customization
- **[placement-and-animations.md](references/placement-and-animations.md):** 8 placements, animation types, animation reference
- **[action-buttons.md](references/action-buttons.md):** Action buttons, callbacks, CloseOnClick, custom templates
- **[template-customization.md](references/template-customization.md):** TitleTemplate, ContentTemplate, CloseButtonTemplate customization

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

### Native OS Notification

Initialize at startup via `WindowsToastBootstrapper.Initialize()`, then set `Mode = ToastMode.Default`.





---

## Key Properties

- **Title:** Bold text at top (string)
- **Message:** Main body text (string)
- **Mode:** Display mode - Default (native OS), Window, or Screen
- **Severity:** Visual severity - None, Info, Success, Warning, Error
- **Duration:** Auto-close timeout (default: 6 seconds)
- **PreventAutoClose:** Disable auto-close (bool)
- **Placement:** Screen position (8 options: TopLeft, TopCenter, etc.)
- **Variant:** Visual variant - Text, Outlined, Filled
- **AccentBrush:** Custom color accent (Brush)
- **ShowAnimationType:** Show animation (FadeIn, Zoom, Slide, Flip, etc.)
- **CloseAnimationType:** Hide animation (FadeOut, etc.)
- **Actions:** Interactive action buttons (List<ToastAction>)
- **Id:** Unique identifier for management (string)

---




