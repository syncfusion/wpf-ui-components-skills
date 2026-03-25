# Orientation and Expand Mode

## Orientation

The `Orientation` property controls whether top-level `MenuItemAdv` items are laid out horizontally (default) or vertically.

### Horizontal (Default)

Items are arranged left-to-right — the standard application menu bar layout:

```xaml
<syncfusion:MenuAdv Orientation="Horizontal" Height="25" VerticalAlignment="Top">
    <syncfusion:MenuItemAdv Header="File"/>
    <syncfusion:MenuItemAdv Header="Edit"/>
    <syncfusion:MenuItemAdv Header="View"/>
    <syncfusion:MenuItemAdv Header="Project"/>
</syncfusion:MenuAdv>
```

### Vertical

Items are arranged top-to-bottom — useful for sidebar navigation menus:

```xaml
<syncfusion:MenuAdv Orientation="Vertical" Width="150" HorizontalAlignment="Left">
    <syncfusion:MenuItemAdv Header="Dashboard"/>
    <syncfusion:MenuItemAdv Header="Reports"/>
    <syncfusion:MenuItemAdv Header="Settings"/>
    <syncfusion:MenuItemAdv Header="Help"/>
</syncfusion:MenuAdv>
```

**Decision guide:**

| Layout | Use When |
|---|---|
| `Horizontal` (default) | Top-of-window application menu bar |
| `Vertical` | Sidebar navigation panel, left/right tool panel |

---

## ExpandMode

`ExpandMode` controls how submenus open when the user interacts with a `MenuItemAdv` that has children.

### ExpandOnClick (Default)

Submenus open only when the user **clicks** the parent item. This mimics standard Windows application behavior:

```xaml
<syncfusion:MenuAdv ExpandMode="ExpandOnClick">
    <syncfusion:MenuItemAdv Header="File">
        <syncfusion:MenuItemAdv Header="New"/>
        <syncfusion:MenuItemAdv Header="Open"/>
    </syncfusion:MenuItemAdv>
</syncfusion:MenuAdv>
```

### ExpandOnMouseOver

Submenus open automatically when the user **hovers** over the parent item — no click required:

```xaml
<syncfusion:MenuAdv ExpandMode="ExpandOnMouseOver">
    <syncfusion:MenuItemAdv Header="File">
        <syncfusion:MenuItemAdv Header="New"/>
        <syncfusion:MenuItemAdv Header="Open"/>
    </syncfusion:MenuItemAdv>
</syncfusion:MenuAdv>
```

**Decision guide:**

| Mode | Use When |
|---|---|
| `ExpandOnClick` | Standard desktop app menus (familiar, intentional) |
| `ExpandOnMouseOver` | Touch-screen UIs, kiosks, or when speed of navigation matters |

---

## Boundary Detection

MenuAdv automatically detects the window boundaries and repositions submenus to prevent them from rendering outside the visible screen area. This behavior is built-in and requires no configuration.

**What it handles:**
- Submenu would extend beyond the right edge → repositioned to open leftward
- Submenu would extend below the bottom edge → repositioned to open upward

---

## Scroll Support

When a `MenuItemAdv` contains more child items than can fit in the visible area, scroll support activates automatically to allow scrolling through the list.

This is automatic — no additional setup needed.

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Vertical menu renders too wide | No `Width` set | Set explicit `Width` on `MenuAdv` when using `Vertical` orientation |
| Submenus open on click in hover mode | `ExpandMode` not set | Explicitly set `ExpandMode="ExpandOnMouseOver"` |
| Menu items overflow off screen | Custom container clipping | Allow MenuAdv to render in a non-clipped region; boundary detection handles window edges only |
