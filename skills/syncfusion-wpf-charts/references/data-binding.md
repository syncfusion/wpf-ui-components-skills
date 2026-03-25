# Data Binding

This guide covers how to bind data to chart series using various data sources and patterns.

## Basic Data Binding

Every chart series requires three key properties for data binding:

- **ItemsSource** - The data collection
- **XBindingPath** - Property name for X-axis values
- **YBindingPath** - Property name for Y-axis values

```xml
<syncfusion:ColumnSeries ItemsSource="{Binding SalesData}"
                         XBindingPath="Month"
                         YBindingPath="Amount"/>
```

## Simple Collection Binding

### Data Model

```csharp
public class SalesData
{
    public string Month { get; set; }
    public double Amount { get; set; }
}
```

### ViewModel

```csharp
using System.Collections.ObjectModel;

public class ChartViewModel
{
    public ObservableCollection<SalesData> SalesData { get; set; }
    
    public ChartViewModel()
    {
        SalesData = new ObservableCollection<SalesData>
        {
            new SalesData { Month = "Jan", Amount = 35000 },
            new SalesData { Month = "Feb", Amount = 48000 },
            new SalesData { Month = "Mar", Amount = 42000 }
        };
    }
}
```

### XAML Binding

```xml
<syncfusion:SfChart>
    <syncfusion:ColumnSeries ItemsSource="{Binding SalesData}"
                             XBindingPath="Month"
                             YBindingPath="Amount"/>
</syncfusion:SfChart>
```

## Dynamic Data Updates

Use `ObservableCollection` for automatic UI updates when data changes:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<DataPoint> _data;
    
    public ObservableCollection<DataPoint> Data
    {
        get { return _data; }
        set
        {
            _data = value;
            OnPropertyChanged("Data");
        }
    }
    
    public void AddDataPoint(DataPoint point)
    {
        Data.Add(point);  // Chart updates automatically
    }
    
    public void UpdateDataPoint(int index, double newValue)
    {
        Data[index].Value = newValue;  // Chart updates automatically
    }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Binding to Different Data Types

### Numeric Data

```csharp
public class NumericData
{
    public int Index { get; set; }
    public double Value { get; set; }
}
```

```xml
<syncfusion:LineSeries ItemsSource="{Binding NumericData}"
                       XBindingPath="Index"
                       YBindingPath="Value"/>
```

### DateTime Data

```csharp
public class TimeSeriesData
{
    public DateTime Date { get; set; }
    public double Price { get; set; }
}
```

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:DateTimeAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:LineSeries ItemsSource="{Binding TimeSeriesData}"
                           XBindingPath="Date"
                           YBindingPath="Price"/>
</syncfusion:SfChart>
```

### Complex Property Binding

Bind to nested properties using dot notation:

```csharp
public class Employee
{
    public string Name { get; set; }
    public Performance PerformanceData { get; set; }
}

public class Performance
{
    public double Score { get; set; }
}
```

```xml
<syncfusion:ColumnSeries ItemsSource="{Binding Employees}"
                         XBindingPath="Name"
                         YBindingPath="PerformanceData.Score"/>
```

## Financial Data Binding

Financial series require four properties: Open, High, Low, Close.

### Data Model

```csharp
public class StockData
{
    public DateTime Date { get; set; }
    public double Open { get; set; }
    public double High { get; set; }
    public double Low { get; set; }
    public double Close { get; set; }
}
```

### Binding

```xml
<syncfusion:CandleSeries ItemsSource="{Binding StockData}"
                         XBindingPath="Date"
                         Open="Open"
                         High="High"
                         Low="Low"
                         Close="Close"/>
```

## Multiple Series Binding

Bind multiple series to different data sources:

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:CategoryAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <syncfusion:ColumnSeries ItemsSource="{Binding Year2023Data}"
                             XBindingPath="Month"
                             YBindingPath="Sales"
                             Label="2023"/>
    
    <syncfusion:ColumnSeries ItemsSource="{Binding Year2024Data}"
                             XBindingPath="Month"
                             YBindingPath="Sales"
                             Label="2024"/>
</syncfusion:SfChart>
```

## Bubble and Scatter Series Binding

Bubble series require Size property in addition to X and Y:

```csharp
public class BubbleData
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Size { get; set; }
}
```

```xml
<syncfusion:BubbleSeries ItemsSource="{Binding BubbleData}"
                         XBindingPath="X"
                         YBindingPath="Y"
                         Size="Size"/>
```

## Range Series Binding

Range series require High and Low properties:

```csharp
public class RangeData
{
    public string Category { get; set; }
    public double High { get; set; }
    public double Low { get; set; }
}
```

```xml
<syncfusion:RangeColumnSeries ItemsSource="{Binding RangeData}"
                              XBindingPath="Category"
                              High="High"
                              Low="Low"/>
```

## Binding to Array or List

Direct binding to arrays or lists:

```csharp
public List<DataPoint> ChartData { get; set; }
public double[] Values { get; set; }
```

```xml
<syncfusion:LineSeries ItemsSource="{Binding ChartData}"
                       XBindingPath="X"
                       YBindingPath="Y"/>
```

## MVVM Pattern Best Practices

### ViewModel Structure

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private ObservableCollection<ChartData> _chartData;
    
    public ObservableCollection<ChartData> ChartData
    {
        get { return _chartData; }
        set
        {
            _chartData = value;
            OnPropertyChanged(nameof(ChartData));
        }
    }
    
    public MainViewModel()
    {
        LoadData();
    }
    
    private void LoadData()
    {
        ChartData = new ObservableCollection<ChartData>
        {
            // Load your data here
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, 
            new PropertyChangedEventArgs(propertyName));
    }
}
```

### Setting DataContext

In Window constructor:

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new MainViewModel();
}
```

Or in XAML:

```xml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>
```

## Key Binding Properties

| Property | Purpose | Required For |
|----------|---------|--------------|
| `ItemsSource` | Data collection | All series |
| `XBindingPath` | X-axis property | All series |
| `YBindingPath` | Y-axis property | Most series |
| `High` | High value property | Financial, Range series |
| `Low` | Low value property | Financial, Range series |
| `Open` | Open value property | Financial series |
| `Close` | Close value property | Financial series |
| `Size` | Bubble size property | Bubble series |

## Performance Tips

1. **Use ObservableCollection** for dynamic data that changes during runtime
2. **Use List or Array** for static data that doesn't change
3. **Batch Updates:** Suspend collection notifications during bulk updates
4. **Avoid Complex Bindings:** Direct property binding is faster than nested properties
5. **Data Sampling:** For very large datasets, consider data sampling or Fast series
