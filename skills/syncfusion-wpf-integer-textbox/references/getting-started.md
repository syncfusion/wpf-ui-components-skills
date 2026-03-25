# Getting Started with IntegerTextBox

This guide covers the initial setup and basic implementation of the Syncfusion WPF IntegerTextBox control, including installation, adding the control to your application, and basic configuration.

## Assembly Deployment

The IntegerTextBox control requires the following assembly reference:

**Required Assembly:**
- `Syncfusion.Shared.WPF`

### Installation via NuGet

Install the IntegerTextBox control using NuGet Package Manager:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Shared.WPF
```

**Package Manager UI:**
1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Shared.WPF"
4. Click "Install"

### Control Dependencies

For more details about control dependencies and assembly requirements, refer to the [Syncfusion WPF control dependencies documentation](https://help.syncfusion.com/wpf/control-dependencies#integertextbox).

## Adding IntegerTextBox via Designer

You can add the IntegerTextBox control using Visual Studio's WPF designer:

1. Open the WPF designer view
2. Locate **Syncfusion Controls** in the Toolbox
3. Drag **IntegerTextBox** from the Toolbox to your designer surface
4. The `Syncfusion.Shared.WPF` assembly will be automatically added to your project references

The control will appear in your XAML with the necessary namespace:

```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100" 
                           Height="25"/>
```

## Adding IntegerTextBox via XAML

To add the IntegerTextBox control manually in XAML:

### Step 1: Create WPF Project

Create a new WPF project in Visual Studio (.NET Framework or .NET Core/.NET 5+).

### Step 2: Add Assembly Reference

Add the `Syncfusion.Shared.WPF` assembly reference to your project:
- Right-click References → Add Reference → Browse
- Navigate to Syncfusion installation folder
- Select `Syncfusion.Shared.WPF.dll`

Or use NuGet as described above.

### Step 3: Import Namespace and Declare Control

Import the Syncfusion WPF schema and declare the IntegerTextBox in your XAML:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="IntegerTextBoxSample.MainWindow"
        Title="IntegerTextBox Sample" Height="350" Width="525">
    <Grid>
        <!-- Adding IntegerTextBox control -->
        <syncfusion:IntegerTextBox x:Name="integerTextBox" 
                                   Width="100" 
                                   Height="25" 
                                   VerticalAlignment="Center" 
                                   HorizontalAlignment="Center"/>
    </Grid>
</Window>
```

**Namespace Declaration:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

This namespace declaration allows you to use all Syncfusion WPF controls with the `syncfusion:` prefix.

## Adding IntegerTextBox via C#

To add the IntegerTextBox control programmatically in C#:

### Step 1: Create WPF Application

Create a new WPF application in Visual Studio.

### Step 2: Add Assembly Reference

Add the `Syncfusion.Shared.WPF` assembly reference as described in the XAML section.

### Step 3: Include Namespace

Add the required namespace in your code-behind file:

```csharp
using Syncfusion.Windows.Shared;
```

### Step 4: Create and Configure IntegerTextBox

Create an instance of IntegerTextBox and add it to your window:

```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

namespace IntegerTextBoxSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create IntegerTextBox instance
            IntegerTextBox integerTextBox = new IntegerTextBox();
            
            // Set dimensions
            integerTextBox.Width = 100;
            integerTextBox.Height = 25;
            
            // Set alignment
            integerTextBox.VerticalAlignment = VerticalAlignment.Center;
            integerTextBox.HorizontalAlignment = HorizontalAlignment.Center;
            
            // Add to window content
            this.Content = integerTextBox;
        }
    }
}
```

**For Adding to Layout Panels:**

```csharp
// Create StackPanel
StackPanel stackPanel = new StackPanel();

// Create multiple IntegerTextBox controls
IntegerTextBox integerTextBox1 = new IntegerTextBox
{
    Width = 150,
    Height = 30,
    Margin = new Thickness(10)
};

IntegerTextBox integerTextBox2 = new IntegerTextBox
{
    Width = 150,
    Height = 30,
    Margin = new Thickness(10)
};

// Add to panel
stackPanel.Children.Add(integerTextBox1);
stackPanel.Children.Add(integerTextBox2);

// Set as window content
this.Content = stackPanel;
```

## Basic Configuration

### Setting Initial Value

**⚠️ Important:** Do NOT use the `Text` property to set the value. Always use the `Value` property for IntegerTextBox.

**XAML:**
```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100" 
                           Height="25"
                           Value="100"/>
```

**C#:**
```csharp
integerTextBox.Value = 100;
```

