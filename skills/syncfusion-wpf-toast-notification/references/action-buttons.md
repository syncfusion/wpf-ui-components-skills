# Action Buttons

## Table of Contents
- [Overview](#overview)
- [Simple Action Buttons](#simple-action-buttons)
- [Action Buttons with Callbacks](#action-buttons-with-callbacks)
- [CloseOnClick Behavior](#closeonclick-behavior)
- [Custom ActionTemplate](#custom-actiontemplate)
- [ShowActionButtons Property](#showactionbuttons-property)
- [Multiple Actions Handling](#multiple-actions-handling)

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



---

## Action Buttons with Callbacks

Execute code when action buttons are clicked via the Callback property:

```csharp
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
        }
    }
});
```

Use `Arguments` property to pass data to callbacks.

---

## CloseOnClick Behavior

Set `CloseOnClick = true` to auto-close toast after button click (default for final actions). Set `CloseOnClick = false` to keep toast open for multiple selections or status updates.

```csharp
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Options",
    PreventAutoClose = true,
    Actions = new List<ToastAction>
    {
        new ToastAction("Option 1") { CloseOnClick = false },
        new ToastAction("Done") { CloseOnClick = true }
    }
});
```

---

## Custom ActionTemplate

Define custom button styling via DataTemplate and assign to `ActionTemplate` property for individual actions. Allows full control over button appearance, colors, and styling. See template-customization.md for detailed examples.



---

## ShowActionButtons Property

Set `ShowActionButtons = false` to hide the action button row. Only applies when `Actions` collection is not empty. Default is `true` for Window and Screen modes.

---

---

## Best Practices

1. **Limit action count:** 2-3 buttons maximum for clarity
2. **Use clear labels:** Action-oriented verbs ("Save", "Delete", not "OK", "Yes")
3. **Set CloseOnClick appropriately:** true for final actions, false for intermediate steps
4. **Provide safe default:** Make the least destructive action most prominent
5. **Use callbacks for navigation:** Open windows, navigate to pages, or execute operations
6. **Handle errors in callbacks:** Show error toasts if action callbacks fail
7. **Test custom templates:** Ensure templates work with different text lengths
