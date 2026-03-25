# Getting Started with SfMaskedEdit

This guide covers installation, setup, and basic configuration of the Syncfusion WPF MaskedTextBox (SfMaskedEdit) control.

## Assembly Deployment

### Required Assemblies

To use SfMaskedEdit, you need the following assemblies:
- **Syncfusion.SfInput.WPF** - Contains SfMaskedEdit control
- **Syncfusion.SfShared.WPF** - Shared utilities and dependencies

### Installation Methods

#### Method 1: NuGet Package Manager (Recommended)

```powershell
Install-Package Syncfusion.SfInput.WPF
```

The NuGet package automatically includes all required dependencies.

#### Method 2: Package Manager UI

1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.SfInput.WPF"
4. Click "Install"

#### Method 3: Syncfusion Control Panel

Install the complete Syncfusion WPF suite via the Control Panel installer, then add assembly references manually.

**Assembly Location:**  
`C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<.NET version>\`

### Verify Installation

After installation, verify the assemblies are referenced:
- Open Solution Explorer
- Expand "References" or "Dependencies"
- Confirm `Syncfusion.SfInput.WPF` and `Syncfusion.SfShared.WPF` are listed

## Adding SfMaskedEdit via Designer

If you have Syncfusion installed, you can drag SfMaskedEdit from the toolbox:

1. Open your XAML file in the designer
2. Open the Toolbox (View → Toolbox)
3. Locate "Syncfusion Controls for WPF"
4. Find "SfMaskedEdit" and drag it to your window/page
5. The required assemblies are automatically added

The designer adds the namespace and control:

```xml
<syncfusion:SfMaskedEdit x:Name="sfMaskedEdit" 
                         Width="200" 
                         Height="30"/>
```

## Adding SfMaskedEdit via XAML

### Step 1: Create a WPF Project

1. Open Visual Studio
2. Create a new WPF Application project
3. Target .NET Framework 4.6.2 or higher (or .NET 5/6/7/8)

### Step 2: Add Assembly References

Add the required NuGet package or assembly references as described above.

### Step 3: Import the Namespace

Add the Syncfusion namespace to your XAML window/page:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="YourNamespace.MainWindow"
        Title="MaskedEdit Sample" Height="450" Width="800">
```

**Namespace breakdown:**
- `xmlns:syncfusion` - Prefix for Syncfusion controls
- `http://schemas.syncfusion.com/wpf` - Syncfusion WPF schema URI

### Step 4: Declare the Control

Add SfMaskedEdit to your layout:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="SfMaskedEditSample.MainWindow"
        Title="SfMaskedEdit Sample" Height="350" Width="525">
    <Grid>
        <syncfusion:SfMaskedEdit x:Name="sfMaskedEdit" 
                                 Width="200" 
                                 Height="30"
                                 HorizontalAlignment="Center"
                                 VerticalAlignment="Center"/>
    </Grid>
</Window>
```

## Adding SfMaskedEdit via C#

### Step 1: Create WPF Project and Add References

Follow the same project creation and reference steps as above.

### Step 2: Import the Namespace in Code

Add the using directive in your code-behind or view model:

```csharp
using Syncfusion.Windows.Controls.Input;
```

### Step 3: Create and Configure the Control

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace SfMaskedEditSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();

            // Create an instance of SfMaskedEdit
            SfMaskedEdit sfMaskedEdit = new SfMaskedEdit();
            sfMaskedEdit.Width = 200;
            sfMaskedEdit.Height = 30;
            sfMaskedEdit.HorizontalAlignment = HorizontalAlignment.Center;
            sfMaskedEdit.VerticalAlignment = VerticalAlignment.Center;

            // Add to the window
            this.Content = sfMaskedEdit;
        }
    }
}
```

### Adding to Existing Layout

If you have an existing Grid or other layout:

```csharp
public MainWindow()
{
    InitializeComponent();

    // Create the control
    SfMaskedEdit sfMaskedEdit = new SfMaskedEdit
    {
        Name = "phoneInput",
        Width = 200,
        Height = 30,
        Margin = new Thickness(10)
    };

    // Add to existing Grid (assuming Grid is named "MainGrid")
    MainGrid.Children.Add(sfMaskedEdit);
}
```

## Basic Mask Configuration

### Setting a Simple Mask

```xml
<syncfusion:SfMaskedEdit Mask="-?\d+\.?\d*"
                         MaskType="RegEx"
                         PromptChar="_"
                         x:Name="numericInput"/>
```

```csharp
SfMaskedEdit numericInput = new SfMaskedEdit();
numericInput.MaskType = MaskType.RegEx;
numericInput.Mask = @"-?\d+\.?\d*";
numericInput.PromptChar = '_';
```

**This mask accepts:**
- Positive and negative numbers
- Whole numbers and decimals
- Example inputs: `123`, `-45`, `12.34`, `-98.76`

### Common Initial Configurations

#### Phone Number

```xml
<syncfusion:SfMaskedEdit 
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"
    PromptChar="_"/>
```

#### Email Address

