# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Null Values](#null-values)
- [Watermark Text](#watermark-text)
- [Watermark Template](#watermark-template)
- [Paste Mode](#paste-mode)
- [Spin Buttons](#spin-buttons)
- [Range Adorner](#range-adorner)
- [Complete Advanced Example](#complete-advanced-example)

## Overview

CurrencyTextBox provides advanced features for enhanced user experience:

- **Null values** - Display specific values or text when null
- **Watermark text** - Placeholder text for empty fields
- **Custom watermark templates** - Rich visual watermarks
- **Advanced paste mode** - Intelligent clipboard insertion
- **Spin buttons** - UpDown buttons for value changes
- **Range adorner** - Visual progress indicator

## Null Values

Control how the control displays when the value is null or not set.

### NullValue Property

**Property:** `NullValue`  
**Type:** `double?`  
**Default:** `null`

**Property:** `UseNullOption`  
**Type:** `bool`  
**Default:** `false`

**⚠️ Critical:** You must set `UseNullOption = true` to enable null value support.

### Displaying Null

By default, CurrencyTextBox displays zero when value is null. With `UseNullOption = true` and `NullValue = null`, it displays nothing.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Height="25"
    Width="100" 
    UseNullOption="True"  
    NullValue="{x:Null}"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.NullValue = null;
currencyTextBox.UseNullOption = true;
```

**Result:** Control appears empty instead of showing "$0.00"

### Custom Null Value

Display a specific value when the control value is null.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Height="25"
    Width="100" 
    UseNullOption="True" 
    NullValue="10"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.NullValue = 10;
currencyTextBox.UseNullOption = true;
```

**Result:** When value is null, displays "$10.00"

### Use Cases for Null Values

1. **Optional fields** - Allow user to skip entry
2. **Default values** - Show suggested default when empty
3. **Placeholder amounts** - Template values for forms
4. **Calculation inputs** - Null until user enters data

### Null Value Priority

**Important:** If both `NullValue` and `WatermarkText` are set, `NullValue` takes precedence. You'll see the NullValue, not the watermark.

## Watermark Text

Display placeholder text when the control is empty and not focused.

### WatermarkText Property

**Property:** `WatermarkText`  
**Type:** `string`  
**Default:** `null`

**Property:** `WatermarkTextIsVisible`  
**Type:** `bool`  
**Default:** `false`

**Property:** `WatermarkTextForeground`  
**Type:** `Brush`  
**Default:** `Black`

**Requirements for watermark to show:**
1. `UseNullOption = true`
2. `WatermarkTextIsVisible = true`
3. Value is null or empty
4. Control does not have focus
5. `NullValue` is not set (or is null)

### Basic Watermark Example

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100"
    Height="25" 
    UseNullOption="True" 
    WatermarkText="Type here"
    WatermarkTextIsVisible="True"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.UseNullOption = true;
currencyTextBox.WatermarkText = "Type Here";
currencyTextBox.WatermarkTextIsVisible = true;
```

**Behavior:**
- Shows "Type here" when empty and unfocused
- Disappears when control gains focus
- Disappears when value is entered

### WatermarkTextForeground

Customize the watermark text color.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100"
    Height="25" 
    UseNullOption="True" 
    WatermarkText="Type here"
    WatermarkTextIsVisible="True" 
    WatermarkTextForeground="Red"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.UseNullOption = true;
currencyTextBox.WatermarkText = "Type Here";
currencyTextBox.WatermarkTextIsVisible = true;
currencyTextBox.WatermarkTextForeground = Brushes.Red;
```

### Watermark Use Cases

1. **Input hints** - "Enter amount"
2. **Format guidance** - "e.g., 1234.56"
3. **Field labels** - "Price" or "Quantity"
4. **Examples** - "Example: $99.99"
5. **Instructions** - "Optional - leave blank if not applicable"

## Watermark Template

Create rich, custom watermark appearances using DataTemplate.

### WatermarkTemplate Property

**Property:** `WatermarkTemplate`  
**Type:** `DataTemplate`

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="200" 
    Height="30"
    WatermarkText="Enter Amount" 
    CornerRadius="3" 
    WatermarkTextIsVisible="True" 
    WatermarkOpacity="0.5" 
    UseNullOption="True">
    <syncfusion:CurrencyTextBox.WatermarkTemplate>
        <DataTemplate>
            <Border Background="Red">
                <TextBlock 
                    Text="{Binding}" 
                    VerticalAlignment="Center" 
                    Margin="5,0,0,0"/>
            </Border>
        </DataTemplate>
    </syncfusion:CurrencyTextBox.WatermarkTemplate>
</syncfusion:CurrencyTextBox>
```

### Advanced Watermark Template

```xml
<syncfusion:CurrencyTextBox 
    Width="250" 
    Height="35"
    UseNullOption="True"
    WatermarkText="Price"
    WatermarkTextIsVisible="True">
    <syncfusion:CurrencyTextBox.WatermarkTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal" Margin="5,0">
                <!-- Icon -->
                <Path Data="M12,2A10,10 0 0,1 22,12A10,10 0 0,1 12,22A10,10 0 0,1 2,12A10,10 0 0,1 12,2M12,4A8,8 0 0,0 4,12A8,8 0 0,0 12,20A8,8 0 0,0 20,12A8,8 0 0,0 12,4M11,17V16H9V14H13V13H10A1,1 0 0,1 9,12V9A1,1 0 0,1 10,8H11V7H13V8H15V10H11V11H14A1,1 0 0,1 15,12V15A1,1 0 0,1 14,16H13V17H11Z"
                      Fill="LightGray"
                      Width="16"
                      Height="16"
                      Stretch="Uniform"
                      VerticalAlignment="Center"
                      Margin="0,0,5,0"/>
                
                <!-- Text -->
                <TextBlock 
                    Text="{Binding}" 
                    Foreground="Gray"
                    FontStyle="Italic"
                    VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:CurrencyTextBox.WatermarkTemplate>
</syncfusion:CurrencyTextBox>
```

### WatermarkOpacity

Control the opacity of the entire watermark.

```xml
<syncfusion:CurrencyTextBox 
    WatermarkText="Amount"
    WatermarkOpacity="0.7"
    UseNullOption="True"
    WatermarkTextIsVisible="True"/>
```

## Paste Mode

Control how clipboard content is inserted into the control.

### PasteMode Property

**Property:** `PasteMode`  
**Type:** `PasteMode`  
**Default:** `Default`  
**Values:** `Default`, `Advanced`

### Default Mode

Replaces the entire value with the clipboard content.

**Example:**
- Current value: $1,234.56
- Clipboard: 789
- Result after paste: $789.00

### Advanced Mode

Intelligently inserts clipboard content based on cursor position and selection.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    PasteMode="Advanced" 
    Value="12345.67"
    Name="currencyTextBox"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.PasteMode = PasteMode.Advanced;
currencyTextBox.Value = 12345.67;
```

### Advanced Paste Mode Behavior

| Scenario | Behavior |
|----------|----------|
| **Whole value selected** | Replaces entire value |
| **Cursor at position, no decimal in clipboard** | Inserts at cursor position |
| **Cursor at position, decimal in clipboard** | No paste (prevents duplicate decimals) |
| **Control value is 0 or null** | Replaces entire value |
| **Selected part contains decimal, clipboard has decimal** | Allowed |
| **Selected part contains decimal, clipboard no decimal** | Not allowed |
| **Selected part no decimal, clipboard has decimal** | Not allowed |
| **Selected part no decimal, clipboard no decimal** | Allowed |

### Advanced Paste Examples

**Example 1: Insert at cursor**
- Value: $1,234.56
- Cursor after '4': $1,234|.56
- Clipboard: 99
- Result: $123,499.56

**Example 2: Prevent invalid paste**
- Value: $1,234.56
- Cursor after '4': $1,234|.56
- Clipboard: 99.99 (contains decimal)
- Result: No paste (would create invalid number)

**Example 3: Replace selection**
- Value: $1,234.56
- Selection: '34' is selected
- Clipboard: 88
- Result: $1,288.56

## Spin Buttons

Add UpDown buttons to increment/decrement the value.

### ShowSpinButton Property

**Property:** `ShowSpinButton`  
**Type:** `bool`  
**Default:** `false`

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    Height="30" 
    Width="150" 
    ShowSpinButton="True" />
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.ShowSpinButton = true;
```

**Features:**
- **Up button** - Increases value by ScrollInterval
- **Down button** - Decreases value by ScrollInterval
- **Respects Min/Max** - Won't exceed boundaries
- **Visual feedback** - Hover and press states

### Spin Button Example with Configuration

```xml
<syncfusion:CurrencyTextBox 
    Width="150" 
    Height="35"
    Value="100"
    MinValue="0"
    MaxValue="1000"
    ScrollInterval="25"
    ShowSpinButton="True"/>
```

**Behavior:**
- Click Up: 100 → 125 → 150 → ...
- Click Down: 100 → 75 → 50 → ...
- Stops at MaxValue (1000) or MinValue (0)

### Use Cases

1. **Quantity selectors** - Product quantity in shopping cart
2. **Age input** - Restricted numeric ranges
3. **Time inputs** - Minutes, seconds with fixed increments
4. **Volume controls** - Audio/video level adjustment
5. **Zoom levels** - Percentage-based controls

## Range Adorner

Display a visual progress indicator showing the value's position within min/max range.

### EnableRangeAdorner Property

**Property:** `EnableRangeAdorner`  
**Type:** `bool`  
**Default:** `false`

**Property:** `RangeAdornerBackground`  
**Type:** `Brush`  
**Default:** System default

**⚠️ Important:** Range adorner only displays when both `MinValue` and `MaxValue` are set.

### Basic Range Adorner

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    MinValue="0" 
    MaxValue="100" 
    Value="630"  
    EnableRangeAdorner="True" />
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.MinValue = 0;
currencyTextBox.MaxValue = 1000;
currencyTextBox.Value = 630;
currencyTextBox.EnableRangeAdorner = true;
```

**Visual effect:** A colored bar fills 63% of the control background (630 out of 1000).

### How Range Adorner Calculation Works

```
Fill Percentage = (Value - MinValue) / (MaxValue - MinValue) * 100%
```

**Examples:**
- MinValue=0, MaxValue=100, Value=50 → 50% fill
- MinValue=100, MaxValue=200, Value=150 → 50% fill
- MinValue=0, MaxValue=1000, Value=750 → 75% fill

### Customizing Adorner Color

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    MinValue="0" 
    MaxValue="1000" 
    Value="570"  
    EnableRangeAdorner="True" 
    RangeAdornerBackground="LightGreen"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.MinValue = 0;
currencyTextBox.MaxValue = 100;
currencyTextBox.Value = 570;
currencyTextBox.EnableRangeAdorner = true;
currencyTextBox.RangeAdornerBackground = Brushes.LightGreen;
```

### Dynamic Color Based on Value

```csharp
currencyTextBox.ValueChanged += (d, e) =>
{
    double percentage = (currencyTextBox.Value - currencyTextBox.MinValue) / 
                        (currencyTextBox.MaxValue - currencyTextBox.MinValue);
    
    if (percentage < 0.33)
        currencyTextBox.RangeAdornerBackground = Brushes.Red; // Low range
    else if (percentage < 0.66)
        currencyTextBox.RangeAdornerBackground = Brushes.Yellow; // Mid range
    else
        currencyTextBox.RangeAdornerBackground = Brushes.Green; // High range
};
```

### Range Adorner Use Cases

1. **Progress indicators** - Savings goals, fundraising
2. **Capacity displays** - Storage usage, budget spending
3. **Performance metrics** - Sales targets, KPIs
4. **Resource meters** - Battery level, fuel gauge
5. **Completion tracking** - Project milestones, task progress

### Gradient Range Adorner

```xml
<syncfusion:CurrencyTextBox 
    MinValue="0" 
    MaxValue="1000" 
    Value="650"
    EnableRangeAdorner="True"
    Width="200"
    Height="35">
    <syncfusion:CurrencyTextBox.RangeAdornerBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="Red" Offset="0"/>
            <GradientStop Color="Yellow" Offset="0.5"/>
            <GradientStop Color="Green" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:CurrencyTextBox.RangeAdornerBackground>
</syncfusion:CurrencyTextBox>
```

## Complete Advanced Example

Combining all advanced features:

**XAML:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Advanced Features Demo" Height="450" Width="500">
    <StackPanel Margin="30">
        <!-- With watermark and null support -->
        <TextBlock Text="Optional Amount:" Margin="0,0,0,5" FontWeight="SemiBold"/>
        <syncfusion:CurrencyTextBox 
            Width="250"
            Height="35"
            UseNullOption="True"
            WatermarkText="Enter amount (optional)"
            WatermarkTextIsVisible="True"
            WatermarkTextForeground="Gray"
            PasteMode="Advanced"
            CornerRadius="5"
            Margin="0,0,0,20"/>
        
        <!-- With spin buttons -->
        <TextBlock Text="Quantity:" Margin="0,0,0,5" FontWeight="SemiBold"/>
        <syncfusion:CurrencyTextBox 
            Width="250"
            Height="35"
            Value="10"
            MinValue="0"
            MaxValue="100"
            ScrollInterval="5"
            ShowSpinButton="True"
            CornerRadius="5"
            Margin="0,0,0,20"/>
        
        <!-- With range adorner -->
        <TextBlock Text="Budget Progress:" Margin="0,0,0,5" FontWeight="SemiBold"/>
        <syncfusion:CurrencyTextBox 
            Width="250"
            Height="40"
            Value="7500"
            MinValue="0"
            MaxValue="10000"
            EnableRangeAdorner="True"
            RangeAdornerBackground="LightBlue"
            IsReadOnly="True"
            FontSize="16"
            TextAlignment="Center"
            CornerRadius="5"
            ToolTip="$7,500 of $10,000 budget used"/>
    </StackPanel>
</Window>
```

**Code-behind with advanced features:**

```csharp
using System.Windows;
using System.Windows.Media;
using Syncfusion.Windows.Shared;

namespace AdvancedFeaturesDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create with all advanced features
            CurrencyTextBox advancedTextBox = new CurrencyTextBox
            {
                Width = 250,
                Height = 40,
                Value = 500,
                MinValue = 0,
                MaxValue = 1000,
                ScrollInterval = 50,
                
                // Null value support
                UseNullOption = true,
                WatermarkText = "Enter target amount",
                WatermarkTextIsVisible = true,
                WatermarkTextForeground = Brushes.Gray,
                
                // Paste mode
                PasteMode = PasteMode.Advanced,
                
                // Spin buttons
                ShowSpinButton = true,
                
                // Range adorner
                EnableRangeAdorner = true,
                RangeAdornerBackground = Brushes.LightGreen
            };
            
            // Dynamic adorner color
            advancedTextBox.ValueChanged += (d, e) =>
            {
                double newValue = (double)e.NewValue;
                double percentage = newValue / advancedTextBox.MaxValue;
                
                if (percentage < 0.5)
                    advancedTextBox.RangeAdornerBackground = Brushes.LightCoral;
                else if (percentage < 0.8)
                    advancedTextBox.RangeAdornerBackground = Brushes.LightGoldenrodYellow;
                else
                    advancedTextBox.RangeAdornerBackground = Brushes.LightGreen;
            };
        }
    }
}
```

## Best Practices

1. **Use watermarks for guidance** - Help users understand expected input
2. **Enable UseNullOption for optional fields** - Better UX than showing $0.00
3. **Advanced paste mode for power users** - More intuitive editing
4. **Spin buttons for constrained inputs** - Clearer for limited ranges
5. **Range adorner for visual feedback** - Show progress/capacity at a glance
6. **Combine features thoughtfully** - Don't overuse; match user needs
7. **Test null value scenarios** - Ensure proper handling in data binding

## Troubleshooting

**Issue:** Watermark not showing  
**Solution:** Ensure `UseNullOption = true`, `WatermarkTextIsVisible = true`, value is null, and `NullValue` is not set

**Issue:** Range adorner not visible  
**Solution:** Both `MinValue` and `MaxValue` must be set, and `EnableRangeAdorner = true`

**Issue:** Spin buttons not appearing  
**Solution:** Set `ShowSpinButton = true`

**Issue:** Advanced paste not working  
**Solution:** Set `PasteMode = PasteMode.Advanced`

**Issue:** Null value shows 0  
**Solution:** Set `UseNullOption = true`

**Issue:** NullValue takes precedence over watermark  
**Solution:** This is expected behavior; use one or the other, not both
