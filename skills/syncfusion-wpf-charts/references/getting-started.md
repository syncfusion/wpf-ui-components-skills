# Getting Started with WPF Charts

This guide covers the essential steps to add and configure Syncfusion WPF Charts (SfChart) in your application.

## Installation

### NuGet Package

Install the Syncfusion WPF Chart package:

### Assembly References

Add reference to:
- `Syncfusion.SfChart.WPF.dll`

## Basic Chart Setup

### Step 1: Add Namespace

In your XAML window, add the Syncfusion namespace:

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
```

### Step 2: Initialize Chart with Axes

Every chart requires two axes - Primary (X-axis) and Secondary (Y-axis):

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:CategoryAxis Header="Category"/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis Header="Values"/>
    </syncfusion:SfChart.SecondaryAxis>
</syncfusion:SfChart>
```

**Note:** If you don't specify axes, SfChart will generate default axes automatically based on your data.

### Step 3: Add a Series

Add a chart series to visualize data:

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:CategoryAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                             XBindingPath="Category"
                             YBindingPath="Value"/>
</syncfusion:SfChart>
```

## Data Binding Setup

### Create Data Model

```csharp
public class ChartData
{
    public string Category { get; set; }
    public double Value { get; set; }
}
```

### Create ViewModel

```csharp
using System.Collections.ObjectModel;

public class ChartViewModel
{
    public ObservableCollection<ChartData> Data { get; set; }
    
    public ChartViewModel()
    {
        Data = new ObservableCollection<ChartData>
        {
            new ChartData { Category = "Product A", Value = 50 },
            new ChartData { Category = "Product B", Value = 70 },
            new ChartData { Category = "Product C", Value = 45 },
            new ChartData { Category = "Product D", Value = 90 }
        };
    }
}
```

### Set DataContext

In code-behind:

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new ChartViewModel();
}
```

Or in XAML:

```xml
<Window.DataContext>
    <local:ChartViewModel/>
</Window.DataContext>
```

## Complete Example

**XAML:**

```xml
<Window x:Class="ChartApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF"
        xmlns:local="clr-namespace:ChartApp"
        Title="Sales Chart" Height="450" Width="800">
    
    <Window.DataContext>
        <local:ChartViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:SfChart Header="Monthly Sales">
            <syncfusion:SfChart.PrimaryAxis>
                <syncfusion:CategoryAxis Header="Month"/>
            </syncfusion:SfChart.PrimaryAxis>
            
            <syncfusion:SfChart.SecondaryAxis>
                <syncfusion:NumericalAxis Header="Sales ($)"/>
            </syncfusion:SfChart.SecondaryAxis>
            
            <syncfusion:ColumnSeries ItemsSource="{Binding SalesData}"
                                     XBindingPath="Month"
                                     YBindingPath="Amount"
                                     Label="Sales"/>
        </syncfusion:SfChart>
    </Grid>
</Window>
```

**Code-behind:**

```csharp
using System.Collections.ObjectModel;
using System.Windows;

namespace ChartApp
{
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Amount { get; set; }
    }
    
    public class ChartViewModel
    {
        public ObservableCollection<SalesInfo> SalesData { get; set; }
        
        public ChartViewModel()
        {
            SalesData = new ObservableCollection<SalesInfo>
            {
                new SalesInfo { Month = "Jan", Amount = 42000 },
                new SalesInfo { Month = "Feb", Amount = 47000 },
                new SalesInfo { Month = "Mar", Amount = 52000 },
                new SalesInfo { Month = "Apr", Amount = 48000 },
                new SalesInfo { Month = "May", Amount = 61000 },
                new SalesInfo { Month = "Jun", Amount = 67000 }
            };
        }
    }
    
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

## Adding Chart Header

```xml
<syncfusion:SfChart Header="Monthly Sales Report">
```

## Adding Data Labels

```xml
<syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                         XBindingPath="Category"
                         YBindingPath="Value">
    <syncfusion:ColumnSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="True"/>
    </syncfusion:ColumnSeries.AdornmentsInfo>
</syncfusion:ColumnSeries>
```

## Adding Legend

```xml
<syncfusion:SfChart>
    <!-- Axes and Series here -->
    
    <syncfusion:SfChart.Legend>
        <syncfusion:ChartLegend/>
    </syncfusion:SfChart.Legend>
</syncfusion:SfChart>
```

## Key Concepts

**ItemsSource:** The data collection to bind to the series  
**XBindingPath:** Property name for X-axis values  
**YBindingPath:** Property name for Y-axis values  
**Label:** Series name displayed in the legend  

## Next Steps

- Explore different series types in `series-types.md`
- Learn about axis configuration in `axes-configuration.md`
- Add interactive features like tooltips and zooming
- Customize chart appearance and styling
