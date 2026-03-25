# Dropdown Behavior

Controls how the popup appears, where it opens, and how the button label is displayed.

---

## Dropdown Direction

`DropDirection` controls where the popup appears relative to the button. Default is `BottomLeft`.

**Available directions:**

| Value | Popup Position |
|---|---|
| `BottomLeft` | Below the button, left-aligned (default) |
| `BottomRight` | Below the button, right-aligned |
| `TopLeft` | Above the button, left-aligned |
| `TopRight` | Above the button, right-aligned |
| `Left` | To the left of the button |
| `Right` | To the right of the button |

```xaml
<syncfusion:DropDownButtonAdv Label="Country"
                               DropDirection="BottomRight"
                               SmallIcon="Images/flag.png">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="India"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="France"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:DropDownButtonAdv>
```

```csharp
dropdownbutton.DropDirection = DropDirection.BottomRight;
```

**When to change the direction:**
- `BottomLeft` / `BottomRight` — standard toolbar/ribbon placement
- `TopLeft` / `TopRight` — button is near the bottom of the screen
- `Left` / `Right` — button is in a side panel

---

## Multiline Text Label

`IsMultiLine` allows the `Label` text to wrap to multiple lines. Only applies when `SizeMode="Large"`:

```xaml
<syncfusion:DropDownButtonAdv SizeMode="Large"
                               LargeIcon="Images/user.png"
                               Label="Sign in with your Syncfusion Account"
                               IsMultiLine="True"/>
```

```csharp
button.SizeMode = SizeMode.Large;
button.IsMultiLine = true;
button.Label = "Sign in with your Syncfusion Account";
```

> **Note:** `IsMultiLine` has no effect on `SizeMode="Small"` or `SizeMode="Normal"`. The label is only visible in Normal and Large modes, and only wraps in Large mode.

---

## Right-to-Left (RTL) Support

Use `FlowDirection="RightToLeft"` to reverse the layout for RTL languages:

```xaml
<syncfusion:DropDownButtonAdv Label="Country"
                               FlowDirection="RightToLeft"
                               SmallIcon="Images/flag.png">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="India"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:DropDownButtonAdv>
```

```csharp
button.FlowDirection = FlowDirection.RightToLeft;
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `IsMultiLine` has no effect | `SizeMode` is not `Large` | Set `SizeMode="Large"` to enable multiline label |
| Popup opens in wrong direction | Default `DropDirection` is `BottomLeft` | Explicitly set `DropDirection` to desired value |
| Popup overlaps the button content | Direction conflicts with available screen space | WPF does not auto-adjust; choose direction based on UI layout |
| RTL label appears reversed unexpectedly | Parent container has `FlowDirection="RightToLeft"` | Explicitly set `FlowDirection="LeftToRight"` on the button to override |
