# Getting Started with WPF Currency TextBox

This guide covers the essential steps to add and configure the Syncfusion WPF CurrencyTextBox control in your application.

## Overview

The CurrencyTextBox control restricts input to decimal values and displays them in currency format. It's ideal for financial applications requiring formatted numeric entry with validation, data binding, and culture support.

## Assembly Deployment

### Required Assemblies

To use CurrencyTextBox, you need the **Syncfusion.Shared.WPF** assembly.

**Reference:** `Syncfusion.Shared.WPF.dll`

### NuGet Package Installation

The recommended approach is to install via NuGet Package Manager:

1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Shared.WPF"
4. Install the package

**Package Name:** `Syncfusion.Shared.WPF`

For detailed NuGet installation instructions, refer to the Syncfusion documentation on installing NuGet packages in WPF applications.

## Adding CurrencyTextBox to Your Application

There are three ways to add the CurrencyTextBox control to your WPF application.

### Method 1: Via Designer

1. Open your WPF window/page in the Visual Studio designer
2. Locate the CurrencyTextBox in the Toolbox (under Syncfusion controls)
3. Drag and drop it onto your design surface

The required assembly reference (`Syncfusion.Shared.WPF`) will be added automatically.

### Method 2: Via XAML

**Step 1:** Create a new WPF project in Visual Studio.

**Step 2:** Add the `Syncfusion.Shared.WPF` assembly reference to your project.

**Step 3:** Import the Syncfusion WPF schema in your XAML:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="CurrencyTextBoxSample.MainWindow"
        Title="CurrencyTextBox Sample" Height="350" Width="525">
    <Grid>
        <!-- Adding CurrencyTextBox control -->
        <syncfusion:CurrencyTextBox 
            x:Name="currencyTextBox" 
            Width="100" 
            Height="25" 
            VerticalAlignment="Center" 
            HorizontalAlignment="Center"/>
    </Grid>
</Window>
```

### Method 3: Via C# Code

**Step 1:** Create a new WPF application via Visual Studio.

**Step 2:** Add the `Syncfusion.Shared.WPF` assembly reference to your project.

**Step 3:** Include the required namespace:

```csharp
using Syncfusion.Windows.Shared;
```

**Step 4:** Create an instance and add it to your window:

```csharp
// Creating an instance of CurrencyTextBox control
CurrencyTextBox currencyTextBox = new CurrencyTextBox();

// Setting height and width
currencyTextBox.Height = 25;
currencyTextBox.Width = 100;

// Adding CurrencyTextBox as window content
this.Content = currencyTextBox;
```

## Setting Value

The value of the CurrencyTextBox is set using the **`Value`** property.

### XAML Example

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100" 
    Height="23" 
    Value="100"/>
```

### C# Example

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 23;
currencyTextBox.Value = 100;
```

**⚠️ Important:** Do NOT use the `Text` property to set the value for CurrencyTextBox. Always use the `Value` property.

## Data Binding

Data binding creates a connection between the application UI and business logic. CurrencyTextBox supports unidirectional and bidirectional binding.

### Binding Between Two CurrencyTextBox Controls

**XAML:**

```xml
<StackPanel>
    <syncfusion:CurrencyTextBox 
        x:Name="currencyTextBox1" 
        Height="25" 
        Width="100" 
        Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}"/>
    
    <syncfusion:CurrencyTextBox 
        x:Name="currencyTextBox2" 
        Width="100" 
        Height="25" 
        Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}" />
</StackPanel>
```

**ViewModel.cs:**

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private double myValue;
    
    public double MyValue
    {
        get { return myValue; }
        set
        {
            myValue = value;
            RaisePropertyChanged("MyValue");
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Set DataContext in code-behind or XAML:**

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new ViewModel();
}
```

## Value Changed Notification

The CurrencyTextBox notifies value changes through the `ValueChanged` event.

### Subscribing to ValueChanged Event

**XAML:**

```xml
<syncfusion:CurrencyTextBox ValueChanged="CurrencyTextBox_ValueChanged"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.ValueChanged += new PropertyChangedCallback(CurrencyTextBox_ValueChanged);
```

### Handling the Event

```csharp
private void CurrencyTextBox_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    // Get old and new value
    var newValue = e.NewValue;
    var oldValue = e.OldValue;
    
    // Your logic here
    Console.WriteLine($"Value changed from {oldValue} to {newValue}");
}
```

