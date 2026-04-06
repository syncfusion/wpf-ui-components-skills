# Customization and Styling

Learn how to customize the appearance and behavior of your TabControl with various styling options, orientations, and themes.

## Tab Orientation and Placement

Control tab header placement with `TabStripPlacement` property:
- **Top** (default): Horizontal header bar at top
- **Bottom**: Horizontal header bar at bottom
- **Left**: Vertical header bar on left (narrow layout)
- **Right**: Vertical header bar on right

Set via XAML: `TabStripPlacement="Left"` or C#: `tabControl.TabStripPlacement = Dock.Left`

## Layout Modes

Configure how tabs are arranged when there are many items using the `TabControlSettings.TabMode` property:

- **SingleLine**: All tabs on one line with scrolling buttons (clean interface)
- **MultiLine**: Tabs wrap to multiple rows (display many tabs without scrolling)
- **MultiLineWithFillWidth**: Tabs wrap and distribute evenly (professional appearance)

## Editable Tab Headers

Enable header editing with `IsHeaderEditingEnabled="True"`. Users double-click to edit or press F2 on the selected tab. Press Enter to confirm, Escape to cancel. Set via `tabControl.IsHeaderEditingEnabled = true` in C#.

## Pin and Unpin Functionality

Enable pin/unpin with `IsPinningEnabled="True"`. Use `tabItem.IsPinned` property to check or set pin state. Pinned tabs appear first and cannot be closed unless unpinned. Users can right-click headers or use the pin icon to toggle pin state.

## Tab Scrolling Configuration

Control scrolling with `TabScrollButtonVisibility` and `TabScrollStyle`:
- **Standard**: Previous/Next buttons
- **Extended**: First/Previous/Next/Last buttons

Set via `TabScrollStyle="Extended"` or `tabControl.TabScrollStyle = TabScrollStyle.Extended`.

## Styling and Themes

### Built-in Themes

Apply Syncfusion themes using `SfSkinManager.SetVisualStyle()` in XAML or C#:

```xml
syncfusion:SfSkinManager.VisualStyle="Office2019Colorful"
```

Available themes: Default, Office2019Colorful, Office2019Black, VisualStudio2013, Lime, Saffron, and more.

## Custom Styling

Style `TabControlExt` and `TabItemExt` using standard WPF Style resources:

```xml
<Style TargetType="syncfusion:TabControlExt">
    <Setter Property="Background" Value="#F5F5F5" />
</Style>
<Style TargetType="syncfusion:TabItemExt">
    <Setter Property="Foreground" Value="#333333" />
    <Setter Property="Padding" Value="10,5,10,5" />
</Style>
```

For custom headers (icons, badges), use `TabItemExt.Header` with StackPanel or Grid layouts combining Image and TextBlock elements.

## Visual Customization Examples

Combine layout modes, styling, and features for specific use cases:
- **Compact Tab Bar**: SingleLine mode + small padding (8,2,8,2) + scroll buttons
- **Wide Tabs with Icons**: MultiLineWithFillWidth mode + icon headers (StackPanel with Image + TextBlock)
- **Vertical Sidebar**: TabStripPlacement="Left" + MultiLine mode + pinning enabled

## Best Practices

- Choose top/bottom placement for horizontal, left/right for vertical layouts
- Keep tab headers concise and descriptive
- Ensure good color contrast in custom styles
- Use clear icons for better UX
- Apply consistent styling across all tabs

## Common Customization Patterns

| Goal | Approach |
|------|----------|
| Clean minimal look | Hide close buttons, use SingleLine mode |
| Document browser | Show close buttons, enable pin/unpin |
| Settings panel | Vertical placement, disable closing |
| Code editor | Dark theme, editable headers |
| Dashboard | MultiLine mode, equal-width tabs |
