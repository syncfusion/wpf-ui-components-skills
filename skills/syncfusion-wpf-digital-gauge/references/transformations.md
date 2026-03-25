# Transformations

This guide explains how to apply scaling and skewing transformations to digital gauge characters for dynamic visual effects.

## Overview

SfDigitalGauge supports two types of transformations:

1. **Scaling** - Adjust character dimensions (height and width)
2. **Skewing** - Apply angular distortion along X or Y axis

These transformations allow you to create dynamic displays with perspective effects, ideal for speedometers, racing dashboards, and stylized digital interfaces.

---

## Scaling Transformations

Scaling changes the size of characters by adjusting their height and width independently.

### CharacterHeight

Controls the vertical size of each character.

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

**Typical ranges:**
- **Small:** 30-45 pixels (compact displays)
- **Medium:** 45-65 pixels (standard displays)
- **Large:** 65-90 pixels (prominent displays)
- **Extra Large:** 90+ pixels (billboard-style)

**Use cases:**
- Adjust for available vertical space
- Emphasize important information with larger text
- Create visual hierarchy in multi-gauge layouts

### CharacterWidth

Controls the horizontal size of each character.

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

**Typical ranges:**
- **Narrow:** 15-25 pixels (condensed text)
- **Normal:** 25-40 pixels (standard proportions)
- **Wide:** 40-60 pixels (enhanced readability)

**Use cases:**
- Fit more characters in limited horizontal space (narrow)
- Improve readability (wide)
- Create condensed or expanded text styles

### Combined Scaling

Adjust both dimensions for complete control:

```csharp
// Tall and narrow (condensed display)
digitalGauge.CharacterHeight = 80;
digitalGauge.CharacterWidth = 25;

// Short and wide (wide display)
digitalGauge.CharacterHeight = 40;
digitalGauge.CharacterWidth = 50;

// Large square (equal dimensions)
digitalGauge.CharacterHeight = 70;
digitalGauge.CharacterWidth = 70;
```

### Maintaining Aspect Ratio

For consistent proportions across different sizes:

```csharp
// Standard 2:1 ratio (height:width)
double baseHeight = 60;
digitalGauge.CharacterHeight = baseHeight;
digitalGauge.CharacterWidth = baseHeight / 2;

// Wide 1.5:1 ratio
digitalGauge.CharacterHeight = 60;
digitalGauge.CharacterWidth = 40;  // 60 / 1.5
```

### Responsive Scaling

Scale based on container size:

```csharp
private void Window_SizeChanged(object sender, SizeChangedEventArgs e)
{
    // Scale to 80% of available height
    double availableHeight = containerGrid.ActualHeight;
    double characterHeight = Math.Min(availableHeight * 0.8, 100);
    
    digitalGauge.CharacterHeight = characterHeight;
    digitalGauge.CharacterWidth = characterHeight / 2;  // Maintain 2:1 ratio
    digitalGauge.CharactersSpacing = characterHeight * 0.15;  // 15% of height
}
```

### Dynamic Scaling Based on Value

Emphasize important values:

```csharp
private void UpdateSpeedDisplay(int speed)
{
    digitalGauge.Value = speed.ToString("D3");
    
    // Scale up for high speeds
    if (speed > 100)
    {
        digitalGauge.CharacterHeight = 80;
        digitalGauge.CharacterWidth = 45;
    }
    else
    {
        digitalGauge.CharacterHeight = 60;
        digitalGauge.CharacterWidth = 30;
    }
}
```

---

## Skewing Transformations

Skewing applies angular distortion to characters, creating perspective and motion effects.

### SkewAngleX

Skews characters horizontally along the X-axis.

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      SkewAngleX="35"
                      CharacterType="SegmentFourteen" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.SkewAngleX = 35;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

**Value:** Angle in degrees
- **Positive values:** Skew to the right (italic-like effect)
- **Negative values:** Skew to the left (reverse italic)
- **Typical range:** -45° to +45°

**Visual effect:** Characters appear slanted horizontally, like italic text.

**Use cases:**
- Racing speedometers (sense of motion)
- Dynamic dashboards (sense of speed)
- Retro/futuristic aesthetics
- Directional indicators (arrow-like appearance)

### SkewAngleY

Skews characters vertically along the Y-axis.

**XAML:**
```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      SkewAngleY="30"
                      CharacterType="SegmentFourteen" />
```

