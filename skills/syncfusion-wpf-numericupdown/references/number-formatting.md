# Number Formatting in WPF NumericUpdown

## Table of Contents
- [Decimal Digit Formatting](#decimal-digit-formatting)
- [Group Separator](#group-separator)
- [NumberFormatInfo Customization](#numberformatinfo-customization)
- [Culture and Localization](#culture-and-localization)
- [Text Alignment](#text-alignment)

## Decimal Digit Formatting

### NumberDecimalDigits Property

Set `NumberDecimalDigits` to control the number of decimal places displayed.

**Common examples:**
- Price: `NumberDecimalDigits="2"` → 99.99, 100.50
- Percentage: `NumberDecimalDigits="1"` → 95.5%, 100.0%
- Scientific: `NumberDecimalDigits="4"` → 3.1416, 2.7183
- Integer: `NumberDecimalDigits="0"` → 100, 999, 42

## Group Separator

### GroupSeperatorEnabled Property

Set `GroupSeperatorEnabled="True"` to display thousand separators in large numbers. The separator character depends on culture (e.g., comma in English `5,555,555`, period in German `5.555.555`, space in French `5 555 555`).

Combine with `NumberDecimalDigits` for precise formatting: `GroupSeperatorEnabled="True"`, `NumberDecimalDigits="2"` → 1,234,567.89

## NumberFormatInfo Customization

### NumberFormatInfo Property

Customize number formatting by specifying culture-specific separators and decimal places.

**XAML with Embedded NumberFormatInfo:**
```xaml
<Window xmlns:globalization="clr-namespace:System.Globalization;assembly=mscorlib">
    <syncfusion:UpDown Name="upDown" Value="5555555" GroupSeperatorEnabled="True">
        <syncfusion:UpDown.NumberFormatInfo>
            <globalization:NumberFormatInfo 
                NumberGroupSeparator="/" 
                NumberDecimalDigits="4" 
                NumberDecimalSeparator="*"/>
        </syncfusion:UpDown.NumberFormatInfo>
    </syncfusion:UpDown>
</Window>
```

**C# Configuration:**
```csharp
// Create and configure NumberFormatInfo
NumberFormatInfo numberFormatInfo = new NumberFormatInfo();
numberFormatInfo.NumberGroupSeparator = "/";
numberFormatInfo.NumberDecimalDigits = 4;
numberFormatInfo.NumberDecimalSeparator = "*";

// Apply to UpDown
updown.Value = 5555555;
updown.GroupSeperatorEnabled = true;
updown.NumberFormatInfo = numberFormatInfo;
```

**Result Display:**
```
5/555/555*0000
```

### Customization Properties

| Property | Default | Example |
|----------|---------|---------|
| NumberGroupSeparator | "," | "/", " ", "'", "|" |
| NumberDecimalSeparator | "." | "*", ",", "٫" |
| NumberDecimalDigits | 2 | 0, 1, 2, 4 |

**Custom format examples:**
- European: `NumberGroupSeparator=" "`, `NumberDecimalSeparator=","`, → 1 234 567,89
- Indian: `NumberGroupSeparator=""`, `NumberDecimalSeparator="."` → 1234567.890
- Currency-like: `NumberGroupSeparator="|"`, `NumberDecimalSeparator="."` → 5|555|555.89

## Culture and Localization

### Culture Property

Set `Culture` using `CultureInfo` to apply culture-specific formatting automatically. Culture determines separators and format conventions.

**Supported cultures examples:**
| Culture | Code | Format Example (1000.5) |
|---------|------|------------------------|
| English (US) | en-US | 1,000.5 |
| German | de-DE | 1.000,5 |
| French | fr-FR | 1 000,5 |
| Spanish | es-ES | 1.000,5 |
| Japanese | ja-JP | 1,000.5 |

**Usage:** Set in C# via `updown.Culture = new CultureInfo("de-DE")` or in XAML via `Culture="de-DE"`. Combine with `GroupSeperatorEnabled="True"` and `NumberDecimalDigits="2"` for full localized formatting (e.g., German: 1.234.567,89).

## Text Alignment

### TextAlignment Property

Set `TextAlignment` to control horizontal alignment of numeric text within the control.

**Options:**
- **Right** (default for numbers): `TextAlignment="Right"` → Best for numeric values
- **Left**: `TextAlignment="Left"` → Match label alignment for consistency
- **Center**: `TextAlignment="Center"` → Highlight important values

## Complete Number Formatting Example

Combine properties for comprehensive formatting:
- **Price (USD):** `NumberDecimalDigits="2"`, `GroupSeperatorEnabled="True"`, `TextAlignment="Right"` → 1,299.99
- **International (German):** `Culture="de-DE"`, `GroupSeperatorEnabled="True"`, `NumberDecimalDigits="2"` → 1.299,99
- **Custom:** Apply `NumberFormatInfo` with custom `NumberGroupSeparator`, `NumberDecimalSeparator`, and `NumberDecimalDigits` properties

---

**Next:** Read [styling-and-appearance.md](styling-and-appearance.md) to customize visual appearance and colors.
