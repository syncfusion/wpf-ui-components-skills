# Segment Styling and Advanced Features

This guide covers advanced styling options including segment thickness, RTL text direction, and fine-tuning visual appearance.

## Overview

Beyond basic customization, SfDigitalGauge provides additional properties for precise control:

| Property | Type | Purpose |
|----------|------|---------|
| **SegmentThickness** | double | Thickness of segment lines |
| **EnableRTLFormat** | bool | Right-to-Left text direction |

These properties help you fine-tune the visual appearance and support international text requirements.

---

## Segment Thickness

The `SegmentThickness` property controls the width/thickness of the lines that form each segment in the display.

### Setting Segment Thickness

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterType="SegmentFourteen"
                      SegmentThickness="5" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.SegmentThickness = 5;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

### Thickness Guidelines

| Thickness | Effect | Best For |
|-----------|--------|----------|
| **1-2** | Thin, delicate | Small displays, fine details |
| **2-4** | Standard | Most use cases (default range) |
| **4-6** | Bold, prominent | Large displays, emphasis |
| **6+** | Very thick | Extra-large displays, artistic |

**Default:** Approximately 2-3 pixels (varies by character size)

### Scaling with Character Size

For consistent appearance, scale segment thickness with character size:

```csharp
double characterHeight = 60;
digitalGauge.CharacterHeight = characterHeight;
digitalGauge.CharacterWidth = characterHeight / 2;

// Scale thickness: 5-8% of character height
digitalGauge.SegmentThickness = characterHeight * 0.06;  // 3.6 pixels for height 60
```

### Examples by Use Case

**Subtle, refined display:**
```csharp
digitalGauge.Value = "12:34:56";
digitalGauge.CharacterHeight = 50;
digitalGauge.CharacterWidth = 28;
digitalGauge.SegmentThickness = 2;
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#146CED");
```

**Bold, high-visibility display:**
```csharp
digitalGauge.Value = "WARNING";
digitalGauge.CharacterHeight = 80;
digitalGauge.CharacterWidth = 50;
digitalGauge.SegmentThickness = 6;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Red);
```

**Professional dashboard:**
```csharp
digitalGauge.Value = "SPEED 088";
digitalGauge.CharacterHeight = 65;
digitalGauge.CharacterWidth = 38;
digitalGauge.SegmentThickness = 4;
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00FF00");
```

### Dynamic Thickness

Adjust thickness based on display state:

```csharp
private void UpdateAlertDisplay(bool isAlert)
{
    digitalGauge.Value = isAlert ? "ALERT" : "NORMAL";
    
    // Thicker segments for alerts
    digitalGauge.SegmentThickness = isAlert ? 6 : 3;
    digitalGauge.CharacterStroke = isAlert ? Brushes.Red : Brushes.Green;
}
```

### Character Type Considerations

**Note:** `SegmentThickness` applies only to segment-based character types (SegmentSeven, SegmentFourteen, SegmentSixteen). It does **not** affect dot matrix displays.

```csharp
// Segment displays: SegmentThickness works
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.SegmentThickness = 4;  // ✓ Applied

// Dot matrix: SegmentThickness has no effect
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
digitalGauge.SegmentThickness = 4;  // ✗ Not applicable
```

---

## RTL (Right-to-Left) Support

The `EnableRTLFormat` property enables right-to-left text direction, essential for displaying Arabic, Hebrew, and other RTL languages.

### Enabling RTL Format

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterType="SegmentFourteen"
                      EnableRTLFormat="True" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.EnableRTLFormat = true;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

**Default:** `false` (Left-to-Right)

### How RTL Works

When `EnableRTLFormat` is `true`, the display reverses character order:

**LTR (EnableRTLFormat = false):**
```
SYNCFUSION  →  S Y N C F U S I O N
```

**RTL (EnableRTLFormat = true):**
```
SYNCFUSION  →  N O I S U F C N Y S
```

### Use Cases

**1. Arabic/Hebrew Text:**
```csharp
digitalGauge.Value = "مرحبا";  // Arabic text
digitalGauge.EnableRTLFormat = true;
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
```

**2. Mirrored Displays:**
```csharp
// For displays viewed through mirrors or reflective surfaces
digitalGauge.Value = "SPEED";
digitalGauge.EnableRTLFormat = true;
```

