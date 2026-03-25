# Getting Started with WPF SfChart3D

This guide covers the complete setup process for adding and configuring the Syncfusion WPF SfChart3D control in your Windows Presentation Foundation application, from installation to creating your first interactive 3D chart.

## Installation and Assembly References

### Adding Chart Reference

The SfChart3D control is part of the Syncfusion WPF Chart package. Add the required assembly reference to your project:

**Required Assembly:**
- `Syncfusion.SfChart.WPF.dll`

## XAML Namespace Configuration

Add the Syncfusion Charts namespace to your XAML file:

**XAML:**
```xml
<Window x:Class="YourNamespace.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:chart="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    
    <!-- Your content here -->
    
</Window>
```

**C# Code-Behind:**
```csharp
using Syncfusion.UI.Xaml.Charts;
```

## Creating Your First 3D Chart

Follow these steps to create a basic 3D Column chart with data binding.

### Step 1: Define the Data Model

Create a class to represent your data points:

```csharp
public class UserProfile
{
    public DateTime TimeStamp { get; set; }
    public double NoOfUsers { get; set; }
}
```

### Step 2: Create the ViewModel

Initialize your data collection with sample data:

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
        
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(0.5), NoOfUsers = 1000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1), NoOfUsers = 5000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1.5), NoOfUsers = 3000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2), NoOfUsers = 4000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2.5), NoOfUsers = 2000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(3), NoOfUsers = 1000 });
    }
}
```

### Step 3: Set DataContext

Bind the ViewModel to your Window's DataContext:

**XAML:**
```xml
<Window.DataContext>
    <local:UsersViewModel/>
</Window.DataContext>
```

**C# Code-Behind:**
```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new UsersViewModel();
}
```

### Step 4: Initialize SfChart3D with Axes

Create an empty chart with PrimaryAxis (X-axis) and SecondaryAxis (Y-axis):

**XAML:**
```xml
<chart:SfChart3D Width="500" Height="500">
    
    <!-- Primary Axis (X-Axis) -->
    <chart:SfChart3D.PrimaryAxis>
        <chart:DateTimeAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <!-- Secondary Axis (Y-Axis) -->
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    Width = 500,
    Height = 500,
    PrimaryAxis = new DateTimeAxis3D(),
    SecondaryAxis = new NumericalAxis3D()
};

this.Content = chart3D;
```

### Step 5: Add Series and Bind Data

Add a 3D Column series and bind it to your data:

**XAML:**
```xml
<chart:SfChart3D Width="500" Height="500">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:DateTimeAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <!-- 3D Column Series -->
    <chart:ColumnSeries3D 
        ItemsSource="{Binding UsersList}" 
        XBindingPath="TimeStamp" 
        YBindingPath="NoOfUsers">
    </chart:ColumnSeries3D>
    
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    Width = 500,
    Height = 500,
    PrimaryAxis = new DateTimeAxis3D(),
    SecondaryAxis = new NumericalAxis3D()
};

ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = new UsersViewModel().UsersList,
    XBindingPath = "TimeStamp",
    YBindingPath = "NoOfUsers"
};

chart3D.Series.Add(series);
this.Content = chart3D;
```

**Key Properties:**
- **ItemsSource:** Binds to your data collection
- **XBindingPath:** Property name for X values (e.g., "TimeStamp")
- **YBindingPath:** Property name for Y values (e.g., "NoOfUsers")

## Adding Chart Title

Set a descriptive title for your chart:

**XAML:**
```xml
<chart:SfChart3D Header="User Activity Chart" Width="500" Height="500">
    <!-- Axes and series configuration -->
</chart:SfChart3D>
```

**C#:**
```csharp
chart3D.Header = "User Activity Chart";
```

## Enabling Data Labels

Display values on each data point using adornments:

**XAML:**
```xml
<chart:ColumnSeries3D 
    ItemsSource="{Binding UsersList}" 
    XBindingPath="TimeStamp" 
    YBindingPath="NoOfUsers">
    
    <!-- Data Labels Configuration -->
    <chart:ColumnSeries3D.AdornmentsInfo>
        <chart:ChartAdornmentInfo3D ShowLabel="True"/>
    </chart:ColumnSeries3D.AdornmentsInfo>
    
</chart:ColumnSeries3D>
```

**C#:**
```csharp
series.AdornmentsInfo = new ChartAdornmentInfo3D() 
{ 
    ShowLabel = true 
};
```

## Adding Legend

Enable legend to identify series:

**XAML:**
```xml
<chart:SfChart3D Header="User Activity Chart" Width="500" Height="500">
    
    <!-- Legend Configuration -->
    <chart:SfChart3D.Legend>
        <chart:ChartLegend/>
    </chart:SfChart3D.Legend>
    
    <!-- Axes and series -->
    
</chart:SfChart3D>
```

**C#:**
```csharp
chart3D.Legend = new ChartLegend();
```

Add a label to your series for legend display:

**XAML:**
```xml
<chart:ColumnSeries3D 
    Label="Active Users"
    ItemsSource="{Binding UsersList}" 
    XBindingPath="TimeStamp" 
    YBindingPath="NoOfUsers">