**C#:**
```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.SkewAngleY = 30;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

**Value:** Angle in degrees
- **Positive values:** Skew upward (top tilts forward)
- **Negative values:** Skew downward (top tilts backward)
- **Typical range:** -45° to +45°

**Visual effect:** Characters appear to recede or advance in 3D space.

**Use cases:**
- 3D perspective effects
- Billboard-style displays (viewing angle simulation)
- Architectural/industrial themes
- Falling/rising text animations

### Skew Angle Guidelines

| Angle Range | Effect | Readability | Best Use |
|-------------|--------|-------------|----------|
| 0° - 15° | Subtle tilt | Excellent | Professional displays |
| 15° - 30° | Noticeable slant | Good | Dynamic dashboards |
| 30° - 45° | Strong skew | Fair | Stylized displays |
| 45° - 60° | Extreme skew | Poor | Artistic/decorative |

---

## Combined Transformations

Apply multiple transformations for complex effects.

### Scaling + Horizontal Skew

Create dynamic speedometer effect:

```csharp
digitalGauge.Value = "125 MPH";
digitalGauge.CharacterHeight = 70;
digitalGauge.CharacterWidth = 40;
digitalGauge.SkewAngleX = 15;
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Cyan);
```

### Scaling + Vertical Skew

Create perspective billboard:

```csharp
digitalGauge.Value = "NEXT EXIT";
digitalGauge.CharacterHeight = 80;
digitalGauge.CharacterWidth = 50;
digitalGauge.SkewAngleY = 20;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

### Dual-Axis Skew

Create 3D depth effect:

```csharp
digitalGauge.Value = "LOADING";
digitalGauge.CharacterHeight = 60;
digitalGauge.CharacterWidth = 35;
digitalGauge.SkewAngleX = 15;
digitalGauge.SkewAngleY = 10;
digitalGauge.CharacterType = CharacterType.SegmentSixteen;
```

### Full Transformation Example

Complete racing dashboard display:

```xaml
<gauge:SfDigitalGauge Value="SPEED 235"
                      Height="90"
                      Width="400"
                      CharacterHeight="75"
                      CharacterWidth="45"
                      CharactersSpacing="12"
                      SkewAngleX="18"
                      CharacterType="SegmentSeven"
                      CharacterStroke="#00FFFF"
                      DimmedBrush="#001A1A"
                      DimmedBrushOpacity="40"
                      SegmentThickness="4" />
```

```csharp
SfDigitalGauge racingGauge = new SfDigitalGauge();
racingGauge.Value = "SPEED 235";
racingGauge.Height = 90;
racingGauge.Width = 400;
racingGauge.CharacterHeight = 75;
racingGauge.CharacterWidth = 45;
racingGauge.CharactersSpacing = 12;
racingGauge.SkewAngleX = 18;
racingGauge.CharacterType = CharacterType.SegmentSeven;
racingGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00FFFF");
racingGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#001A1A");
racingGauge.DimmedBrushOpacity = 40;
racingGauge.SegmentThickness = 4;
```

---

## Practical Use Cases

### Use Case 1: Digital Speedometer

```csharp
private void UpdateSpeedometer(int speed)
{
    SfDigitalGauge speedGauge = new SfDigitalGauge();
    speedGauge.Value = $"{speed:D3} MPH";
    speedGauge.CharacterHeight = 80;
    speedGauge.CharacterWidth = 48;
    speedGauge.CharactersSpacing = 10;
    speedGauge.SkewAngleX = 20;  // Forward motion feel
    speedGauge.CharacterType = CharacterType.SegmentSeven;
    speedGauge.CharacterStroke = speed > 65 ? Brushes.Red : Brushes.Green;
}
```

### Use Case 2: Digital Odometer

```csharp
private void DisplayMileage(double miles)
{
    SfDigitalGauge odometer = new SfDigitalGauge();
    odometer.Value = miles.ToString("000000.0");
    odometer.CharacterHeight = 45;
    odometer.CharacterWidth = 25;
    odometer.CharactersSpacing = 2;
    odometer.SkewAngleX = 5;  // Subtle tilt
    odometer.CharacterType = CharacterType.SegmentSeven;
    odometer.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#FF6600");
}
```

### Use Case 3: Digital Clock with Style

```csharp
private void UpdateClock()
{
    SfDigitalGauge clock = new SfDigitalGauge();
    clock.Value = DateTime.Now.ToString("HH:mm:ss");
    clock.CharacterHeight = 70;
    clock.CharacterWidth = 40;
    clock.CharactersSpacing = 8;
    clock.SkewAngleX = 10;  // Modern slant
    clock.CharacterType = CharacterType.EightCrossEightDotMatrix;
    clock.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#00B0FF");
}
```

