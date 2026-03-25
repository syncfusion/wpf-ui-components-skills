# Customization and Styling

This guide covers all customization options for controlling the appearance of digital gauge characters and segments.

## Table of Contents
- [Overview](#overview)
- [Character Sizing](#character-sizing)
- [Character Spacing](#character-spacing)
- [Character Stroke Color](#character-stroke-color)
- [Dimmed Segments](#dimmed-segments)
- [Dimmed Brush Opacity](#dimmed-brush-opacity)
- [Complete Customization Example](#complete-customization-example)
- [Common Styling Patterns](#common-styling-patterns)
- [Best Practices](#best-practices)

---

## Overview

SfDigitalGauge provides comprehensive customization options to create visually appealing digital displays:

| Property | Type | Purpose |
|----------|------|---------|
| **CharacterHeight** | double | Height of each character |
| **CharacterWidth** | double | Width of each character |
| **CharactersSpacing** | double | Distance between characters |
| **CharacterStroke** | Brush | Color of active segments |
| **DimmedBrush** | Brush | Color of inactive segments |
| **DimmedBrushOpacity** | double | Opacity of dimmed segments |

These properties work together to create readable, professional-looking digital displays.

---

## Character Sizing

Control the dimensions of individual characters to fit your layout and ensure readability.

### CharacterHeight

Sets the height of each character in pixels.

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterHeight="70"
                      CharacterType="SegmentFourteen" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.CharacterHeight = 70;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

**Typical values:**
- Small: 30-40 pixels (compact displays)
- Medium: 40-60 pixels (standard displays)
- Large: 60-100 pixels (prominent displays)
- Extra large: 100+ pixels (billboard-style displays)

### CharacterWidth

Sets the width of each character in pixels.

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterWidth="60"
                      CharacterType="SegmentFourteen" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.CharacterWidth = 60;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

**Typical values:**
- Narrow: 15-25 pixels (condensed text)
- Standard: 25-40 pixels (normal proportions)
- Wide: 40-60 pixels (easier to read)

### Aspect Ratio Guidelines

For optimal readability, maintain appropriate aspect ratios:

| Character Type | Recommended Height:Width Ratio |
|---------------|--------------------------------|
| SegmentSeven | 2:1 to 2.5:1 |
| SegmentFourteen | 2:1 to 2.2:1 |
| SegmentSixteen | 2:1 to 2.2:1 |
| EightCrossEightDotMatrix | 1.5:1 to 2:1 |

**Example (2:1 ratio):**
```csharp
digitalGauge.CharacterHeight = 60;
digitalGauge.CharacterWidth = 30;  // Height / 2
```

### Dynamic Sizing

Adjust size based on available space:

```csharp
// Calculate based on container size
double availableHeight = containerGrid.ActualHeight * 0.8;
double characterHeight = Math.Min(availableHeight, 100);
double characterWidth = characterHeight / 2;

digitalGauge.CharacterHeight = characterHeight;
digitalGauge.CharacterWidth = characterWidth;
```

---

## Character Spacing

Control the distance between individual characters for better readability and visual appeal.

### CharactersSpacing Property

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharactersSpacing="50"
                      CharacterType="SegmentFourteen" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.CharactersSpacing = 50;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

**Note:** The property is named `CharactersSpacing` (plural), not `CharacterSpacing`.

### Spacing Guidelines

| Spacing | Pixels | Use Case |
|---------|--------|----------|
| **Tight** | 0-5 | Compact displays, limited space |
| **Normal** | 5-15 | Standard displays, good readability |
| **Loose** | 15-30 | Emphasis, large displays |
| **Extra loose** | 30+ | Billboard-style, artistic layouts |

### Examples by Use Case

**Digital clock (tight spacing):**
```csharp
digitalGauge.Value = "12:34:56";
digitalGauge.CharactersSpacing = 3;
```

**Dashboard display (normal spacing):**
```csharp
digitalGauge.Value = "SPEED 088";
digitalGauge.CharactersSpacing = 10;
```

**Prominent header (loose spacing):**
```csharp
digitalGauge.Value = "WELCOME";
digitalGauge.CharactersSpacing = 25;
```

### Calculating Total Width

Estimate the total width of your digital gauge:

```csharp
// Formula: (CharacterWidth × CharCount) + (CharactersSpacing × (CharCount - 1))
int charCount = "SYNCFUSION".Length;
double totalWidth = (characterWidth * charCount) + (charactersSpacing * (charCount - 1));

// Set gauge width
digitalGauge.Width = totalWidth + 20; // Add padding
```

---

## Character Stroke Color

The `CharacterStroke` property defines the color of active (lit) segments.

### Setting Stroke Color

**XAML (named colors):**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterStroke="Blue"
                      CharacterType="SegmentFourteen" />
```

**XAML (hex colors):**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterStroke="#146CED"
                      CharacterType="SegmentFourteen" />
```

**C# (named colors):**
```csharp
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Blue);
```

**C# (hex colors):**
```csharp
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#146CED");
```

**C# (RGB colors):**
```csharp
digitalGauge.CharacterStroke = new SolidColorBrush(Color.FromRgb(20, 108, 237));
```

### Color Recommendations

**By Use Case:**

| Use Case | Recommended Colors | Hex Values |
|----------|-------------------|------------|
| Standard display | Blue, Cyan | #146CED, #00BFFF |
| Success/Active | Green, Lime | #00FF00, #32CD32 |
| Warning | Yellow, Orange | #FFD700, #FF8C00 |
| Error/Critical | Red, Crimson | #FF0000, #DC143C |
| Neutral | White, Gray | #FFFFFF, #C0C0C0 |
| Retro/Classic | Amber, Red | #FFBF00, #FF0000 |

**For Dark Backgrounds:**
```csharp
// Bright, high-contrast colors
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Cyan);
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00FF00");
```

**For Light Backgrounds:**
```csharp
// Darker, visible colors
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Navy);
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#146CED");
```

### Dynamic Color Changes

Change colors based on state or value:

```csharp
// Temperature color coding
double temperature = 85.0;
if (temperature < 60)
    digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Blue);
else if (temperature < 80)
    digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Green);
else
    digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Red);
```

```csharp
// Status indication
bool isOnline = CheckConnectionStatus();
digitalGauge.Value = isOnline ? "ONLINE" : "OFFLINE";
digitalGauge.CharacterStroke = isOnline ? Brushes.Green : Brushes.Red;
```

---

## Dimmed Segments

The `DimmedBrush` property controls the color of inactive (unlit) segments, creating the classic LED display effect.

### What Are Dimmed Segments?

In a physical LED display, inactive segments are still visible but appear dimmed. This creates depth and makes the display look more authentic.

**Example:** When displaying "8", all 7 segments are lit. When displaying "1", only 2 segments are lit, and the other 5 appear dimmed.

### Setting Dimmed Brush

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterType="SegmentFourteen"
                      DimmedBrush="SkyBlue" />
```

**C#:**
```csharp
digitalGauge.DimmedBrush = new SolidColorBrush(Colors.SkyBlue);
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#E0E0E0");
```

### Matching Background

For best results, match the dimmed brush to your background:

**On white background:**
```csharp
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Navy);
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#F0F0F0");
```

**On dark background:**
```csharp
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Cyan);
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#1A1A1A");
```

**On colored background:**
```csharp
// Background is light blue (#E6F2FF)
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#D0E8FF");
```

### Subtle vs. Prominent Dimmed Segments

**Subtle (barely visible):**
```csharp
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#F8F8F8");
digitalGauge.DimmedBrushOpacity = 20;
```

**Prominent (clearly visible):**
```csharp
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#C0C0C0");
digitalGauge.DimmedBrushOpacity = 60;
```

---

## Dimmed Brush Opacity

Control the transparency of dimmed segments to fine-tune the visual effect.

### DimmedBrushOpacity Property

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      DimmedBrush="Blue"
                      DimmedBrushOpacity="20"
                      CharacterType="SegmentFourteen" />
```

**C#:**
```csharp
digitalGauge.DimmedBrush = new SolidColorBrush(Colors.Blue);
digitalGauge.DimmedBrushOpacity = 20;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

**Value range:** 0-100
- **0:** Completely transparent (invisible dimmed segments)
- **20-40:** Subtle effect (recommended for most cases)
- **50-70:** Moderate visibility
- **80-100:** Highly visible dimmed segments

### Opacity Guidelines

| Opacity | Effect | Best For |
|---------|--------|----------|
| 0-10 | Nearly invisible | Minimalist designs |
| 10-30 | Subtle hint | Professional displays |
| 30-50 | Visible structure | Standard LED look |
| 50-80 | Prominent dimming | Retro/classic look |
| 80-100 | Very prominent | Artistic/stylized |

### Examples

**Minimal dimming (clean look):**
```csharp
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#E0E0E0");
digitalGauge.DimmedBrushOpacity = 15;
```

**Standard LED effect:**
```csharp
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#404040");
digitalGauge.DimmedBrushOpacity = 35;
```

**High-contrast retro:**
```csharp
digitalGauge.DimmedBrush = new SolidColorBrush(Colors.DarkRed);
digitalGauge.DimmedBrushOpacity = 70;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Red);
```

---

## Complete Customization Example

### Professional Dashboard Display

```xaml
<gauge:SfDigitalGauge Value="TEMP 72°F"
                      Height="80"
                      Width="320"
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="55"
                      CharacterWidth="32"
                      CharactersSpacing="8"
                      CharacterStroke="#00FF00"
                      DimmedBrush="#1A1A1A"
                      DimmedBrushOpacity="30"
                      SegmentThickness="3"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center" />
```

```csharp
SfDigitalGauge dashboardGauge = new SfDigitalGauge();
dashboardGauge.Value = "TEMP 72°F";
dashboardGauge.Height = 80;
dashboardGauge.Width = 320;
dashboardGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
dashboardGauge.CharacterHeight = 55;
dashboardGauge.CharacterWidth = 32;
dashboardGauge.CharactersSpacing = 8;
dashboardGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00FF00");
dashboardGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#1A1A1A");
dashboardGauge.DimmedBrushOpacity = 30;
dashboardGauge.SegmentThickness = 3;
dashboardGauge.HorizontalAlignment = HorizontalAlignment.Center;
dashboardGauge.VerticalAlignment = VerticalAlignment.Center;
```

---

## Common Styling Patterns

### Pattern 1: Classic Red LED Clock

```csharp
digitalGauge.Value = DateTime.Now.ToString("HH:mm:ss");
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.CharacterHeight = 60;
digitalGauge.CharacterWidth = 35;
digitalGauge.CharactersSpacing = 5;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Red);
digitalGauge.DimmedBrush = new SolidColorBrush(Colors.DarkRed);
digitalGauge.DimmedBrushOpacity = 40;
```

### Pattern 2: Modern Blue Display

```csharp
digitalGauge.Value = "STATUS OK";
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
digitalGauge.CharacterHeight = 50;
digitalGauge.CharacterWidth = 28;
digitalGauge.CharactersSpacing = 12;
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00B0FF");
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#E3F2FD");
digitalGauge.DimmedBrushOpacity = 25;
```

### Pattern 3: High-Visibility Green Counter

```csharp
digitalGauge.Value = count.ToString("D6");
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.CharacterHeight = 70;
digitalGauge.CharacterWidth = 40;
digitalGauge.CharactersSpacing = 8;
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00FF00");
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#003300");
digitalGauge.DimmedBrushOpacity = 50;
```

### Pattern 4: Retro Amber Display

```csharp
digitalGauge.Value = "LOADING...";
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
digitalGauge.CharacterHeight = 45;
digitalGauge.CharacterWidth = 30;
digitalGauge.CharactersSpacing = 6;
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#FFBF00");
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#332800");
digitalGauge.DimmedBrushOpacity = 35;
```

---

## Best Practices

### 1. Ensure Readability

- Use high contrast between CharacterStroke and background
- Maintain minimum character height of 40px for readability
- Test visibility in actual lighting conditions

### 2. Match Dimmed Segments to Background

```csharp
// Good: Dimmed color similar to background
Background = "#1E1E1E" → DimmedBrush = "#2A2A2A"

// Bad: Dimmed color contrasts with background
Background = "#FFFFFF" → DimmedBrush = "#000000" (too stark)
```

### 3. Appropriate Opacity Levels

- **Professional displays:** 20-35 opacity
- **Retro/gaming:** 40-60 opacity
- **Minimalist:** 10-20 opacity

### 4. Consistent Aspect Ratios

```csharp
// Maintain 2:1 ratio for segment displays
double ratio = 2.0;
digitalGauge.CharacterWidth = digitalGauge.CharacterHeight / ratio;
```

### 5. Responsive Sizing

```csharp
// Scale based on parent container
private void AdjustGaugeSize()
{
    double scale = Math.Min(container.ActualWidth / 400, container.ActualHeight / 100);
    digitalGauge.CharacterHeight = 60 * scale;
    digitalGauge.CharacterWidth = 30 * scale;
    digitalGauge.CharactersSpacing = 10 * scale;
}
```

### 6. Color Accessibility

- Ensure WCAG AA color contrast (4.5:1 minimum)
- Avoid red-green combinations for color-blind users
- Provide alternative indicators beyond color alone

---

## Related Topics

- [Character Types](character-types.md) - Choosing the right segment display
- [Transformations](transformations.md) - Scaling and skewing effects
- [Segment Styling](segment-styling.md) - Thickness and RTL support
