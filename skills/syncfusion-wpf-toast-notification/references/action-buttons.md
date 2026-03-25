# Action Buttons

## Table of Contents
- [Overview](#overview)
- [Simple Action Buttons](#simple-action-buttons)
- [Action Buttons with Callbacks](#action-buttons-with-callbacks)
- [CloseOnClick Behavior](#closeonclick-behavior)
- [Custom ActionTemplate](#custom-actiontemplate)
- [ShowActionButtons Property](#showactionbuttons-property)
- [Multiple Actions Handling](#multiple-actions-handling)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

Toast notifications can include interactive action buttons that allow users to respond directly to notifications. Action buttons provide a way to execute callbacks, navigate, or dismiss toasts with user interaction.

**Key Concepts:**
- **ToastAction:** Defines a single action button
- **Label:** Button text displayed to user
- **Callback:** Method executed when button is clicked
- **Arguments:** Optional data passed to callback
- **CloseOnClick:** Controls whether toast closes after button click
- **ActionTemplate:** Custom DataTemplate for button styling

---

## Simple Action Buttons

### Basic Action Button

```csharp
// Toast with simple action buttons
SfToastNotification.Show(this, new ToastOptions
{
    Title = "File Saved",
    Message = "Your document has been saved successfully.",
    Mode = ToastMode.Screen,
    Actions = new List<ToastAction>
    {
        new ToastAction("Undo"),
        new ToastAction("Ok")
        {
            CloseOnClick = true  // Close toast when clicked
        }
    }
});
```

### Two-Button Pattern

```csharp
// Accept/Decline pattern
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Update Available",
    Message = "A new version is available. Install now?",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Info,
    Actions = new List<ToastAction>
    {
        new ToastAction("Install")
        {
            CloseOnClick = true
        },
        new ToastAction("Later")
        {
            CloseOnClick = true
        }
    }
});
```

### Three-Button Pattern

```csharp
// Multiple options pattern
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Unsaved Changes",
    Message = "Do you want to save your changes?",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Warning,
    Actions = new List<ToastAction>
    {
        new ToastAction("Save")
        {
            CloseOnClick = true
        },
        new ToastAction("Don't Save")
        {
            CloseOnClick = true
        },
        new ToastAction("Cancel")
        {
            CloseOnClick = true
        }
    }
});
```

---

## Action Buttons with Callbacks

### Basic Callback

```csharp
// Action button with callback method
SfToastNotification.Show(this, new ToastOptions
{
    Title = "New Message",
    Message = "You have a new message from John.",
    Mode = ToastMode.Screen,
    Actions = new List<ToastAction>
    {
        new ToastAction("Reply")
        {
            Callback = () => OpenReplyWindow(),
            CloseOnClick = true
        },
        new ToastAction("Dismiss")
        {
            CloseOnClick = true
        }
    }
});

private void OpenReplyWindow()
{
    // Open reply window logic
    var replyWindow = new ReplyWindow();
    replyWindow.Show();
}
```

### Callback with Arguments

```csharp
// Pass data to callback using Arguments property
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Task Completed",
    Message = "Data export is complete.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Success,
    Actions = new List<ToastAction>
    {
        new ToastAction("Open File")
        {
            Arguments = "C:\\Exports\\data.xlsx",
            Callback = () => OpenFile("C:\\Exports\\data.xlsx"),
            CloseOnClick = true
        },
        new ToastAction("Open Folder")
        {
            Arguments = "C:\\Exports",
            Callback = () => OpenFolder("C:\\Exports"),
            CloseOnClick = true
        }
    }
});

private void OpenFile(string filePath)
{
    System.Diagnostics.Process.Start(new System.Diagnostics.ProcessStartInfo
    {
        FileName = filePath,
        UseShellExecute = true
    });
}

private void OpenFolder(string folderPath)
{
    System.Diagnostics.Process.Start("explorer.exe", folderPath);
}
```

### Async Callback Pattern

```csharp
// Action button with async operation
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Sync Required",
    Message = "Your data needs to be synchronized.",
    Mode = ToastMode.Screen,
    Actions = new List<ToastAction>
    {
        new ToastAction("Sync Now")
        {
            Callback = async () => await SynchronizeDataAsync(),
            CloseOnClick = true
        },
        new ToastAction("Later")
        {
            CloseOnClick = true
        }
    }
});

private async Task SynchronizeDataAsync()
{
    // Show progress toast
    var syncToastId = "sync-in-progress";
    SfToastNotification.Show(this, new ToastOptions
    {
        Id = syncToastId,
        Title = "Synchronizing",
        Message = "Please wait...",
        Mode = ToastMode.Screen,
        PreventAutoClose = true
    });

    try
    {
        // Simulate sync operation
        await Task.Delay(3000);
        
        // Close progress toast
        SfToastNotification.Close(syncToastId);
        
        // Show success toast
        SfToastNotification.Show(this, new ToastOptions
        {
            Title = "Sync Complete",
            Message = "Data synchronized successfully.",
            Severity = ToastSeverity.Success,
            Mode = ToastMode.Screen
        });
    }
    catch (Exception ex)
    {
        SfToastNotification.Close(syncToastId);
        SfToastNotification.Show(this, new ToastOptions
        {
            Title = "Sync Failed",
            Message = ex.Message,
            Severity = ToastSeverity.Error,
            Mode = ToastMode.Screen
        });
    }
}
```

---

## CloseOnClick Behavior

### Understanding CloseOnClick

The `CloseOnClick` property determines whether the toast automatically closes when the action button is clicked.

```csharp
// Action that keeps toast open
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Options",
    Message = "Select multiple options if needed.",
    Mode = ToastMode.Screen,
    PreventAutoClose = true,
    Actions = new List<ToastAction>
    {
        new ToastAction("Option 1")
        {
            Callback = () => SelectOption(1),
            CloseOnClick = false  // Toast stays open
        },
        new ToastAction("Option 2")
        {
            Callback = () => SelectOption(2),
            CloseOnClick = false  // Toast stays open
        },
        new ToastAction("Done")
        {
            CloseOnClick = true  // Closes toast
        }
    }
});
```

### When to Use CloseOnClick = false

**Use Cases:**
1. Multiple selections allowed
2. Progressive workflows
3. Status indicators that update
4. Forms within toasts

**Example: Multi-step workflow**
```csharp
private int currentStep = 1;

private void ShowWorkflowToast()
{
    SfToastNotification.Show(this, new ToastOptions
    {
        Id = "workflow-toast",
        Title = $"Step {currentStep} of 3",
        Message = GetStepMessage(currentStep),
        Mode = ToastMode.Screen,
        PreventAutoClose = true,
        Actions = new List<ToastAction>
        {
            new ToastAction("Next")
            {
                Callback = () => NextStep(),
                CloseOnClick = false  // Don't close, go to next step
            },
            new ToastAction("Cancel")
            {
                CloseOnClick = true
            }
        }
    });
}

private void NextStep()
{
    currentStep++;
    
    if (currentStep <= 3)
    {
        // Close current and show next
        SfToastNotification.Close("workflow-toast");
        ShowWorkflowToast();
    }
    else
    {
        // Workflow complete
        SfToastNotification.Close("workflow-toast");
        SfToastNotification.Show(this, new ToastOptions
        {
            Title = "Complete",
            Message = "Workflow completed successfully!",
            Severity = ToastSeverity.Success,
            Mode = ToastMode.Screen
        });
    }
}

private string GetStepMessage(int step)
{
    return step switch
    {
        1 => "Enter your information.",
        2 => "Review the details.",
        3 => "Confirm submission.",
        _ => ""
    };
}
```

---

## Custom ActionTemplate

Custom templates allow full control over action button appearance and styling.

### Defining DataTemplate in XAML

```xml
<!-- MainWindow.xaml -->
<Window.Resources>
    <!-- Custom style for action buttons -->
    <Style x:Key="ToastActionButtonStyle" TargetType="Button">
        <Setter Property="Background" Value="#007ACC"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="BorderThickness" Value="0" />
        <Setter Property="Padding" Value="12,6" />
        <Setter Property="Margin" Value="4,0" />
        <Setter Property="MinWidth" Value="80" />
        <Setter Property="Height" Value="32" />
        <Setter Property="FontWeight" Value="SemiBold"/>
        <Setter Property="Cursor" Value="Hand"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="Button">
                    <Border Background="{TemplateBinding Background}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}"
                            CornerRadius="4"
                            Padding="{TemplateBinding Padding}">
                        <ContentPresenter HorizontalAlignment="Center"
                                        VerticalAlignment="Center"/>
                    </Border>
                    <ControlTemplate.Triggers>
                        <Trigger Property="IsMouseOver" Value="True">
                            <Setter Property="Background" Value="#005A9E"/>
                        </Trigger>
                        <Trigger Property="IsPressed" Value="True">
                            <Setter Property="Background" Value="#004578"/>
                        </Trigger>
                    </ControlTemplate.Triggers>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    
    <!-- Custom DataTemplate for action buttons -->
    <DataTemplate x:Key="CustomActionTemplate">
        <Button Style="{StaticResource ToastActionButtonStyle}"
                Content="{Binding Label}"
                Tag="{Binding}" />
    </DataTemplate>
</Window.Resources>
```

### Using Custom Template in Code

```csharp
// Apply custom template to action buttons
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Custom Styled Actions",
    Message = "These buttons use custom styling.",
    Mode = ToastMode.Screen,
    Actions = new List<ToastAction>
    {
        new ToastAction("Primary")
        {
            ActionTemplate = (DataTemplate)Application.Current.Resources["CustomActionTemplate"],
            CloseOnClick = true
        },
        new ToastAction("Secondary")
        {
            ActionTemplate = (DataTemplate)Application.Current.Resources["CustomActionTemplate"],
            CloseOnClick = true
        }
    }
});
```

### Different Templates per Action

```xml
<!-- Define multiple templates -->
<Window.Resources>
    <DataTemplate x:Key="PrimaryActionTemplate">
        <Button Style="{StaticResource PrimaryButtonStyle}"
                Content="{Binding Label}"
                Tag="{Binding}" />
    </DataTemplate>
    
    <DataTemplate x:Key="SecondaryActionTemplate">
        <Button Style="{StaticResource SecondaryButtonStyle}"
                Content="{Binding Label}"
                Tag="{Binding}" />
    </DataTemplate>
</Window.Resources>
```

```csharp
// Apply different templates to different actions
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Confirm Action",
    Message = "Are you sure you want to delete this item?",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Warning,
    Actions = new List<ToastAction>
    {
        new ToastAction("Delete")
        {
            ActionTemplate = (DataTemplate)Application.Current.Resources["PrimaryActionTemplate"],
            Callback = () => DeleteItem(),
            CloseOnClick = true
        },
        new ToastAction("Cancel")
        {
            ActionTemplate = (DataTemplate)Application.Current.Resources["SecondaryActionTemplate"],
            CloseOnClick = true
        }
    }
});
```

---

## ShowActionButtons Property

Control whether the action button row is visible.

### Hiding All Action Buttons

```csharp
// Toast without action buttons
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Information",
    Message = "This is a simple notification without actions.",
    Mode = ToastMode.Screen,
    ShowActionButtons = false  // Hide action button row
});
```

### Conditional Action Buttons

```csharp
// Show actions only for certain conditions
bool userCanReply = CheckUserPermissions();

SfToastNotification.Show(this, new ToastOptions
{
    Title = "New Comment",
    Message = "Someone commented on your post.",
    Mode = ToastMode.Screen,
    ShowActionButtons = userCanReply,  // Conditional
    Actions = userCanReply ? new List<ToastAction>
    {
        new ToastAction("Reply") { CloseOnClick = true },
        new ToastAction("View") { CloseOnClick = true }
    } : null
});
```

---

## Multiple Actions Handling

### Best Practices for Multiple Actions

**Recommended:** 2-3 action buttons maximum for clarity

```csharp
// Good: 2 clear options
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Confirmation",
    Message = "Save changes before closing?",
    Actions = new List<ToastAction>
    {
        new ToastAction("Save"),
        new ToastAction("Don't Save")
    }
});

// Acceptable: 3 options with clear hierarchy
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Unsaved Changes",
    Message = "You have unsaved changes.",
    Actions = new List<ToastAction>
    {
        new ToastAction("Save"),
        new ToastAction("Discard"),
        new ToastAction("Cancel")
    }
});

// Avoid: Too many options (confusing)
// Consider using a dialog instead if you need 4+ actions
```

### Action Priority Pattern

```csharp
// Primary, secondary, and tertiary actions
SfToastNotification.Show(this, new ToastOptions
{
    Title = "File Conflict",
    Message = "A file with this name already exists.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Warning,
    Actions = new List<ToastAction>
    {
        new ToastAction("Replace")  // Primary action (destructive)
        {
            Callback = () => ReplaceFile(),
            CloseOnClick = true
        },
        new ToastAction("Keep Both")  // Secondary action (safe)
        {
            Callback = () => KeepBothFiles(),
            CloseOnClick = true
        },
        new ToastAction("Cancel")  // Tertiary action (cancel)
        {
            CloseOnClick = true
        }
    }
});
```

---

## Edge Cases and Troubleshooting

### Issue: Callback Not Firing

**Problem:** Action button callback method not executing.

**Solution:** Ensure callback is properly assigned:

```csharp
// WRONG: Missing callback
new ToastAction("Click Me")
{
    CloseOnClick = true
    // ❌ No Callback assigned
}

// CORRECT: Callback assigned
new ToastAction("Click Me")
{
    Callback = () => HandleClick(),  // ✅ Callback assigned
    CloseOnClick = true
}
```

### Issue: Toast Closes Before Callback Completes

**Problem:** Toast closes immediately, interrupting long-running callback.

**Solution:** Set `CloseOnClick = false` and manually close after operation:

```csharp
new ToastAction("Process")
{
    Callback = async () =>
    {
        await LongRunningOperation();
        SfToastNotification.Close("process-toast");  // Close manually when done
    },
    CloseOnClick = false  // Don't auto-close
}
```

### Issue: Actions Not Appearing

**Problem:** Actions defined but not visible in toast.

**Possible Causes:**
1. `ShowActionButtons = false`
2. `Actions` list is empty or null
3. Mode is Default (limited action support)

**Solution:**
```csharp
// Ensure proper configuration
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Test",
    Mode = ToastMode.Screen,           // ✅ Use Screen/Window mode
    ShowActionButtons = true,          // ✅ Explicitly enable
    Actions = new List<ToastAction>    // ✅ Non-empty list
    {
        new ToastAction("Action 1")
    }
});
```

### Issue: Custom Template Not Applied

**Problem:** ActionTemplate set but default styling appears.

**Solution:** Verify template resource exists and is accessible:

```csharp
// Check if resource exists
if (Application.Current.Resources.Contains("CustomActionTemplate"))
{
    var template = (DataTemplate)Application.Current.Resources["CustomActionTemplate"];
    new ToastAction("Styled")
    {
        ActionTemplate = template
    };
}
else
{
    // Fallback to default
    new ToastAction("Styled");
}
```

## Best Practices

1. **Limit action count:** 2-3 buttons maximum for clarity
2. **Use clear labels:** Action-oriented verbs ("Save", "Delete", not "OK", "Yes")
3. **Set CloseOnClick appropriately:** true for final actions, false for intermediate steps
4. **Provide safe default:** Make the least destructive action most prominent
5. **Use callbacks for navigation:** Open windows, navigate to pages, or execute operations
6. **Handle errors in callbacks:** Show error toasts if action callbacks fail
7. **Consider async operations:** Use async callbacks for long-running tasks
8. **Test custom templates:** Ensure templates work with different text lengths
