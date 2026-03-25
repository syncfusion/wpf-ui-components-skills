# Range Adorner

## Overview

The Range Adorner feature provides visual feedback by displaying a progress bar-like indicator within the IntegerTextBox control. This adorner layer fills the control area proportionally based on the current value relative to the minimum and maximum values, offering users an intuitive visual representation of where the value stands within the allowed range.

## EnableRangeAdorner Property

Enable the range adorner visualization by setting the `EnableRangeAdorner` property.

**Property:** `EnableRangeAdorner`  
**Type:** `bool`  
**Default:** `false`

### Basic Range Adorner

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Value="63" 
                           MinValue="0" 
                           MaxValue="100" 
                           EnableRangeAdorner="True"
                           Width="200"
                           Height="30"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.MinValue = 0;
integerTextBox.MaxValue = 100;
integerTextBox.Value = 63;
integerTextBox.EnableRangeAdorner = true;
```

**Visual Result:** The control displays with a filled background showing 63% progress (since value is 63 out of 100).

### Important Requirements

**⚠️ Critical:** Range adorner requires BOTH `MinValue` and `MaxValue` to be set. The adorner will not display if either is missing.

```xml
<!-- ❌ Won't display adorner (no min/max set) -->
<syncfusion:IntegerTextBox Value="50" 
                           EnableRangeAdorner="True"/>

<!-- ✅ Will display adorner -->
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"/>
```

## How Range Adorner Works

The adorner calculates the fill percentage based on the formula:

```
Fill Percentage = (Value - MinValue) / (MaxValue - MinValue) × 100%
```

### Fill Calculation Examples

**Example 1:** Value in middle of range
```
Value = 50, MinValue = 0, MaxValue = 100
Fill = (50 - 0) / (100 - 0) = 0.5 = 50%
```

**Example 2:** Value at 3/4 of range
```
Value = 75, MinValue = 0, MaxValue = 100
Fill = (75 - 0) / (100 - 0) = 0.75 = 75%
```

**Example 3:** Value in negative range
```
Value = -25, MinValue = -100, MaxValue = 0
Fill = (-25 - (-100)) / (0 - (-100)) = 75 / 100 = 75%
```

**Example 4:** Value in custom range
```
Value = 150, MinValue = 100, MaxValue = 200
Fill = (150 - 100) / (200 - 100) = 50 / 100 = 50%
```

## RangeAdornerBackground Property

Customize the color of the range adorner using the `RangeAdornerBackground` property.

**Property:** `RangeAdornerBackground`  
**Type:** `Brush`  
**Default:** System default

### Custom Adorner Color

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           MinValue="0" 
                           MaxValue="100" 
                           Value="57" 
                           EnableRangeAdorner="True" 
                           RangeAdornerBackground="LightGreen"
                           Width="200"
                           Height="30"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.MinValue = 0;
integerTextBox.MaxValue = 100;
integerTextBox.Value = 57;
integerTextBox.EnableRangeAdorner = true;
integerTextBox.RangeAdornerBackground = Brushes.LightGreen;
```

### Color Examples

**Light Blue (default style):**
```xml
<syncfusion:IntegerTextBox Value="40"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightBlue"
                           Width="200"
                           Height="30"/>
```

**Success Green:**
```xml
<syncfusion:IntegerTextBox Value="80"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightGreen"
                           Width="200"
                           Height="30"/>
```

**Warning Yellow:**
```xml
<syncfusion:IntegerTextBox Value="60"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightYellow"
                           Width="200"
                           Height="30"/>
```

**Danger Red:**
```xml
<syncfusion:IntegerTextBox Value="95"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightPink"
                           Width="200"
                           Height="30"/>
```

**Custom Hex Color:**
```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="#E3F2FD"
                           Width="200"
                           Height="30"/>
```

## Gradient Adorner Background

Create more sophisticated visual effects using gradient brushes:

```xml
<syncfusion:IntegerTextBox Value="70"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           Width="250"
                           Height="35">
    <syncfusion:IntegerTextBox.RangeAdornerBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="LightGreen" Offset="0"/>
            <GradientStop Color="Yellow" Offset="0.5"/>
            <GradientStop Color="OrangeRed" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:IntegerTextBox.RangeAdornerBackground>
</syncfusion:IntegerTextBox>
```

## Practical Use Cases

### Progress Indicator

Show task or process completion:

