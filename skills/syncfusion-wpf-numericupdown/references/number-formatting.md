# Number Formatting in WPF NumericUpdown

## Table of Contents
- [Decimal Digit Formatting](#decimal-digit-formatting)
- [Group Separator](#group-separator)
- [NumberFormatInfo Customization](#numberformatinfo-customization)
- [Culture and Localization](#culture-and-localization)
- [Text Alignment](#text-alignment)

## Decimal Digit Formatting

### NumberDecimalDigits Property

Control the number of decimal places displayed in the UpDown control.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  NumberDecimalDigits="4" 
                  Height="25" 
                  Width="100" />
```

**C#:**
```csharp
UpDown updown = new UpDown();
updown.NumberDecimalDigits = 4;
updown.Height = 25;
updown.Width = 100;
grid.Children.Add(updown);
```

### Common Decimal Formatting Examples

**Price Input (2 decimals):**
```csharp
updown.NumberDecimalDigits = 2;  // Display: 99.99, 100.50, etc.
```

**Percentage Display (1 decimal):**
```csharp
updown.NumberDecimalDigits = 1;  // Display: 95.5%, 100.0%
```

**Scientific Values (4 decimals):**
```csharp
updown.NumberDecimalDigits = 4;  // Display: 3.1416, 2.7183
```

**Integer Values (0 decimals):**
```csharp
updown.NumberDecimalDigits = 0;  // Display: 100, 999, 42
```

### Decimal Formatting Example

```xaml
<StackPanel Margin="20">
    <TextBlock Text="Precision Comparison" FontSize="14" FontWeight="Bold"/>
    
    <!-- 2 decimal places - Price -->
    <StackPanel Margin="0,10,0,0">
        <TextBlock Text="Price (2 decimals):"/>
        <syncfusion:UpDown Name="priceUpDown" 
                          NumberDecimalDigits="2" 
                          Value="19.99" 
                          Height="25" 
                          Width="150"/>
    </StackPanel>
    
    <!-- 4 decimal places - Measurement -->
    <StackPanel Margin="0,10,0,0">
        <TextBlock Text="Measurement (4 decimals):"/>
        <syncfusion:UpDown Name="measureUpDown" 
                          NumberDecimalDigits="4" 
                          Value="3.1416" 
                          Height="25" 
                          Width="150"/>
    </StackPanel>
    
    <!-- 0 decimal places - Quantity -->
    <StackPanel Margin="0,10,0,0">
        <TextBlock Text="Quantity (0 decimals):"/>
        <syncfusion:UpDown Name="qtyUpDown" 
                          NumberDecimalDigits="0" 
                          Value="100" 
                          Height="25" 
                          Width="150"/>
    </StackPanel>
</StackPanel>
```

## Group Separator

### GroupSeperatorEnabled Property

Display thousand separators (also called group separators) in large numbers.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  Value="5555555" 
                  GroupSeperatorEnabled="True"
                  Width="100" 
                  Height="25" />
```

**C#:**
```csharp
updown.Value = 5555555;
updown.GroupSeperatorEnabled = true;
```

### Display Examples

**GroupSeperatorEnabled = False:**
```
5555555
1000000
```

**GroupSeperatorEnabled = True:**
```
5,555,555  (English locale)
5.555.555  (German locale)
5 555 555  (French locale)
```

The separator character depends on the culture settings.

### Combined Decimals and Separators

```csharp
updown.Value = 1234567.89;
updown.NumberDecimalDigits = 2;
updown.GroupSeperatorEnabled = true;

// Display: 1,234,567.89
```

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
| NumberGroupSeparator | "," | "/", " ", "'" |
| NumberDecimalSeparator | "." | "*", ",", "٫" |
| NumberDecimalDigits | 2 | 0, 1, 2, 4 |

### Examples of Custom Formats

**Format 1: European Style (space as thousand separator, comma as decimal)**
```csharp
var fmt = new NumberFormatInfo();
fmt.NumberGroupSeparator = " ";
fmt.NumberDecimalSeparator = ",";
fmt.NumberDecimalDigits = 2;

updown.NumberFormatInfo = fmt;
// 1 234 567,89
```

**Format 2: Indian Style (no group separator, period as decimal)**
```csharp
var fmt = new NumberFormatInfo();
fmt.NumberGroupSeparator = "";
fmt.NumberDecimalSeparator = ".";
fmt.NumberDecimalDigits = 3;

updown.NumberFormatInfo = fmt;
// 1234567.890
```

**Format 3: Currency-Like (pipe separator, currency symbol)**
```csharp
var fmt = new NumberFormatInfo();
fmt.NumberGroupSeparator = "|";
fmt.NumberDecimalSeparator = ".";
fmt.NumberDecimalDigits = 2;

updown.NumberFormatInfo = fmt;
// 5|555|555.89
```

## Culture and Localization

### Culture Property

Apply culture-specific formatting automatically. The Culture property uses CultureInfo to determine separators and format conventions.

**C# - Set Culture:**
```csharp
using System.Globalization;

// US English
updown.Culture = new CultureInfo("en-US");
updown.Value = 1234567.89;
// Display: 1,234,567.89

// German
updown.Culture = new CultureInfo("de-DE");
updown.Value = 1234567.89;
// Display: 1.234.567,89

// French
updown.Culture = new CultureInfo("fr-FR");
updown.Value = 1234567.89;
// Display: 1 234 567,89
```

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  Culture="bs-Latn" 
                  Value="5555555" 
                  Width="100" 
                  Height="25" />
```

### Supported Cultures

| Culture | Code | Format Example (1000.5) |
|---------|------|------------------------|
| English (US) | en-US | 1,000.5 |
| German | de-DE | 1.000,5 |
| French | fr-FR | 1 000,5 |
| Spanish | es-ES | 1.000,5 |
| Italian | it-IT | 1.000,5 |
| Russian | ru-RU | 1 000,5 |
| Chinese (Simplified) | zh-CN | 1,000.5 |
| Japanese | ja-JP | 1,000.5 |

### Culture and Group Separator Interaction

Enable group separator to see culture-specific thousand separators:

```csharp
updown.Value = 1234567.89;
updown.NumberDecimalDigits = 2;
updown.GroupSeperatorEnabled = true;
updown.Culture = new CultureInfo("de-DE");

// Display: 1.234.567,89 (German format)
```

### Complete Localization Example

```csharp
public void SetupLocalizedUpDown(string cultureCode)
{
    var cultureInfo = new CultureInfo(cultureCode);
    
    updown.Culture = cultureInfo;
    updown.NumberDecimalDigits = 2;
    updown.GroupSeperatorEnabled = true;
    updown.Value = 9999.99;
}

// Usage:
SetupLocalizedUpDown("en-US");     // 9,999.99
SetupLocalizedUpDown("de-DE");     // 9.999,99
SetupLocalizedUpDown("fr-FR");     // 9 999,99
```

## Text Alignment

### TextAlignment Property

Control horizontal alignment of the numeric text within the UpDown control.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  TextAlignment="Right" 
                  Value="100" 
                  Height="25" 
                  Width="150" />
```

**C#:**
```csharp
updown.TextAlignment = TextAlignment.Right;
```

### Alignment Options

**Left Alignment:**
```xaml
<syncfusion:UpDown TextAlignment="Left" Value="100" Width="100"/>
```
Display: `100               `

**Center Alignment:**
```xaml
<syncfusion:UpDown TextAlignment="Center" Value="100" Width="100"/>
```
Display: `       100        `

**Right Alignment (Default for numbers):**
```xaml
<syncfusion:UpDown TextAlignment="Right" Value="100" Width="100"/>
```
Display: `               100`

### Common Alignment Patterns

**Right-Aligned (Standard for numbers):**
```csharp
updown.TextAlignment = TextAlignment.Right;  // Best for numeric values
```

**Left-Aligned (For visual consistency with labels):**
```csharp
updown.TextAlignment = TextAlignment.Left;   // Match label alignment
```

**Center-Aligned (For emphasis):**
```csharp
updown.TextAlignment = TextAlignment.Center;  // Highlight important values
```

## Complete Number Formatting Example

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:globalization="clr-namespace:System.Globalization;assembly=mscorlib">
    <StackPanel Margin="20" Spacing="15">
        
        <!-- Price with USD format -->
        <StackPanel>
            <TextBlock Text="Price (USD):"/>
            <syncfusion:UpDown Name="priceUpDown"
                              NumberDecimalDigits="2"
                              GroupSeperatorEnabled="True"
                              TextAlignment="Right"
                              Value="1299.99"
                              Width="150"
                              Height="30"/>
        </StackPanel>
        
        <!-- International format -->
        <StackPanel>
            <TextBlock Text="International (German):"/>
            <syncfusion:UpDown Name="germanUpDown"
                              Culture="de-DE"
                              NumberDecimalDigits="2"
                              GroupSeperatorEnabled="True"
                              Value="1299.99"
                              Width="150"
                              Height="30"/>
        </StackPanel>
        
        <!-- Custom formatting -->
        <StackPanel>
            <TextBlock Text="Custom Format:"/>
            <syncfusion:UpDown Name="customUpDown"
                              Value="5555555"
                              GroupSeperatorEnabled="True"
                              Width="150"
                              Height="30">
                <syncfusion:UpDown.NumberFormatInfo>
                    <globalization:NumberFormatInfo 
                        NumberGroupSeparator="'" 
                        NumberDecimalDigits="2" 
                        NumberDecimalSeparator=","/>
                </syncfusion:UpDown.NumberFormatInfo>
            </syncfusion:UpDown>
        </StackPanel>
        
    </StackPanel>
</Window>
```

---

**Next:** Read [styling-and-appearance.md](styling-and-appearance.md) to customize visual appearance and colors.