### Use Case 4: 3D Billboard Effect

```csharp
private void CreateBillboard(string message)
{
    SfDigitalGauge billboard = new SfDigitalGauge();
    billboard.Value = message;
    billboard.CharacterHeight = 100;
    billboard.CharacterWidth = 65;
    billboard.CharactersSpacing = 15;
    billboard.SkewAngleY = 25;  // Perspective from below
    billboard.CharacterType = CharacterType.SegmentSixteen;
    billboard.CharacterStroke = new SolidColorBrush(Colors.Yellow);
}
```

### Use Case 5: Animated Skew Effect

```csharp
// Animate skew angle for dynamic effect
private DispatcherTimer skewTimer;
private double currentAngle = 0;

private void StartSkewAnimation()
{
    skewTimer = new DispatcherTimer();
    skewTimer.Interval = TimeSpan.FromMilliseconds(50);
    skewTimer.Tick += (s, e) =>
    {
        currentAngle += 1;
        if (currentAngle > 30) currentAngle = -30;
        
        digitalGauge.SkewAngleX = currentAngle;
    };
    skewTimer.Start();
}
```

---

## Best Practices

### 1. Maintain Readability

- Keep skew angles ≤ 30° for readable displays
- Test at various viewing distances
- Ensure text remains legible after transformation

```csharp
// Good: Subtle skew for style
digitalGauge.SkewAngleX = 15;

// Avoid: Extreme skew reduces readability
digitalGauge.SkewAngleX = 60;  // Too much!
```

### 2. Match Context

- Racing/speed → Horizontal skew (SkewAngleX)
- Billboard/signage → Vertical skew (SkewAngleY)
- Professional displays → Minimal or no skew

### 3. Consider Character Type

Different character types respond differently to skewing:

```csharp
// Segment displays (7, 14, 16) handle skew well
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.SkewAngleX = 25;  // Clear, readable

// Dot matrix may become unclear with heavy skew
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
digitalGauge.SkewAngleX = 15;  // Lighter skew recommended
```

### 4. Scale Appropriately

Larger characters can handle more skew:

```csharp
// Small characters: minimal skew
digitalGauge.CharacterHeight = 40;
digitalGauge.SkewAngleX = 10;

// Large characters: more skew acceptable
digitalGauge.CharacterHeight = 80;
digitalGauge.SkewAngleX = 25;
```

### 5. Coordinate with Layout

Account for skew in layout calculations:

```csharp
// Horizontal skew increases required width
double skewAngle = 20;
double extraWidth = digitalGauge.CharacterHeight * Math.Tan(skewAngle * Math.PI / 180);
digitalGauge.Width = normalWidth + extraWidth;
```

### 6. Test Performance

Heavy transformations can impact performance:

```csharp
// OK: Static or infrequent updates
digitalGauge.SkewAngleX = 30;
digitalGauge.SkewAngleY = 20;

// Caution: Frequent updates (e.g., animations)
// Keep transformations simpler for better performance
```

---

## Troubleshooting

### Issue: Characters Are Clipped

**Problem:** Skewed characters extend beyond gauge bounds.

**Solution:** Increase gauge width/height or add margins:

```csharp
digitalGauge.Width = originalWidth * 1.3;  // 30% extra for skew
digitalGauge.Margin = new Thickness(20, 10, 20, 10);
```

### Issue: Text Is Unreadable

**Problem:** Too much skew angle.

**Solution:** Reduce skew angle or increase character size:

```csharp
// Before (hard to read)
digitalGauge.SkewAngleX = 45;
digitalGauge.CharacterHeight = 40;

// After (much better)
digitalGauge.SkewAngleX = 20;
digitalGauge.CharacterHeight = 60;
```

### Issue: Inconsistent Appearance Across Character Types

**Problem:** Same skew looks different on different character types.

**Solution:** Adjust skew per character type:

```csharp
switch (digitalGauge.CharacterType)
{
    case CharacterType.SegmentSeven:
        digitalGauge.SkewAngleX = 20;
        break;
    case CharacterType.EightCrossEightDotMatrix:
        digitalGauge.SkewAngleX = 12;  // Less skew for dot matrix
        break;
}
```

---

## Related Topics

- [Customization](customization.md) - Character sizing and styling
- [Segment Styling](segment-styling.md) - Thickness and additional styling
- [Character Types](character-types.md) - Choosing the right display type
