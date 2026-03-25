# Appearance and Customization

## Table of Contents
- [Overview](#overview)
- [Foreground Colors](#foreground-colors)
- [Background Customization](#background-customization)
- [Corner Radius](#corner-radius)
- [Selection Appearance](#selection-appearance)
- [Text Alignment](#text-alignment)
- [ToolTip Configuration](#tooltip-configuration)
- [Complete Styling Examples](#complete-styling-examples)

## Overview

IntegerTextBox provides extensive appearance customization options including value-based foreground colors, background styling, border rounding, selection highlighting, text alignment, and tooltip configuration. These features allow you to create visually appealing and user-friendly integer input controls.

## Foreground Colors

Customize the text color based on the value state (positive, negative, or zero) to provide visual feedback to users.

### Positive Foreground

Set the foreground color for positive values using the `PositiveForeground` property.

**Property:** `PositiveForeground`  
**Type:** `Brush`  
**Default:** `Black`

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Value="10" 
                           Width="100" 
                           Height="25" 
                           PositiveForeground="Blue"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Width = 100;
integerTextBox.Height = 25;
integerTextBox.Value = 10;
integerTextBox.PositiveForeground = Brushes.Blue;
```

**Result:** When value is positive (greater than 0), text displays in blue.

### Negative Foreground

Set the foreground color for negative values using the `NegativeForeground` property. Must enable `ApplyNegativeForeground` to see the effect.

**Properties:**
- `NegativeForeground` - Color for negative values (default: `Red`)
- `ApplyNegativeForeground` - Enable negative coloring (default: `false`)

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Value="-10" 
                           Width="100" 
                           Height="25"
                           NegativeForeground="SpringGreen" 
                           ApplyNegativeForeground="True"/>
```

```csharp
integerTextBox.Value = -10;
integerTextBox.ApplyNegativeForeground = true;   
integerTextBox.NegativeForeground = Brushes.SpringGreen;
```

**Result:** When value is negative (less than 0), text displays in spring green.

**⚠️ Important:** Set `ApplyNegativeForeground="True"` to enable negative foreground coloring.

### Zero Color

Set the foreground color for zero value using the `ZeroColor` property. Must enable `ApplyZeroColor` to see the effect.

**Properties:**
- `ZeroColor` - Color for zero value (default: `Green`)
- `ApplyZeroColor` - Enable zero coloring (default: `false`)

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Value="0" 
                           Width="100" 
                           Height="25"
                           ApplyZeroColor="True" 
                           ZeroColor="DarkGoldenrod"/>
```

```csharp
integerTextBox.Value = 0;
integerTextBox.ApplyZeroColor = true;
integerTextBox.ZeroColor = Brushes.DarkGoldenrod;
```

**Result:** When value is exactly 0, text displays in dark goldenrod.

**⚠️ Important:** Set `ApplyZeroColor="True"` to enable zero color.

### Combined Foreground Colors

Use all three foreground properties together for complete value-based coloring:

```xml
<syncfusion:IntegerTextBox Value="-25"
                           MinValue="-100"
                           MaxValue="100"
                           PositiveForeground="Green"
                           NegativeForeground="Red"
                           ApplyNegativeForeground="True"
                           ZeroColor="Gray"
                           ApplyZeroColor="True"
                           ShowSpinButton="True"
                           Width="150"
                           Height="30"/>
```

**Behavior:**
- Value > 0: Green text
- Value < 0: Red text
- Value = 0: Gray text

### Financial Dashboard Example

```xml
<StackPanel Margin="20">
    <TextBlock Text="Account Balance:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox x:Name="balanceTextBox"
                               Value="5000"
                               MinValue="-10000"
                               MaxValue="1000000"
                               PositiveForeground="DarkGreen"
                               NegativeForeground="DarkRed"
                               ApplyNegativeForeground="True"
                               ZeroColor="Orange"
                               ApplyZeroColor="True"
                               GroupSeperatorEnabled="True"
                               FontSize="16"
                               FontWeight="Bold"
                               Width="200"
                               Height="35"/>
</StackPanel>
```

## Background Customization

Customize the control's background color using the `Background` property.

**Property:** `Background`  
**Type:** `Brush`  
**Default:** `White`

### Solid Color Background

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100"
                           Height="25" 
                           Value="80" 
                           Background="Cyan"/>
```

```csharp
integerTextBox.Background = Brushes.Cyan;
```

### Gradient Background

```xml
<syncfusion:IntegerTextBox Value="100" 
                           Width="200" 
                           Height="35">
    <syncfusion:IntegerTextBox.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="LightBlue" Offset="0"/>
            <GradientStop Color="White" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:IntegerTextBox.Background>
</syncfusion:IntegerTextBox>
```

### Subtle Background Colors

```xml
<!-- Light gray for read-only fields -->
<syncfusion:IntegerTextBox Value="100"
                           IsReadOnly="True"
                           Background="LightGray"
                           Width="150"
                           Height="30"/>

<!-- Light yellow for required fields -->
<syncfusion:IntegerTextBox Value="50"
                           Background="LightYellow"
                           Width="150"
                           Height="30"/>

<!-- Light green for valid fields -->
<syncfusion:IntegerTextBox Value="75"
                           Background="LightGreen"
                           Width="150"
                           Height="30"/>
```

## Corner Radius

Round the corners of the control border using the `CornerRadius` property.

**Property:** `CornerRadius`  
**Type:** `CornerRadius`  
**Default:** `1`

### Uniform Corner Radius

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100" 
                           Height="25" 
                           CornerRadius="5"/>
```

```csharp
integerTextBox.CornerRadius = new CornerRadius(5);
```

### Different Corner Values

```xml
<!-- Top corners rounded, bottom square -->
<syncfusion:IntegerTextBox CornerRadius="10,10,0,0" 
                           Width="150" 
                           Height="30"/>

<!-- Left corners rounded, right square -->
<syncfusion:IntegerTextBox CornerRadius="10,0,0,10" 
                           Width="150" 
                           Height="30"/>
```

### Modern Rounded Style

```xml
<syncfusion:IntegerTextBox Value="100"
                           CornerRadius="15"
                           Background="WhiteSmoke"
                           BorderThickness="2"
                           BorderBrush="DodgerBlue"
                           Padding="10,5"
                           Width="200"
                           Height="40"/>
```

### Pill-Shaped Control

```xml
<syncfusion:IntegerTextBox Value="50"
                           CornerRadius="20"
                           Background="AliceBlue"
                           Padding="15,5"
                           Width="150"
                           Height="40"/>
```

## Selection Appearance

Customize how selected text appears using selection brush and opacity properties.

**Properties:**
- `SelectionBrush` - Background color of selected text
- `SelectionOpacity` - Transparency of selection brush

### Custom Selection Brush

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100" 
                           Height="25" 
                           Value="12345"
                           SelectionBrush="Red" 
                           SelectionOpacity="0.5"/>
```

```csharp
integerTextBox.SelectionBrush = Brushes.Red;
integerTextBox.SelectionOpacity = 0.5;
```

### Selection Opacity Examples

```xml
<!-- Fully opaque selection -->
<syncfusion:IntegerTextBox Value="12345"
                           SelectionBrush="CornflowerBlue"
                           SelectionOpacity="1.0"
                           Width="150"
                           Height="30"/>

<!-- Semi-transparent selection -->
<syncfusion:IntegerTextBox Value="12345"
                           SelectionBrush="Orange"
                           SelectionOpacity="0.3"
                           Width="150"
                           Height="30"/>

<!-- Very light selection -->
<syncfusion:IntegerTextBox Value="12345"
                           SelectionBrush="Green"
                           SelectionOpacity="0.2"
                           Width="150"
                           Height="30"/>
```

### Branded Selection Style

```xml
<syncfusion:IntegerTextBox Value="999"
                           SelectionBrush="#FF4081"
                           SelectionOpacity="0.4"
                           Foreground="#212121"
                           Background="#FAFAFA"
                           FontSize="14"
                           Width="150"
                           Height="35"/>
```

## Text Alignment

Align the value text horizontally within the control using the `TextAlignment` property.

**Property:** `TextAlignment`  
**Type:** `TextAlignment`  
**Default:** `Left`  
**Values:** `Left`, `Center`, `Right`

### Left Alignment (Default)

```xml
<syncfusion:IntegerTextBox Value="12345" 
                           TextAlignment="Left"
                           Width="150" 
                           Height="30"/>
```

```csharp
integerTextBox.TextAlignment = TextAlignment.Left;
```

### Center Alignment

```xml
<syncfusion:IntegerTextBox Value="12345" 
                           TextAlignment="Center"
                           Width="150" 
                           Height="30"/>
```

```csharp
integerTextBox.TextAlignment = TextAlignment.Center;
```

### Right Alignment

Particularly useful for numeric values, especially in tables or forms:

```xml
<syncfusion:IntegerTextBox Value="12345" 
                           TextAlignment="Right"
                           Width="150" 
                           Height="30"/>
```

```csharp
integerTextBox.TextAlignment = TextAlignment.Right;
```

### Alignment in Forms

```xml
<Grid Margin="20">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    
    <TextBlock Grid.Row="0" Grid.Column="0" Text="Price:" Margin="0,0,10,10"/>
    <syncfusion:IntegerTextBox Grid.Row="0" Grid.Column="1" 
                               Value="1599"
                               TextAlignment="Right"
                               Width="150" 
                               Height="30"
                               Margin="0,0,0,10"/>
    
    <TextBlock Grid.Row="1" Grid.Column="0" Text="Quantity:" Margin="0,0,10,10"/>
    <syncfusion:IntegerTextBox Grid.Row="1" Grid.Column="1" 
                               Value="5"
                               TextAlignment="Right"
                               Width="150" 
                               Height="30"
                               Margin="0,0,0,10"/>
    
    <TextBlock Grid.Row="2" Grid.Column="0" Text="Total:" Margin="0,0,10,0"/>
    <syncfusion:IntegerTextBox Grid.Row="2" Grid.Column="1" 
                               Value="7995"
                               TextAlignment="Right"
                               IsReadOnly="True"
                               Background="LightGray"
                               FontWeight="Bold"
                               Width="150" 
                               Height="30"/>
</Grid>
```

## ToolTip Configuration

Provide helpful information to users via tooltips using the `ToolTip` property.

**Property:** `ToolTip`  
**Type:** `object`

### Simple Text ToolTip

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100" 
                           Height="25" 
                           ToolTip="Enter Integer Value"/>
```

```csharp
integerTextBox.ToolTip = "Enter Integer Value";
```

### Descriptive ToolTip

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           ToolTip="Enter a value between 0 and 100"
                           Width="150"
                           Height="30"/>
```

### Multi-Line ToolTip

```xml
<syncfusion:IntegerTextBox MinValue="18"
                           MaxValue="120"
                           Width="150"
                           Height="30">
    <syncfusion:IntegerTextBox.ToolTip>
        <TextBlock>
            <Run Text="Age Requirement" FontWeight="Bold"/>
            <LineBreak/>
            <Run Text="Enter your age (18-120)"/>
            <LineBreak/>
            <Run Text="Must be 18 or older" FontStyle="Italic"/>
        </TextBlock>
    </syncfusion:IntegerTextBox.ToolTip>
</syncfusion:IntegerTextBox>
```

### Custom Styled ToolTip

```xml
<syncfusion:IntegerTextBox Value="1000"
                           Width="150"
                           Height="30">
    <syncfusion:IntegerTextBox.ToolTip>
        <Border Background="DarkBlue" 
                BorderBrush="White" 
                BorderThickness="2"
                CornerRadius="5" 
                Padding="10">
            <StackPanel>
                <TextBlock Text="💰 Amount" 
                           Foreground="White" 
                           FontWeight="Bold"
                           Margin="0,0,0,5"/>
                <TextBlock Text="Enter the transaction amount" 
                           Foreground="LightGray"
                           TextWrapping="Wrap"
                           MaxWidth="200"/>
            </StackPanel>
        </Border>
    </syncfusion:IntegerTextBox.ToolTip>
</syncfusion:IntegerTextBox>
```

### ToolTip with Validation Info

```xml
<syncfusion:IntegerTextBox MinValue="1"
                           MaxValue="999"
                           Width="150"
                           Height="30">
    <syncfusion:IntegerTextBox.ToolTip>
        <StackPanel>
            <TextBlock Text="Quantity Input" FontWeight="Bold"/>
            <TextBlock Text="• Minimum: 1" Margin="0,5,0,0"/>
            <TextBlock Text="• Maximum: 999"/>
            <TextBlock Text="• Required field" Foreground="Red" Margin="0,5,0,0"/>
        </StackPanel>
    </syncfusion:IntegerTextBox.ToolTip>
</syncfusion:IntegerTextBox>
```

## Complete Styling Examples

### Modern Card Style

```xml
<Border Background="White"
        BorderBrush="LightGray"
        BorderThickness="1"
        CornerRadius="8"
        Padding="15"
        Width="250">
    <StackPanel>
        <TextBlock Text="Enter Amount" 
                   FontSize="12" 
                   Foreground="Gray"
                   Margin="0,0,0,5"/>
        <syncfusion:IntegerTextBox Value="1000"
                                   MinValue="0"
                                   MaxValue="999999"
                                   GroupSeperatorEnabled="True"
                                   CornerRadius="4"
                                   BorderThickness="2"
                                   BorderBrush="#E0E0E0"
                                   Padding="10,8"
                                   FontSize="16"
                                   TextAlignment="Right"
                                   SelectionBrush="CornflowerBlue"
                                   SelectionOpacity="0.3"
                                   Height="40"/>
    </StackPanel>
</Border>
```

### Dark Theme Style

```xml
<syncfusion:IntegerTextBox Value="500"
                           Background="#2C2C2C"
                           Foreground="White"
                           PositiveForeground="#4CAF50"
                           BorderBrush="#555555"
                           BorderThickness="1"
                           CornerRadius="3"
                           SelectionBrush="#2196F3"
                           SelectionOpacity="0.5"
                           Padding="10,5"
                           FontSize="14"
                           Width="200"
                           Height="35"/>
```

### Material Design Inspired

```xml
<StackPanel Margin="20">
    <TextBlock Text="QUANTITY" 
               FontSize="10" 
               Foreground="#757575"
               Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="42"
                               MinValue="1"
                               MaxValue="999"
                               Background="Transparent"
                               BorderThickness="0,0,0,2"
                               BorderBrush="#2196F3"
                               CornerRadius="0"
                               Padding="2,8"
                               FontSize="16"
                               Foreground="#212121"
                               SelectionBrush="#2196F3"
                               SelectionOpacity="0.2"
                               Width="200"
                               Height="35"/>
</StackPanel>
```

### Financial Application Style

```xml
<Grid Margin="20">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <TextBlock Grid.Row="0" 
               Text="Portfolio Value" 
               FontSize="14" 
               FontWeight="Bold"
               Margin="0,0,0,10"/>
    
    <syncfusion:IntegerTextBox Grid.Row="1"
                               Value="1250000"
                               MinValue="0"
                               GroupSeperatorEnabled="True"
                               Culture="en-US"
                               TextAlignment="Right"
                               FontSize="24"
                               FontWeight="Bold"
                               PositiveForeground="DarkGreen"
                               Background="Honeydew"
                               BorderBrush="ForestGreen"
                               BorderThickness="2"
                               CornerRadius="5"
                               Padding="15,10"
                               IsReadOnly="True"
                               Width="300"
                               Height="50"/>
</Grid>
```

### Form Field with Icon

```xml
<Grid Width="250" Height="40">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    
    <Border Grid.Column="0" 
            Background="#2196F3" 
            CornerRadius="5,0,0,5"
            Width="40">
        <TextBlock Text="📦" 
                   FontSize="20" 
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Border>
    
    <syncfusion:IntegerTextBox Grid.Column="1"
                               Value="25"
                               MinValue="1"
                               MaxValue="999"
                               BorderThickness="1,1,1,1"
                               BorderBrush="#2196F3"
                               CornerRadius="0,5,5,0"
                               Padding="10,5"
                               FontSize="14"
                               ToolTip="Enter quantity"/>
</Grid>
```

### Validation State Styling

```xml
<StackPanel Margin="20" Width="200">
    <!-- Valid state -->
    <TextBlock Text="Valid Input:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="50"
                               BorderBrush="Green"
                               BorderThickness="2"
                               Background="LightGreen"
                               CornerRadius="3"
                               Height="30"
                               Margin="0,0,0,15"/>
    
    <!-- Warning state -->
    <TextBlock Text="Warning State:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="95"
                               BorderBrush="Orange"
                               BorderThickness="2"
                               Background="LightYellow"
                               CornerRadius="3"
                               Height="30"
                               Margin="0,0,0,15"/>
    
    <!-- Error state -->
    <TextBlock Text="Error State:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="150"
                               BorderBrush="Red"
                               BorderThickness="2"
                               Background="LightPink"
                               CornerRadius="3"
                               Height="30"/>
</StackPanel>
```

### Dashboard Widget Style

```xml
<Border Background="White"
        BorderBrush="#E0E0E0"
        BorderThickness="1"
        CornerRadius="10"
        Padding="20"
        Width="300">
    <StackPanel>
        <TextBlock Text="📊 Current Value" 
                   FontSize="16" 
                   FontWeight="Bold"
                   Margin="0,0,0,10"/>
        
        <syncfusion:IntegerTextBox Value="87563"
                                   GroupSeperatorEnabled="True"
                                   TextAlignment="Center"
                                   FontSize="32"
                                   FontWeight="Bold"
                                   PositiveForeground="#4CAF50"
                                   Background="Transparent"
                                   BorderThickness="0"
                                   IsReadOnly="True"
                                   Height="50"
                                   Margin="0,0,0,10"/>
        
        <syncfusion:IntegerTextBox Value="87563"
                                   MinValue="0"
                                   MaxValue="100000"
                                   EnableRangeAdorner="True"
                                   RangeAdornerBackground="LightBlue"
                                   CornerRadius="10"
                                   Height="30"/>
    </StackPanel>
</Border>
```

## Combining Appearance Properties

Create sophisticated styling by combining multiple appearance properties:

```xml
<syncfusion:IntegerTextBox Value="-250"
                           MinValue="-1000"
                           MaxValue="1000"
                           
                           <!-- Foreground colors -->
                           PositiveForeground="DarkGreen"
                           NegativeForeground="DarkRed"
                           ApplyNegativeForeground="True"
                           ZeroColor="Gray"
                           ApplyZeroColor="True"
                           
                           <!-- Background and border -->
                           Background="WhiteSmoke"
                           BorderBrush="CornflowerBlue"
                           BorderThickness="2"
                           CornerRadius="8"
                           
                           <!-- Selection -->
                           SelectionBrush="LightBlue"
                           SelectionOpacity="0.4"
                           
                           <!-- Layout -->
                           TextAlignment="Right"
                           Padding="12,8"
                           FontSize="16"
                           FontWeight="SemiBold"
                           
                           <!-- Interaction -->
                           ShowSpinButton="True"
                           ToolTip="Enter transaction amount"
                           
                           <!-- Formatting -->
                           GroupSeperatorEnabled="True"
                           
                           Width="250"
                           Height="40"/>
```

These appearance customization options allow you to create IntegerTextBox controls that perfectly match your application's design language and provide excellent user experience with visual feedback.
