# Getting Started with SfTimePicker

## Installation and Assembly References

To use SfTimePicker in your WPF application, add the following assemblies to your project:

- **Syncfusion.SfInput.WPF** - Core time picker control
- **Syncfusion.SfShared.WPF** - Shared dependencies

### Adding via NuGet Package Manager

```powershell
Install-Package Syncfusion.SfInput.WPF
```

For detailed NuGet installation guidance, see the [Syncfusion WPF NuGet documentation](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages).

## Adding Control via Designer

The easiest way to add SfTimePicker:

1. Open your WPF project in Visual Studio
2. Locate the Syncfusion TimePicker in the **Toolbox**
3. Drag it onto your designer view
4. The required assembly references are **automatically added**

## Adding Control Manually in XAML

To add SfTimePicker manually in XAML:

1. Add assembly references to your project (see above)
2. Import the Syncfusion schema: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`
3. Declare the control:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        Title="TimePicker Sample" Height="350" Width="525">
    <Grid>
        <!-- Basic SfTimePicker control -->
        <syncfusion:SfTimePicker x:Name="sfTimePicker" 
                                 Width="200"/>
    </Grid>
</Window>
```

## Adding Control Manually in C#

To create SfTimePicker programmatically:

```csharp
using Syncfusion.Windows.Controls.Input;
using System.Windows;

namespace SfTimePickerSample 
{    
    public partial class MainWindow : Window 
    {
        public MainWindow() 
        {
            InitializeComponent();

            // Create an instance of SfTimePicker
            SfTimePicker sfTimePicker = new SfTimePicker();

            // Set size and initial value
            sfTimePicker.Width = 200;
            sfTimePicker.Value = new TimeSpan(14, 30, 00); // 2:30 PM

            // Add to window
            this.Content = sfTimePicker;
        } 
    }
}
```

## Basic Time Picker Example

Here's a complete example with a time picker and value display:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        Title="Time Picker Example" Height="200" Width="300">
    <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center" Spacing="10">
        <TextBlock Text="Select Time:" FontSize="14" FontWeight="Bold"/>
        <syncfusion:SfTimePicker x:Name="timePicker" 
                                 Width="150"
                                 Value="09:00:00"
                                 ValueChanged="TimePicker_ValueChanged"/>
        <TextBlock x:Name="resultText" Text="Selected time: 09:00 AM" FontSize="12"/>
    </StackPanel>
</Window>
```

```csharp
private void TimePicker_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) 
{
    TimeSpan newTime = (TimeSpan)e.NewValue;
    resultText.Text = $"Selected time: {newTime:hh\\:mm\\ tt}";
}
```

## Default Behavior

If you don't set an explicit value, SfTimePicker automatically uses the **current system time**.

```xaml
<!-- Will default to current system time -->
<syncfusion:SfTimePicker x:Name="timePicker" Width="200"/>
```

## Applying Themes

SfTimePicker supports multiple built-in themes via **SfSkinManager**:

```xaml
<syncfusion:SfTimePicker x:Name="timePicker" 
                         syncfusion:SfSkinManager.VisualStyle="MaterialLight"/>
```

**Available themes:** MaterialLight, MaterialDark, Office2019Blue, Office2019Black, HighContrastBlack, etc.

For theme customization via **Theme Studio**, see the [Syncfusion WPF Themes documentation](https://help.syncfusion.com/wpf/themes/theme-studio).

## ValueChanged Event

Handle time value changes with the `ValueChanged` event:

```xaml
<syncfusion:SfTimePicker ValueChanged="TimePicker_ValueChanged" 
                         Name="sfTimePicker"/>
```

```csharp
private void TimePicker_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) 
{
    TimeSpan oldTime = (TimeSpan)e.OldValue;
    TimeSpan newTime = (TimeSpan)e.NewValue;
    
    Console.WriteLine($"Previous time: {oldTime}");
    Console.WriteLine($"New time: {newTime}");
}
```

The event provides:
- **OldValue** - Previously selected time
- **NewValue** - Currently selected time

## Next Steps

- Explore **[Setting Time Values](setting-time-values.md)** to manage time programmatically
- Learn **[Time Formatting](time-formatting.md)** for custom display formats
- Customize the **[Time Selector](time-selector-customization.md)** appearance
