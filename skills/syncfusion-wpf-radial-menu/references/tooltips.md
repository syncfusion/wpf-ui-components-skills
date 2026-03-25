# Tooltips in WPF Radial Menu

This guide covers how to configure and position tooltips for radial menu items in the Syncfusion WPF Radial Menu control.

---

## Overview

Tooltips provide additional context for radial menu items when users hover over them with the mouse. The SfRadialMenu supports customizable tooltip text and placement for each menu item.

---

## ToolTip Property

The `ToolTip` property of `SfRadialMenuItem` sets the text that appears when the user hovers over the menu item.

### Basic Tooltip

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold" 
                                 ToolTip="Apply bold formatting"/>
    <navigation:SfRadialMenuItem Header="Copy" 
                                 ToolTip="Copy selected text"/>
    <navigation:SfRadialMenuItem Header="Paste" 
                                 ToolTip="Paste from clipboard"/>
    <navigation:SfRadialMenuItem Header="Cut" 
                                 ToolTip="Cut selected text"/>
</navigation:SfRadialMenu>
```

### Code-Behind Implementation

```csharp
SfRadialMenuItem bold = new SfRadialMenuItem() 
{ 
    Header = "Bold", 
    ToolTip = "Apply bold formatting" 
};

SfRadialMenuItem copy = new SfRadialMenuItem() 
{ 
    Header = "Copy", 
    ToolTip = "Copy selected text" 
};

SfRadialMenuItem paste = new SfRadialMenuItem() 
{ 
    Header = "Paste", 
    ToolTip = "Paste from clipboard" 
};

radialMenu.Items.Add(bold);
radialMenu.Items.Add(copy);
radialMenu.Items.Add(paste);
```

---

## ToolTipPlacement Property

The `ToolTipPlacement` property controls the position where the tooltip appears relative to the radial menu. It supports five placement options:

| Placement | Description |
|-----------|-------------|
| **None** | Tooltip is hidden |
| **Left** | Tooltip appears left of the radial menu |
| **Top** | Tooltip appears above the radial menu |
| **Right** | Tooltip appears right of the radial menu |
| **Bottom** | Tooltip appears below the radial menu |

### None Placement (Hidden)

Hides the tooltip completely:

```xaml
<navigation:SfRadialMenuItem Header="Bold" 
                             ToolTip="Bold formatting" 
                             ToolTipPlacement="None"/>
```

**Use Case:** When you want to temporarily disable tooltips without removing the ToolTip property.

### Left Placement

Displays tooltip to the left of the radial menu:

```xaml
<navigation:SfRadialMenuItem Header="Bold" 
                             ToolTip="Apply bold formatting" 
                             ToolTipPlacement="Left"/>
```

```csharp
boldItem.ToolTipPlacement = System.Windows.Controls.Primitives.PlacementMode.Left;
```

**Use Case:** When the radial menu is positioned on the right side of the screen.

### Top Placement

Displays tooltip above the radial menu:

```xaml
<navigation:SfRadialMenuItem Header="Bold" 
                             ToolTip="Apply bold formatting" 
                             ToolTipPlacement="Top"/>
```

```csharp
boldItem.ToolTipPlacement = System.Windows.Controls.Primitives.PlacementMode.Top;
```

**Use Case:** Default choice, works well in most scenarios. Recommended for bottom-positioned menus.

### Right Placement

Displays tooltip to the right of the radial menu:

```xaml
<navigation:SfRadialMenuItem Header="Bold" 
                             ToolTip="Apply bold formatting" 
                             ToolTipPlacement="Right"/>
```

```csharp
boldItem.ToolTipPlacement = System.Windows.Controls.Primitives.PlacementMode.Right;
```

**Use Case:** When the radial menu is positioned on the left side of the screen.

### Bottom Placement

Displays tooltip below the radial menu:

```xaml
<navigation:SfRadialMenuItem Header="Bold" 
                             ToolTip="Apply bold formatting" 
                             ToolTipPlacement="Bottom"/>