## Min Max Value Restriction

Restrict the Value within maximum and minimum limits using `MinValue` and `MaxValue` properties.

### XAML Example

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100" 
    Height="25" 
    Value="455" 
    MaxValue="999.99" 
    MinValue="-999.99"/>
```

### C# Example

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;

// Setting minimum value
currencyTextBox.MinValue = -999.99;

// Setting maximum value
currencyTextBox.MaxValue = 999.99;

currencyTextBox.Value = 455;
```

**Behavior:** The control prevents values outside the min/max range. If user attempts to enter an invalid value, it will be automatically corrected based on validation mode settings.

## Step Interval for Increment/Decrement

The CurrencyTextBox allows value changes via keyboard arrow keys or mouse wheel.

### ScrollInterval Property

Use `ScrollInterval` to specify the increment/decrement amount. Default is 1.

**XAML:**

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Width="100" 
    Height="25" 
    Value="8" 
    IsScrollingOnCircle="True" 
    ScrollInterval="4"/>
```

**C#:**

```csharp
CurrencyTextBox currencyTextBox = new CurrencyTextBox();
currencyTextBox.Width = 100;
currencyTextBox.Height = 25;
currencyTextBox.MinValue = 0;
currencyTextBox.MaxValue = 100;
currencyTextBox.Value = 8;
currencyTextBox.IsScrollingOnCircle = true;
currencyTextBox.ScrollInterval = 4;
```

**Usage:**
- Press **Up Arrow** to increase value by ScrollInterval
- Press **Down Arrow** to decrease value by ScrollInterval
- **Mouse Wheel** scrolling (when `IsScrollingOnCircle = true`)

## Number Formatting Basics

Customize the number format using Culture, NumberFormat, or dedicated properties.

### Quick Culture Example

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Height="25" 
    Width="150"  
    Value="1234567"
    Culture="en-US"/>
```

### Quick NumberFormat Example

```xml
<syncfusion:CurrencyTextBox 
    x:Name="currencyTextBox" 
    Height="25" 
    Width="150"  
    Value="1234567">
    <syncfusion:CurrencyTextBox.NumberFormat>
        <numberformat:NumberFormatInfo 
            CurrencyGroupSeparator="/" 
            CurrencyDecimalDigits="4" 
            CurrencyDecimalSeparator="*"   
            CurrencySymbol="$"/>
    </syncfusion:CurrencyTextBox.NumberFormat>
</syncfusion:CurrencyTextBox>
```

**Note:** Detailed formatting is covered in the formatting-culture.md reference file.

## Complete Example

Here's a complete working example combining the key concepts:

**MainWindow.xaml:**

```xml
<Window x:Class="CurrencyTextBoxDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Currency TextBox Demo" Height="300" Width="400">
    <StackPanel Margin="20">
        <TextBlock Text="Product Price:" Margin="0,0,0,5"/>
        <syncfusion:CurrencyTextBox 
            x:Name="priceTextBox"
            Width="200"
            Height="30"
            Value="99.99"
            MinValue="0"
            MaxValue="99999.99"
            ScrollInterval="10"
            Culture="en-US"
            ValueChanged="PriceTextBox_ValueChanged"/>
        
        <TextBlock x:Name="resultText" Margin="0,10,0,0"/>
    </StackPanel>
</Window>
```

**MainWindow.xaml.cs:**

```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

namespace CurrencyTextBoxDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
        
        private void PriceTextBox_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
        {
            if (e.NewValue != null)
            {
                resultText.Text = $"Current Price: {e.NewValue:C2}";
            }
        }
    }
}
```

## Common Gotchas

1. **Always use `Value` property** - Never use `Text` to get/set currency values
2. **Assembly reference required** - Ensure `Syncfusion.Shared.WPF` is referenced
3. **Namespace import** - Include `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"` in XAML
4. **UpdateSourceTrigger** - Use `UpdateSourceTrigger=PropertyChanged` for immediate binding updates
5. **Type mismatch** - `Value` property is of type `double`, not `string`

## Next Steps

- **Formatting and Culture** → Read [formatting-culture.md](formatting-culture.md) for culture-specific formatting
- **Validation** → Read [validation-restrictions.md](validation-restrictions.md) for advanced validation
- **Appearance** → Read [appearance-styling.md](appearance-styling.md) for styling options
