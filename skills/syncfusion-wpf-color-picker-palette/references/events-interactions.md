# Events & Interactions

## Table of Contents
- [SelectedBrushChanged Event](#selectedbrushchanged-event)
- [SelectedCommand Binding](#selectedcommand-binding)
- [No Color Button](#no-color-button)
- [More Colors Window](#more-colors-window)
- [Color Tab Visibility](#color-tab-visibility)
- [Tooltip Support](#tooltip-support)

## SelectedBrushChanged Event

The `SelectedBrushChanged` event fires whenever the user selects a color, providing access to both old and new values.

### Basic Event Handling
```xaml
<syncfusion:ColorPickerPalette SelectedBrushChanged="ColorPickerPalette_SelectedBrushChanged"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
private void ColorPickerPalette_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e)
{
    // Get new selected values
    var newColor = e.NewColor;
    var newBrush = e.NewBrush;

    // Get previous values
    var oldColor = e.OldColor;
    var oldBrush = e.OldBrush;

    // Apply color to UI elements
    myTextBlock.Foreground = newBrush;
    statusLabel.Text = $"Color changed: {oldColor} → {newColor}";
}
```

### Example: Real-time Preview
Access `e.NewBrush` from the event to apply color changes immediately. Use `e.OldColor`/`e.OldBrush` to track previous values. Handle special cases like `Colors.Transparent` or neutral colors as needed.

## SelectedCommand Binding

In Split mode, use `SelectedCommand` to execute logic when the button is clicked: `SelectedCommand="{Binding ApplyColorCommand}"`. Implement ICommand in your ViewModel to handle the color application.

## No Color Button

Enable with `NoColorVisibility="Visible"`. Detect in event handler: `if (e.NewColor == Colors.Transparent)` indicates "No Color" was clicked. Handle by removing fill, opacity, or resetting to default.

## More Colors Window

Enable with `MoreColorOptionVisibility="Visible"` to open extended color picker. Control tabs with `IsStandardTabVisible` (140-color hexagon) and `IsCustomTabVisible` (custom picker). More Colors auto-hides if both tabs are collapsed.

## Color Tab Visibility

Control tabs in the "More Colors" window: `IsStandardTabVisible` for 140-color hexagon, `IsCustomTabVisible` for custom picker. Set both to `"Visible"` in XAML or use C# property assignment.

## Tooltip Support

Tooltips display color names on hover. Theme colors show default names (Red, Green, etc.). Custom colors show their `ColorName` property. Recently Used panel colors also display tooltips. See [color-management.md](color-management.md#custom-colors-collection) for custom color examples.

## Complete Interaction Example

For a complete example combining multiple features (Split mode, events, tooltips, custom colors), see [appearance-customization.md](appearance-customization.md#complete-customization-example) for comprehensive setup and [color-management.md](color-management.md) for color handling patterns.