```

```csharp
boldItem.ToolTipPlacement = System.Windows.Controls.Primitives.PlacementMode.Bottom;
```

**Use Case:** When the radial menu appears at the top of the screen.

---

## Complete Examples

### Example 1: Different Placements for Different Items

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Top Item" 
                                 ToolTip="This tooltip appears on top" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Right Item" 
                                 ToolTip="This tooltip appears on the right" 
                                 ToolTipPlacement="Right"/>
    <navigation:SfRadialMenuItem Header="Bottom Item" 
                                 ToolTip="This tooltip appears at bottom" 
                                 ToolTipPlacement="Bottom"/>
    <navigation:SfRadialMenuItem Header="Left Item" 
                                 ToolTip="This tooltip appears on the left" 
                                 ToolTipPlacement="Left"/>
</navigation:SfRadialMenu>
```

### Example 2: Consistent Top Placement

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold" 
                                 ToolTip="Make text bold (Ctrl+B)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Italic" 
                                 ToolTip="Make text italic (Ctrl+I)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Underline" 
                                 ToolTip="Underline text (Ctrl+U)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Strikethrough" 
                                 ToolTip="Strikethrough text" 
                                 ToolTipPlacement="Top"/>
</navigation:SfRadialMenu>
```

### Example 3: Context-Aware Tooltip Placement

```csharp
public class EditorViewModel
{
    public void SetupRadialMenu(SfRadialMenu menu, Point menuPosition, Size screenSize)
    {
        // Determine placement based on screen position
        var placement = DetermineOptimalPlacement(menuPosition, screenSize);
        
        foreach (SfRadialMenuItem item in menu.Items)
        {
            item.ToolTipPlacement = placement;
        }
    }
    
    private System.Windows.Controls.Primitives.PlacementMode DetermineOptimalPlacement(
        Point position, Size screenSize)
    {
        // Menu in top half: show tooltip below
        if (position.Y < screenSize.Height / 2)
            return System.Windows.Controls.Primitives.PlacementMode.Bottom;
        
        // Menu in bottom half: show tooltip above
        return System.Windows.Controls.Primitives.PlacementMode.Top;
    }
}
```

---

## Tooltip Content Customization

### Descriptive Tooltips

Provide clear descriptions of what each action does:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Save" 
                                 ToolTip="Save the current document to disk" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Export" 
                                 ToolTip="Export document to PDF format" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Print" 
                                 ToolTip="Send document to printer" 
                                 ToolTipPlacement="Top"/>
</navigation:SfRadialMenu>
```

### Tooltips with Keyboard Shortcuts

Include keyboard shortcuts in tooltips for discoverability:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Cut" 
                                 ToolTip="Cut (Ctrl+X)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Copy" 
                                 ToolTip="Copy (Ctrl+C)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Paste" 
                                 ToolTip="Paste (Ctrl+V)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Undo" 
                                 ToolTip="Undo (Ctrl+Z)" 
                                 ToolTipPlacement="Top"/>
</navigation:SfRadialMenu>
```

### Dynamic Tooltip Content

Update tooltip content based on application state:

```csharp
public void UpdateTooltips(SfRadialMenu menu, bool hasSelection)
{
    var copyItem = menu.Items.OfType<SfRadialMenuItem>()
        .FirstOrDefault(i => i.Header?.ToString() == "Copy");
    
    if (copyItem != null)
    {
        copyItem.ToolTip = hasSelection 
            ? "Copy selected text (Ctrl+C)" 
            : "Copy (No text selected)";
    }
}
```

---

## Tooltip Best Practices

### Content Guidelines

1. **Be Concise:** Keep tooltip text short and informative
2. **Add Value:** Don't just repeat the header text
3. **Include Shortcuts:** Show keyboard shortcuts when applicable
4. **Use Consistent Language:** Maintain consistent terminology

### Good vs Bad Examples

**Good:**
```xaml
<navigation:SfRadialMenuItem Header="Save" 
                             ToolTip="Save document (Ctrl+S)"/>
```

**Bad:**
```xaml
<navigation:SfRadialMenuItem Header="Save" 
                             ToolTip="Save"/>
<!-- Tooltip adds no new information -->
```

**Good:**
```xaml
<navigation:SfRadialMenuItem Header="Format" 
                             ToolTip="Text formatting options"/>
