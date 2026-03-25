# Getting Started with Syncfusion WPF SmithChart

## Table of Contents
- [Overview](#overview)
- [Installation and Assembly Reference](#installation-and-assembly-reference)
- [Creating SmithChart from XAML](#creating-smithchart-from-xaml)
- [Creating SmithChart from Code-Behind](#creating-smithchart-from-code-behind)
- [Data Model Setup](#data-model-setup)
- [Theme Support](#theme-support)
- [Complete Examples](#complete-examples)

## Overview

This guide walks through the complete process of setting up and implementing a Syncfusion WPF SmithChart (SfSmithChart) component. The SmithChart is used to visualize impedance and admittance data for transmission lines in high-frequency circuit applications.

**What you'll learn:**
- How to add assembly references
- Initialize the SmithChart control
- Create data models for transmission line data
- Add header, axes, series, and legend
- Apply themes to the chart

## Installation and Assembly Reference

### Step 1: Add Assembly Reference via Reference Manager

Add reference to the [Syncfusion.SfSmithChart.WPF](https://www.nuget.org/packages/Syncfusion.SfSmithChart.WPF) NuGet package.

**Package Manager Console:**
```
Install-Package Syncfusion.SfSmithChart.WPF
```

**NuGet Package Manager UI:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.SfSmithChart.WPF"
3. Click Install

### Step 2: Add Namespace Declaration

Add the Syncfusion namespace to your XAML file:

```xml
xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.SmithChart;assembly=Syncfusion.SfSmithChart.WPF"
```

For code-behind (C#), add the using statement:

```csharp
using Syncfusion.UI.Xaml.SmithChart;
```

### Step 3: Add SmithChart from Toolbox (Optional)

You can also drag and drop the SfSmithChart control from the Visual Studio Toolbox. This automatically:
- Adds the assembly reference
- Generates the xmlns namespace code
- Places the control in your XAML

## Creating SmithChart from XAML

### Basic SmithChart Initialization

The simplest SmithChart with default axes:

```xml
<syncfusion:SfSmithChart>
</syncfusion:SfSmithChart>
```

This creates a SmithChart with default resistance (horizontal) and reactance (radial) axes.

### Adding Header

Add a descriptive header to identify the chart's purpose:

```xml
<syncfusion:SfSmithChart Header="Impedance Transmission" Height="400" Width="500">
</syncfusion:SfSmithChart>
```

### Configuring Axes

Customize the resistance and reactance axes:

```xml
<syncfusion:SfSmithChart Header="Impedance Transmission" Height="400" Width="500">
    <!-- Horizontal Axis (Resistance) -->
    <syncfusion:SfSmithChart.HorizontalAxis>
        <syncfusion:HorizontalAxis FontSize="11" FontFamily="Segoe UI"/>
    </syncfusion:SfSmithChart.HorizontalAxis>
    
    <!-- Radial Axis (Reactance) -->
    <syncfusion:SfSmithChart.RadialAxis>
        <syncfusion:RadialAxis FontSize="11" FontFamily="Segoe UI"/>
    </syncfusion:SfSmithChart.RadialAxis>
</syncfusion:SfSmithChart>
```

### Adding Series

Add a LineSeries to visualize transmission data:

```xml
<syncfusion:SfSmithChart Header="Impedance Transmission" Height="400" Width="500">
    <!-- LineSeries with data binding -->
    <syncfusion:LineSeries ResistancePath="Resistance" 
                           ReactancePath="Reactance" 
                           ItemsSource="{Binding Data}" 
                           Label="TransmissionLine">
    </syncfusion:LineSeries>
    
    <!-- Axes configuration here -->
</syncfusion:SfSmithChart>
```

**Key Properties:**
- **ItemsSource**: Binds to your data collection
- **ResistancePath**: Property name for resistance values (X-axis)
- **ReactancePath**: Property name for reactance values (Y-axis)
- **Label**: Series name displayed in the legend

### Adding Legend

Include a legend to display series information:

```xml
<syncfusion:SfSmithChart Header="Impedance Transmission" Height="400" Width="500">
    <!-- Series and axes here -->
    
    <!-- Legend -->
    <syncfusion:SfSmithChart.Legend>
        <syncfusion:SmithChartLegend/>
    </syncfusion:SfSmithChart.Legend>
</syncfusion:SfSmithChart>
```

### Complete XAML Example

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.SmithChart;assembly=Syncfusion.SfSmithChart.WPF"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Grid>
        <syncfusion:SfSmithChart Header="Impedance Transmission" Height="400" Width="500">
            <!-- LineSeries -->
            <syncfusion:LineSeries ResistancePath="Resistance" 
                                   ReactancePath="Reactance" 
                                   ItemsSource="{Binding Data}" 
                                   Label="TransmissionLine">
            </syncfusion:LineSeries>
            
            <!-- Horizontal Axis (Resistance) -->
            <syncfusion:SfSmithChart.HorizontalAxis>
                <syncfusion:HorizontalAxis FontSize="11"/>
            </syncfusion:SfSmithChart.HorizontalAxis>
            
            <!-- Radial Axis (Reactance) -->
            <syncfusion:SfSmithChart.RadialAxis>
                <syncfusion:RadialAxis FontSize="11"/>
            </syncfusion:SfSmithChart.RadialAxis>
            
            <!-- Legend -->
            <syncfusion:SfSmithChart.Legend>
                <syncfusion:SmithChartLegend/>
            </syncfusion:SfSmithChart.Legend>
        </syncfusion:SfSmithChart>
    </Grid>
</Window>
```

## Creating SmithChart from Code-Behind

### Basic Initialization

Create and configure SmithChart programmatically in C#:

```csharp
using Syncfusion.UI.Xaml.SmithChart;
using System.Windows.Controls;

// Create SmithChart instance
SfSmithChart chart = new SfSmithChart();
chart.Header = "Impedance Transmission";
chart.Height = 400;
chart.Width = 500;
```

### Adding Axes

Configure horizontal and radial axes:

```csharp
// Horizontal Axis (Resistance)
chart.HorizontalAxis = new HorizontalAxis();
chart.HorizontalAxis.FontSize = 11;
chart.HorizontalAxis.FontFamily = new FontFamily("Segoe UI");

// Radial Axis (Reactance)
chart.RadialAxis = new RadialAxis();
chart.RadialAxis.FontSize = 11;
chart.RadialAxis.FontFamily = new FontFamily("Segoe UI");
```

### Adding Series

Create and add a LineSeries:

```csharp
LineSeries series = new LineSeries();
series.ItemsSource = Data; // Your data collection
series.ResistancePath = "Resistance";
series.ReactancePath = "Reactance";
series.Label = "TransmissionLine";

chart.Series.Add(series);
```

### Adding Legend

```csharp
SmithChartLegend legend = new SmithChartLegend();
chart.Legend = legend;
```

### Adding to Layout

Add the chart to your window or grid:

```csharp
// Add to Grid
this.RootGrid.Children.Add(chart);

// Or set as Window Content
this.Content = chart;
```

### Complete Code-Behind Example

```csharp
using Syncfusion.UI.Xaml.SmithChart;
using System.Collections.ObjectModel;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;

public partial class MainWindow : Window
{
    public ObservableCollection<TransmissionData> Data { get; set; }
    
    public MainWindow()
    {
        InitializeComponent();
        this.DataContext = this;
        
        // Initialize data
        InitializeData();
        
        // Create SmithChart
        CreateSmithChart();
    }
    
    private void CreateSmithChart()
    {
        SfSmithChart chart = new SfSmithChart();
        chart.Header = "Impedance Transmission";
        chart.Height = 400;
        chart.Width = 500;
        
        // Configure Horizontal Axis (Resistance)
        chart.HorizontalAxis = new HorizontalAxis();
        chart.HorizontalAxis.FontSize = 11;
        chart.HorizontalAxis.FontFamily = new FontFamily("Segoe UI");
        
        // Configure Radial Axis (Reactance)
        chart.RadialAxis = new RadialAxis();
        chart.RadialAxis.FontSize = 11;
        chart.RadialAxis.FontFamily = new FontFamily("Segoe UI");
        
        // Add LineSeries
        LineSeries series = new LineSeries();
        series.ItemsSource = Data;
        series.ResistancePath = "Resistance";
        series.ReactancePath = "Reactance";
        series.Label = "TransmissionLine";
        chart.Series.Add(series);
        
        // Add Legend
        SmithChartLegend legend = new SmithChartLegend();
        chart.Legend = legend;
        
        // Add to layout
        this.RootGrid.Children.Add(chart);
    }
    
    private void InitializeData()
    {
        Data = new ObservableCollection<TransmissionData>();
        Data.Add(new TransmissionData() { Resistance = 0, Reactance = 0.05 });
        Data.Add(new TransmissionData() { Resistance = 0.3, Reactance = 0.1 });
        Data.Add(new TransmissionData() { Resistance = 0.5, Reactance = 0.2 });
        Data.Add(new TransmissionData() { Resistance = 1.0, Reactance = 0.4 });
        Data.Add(new TransmissionData() { Resistance = 1.5, Reactance = 0.5 });
        Data.Add(new TransmissionData() { Resistance = 2.0, Reactance = 0.5 });
        Data.Add(new TransmissionData() { Resistance = 2.5, Reactance = 0.4 });
        Data.Add(new TransmissionData() { Resistance = 3.5, Reactance = 0.0 });
        Data.Add(new TransmissionData() { Resistance = 4.5, Reactance = -0.5 });
        Data.Add(new TransmissionData() { Resistance = 5, Reactance = -1.0 });
        Data.Add(new TransmissionData() { Resistance = 6, Reactance = -1.5 });
        Data.Add(new TransmissionData() { Resistance = 7, Reactance = -2.5 });
        Data.Add(new TransmissionData() { Resistance = 8, Reactance = -3.5 });
        Data.Add(new TransmissionData() { Resistance = 9, Reactance = -4.5 });
        Data.Add(new TransmissionData() { Resistance = 10, Reactance = -10 });
        Data.Add(new TransmissionData() { Resistance = 20, Reactance = -50 });
    }
}
```

## Data Model Setup

### Creating the Data Model Class

Define a class to represent transmission line data points:

```csharp
public class TransmissionData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
}
```

**Properties:**
- **Resistance**: Normalized resistance value (horizontal axis)
- **Reactance**: Normalized reactance value (radial axis)

### Sample Impedance Transmission Data

Typical impedance transmission line data:

| Resistance | Reactance |
|------------|-----------|
| 0 | 0.05 |
| 0.3 | 0.1 |
| 0.5 | 0.2 |
| 1.0 | 0.4 |
| 1.5 | 0.5 |
| 2.0 | 0.5 |
| 2.5 | 0.4 |
| 3.5 | 0.0 |
| 4.5 | -0.5 |
| 5.0 | -1.0 |
| 6.0 | -1.5 |
| 7.0 | -2.5 |
| 8.0 | -3.5 |
| 9.0 | -4.5 |
| 10 | -10 |
| 20 | -50 |

### Initializing Data Collection

Create an ObservableCollection to hold your data:

```csharp
public ObservableCollection<TransmissionData> Data { get; set; }

public MainWindow()
{
    InitializeComponent();
    this.DataContext = this; // Required for data binding
    
    Data = new ObservableCollection<TransmissionData>();
    
    // Add data points
    Data.Add(new TransmissionData() { Resistance = 0, Reactance = 0.05 });
    Data.Add(new TransmissionData() { Resistance = 0.3, Reactance = 0.1 });
    Data.Add(new TransmissionData() { Resistance = 0.5, Reactance = 0.2 });
    // ... add remaining data points
}
```

**Important:** Set `DataContext = this` to enable data binding in XAML.

### Using ViewModels (MVVM Pattern)

For better architecture, use a ViewModel:

```csharp
public class SmithChartViewModel : INotifyPropertyChanged
{
    private ObservableCollection<TransmissionData> data;
    
    public ObservableCollection<TransmissionData> Data
    {
        get { return data; }
        set
        {
            data = value;
            OnPropertyChanged("Data");
        }
    }
    
    public SmithChartViewModel()
    {
        LoadData();
    }
    
    private void LoadData()
    {
        Data = new ObservableCollection<TransmissionData>
        {
            new TransmissionData { Resistance = 0, Reactance = 0.05 },
            new TransmissionData { Resistance = 0.3, Reactance = 0.1 },
            new TransmissionData { Resistance = 1.0, Reactance = 0.4 },
            // ... more data
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

In XAML, set the ViewModel as DataContext:

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.SmithChart;assembly=Syncfusion.SfSmithChart.WPF"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Window.DataContext>
        <local:SmithChartViewModel/>
    </Window.DataContext>
    <Grid>
        <syncfusion:SfSmithChart Header="Impedance Transmission" Height="400" Width="500">
            <!-- LineSeries -->
            <syncfusion:LineSeries ResistancePath="Resistance" 
                                   ReactancePath="Reactance" 
                                   ItemsSource="{Binding Data}" 
                                   Label="TransmissionLine">
            </syncfusion:LineSeries>
            
            <!-- Horizontal Axis (Resistance) -->
            <syncfusion:SfSmithChart.HorizontalAxis>
                <syncfusion:HorizontalAxis FontSize="11"/>
            </syncfusion:SfSmithChart.HorizontalAxis>
            
            <!-- Radial Axis (Reactance) -->
            <syncfusion:SfSmithChart.RadialAxis>
                <syncfusion:RadialAxis FontSize="11"/>
            </syncfusion:SfSmithChart.RadialAxis>
            
            <!-- Legend -->
            <syncfusion:SfSmithChart.Legend>
                <syncfusion:SmithChartLegend/>
            </syncfusion:SfSmithChart.Legend>
        </syncfusion:SfSmithChart>
    </Grid>
</Window>
```

## Theme Support

Apply built-in themes to the `SmithChart` using `SfSkinManager`.

### Available Themes

- FluentLight / FluentDark
- MaterialLight / MaterialDark
- Office2019Colorful / Office2019Black / Office2019White
- SystemTheme
- Material3Light / Material3Dark
- Windows11LighT / Windows11Dark

### Applying Theme via SfSkinManager

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=Windows11Light}">
    <Grid>
        <syncfusion:SfSmithChart Header="Impedance Transmission">
            <!-- Chart content -->
        </syncfusion:SfSmithChart>
    </Grid>
</Window>
```

### Applying Theme in Code-Behind

```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetTheme(this, new Theme("Windows11Light"));
```

### Creating Custom Theme with ThemeStudio

Use Syncfusion ThemeStudio to create custom themes:
1. Visit Syncfusion ThemeStudio
2. Customize colors and styles
3. Export theme package
4. Apply to your application

## Complete Examples

### Minimal Example (XAML)

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.SmithChart;assembly=Syncfusion.SfSmithChart.WPF">
    <syncfusion:SfSmithChart>
        <syncfusion:LineSeries ResistancePath="Resistance" 
                               ReactancePath="Reactance" 
                               ItemsSource="{Binding Data}"/>
    </syncfusion:SfSmithChart>
</Window>
```

### Standard Example with All Components

```xml
<syncfusion:SfSmithChart Header="Impedance Transmission" 
                         Height="400" Width="500"
                         Background="White">
    
    <!-- Series -->
    <syncfusion:LineSeries ResistancePath="Resistance" 
                           ReactancePath="Reactance" 
                           ItemsSource="{Binding Data}" 
                           Label="TransmissionLine"
                           ShowMarker="True"
                           MarkerType="Circle"/>
    
    <!-- Horizontal Axis -->
    <syncfusion:SfSmithChart.HorizontalAxis>
        <syncfusion:HorizontalAxis FontSize="11" 
                                   FontFamily="Segoe UI"
                                   ShowMajorGridlines="True"/>
    </syncfusion:SfSmithChart.HorizontalAxis>
    
    <!-- Radial Axis -->
    <syncfusion:SfSmithChart.RadialAxis>
        <syncfusion:RadialAxis FontSize="11" 
                               FontFamily="Segoe UI"
                               ShowMajorGridlines="True"/>
    </syncfusion:SfSmithChart.RadialAxis>
    
    <!-- Legend -->
    <syncfusion:SfSmithChart.Legend>
        <syncfusion:SmithChartLegend DockPosition="Bottom"/>
    </syncfusion:SfSmithChart.Legend>
</syncfusion:SfSmithChart>
```

## Troubleshooting

### Assembly Not Found
**Issue**: Cannot find Syncfusion.SfSmithChart.WPF assembly  
**Solution**: 
- Verify Syncfusion controls are installed
- Check correct .NET Framework version
- Rebuild project after adding reference

### Data Not Displaying
**Issue**: Chart shows axes but no data line  
**Solution**:
- Verify ItemsSource is bound correctly
- Check ResistancePath/ReactancePath match property names exactly (case-sensitive)
- Ensure DataContext is set
- Verify data collection is not empty

### XAML Namespace Error
**Issue**: "The name 'SfSmithChart' does not exist in the namespace..."  
**Solution**:
- Ensure xmlns namespace is correctly declared
- Check assembly name in namespace declaration
- Verify assembly is referenced in project

### Binding Errors
**Issue**: Data binding not working  
**Solution**:
- Set `DataContext = this` in code-behind constructor
- Use ObservableCollection for dynamic updates
- Implement INotifyPropertyChanged for ViewModel
- Check Output window for binding errors

## Next Steps

Now that you have a basic SmithChart running:
- Learn about series customization and multiple series
- Explore axes configuration and gridline customization
- Add data markers and labels for better visualization
- Configure legends and appearance
- Implement interactive features like tooltips