**3. International Applications:**
```csharp
private void SetCultureDisplay(CultureInfo culture)
{
    digitalGauge.EnableRTLFormat = culture.TextInfo.IsRightToLeft;
    digitalGauge.Value = GetLocalizedText();
}
```

### Complete RTL Example

```xaml
<gauge:SfDigitalGauge Value="STATUS OK"
                      CharacterType="SegmentFourteen"
                      CharacterHeight="55"
                      CharacterWidth="32"
                      EnableRTLFormat="True"
                      CharacterStroke="Blue"
                      HorizontalAlignment="Right" />
```

```csharp
SfDigitalGauge rtlGauge = new SfDigitalGauge();
rtlGauge.Value = "STATUS OK";
rtlGauge.CharacterType = CharacterType.SegmentFourteen;
rtlGauge.CharacterHeight = 55;
rtlGauge.CharacterWidth = 32;
rtlGauge.EnableRTLFormat = true;
rtlGauge.CharacterStroke = new SolidColorBrush(Colors.Blue);
rtlGauge.HorizontalAlignment = HorizontalAlignment.Right;
```

---

## Complete Styling Example

### Professional Dashboard Display

Combining all styling options:

```xaml
<gauge:SfDigitalGauge Value="TEMP 72°F"
                      Height="90"
                      Width="350"
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="60"
                      CharacterWidth="35"
                      CharactersSpacing="10"
                      CharacterStroke="#00FF00"
                      DimmedBrush="#1A1A1A"
                      DimmedBrushOpacity="30"
                      SegmentThickness="4"
                      EnableRTLFormat="False"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center" />
```

```csharp
SfDigitalGauge professionalGauge = new SfDigitalGauge();
professionalGauge.Value = "TEMP 72°F";
professionalGauge.Height = 90;
professionalGauge.Width = 350;
professionalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
professionalGauge.CharacterHeight = 60;
professionalGauge.CharacterWidth = 35;
professionalGauge.CharactersSpacing = 10;
professionalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00FF00");
professionalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#1A1A1A");
professionalGauge.DimmedBrushOpacity = 30;
professionalGauge.SegmentThickness = 4;
professionalGauge.EnableRTLFormat = false;
professionalGauge.HorizontalAlignment = HorizontalAlignment.Center;
professionalGauge.VerticalAlignment = VerticalAlignment.Center;
```

---

## Styling Patterns

### Pattern 1: High-Contrast Display

Bold segments with strong contrast:

```csharp
digitalGauge.Value = "CRITICAL";
digitalGauge.CharacterHeight = 70;
digitalGauge.CharacterWidth = 42;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Red);
digitalGauge.SegmentThickness = 6;
digitalGauge.DimmedBrush = new SolidColorBrush(Colors.Black);
digitalGauge.DimmedBrushOpacity = 50;
```

### Pattern 2: Subtle Refined Display

Thin segments with minimal dimming:

```csharp
digitalGauge.Value = "12:34:56";
digitalGauge.CharacterHeight = 50;
digitalGauge.CharacterWidth = 28;
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#146CED");
digitalGauge.SegmentThickness = 2;
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#F0F0F0");
digitalGauge.DimmedBrushOpacity = 15;
```

### Pattern 3: Retro LED Style

Classic thick segments with strong dimming:

```csharp
digitalGauge.Value = "88:88";
digitalGauge.CharacterHeight = 80;
digitalGauge.CharacterWidth = 48;
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Red);
digitalGauge.SegmentThickness = 5;
digitalGauge.DimmedBrush = new SolidColorBrush(Colors.DarkRed);
digitalGauge.DimmedBrushOpacity = 60;
```

### Pattern 4: Modern Minimalist

Clean, simple styling:

```csharp
digitalGauge.Value = "ONLINE";
digitalGauge.CharacterHeight = 55;
digitalGauge.CharacterWidth = 32;
digitalGauge.CharacterType = CharacterType.SegmentSixteen;
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00B0FF");
digitalGauge.SegmentThickness = 3;
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#FAFAFA");
digitalGauge.DimmedBrushOpacity = 10;
```

---

## Best Practices

### 1. Segment Thickness Proportions

