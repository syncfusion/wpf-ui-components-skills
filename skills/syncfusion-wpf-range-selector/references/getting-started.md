# Getting Started with Range Selectors

Complete guide to installing, configuring, and implementing the Syncfusion WPF Range Selector (SfDateTimeRangeNavigator) control in your application.

## Visual Structure

The SfDateTimeRangeNavigator is composed of four main visual elements:

### 1. Higher Level Bar
Contains timespan format that is one level higher than the lower level bar. For example:
- If lower bar shows months (MMM), higher bar shows years (yyyy)
- If lower bar shows days, higher bar shows months
- Provides hierarchical context for time navigation

### 2. Lower Level Bar
Contains timespan format that is one level lower than the higher level bar. For example:
- If higher bar shows years (yyyy), lower bar shows months (MMM)
- If higher bar shows months, lower bar shows days
- Provides detailed time granularity

### 3. Content Area
Holds any type of UI element inside the navigator:
- Charts (SfChart with any series type)
- Sparklines (lightweight data preview)
- Custom visualizations
- Any WPF UIElement

### 4. Resizable Scrollbar
Allows users to:
- Zoom into specific time ranges by resizing
- Scroll through large datasets by dragging
- Select precise regions with visual feedback
- View selected range boundaries with thumbs

## Required Dependencies

The Range Selector requires these assemblies:
- **Syncfusion.SfChart.WPF** - Core chart and navigator controls
- **Syncfusion.Data.WPF** - Data management utilities
- **Syncfusion.Shared.WPF** - Shared components and utilities

### XAML Namespace

Add the Syncfusion namespace to your XAML Window or UserControl:

```xml
<Window x:Class="YourApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    <!-- Your content -->
</Window>
```

### C# Namespace

Add using directives in your code-behind:

```csharp
using Syncfusion.UI.Xaml.Charts;
using System.Collections.ObjectModel;
using System.ComponentModel;
```

## Basic Implementation

### Step 1: Create a Data Model

Define a class to hold your time-series data:

```csharp
public class DataPoint
{
    public DateTime Date { get; set; }
    public double Value { get; set; }
}
```

### Step 2: Create a ViewModel

