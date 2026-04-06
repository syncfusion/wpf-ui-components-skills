# Styling and Appearance in WPF NumericUpdown

## Table of Contents
- [Positive Value Styling](#positive-value-styling)
- [Negative Value Styling](#negative-value-styling)
- [Zero Value Styling](#zero-value-styling)
- [Focused State Styling](#focused-state-styling)
- [Theme Integration](#theme-integration)
- [Custom Theme Creation](#custom-theme-creation)

## Positive Value Styling

### Basic Positive Value Colors

Customize background and foreground colors using `Background` and `Foreground` properties. Use any WPF named color (e.g., `Brushes.MediumBlue`, `Brushes.White`), ARGB with transparency (`Color.FromArgb(100, 0, 128, 255)`), or hex colors via `ColorConverter`.

**Common patterns:**
- Success/Positive: `Background="LightGreen"`, `Foreground="DarkGreen"`
- Professional Blue: `Background="AliceBlue"`, `Foreground="MidnightBlue"`
- High Contrast: `Background="Black"`, `Foreground="Yellow"`

## Negative Value Styling

### EnableNegativeColors Property

Set `EnableNegativeColors="True"` to enable special styling for negative values. When true, `NegativeBackground` and `NegativeForeground` colors are applied to negative numeric values.

**Common patterns:**
- Alert/Warning: `NegativeBackground="LightCoral"`, `NegativeForeground="DarkRed"`
- Loss Indicator (financial): `NegativeBackground="MistyRose"`, `NegativeForeground="Red"`
- Subtle Negative: `NegativeBackground="WhiteSmoke"`, `NegativeForeground="Gray"`

## Zero Value Styling

### ApplyZeroColor Property

Set `ApplyZeroColor="True"` and specify `ZeroColor` to apply special foreground color when value is exactly 0.

**Common patterns:**
- Neutral/Reset Indicator: `ZeroColor="Gray"`
- Break-even Point: `ZeroColor="Orange"`
- Special Highlight: `ZeroColor="Purple"`

### Three-State Color Styling

Combine positive, negative, and zero colors for comprehensive value indication:

```csharp
// Positive values - Green
updown.Background = Brushes.LightGreen;
updown.Foreground = Brushes.DarkGreen;

// Negative values - Red
updown.EnableNegativeColors = true;
updown.NegativeBackground = Brushes.LightCoral;
updown.NegativeForeground = Brushes.DarkRed;

// Zero value - Gray
updown.ApplyZeroColor = true;
updown.ZeroColor = Brushes.Gray;
```

**Display Behavior:**
- Value = 100: Green background with dark green text
- Value = -50: Light coral background with dark red text
- Value = 0: Gray text (default background)

## Focused State Styling

### EnableFocusedColors Property

Set `EnableFocusedColors="True"` to apply special styling when the control receives keyboard focus. Properties include:
- **FocusedBackground**: Background color when focused
- **FocusedForeground**: Text color when focused  
- **FocusedBorderBrush**: Border color when focused

**Important:** When focused, positive/negative/zero value colors are temporarily replaced by focused colors. When focus is lost, original value-based colors are restored.

**Common patterns:**
- Highlight on Focus: `FocusedBackground="LightYellow"`, `FocusedBorderBrush="Orange"`
- Prominent: `FocusedBackground="Cyan"`, `FocusedForeground="Black"`, `FocusedBorderBrush="Blue"`
- Subtle: `FocusedBorderBrush="LightGray"` only

### Complete Styling with Focus Example

Combine all color properties for comprehensive styling: positive background/foreground, negative colors (when `EnableNegativeColors="True"`), zero color (when `ApplyZeroColor="True"`), and focused state colors (when `EnableFocusedColors="True"`). Focused colors override value-based colors while control has focus; value colors are restored when focus is lost.

## Theme Integration

### SfSkinManager

Apply built-in themes in XAML using `syncfusion:SfSkinManager.VisualStyle="ThemeName"` on the Window, or in C# using `SfSkinManager.SetVisualStyle(control, VisualStyles.ThemeName)` for specific controls or windows.

**Available themes:** MaterialLight, MaterialDark, Office2019Colorful, Office2019Black, Office2019HighContrast, FluentLight, FluentDark.

**Example:** Set window theme in XAML (`syncfusion:SfSkinManager.VisualStyle="MaterialLight"`) or apply to specific UpDown in C# (`SfSkinManager.SetVisualStyle(upDown, VisualStyles.MaterialDark)`).

## Custom Theme Creation

### Using Theme Studio

Launch Theme Studio from Visual Studio (Extensions → Syncfusion → Launch Theme Studio). Select UpDown component and base theme (Material, Office, Fluent). Customize colors (primary, secondary, accent, background, foreground, focus, hover, borders). Export as XAML resource dictionary and merge into your Window.Resources.

**Apply custom theme:** Merge the exported XAML into `Window.Resources` using `ResourceDictionary.MergedDictionaries`.

**Override theme colors:** After applying a theme, override specific properties in C# if needed (e.g., `updown.Foreground = Brushes.CustomColor;`).

## Complete Styling Example

Combine all styling features in a single UpDown: positive colors (Background/Foreground), negative colors (`EnableNegativeColors` with NegativeBackground/NegativeForeground), zero color (`ApplyZeroColor` with ZeroColor), and focused colors (`EnableFocusedColors` with FocusedBackground/FocusedBorderBrush). Apply a base theme with `SfSkinManager.VisualStyle` and override individual properties as needed.

---

**Next:** Read [advanced-configuration.md](advanced-configuration.md) for step intervals, animations, and complex scenarios.
