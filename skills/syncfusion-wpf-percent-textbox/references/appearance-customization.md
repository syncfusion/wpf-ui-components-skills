# Appearance Customization

## Table of Contents
- [Foreground Colors](#foreground-colors)
- [Background Customization](#background-customization)
- [Corner Radius](#corner-radius)
- [Selection Appearance](#selection-appearance)
- [Text Alignment](#text-alignment)
- [ToolTip Support](#tooltip-support)

## Foreground Colors

PercentTextBox allows different foreground colors based on the value's sign (positive, negative, or zero).

### Foreground for Positive Values

Set the foreground color when PercentValue is positive:

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           PositiveForeground="Blue"/>
```

**Result:** "50 %" displays in blue color

**C#:**
```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 50;
percentTextBox.PositiveForeground = Brushes.Blue;
```

**Default:** Black

**Use cases:**
- Financial gains (green)
- Above-target performance (blue)
- Approved percentages (dark green)

### Foreground for Negative Values

Set the foreground color when PercentValue is negative:

```xml
<syncfusion:PercentTextBox PercentValue="-25"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"/>
```

**Result:** "-25 %" displays in red color

**Important:** Must set `ApplyNegativeForeground="True"` to enable negative foreground coloring.

**C#:**
```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = -25;
percentTextBox.NegativeForeground = Brushes.Red;
percentTextBox.ApplyNegativeForeground = true; // Required!
```

**Default:** Red (but must be enabled)

**Use cases:**
- Financial losses (red)
- Below-target performance (dark red)
- Declined rates (orange-red)

### Foreground for Zero Values

Set the foreground color when PercentValue is exactly zero:

```xml
<syncfusion:PercentTextBox PercentValue="0"
                           ZeroColor="Gray"
                           ApplyZeroColor="True"/>
```

**Result:** "0 %" displays in gray color

**Important:** Must set `ApplyZeroColor="True"` to enable zero value coloring.

**C#:**
```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.PercentValue = 0;
percentTextBox.ZeroColor = Brushes.Gray;
percentTextBox.ApplyZeroColor = true; // Required!
```

**Default:** Green (but must be enabled)

**Use cases:**
- Neutral state indication
- Baseline values
- No change scenarios

### Complete Foreground Example

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="75"
                           PositiveForeground="Green"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"
                           ZeroColor="Gray"
                           ApplyZeroColor="True"/>
```

**Behavior:**
- Positive values (e.g., 75): Green
- Negative values (e.g., -50): Red
- Zero value: Gray

### Dynamic Foreground Changes

```csharp
// Changes color automatically based on value
private void UpdateValue(double newValue)
{
    percentTextBox.PercentValue = newValue;
    // Color updates automatically based on sign
}

// Example: Value changes from 50 → -20 → 0
percentTextBox.PercentValue = 50;   // Green
percentTextBox.PercentValue = -20;  // Red
percentTextBox.PercentValue = 0;    // Gray
```

### Custom Color Schemes

**Financial Dashboard:**
```xml
<syncfusion:PercentTextBox PositiveForeground="#006400"
                           NegativeForeground="#8B0000"
                           ApplyNegativeForeground="True"
                           ZeroColor="#696969"
                           ApplyZeroColor="True"/>
```

**Traffic Light Indicators:**
```xml
<syncfusion:PercentTextBox PositiveForeground="Green"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"
                           ZeroColor="Orange"
                           ApplyZeroColor="True"/>
```

## Background Customization

Set the control's background color or brush.

### Solid Color Background

```xml
<syncfusion:PercentTextBox Background="LightYellow"
                           PercentValue="50"/>
```

**C#:**
```csharp
percentTextBox.Background = Brushes.LightYellow;
```

### Gradient Background

```xml
<syncfusion:PercentTextBox PercentValue="50">
    <syncfusion:PercentTextBox.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="LightBlue" Offset="0"/>
            <GradientStop Color="White" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:PercentTextBox.Background>
</syncfusion:PercentTextBox>
```

### Transparent Background

```xml
<syncfusion:PercentTextBox Background="Transparent"
                           PercentValue="50"/>
```

### Conditional Background

```csharp
// Change background based on value
private void PercentTextBox_PercentValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    double value = (double)e.NewValue;
    
    if (value >= 80)
        percentTextBox.Background = Brushes.LightGreen;
    else if (value >= 50)
        percentTextBox.Background = Brushes.LightYellow;
    else
        percentTextBox.Background = Brushes.LightCoral;
}
```

**Default:** White

## Corner Radius

Create rounded corners for the control border.

### Basic Corner Radius

```xml
<syncfusion:PercentTextBox CornerRadius="5"
                           PercentValue="50"/>
```

**Result:** All four corners rounded with radius of 5

**C#:**
```csharp
percentTextBox.CornerRadius = new CornerRadius(5);
```

### Asymmetric Corner Radius

```xml
<!-- Different radius for each corner: TopLeft, TopRight, BottomRight, BottomLeft -->
<syncfusion:PercentTextBox CornerRadius="10,5,10,5"
                           PercentValue="50"/>
```

**Result:** Top-left and bottom-right have radius 10, others have radius 5

**C#:**
```csharp
// topLeft, topRight, bottomRight, bottomLeft
percentTextBox.CornerRadius = new CornerRadius(10, 5, 10, 5);
```

### Pill-Shaped Control

```xml
<syncfusion:PercentTextBox CornerRadius="15"
                           Height="30"
                           Width="150"
                           PercentValue="50"/>
```

**Result:** Fully rounded ends (pill shape)

### Square Corners (No Radius)

```xml
<syncfusion:PercentTextBox CornerRadius="0"
                           PercentValue="50"/>
```

**Default:** CornerRadius = 1

## Selection Appearance

Customize the appearance of selected text.

### Selection Brush

Change the background color of selected text:

```xml
<syncfusion:PercentTextBox SelectionBrush="LightBlue"
                           PercentValue="123456"/>
```

**Result:** Selected text has light blue background

**C#:**
```csharp
percentTextBox.SelectionBrush = Brushes.LightBlue;
```

### Selection Opacity

Control transparency of selection brush:

```xml
<syncfusion:PercentTextBox SelectionBrush="Red"
                           SelectionOpacity="0.3"
                           PercentValue="123456"/>
```

**Result:** Selected text has semi-transparent red background (30% opacity)

**Values:** 0.0 (fully transparent) to 1.0 (fully opaque)

**C#:**
```csharp
percentTextBox.SelectionBrush = Brushes.Red;
percentTextBox.SelectionOpacity = 0.3;
```

### Complete Selection Example

```xml
<syncfusion:PercentTextBox PercentValue="123456.78"
                           SelectionBrush="Yellow"
                           SelectionOpacity="0.5"
                           Foreground="Black"/>
```

**Result:** Selected text has 50% transparent yellow background with black text

### Custom Selection Styles

**Subtle Selection:**
```xml
<syncfusion:PercentTextBox SelectionBrush="LightGray"
                           SelectionOpacity="0.4"/>
```

**High Contrast Selection:**
```xml
<syncfusion:PercentTextBox SelectionBrush="Blue"
                           SelectionOpacity="0.8"
                           Foreground="White"/>
```

## Text Alignment

Control horizontal alignment of text within the control.

### Left Alignment (Default)

```xml
<syncfusion:PercentTextBox TextAlignment="Left"
                           Width="200"
                           PercentValue="50"/>
```

**Result:** "50 %" aligned to left side

### Center Alignment

```xml
<syncfusion:PercentTextBox TextAlignment="Center"
                           Width="200"
                           PercentValue="50"/>
```

**Result:** "50 %" centered in control

**Use cases:**
- Dashboard displays
- Summary cards
- Symmetric layouts

### Right Alignment

```xml
<syncfusion:PercentTextBox TextAlignment="Right"
                           Width="200"
                           PercentValue="50"/>
```

**Result:** "50 %" aligned to right side

**Use cases:**
- Financial tables
- Numeric columns
- Accounting layouts

**C#:**
```csharp
using System.Windows;

percentTextBox.TextAlignment = TextAlignment.Left;
percentTextBox.TextAlignment = TextAlignment.Center;
percentTextBox.TextAlignment = TextAlignment.Right;
```

### Alignment with Different Widths

```xml
<StackPanel>
    <!-- Narrow control -->
    <syncfusion:PercentTextBox Width="100" 
                               TextAlignment="Center"
                               PercentValue="50"/>
    
    <!-- Wide control -->
    <syncfusion:PercentTextBox Width="300" 
                               TextAlignment="Right"
                               PercentValue="50"/>
</StackPanel>
```

**Observation:** Alignment impact is more visible with wider controls

## ToolTip Support

Display additional information when user hovers over the control.

### Simple Text ToolTip

```xml
<syncfusion:PercentTextBox PercentValue="75"
                           ToolTip="Enter discount percentage"/>
```

**Result:** Hover shows "Enter discount percentage"

**C#:**
```csharp
percentTextBox.ToolTip = "Enter discount percentage";
```

### Dynamic ToolTip

```csharp
private void PercentTextBox_PercentValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    double value = (double)e.NewValue;
    percentTextBox.ToolTip = $"Current value: {value}%";
}
```

### Rich ToolTip Content

```xml
<syncfusion:PercentTextBox PercentValue="50">
    <syncfusion:PercentTextBox.ToolTip>
        <ToolTip>
            <StackPanel>
                <TextBlock Text="Discount Percentage" FontWeight="Bold"/>
                <TextBlock Text="Enter value between 0-100"/>
                <TextBlock Text="Format: 50.5 for 50.5%" FontStyle="Italic"/>
            </StackPanel>
        </ToolTip>
    </syncfusion:PercentTextBox.ToolTip>
</syncfusion:PercentTextBox>
```

### ToolTip with Validation Message

```csharp
percentTextBox.ToolTip = "Valid range: 0% to 100%";

// Update tooltip on validation error
if (percentTextBox.PercentValue > 100)
{
    percentTextBox.ToolTip = "Error: Value exceeds maximum (100%)";
}
```

### Custom ToolTip Style

```xml
<syncfusion:PercentTextBox PercentValue="50">
    <syncfusion:PercentTextBox.ToolTip>
        <ToolTip Background="LightYellow" 
                 Foreground="Black"
                 BorderBrush="Orange"
                 BorderThickness="2"
                 Padding="10">
            <TextBlock Text="Enter percentage value" FontSize="14"/>
        </ToolTip>
    </syncfusion:PercentTextBox.ToolTip>
</syncfusion:PercentTextBox>
```

## Complete Appearance Example

Combining multiple appearance properties:

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="200"
                           Height="35"
                           PercentValue="75.5"
                           
                           <!-- Foreground -->
                           PositiveForeground="Green"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"
                           ZeroColor="Gray"
                           ApplyZeroColor="True"
                           
                           <!-- Background -->
                           Background="LightYellow"
                           
                           <!-- Border -->
                           CornerRadius="5"
                           BorderBrush="Orange"
                           BorderThickness="2"
                           
                           <!-- Selection -->
                           SelectionBrush="LightBlue"
                           SelectionOpacity="0.5"
                           
                           <!-- Text -->
                           TextAlignment="Center"
                           FontSize="16"
                           FontWeight="Bold"
                           
                           <!-- Tooltip -->
                           ToolTip="Current percentage value"/>
```

## Appearance Patterns

### Pattern 1: Financial Dashboard Cell

```xml
<syncfusion:PercentTextBox PercentValue="{Binding ROI}"
                           PositiveForeground="#006400"
                           NegativeForeground="#8B0000"
                           ApplyNegativeForeground="True"
                           TextAlignment="Right"
                           Background="White"
                           BorderBrush="LightGray"
                           BorderThickness="1"
                           CornerRadius="3"/>
```

### Pattern 2: Modern Rounded Input

```xml
<syncfusion:PercentTextBox PercentValue="50"
                           CornerRadius="15"
                           Background="#F5F5F5"
                           BorderThickness="0"
                           Padding="10,5"
                           TextAlignment="Center"
                           SelectionBrush="DodgerBlue"
                           SelectionOpacity="0.3"/>
```

### Pattern 3: Alert-Style Indicator

```xml
<syncfusion:PercentTextBox PercentValue="-15"
                           NegativeForeground="White"
                           ApplyNegativeForeground="True"
                           Background="Red"
                           CornerRadius="5"
                           TextAlignment="Center"
                           FontWeight="Bold"
                           ToolTip="Below threshold!"/>
```

### Pattern 4: Minimal Clean Style

```xml
<syncfusion:PercentTextBox PercentValue="75"
                           Background="Transparent"
                           BorderThickness="0,0,0,1"
                           BorderBrush="Gray"
                           CornerRadius="0"
                           TextAlignment="Left"
                           SelectionBrush="LightGray"
                           SelectionOpacity="0.4"/>
```

## Best Practices

### 1. Use Color-Coding for Value States

```xml
<syncfusion:PercentTextBox PositiveForeground="Green"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"/>
```

### 2. Ensure Sufficient Contrast

```xml
<!-- Good: High contrast -->
<syncfusion:PercentTextBox Foreground="Black" Background="White"/>

<!-- Avoid: Low contrast -->
<syncfusion:PercentTextBox Foreground="LightGray" Background="White"/>
```

### 3. Keep Corner Radius Proportional to Height

```xml
<!-- Height 30 → CornerRadius 5 -->
<syncfusion:PercentTextBox Height="30" CornerRadius="5"/>

<!-- Height 50 → CornerRadius 8 -->
<syncfusion:PercentTextBox Height="50" CornerRadius="8"/>
```

### 4. Match TextAlignment to Layout

```xml
<!-- Tables: Right align -->
<syncfusion:PercentTextBox TextAlignment="Right"/>

<!-- Standalone: Center align -->
<syncfusion:PercentTextBox TextAlignment="Center"/>
```

### 5. Provide Informative ToolTips

```xml
<syncfusion:PercentTextBox ToolTip="Valid range: 0-100%, decimals allowed"/>
```