Maintain proper proportion between segment thickness and character size:

```csharp
// Good: Thickness is 5-8% of character height
digitalGauge.CharacterHeight = 60;
digitalGauge.SegmentThickness = 4;  // 6.7% of height

// Avoid: Thickness too large relative to size
digitalGauge.CharacterHeight = 40;
digitalGauge.SegmentThickness = 10;  // 25% of height - too thick!
```

### 2. RTL Text Alignment

When using RTL, align content appropriately:

```csharp
digitalGauge.EnableRTLFormat = true;
digitalGauge.HorizontalAlignment = HorizontalAlignment.Right;  // Align right for RTL
```

### 3. Test Visibility

Always test your styling in actual display conditions:

- Different screen sizes
- Various lighting conditions
- Different viewing distances
- On target hardware

### 4. Accessibility

Ensure displays are readable for all users:

```csharp
// Minimum contrast ratio (WCAG AA): 4.5:1
// Use high contrast between CharacterStroke and background
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.White);
// On dark background (#1A1A1A) = high contrast ✓
```

### 5. Performance

Keep styling computationally efficient:

```csharp
// Efficient: Set properties once
digitalGauge.SegmentThickness = 4;
digitalGauge.EnableRTLFormat = isRTL;

// Avoid: Frequent property changes in tight loops
// timer.Tick += (s, e) => digitalGauge.SegmentThickness++; // ✗ Bad
```

---

## Troubleshooting

### Issue: Segments Look Blurry

**Problem:** Segment thickness is not a whole number.

**Solution:** Use integer values for thickness:

```csharp
// Before (may be blurry)
digitalGauge.SegmentThickness = 3.7;

// After (sharp)
digitalGauge.SegmentThickness = 4;
```

### Issue: RTL Text Not Displaying Correctly

**Problem:** RTL characters appear in wrong order.

**Solution:** Ensure EnableRTLFormat is set and use appropriate CharacterType:

```csharp
digitalGauge.EnableRTLFormat = true;
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix; // Best for international text
```

### Issue: Thick Segments Overlap

**Problem:** Segment thickness is too large for character size.

**Solution:** Reduce thickness or increase character dimensions:

```csharp
// Before (overlapping)
digitalGauge.CharacterHeight = 30;
digitalGauge.SegmentThickness = 8;

// After (proper proportion)
digitalGauge.CharacterHeight = 60;
digitalGauge.SegmentThickness = 4;
```

### Issue: Inconsistent Appearance

**Problem:** Same thickness looks different on different character types.

**Solution:** Adjust thickness per character type:

```csharp
switch (digitalGauge.CharacterType)
{
    case CharacterType.SegmentSeven:
        digitalGauge.SegmentThickness = 4;
        break;
    case CharacterType.SegmentFourteen:
    case CharacterType.SegmentSixteen:
        digitalGauge.SegmentThickness = 3;  // Slightly thinner for busier segments
        break;
}
```

---

## Edge Cases and Considerations

### Very Small Displays

For small character sizes, use thinner segments:

```csharp
if (digitalGauge.CharacterHeight < 40)
{
    digitalGauge.SegmentThickness = Math.Max(1, digitalGauge.CharacterHeight * 0.04);
}
```

### Very Large Displays

For large characters, scale thickness appropriately:

```csharp
if (digitalGauge.CharacterHeight > 100)
{
    digitalGauge.SegmentThickness = digitalGauge.CharacterHeight * 0.05;
}
```

### Mixed LTR/RTL Content

Handle mixed content carefully:

```csharp
// If content contains both English and Arabic
// Use EightCrossEightDotMatrix for best rendering
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
digitalGauge.EnableRTLFormat = /* determine based on primary language */;
```

### High-DPI Displays

Account for display scaling:

```csharp
double dpiScale = VisualTreeHelper.GetDpi(this).PixelsPerInchX / 96.0;
digitalGauge.SegmentThickness = baseThickness * dpiScale;
```

---

## Related Topics

- [Customization](customization.md) - Character sizing and color styling
- [Transformations](transformations.md) - Scaling and skewing effects
- [Character Types](character-types.md) - Choosing the right display type
- [Getting Started](getting-started.md) - Basic setup and configuration