```

**Bad:**
```xaml
<navigation:SfRadialMenuItem Header="Format" 
                             ToolTip="Click here to see the formatting menu that will allow you to format your text in various ways"/>
<!-- Too verbose -->
```

### Placement Guidelines

1. **Consistent Placement:** Use the same placement for all items in a menu
2. **Screen Awareness:** Consider screen edges when choosing placement
3. **Top Placement Default:** Top is usually the safest default choice
4. **Test with Real Data:** Verify tooltips don't overlap with important UI elements

---

## Common Patterns

### Pattern 1: All Items with Tooltips

Ensure every menu item has a helpful tooltip:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="New" 
                                 ToolTip="Create new document (Ctrl+N)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Open" 
                                 ToolTip="Open existing document (Ctrl+O)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Save" 
                                 ToolTip="Save document (Ctrl+S)" 
                                 ToolTipPlacement="Top"/>
    <navigation:SfRadialMenuItem Header="Close" 
                                 ToolTip="Close document (Ctrl+W)" 
                                 ToolTipPlacement="Top"/>
</navigation:SfRadialMenu>
```

### Pattern 2: Conditional Tooltip Display

Show/hide tooltips based on user preference:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold" 
                                 ToolTip="Apply bold formatting"
                                 ToolTipPlacement="{Binding ShowTooltips, 
                                     Converter={StaticResource BoolToPlacementConverter}}"/>
</navigation:SfRadialMenu>
```

```csharp
public class BoolToPlacementConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        bool showTooltips = (bool)value;
        return showTooltips 
            ? System.Windows.Controls.Primitives.PlacementMode.Top 
            : System.Windows.Controls.Primitives.PlacementMode.None;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Pattern 3: Hierarchical Tooltips

Provide context-appropriate tooltips for nested items:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Format" 
                                 ToolTip="Text formatting options" 
                                 ToolTipPlacement="Top">
        <navigation:SfRadialMenuItem Header="Bold" 
                                     ToolTip="Make text bold" 
                                     ToolTipPlacement="Top"/>
        <navigation:SfRadialMenuItem Header="Italic" 
                                     ToolTip="Make text italic" 
                                     ToolTipPlacement="Top"/>
        <navigation:SfRadialMenuItem Header="Underline" 
                                     ToolTip="Underline text" 
                                     ToolTipPlacement="Top"/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

---

## Accessibility Considerations

### Screen Readers

Tooltips are read by screen readers, providing important context for users with visual impairments:

```xaml
<!-- Good: Descriptive tooltip helps screen reader users -->
<navigation:SfRadialMenuItem Header="Share" 
                             ToolTip="Share document with other users"/>
```

### Keyboard Navigation

Ensure tooltips appear for keyboard navigation as well as mouse hover:

```csharp
// Tooltips automatically appear on focus for keyboard users
// No additional code needed if ToolTip property is set
```

### High Contrast Mode

Tooltips automatically adapt to Windows High Contrast mode, but test to ensure readability.

---

## Troubleshooting

### Issue: Tooltips Not Appearing

**Problem:** ToolTip property is set but tooltips don't show.

**Solution:** Check ToolTipPlacement is not set to "None":
```xaml
<!-- Wrong -->
<navigation:SfRadialMenuItem Header="Item" 
                             ToolTip="Description" 
                             ToolTipPlacement="None"/>

<!-- Correct -->
<navigation:SfRadialMenuItem Header="Item" 
                             ToolTip="Description" 
                             ToolTipPlacement="Top"/>
```

### Issue: Tooltips Cut Off at Screen Edge

**Problem:** Tooltips are clipped by the screen edge.

**Solution:** Change ToolTipPlacement to a position that fits on screen, or let WPF automatically adjust:
```csharp
// WPF will automatically reposition tooltips to stay on screen
// Choose initial placement based on typical menu position
```

### Issue: Tooltip Appears Too Quickly/Slowly

**Problem:** Tooltip timing doesn't match user expectations.

**Solution:** Adjust system-level tooltip settings or use custom tooltip controls with configured delays:
```csharp
// System-level tooltip settings apply
// For custom timing, create a custom ToolTip control
```
