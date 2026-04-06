# Getting Started with WPF NumericUpdown

## Assembly and Installation

To use the UpDown control, add the **Syncfusion.Shared.WPF** assembly reference to your WPF project.

### NuGet Package Installation

```bash
Install-Package Syncfusion.Shared.WPF
```

### Assembly Dependencies

Add reference to: `Syncfusion.Shared.WPF.dll`

**Namespace:**
```csharp
using Syncfusion.Windows.Shared;
```

## Adding UpDown via Designer

1. Open your WPF project in Visual Studio
2. In the Toolbox, locate the Syncfusion WPF Controls section
3. Drag the **UpDown** control onto your design view
4. The `Syncfusion.Shared.WPF` assembly is automatically added to references

Visual Studio Designer Integration provides automatic assembly registration.

## Adding UpDown via XAML

Import the Syncfusion schema in your XAML file:

```xaml
<Window x:Class="Application_New.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"    
        Title="MainWindow" Height="350" Width="525">
    <Grid Name="grid">
        <!-- Basic UpDown control -->
        <syncfusion:UpDown Name="upDown" Width="100" Height="25"/>
    </Grid>
</Window>
```

### XAML with Basic Properties

```xaml
<syncfusion:UpDown Name="upDown" 
                  Height="25" 
                  Width="100"
                  Value="10" 
                  Step="1" />
```

## Adding UpDown via C#

```csharp
// Create a new UpDown control
UpDown updown = new UpDown();
updown.Width = 100;
updown.Height = 25;
updown.Value = 10;
updown.Step = 1;

// Add to your container (Grid, StackPanel, etc.)
grid.Children.Add(updown);
```

## Basic Properties

**Value Property** - Set the initial numeric value: `Value="100"` in XAML or `updown.Value = 100;` in C#.

**Step Property** - Define the increment/decrement interval when spin buttons are clicked: `Step="5"` in XAML or `updown.Step = 5;` in C#.

## Theme Application

Apply built-in themes using `SfSkinManager` in XAML (`syncfusion:SfSkinManager.VisualStyle="MaterialLight"` on Window) or C# (`SfSkinManager.SetVisualStyle(this, VisualStyles.MaterialLight)`).

**Available themes:** MaterialLight, MaterialDark, Office2019Colorful, Office2019Black, Office2019HighContrast, FluentLight, FluentDark.

**Custom themes:** Launch Theme Studio from Visual Studio (Extensions menu), customize colors/fonts/styles for UpDown, export as XAML, and merge into application resources.

## Complete Getting Started Example

```xaml
<Window x:Class="UpDownExample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        syncfusion:SfSkinManager.VisualStyle="MaterialLight"
        Title="UpDown Example" Height="200" Width="400">
    <StackPanel Margin="20" VerticalAlignment="Center">
        <TextBlock Text="Enter a Value:" FontSize="14" Margin="0,0,0,10"/>
        
        <syncfusion:UpDown Name="upDown" 
                          Height="30" 
                          Width="150"
                          Value="50"
                          Step="10"
                          MinValue="0"
                          MaxValue="100"
                          VerticalAlignment="Center"/>
        
        <TextBlock x:Name="valueDisplay" 
                  Text="Current Value: 50" 
                  FontSize="12" 
                  Margin="0,20,0,0"/>
    </StackPanel>
</Window>
```

**Code-behind:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Subscribe to value change events
        upDown.ValueChanged += (d, e) => 
        {
            valueDisplay.Text = $"Current Value: {e.NewValue}";
        };
    }
}
```

## Common Issues

**Assembly not found:** Ensure `Syncfusion.Shared.WPF` is installed via NuGet and referenced in the project.

**Schema namespace error:** Verify the xmlns:syncfusion attribute points to `http://schemas.syncfusion.com/wpf`.

**Controls not visible in Toolbox:** Restart Visual Studio after NuGet package installation to refresh the Toolbox.

---

**Next:** Read [value-and-restrictions.md](value-and-restrictions.md) to manage values and implement constraints.