### Setting Min and Max Values

Restrict the input value within a specific range:

**XAML:**
```xml
<syncfusion:IntegerTextBox x:Name="integerTextBox" 
                           Width="100" 
                           Height="25"
                           Value="50"
                           MinValue="0"
                           MaxValue="100"/>
```

**C#:**
```csharp
integerTextBox.Value = 50;
integerTextBox.MinValue = 0;
integerTextBox.MaxValue = 100;
```

The control will prevent users from entering values outside this range based on validation settings.

## Data Binding Setup

IntegerTextBox supports full data binding with the MVVM pattern.

### ViewModel Implementation

Create a ViewModel with INotifyPropertyChanged:

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
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Binding in XAML

Bind the Value property to your ViewModel:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:IntegerTextBoxSample"
        x:Class="IntegerTextBoxSample.MainWindow">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <StackPanel Margin="20">
        <syncfusion:IntegerTextBox x:Name="integerTextBox1" 
                                   Width="150" 
                                   Height="30"
                                   Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}"/>
        
        <!-- Second control bound to same property for synchronization -->
        <syncfusion:IntegerTextBox x:Name="integerTextBox2" 
                                   Width="150" 
                                   Height="30"
                                   Margin="0,10,0,0"
                                   Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}"/>
        
        <TextBlock Text="{Binding MyValue, StringFormat='Current Value: {0}'}" 
                   Margin="0,10,0,0"
                   FontSize="14"/>
    </StackPanel>
</Window>
```

**Binding Modes:**
- `Mode=TwoWay` - Default, allows bidirectional data flow
- `UpdateSourceTrigger=PropertyChanged` - Updates source immediately on value change

### Two-Way Binding Example

Both IntegerTextBox controls will stay synchronized through the shared ViewModel property:

```xml
<StackPanel>
    <TextBlock Text="Value 1:"/>
    <syncfusion:IntegerTextBox Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}" 
                               Width="100" 
                               Height="25"/>
    
    <TextBlock Text="Value 2:" Margin="0,10,0,0"/>
    <syncfusion:IntegerTextBox Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}" 
                               Width="100" 
                               Height="25"/>
</StackPanel>
```

When you change the value in either control, both controls update automatically.

## Basic Formatting

Enable number group separators for better readability:

**XAML:**
```xml
<syncfusion:IntegerTextBox Value="123456789"
                           GroupSeperatorEnabled="True"
                           Width="150"
                           Height="30"/>
```

**C#:**
```csharp
integerTextBox.Value = 123456789;
integerTextBox.GroupSeperatorEnabled = true;
```

The control will display: `123,456,789` (based on current culture).

## Complete Getting Started Example

Here's a complete example combining all basic features:

```xml
<Window x:Class="IntegerTextBoxSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:IntegerTextBoxSample"
        Title="IntegerTextBox Getting Started" Height="350" Width="500">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <StackPanel Margin="20">
        <TextBlock Text="Enter Quantity (1-1000):" 
                   FontWeight="Bold" 
                   Margin="0,0,0,5"/>
        
        <syncfusion:IntegerTextBox Value="{Binding Quantity, UpdateSourceTrigger=PropertyChanged}"
                                   MinValue="1"
                                   MaxValue="1000"
                                   Width="200"
                                   Height="30"
                                   HorizontalAlignment="Left"/>
        
        <TextBlock Text="{Binding Quantity, StringFormat='Selected Quantity: {0}'}" 
                   Margin="0,15,0,0"
                   FontSize="14"/>
    </StackPanel>
</Window>
```

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows;

namespace IntegerTextBoxSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
    
    public class ViewModel : INotifyPropertyChanged
    {
        private int quantity = 10;
        
        public int Quantity
        {
            get => quantity;
            set
            {
                if (quantity != value)
                {
                    quantity = value;
                    OnPropertyChanged();
                }
            }
        }
        
        public event PropertyChangedEventHandler PropertyChanged;
        
        protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

## Next Steps

Now that you have the IntegerTextBox set up, explore advanced features:

- **Value Management** - Learn about paste modes, spin buttons, null values, and watermarks
- **Validation and Restrictions** - Configure validation modes and input restrictions
- **Formatting and Culture** - Customize number formatting and globalization support
- **Appearance and Customization** - Style the control with colors, themes, and visual effects
- **Step Interval and Scrolling** - Enable interactive value adjustment with keyboard and mouse
- **Range Adorner** - Add visual progress indication

Refer to the respective reference files in the `references/` directory for detailed documentation on each feature.