Set up a ViewModel with observable data:

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    public ObservableCollection<DataPoint> DataCollection { get; set; }
    
    public MainViewModel()
    {
        DataCollection = new ObservableCollection<DataPoint>();
        GenerateData();
    }
    
    private void GenerateData()
    {
        DateTime startDate = new DateTime(2024, 1, 1);
        Random random = new Random();
        
        for (int i = 0; i < 365; i++)
        {
            DataCollection.Add(new DataPoint
            {
                Date = startDate.AddDays(i),
                Value = 50 + random.Next(-10, 20) + (i * 0.1)
            });
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Step 3: Initialize SfDateTimeRangeNavigator in XAML

Create a basic range navigator:

```xml
<Window x:Class="RangeSelectorDemo.MainWindow"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    <Window.DataContext>
        <local:MainViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:SfDateTimeRangeNavigator 
            ItemsSource="{Binding DataCollection}" 
            XBindingPath="Date"
            Height="150"
            VerticalAlignment="Bottom">
        </syncfusion:SfDateTimeRangeNavigator>
    </Grid>
</Window>
```

**Key Properties:**
- **ItemsSource** - Binds to your data collection (must implement IEnumerable)
- **XBindingPath** - Specifies the property name containing DateTime values

### Step 4: Add Content to the Navigator

Display a visualization inside the navigator using the Content property:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding DataCollection}" 
    XBindingPath="Date"
    Height="150">
    
    <syncfusion:SfDateTimeRangeNavigator.Content>
        <syncfusion:SfLineSparkline 
            ItemsSource="{Binding DataCollection}"
            YBindingPath="Value"/>
    </syncfusion:SfDateTimeRangeNavigator.Content>
    
</syncfusion:SfDateTimeRangeNavigator>
```

## Complete Working Example

### XAML (MainWindow.xaml)

```xml
<Window x:Class="RangeSelectorDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:RangeSelectorDemo"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF"
        Title="Range Selector Demo" Height="600" Width="900">
    
    <Window.DataContext>
        <local:MainViewModel/>
    </Window.DataContext>
    
    <Grid Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="150"/>
        </Grid.RowDefinitions>
        
        <!-- Main Chart -->
        <syncfusion:SfChart Grid.Row="0" x:Name="Chart">
            <syncfusion:SfChart.PrimaryAxis>
                <syncfusion:DateTimeAxis 
                    Header="Date"
                    LabelFormat="MMM dd"
                    ZoomPosition="{Binding ElementName=RangeNavigator, Path=ZoomPosition, Mode=TwoWay}"
                    ZoomFactor="{Binding ElementName=RangeNavigator, Path=ZoomFactor, Mode=TwoWay}"/>
            </syncfusion:SfChart.PrimaryAxis>
            
            <syncfusion:SfChart.SecondaryAxis>
                <syncfusion:NumericalAxis Header="Value"/>
            </syncfusion:SfChart.SecondaryAxis>
            
            <syncfusion:LineSeries 
                ItemsSource="{Binding DataCollection}" 
                XBindingPath="Date"
                YBindingPath="Value"
                Label="Data Series">
            </syncfusion:LineSeries>
        </syncfusion:SfChart>
        
        <!-- Range Navigator -->
        <syncfusion:SfDateTimeRangeNavigator 
            Grid.Row="1"
            x:Name="RangeNavigator"
            ItemsSource="{Binding DataCollection}"
            XBindingPath="Date"
            Margin="0,10,0,0">
            
            <syncfusion:SfDateTimeRangeNavigator.Content>
                <syncfusion:SfLineSparkline 
                    ItemsSource="{Binding DataCollection}"
                    YBindingPath="Value"/>
            </syncfusion:SfDateTimeRangeNavigator.Content>
            
        </syncfusion:SfDateTimeRangeNavigator>
    </Grid>
</Window>
```

This example demonstrates:
- Synchronized zooming between chart and range navigator
- Two-way binding of ZoomPosition and ZoomFactor
- Sparkline preview in the navigator content area
- Proper layout with Grid rows

## Code-Behind Implementation

You can also create the range navigator entirely in code-behind:

### Complete C# Example

```csharp
using System;
using System.Collections.ObjectModel;
using System.Windows;
using Syncfusion.UI.Xaml.Charts;

namespace RangeSelectorDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            CreateRangeNavigator();
        }
        
        private void CreateRangeNavigator()
        {
            // Create data
            var viewModel = new MainViewModel();
            
            // Create sparkline for content
            SfLineSparkline sparkline = new SfLineSparkline
            {
                ItemsSource = viewModel.DataCollection,
                YBindingPath = "Value"
            };
            
            // Create range navigator
            SfDateTimeRangeNavigator rangeNavigator = new SfDateTimeRangeNavigator
            {
                ItemsSource = viewModel.DataCollection,
                XBindingPath = "Date",
                Height = 150,
                VerticalAlignment = VerticalAlignment.Bottom,
                Content = sparkline
            };
            
            // Create chart
            SfChart chart = new SfChart();
            
            chart.PrimaryAxis = new DateTimeAxis
            {
                Header = "Date",
                LabelFormat = "MMM dd"
            };
            
            chart.SecondaryAxis = new NumericalAxis
            {
                Header = "Value"
            };
            
            LineSeries series = new LineSeries
            {
                ItemsSource = viewModel.DataCollection,
                XBindingPath = "Date",
                YBindingPath = "Value",
                Label = "Data Series"
            };
            
            chart.Series.Add(series);
            
            // Create binding for synchronization
            System.Windows.Data.Binding zoomPositionBinding = new System.Windows.Data.Binding
            {
                Source = rangeNavigator,
                Path = new PropertyPath("ZoomPosition"),
                Mode = System.Windows.Data.BindingMode.TwoWay
            };
            
            System.Windows.Data.Binding zoomFactorBinding = new System.Windows.Data.Binding
            {
                Source = rangeNavigator,
                Path = new PropertyPath("ZoomFactor"),
                Mode = System.Windows.Data.BindingMode.TwoWay
            };
            
            chart.PrimaryAxis.SetBinding(DateTimeAxis.ZoomPositionProperty, zoomPositionBinding);
            chart.PrimaryAxis.SetBinding(DateTimeAxis.ZoomFactorProperty, zoomFactorBinding);
            
            // Add to layout
            Grid mainGrid = new Grid { Margin = new Thickness(20) };
            mainGrid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star) });
            mainGrid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(150) });
            
            Grid.SetRow(chart, 0);
            Grid.SetRow(rangeNavigator, 1);
            
            mainGrid.Children.Add(chart);
            mainGrid.Children.Add(rangeNavigator);
            
            this.Content = mainGrid;
        }
    }
}
```

## Working with Different Content Types

### Using SfChart as Content

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding DataCollection}" 
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.Content>
        <syncfusion:SfChart>
            <syncfusion:SfChart.PrimaryAxis>
                <syncfusion:CategoryAxis Visibility="Collapsed"/>
            </syncfusion:SfChart.PrimaryAxis>
            
            <syncfusion:SfChart.SecondaryAxis>
                <syncfusion:NumericalAxis Visibility="Collapsed"/>
            </syncfusion:SfChart.SecondaryAxis>
            
            <syncfusion:AreaSeries 
                ItemsSource="{Binding DataCollection}"
                XBindingPath="Date"
                YBindingPath="Value">
            </syncfusion:AreaSeries>
        </syncfusion:SfChart>
    </syncfusion:SfDateTimeRangeNavigator.Content>
    
</syncfusion:SfDateTimeRangeNavigator>
```

