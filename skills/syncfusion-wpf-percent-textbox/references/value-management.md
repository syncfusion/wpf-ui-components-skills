# Value Management and Binding

## Table of Contents
- [Setting and Changing PercentValue](#setting-and-changing-percentvalue)
- [Data Binding](#data-binding)
- [PercentValue Changed Event](#percentvalue-changed-event)
- [Null Value Support](#null-value-support)
- [Watermark Text](#watermark-text)
- [Paste Mode](#paste-mode)
- [Spin Buttons](#spin-buttons)

## Setting and Changing PercentValue

The `PercentValue` property is the primary way to set and retrieve the percentage value in the PercentTextBox control.

### Basic Value Setting

**XAML:**
```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="75.5"/>
```

**C#:**
```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.Width = 150;
percentTextBox.Height = 30;
percentTextBox.PercentValue = 75.5;
```

### Programmatic Value Changes

```csharp
// Get current value
double currentValue = percentTextBox.PercentValue;

// Set new value
percentTextBox.PercentValue = 50.0;

// Increment value
percentTextBox.PercentValue += 10;

// Calculate and set value
double discount = CalculateDiscount();
percentTextBox.PercentValue = discount;
```

## Data Binding

Data binding enables automatic synchronization between the PercentTextBox and your ViewModel properties.

### Unidirectional Binding (Source → Target)

Updates flow from ViewModel to PercentTextBox only:

**XAML:**
```xml
<syncfusion:PercentTextBox PercentValue="{Binding DiscountPercentage, Mode=OneWay}"
                           Width="150"
                           Height="30"/>
```

**ViewModel:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    private double discountPercentage = 15.0;
    
    public double DiscountPercentage
    {
        get { return discountPercentage; }
        set
        {
            discountPercentage = value;
            OnPropertyChanged(nameof(DiscountPercentage));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedPropertyChangedEventArgs(propertyName));
    }
}
```

### Bidirectional Binding (Source ↔ Target)

Updates flow in both directions - changes in either PercentTextBox or ViewModel update the other:

**XAML:**
```xml
<syncfusion:PercentTextBox PercentValue="{Binding DiscountPercentage, 
                                                   Mode=TwoWay,
                                                   UpdateSourceTrigger=PropertyChanged}"
                           Width="150"
                           Height="30"/>
```

**Why UpdateSourceTrigger=PropertyChanged?**
- **Default:** Updates ViewModel only when PercentTextBox loses focus
- **PropertyChanged:** Updates ViewModel immediately on every keystroke
- Use PropertyChanged for real-time synchronization

### Binding Multiple Controls to Same Property

```xml
<StackPanel>
    <TextBlock Text="Discount Percentage:"/>
    <syncfusion:PercentTextBox PercentValue="{Binding DiscountPercentage, 
                                                       UpdateSourceTrigger=PropertyChanged}"
                               Width="150" Height="30" Margin="0,5"/>
    
    <TextBlock Text="Same value displayed here:"/>
    <syncfusion:PercentTextBox PercentValue="{Binding DiscountPercentage, 
                                                       UpdateSourceTrigger=PropertyChanged}"
                               Width="150" Height="30" Margin="0,5"/>
    
    <TextBlock Text="{Binding DiscountPercentage, StringFormat='Current: {0}%'}"/>
</StackPanel>
```

Both PercentTextBoxes stay synchronized automatically.

### Complete Data Binding Example

**MainWindow.xaml:**
```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:PercentTextBoxSample"
        Title="Data Binding Example" Height="300" Width="400">
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <StackPanel Margin="20">
        <TextBlock Text="Tax Rate:"/>
        <syncfusion:PercentTextBox PercentValue="{Binding TaxRate, 
                                                           UpdateSourceTrigger=PropertyChanged}"
                                   Width="200" Height="30" Margin="0,5,0,20"/>
        
        <TextBlock Text="Commission:"/>
        <syncfusion:PercentTextBox PercentValue="{Binding Commission, 
                                                           UpdateSourceTrigger=PropertyChanged}"
                                   Width="200" Height="30" Margin="0,5,0,20"/>
        
        <Button Content="Reset Values" Command="{Binding ResetCommand}" Width="100"/>
    </StackPanel>
</Window>
```

**ViewModel.cs:**
```csharp
using System.ComponentModel;
using System.Windows.Input;

public class ViewModel : INotifyPropertyChanged
{
    private double taxRate = 7.5;
    private double commission = 15.0;

    public double TaxRate
    {
        get { return taxRate; }
        set
        {
            taxRate = value;
            OnPropertyChanged(nameof(TaxRate));
        }
    }

    public double Commission
    {
        get { return commission; }
        set
        {
            commission = value;
            OnPropertyChanged(nameof(Commission));
        }
    }

    public ICommand ResetCommand { get; }

    public ViewModel()
    {
        ResetCommand = new RelayCommand(ResetValues);
    }

    private void ResetValues()
    {
        TaxRate = 0;
        Commission = 0;
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## PercentValue Changed Event

Track value changes using the `PercentValueChanged` event.

### Event Setup

**XAML:**
```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           PercentValueChanged="PercentTextBox_PercentValueChanged"/>
```

**C#:**
```csharp
percentTextBox.PercentValueChanged += PercentTextBox_PercentValueChanged;
```

### Event Handler

```csharp
private void PercentTextBox_PercentValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    // Get old and new values
    double oldValue = (double)e.OldValue;
    double newValue = (double)e.NewValue;
    
    // Calculate change
    double change = newValue - oldValue;
    
    // Log or respond to change
    Console.WriteLine($"Value changed from {oldValue}% to {newValue}% (change: {change}%)");
    
    // Trigger other updates
    UpdateCalculations(newValue);
}

private void UpdateCalculations(double newPercent)
{
    // Perform dependent calculations
    double result = baseAmount * (newPercent / 100);
    resultTextBlock.Text = $"Result: {result:C}";
}
```

### Use Cases for PercentValueChanged

1. **Real-time Calculations:** Update totals when discount percentage changes
2. **Validation:** Enforce business rules (e.g., total allocations can't exceed 100%)
3. **Logging:** Track user input history
4. **Conditional Formatting:** Change appearance based on value thresholds

## Null Value Support

By default, PercentTextBox displays zero when the value is null. Configure null handling using `UseNullOption` and `NullValue` properties.

### Displaying Null as Empty

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           UseNullOption="True"
                           NullValue="{x:Null}"/>
```

**Result:** Control shows empty/blank when value is null.

### Displaying Custom Value for Null

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           UseNullOption="True"
                           NullValue="10"/>
```

**Result:** Control shows "10 %" when value is null.

### Programmatic Null Handling

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.UseNullOption = true;
percentTextBox.NullValue = null; // or specific value like 10

// Set to null
percentTextBox.PercentValue = double.NaN; // or use nullable binding

// Check if null
if (double.IsNaN(percentTextBox.PercentValue))
{
    // Handle null case
}
```

## Watermark Text

Display placeholder text when the control is empty, unfocused, and null.

### Basic Watermark

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           UseNullOption="True"
                           WatermarkText="Enter percentage"
                           WatermarkTextIsVisible="True"/>
```

**When watermark appears:**
- PercentValue is null or empty
- Control does not have focus
- UseNullOption is True
- WatermarkTextIsVisible is True

### Watermark Foreground Color

```xml
<syncfusion:PercentTextBox WatermarkText="Type here"
                           WatermarkTextIsVisible="True"
                           WatermarkTextForeground="Gray"
                           UseNullOption="True"/>
```

### Custom Watermark Template

Create rich watermark content with DataTemplate:

```xml
<syncfusion:PercentTextBox Width="200" Height="35"
                           WatermarkText="Enter discount %"
                           WatermarkTextIsVisible="True"
                           WatermarkOpacity="0.6"
                           UseNullOption="True">
    <syncfusion:PercentTextBox.WatermarkTemplate>
        <DataTemplate>
            <Border Background="LightYellow" 
                    CornerRadius="3"
                    Padding="5,0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Text="💡" Margin="0,0,5,0" VerticalAlignment="Center"/>
                    <TextBlock Text="{Binding}" 
                               VerticalAlignment="Center"
                               FontStyle="Italic"
                               Foreground="DarkGray"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:PercentTextBox.WatermarkTemplate>
</syncfusion:PercentTextBox>
```

### Important: NullValue vs WatermarkText Priority

If both `NullValue` and `WatermarkText` are specified, `NullValue` takes precedence:

```xml
<!-- Will show "10 %" (NullValue), NOT watermark -->
<syncfusion:PercentTextBox UseNullOption="True"
                           NullValue="10"
                           WatermarkText="This won't show"/>

<!-- Will show watermark -->
<syncfusion:PercentTextBox UseNullOption="True"
                           NullValue="{x:Null}"
                           WatermarkText="This will show"/>
```

## Paste Mode

Control how pasted text is inserted into the PercentTextBox.

### Default Paste Mode

By default, pasting replaces the entire value:

```xml
<syncfusion:PercentTextBox PasteMode="Default"
                           PercentValue="100"/>
```

**Behavior:** If you copy "50" and paste, entire value becomes "50 %"

### Advanced Paste Mode

Enables smart pasting based on cursor position and selection:

```xml
<syncfusion:PercentTextBox PasteMode="Advanced"
                           PercentValue="12345.67"/>
```

**Advanced Paste Mode Behavior:**

| Scenario | Behavior |
|----------|----------|
| Whole value selected | Replaces entire value |
| Cursor at position, no decimal in copied text | Inserts at cursor |
| Cursor at position, decimal in copied text | Paste blocked |
| Control value is 0 or null | Replaces entire value |
| Part selected with decimal separator | Copied text must have decimal separator |
| Part selected without decimal separator | Copied text must NOT have decimal separator |

**Example:**
```csharp
// Value: 12345.67, cursor after "123"
// Copy "99" → Result: 12399345.67 (inserted at cursor)

// Value: 12345.67, select "234"
// Copy "99" → Result: 1299945.67 (replaced selection)

// Value: 12345.67, cursor after "."
// Copy "50.3" → Paste blocked (has decimal separator)
```

## Spin Buttons

Add up/down buttons for incrementing/decrementing values.

### Enable Spin Buttons

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="50"
                           ShowSpinButton="True"
                           ScrollInterval="5"/>
```

**Result:** Small up/down buttons appear on the right side of the control.

### Spin Button Behavior

- **Up Button:** Increases value by ScrollInterval
- **Down Button:** Decreases value by ScrollInterval
- Respects MinValue/MaxValue limits
- Can be clicked repeatedly for continuous changes

```csharp
PercentTextBox percentTextBox = new PercentTextBox();
percentTextBox.ShowSpinButton = true;
percentTextBox.ScrollInterval = 10; // Each click changes by 10%
percentTextBox.MinValue = 0;
percentTextBox.MaxValue = 100;
```

## Best Practices

### 1. Always Use UpdateSourceTrigger=PropertyChanged for Real-time Updates

```xml
<syncfusion:PercentTextBox PercentValue="{Binding Value, UpdateSourceTrigger=PropertyChanged}"/>
```

### 2. Handle Null Values Appropriately

```xml
<syncfusion:PercentTextBox UseNullOption="True" 
                           NullValue="{x:Null}"
                           WatermarkText="Optional percentage"/>
```

### 3. Provide User Guidance with Watermarks

```xml
<syncfusion:PercentTextBox WatermarkText="E.g., 15 for 15%"
                           WatermarkTextIsVisible="True"
                           UseNullOption="True"/>
```

### 4. Use PercentValueChanged for Dependent Calculations

```csharp
private void PercentTextBox_PercentValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    double percent = (double)e.NewValue;
    totalTextBlock.Text = (basePrice * percent / 100).ToString("C");
}
```

### 5. Consider Spin Buttons for Discrete Value Selection

```xml
<syncfusion:PercentTextBox ShowSpinButton="True" 
                           ScrollInterval="5"
                           PercentValue="25"/>
```
