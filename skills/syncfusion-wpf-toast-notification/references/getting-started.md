# Getting Started with Toast Notifications

## Overview

SfToastNotification is a **non-UI control** that displays toast notifications in WPF applications without requiring any XAML markup. All configuration and display logic is done entirely through C# code, making it lightweight and easy to integrate.

## Assembly Deployment

### Method 1: NuGet Package (Recommended)

Install the **Syncfusion.SfToastNotification.WPF** NuGet package. This automatically installs all required dependencies.

```powershell
Install-Package Syncfusion.SfToastNotification.WPF
```

### Method 2: Manual Assembly References

Add references to the following assemblies in your WPF project:
- `Syncfusion.SfToastNotification.WPF`
- `Syncfusion.Shared.WPF`

## Non-UI Control Architecture

**Key Concept:** SfToastNotification is NOT a visual control. It doesn't have a visual tree or XAML representation. Instead:
- It's a **static service API** for showing/closing toasts
- No need to instantiate objects or add controls to UI
- Call `SfToastNotification.Show()` from anywhere in your code
- Toast UI is dynamically created and managed internally

## Basic Implementation

### Simple Toast with Title and Message

```csharp
using System;
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

        private void ShowBasicToast_Click(object sender, RoutedEventArgs e)
        {
            // Show a simple information toast
            SfToastNotification.Show(this, new ToastOptions
            {
                Title = "Welcome",
                Message = "Hello! This is your first toast notification."
            });
        }
    }
}
```

## Toast Content Properties

### Title, Message, and Header

```csharp
// Toast with all three text properties
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Email Sent",           // Bold text at top
    Header = "Notification",         // Additional header (Window/Screen mode only)
    Message = "Your email has been sent to 3 recipients.",  // Main body text
    Mode = ToastMode.Screen
});
```

**Important Notes:**
- **Title:** Represents the bold, prominent text at the top of the toast
- **Message:** The primary content that conveys information
- **Header:** Only applies to Window and Screen modes (ignored in Default/native mode)
- All three properties are optional, but at least Title or Message should be provided

## Duration Configuration

Control how long the toast remains visible using the `Duration` property.

### Default Duration (6 seconds)

```csharp
// Uses default 6-second duration
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Default Duration",
    Message = "This toast will auto-close in 6 seconds."
});
```

### Custom Duration

```csharp
// Toast with 10-second duration
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Processing",
    Message = "Your request is being processed...",
    Mode = ToastMode.Screen,
    Duration = TimeSpan.FromSeconds(10)
});

// Short duration for quick notifications
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Saved",
    Message = "Changes saved.",
    Duration = TimeSpan.FromSeconds(3)
});
```

### Prevent Auto-Close

Use `PreventAutoClose` to keep the toast visible until the user manually closes it.

```csharp
// Toast remains until user closes it manually
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Important Update",
    Message = "Please review this critical notification.",
    Mode = ToastMode.Screen,
    PreventAutoClose = true  // Disable auto-close
});
```

**When to Use:**
- Critical notifications requiring user acknowledgment
- Error messages that need attention
- Confirmations before proceeding with actions
- Information that should remain visible for reference

## Toast Lifecycle Management

### Creating Toasts with IDs

Assign unique IDs to track and manage specific toast instances.

```csharp
// Create toast with custom ID
var options = new ToastOptions
{
    Id = "toast-123",
    Title = "Download Started",
    Message = "Downloading file...",
    Mode = ToastMode.Screen,
    PreventAutoClose = true
};

SfToastNotification.Show(this, options);
```

### Closing Specific Toasts

```csharp
// Close toast by ID
SfToastNotification.Close("toast-123");
```

### Closing All Toasts

```csharp
// Close all currently displayed toasts
SfToastNotification.CloseAll();
```

### Lifecycle Example

Show progress toast, wait for operation, close it, then show completion:

```csharp
var toastId = "progress-" + DateTime.Now.Ticks;
SfToastNotification.Show(this, new ToastOptions { Id = toastId, Title = "Processing...", PreventAutoClose = true });
await DoWorkAsync();
SfToastNotification.Close(toastId);
SfToastNotification.Show(this, new ToastOptions { Title = "Done!", Severity = ToastSeverity.Success });
```



## Best Practices

1. **Always provide context:** Include window reference (`this` or `Application.Current.MainWindow`)
2. **Use appropriate durations:** Longer messages need more display time (Rule: ~3-5 seconds typical)
3. **Assign IDs for managed toasts:** Use unique IDs when you need to close specific toasts
4. **Avoid toast spam:** Call `SfToastNotification.CloseAll()` before showing similar notifications
5. **Choose appropriate mode:** Use Screen mode for global notifications, Window mode for context-specific alerts