</chart:ColumnSeries3D>
```

**C#:**
```csharp
series.Label = "Active Users";
```

## Enabling Tooltips

Show detailed information on data point interaction:

**XAML:**
```xml
<chart:ColumnSeries3D 
    Label="Active Users"
    ShowTooltip="True"
    ItemsSource="{Binding UsersList}" 
    XBindingPath="TimeStamp" 
    YBindingPath="NoOfUsers">
</chart:ColumnSeries3D>
```

**C#:**
```csharp
series.ShowTooltip = true;
```

## Complete Example

Here's the complete XAML for a fully configured 3D chart:

```xml
<Window x:Class="GettingStarted_3DCharts.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:chart="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF"
        xmlns:local="clr-namespace:GettingStarted_3DCharts"
        Title="3D Chart Demo" Height="600" Width="800">
    
    <Window.DataContext>
        <local:UsersViewModel/>
    </Window.DataContext>
    
    <Grid>
        <chart:SfChart3D Header="User Activity Chart" Width="500" Height="500">
            
            <!-- Primary Axis (X-Axis) -->
            <chart:SfChart3D.PrimaryAxis>
                <chart:DateTimeAxis3D/>
            </chart:SfChart3D.PrimaryAxis>
            
            <!-- Secondary Axis (Y-Axis) -->
            <chart:SfChart3D.SecondaryAxis>
                <chart:NumericalAxis3D/>
            </chart:SfChart3D.SecondaryAxis>
            
            <!-- Legend -->
            <chart:SfChart3D.Legend>
                <chart:ChartLegend/>
            </chart:SfChart3D.Legend>
            
            <!-- 3D Column Series -->
            <chart:ColumnSeries3D 
                Label="Active Users"
                ShowTooltip="True"
                ItemsSource="{Binding UsersList}" 
                XBindingPath="TimeStamp" 
                YBindingPath="NoOfUsers">
                
                <!-- Data Labels -->
                <chart:ColumnSeries3D.AdornmentsInfo>
                    <chart:ChartAdornmentInfo3D ShowLabel="True"/>
                </chart:ColumnSeries3D.AdornmentsInfo>
                
            </chart:ColumnSeries3D>
            
        </chart:SfChart3D>
    </Grid>
</Window>
```

And the complete C# code:

```csharp
using System;
using System.Collections.ObjectModel;
using System.Windows;
using Syncfusion.UI.Xaml.Charts;

namespace GettingStarted_3DCharts
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Alternative: Create chart programmatically
            // CreateChart();
        }
        
        private void CreateChart()
        {
            SfChart3D chart3D = new SfChart3D()
            {
                Header = "User Activity Chart",
                Width = 500,
                Height = 500,
                PrimaryAxis = new DateTimeAxis3D(),
                SecondaryAxis = new NumericalAxis3D(),
                Legend = new ChartLegend()
            };
            
            ColumnSeries3D series = new ColumnSeries3D()
            {
                ItemsSource = new UsersViewModel().UsersList,
                XBindingPath = "TimeStamp",
                YBindingPath = "NoOfUsers",
                Label = "Active Users",
                ShowTooltip = true,
                AdornmentsInfo = new ChartAdornmentInfo3D() { ShowLabel = true }
            };
            
            chart3D.Series.Add(series);
            this.Content = chart3D;
        }
    }
    
    // Data Model
    public class UserProfile
    {
        public DateTime TimeStamp { get; set; }
        public double NoOfUsers { get; set; }
    }
    
    // ViewModel
    public class UsersViewModel
    {
        public ObservableCollection<UserProfile> UsersList { get; set; }
        
        public UsersViewModel()
        {
            UsersList = new ObservableCollection<UserProfile>();
            DateTime date = DateTime.Today;
            
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(0.5), NoOfUsers = 1000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1), NoOfUsers = 5000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1.5), NoOfUsers = 3000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2), NoOfUsers = 4000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2.5), NoOfUsers = 2000 });
            UsersList.Add(new UserProfile { TimeStamp = date.AddHours(3), NoOfUsers = 1000 });
        }
    }
}
```

## Next Steps

Now that you have a basic 3D chart working, explore:
- **Series Types:** Try different chart types (Bar, Line, Pie, Area, etc.)
- **Axes Configuration:** Customize axis types, ranges, and labels
- **Data Adornments:** Advanced label positioning and marker customization
- **Appearance:** Apply color palettes and custom styling
- **Interactive Features:** Enable rotation, zooming, and selection

## Troubleshooting

**Chart not displaying:**
- Verify `Syncfusion.SfChart.WPF.dll` is referenced
- Check XAML namespace is correct: `clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF`
- Ensure ItemsSource contains data
- Verify XBindingPath and YBindingPath match property names exactly (case-sensitive)

**Data not showing:**
- Check DataContext is set correctly
- Verify data collection implements INotifyCollectionChanged (use ObservableCollection)
- Use binding diagnostics in Output window
- Ensure axis types match data types (e.g., DateTimeAxis3D for DateTime values)
