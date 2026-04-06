# Add/Remove Button Functionality

This guide covers the EnableAddRemoveButton feature that allows users to customize which toolbar items are visible.

## Table of Contents
- [Overview](#overview)
- [Enabling Add/Remove Button](#enabling-addremove-button)
- [Setting Icon and Label Properties](#setting-icon-and-label-properties)
- [Hiding Items with IsAvailable](#hiding-items-with-isavailable)
- [ToolBarItemInfoCollection](#toolbariteminfoCollection)
- [Best Practices](#best-practices)

## Overview

The Add/Remove button feature provides:

- **Dropdown menu** with checkboxes for each toolbar item
- **User customization** of visible items without code changes
- **Persistent state** (items remain hidden until user shows them again)
- **Icon and label display** in the dropdown menu
- **IsAvailable property** to programmatically hide items

**When to Use:**
- Applications with many toolbar commands
- Power users who want to customize their workspace
- Optional features that not all users need
- Reducing toolbar clutter

## Enabling Add/Remove Button

### EnableAddRemoveButton Property

**Type:** `bool`  
**Default:** `false`

**XAML:**

```xaml
<syncfusion:ToolBarAdv EnableAddRemoveButton="True" ToolBarName="File">
    <Button Content="New"/>
    <Button Content="Open"/>
    <Button Content="Save"/>
</syncfusion:ToolBarAdv>
```

**Result:** A dropdown button appears on the right side of the toolbar showing all items with checkboxes.

**C#:**

```csharp
ToolBarAdv toolBar = new ToolBarAdv
{
    ToolBarName = "File",
    EnableAddRemoveButton = true
};
```

### Visual Behavior

When `EnableAddRemoveButton="True"`:

1. **Dropdown button** (down arrow) appears at toolbar end
2. **Clicking dropdown** shows menu with all toolbar items
3. **Checkboxes** indicate which items are currently visible
4. **Unchecking item** hides it from toolbar
5. **Checking item** shows it again

## Setting Icon and Label Properties

For items to appear properly in the Add/Remove dropdown, use attached properties `Icon` and `Label`.

### Icon Property

**Type:** Attached property (string - image path)

**Purpose:** Shows icon next to item name in dropdown menu

### Label Property

**Type:** Attached property (string)

**Purpose:** Display name in dropdown menu (defaults to Content if not set)

### Basic Example

```xaml
<syncfusion:ToolBarAdv EnableAddRemoveButton="True">
    <!-- With Icon and Label -->
    <Button syncfusion:ToolBarAdv.Label="New Document"
            syncfusion:ToolBarAdv.Icon="Images/NewDocumentHS.png"
            ToolTip="Create new document">
        <Image Source="Images/NewDocumentHS.png" Width="16" Height="16"/>
    </Button>
    
    <Button syncfusion:ToolBarAdv.Label="Open Document"
            syncfusion:ToolBarAdv.Icon="Images/openHS.png"
            ToolTip="Open existing document">
        <Image Source="Images/openHS.png" Width="16" Height="16"/>
    </Button>
    
    <Button syncfusion:ToolBarAdv.Label="Save Document"
            syncfusion:ToolBarAdv.Icon="Images/saveHS.png"
            ToolTip="Save current document">
        <Image Source="Images/saveHS.png" Width="16" Height="16"/>
    </Button>
</syncfusion:ToolBarAdv>
```

### Without Icon

If `Icon` is not specified, only the label appears in dropdown:

```xaml
<Button syncfusion:ToolBarAdv.Label="Print"
        Content="Print"/>
```

### Setting Properties in C#

```csharp
Button newButton = new Button
{
    ToolTip = "New Document",
    Content = new Image 
    { 
        Source = new BitmapImage(new Uri("Images/NewDocumentHS.png", UriKind.Relative)),
        Width = 16,
        Height = 16
    }
};

// Set attached properties
ToolBarAdv.SetLabel(newButton, "New Document");
ToolBarAdv.SetIcon(newButton, "Images/NewDocumentHS.png");

toolBar.Items.Add(newButton);
```

### Label Fallback

If `Label` is not set, the control falls back to:
1. `Content` property (if it's a string)
2. `ToolTip` property
3. Empty string

**Example:**

```xaml
<!-- Label will be "Save" from Content -->
<Button Content="Save" 
        syncfusion:ToolBarAdv.Icon="Images/saveHS.png"/>

<!-- Label will be "Save Document" from ToolTip -->
<Button ToolTip="Save Document"
        syncfusion:ToolBarAdv.Icon="Images/saveHS.png">
    <Image Source="Images/saveHS.png" Width="16" Height="16"/>
</Button>
```

## Hiding Items with IsAvailable

The `IsAvailable` attached property controls whether an item appears in the toolbar and Add/Remove menu.

### IsAvailable Property

**Type:** Attached property (bool)  
**Default:** `true`

**Purpose:** Programmatically hide items (different from user hiding via dropdown)

### Using IsAvailable

```xaml
<syncfusion:ToolBarAdv EnableAddRemoveButton="True">
    <!-- Always visible -->
    <Button syncfusion:ToolBarAdv.Label="New"
            syncfusion:ToolBarAdv.IsAvailable="True"
            Content="New"/>
    
    <!-- Hidden by default -->
    <Button syncfusion:ToolBarAdv.Label="Advanced Options"
            syncfusion:ToolBarAdv.IsAvailable="False"
            Content="Options"/>
</syncfusion:ToolBarAdv>
```

**Result:** "Advanced Options" button does not appear in toolbar or Add/Remove menu.

### Difference from Visibility

| Property | Effect | Add/Remove Menu | Use Case |
|----------|--------|-----------------|----------|
| `IsAvailable="False"` | Hidden from toolbar and menu | Not listed | Feature not available/licensed |
| `Visibility="Collapsed"` | Hidden from toolbar | Still in menu | Temporarily hidden |
| User unchecks in menu | Hidden from toolbar | Listed but unchecked | User preference |

### Dynamic Availability

**Toggle based on license/permissions:**

```csharp
// Check license or permissions
bool hasAdvancedFeature = LicenseManager.HasFeature("Advanced");

// Set availability
ToolBarAdv.SetIsAvailable(advancedButton, hasAdvancedFeature);
```

**Context-dependent availability:**

```csharp
private void OnSelectionChanged(object sender, EventArgs e)
{
    bool hasSelection = editor.SelectedText.Length > 0;
    
    // Enable cut/copy only when text is selected
    ToolBarAdv.SetIsAvailable(cutButton, hasSelection);
    ToolBarAdv.SetIsAvailable(copyButton, hasSelection);
}
```

### Setting IsAvailable in C#

```csharp
Button optionsButton = new Button { Content = "Options" };
ToolBarAdv.SetLabel(optionsButton, "Advanced Options");
ToolBarAdv.SetIsAvailable(optionsButton, false); // Hidden initially

toolBar.Items.Add(optionsButton);

// Show when needed
ToolBarAdv.SetIsAvailable(optionsButton, true);
```

## ToolBarItemInfoCollection

The `ToolBarItemInfoCollection` property provides access to metadata about toolbar items for the Add/Remove feature.

### Accessing Item Information

```csharp
// Get collection of toolbar items
var itemInfos = toolBar.ToolBarItemInfoCollection;

foreach (var itemInfo in itemInfos)
{
    Console.WriteLine($"Label: {itemInfo.Label}");
    Console.WriteLine($"Icon: {itemInfo.Icon}");
    Console.WriteLine($"IsAvailable: {itemInfo.IsAvailable}");
    Console.WriteLine($"IsChecked: {itemInfo.IsChecked}"); // User visibility choice
}
```

### ToolBarItemInfo Properties

| Property | Type | Description |
|----------|------|-------------|
| `Label` | string | Display name in Add/Remove menu |
| `Icon` | string | Path to icon image |
| `IsAvailable` | bool | Whether item is available (programmatic) |
| `IsChecked` | bool | Whether user has item checked as visible |
| `Item` | object | Reference to actual toolbar control |

### Monitoring User Changes

```csharp
toolBar.PropertyChanged += (s, e) =>
{
    if (e.PropertyName == "ToolBarItemInfoCollection")
    {
        // User changed item visibility
        foreach (var itemInfo in toolBar.ToolBarItemInfoCollection)
        {
            if (itemInfo.IsChecked != itemInfo.Item.Visibility == Visibility.Visible)
            {
                Console.WriteLine($"{itemInfo.Label} visibility changed");
            }
        }
    }
};
```

### Programmatically Set User Preferences

```csharp
// Find specific item
var itemInfo = toolBar.ToolBarItemInfoCollection
    .FirstOrDefault(i => i.Label == "Print Preview");

if (itemInfo != null)
{
    // Programmatically hide as if user unchecked it
    itemInfo.IsChecked = false;
}
```

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Label not showing in dropdown | `Label` attached property not set | Add `syncfusion:ToolBarAdv.Label="..."` to each button |
| Icon missing in dropdown | `Icon` attached property not set | Add `syncfusion:ToolBarAdv.Icon="path/to/icon.png"` |
| `IsAvailable=False` vs `Visibility=Collapsed` | `Visibility` still lists item in Add/Remove menu | Use `IsAvailable="False"` to fully suppress from both toolbar and menu |
| Dropdown button not appearing | `EnableAddRemoveButton` not set | Set `EnableAddRemoveButton="True"` on the `ToolBarAdv` |
