# Getting Started with Sparklines

This guide covers the essential steps to set up and create your first Syncfusion WPF Sparkline control, including installation, assembly references, data binding, and basic configuration.


## Assembly References

The sparkline controls require the following assembly:
- **Syncfusion.SfChart.WPF** - Contains all sparkline control types

This assembly includes:
- SfLineSparkline
- SfColumnSparkline
- SfAreaSparkline
- SfWinLossSparkline

## Namespace Imports

### XAML Namespace

Add the Syncfusion namespace to your XAML file:

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF"
        ...>
```

Or use a shorter alias:

```xml
<Window xmlns:sf="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF"
        ...>
```

### C# Namespace

Import the namespace in your C# code:

```csharp
using Syncfusion.UI.Xaml.Charts;
```

## Creating Your First Sparkline

### Step 1: Initialize the Sparkline

The simplest sparkline creation in XAML:

```xml
<syncfusion:SfLineSparkline>
</syncfusion:SfLineSparkline>
```

Or in C#:

```csharp
SfLineSparkline sparkline = new SfLineSparkline();
```

**Note:** This creates an empty sparkline. You need to bind data for it to display anything.

### Step 2: Create a Data Source

Sparklines require a data collection to visualize. Create a simple data model:

```csharp
public class UserProfile
{
    public DateTime TimeStamp { get; set; }
    public double NoOfUsers { get; set; }
}
```

### Step 3: Create a ViewModel

Set up an ObservableCollection with sample data:

```csharp
using System;
using System.Collections.ObjectModel;

public class UsersViewModel
{
    public ObservableCollection<UserProfile> UsersList { get; set; }
    
    public UsersViewModel()
    {
        UsersList = new ObservableCollection<UserProfile>();
        DateTime date = DateTime.Today;
        
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(0.5), NoOfUsers = 3000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1.0), NoOfUsers = 5000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1.5), NoOfUsers = -3000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2.0), NoOfUsers = -4000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2.5), NoOfUsers = 2000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(3.0), NoOfUsers = 3000 });
    }
}
```

**Important:** Syncfusion sparklines support:
- `ObservableCollection<T>` (recommended for dynamic updates)
- Any collection implementing `IEnumerable`
- Simple collections of double values: `List<double>`, `double[]`, etc.

### Step 4: Bind Data to Sparkline

#### XAML Data Binding

```xml
<Window ...>
    
    <Window.DataContext>
        <local:UsersViewModel/>
    </Window.DataContext>
    
    <syncfusion:SfLineSparkline 
        ItemsSource="{Binding UsersList}" 
        YBindingPath="NoOfUsers">
    </syncfusion:SfLineSparkline>
    
</Window>
```

#### C# Data Binding

```csharp
SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new UsersViewModel().UsersList,
    YBindingPath = "NoOfUsers"
};

// Add sparkline to your container
this.Content = sparkline;
// or: myGrid.Children.Add(sparkline);
```

### Understanding Data Binding Properties

**ItemsSource:**
- Binds the data collection to the sparkline
- Accepts any `IEnumerable` collection
- Can be bound to ObservableCollection for automatic updates

**YBindingPath:**
- Specifies which property contains Y-axis values
- Required property for data display
- Must match the property name in your data model (e.g., "NoOfUsers")

**XBindingPath (Optional):**
- Specifies which property contains X-axis values
- If omitted, data points are positioned by index (0, 1, 2, ...)
- Useful for time-series or specific X-axis positioning

Example with XBindingPath:

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    XBindingPath="TimeStamp">
</syncfusion:SfLineSparkline>
```

## Complete Working Example

### XAML

```xml
<Window x:Class="SparklineDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:SparklineDemo"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF"
        Title="Sparkline Demo" Height="300" Width="400">
    
    <Window.DataContext>
        <local:UsersViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:SfLineSparkline 
            ItemsSource="{Binding UsersList}" 
            YBindingPath="NoOfUsers"
            Interior="#4a4a4a"
            BorderBrush="DarkGray"
            BorderThickness="1"
            Margin="20"/>
    </Grid>
    
</Window>
```

### C# Code-Behind

```csharp
using System;
using System.Collections.ObjectModel;
using System.Windows;
using Syncfusion.UI.Xaml.Charts;

namespace SparklineDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
    
    public class UserProfile
    {
        public DateTime TimeStamp { get; set; }
        public double NoOfUsers { get; set; }
    }
    
    public class UsersViewModel
    {
        public ObservableCollection<UserProfile> UsersList { get; set; }
        
        public UsersViewModel()
        {
            UsersList = new ObservableCollection<UserProfile>();
            DateTime date = DateTime.Today;
            
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(0.5), NoOfUsers = 3000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1.0), NoOfUsers = 5000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1.5), NoOfUsers = -3000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2.0), NoOfUsers = -4000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2.5), NoOfUsers = 2000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(3.0), NoOfUsers = 3000 });
        }
    }
}
```

## Simple Data Collections

For quick prototyping, you can use simple double collections:

```csharp
// Simple double array
double[] values = { 3000, 5000, -3000, -4000, 2000, 3000 };

SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = values,
    YBindingPath = "" // Leave empty for simple collections
};
```

Or a List:

```csharp
List<double> values = new List<double> { 3000, 5000, -3000, -4000, 2000, 3000 };

SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = values
};
```

**Note:** When using simple numeric collections, you don't need to specify YBindingPath.

## Next Steps

Now that you have a basic sparkline set up, explore:
- **[Sparkline Types](sparkline-types.md)** - Learn about Line, Column, Area, and WinLoss types
- **[Markers](markers.md)** - Add visual indicators for data points
- **[Track Ball](track-ball.md)** - Enable interactive data inspection
- **[Customization](segment-customization.md)** - Style segments and special points

## Troubleshooting

**Sparkline not displaying:**
- Verify ItemsSource is bound correctly
- Check YBindingPath matches your property name exactly (case-sensitive)
- Ensure data collection contains valid numeric values
- Confirm the sparkline has non-zero Height and Width

**Build errors:**
- Make sure Syncfusion.SfChart.WPF assembly is referenced
- Verify namespace imports are correct
- Check that assembly version matches your Syncfusion license

**Data not updating:**
- Use ObservableCollection instead of List for automatic updates
- Implement INotifyPropertyChanged if binding to object properties
