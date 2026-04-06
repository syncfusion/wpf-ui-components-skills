# Tooltips in WPF Range Slider

## Table of Contents

1. [Overview](#overview)
2. [Thumb ToolTip Basics](#thumb-tooltip-basics)
3. [ToolTip Precision](#tooltip-precision)
4. [ToolTip Format](#tooltip-format)
5. [ToolTip Position](#tooltip-position)
   - [BottomRight Position](#bottomright-position)
   - [TopLeft Position](#topleft-position)
   - [None Position](#none-position)
6. [ToolTip Display Mode](#tooltip-display-mode)
   - [Always Display](#always-display)
   - [OnThumbMove Display](#onthumbmove-display)
   - [Never Display](#never-display)
7. [ToolTip Style Customization](#tooltip-style-customization)
   - [Basic Style Customization](#basic-style-customization)
   - [Advanced Style with Icons](#advanced-style-with-icons)
8. [Thumb Interval](#thumb-interval)
9. [Best Practices](#best-practices)
10. [Troubleshooting](#troubleshooting)
11. [Complete Examples](#complete-examples)

## Overview

The Thumb tooltip in WPF Range Slider displays the current value where the Thumb stands. This provides immediate visual feedback to users as they interact with the slider, showing the exact value being selected.

**Key Features:**
- Displays current thumb value
- Customizable precision and format
- Flexible positioning options
- Configurable display modes
- Full styling support

## Thumb ToolTip Basics

By default, the Range Slider displays a tooltip when the user interacts with the thumb. The tooltip shows the current value of the slider at the thumb position.

```xaml
<editors:SfRangeSlider
    Width="300"
    VerticalAlignment="Center"
    Maximum="100"
    Minimum="0"
    Value="50" />
```

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

## ToolTip Precision

The `ThumbToolTipPrecision` property defines the number of decimal places displayed in the tooltip. This is particularly useful when working with decimal values.

**Property:** `ThumbToolTipPrecision`  
**Type:** `int`  
**Default Value:** `0`

### XAML Example

```xaml
<editors:SfRangeSlider
    Width="300"
    VerticalAlignment="Center"
    Maximum="100"
    Minimum="0"
    ThumbToolTipPrecision="2"
    Value="50.567" />
```

### C# Example

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ThumbToolTipPrecision = 2,
    Value = 50.567
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

**Output:** The tooltip will display `50.57` instead of `50.567`

> **Note:** The `ThumbToolTipPrecision` property is only applicable when the `ToolTipFormat` value is `N` (numeric format).

## ToolTip Format

The `ToolTipFormat` property specifies the format specifier for the tooltip display value. You can use standard .NET format strings to customize how values are displayed.

**Property:** `ToolTipFormat`  
**Type:** `string`  
**Default Value:** `"N"`

### Common Format Specifiers

- `N` or `N0`: Numeric format without decimal places
- `N2`: Numeric format with 2 decimal places
- `C` or `C0`: Currency format without decimal places
- `C2`: Currency format with 2 decimal places
- `P`: Percentage format
- `F2`: Fixed-point format with 2 decimal places
- `#,0`: Custom format with thousand separator

### XAML Example - Currency Format

```xaml
<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    ToolTipFormat="C0"
    Value="50" />
```

### C# Example - Currency Format

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ToolTipFormat = "C0",
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

**Output:** The tooltip will display `$50` (or currency symbol based on culture)

### XAML Example - Percentage Format

```xaml
<editors:SfRangeSlider
    Width="300"
    Maximum="1"
    Minimum="0"
    ToolTipFormat="P0"
    Value="0.5" />
```

### C# Example - Percentage Format

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 1,
    Minimum = 0,
    ToolTipFormat = "P0",
    Value = 0.5
};
```

**Output:** The tooltip will display `50%`

## ToolTip Position

The `ThumbToolTipPlacement` property controls the position of the tooltip in relation to the thumb. It provides three positioning options.

**Property:** `ThumbToolTipPlacement`  
**Type:** `ThumbToolTipPlacement` enum  
**Values:** `BottomRight`, `TopLeft`, `None`

### BottomRight Position

Tooltip is placed either below the thumb in horizontal orientation or right of the thumb in vertical orientation.

**XAML Example:**

```xaml
<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    ThumbToolTipPlacement="BottomRight"
    Value="50" />
```

**C# Example:**

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ThumbToolTipPlacement = ThumbToolTipPlacement.BottomRight,
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

### TopLeft Position

Tooltip is placed either above the thumb in horizontal orientation or left of the thumb in vertical orientation.

**XAML Example:**

```xaml
<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    ThumbToolTipPlacement="TopLeft"
    Value="50" />
```

**C# Example:**

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ThumbToolTipPlacement = ThumbToolTipPlacement.TopLeft,
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

### None Position

No tooltip appears when this option is set.

**XAML Example:**

```xaml
<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    ThumbToolTipPlacement="None"
    Value="50" />
```

**C# Example:**

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ThumbToolTipPlacement = ThumbToolTipPlacement.None,
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

## ToolTip Display Mode

The `ToolTipDisplayMode` property determines when the tooltip should be displayed during user interaction.

**Property:** `ToolTipDisplayMode`  
**Type:** `ToolTipDisplayMode` enum  
**Values:** `Always`, `OnFocus`  
**Default Value:** `OnFocus`

### Always Display

When set to `Always`, the tooltip is always visible regardless of user interaction.

**XAML Example:**

```xaml
<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    ToolTipDisplayMode="Always"
    ThumbToolTipPlacement="TopLeft"
    Value="50" />
```

**C# Example:**

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ToolTipDisplayMode = ToolTipDisplayMode.Always,
    ThumbToolTipPlacement = ThumbToolTipPlacement.TopLeft,
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

**Use Case:** Best for dashboards or monitoring interfaces where the value should always be visible.

### OnFocus Display

When set to `OnFocus`, the tooltip appears only when the thumb is being dragged. This is the default behavior.

**XAML Example:**

```xaml
<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    ToolTipDisplayMode="OnFocus"
    ThumbToolTipPlacement="TopLeft"
    Value="50" />
```

**C# Example:**

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ToolTipDisplayMode = ToolTipDisplayMode.OnFocus,
    ThumbToolTipPlacement = ThumbToolTipPlacement.TopLeft,
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

**Use Case:** Standard behavior for most interactive sliders, providing feedback during adjustment.


## ToolTip Style Customization

The `ToolTipStyle` property allows complete customization of the tooltip appearance. You can define custom styles with properties like Background, Foreground, BorderBrush, BorderThickness, FontSize, FontFamily, and more.

**Property:** `ToolTipStyle`  
**Type:** `Style`  
**Target Type:** `ToolTip`

### Basic Style Customization

**XAML Example:**

```xaml
<Window.Resources>
    <Style x:Key="CustomToolTipStyle" TargetType="ToolTip">
        <Setter Property="Background" Value="#FF4CAF50"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="BorderBrush" Value="#FF2E7D32"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="FontWeight" Value="Bold"/>
        <Setter Property="Padding" Value="10,5"/>
        <Setter Property="HasDropShadow" Value="True"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ToolTip">
                    <Border Background="{TemplateBinding Background}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}"
                            CornerRadius="5"
                            Padding="{TemplateBinding Padding}">
                        <ContentPresenter Content="{TemplateBinding Content}"
                                        ContentTemplate="{TemplateBinding ContentTemplate}"
                                        HorizontalAlignment="Center"
                                        VerticalAlignment="Center"/>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    ToolTipStyle="{StaticResource CustomToolTipStyle}"
    ThumbToolTipPlacement="TopLeft"
    ToolTipFormat="N2"
    Value="50" />
```

**C# Example:**

```csharp
Grid parentGrid = new Grid();

// Define custom tooltip style
Style customToolTipStyle = new Style(typeof(ToolTip));
customToolTipStyle.Setters.Add(new Setter(ToolTip.BackgroundProperty, 
    new SolidColorBrush((Color)ColorConverter.ConvertFromString("#FF4CAF50"))));
customToolTipStyle.Setters.Add(new Setter(ToolTip.ForegroundProperty, Brushes.White));
customToolTipStyle.Setters.Add(new Setter(ToolTip.BorderBrushProperty, 
    new SolidColorBrush((Color)ColorConverter.ConvertFromString("#FF2E7D32"))));
customToolTipStyle.Setters.Add(new Setter(ToolTip.BorderThicknessProperty, new Thickness(2)));
customToolTipStyle.Setters.Add(new Setter(ToolTip.FontSizeProperty, 14.0));
customToolTipStyle.Setters.Add(new Setter(ToolTip.FontWeightProperty, FontWeights.Bold));
customToolTipStyle.Setters.Add(new Setter(ToolTip.PaddingProperty, new Thickness(10, 5, 10, 5)));

SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ToolTipStyle = customToolTipStyle,
    ThumbToolTipPlacement = ThumbToolTipPlacement.TopLeft,
    ToolTipFormat = "N2",
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

### Advanced Style with Icons

Create sophisticated tooltip designs with custom layouts and icons:

**XAML Example:**

```xaml
<Window.Resources>
    <Style x:Key="AdvancedToolTipStyle" TargetType="ToolTip">
        <Setter Property="Background" Value="#FF2196F3"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="BorderBrush" Value="#FF1976D2"/>
        <Setter Property="BorderThickness" Value="1"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ToolTip">
                    <Border Background="{TemplateBinding Background}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}"
                            CornerRadius="8"
                            Padding="12,8">
                        <StackPanel Orientation="Horizontal">
                            <Path Data="M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2Z" 
                                  Fill="White" 
                                  Width="12" 
                                  Height="12" 
                                  Stretch="Uniform"
                                  Margin="0,0,6,0"/>
                            <TextBlock Text="{TemplateBinding Content}"
                                     FontSize="13"
                                     FontWeight="SemiBold"
                                     VerticalAlignment="Center"/>
                        </StackPanel>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    ShowRange="True"
    RangeStart="30"
    RangeEnd="70"
    ToolTipStyle="{StaticResource AdvancedToolTipStyle}"
    ThumbToolTipPlacement="TopLeft"
    ToolTipFormat="N0" />
```

**C# Example:**

```csharp
Grid parentGrid = new Grid();

// Define advanced tooltip style with custom template
Style advancedToolTipStyle = new Style(typeof(ToolTip));

ControlTemplate template = new ControlTemplate(typeof(ToolTip));
FrameworkElementFactory border = new FrameworkElementFactory(typeof(Border));
border.SetValue(Border.BackgroundProperty, 
    new SolidColorBrush((Color)ColorConverter.ConvertFromString("#FF2196F3")));
border.SetValue(Border.BorderBrushProperty, 
    new SolidColorBrush((Color)ColorConverter.ConvertFromString("#FF1976D2")));
border.SetValue(Border.BorderThicknessProperty, new Thickness(1));
border.SetValue(Border.CornerRadiusProperty, new CornerRadius(8));
border.SetValue(Border.PaddingProperty, new Thickness(12, 8, 12, 8));

FrameworkElementFactory contentPresenter = new FrameworkElementFactory(typeof(ContentPresenter));
contentPresenter.SetBinding(ContentPresenter.ContentProperty, 
    new TemplateBinding(ToolTip.ContentProperty));

border.AppendChild(contentPresenter);
template.VisualTree = border;

advancedToolTipStyle.Setters.Add(new Setter(ToolTip.TemplateProperty, template));

SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ShowRange = true,
    RangeStart = 30,
    RangeEnd = 70,
    ToolTipStyle = advancedToolTipStyle,
    ThumbToolTipPlacement = ThumbToolTipPlacement.TopLeft,
    ToolTipFormat = "N0"
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

## Thumb Interval

The `ThumbInterval` property sets the minimum distance between two thumbs in a range slider. The thumbs cannot be moved within this range, ensuring a minimum gap between values.

**Property:** `ThumbInterval`  
**Type:** `double`  
**Default Value:** `0`

**XAML Example:**

```xaml
<editors:SfRangeSlider
    Width="300"
    Maximum="100"
    Minimum="0"
    RangeEnd="50"
    RangeStart="20"
    ShowRange="True"
    ThumbInterval="10"
    Value="50" />
```

**C# Example:**

```csharp
Grid parentGrid = new Grid();
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Maximum = 100,
    Minimum = 0,
    ShowRange = true,
    RangeStart = 20,
    RangeEnd = 60,
    ThumbInterval = 10,
    Value = 50
};

parentGrid.Children.Add(rangeSlider);
this.Content = parentGrid;
```

**Use Case:** Useful for scenarios like price ranges where you want to maintain a minimum spread, or time ranges where you need a minimum duration.

## Best Practices

### 1. Choose Appropriate Format

Match the tooltip format to your data type:
- Use `N` for general numeric values
- Use `C` for currency/monetary values
- Use `P` for percentages
- Use `F` for fixed-point decimals

### 2. Set Proper Precision

Consider the precision appropriate for your use case:
- Financial data: Usually 2 decimal places
- Scientific data: May need 4+ decimal places
- General UI: Often 0-2 decimal places

### 3. Position for Visibility

Choose tooltip placement based on UI layout:
- `TopLeft`: Better for bottom-aligned controls
- `BottomRight`: Better for top-aligned controls
- Consider vertical orientation implications

### 4. Display Mode Selection

Choose display mode based on user needs:
- `Always`: For monitoring/dashboard scenarios
- `OnFocus`: For interactive adjustment scenarios

### 5. Styling Consistency

Ensure tooltip styling matches your application theme:
- Use consistent color schemes
- Match font families and sizes
- Consider accessibility (contrast ratios)

## Troubleshooting

### Tooltip Not Appearing

**Problem:** Tooltip doesn't show when dragging the thumb.

**Solutions:**
1. Check `ToolTipDisplayMode` is not set to `Never`
2. Verify `ThumbToolTipPlacement` is not set to `None`
3. Ensure the slider has focus and is enabled
4. Check for overlapping UI elements blocking the tooltip

### Precision Not Working

**Problem:** `ThumbToolTipPrecision` doesn't affect the displayed value.

**Solutions:**
1. Ensure `ToolTipFormat` is set to `"N"` or numeric format
2. Verify precision value is appropriate (0-15)
3. Check that you're using decimal/double values, not integers

### Custom Style Not Applied

**Problem:** Custom tooltip style doesn't appear.

**Solutions:**
1. Verify the style is defined in Resources before the slider
2. Check the `x:Key` matches the `StaticResource` reference
3. Ensure `TargetType` is set to `ToolTip`
4. Verify there are no conflicting styles

### Format String Not Working

**Problem:** Custom format string displays incorrectly.

**Solutions:**
1. Verify the format string is valid .NET format
2. Check culture settings if using currency
3. Ensure the value type matches the format (e.g., decimal for currency)

## Complete Examples

### Example 1: Financial Slider with Currency Tooltip

```xaml
<Window.Resources>
    <Style x:Key="FinancialToolTipStyle" TargetType="ToolTip">
        <Setter Property="Background" Value="#FF1B5E20"/>
        <Setter Property="Foreground" Value="#FFFFFFFF"/>
        <Setter Property="BorderBrush" Value="#FF4CAF50"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="FontSize" Value="16"/>
        <Setter Property="FontWeight" Value="Bold"/>
        <Setter Property="Padding" Value="12,6"/>
    </Style>
</Window.Resources>

<editors:SfRangeSlider
    Width="400"
    Maximum="10000"
    Minimum="0"
    ToolTipFormat="C0"
    ToolTipDisplayMode="Always"
    ThumbToolTipPlacement="TopLeft"
    ToolTipStyle="{StaticResource FinancialToolTipStyle}"
    Value="5000"
    ShowRange="True"
    RangeStart="2000"
    RangeEnd="8000"
    ThumbInterval="500" />
```

### Example 2: Temperature Slider with Custom Format

```xaml
<Window.Resources>
    <Style x:Key="TemperatureToolTipStyle" TargetType="ToolTip">
        <Setter Property="Background" Value="#FFFF5722"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Padding" Value="10,5"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ToolTip">
                    <Border Background="{TemplateBinding Background}"
                            CornerRadius="4"
                            Padding="{TemplateBinding Padding}">
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="{TemplateBinding Content}"
                                     FontSize="{TemplateBinding FontSize}"
                                     FontWeight="SemiBold"
                                     Foreground="{TemplateBinding Foreground}"/>
                            <TextBlock Text="°C"
                                     FontSize="{TemplateBinding FontSize}"
                                     FontWeight="Normal"
                                     Margin="2,0,0,0"
                                     Foreground="{TemplateBinding Foreground}"/>
                        </StackPanel>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<editors:SfRangeSlider
    Width="350"
    Maximum="100"
    Minimum="-20"
    ToolTipFormat="N1"
    ThumbToolTipPrecision="1"
    ToolTipDisplayMode="OnFocus"
    ThumbToolTipPlacement="TopLeft"
    ToolTipStyle="{StaticResource TemperatureToolTipStyle}"
    Value="22.5" />
```

### Example 3: Percentage Slider with Always-Visible Tooltip

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        CreatePercentageSlider();
    }

    private void CreatePercentageSlider()
    {
        Grid parentGrid = new Grid();

        // Create custom tooltip style
        Style tooltipStyle = new Style(typeof(ToolTip));
        tooltipStyle.Setters.Add(new Setter(ToolTip.BackgroundProperty, 
            new SolidColorBrush((Color)ColorConverter.ConvertFromString("#FF673AB7"))));
        tooltipStyle.Setters.Add(new Setter(ToolTip.ForegroundProperty, Brushes.White));
        tooltipStyle.Setters.Add(new Setter(ToolTip.FontSizeProperty, 15.0));
        tooltipStyle.Setters.Add(new Setter(ToolTip.FontWeightProperty, FontWeights.SemiBold));
        tooltipStyle.Setters.Add(new Setter(ToolTip.PaddingProperty, new Thickness(10, 6, 10, 6)));

        // Create range slider
        SfRangeSlider rangeSlider = new SfRangeSlider()
        {
            Width = 400,
            Maximum = 1,
            Minimum = 0,
            Value = 0.75,
            ToolTipFormat = "P0",
            ToolTipDisplayMode = ToolTipDisplayMode.Always,
            ThumbToolTipPlacement = ThumbToolTipPlacement.TopLeft,
            ToolTipStyle = tooltipStyle
        };

        parentGrid.Children.Add(rangeSlider);
        this.Content = parentGrid;
    }
}
```

> **Note:** The `ToolTipStyle` property provides complete control over tooltip appearance. Combine it with `ToolTipFormat`, `ThumbToolTipPrecision`, and positioning options to create fully customized tooltips that enhance your application's user experience.