```xml
<syncfusion:SfMaskedEdit 
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
    MaskType="RegEx"/>
```

#### Currency

```xml
<syncfusion:SfMaskedEdit 
    Mask="$ \d+\.\d{2}"
    MaskType="RegEx"
    Value="100.00"/>
```

## Setting Initial Values

You can set an initial value using the `Value` property:

```xml
<syncfusion:SfMaskedEdit 
    Value="4553456789"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
    MaskType="RegEx"
    x:Name="phoneNumber"/>
```

```csharp
SfMaskedEdit phoneNumber = new SfMaskedEdit();
phoneNumber.Value = "4553456789";
phoneNumber.MaskType = MaskType.RegEx;
phoneNumber.Mask = @"\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}";
```

The value is automatically formatted according to the mask:
- **Input:** `"4553456789"`
- **Displayed:** `(455) 345-6789`

## Handling ValueChanged Events

Subscribe to the `ValueChanged` event to respond when the user modifies the value:

### In XAML

```xml
<syncfusion:SfMaskedEdit ValueChanged="SfMaskedEdit_ValueChanged" 
                         Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
                         MaskType="RegEx"
                         x:Name="phoneInput"/>
```

### In Code-Behind

```csharp
public MainWindow()
{
    InitializeComponent();
    phoneInput.ValueChanged += PhoneInput_ValueChanged;
}

private void PhoneInput_ValueChanged(object sender, EventArgs e)
{
    var maskedEdit = sender as SfMaskedEdit;
    string currentValue = maskedEdit.Value;
    
    // Check if input is valid
    if (!maskedEdit.HasError)
    {
        MessageBox.Show($"Valid phone number: {currentValue}");
    }
    else
    {
        MessageBox.Show("Please enter a valid phone number");
    }
}
```

### When Does ValueChanged Fire?

The event fires based on the `ValidationMode` property:
- **KeyPress** (default): Fires on each valid keystroke
- **LostFocus**: Fires only when the control loses focus

```xml
<syncfusion:SfMaskedEdit 
    ValidationMode="LostFocus"
    ValueChanged="SfMaskedEdit_ValueChanged"/>
```

## Complete Getting Started Example

Here's a complete example combining all concepts:

### XAML

```xml
<Window x:Class="MaskedEditDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MaskedEdit Demo" Height="300" Width="400">
    <StackPanel Margin="20">
        <TextBlock Text="Enter Phone Number:" FontWeight="Bold" Margin="0,0,0,5"/>
        <syncfusion:SfMaskedEdit 
            x:Name="phoneInput"
            Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}" 
            MaskType="RegEx"
            PromptChar="_"
            Watermark="(___) ___-____"
            Width="200"
            Height="30"
            HorizontalAlignment="Left"
            ValueChanged="PhoneInput_ValueChanged"/>
        
        <TextBlock Text="Enter Email:" FontWeight="Bold" Margin="0,20,0,5"/>
        <syncfusion:SfMaskedEdit 
            x:Name="emailInput"
            Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
            MaskType="RegEx"
            Watermark="example@domain.com"
            Width="250"
            Height="30"
            HorizontalAlignment="Left"
            ValidationMode="LostFocus"/>
        
        <Button Content="Submit" Width="100" Margin="0,20,0,0" 
                HorizontalAlignment="Left" Click="Submit_Click"/>
    </StackPanel>
</Window>
```

### Code-Behind

```csharp
using System;
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace MaskedEditDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void PhoneInput_ValueChanged(object sender, EventArgs e)
        {
            var maskedEdit = sender as SfMaskedEdit;
            
            // Update UI or validate as user types
            if (!string.IsNullOrWhiteSpace(maskedEdit.Value) && !maskedEdit.HasError)
            {
                // Valid phone number entered
                Console.WriteLine($"Phone: {maskedEdit.Value}");
            }
        }

        private void Submit_Click(object sender, RoutedEventArgs e)
        {
            // Validate both fields
            if (!phoneInput.HasError && !emailInput.HasError)
            {
                string phone = phoneInput.Value;
                string email = emailInput.Value;
                
                MessageBox.Show($"Phone: {phone}\nEmail: {email}", 
                    "Form Submitted", MessageBoxButton.OK, MessageBoxImage.Information);
            }
            else
            {
                MessageBox.Show("Please fill all fields correctly.", 
                    "Validation Error", MessageBoxButton.OK, MessageBoxImage.Warning);
            }
        }
    }
}
```

## License Configuration

Syncfusion controls require a license key. Register it in your App.xaml.cs:

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Register Syncfusion license
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        base.OnStartup(e);
    }
}
```

**Get your license key:**
- **Free Community License:** https://www.syncfusion.com/products/communitylicense
- **Trial License:** Provided during trial download
- **Commercial License:** From your license dashboard

## Next Steps

Now that you have SfMaskedEdit set up, explore:
- **Mask Patterns:** Learn to create custom masks for any format
- **Value Management:** Control how values are formatted
- **Appearance:** Customize colors, borders, and themes
- **Validation:** Implement KeyPress or LostFocus validation
