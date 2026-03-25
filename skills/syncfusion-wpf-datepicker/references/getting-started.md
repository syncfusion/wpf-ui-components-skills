# Getting Started with SfDatePicker

## Table of Contents
- [Installation and Setup](#installation-and-setup)
- [Adding via Designer](#adding-via-designer)
- [Adding Manually in XAML](#adding-manually-in-xaml)
- [Adding Manually in C#](#adding-manually-in-c)
- [Setting Initial Date](#setting-initial-date)
- [Handling Date Change Events](#handling-date-change-events)

## Installation and Setup

The SfDatePicker control requires NuGet packages to be installed. These assemblies provide the core functionality for the date picker.

### Required NuGet Packages
Install the following packages via NuGet Package Manager:

- **Syncfusion.SfInput.WPF** – Core input controls
- **Syncfusion.SfShared.WPF** – Shared components and utilities

### Required Assembly References
Add these assembly references to your project:
```
Syncfusion.SfInput.WPF
Syncfusion.SfShared.WPF
```

### NuGet Installation Steps
1. Right-click on your WPF project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for "Syncfusion.SfInput.WPF"
4. Click **Install** (this will also install SfShared.WPF as a dependency)

For more details, refer to the official NuGet installation guide for WPF.

## Adding via Designer

The easiest way to add SfDatePicker to your WPF application is via the Visual Studio Designer.

### Steps:
1. Open the **Toolbox** in Visual Studio
2. Search for **SfDatePicker**
3. Drag it onto your Window or UserControl
4. The required assemblies will be automatically added to your project

**Result:** The control is added to your XAML and ready to use. Required assembly references are automatically configured.

## Adding Manually in XAML

If you prefer to add the control via XAML, follow these steps:

### Step 1: Add Assembly References
Ensure the following assemblies are referenced in your project:
- Syncfusion.SfInput.WPF
- Syncfusion.SfShared.WPF

### Step 2: Import Syncfusion Namespace
Add the Syncfusion namespace to your XAML file:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Step 3: Declare the Control
Add the SfDatePicker control to your XAML:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="DatePickerSample.MainWindow"
        Title="SfDatePicker Sample" 
        Height="350" 
        Width="525">
    <Grid>
        <!-- Adding SfDatePicker control -->
        <syncfusion:SfDatePicker x:Name="sfDatePicker" 
                                 Width="200"
                                 VerticalAlignment="Center"
                                 HorizontalAlignment="Center"/>
    </Grid>
</Window>
```

**Key attributes:**
- `x:Name` – Unique identifier for code-behind access
- `Width` – Control width
- `VerticalAlignment` and `HorizontalAlignment` – Positioning

## Adding Manually in C#

Add SfDatePicker programmatically in your C# code:

### Step 1: Add Assembly References
Ensure these assemblies are referenced:
- Syncfusion.SfInput.WPF
- Syncfusion.SfShared.WPF

### Step 2: Import the Namespace
```csharp
using Syncfusion.Windows.Controls.Input;
```

### Step 3: Create and Configure the Control

```csharp
using Syncfusion.Windows.Controls.Input;
using System.Windows;

namespace DatePickerSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();

            // Create an instance of SfDatePicker control
            SfDatePicker sfDatePicker = new SfDatePicker();
            
            // Configure the control
            sfDatePicker.Width = 200;
            sfDatePicker.VerticalAlignment = VerticalAlignment.Center;
            sfDatePicker.HorizontalAlignment = HorizontalAlignment.Center;

            // Add to the window
            this.Content = sfDatePicker;
        }
    }
}
```

## Setting Initial Date

By default, SfDatePicker displays the current system date. You can set a specific date using the **Value** property.

### Via XAML
```xaml
<syncfusion:SfDatePicker Value="5/30/2026" 
                         x:Name="sfDatePicker"/>
```

### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.Value = new DateTime(2026, 5, 30);
```

### Using Data Binding
```xaml
<syncfusion:SfDatePicker Value="{Binding SelectedDate}" 
                         x:Name="sfDatePicker"/>
```

**Behavior:** If no value is assigned, the control automatically displays the current system date.

## Handling Date Change Events

When a user selects a new date, the **ValueChanged** event fires. This event provides access to both the previous and newly selected dates.

### Event Handler Details

The `ValueChanged` event passes a `DependencyPropertyChangedEventArgs` object with:
- **OldValue** – Previously selected date
- **NewValue** – Currently selected date

### Via XAML
```xaml
<syncfusion:SfDatePicker ValueChanged="SfDatePicker_ValueChanged" 
                         x:Name="sfDatePicker"/>
```

### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.ValueChanged += SfDatePicker_ValueChanged;
```

### Event Handler Implementation

```csharp
private void SfDatePicker_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (e.OldValue != null && e.NewValue != null)
    {
        DateTime oldDate = (DateTime)e.OldValue;
        DateTime newDate = (DateTime)e.NewValue;
        
        Console.WriteLine($"Old selected date: {oldDate:d}");
        Console.WriteLine($"New selected date: {newDate:d}");
        
        // Perform custom logic
        MessageBox.Show($"Date changed from {oldDate:d} to {newDate:d}");
    }
}
```

### Common Event Scenarios

**Scenario 1: Log date changes**
```csharp
sfDatePicker.ValueChanged += (d, e) =>
{
    System.Diagnostics.Debug.WriteLine($"Selected: {e.NewValue}");
};
```

**Scenario 2: Update dependent controls**
```csharp
sfDatePicker.ValueChanged += (d, e) =>
{
    DateTime selectedDate = (DateTime)e.NewValue;
    int dayOfWeek = (int)selectedDate.DayOfWeek;
    dayOfWeekLabel.Content = selectedDate.ToString("dddd");
};
```

**Scenario 3: Validate date selection**
```csharp
sfDatePicker.ValueChanged += (d, e) =>
{
    DateTime selectedDate = (DateTime)e.NewValue;
    if (selectedDate < DateTime.Now.Date)
    {
        MessageBox.Show("Past dates not allowed");
        sfDatePicker.Value = DateTime.Now;
    }
};
```

## Next Steps

Once you've set up the basic control, explore:
- **Date Formatting** – Display dates in various formats
- **Appearance and Styling** – Customize colors and themes
- **Value Management** – Handle null values and watermarks
- **Date Selector Customization** – Modify the dropdown picker appearance