### Using Column Sparkline

```xml
<syncfusion:SfDateTimeRangeNavigator.Content>
    <syncfusion:SfColumnSparkline 
        ItemsSource="{Binding DataCollection}"
        YBindingPath="Value"/>
</syncfusion:SfDateTimeRangeNavigator.Content>
```

## Best Practices

### 1. Data Collection Requirements
- ItemsSource must implement `IEnumerable`
- Use `ObservableCollection<T>` for dynamic data updates
- Ensure date-time values are in the property specified by XBindingPath

### 2. Performance Optimization
- Use sparklines instead of full charts for content when possible
- Enable deferred updates for large datasets (see interactivity.md)
- Consider data sampling for extremely large time series

### 3. Layout Considerations
- Set explicit Height for the navigator (typically 100-200 pixels)
- Place navigator at the bottom of your visualization for intuitive navigation
- Leave enough space for label bars (minimum 60 pixels)

### 4. Binding Best Practices
- Always use TwoWay binding mode for ZoomPosition and ZoomFactor
- Set ElementName binding for XAML synchronization
- Use INotifyPropertyChanged in ViewModels for proper updates

## Common Issues and Quick Fixes

### Navigator Not Displaying
**Problem:** Empty or blank navigator  
**Solution:** Verify ItemsSource is populated and XBindingPath property name matches your data model

### Labels Not Showing
**Problem:** Label bars are empty  
**Solution:** Ensure date-time data covers a sufficient range (at least a few days)

### Content Not Visible
**Problem:** Content area is blank  
**Solution:** Set explicit Height on the navigator; check that Content element has valid data binding

### Zoom Not Synchronizing
**Problem:** Chart doesn't zoom with navigator  
**Solution:** Verify TwoWay binding on both ZoomPosition and ZoomFactor properties

## Next Steps

Now that you have a working range navigator:

- **Add interactivity:** Read [interactivity.md](interactivity.md) for events, zoom configuration, and customization
- **Customize labels:** Read [label-customization.md](label-customization.md) for interval types and styling
- **Configure tooltips:** Read [tooltip-support.md](tooltip-support.md) for tooltip display options

## Summary

You've learned how to:
- ✓ Install and reference required assemblies
- ✓ Register Syncfusion license
- ✓ Import necessary namespaces
- ✓ Create data models and ViewModels
- ✓ Initialize SfDateTimeRangeNavigator in XAML and code
- ✓ Bind data using ItemsSource and XBindingPath
- ✓ Add content (charts, sparklines) to the navigator
- ✓ Synchronize with main chart using zoom bindings
- ✓ Follow best practices for optimal implementation
