# Appearance and Styling in SfTimePicker

## Setting Foreground Color

Change text color using `Foreground` property (control text) and customize selector item colors via `SelectorStyle` with `Foreground` (unselected items) and `SelectedForeground` (highlighted items) setters.

## Setting Background Color

Set `Background` property for control background. Customize selector background via `SelectorStyle` → `Background` setter. Use `AccentBrush` property to set the highlight color for selected time values (e.g., `AccentBrush="Green"`).

## Color Customization

Combine `Foreground`, `Background`, and `AccentBrush` properties with `SelectorStyle` to create comprehensive color schemes. Selector style setters: `Foreground` (unselected), `SelectedForeground` (highlighted), `Background` (selector background).

## Flow Direction (RTL Support)

Set `FlowDirection="LeftToRight"` (default) or `FlowDirection="RightToLeft"` to support right-to-left languages. For RTL cultures, combine with `AllowNull="True"` and Arabic/RTL `Watermark` text. RTL automatically reverses layout, text alignment, and selector direction.

## Editing Modes

Control user interaction by combining `AllowInlineEditing` and `ShowDropDownButton`:

- **Free Editing (default):** `AllowInlineEditing="True"`, `ShowDropDownButton="True"` → Users type or use dropdown
- **Selection-Only:** `AllowInlineEditing="False"`, `ShowDropDownButton="True"` → Dropdown only, no typing
- **Read-Only:** `AllowInlineEditing="False"`, `ShowDropDownButton="False"` → Display only, no editing

### Editing Mode Truth Table

| AllowInlineEditing | ShowDropDownButton | Behavior |
|--------------------|-------------------|----------|
| true | true | Free typing + dropdown selection |
| true | false | Free typing only |
| false | true | Dropdown selection only |
| false | false | Read-only display |

## Theme Application

Apply built-in themes via `SfSkinManager`. Available themes: MaterialLight, MaterialDark, Office2019Blue, Office2019Black, Office2019Colourful, HighContrastBlack, HighContrastWhite, Fluent, FluentDark.

**Apply to single control** in XAML: `syncfusion:SfSkinManager.VisualStyle="MaterialLight"`. **Apply to entire app** in App.xaml: Set `SfSkinManager.VisualStyle` on `<Application>`. **Switch dynamically** in C#: `SfSkinManager.SetVisualStyle(timePicker, VisualStyles.MaterialDark);`

## Common Styling Patterns

**Business Professional:** Black foreground, white background, blue accent (`AccentBrush="#0078D4"`), dropdown-only editing, Fluent theme.

**Dark Modern:** White foreground, dark background (`#2D2D2D`), cyan accent (`#00D9FF`), FluentDark theme, customize selector colors via `SelectorStyle`.

**High Contrast Accessibility:** Use `HighContrastBlack` or `HighContrastWhite` theme with white foreground and black background for maximum contrast.

**Minimal/Clean:** Transparent background, gray foreground, `BorderThickness="0"` for borderless appearance.

**Material Design:** Use `MaterialLight` theme with Material colors (`#2196F3` blue accent, `#424242` foreground), increased selector item size (50x50).

## Custom Theme (Theme Studio)

For advanced theme customization:

1. Open Syncfusion **Theme Studio**
2. Create custom theme colors
3. Apply to application

See [Syncfusion WPF Theme Studio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme) for detailed instructions.

## Troubleshooting

### Issue: Theme Not Applied

**Solution:** Ensure Syncfusion namespace is imported:
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Issue: Foreground Color Invisible

**Solution:** Verify sufficient contrast between Foreground and Background colors.

### Issue: RTL Layout Breaks Layout

**Solution:** Use appropriate text alignment and margin settings for RTL:
```xaml
<syncfusion:SfTimePicker FlowDirection="RightToLeft"
                         TextAlignment="Right"/>
```

## Next Steps

- Explore **[Time Selector Customization](time-selector-customization.md)** for advanced UI customization
- Learn **[Time Formatting](time-formatting.md)** for display format control
