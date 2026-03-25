# Appearance and Visual Styles

The TaskBar control supports a wide range of built-in visual styles applied via `SkinStorage.SetVisualStyle()`.

---

## Applying a Visual Style

Use `SkinStorage.SetVisualStyle()` to apply a named theme to the TaskBar control.

**C#:**
```csharp
using Syncfusion.Windows.Shared;

// Apply style after InitializeComponent()
SkinStorage.SetVisualStyle(taskBar, "Office2007Blue");
```

**XAML — set via attached property:**
```xml
<syncfusion:TaskBar Name="taskBar"
                    syncfusion:SkinStorage.VisualStyle="Office2007Blue">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock TextWrapping="Wrap"
                       Text="This TaskBar has a TaskBarItem." />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

> **Note:** `SkinStorage` is part of `Syncfusion.Windows.Shared`. Ensure `Syncfusion.Shared.WPF` is referenced.

---

## Available Visual Styles

| Style Name | Description |
|------------|-------------|
| `Default` | Standard WPF default appearance |
| `WinXP` | Windows XP style |
| `Zune` | Microsoft Zune-inspired dark style |
| `Aero` | Windows Aero glass-like style |
| `Office2003` | Microsoft Office 2003 style |
| `Office2007Blue` | Office 2007 Blue theme |
| `Office2007Black` | Office 2007 Black theme |
| `Office2007Silver` | Office 2007 Silver theme |
| `Office2010Blue` | Office 2010 Blue theme |
| `Office2010Black` | Office 2010 Black theme |
| `Office2010Silver` | Office 2010 Silver theme |
| `Blend` | Expression Blend dark style |
| `ShinyBlue` | Shiny metallic blue |
| `ShinyRed` | Shiny metallic red |
| `SyncOrange` | Syncfusion orange brand style |
| `VS2010` | Visual Studio 2010 style |
| `Metro` | Windows 8 Metro flat style |
| `Transparent` | Transparent/borderless style |
| `LunaRoyale` | Windows XP Luna Royal |
| `LunaHomestead` | Windows XP Luna Homestead |
| `LunaMetallic` | Windows XP Luna Metallic |

---

## Usage Examples

**Office 2007 Blue:**
```csharp
SkinStorage.SetVisualStyle(taskBar, "Office2007Blue");
```

**Metro (flat modern style):**
```csharp
SkinStorage.SetVisualStyle(taskBar, "Metro");
```

**Blend (dark expression style):**
```csharp
SkinStorage.SetVisualStyle(taskBar, "Blend");
```

**VS2010:**
```csharp
SkinStorage.SetVisualStyle(taskBar, "VS2010");
```

**Transparent (no background):**
```csharp
SkinStorage.SetVisualStyle(taskBar, "Transparent");
```

---

## Choosing a Style

| Use case | Recommended style |
|----------|-------------------|
| Standard enterprise app | `Office2007Blue` or `Office2010Blue` |
| Modern flat UI | `Metro` |
| Dark-themed IDE/tool | `Blend` or `VS2010` |
| Match Windows system | `Aero` |
| Minimal / custom themed | `Transparent` |

---

## Related References

- [Getting Started](getting-started.md) — Initial setup
- [Items and Content](items-and-content.md) — TaskBarItem structure
- [Layout and Orientation](layout-and-orientation.md) — GroupMargin, GroupWidth, Orientation
