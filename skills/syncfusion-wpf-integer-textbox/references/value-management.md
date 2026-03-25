# Value Management

## Table of Contents
- [Overview](#overview)
- [Value Property](#value-property)
- [Changing Integer Values](#changing-integer-values)
- [Data Binding](#data-binding)
- [Paste Mode Configuration](#paste-mode-configuration)
- [Spin Button Features](#spin-button-features)
- [ValueChanged Event](#valuechanged-event)
- [Null Value Handling](#null-value-handling)
- [Watermark Text](#watermark-text)
- [Watermark Customization](#watermark-customization)

## Overview

The IntegerTextBox provides comprehensive value management features including direct value setting, data binding, paste operations, spin buttons for increment/decrement, event handling, null value support, and watermark display for empty states.

## Value Property

The `Value` property is the primary way to get or set the integer value in IntegerTextBox.

**Property Type:** `int?` (nullable integer)  
**Default Value:** `0`

### Setting Value in XAML

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="150" 
                           Height="25"
                           Value="10"/>
```

### Setting Value in C#

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Width = 150;
integerTextBox.Height = 25;
integerTextBox.Value = 10;
```

### Important: Do Not Use Text Property

**⚠️ Critical:** Do NOT use the `Text` property to set values for IntegerTextBox. The `Text` property is inherited from the base TextBox class but should not be used as it may cause unexpected behavior.

```csharp
// ✅ CORRECT
integerTextBox.Value = 100;

// ❌ INCORRECT - Do not use
// integerTextBox.Text = "100";
```

## Changing Integer Values

You can change the IntegerTextBox value programmatically or through user interaction.

### Programmatic Value Change

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Height="25"
                           Width="150" 
                           Value="10"/>

<Button Content="Set to 100" 
        Click="SetValueButton_Click" 
        Margin="0,10,0,0"/>
```

```csharp
private void SetValueButton_Click(object sender, RoutedEventArgs e)
{
    integerTextBox.Value = 100;
}
```

## Data Binding

IntegerTextBox supports full data binding capabilities for the Value property, making it perfect for MVVM applications.

### Basic Data Binding

Bind the Value property to a ViewModel property:

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox1" 
                           Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}" 
                           Height="25" 
                           Width="100"/>

<syncfusion:IntegerTextBox x:Name="integerTextBox2" 
                           Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}" 
                           Width="100" 
                           Height="25"/>
```

### ViewModel Implementation

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class ViewModel : INotifyPropertyChanged
{
    private int myValue;
    
    public int MyValue
    {
        get => myValue;
        set
        {
            if (myValue != value)
            {
                myValue = value;
                OnPropertyChanged();
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedPropertyChangedEventArgs(propertyName));
    }
}
```

### Binding Options

**UpdateSourceTrigger Options:**
- `PropertyChanged` - Updates immediately on value change (recommended)
- `LostFocus` - Updates when control loses focus
- `Explicit` - Updates only when explicitly triggered

```xml
<!-- Immediate update -->
<syncfusion:IntegerTextBox Value="{Binding Quantity, UpdateSourceTrigger=PropertyChanged}"/>

<!-- Update on focus lost -->
<syncfusion:IntegerTextBox Value="{Binding Quantity, UpdateSourceTrigger=LostFocus}"/>
```

## Paste Mode Configuration

The IntegerTextBox provides two paste modes to control how clipboard content is pasted into the control.

**Property:** `PasteMode`  
**Type:** `PasteMode` enum  
**Values:** `Default`, `Advanced`  
**Default:** `Default`

### Default Paste Mode

In `Default` mode, pasting replaces the entire value with the copied value, applying the current number format.

```xml
<syncfusion:IntegerTextBox PasteMode="Default" 
                           Value="12345"
                           Width="150"
                           Height="30"/>
```

### Advanced Paste Mode

In `Advanced` mode, the paste behavior is context-sensitive based on selection and cursor position.

```xml
<syncfusion:IntegerTextBox PasteMode="Advanced" 
                           Value="12345"
                           Width="150"
                           Height="30"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.PasteMode = PasteMode.Advanced;
integerTextBox.Value = 12345;
```

### Advanced Paste Mode Behavior Table

| Scenario | Paste Behavior |
|----------|----------------|
| **Whole value selected** | Replaces entire value with copied value |
| **Cursor at position, no decimal in copied value** | Inserts copied value at cursor position |
| **Cursor at position, decimal in copied value** | Paste operation blocked (integers only) |
| **Cursor at position, current value is 0 or null** | Replaces entire value with copied value |
| **Part of number selected, copied value has decimal** | Paste operation blocked |

### Paste Mode Examples

**Scenario 1:** Cursor at position 3 in value "12345", copying "67":
- Default mode: Entire value becomes "67"
- Advanced mode: Value becomes "12673 45"

**Scenario 2:** Whole value selected, copying "999":
- Both modes: Value becomes "999"

**Scenario 3:** Copying "12.5" (contains decimal):
- Advanced mode: Paste operation is blocked (no change)

## Spin Button Features

Enable up/down spin buttons for easy value increment and decrement.

**Property:** `ShowSpinButton`  
**Type:** `bool`  
**Default:** `false`

### Enabling Spin Buttons

```xml
<syncfusion:IntegerTextBox Height="30" 
                           Width="150" 
                           Value="50"
                           ShowSpinButton="True"/>
```

```csharp
IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Height = 30;
integerTextBox.Width = 150;
integerTextBox.Value = 50;
integerTextBox.ShowSpinButton = true;
```

### Spin Button Behavior

When `ShowSpinButton` is `true`:
- **Up Button** - Increments value by `ScrollInterval`
- **Down Button** - Decrements value by `ScrollInterval`
- Respects `MinValue` and `MaxValue` constraints

### Spin Button with Scroll Interval

```xml
<syncfusion:IntegerTextBox Value="0"
                           MinValue="0"
                           MaxValue="100"
                           ScrollInterval="5"
                           ShowSpinButton="True"
                           Width="150"
                           Height="30"/>
```

This configuration creates a control where:
- Clicking up button increases by 5
- Clicking down button decreases by 5
- Value stays between 0 and 100

## ValueChanged Event

The `ValueChanged` event fires whenever the Value property changes, allowing you to respond to value updates.

**Event:** `ValueChanged`  
**Event Args:** `DependencyPropertyChangedEventArgs`

### Subscribing to ValueChanged

**XAML:**
```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox"
                           ValueChanged="IntegerTextBox_ValueChanged"
                           Width="150"
                           Height="30"/>
```

**C#:**
```csharp
integerTextBox.ValueChanged += new PropertyChangedCallback(IntegerTextBox_ValueChanged);
```

### Event Handler Implementation

```csharp
private void IntegerTextBox_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    // Get the control that fired the event
    IntegerTextBox control = d as IntegerTextBox;
    
    // Get old and new values
    var oldValue = e.OldValue;
    var newValue = e.NewValue;
    
    // Perform actions based on value change
    MessageBox.Show($"Value changed from {oldValue} to {newValue}");
    
    // Example: Update related controls or perform calculations
    if (newValue != null && (int)newValue > 100)
    {
        // Handle values over 100
    }
}
```

### Practical ValueChanged Example

```xml
<StackPanel Margin="20">
    <TextBlock Text="Enter Price:" Margin="0,0,0,5"/>
    <syncfusion:IntegerTextBox x:Name="priceTextBox"
                               ValueChanged="PriceTextBox_ValueChanged"
                               MinValue="0"
                               Width="150"
                               Height="30"/>
    
    <TextBlock Text="Enter Quantity:" Margin="0,15,0,5"/>
    <syncfusion:IntegerTextBox x:Name="quantityTextBox"
                               ValueChanged="CalculateTotal"
                               MinValue="1"
                               Width="150"
                               Height="30"/>
    
    <TextBlock x:Name="totalTextBlock" 
               Text="Total: $0" 
               FontSize="16"
               FontWeight="Bold"
               Margin="0,15,0,0"/>
</StackPanel>
```

```csharp
private void PriceTextBox_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    CalculateTotal(d, e);
}

private void CalculateTotal(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (priceTextBox.Value != null && quantityTextBox.Value != null)
    {
        int price = priceTextBox.Value.Value;
        int quantity = quantityTextBox.Value.Value;
        int total = price * quantity;
        
        totalTextBlock.Text = $"Total: ${total:N0}";
    }
}
```

## Null Value Handling

IntegerTextBox can display null or custom values when the Value is null, providing better user experience for optional fields.

**Properties:**
- `UseNullOption` - Enable null value handling (default: `false`)
- `NullValue` - Value to display when null (default: `null`)

### Displaying Null

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Height="25"
                           Width="100" 
                           UseNullOption="True"  
                           NullValue="{x:Null}"/>
```

```csharp
integerTextBox.UseNullOption = true;
integerTextBox.NullValue = null;
```

When `Value` is null and `UseNullOption` is true, the control displays an empty field.

### Displaying Custom Null Value

Set a default integer value to display instead of null:

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Height="25"
                           Width="100" 
                           UseNullOption="True" 
                           NullValue="10"/>
```

```csharp
integerTextBox.UseNullOption = true;
integerTextBox.NullValue = 10;
```

When `Value` is null, the control displays "10".

### Null Value Behavior

**Important Rules:**
1. `UseNullOption` must be `true` to enable null value handling
2. If `NullValue` and `WatermarkText` are both set, only `NullValue` displays
3. Setting `Value` to `null` when `UseNullOption` is `false` displays 0

## Watermark Text

Display informative text when the control is empty and not focused, guiding users on what to enter.

**Properties:**
- `WatermarkText` - The text to display (default: `string.Empty`)
- `WatermarkTextIsVisible` - Enable watermark display (default: `false`)

### Basic Watermark

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100"
                           Height="25" 
                           UseNullOption="True" 
                           WatermarkText="Type here"
                           WatermarkTextIsVisible="True"/>
```

```csharp
integerTextBox.UseNullOption = true;
integerTextBox.WatermarkText = "Type here";
integerTextBox.WatermarkTextIsVisible = true;
```

### Watermark Display Conditions

Watermark text appears when ALL of the following are true:
1. `WatermarkTextIsVisible` is `true`
2. `UseNullOption` is `true`
3. `Value` is `null` or empty
4. Control does not have focus

### Setting Watermark Foreground

Customize the watermark text color:

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100"
                           Height="25" 
                           UseNullOption="True" 
                           WatermarkText="Enter age"
                           WatermarkTextIsVisible="True" 
                           WatermarkTextForeground="Gray"/>
```

```csharp
integerTextBox.UseNullOption = true;
integerTextBox.WatermarkText = "Enter age";
integerTextBox.WatermarkTextIsVisible = true;
integerTextBox.WatermarkTextForeground = Brushes.Gray;
```

**Default:** `Black`

## Watermark Customization

### Watermark with Opacity

Control watermark transparency:

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="150" 
                           Height="30"
                           WatermarkText="Enter value" 
                           WatermarkTextIsVisible="True" 
                           WatermarkOpacity="0.5" 
                           UseNullOption="True"/>
```

### Custom Watermark Template

Create a fully customized watermark appearance using `WatermarkTemplate`:

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="200" 
                           Height="30"
                           WatermarkText="Type Here" 
                           CornerRadius="3" 
                           WatermarkTextIsVisible="True" 
                           WatermarkOpacity="0.5" 
                           UseNullOption="True">
    <syncfusion:IntegerTextBox.WatermarkTemplate>
        <DataTemplate>
            <Border Background="LightYellow" 
                    BorderBrush="Orange" 
                    BorderThickness="1"
                    CornerRadius="3"
                    Padding="5,2">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Text="💡" 
                               VerticalAlignment="Center" 
                               Margin="0,0,5,0"/>
                    <TextBlock Text="{Binding}" 
                               VerticalAlignment="Center"
                               FontStyle="Italic"
                               Foreground="DarkOrange"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:IntegerTextBox.WatermarkTemplate>
</syncfusion:IntegerTextBox>
```

### Watermark Template with Icon

```xml
<syncfusion:IntegerTextBox Width="200" 
                           Height="35"
                           WatermarkText="Enter quantity (1-999)" 
                           WatermarkTextIsVisible="True" 
                           UseNullOption="True">
    <syncfusion:IntegerTextBox.WatermarkTemplate>
        <DataTemplate>
            <Border Background="#FFF9F9" 
                    Padding="5,0,0,0">
                <StackPanel Orientation="Horizontal">
                    <Path Data="M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2Z" 
                          Fill="LightGray" 
                          Width="16" 
                          Height="16"
                          Stretch="Uniform"
                          VerticalAlignment="Center"
                          Margin="0,0,5,0"/>
                    <TextBlock Text="{Binding}" 
                               VerticalAlignment="Center"
                               Foreground="Gray"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:IntegerTextBox.WatermarkTemplate>
</syncfusion:IntegerTextBox>
```

## Important Notes

### NullValue vs Watermark Priority

When both `NullValue` and `WatermarkText` are specified:
- `NullValue` takes priority and displays
- `WatermarkText` is ignored

```xml
<!-- Only "999" will display when value is null -->
<syncfusion:IntegerTextBox UseNullOption="True"
                           NullValue="999"
                           WatermarkText="Enter value"
                           WatermarkTextIsVisible="True"/>
```

### UseNullOption Requirement

The `UseNullOption` property MUST be enabled to see either `NullValue` or `WatermarkText`:

```xml
<!-- ❌ Won't show null value or watermark -->
<syncfusion:IntegerTextBox NullValue="10"
                           WatermarkText="Enter value"/>

<!-- ✅ Will show null value or watermark -->
<syncfusion:IntegerTextBox UseNullOption="True"
                           NullValue="10"
                           WatermarkText="Enter value"/>
```

## Complete Example

```xml
<StackPanel Margin="20">
    <TextBlock Text="Product Quantity:" FontWeight="Bold"/>
    
    <syncfusion:IntegerTextBox x:Name="quantityTextBox"
                               Width="200"
                               Height="35"
                               Value="1"
                               MinValue="1"
                               MaxValue="999"
                               ShowSpinButton="True"
                               ScrollInterval="1"
                               PasteMode="Advanced"
                               UseNullOption="True"
                               WatermarkText="Enter quantity (1-999)"
                               WatermarkTextIsVisible="True"
                               WatermarkTextForeground="Gray"
                               ValueChanged="QuantityTextBox_ValueChanged"
                               Margin="0,5,0,0"/>
    
    <TextBlock x:Name="statusText" 
               Text="Quantity: 1" 
               Margin="0,10,0,0"
               FontSize="14"/>
</StackPanel>
```

```csharp
private void QuantityTextBox_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (e.NewValue != null)
    {
        int quantity = (int)e.NewValue;
        statusText.Text = $"Quantity: {quantity}";
        
        // Additional logic based on quantity
        if (quantity > 100)
        {
            statusText.Foreground = Brushes.Orange;
            statusText.Text += " (Bulk order)";
        }
        else
        {
            statusText.Foreground = Brushes.Black;
        }
    }
}
```

This example demonstrates:
- Value binding and initial value
- Min/max constraints
- Spin buttons with scroll interval
- Advanced paste mode
- Null value support
- Watermark text
- ValueChanged event handling
- Dynamic UI updates
