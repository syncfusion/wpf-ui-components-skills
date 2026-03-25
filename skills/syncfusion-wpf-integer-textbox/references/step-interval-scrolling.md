# Step Interval and Scrolling

## Table of Contents
- [Overview](#overview)
- [ScrollInterval Property](#scrollinterval-property)
- [Up and Down Arrow Keys](#up-and-down-arrow-keys)
- [Mouse Wheel Scrolling](#mouse-wheel-scrolling)
- [Click and Drag Scrolling](#click-and-drag-scrolling)
- [Text Selection on Focus](#text-selection-on-focus)
- [Complete Examples](#complete-examples)

## Overview

IntegerTextBox provides multiple interactive methods for adjusting values beyond manual typing. Users can increment or decrement values using arrow keys, mouse wheel, or click-and-drag gestures. The `ScrollInterval` property controls the step size for all these interactions, providing consistent value adjustment behavior.

## ScrollInterval Property

The `ScrollInterval` property defines the increment or decrement step when using arrow keys, mouse wheel, or spin buttons.

**Property:** `ScrollInterval`  
**Type:** `int`  
**Default:** `1`

### Basic ScrollInterval Configuration

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="150"
                           Height="25" 
                           Value="10" 
                           ScrollInterval="2"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Width = 150;
integerTextBox.Height = 25;
integerTextBox.Value = 10;
integerTextBox.ScrollInterval = 2;
```

**Behavior:**
- Each up arrow press increases value by 2
- Each down arrow press decreases value by 2
- Mouse wheel scroll changes value by 2
- Spin button clicks change value by 2

### ScrollInterval Examples

**Unit increments (default):**
```xml
<syncfusion:IntegerTextBox Value="50" ScrollInterval="1" Width="150" Height="30"/>
```
Changes: 50 → 51 → 52 → 53...

**Steps of 5:**
```xml
<syncfusion:IntegerTextBox Value="0" ScrollInterval="5" Width="150" Height="30"/>
```
Changes: 0 → 5 → 10 → 15 → 20...

**Steps of 10:**
```xml
<syncfusion:IntegerTextBox Value="100" ScrollInterval="10" Width="150" Height="30"/>
```
Changes: 100 → 110 → 120 → 130...

**Steps of 100:**
```xml
<syncfusion:IntegerTextBox Value="1000" ScrollInterval="100" Width="150" Height="30"/>
```
Changes: 1000 → 1100 → 1200 → 1300...

### ScrollInterval with Constraints

Combine with min/max values for bounded stepping:

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           ScrollInterval="10"
                           Width="150"
                           Height="30"/>
```

**Behavior:**
- At value 0: Can only increase (0 → 10 → 20...)
- At value 100: Can only decrease (100 → 90 → 80...)
- Stops at boundaries automatically

## Up and Down Arrow Keys

The IntegerTextBox allows users to adjust the value using keyboard arrow keys.

**Keys:**
- **Up Arrow (↑)** - Increases value by `ScrollInterval`
- **Down Arrow (↓)** - Decreases value by `ScrollInterval`

### Basic Arrow Key Usage

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="150"
                           Height="25" 
                           Value="10" 
                           ScrollInterval="2"/>
```

**User Experience:**
1. Click into the IntegerTextBox to give it focus
2. Press Up Arrow: Value changes from 10 → 12
3. Press Up Arrow again: Value changes from 12 → 14
4. Press Down Arrow: Value changes from 14 → 12

### Arrow Keys with Range

```xml
<syncfusion:IntegerTextBox Value="8"
                           MinValue="0"
                           MaxValue="20"
                           ScrollInterval="4"
                           Width="150"
                           Height="30"/>
```

**Interaction:**
- Starting at 8
- Press Up Arrow: 8 → 12 → 16 → 20 (stops at max)
- Press Down Arrow: 20 → 16 → 12 → 8 → 4 → 0 (stops at min)

### Rapid Value Adjustment

Users can hold down arrow keys for continuous value changes:

```xml
<syncfusion:IntegerTextBox Value="0"
                           MinValue="0"
                           MaxValue="1000"
                           ScrollInterval="10"
                           Width="150"
                           Height="30"/>
```

**Behavior:**
- Hold Up Arrow: 0 → 10 → 20 → 30... (rapid increments)
- Hold Down Arrow: Value rapidly decrements

## Mouse Wheel Scrolling

Enable value adjustment using the mouse wheel when hovering over the control.

**Property:** `IsScrollingOnCircle`  
**Type:** `bool`  
**Default:** `true`

### Basic Mouse Wheel Configuration

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="150" 
                           Height="25" 
                           Value="34" 
                           IsScrollingOnCircle="True" 
                           ScrollInterval="3"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Width = 150;
integerTextBox.Height = 25;
integerTextBox.Value = 34;
integerTextBox.IsScrollingOnCircle = true;
integerTextBox.ScrollInterval = 3;
```

**Behavior:**
- Scroll wheel up: Value increases by 3
- Scroll wheel down: Value decreases by 3
- No focus required, just hover over control

### Enabling Mouse Wheel Scrolling

```xml
<!-- Enabled (default) -->
<syncfusion:IntegerTextBox Value="50"
                           IsScrollingOnCircle="True"
                           ScrollInterval="5"
                           Width="150"
                           Height="30"/>
```

**User Experience:**
1. Hover mouse over the IntegerTextBox
2. Scroll wheel up: 50 → 55 → 60 → 65...
3. Scroll wheel down: 65 → 60 → 55 → 50...

### Disabling Mouse Wheel Scrolling

```xml
<!-- Disabled -->
<syncfusion:IntegerTextBox Value="50"
                           IsScrollingOnCircle="False"
                           ScrollInterval="5"
                           Width="150"
                           Height="30"/>
```

**Behavior:** Mouse wheel has no effect on the value.

### Mouse Wheel with Boundaries

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           IsScrollingOnCircle="True"
                           ScrollInterval="10"
                           Width="150"
                           Height="30"/>
```

**Behavior:**
- Scrolling up stops at 100
- Scrolling down stops at 0
- Respects min/max boundaries

### Fine vs Coarse Adjustment

**Fine adjustment:**
```xml
<syncfusion:IntegerTextBox Value="100"
                           ScrollInterval="1"
                           IsScrollingOnCircle="True"
                           Width="150"
                           Height="30"/>
```
Single-unit changes for precision.

**Coarse adjustment:**
```xml
<syncfusion:IntegerTextBox Value="0"
                           ScrollInterval="50"
                           IsScrollingOnCircle="True"
                           Width="150"
                           Height="30"/>
```
Large jumps for quick range traversal.

## Click and Drag Scrolling

Enable value adjustment by clicking and dragging the mouse horizontally or vertically.

**Property:** `EnableExtendedScrolling`  
**Type:** `bool`  
**Default:** `false`

### Basic Click and Drag Configuration

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="120" 
                           Height="25" 
                           Value="10" 
                           ScrollInterval="5" 
                           EnableExtendedScrolling="True"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Width = 120;
integerTextBox.Height = 25;
integerTextBox.Value = 10;
integerTextBox.ScrollInterval = 5;
integerTextBox.EnableExtendedScrolling = true;
```

### Click and Drag Behavior

**Important:** Control must be in an **unfocused state** for click-and-drag to work.

**Drag directions:**
- **Drag Right** - Increases value by `ScrollInterval`
- **Drag Up** - Increases value by `ScrollInterval`
- **Drag Left** - Decreases value by `ScrollInterval`
- **Drag Down** - Decreases value by `ScrollInterval`

### User Experience

1. Ensure IntegerTextBox is not focused (click elsewhere first)
2. Click on the IntegerTextBox value
3. Drag mouse right or up: Value increases
4. Drag mouse left or down: Value decreases
5. Distance dragged determines number of steps

### Click and Drag with Large Steps

```xml
<syncfusion:IntegerTextBox Value="100"
                           MinValue="0"
                           MaxValue="1000"
                           ScrollInterval="25"
                           EnableExtendedScrolling="True"
                           Width="200"
                           Height="30"/>
```

**Behavior:**
- Drag right: 100 → 125 → 150 → 175...
- Drag left: 175 → 150 → 125 → 100...

### Combining Click-Drag with Other Methods

```xml
<syncfusion:IntegerTextBox Value="50"
                           MinValue="0"
                           MaxValue="100"
                           ScrollInterval="5"
                           ShowSpinButton="True"
                           IsScrollingOnCircle="True"
                           EnableExtendedScrolling="True"
                           Width="150"
                           Height="30"/>
```

Users can choose their preferred adjustment method:
- Spin buttons for precise clicks
- Arrow keys for keyboard navigation
- Mouse wheel for quick scrolling
- Click-and-drag for visual adjustment

## Text Selection on Focus

Control whether text is automatically selected when the control receives focus.

**Property:** `TextSelectionOnFocus`  
**Type:** `bool`  
**Default:** `true`

### Auto-Selection Enabled (Default)

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Value="12345"
                           TextSelectionOnFocus="True"
                           Width="150"
                           Height="30"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Value = 12345;
integerTextBox.TextSelectionOnFocus = true;
```

**Behavior:**
- Tab into control: All text automatically selected
- Click into control: All text automatically selected
- Start typing: Replaces entire value
- Press arrow key: Deselects and positions cursor

### Auto-Selection Disabled

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Value="12345"
                           TextSelectionOnFocus="False"
                           Width="150"
                           Height="30"/>
```

```csharp
integerTextBox.TextSelectionOnFocus = false;
```

**Behavior:**
- Tab into control: Cursor at end, no selection
- Click into control: Cursor at click position, no selection
- Start typing: Inserts at cursor position
- Better for editing existing values

### Use Cases

**Enable auto-selection when:**
- Users typically replace entire value
- Control used in forms with tab navigation
- Quick data entry is priority

```xml
<!-- Good for forms -->
<syncfusion:IntegerTextBox Value="0"
                           TextSelectionOnFocus="True"
                           MinValue="1"
                           MaxValue="999"
                           Width="150"
                           Height="30"/>
```

**Disable auto-selection when:**
- Users need to edit portions of value
- Control has large values with formatting
- Editing mode is more common than replacement

```xml
<!-- Good for editing -->
<syncfusion:IntegerTextBox Value="1234567890"
                           TextSelectionOnFocus="False"
                           GroupSeperatorEnabled="True"
                           Width="200"
                           Height="30"/>
```

## Complete Examples

### Percentage Slider Alternative

```xml
<StackPanel Margin="20">
    <TextBlock Text="Progress (%)" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="50"
                               MinValue="0"
                               MaxValue="100"
                               ScrollInterval="5"
                               ShowSpinButton="True"
                               IsScrollingOnCircle="True"
                               EnableExtendedScrolling="True"
                               EnableRangeAdorner="True"
                               RangeAdornerBackground="LightGreen"
                               TextAlignment="Center"
                               FontSize="16"
                               Width="200"
                               Height="40"/>
    <TextBlock Text="Use arrow keys, mouse wheel, or drag to adjust" 
               FontSize="10" 
               Foreground="Gray"
               Margin="0,5,0,0"/>
</StackPanel>
```

### Volume Control

```xml
<StackPanel Orientation="Horizontal" Margin="20">
    <TextBlock Text="🔊 Volume:" 
               VerticalAlignment="Center" 
               Margin="0,0,10,0"/>
    <syncfusion:IntegerTextBox Value="75"
                               MinValue="0"
                               MaxValue="100"
                               ScrollInterval="5"
                               IsScrollingOnCircle="True"
                               EnableExtendedScrolling="True"
                               TextAlignment="Center"
                               Width="80"
                               Height="30"/>
</StackPanel>
```

### Temperature Adjuster

```xml
<Grid Margin="20">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <TextBlock Grid.Row="0" 
               Text="🌡️ Temperature (°C)" 
               FontSize="14"
               Margin="0,0,0,10"/>
    
    <syncfusion:IntegerTextBox Grid.Row="1"
                               Value="22"
                               MinValue="16"
                               MaxValue="30"
                               ScrollInterval="1"
                               ShowSpinButton="True"
                               IsScrollingOnCircle="True"
                               TextAlignment="Center"
                               FontSize="24"
                               FontWeight="Bold"
                               Width="120"
                               Height="50"/>
</Grid>
```

### Quantity Selector for E-commerce

```xml
<Border BorderBrush="LightGray" 
        BorderThickness="1" 
        CornerRadius="5" 
        Padding="15"
        Width="250">
    <StackPanel>
        <TextBlock Text="Select Quantity" 
                   FontWeight="Bold" 
                   Margin="0,0,0,10"/>
        
        <syncfusion:IntegerTextBox Value="1"
                                   MinValue="1"
                                   MaxValue="99"
                                   ScrollInterval="1"
                                   ShowSpinButton="True"
                                   IsScrollingOnCircle="True"
                                   TextSelectionOnFocus="True"
                                   Width="100"
                                   Height="35"
                                   HorizontalAlignment="Left"/>
        
        <TextBlock Text="• Use ↑↓ arrows or spin buttons&#x0a;• Scroll mouse wheel&#x0a;• Type directly" 
                   FontSize="10" 
                   Foreground="Gray"
                   Margin="0,10,0,0"/>
    </StackPanel>
</Border>
```

### Time Duration Picker (Minutes)

```xml
<StackPanel Orientation="Horizontal" Margin="20">
    <syncfusion:IntegerTextBox Value="30"
                               MinValue="0"
                               MaxValue="1440"
                               ScrollInterval="15"
                               ShowSpinButton="True"
                               IsScrollingOnCircle="True"
                               TextAlignment="Right"
                               Width="100"
                               Height="35"/>
    <TextBlock Text=" minutes" 
               VerticalAlignment="Center" 
               FontSize="14"
               Margin="10,0,0,0"/>
</StackPanel>
```

### Precision vs Speed Controls

```xml
<StackPanel Margin="20" Width="300">
    <!-- Fine control -->
    <TextBlock Text="Fine Adjustment (±1):" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="100"
                               ScrollInterval="1"
                               IsScrollingOnCircle="True"
                               ShowSpinButton="True"
                               Width="200"
                               Height="30"
                               Margin="0,0,0,20"/>
    
    <!-- Medium control -->
    <TextBlock Text="Medium Adjustment (±10):" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="100"
                               ScrollInterval="10"
                               IsScrollingOnCircle="True"
                               ShowSpinButton="True"
                               Width="200"
                               Height="30"
                               Margin="0,0,0,20"/>
    
    <!-- Coarse control -->
    <TextBlock Text="Coarse Adjustment (±100):" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox Value="100"
                               ScrollInterval="100"
                               IsScrollingOnCircle="True"
                               ShowSpinButton="True"
                               Width="200"
                               Height="30"/>
</StackPanel>
```

### Age Selector with Visual Feedback

```xml
<StackPanel Margin="20">
    <TextBlock Text="Age" FontSize="14" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox x:Name="ageTextBox"
                               Value="25"
                               MinValue="18"
                               MaxValue="120"
                               ScrollInterval="1"
                               ShowSpinButton="True"
                               IsScrollingOnCircle="True"
                               EnableExtendedScrolling="True"
                               TextSelectionOnFocus="True"
                               TextAlignment="Center"
                               FontSize="16"
                               Width="150"
                               Height="35"/>
    <TextBlock Text="18-120 years (use arrows or mouse)" 
               FontSize="10" 
               Foreground="Gray"
               Margin="0,5,0,0"/>
</StackPanel>
```

### Gaming Settings (Sensitivity)

```xml
<GroupBox Header="Mouse Sensitivity" Padding="15" Width="280">
    <StackPanel>
        <syncfusion:IntegerTextBox Value="50"
                                   MinValue="1"
                                   MaxValue="100"
                                   ScrollInterval="5"
                                   IsScrollingOnCircle="True"
                                   EnableExtendedScrolling="True"
                                   EnableRangeAdorner="True"
                                   RangeAdornerBackground="CornflowerBlue"
                                   TextAlignment="Center"
                                   FontSize="18"
                                   FontWeight="Bold"
                                   Height="40"/>
        
        <Grid Margin="0,10,0,0">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Column="0" Text="Low" FontSize="10" Foreground="Gray"/>
            <TextBlock Grid.Column="1" Text="High" FontSize="10" Foreground="Gray" HorizontalAlignment="Right"/>
        </Grid>
    </StackPanel>
</GroupBox>
```

These interactive features make IntegerTextBox highly user-friendly, allowing users to choose their preferred method of value adjustment based on context and personal preference.