```xml
<StackPanel Margin="20">
    <TextBlock Text="Task Progress" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="65"
                               MinValue="0"
                               MaxValue="100"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="CornflowerBlue"
                               TextAlignment="Center"
                               FontWeight="Bold"
                               Width="300"
                               Height="35"/>
    <TextBlock Text="65% Complete" 
               HorizontalAlignment="Center" 
               Margin="0,5,0,0"/>
</StackPanel>
```

### Temperature Gauge

Visual representation of temperature within range:

```xml
<StackPanel Margin="20">
    <TextBlock Text="Temperature (°C)" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="28"
                               MinValue="0"
                               MaxValue="50"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="Orange"
                               ShowSpinButton="True"
                               TextAlignment="Center"
                               FontSize="16"
                               Width="200"
                               Height="40"/>
</StackPanel>
```

### Volume Control

Audio volume with visual feedback:

```xml
<StackPanel Orientation="Horizontal" Margin="20">
    <TextBlock Text="🔊" 
               FontSize="20" 
               VerticalAlignment="Center" 
               Margin="0,0,10,0"/>
    <syncfusion:IntegerTextBox Value="75"
                               MinValue="0"
                               MaxValue="100"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightBlue"
                               ScrollInterval="5"
                               IsScrollingOnCircle="True"
                               TextAlignment="Center"
                               Width="150"
                               Height="30"/>
</StackPanel>
```

### Battery Level Indicator

```xml
<StackPanel Margin="20">
    <TextBlock Text="🔋 Battery Level" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="45"
                               MinValue="0"
                               MaxValue="100"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightGreen"
                               IsReadOnly="True"
                               TextAlignment="Center"
                               FontSize="18"
                               FontWeight="Bold"
                               Width="200"
                               Height="40"/>
    <TextBlock Text="45% remaining" 
               HorizontalAlignment="Center" 
               FontSize="10"
               Foreground="Gray"
               Margin="0,5,0,0"/>
</StackPanel>
```

### Disk Space Usage

```xml
<StackPanel Margin="20">
    <TextBlock Text="Storage Used (GB)" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="750"
                               MinValue="0"
                               MaxValue="1000"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightCoral"
                               GroupSeperatorEnabled="True"
                               TextAlignment="Right"
                               FontSize="14"
                               Width="200"
                               Height="35"/>
    <TextBlock Text="750 GB of 1,000 GB used" 
               FontSize="10"
               Foreground="Gray"
               Margin="0,5,0,0"/>
</StackPanel>
```

### Download Progress

```xml
<Border BorderBrush="Gray" 
        BorderThickness="1" 
        CornerRadius="5" 
        Padding="15"
        Width="350">
    <StackPanel>
        <TextBlock Text="Downloading..." 
                   FontWeight="Bold" 
                   Margin="0,0,0,10"/>
        <syncfusion:IntegerTextBox Value="342"
                                   MinValue="0"
                                   MaxValue="500"
                                   EnableRangeAdorner="True"
                                   RangeAdornerBackground="#4CAF50"
                                   IsReadOnly="True"
                                   TextAlignment="Center"
                                   FontSize="16"
                                   Height="40"/>
        <TextBlock Text="342 MB / 500 MB" 
                   HorizontalAlignment="Center"
                   FontSize="12"
                   Foreground="Gray"
                   Margin="0,10,0,0"/>
    </StackPanel>
</Border>
```

### Age Input with Visual Range

```xml
<StackPanel Margin="20">
    <TextBlock Text="Your Age" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="35"
                               MinValue="18"
                               MaxValue="100"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightSkyBlue"
                               ShowSpinButton="True"
                               TextAlignment="Center"
                               Width="180"
                               Height="35"/>
    <TextBlock Text="Must be 18 or older" 
               FontSize="10"
               Foreground="Gray"
               Margin="0,5,0,0"/>
</StackPanel>
```

### Slider Alternative

Combine range adorner with interactive features for slider-like behavior:

```xml
<StackPanel Margin="20" Width="300">
    <TextBlock Text="Brightness" 
               FontWeight="Bold" 
               Margin="0,0,0,10"/>
    <syncfusion:IntegerTextBox Value="60"
                               MinValue="0"
                               MaxValue="100"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="Gold"
                               ScrollInterval="5"
                               IsScrollingOnCircle="True"
                               EnableExtendedScrolling="True"
                               ShowSpinButton="True"
                               TextAlignment="Center"
                               FontSize="18"
                               FontWeight="Bold"
                               Height="45"/>
    <Grid Margin="0,5,0,0">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Column="0" Text="Dim" FontSize="10" Foreground="Gray"/>
        <TextBlock Grid.Column="1" Text="Bright" FontSize="10" Foreground="Gray" HorizontalAlignment="Right"/>
    </Grid>
</StackPanel>
```

