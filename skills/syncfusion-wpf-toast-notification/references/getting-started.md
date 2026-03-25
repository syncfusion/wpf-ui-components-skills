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

### Practical Lifecycle Example

```csharp
// Show progress toast
var downloadToastId = "download-" + DateTime.Now.Ticks;
SfToastNotification.Show(this, new ToastOptions
{
    Id = downloadToastId,
    Title = "Downloading",
    Message = "Please wait while the file downloads...",
    Mode = ToastMode.Screen,
    PreventAutoClose = true
});

// Simulate download completion
await DownloadFileAsync();

// Close progress toast
SfToastNotification.Close(downloadToastId);

// Show completion toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Download Complete",
    Message = "File downloaded successfully.",
    Severity = ToastSeverity.Success,
    Mode = ToastMode.Screen,
    Duration = TimeSpan.FromSeconds(4)
});
```

## Action Buttons

### Show or Hide Action Buttons

Control whether the action button row is visible using `ShowActionButtons`.

```csharp
// Toast WITHOUT action buttons
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Synchronized",
    Message = "Your project has been synchronized.",
    Mode = ToastMode.Screen,
    ShowActionButtons = false  // Hide action button row
});

// Toast WITH action buttons (default behavior)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Update Available",
    Message = "A new version is available.",
    Mode = ToastMode.Screen,
    ShowActionButtons = true,  // Explicitly enable (default is true)
    Actions = new List<ToastAction>
    {
        new ToastAction("Update Now"),
        new ToastAction("Later")
    }
});
```

**Default Behavior:**
- `ShowActionButtons = true` by default for Window and Screen modes
- Action buttons only appear if `Actions` collection is not empty
- Not applicable to Default (native) mode

## Close Button

### Show or Hide Close Button

Control the close button visibility using `ShowCloseButton`.

```csharp
// Toast with close button hidden
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Auto-Closing",
    Message = "This toast will auto-close. No manual close available.",
    Mode = ToastMode.Screen,
    ShowCloseButton = false,  // Hide close button
    Duration = TimeSpan.FromSeconds(5)
});

// Toast with close button visible (default)
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Review Needed",
    Message = "Please review the changes before proceeding.",
    Mode = ToastMode.Screen,
    ShowCloseButton = true  // Default behavior
});
```

**Important Notes:**
- Close button is only available in Window and Screen modes
- Not applicable to Default (native OS) mode
- Default value is `true`
- Consider hiding when using `PreventAutoClose = false` and short duration

## Complete Example: Multi-Step Workflow

```csharp
public class FileOperationManager
{
    public async Task ProcessFileAsync(string filePath)
    {
        // Step 1: Show processing toast
        var processingId = "file-processing";
        SfToastNotification.Show(Application.Current.MainWindow, new ToastOptions
        {
            Id = processingId,
            Title = "Processing",
            Message = $"Processing {Path.GetFileName(filePath)}...",
            Mode = ToastMode.Screen,
            PreventAutoClose = true,
            ShowCloseButton = false
        });

        try
        {
            // Simulate file processing
            await Task.Delay(3000);
            
            // Step 2: Close processing toast
            SfToastNotification.Close(processingId);
            
            // Step 3: Show success toast
            SfToastNotification.Show(Application.Current.MainWindow, new ToastOptions
            {
                Title = "Success",
                Message = "File processed successfully!",
                Severity = ToastSeverity.Success,
                Mode = ToastMode.Screen,
                Duration = TimeSpan.FromSeconds(4)
            });
        }
        catch (Exception ex)
        {
            // Step 2b: Close processing toast
            SfToastNotification.Close(processingId);
            
            // Step 3b: Show error toast
            SfToastNotification.Show(Application.Current.MainWindow, new ToastOptions
            {
                Title = "Error",
                Message = $"Failed to process file: {ex.Message}",
                Severity = ToastSeverity.Error,
                Mode = ToastMode.Screen,
                PreventAutoClose = true,
                Actions = new List<ToastAction>
                {
                    new ToastAction("Retry")
                    {
                        Callback = () => ProcessFileAsync(filePath),
                        CloseOnClick = true
                    },
                    new ToastAction("Cancel")
                    {
                        CloseOnClick = true
                    }
                }
            });
        }
    }
}
```

## Edge Cases and Troubleshooting

### Issue: Toast Not Appearing

**Possible Causes:**
1. Window reference is null or invalid
2. Application is not in foreground (for Window mode)
3. Native mode requires WindowsToastBootstrapper initialization

**Solution:**
```csharp
// Ensure valid window reference
if (Application.Current.MainWindow != null)
{
    SfToastNotification.Show(Application.Current.MainWindow, new ToastOptions
    {
        Title = "Test",
        Message = "Testing toast visibility",
        Mode = ToastMode.Screen  // Use Screen mode to avoid window focus issues
    });
}
```

### Issue: Multiple Toasts Stacking

**Problem:** Too many toasts displayed simultaneously.

**Solution:**
```csharp
// Close all existing toasts before showing new one
SfToastNotification.CloseAll();

SfToastNotification.Show(this, new ToastOptions
{
    Title = "Latest Notification",
    Message = "This replaces all previous toasts."
});
```

### Issue: Toast Duration Too Short

**Problem:** Toast disappears before user can read it.

**Solution:** Calculate appropriate duration based on message length:

```csharp
private TimeSpan CalculateReadingDuration(string message)
{
    // Rule: ~200 milliseconds per word, minimum 3 seconds
    int wordCount = message.Split(' ').Length;
    int milliseconds = Math.Max(3000, wordCount * 200);
    return TimeSpan.FromMilliseconds(milliseconds);
}

SfToastNotification.Show(this, new ToastOptions
{
    Title = "Information",
    Message = "This is a longer message that requires more time to read carefully.",
    Duration = CalculateReadingDuration("This is a longer message that requires more time to read carefully.")
});
```

## Best Practices

1. **Always provide context:** Include window reference (`this` or `Application.Current.MainWindow`)
2. **Use appropriate durations:** Longer messages need longer display times
3. **Assign IDs for managed toasts:** Use unique IDs when you need to close specific toasts
4. **Handle exceptions gracefully:** Show error toasts with recovery options
5. **Avoid toast spam:** Close previous toasts before showing similar notifications
6. **Choose appropriate mode:** Use Screen mode for global notifications, Window mode for context-specific alerts
