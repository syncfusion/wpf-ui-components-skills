# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Foreground Colors](#foreground-colors)
- [Background](#background)
- [Corner Radius](#corner-radius)
- [Selection Styling](#selection-styling)
- [Text Alignment](#text-alignment)
- [Tooltip](#tooltip)
- [Complete Styling Example](#complete-styling-example)

## Overview

CurrencyTextBox provides extensive appearance customization options:

- **Dynamic foreground colors** - Different colors for positive, negative, and zero values
- **Background customization** - Custom background colors
- **Rounded corners** - Corner radius for modern UI design
- **Selection styling** - Custom colors for selected text
- **Text alignment** - Left, center, or right alignment
- **Tooltips** - Informational tooltips

## Foreground Colors

The CurrencyTextBox can display different foreground colors based on the value's sign (positive, negative, or zero).

### Positive Value Foreground

**Property:** `PositiveForeground`  
**Type:** `Brush`  
**Default:** `Black`

Applied when the value is positive (> 0).

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Value="10" 
    Width="100" 
    Height="25" 
    PositiveForeground="Blue" />
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.Value = 10;
currencyTextBox.PositiveForeground = Brushes.Blue;
```

**Result:** The text "$10.00" appears in blue.

### Negative Value Foreground

**Property:** `NegativeForeground`  
**Type:** `Brush`  
**Default:** `Red`

**Property:** `ApplyNegativeForeground`  
**Type:** `bool`  
**Default:** `false`

Applied when the value is negative (< 0) and `ApplyNegativeForeground` is `true`.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Value="-10" 
    Width="100" 
    Height="25"
    NegativeForeground="SpringGreen" 
    ApplyNegativeForeground="True" />
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.Value = -10;
currencyTextBox.ApplyNegativeForeground = true;   
currencyTextBox.NegativeForeground = Brushes.SpringGreen;
```

**⚠️ Important:** You must set `ApplyNegativeForeground = true` for the negative foreground to take effect.

**Result:** The text "-$10.00" appears in spring green.

### Zero Value Foreground

**Property:** `ZeroColor`  
**Type:** `Brush`  
**Default:** `Green`

**Property:** `ApplyZeroColor`  
**Type:** `bool`  
**Default:** `false`

Applied when the value is exactly zero and `ApplyZeroColor` is `true`.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Value="0" 
    Width="100" 
    Height="25"
    ApplyZeroColor="True" 
    ZeroColor="DarkGoldenrod"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.Value = 0;
currencyTextBox.ApplyZeroColor = true;
currencyTextBox.ZeroColor = Brushes.DarkGoldenrod;
```

**⚠️ Important:** You must set `ApplyZeroColor = true` for the zero color to take effect.

**Result:** The text "$0.00" appears in dark goldenrod.

### Combined Foreground Example

**XAML:**

```xml
<StackPanel Margin="20">
    <!-- Positive value (blue) -->
    <syncfusion:CurrencyTextBox 
        Value="100" 
        Width="150" 
        Height="30"
        PositiveForeground="Blue"
        Margin="0,0,0,10"/>
    
    <!-- Negative value (red) -->
    <syncfusion:CurrencyTextBox 
        Value="-50" 
        Width="150" 
        Height="30"
        NegativeForeground="Red"
        ApplyNegativeForeground="True"
        Margin="0,0,0,10"/>
    
    <!-- Zero value (gray) -->
    <syncfusion:CurrencyTextBox 
        Value="0" 
        Width="150" 
        Height="30"
        ZeroColor="Gray"
        ApplyZeroColor="True"/>
</StackPanel>
```

### Financial Dashboard Example

```csharp
// Color-code profit/loss in a financial dashboard
CurrencyTextBox profitLossTextBox = new CurrencyTextBox();
profitLossTextBox.Width = 150;
profitLossTextBox.Height = 30;

// Green for profit, red for loss, yellow for break-even
profitLossTextBox.PositiveForeground = Brushes.Green;
profitLossTextBox.NegativeForeground = Brushes.Red;
profitLossTextBox.ApplyNegativeForeground = true;
profitLossTextBox.ZeroColor = Brushes.Orange;
profitLossTextBox.ApplyZeroColor = true;

// Value determines color
profitLossTextBox.Value = -1500; // Shows in red
```

## Background

Customize the control's background color.

**Property:** `Background`  
**Type:** `Brush`  
**Default:** `White`

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100"
    Height="25" 
    Value="80" 
    Background="Cyan"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.Background = Brushes.Cyan;
```

### Gradient Background Example

```xml
<syncfusion:CurrencyTextBox Width="150" Height="30" Value="1234.56">
    <syncfusion:CurrencyTextBox.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="LightBlue" Offset="0"/>
            <GradientStop Color="White" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:CurrencyTextBox.Background>
</syncfusion:CurrencyTextBox>
```

### Disabled State Background

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.IsEnabled = false;
currencyTextBox.Background = Brushes.LightGray; // Visual indicator of disabled state
```

## Corner Radius

Create rounded borders using the `CornerRadius` property.

**Property:** `CornerRadius`  
**Type:** `CornerRadius`  
**Default:** `1`

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100" 
    Height="25" 
    CornerRadius="5"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.CornerRadius = new CornerRadius(5);
```

### Corner Radius Variations

**Uniform radius (all corners equal):**
```csharp
currencyTextBox.CornerRadius = new CornerRadius(10);
```

**Individual corner control:**
```csharp
// topLeft, topRight, bottomRight, bottomLeft
currencyTextBox.CornerRadius = new CornerRadius(10, 0, 10, 0);
```

**Pill-shaped (fully rounded):**
```csharp
currencyTextBox.Height = 30;
currencyTextBox.CornerRadius = new CornerRadius(15); // Half of height
```

### Modern UI Example

```xml
<syncfusion:CurrencyTextBox 
    Width="200" 
    Height="35"
    Value="999.99"
    CornerRadius="17.5"
    Background="#F0F0F0"
    BorderBrush="#CCCCCC"
    BorderThickness="1"
    Padding="10,0"/>
```

## Selection Styling

Customize the appearance of selected text.

### SelectionBrush

**Property:** `SelectionBrush`  
**Type:** `Brush`  
**Default:** System default (usually light blue)

**Property:** `SelectionOpacity`  
**Type:** `double`  
**Default:** `1.0`  
**Range:** `0.0` to `1.0`

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100" 
    Height="25" 
    SelectionBrush="Red" 
    SelectionOpacity="0.5"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.SelectionBrush = Brushes.Red;
currencyTextBox.SelectionOpacity = 0.3;
```

**Effect:** When user selects text, the selection background appears as semi-transparent red.

### Selection Examples

**Subtle selection:**
```csharp
currencyTextBox.SelectionBrush = Brushes.LightGray;
currencyTextBox.SelectionOpacity = 0.5;
```

**High contrast selection:**
```csharp
currencyTextBox.SelectionBrush = Brushes.Yellow;
currencyTextBox.SelectionOpacity = 1.0;
```

**Brand-colored selection:**
```csharp
currencyTextBox.SelectionBrush = new SolidColorBrush(Color.FromRgb(0, 120, 215)); // Brand blue
currencyTextBox.SelectionOpacity = 0.6;
```

## Text Alignment

Control horizontal text alignment within the control.

**Property:** `TextAlignment`  
**Type:** `TextAlignment`  
**Default:** `Left`  
**Values:** `Left`, `Center`, `Right`, `Justify`

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100" 
    Height="25" 
    TextAlignment="Center"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.TextAlignment = TextAlignment.Center;
```

### Alignment Use Cases

**Left (default):**
- Standard text input behavior
- Most form fields
- Mixed content layouts

**Center:**
- Totals and summaries
- Dashboard displays
- Centered UI designs
- Calculator-style inputs

**Right:**
- Financial statements (traditional accounting)
- Columnar numeric data
- Align with right-aligned labels
- Spreadsheet-style layouts

### Alignment Example

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <TextBlock Grid.Row="0" Text="Subtotal:" HorizontalAlignment="Right" Margin="0,0,10,5"/>
    <syncfusion:CurrencyTextBox Grid.Row="0" Value="1000" TextAlignment="Right" Width="150" HorizontalAlignment="Right"/>
    
    <TextBlock Grid.Row="1" Text="Tax:" HorizontalAlignment="Right" Margin="0,0,10,5"/>
    <syncfusion:CurrencyTextBox Grid.Row="1" Value="80" TextAlignment="Right" Width="150" HorizontalAlignment="Right"/>
    
    <TextBlock Grid.Row="2" Text="Total:" HorizontalAlignment="Right" Margin="0,0,10,5" FontWeight="Bold"/>
    <syncfusion:CurrencyTextBox Grid.Row="2" Value="1080" TextAlignment="Right" Width="150" HorizontalAlignment="Right" FontWeight="Bold"/>
</Grid>
```

## Tooltip

Display informational tooltips when user hovers over the control.

**Property:** `ToolTip`  
**Type:** `object`  
**Default:** `null`

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100" 
    Height="25" 
    ToolTip="Enter Currency Value"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.ToolTip = "Enter Currency Value";
```

### Custom Tooltip

```xml
<syncfusion:CurrencyTextBox Width="150" Height="30" Value="100">
    <syncfusion:CurrencyTextBox.ToolTip>
        <ToolTip>
            <StackPanel>
                <TextBlock Text="Product Price" FontWeight="Bold"/>
                <TextBlock Text="Range: $0 - $10,000" FontSize="10"/>
                <TextBlock Text="Use arrow keys to adjust" FontSize="10" Foreground="Gray"/>
            </StackPanel>
        </ToolTip>
    </syncfusion:CurrencyTextBox.ToolTip>
</syncfusion:CurrencyTextBox>
```

### Dynamic Tooltip

```csharp
currencyTextBox.ValueChanged += (d, e) =>
{
    currencyTextBox.ToolTip = $"Current value: {e.NewValue:C2}";
};
```

### Tooltip Use Cases

1. **Instructions** - "Enter amount between $0 and $1000"
2. **Validation hints** - "Required field - must be greater than 0"
3. **Format guidance** - "Use arrow keys or mouse wheel to adjust"
4. **Context help** - "This is the net profit after taxes"
5. **Keyboard shortcuts** - "Press Ctrl+Up to increment by 100"

## Complete Styling Example

Here's a comprehensive example combining all appearance features:

**XAML:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Currency TextBox Styling" Height="400" Width="500">
    <StackPanel Margin="30">
        <!-- Modern styled input -->
        <TextBlock Text="Premium Amount:" Margin="0,0,0,5" FontWeight="SemiBold"/>
        <syncfusion:CurrencyTextBox 
            Width="250"
            Height="40"
            Value="1234.56"
            CornerRadius="20"
            Background="#F8F9FA"
            BorderBrush="#DEE2E6"
            BorderThickness="2"
            Padding="15,0"
            FontSize="16"
            TextAlignment="Center"
            ToolTip="Enter premium amount ($0 - $100,000)"
            Margin="0,0,0,20"/>
        
        <!-- Financial dashboard style -->
        <TextBlock Text="Profit/Loss:" Margin="0,0,0,5" FontWeight="SemiBold"/>
        <syncfusion:CurrencyTextBox 
            Width="250"
            Height="35"
            Value="-500"
            PositiveForeground="Green"
            NegativeForeground="Red"
            ApplyNegativeForeground="True"
            ZeroColor="Gray"
            ApplyZeroColor="True"
            Background="White"
            CornerRadius="5"
            TextAlignment="Right"
            FontSize="14"
            FontWeight="Bold"
            SelectionBrush="Yellow"
            SelectionOpacity="0.6"
            Margin="0,0,0,20"/>
        
        <!-- Read-only display -->
        <TextBlock Text="Total Revenue:" Margin="0,0,0,5" FontWeight="SemiBold"/>
        <syncfusion:CurrencyTextBox 
            Width="250"
            Height="35"
            Value="98765.43"
            IsReadOnly="True"
            Background="LightGray"
            Foreground="DarkSlateGray"
            TextAlignment="Right"
            FontSize="16"
            FontWeight="Bold"
            CornerRadius="5"
            ToolTip="Calculated field - cannot be edited"/>
    </StackPanel>
</Window>
```

**Code-behind for dynamic styling:**

```csharp
using System.Windows;
using System.Windows.Media;
using Syncfusion.Windows.Shared;

namespace StylingDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create styled currency textbox programmatically
            CurrencyTextBox styledTextBox = new CurrencyTextBox
            {
                Width = 250,
                Height = 40,
                Value = 5000,
                CornerRadius = new CornerRadius(10),
                Background = new SolidColorBrush(Color.FromRgb(248, 249, 250)),
                BorderBrush = new SolidColorBrush(Color.FromRgb(108, 117, 125)),
                BorderThickness = new Thickness(1),
                Padding = new Thickness(10, 0, 10, 0),
                TextAlignment = TextAlignment.Center,
                FontSize = 14,
                ToolTip = "Styled programmatically"
            };
            
            // Add to layout
            // yourPanel.Children.Add(styledTextBox);
        }
    }
}
```

## Best Practices

1. **Use semantic colors** - Green for positive, red for negative follows conventions
2. **Maintain contrast** - Ensure text is readable against background
3. **Consistent corner radius** - Match other controls in your application
4. **Subtle selection colors** - Don't distract from content
5. **Right-align financial data** - Traditional accounting format
6. **Provide helpful tooltips** - Guide users on valid input ranges
7. **Test accessibility** - Verify color combinations meet WCAG standards
8. **Match brand guidelines** - Use company colors appropriately

## Accessibility Considerations

- **Color alone is not enough** - Don't rely solely on color to convey information
- **Sufficient contrast** - Foreground/background must meet contrast ratios (4.5:1 minimum)
- **Tooltips supplement** - Should not contain essential information unavailable elsewhere
- **Font size** - Minimum 12pt for readability
- **Focus indicators** - Ensure focus state is clearly visible

## Troubleshooting

**Issue:** Negative foreground not showing  
**Solution:** Set `ApplyNegativeForeground = true`

**Issue:** Zero color not showing  
**Solution:** Set `ApplyZeroColor = true`

**Issue:** Corner radius not visible  
**Solution:** Ensure BorderBrush is set to see the rounded border

**Issue:** Selection color not changing  
**Solution:** Check SelectionBrush and SelectionOpacity are both set

**Issue:** Tooltip not appearing  
**Solution:** Ensure ToolTip property is set and hover over control

**Issue:** Text alignment not working  
**Solution:** Verify TextAlignment property is set correctly (Left/Center/Right)