### Settings Configuration

```xml
<GroupBox Header="Performance Settings" Padding="15" Width="320">
    <StackPanel>
        <TextBlock Text="Cache Size (MB):" Margin="0,0,0,5"/>
        <syncfusion:IntegerTextBox Value="512"
                                   MinValue="128"
                                   MaxValue="2048"
                                   EnableRangeAdorner="True"
                                   RangeAdornerBackground="LightCyan"
                                   ScrollInterval="128"
                                   IsScrollingOnCircle="True"
                                   GroupSeperatorEnabled="True"
                                   TextAlignment="Center"
                                   Width="280"
                                   Height="35"/>
        <TextBlock Text="Range: 128 MB - 2,048 MB" 
                   FontSize="10"
                   Foreground="Gray"
                   HorizontalAlignment="Center"
                   Margin="0,5,0,0"/>
    </StackPanel>
</GroupBox>
```

### Multi-Level Indicator

Show different visual states based on value ranges:

```xml
<StackPanel Margin="20">
    <TextBlock Text="CPU Usage (%)" Margin="0,0,0,5"/>
    
    <!-- Low usage (green) -->
    <syncfusion:IntegerTextBox x:Name="cpuLow"
                               Value="25"
                               MinValue="0"
                               MaxValue="100"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightGreen"
                               TextAlignment="Center"
                               Width="200"
                               Height="35"
                               Margin="0,0,0,10"/>
    
    <!-- Medium usage (yellow) -->
    <syncfusion:IntegerTextBox x:Name="cpuMedium"
                               Value="65"
                               MinValue="0"
                               MaxValue="100"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightYellow"
                               TextAlignment="Center"
                               Width="200"
                               Height="35"
                               Margin="0,0,0,10"/>
    
    <!-- High usage (red) -->
    <syncfusion:IntegerTextBox x:Name="cpuHigh"
                               Value="90"
                               MinValue="0"
                               MaxValue="100"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightCoral"
                               TextAlignment="Center"
                               Width="200"
                               Height="35"/>
</StackPanel>
```

## Combining Range Adorner with Other Features

### With Spin Buttons

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightBlue"
                           ShowSpinButton="True"
                           ScrollInterval="10"
                           Width="200"
                           Height="35"/>
```

### With Value-Based Colors

```xml
<syncfusion:IntegerTextBox Value="-20"
                           MinValue="-100"
                           MaxValue="100"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="LightSteelBlue"
                           PositiveForeground="Green"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"
                           ZeroColor="Gray"
                           ApplyZeroColor="True"
                           TextAlignment="Center"
                           FontSize="16"
                           FontWeight="Bold"
                           Width="200"
                           Height="35"/>
```

### With Number Formatting

```xml
<syncfusion:IntegerTextBox Value="1250000"
                           MinValue="0"
                           MaxValue="10000000"
                           EnableRangeAdorner="True"
                           RangeAdornerBackground="PaleGreen"
                           GroupSeperatorEnabled="True"
                           Culture="en-US"
                           TextAlignment="Right"
                           FontSize="14"
                           Width="250"
                           Height="35"/>
```

## Best Practices

### Always Set Min and Max

```xml
<!-- ✅ Correct - Range adorner will display -->
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           EnableRangeAdorner="True"/>

<!-- ❌ Incorrect - Range adorner won't display -->
<syncfusion:IntegerTextBox Value="50"
                           EnableRangeAdorner="True"/>
```

### Choose Appropriate Colors

Match adorner colors to your use case:
- **Green** - Success, healthy, positive
- **Blue** - Neutral, information, progress
- **Yellow** - Warning, caution, moderate
- **Red** - Danger, critical, maximum

### Provide Context

Include labels or tooltips to explain what the visual indicator represents:

```xml
<StackPanel>
    <TextBlock Text="Memory Usage" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="6"
                               MinValue="0"
                               MaxValue="16"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightBlue"
                               ToolTip="Current memory usage out of 16 GB total"
                               Width="200"
                               Height="35"/>
    <TextBlock Text="6 GB / 16 GB" 
               FontSize="10"
               Foreground="Gray"
               Margin="0,5,0,0"/>
</StackPanel>
```

The Range Adorner feature transforms IntegerTextBox into a powerful visual feedback control, perfect for scenarios where users benefit from seeing value position within a range at a glance.
