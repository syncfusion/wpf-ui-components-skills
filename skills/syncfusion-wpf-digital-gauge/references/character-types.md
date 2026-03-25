# Character Types and Segments

This guide explains the four different segment display types available in SfDigitalGauge and when to use each one.

## Table of Contents
- [Overview](#overview)
- [CharacterType Property](#charactertype-property)
- [7-Segment Display](#7-segment-display)
- [14-Segment Display](#14-segment-display)
- [16-Segment Display](#16-segment-display)
- [8×8 Dot Matrix Display](#8×8-dot-matrix-display)
- [Character Type Comparison](#character-type-comparison)
- [Choosing the Right Character Type](#choosing-the-right-character-type)
- [Best Practices](#best-practices)

---

## Overview

SfDigitalGauge supports four different segment configurations, each optimized for different display needs:

| Type | Segments | Best For | Characters Supported |
|------|----------|----------|---------------------|
| **SegmentSeven** | 7 segments | Numbers | 0-9, limited symbols |
| **SegmentFourteen** | 14 segments | Mixed content | A-Z, 0-9, some symbols |
| **SegmentSixteen** | 16 segments | Mixed content | A-Z, 0-9, some symbols |
| **EightCrossEightDotMatrix** | 8×8 matrix | All characters | Full ASCII + special chars |

The segment type significantly impacts the appearance and readability of your display.

---

## CharacterType Property

Set the character type using the `CharacterType` property:

**XAML:**
```xaml
<gauge:SfDigitalGauge CharacterType="SegmentSeven" />
```

**C#:**
```csharp
digitalGauge.CharacterType = CharacterType.SegmentSeven;
```

**Enum values:**
- `CharacterType.SegmentSeven`
- `CharacterType.SegmentFourteen`
- `CharacterType.SegmentSixteen`
- `CharacterType.EightCrossEightDotMatrix`

**Default:** SegmentSeven

---

## 7-Segment Display

The classic digital display format with 7 LED segments arranged in a figure-eight pattern.

### When to Use

Use 7-segment displays for:
- **Numeric-only content** (calculators, counters)
- **Simple timers** (00:00)
- **Price displays** ($99.99)
- **Odometers** (distance, mileage)
- **Basic measurements** (temperature in numbers)

### Character Support

- **Digits:** 0-9 (fully supported)
- **Limited letters:** Some uppercase letters (A, b, C, d, E, F, H, L, o, P, etc.)
- **Symbols:** Minus (-), some punctuation

### Example

```xaml
<gauge:SfDigitalGauge Value="12345" 
                      CharacterType="SegmentSeven"
                      CharacterHeight="50"
                      CharacterWidth="30"
                      CharacterStroke="Red" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "12345";
digitalGauge.CharacterType = CharacterType.SegmentSeven;
digitalGauge.CharacterHeight = 50;
digitalGauge.CharacterWidth = 30;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Red);
```

### Use Cases

**Digital Clock:**
```csharp
digitalGauge.Value = DateTime.Now.ToString("HH:mm:ss");
digitalGauge.CharacterType = CharacterType.SegmentSeven;
```

**Counter:**
```csharp
int count = 1234;
digitalGauge.Value = count.ToString("D5"); // "01234"
digitalGauge.CharacterType = CharacterType.SegmentSeven;
```

**Temperature (numbers only):**
```csharp
double temp = 72.5;
digitalGauge.Value = temp.ToString("F1"); // "72.5"
digitalGauge.CharacterType = CharacterType.SegmentSeven;
```

---

## 14-Segment Display

An alphanumeric display with 14 segments that can show both letters and numbers clearly.

### When to Use

Use 14-segment displays for:
- **Alphanumeric labels** (product codes, part numbers)
- **Status messages** ("READY", "ERROR")
- **Mixed content** (speed indicators like "55 MPH")
- **Text-heavy displays** requiring better letter clarity than 7-segment

### Character Support

- **Digits:** 0-9 (fully supported)
- **Letters:** A-Z (uppercase, most readable)
- **Lowercase:** a-z (supported but less readable)
- **Symbols:** Most common punctuation and symbols

### Example

```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterType="SegmentFourteen"
                      CharacterHeight="60"
                      CharacterWidth="35"
                      CharacterStroke="Green" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
digitalGauge.CharacterHeight = 60;
digitalGauge.CharacterWidth = 35;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Green);
```

### Use Cases

**Status Indicator:**
```csharp
string status = isConnected ? "ONLINE" : "OFFLINE";
digitalGauge.Value = status;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
digitalGauge.CharacterStroke = isConnected ? Brushes.Green : Brushes.Red;
```

**Speed Display:**
```csharp
int speed = 88;
digitalGauge.Value = $"SPEED {speed:D3}";
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

**Product Code:**
```csharp
string productCode = "AB12-3456";
digitalGauge.Value = productCode;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

---

## 16-Segment Display

An enhanced alphanumeric display with 16 segments providing even better letter rendering than 14-segment.

### When to Use

Use 16-segment displays for:
- **Clear text rendering** where readability is critical
- **Alphanumeric displays** needing professional appearance
- **Dashboard labels** with mixed content
- **When 14-segment isn't clear enough** for your specific use case

### Character Support

- **Full alphanumeric:** A-Z, a-z, 0-9 (all highly readable)
- **Symbols:** Extensive symbol support
- **Better rendering:** Clearer display of diagonal characters (M, W, X, etc.)

### Example

```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterType="SegmentSixteen"
                      CharacterHeight="60"
                      CharacterWidth="35"
                      CharacterStroke="Blue" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.CharacterType = CharacterType.SegmentSixteen;
digitalGauge.CharacterHeight = 60;
digitalGauge.CharacterWidth = 35;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Blue);
```

### Use Cases

**System Message:**
```csharp
digitalGauge.Value = "SYSTEM READY";
digitalGauge.CharacterType = CharacterType.SegmentSixteen;
```

**User Name Display:**
```csharp
digitalGauge.Value = userName.ToUpper();
digitalGauge.CharacterType = CharacterType.SegmentSixteen;
```

**Measurement Label:**
```csharp
digitalGauge.Value = $"TEMP {temperature}C";
digitalGauge.CharacterType = CharacterType.SegmentSixteen;
```

---

## 8×8 Dot Matrix Display

A versatile pixel-based display using an 8×8 grid of dots, capable of showing any character.

### When to Use

Use dot matrix displays for:
- **Special characters** (@, #, %, &, etc.)
- **Email addresses** (user@domain.com)
- **URLs** (www.example.com)
- **Complex symbols** (°, ™, ©, etc.)
- **When you need full character set support**
- **Retro/classic digital aesthetics**

### Character Support

- **Full ASCII:** All standard ASCII characters
- **Special symbols:** °, ±, ×, ÷, ™, ©, etc.
- **Punctuation:** All punctuation marks
- **International characters:** Extended character support

### How It Works

Each character is rendered on an 8×8 grid of dots:
- **Active dots:** Use `CharacterStroke` color
- **Inactive dots:** Use `DimmedBrush` color at `DimmedBrushOpacity`

### Example

```xaml
<gauge:SfDigitalGauge Value="SYNCFUSION" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="60"
                      CharacterWidth="35"
                      CharacterStroke="Orange"
                      DimmedBrush="#E0E0E0" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "SYNCFUSION";
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
digitalGauge.CharacterHeight = 60;
digitalGauge.CharacterWidth = 35;
digitalGauge.CharacterStroke = new SolidColorBrush(Colors.Orange);
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#E0E0E0");
```

### Use Cases

**Email Display:**
```csharp
digitalGauge.Value = "user@example.com";
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
```

**Temperature with Symbol:**
```csharp
digitalGauge.Value = "72°F";
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
```

**Copyright Notice:**
```csharp
digitalGauge.Value = "© 2024 COMPANY";
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
```

**Mixed Special Characters:**
```csharp
digitalGauge.Value = "#TAG @USER 100%";
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
```

---

## Character Type Comparison

### Visual Comparison

Displaying "GAUGE 123":

```csharp
// 7-Segment (numbers clear, letters limited)
gauge1.Value = "123";
gauge1.CharacterType = CharacterType.SegmentSeven;

// 14-Segment (good for alphanumeric)
gauge2.Value = "GAUGE 123";
gauge2.CharacterType = CharacterType.SegmentFourteen;

// 16-Segment (clearer letters)
gauge3.Value = "GAUGE 123";
gauge3.CharacterType = CharacterType.SegmentSixteen;

// Dot Matrix (full character support)
gauge4.Value = "GAUGE 123";
gauge4.CharacterType = CharacterType.EightCrossEightDotMatrix;
```

### Performance Considerations

| Type | Rendering Speed | Memory | Best Performance |
|------|----------------|--------|------------------|
| SegmentSeven | Fastest | Lowest | ✓ |
| SegmentFourteen | Fast | Low | ✓ |
| SegmentSixteen | Fast | Low | ✓ |
| EightCrossEightDotMatrix | Moderate | Moderate | For <20 chars |

---

## Choosing the Right Character Type

### Decision Tree

```
Do you need special characters (@, #, %, etc.)?
├─ YES → Use EightCrossEightDotMatrix
└─ NO
   └─ Do you need letters?
      ├─ YES
      │  └─ Are diagonal letters (M, W, X) important?
      │     ├─ YES → Use SegmentSixteen
      │     └─ NO → Use SegmentFourteen
      └─ NO → Use SegmentSeven
```

### By Use Case

| Use Case | Recommended Type | Alternative |
|----------|-----------------|-------------|
| Digital clock | SegmentSeven | EightCrossEightDotMatrix |
| Counter | SegmentSeven | - |
| Status message | SegmentFourteen | SegmentSixteen |
| Product code | SegmentFourteen | SegmentSixteen |
| Email/URL | EightCrossEightDotMatrix | - |
| Temperature with ° | EightCrossEightDotMatrix | SegmentSeven (no symbol) |
| Speed display | SegmentSeven | SegmentFourteen (with "MPH") |
| User message | SegmentSixteen | SegmentFourteen |

---

## Best Practices

1. **Match content to type:**
   - Numbers only → Always use SegmentSeven for clarity
   - Mixed alphanumeric → SegmentFourteen or SegmentSixteen
   - Special characters → Must use EightCrossEightDotMatrix

2. **Consider readability:**
   - SegmentSeven has highest clarity for numbers
   - SegmentSixteen has best letter clarity
   - Dot matrix needs larger size for small text

3. **Optimize for content:**
   - Don't use dot matrix if segment displays suffice (better performance)
   - Use 14-segment unless you need the extra clarity of 16-segment

4. **Test with actual data:**
   - Preview with real values your application will display
   - Ensure all characters render correctly

5. **Size appropriately:**
   - Dot matrix needs larger CharacterHeight (60+) for clarity
   - Segment displays work well at smaller sizes (40+)

---

## Related Topics

- [Getting Started](getting-started.md) - Basic setup and initialization
- [Customization](customization.md) - Styling character appearance
- [Transformations](transformations.md) - Scaling and skewing effects
