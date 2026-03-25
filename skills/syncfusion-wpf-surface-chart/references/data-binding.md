# Data Binding in WPF Surface Chart

## Table of Contents
- [Overview](#overview)
- [Data Structure Requirements](#data-structure-requirements)
- [Method 1: Using ItemsSource](#method-1-using-itemssource)
- [Method 2: Using Data.AddPoints](#method-2-using-dataaddpoints)
- [Data Model Setup](#data-model-setup)
- [Common Data Patterns](#common-data-patterns)
- [Best Practices](#best-practices)

## Overview

In surface chart, data must be applied in a grid table format containing a specified number of rows and columns. The SfSurfaceChart provides two primary methods for binding data:

1. **ItemsSource Property** - Bind to a collection via MVVM pattern
2. **Data.AddPoints Method** - Directly add data points programmatically

Both methods require specifying RowSize and ColumnSize to match your data dimensions.

## Data Structure Requirements

### Grid Table Format

Surface chart data follows a grid structure:

|     | Z₀  | Z₁  | Z₂  | ... | Zₙ  |
|-----|-----|-----|-----|-----|-----|
| X₀  | Y₀₀ | Y₀₁ | Y₀₂ | ... | Y₀ₙ |
| X₁  | Y₁₀ | Y₁₁ | Y₁₂ | ... | Y₁ₙ |
| X₂  | Y₂₀ | Y₂₁ | Y₂₂ | ... | Y₂ₙ |
| ... | ... | ... | ... | ... | ... |
| Xₙ  | Yₙ₀ | Yₙ₁ | Yₙ₂ | ... | Yₙₙ |

**Key Requirements:**
- Data must be organized in a grid with consistent rows and columns
- RowSize = Number of rows in the grid
- ColumnSize = Number of columns in the grid
- Total data points = RowSize × ColumnSize
- Data order matters - organize in row-major or column-major format

## Method 1: Using ItemsSource

### Overview

This method uses MVVM pattern with data binding. Each item in the collection holds the model properties that map to XBindingPath, YBindingPath, and ZBindingPath.

**Advantages:**
- Clean separation of concerns (MVVM)
- Automatic UI updates with ObservableCollection
- Easier data manipulation
- Better for dynamic data

### Basic Data Model

```csharp
public class Data
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Z { get; set; }
}
```

### ViewModel Setup

```csharp
public class ViewModel
{
    public ObservableCollection<Data> DataValue { get; set; }
    public int RowSize { get; set; }
    public int ColumnSize { get; set; }
    
    public ViewModel()
    {
        DataValue = new ObservableCollection<Data>();
        
        // Generate data: y = x*sin(z) + z*sin(x)
        for (double x = -10; x < 10; x++)
        {
            for (double z = -10; z < 10; z++)
            {
                double y = x * Math.Sin(z) + z * Math.Sin(x);
                DataValue.Add(new Data() { X = x, Y = y, Z = z });
            }
        }
        
        RowSize = 20;
        ColumnSize = 20;
    }
}
```

### XAML Binding

```xaml
<Grid.DataContext>
    <local:ViewModel />
</Grid.DataContext>

<chart:SfSurfaceChart ItemsSource="{Binding DataValue}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}">
</chart:SfSurfaceChart>
```

### Code-Behind Binding

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.SetBinding(SfSurfaceChart.ItemsSourceProperty, "DataValue");
chart.SetBinding(SfSurfaceChart.RowSizeProperty, "RowSize");
chart.SetBinding(SfSurfaceChart.ColumnSizeProperty, "ColumnSize");
chart.XBindingPath = "X";
chart.YBindingPath = "Y";
chart.ZBindingPath = "Z";

ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;
colorBar.ShowLabel = true;
chart.ColorBar = colorBar;

grid.Children.Add(chart);
```

### Important Notes for ItemsSource

1. **Binding Path Names:** XBindingPath, YBindingPath, and ZBindingPath must match property names in your data model exactly (case-sensitive)
2. **Collection Type:** Use ObservableCollection for automatic UI updates
3. **RowSize and ColumnSize:** Must match the actual data dimensions
4. **Data Order:** Organize data in consistent row-major or column-major order

## Method 2: Using Data.AddPoints

### Overview

This method directly passes data points to the Data property using the AddPoints(x, y, z) method. No ItemsSource or member paths are needed.

**Advantages:**
- Simpler for static data
- More direct control
- No need for data model classes
- Faster for one-time data loading

### Basic Implementation

```xaml
<chart:SfSurfaceChart x:Name="surface" />
```

```csharp
public MainWindow()
{
    InitializeComponent();
    SetData();
}

private void SetData()
{
    for (double x = -10; x < 10; x++)
    {
        for (double z = -10; z < 10; z++)
        {
            double y = x * Math.Sin(z) + z * Math.Sin(x);
            surface.Data.AddPoints(x, y, z);  // Directly add data points
        }
    }
    
    surface.RowSize = 20;
    surface.ColumnSize = 20;
}
```

### Complete Example

```csharp
SfSurfaceChart surface = new SfSurfaceChart();
surface.Header = "Direct Data Input";
surface.Type = SurfaceType.Surface;

// Generate paraboloid: y = x² + z²
for (double x = -5; x <= 5; x += 0.5)
{
    for (double z = -5; z <= 5; z += 0.5)
    {
        double y = (x * x) + (z * z);
        surface.Data.AddPoints(x, y, z);
    }
}

surface.RowSize = 21;  // Number of x values
surface.ColumnSize = 21;  // Number of z values

grid.Children.Add(surface);
```

### Important Notes for AddPoints

1. **Call AddPoints in correct order:** Data must be added in row-major or column-major order
2. **RowSize and ColumnSize are required:** Must be set after adding all points
3. **No automatic updates:** Changes to data require regenerating the entire dataset
4. **Better for static data:** Best used when data doesn't change frequently

## Data Model Setup

### Simple Mathematical Function

```csharp
public class ViewModel
{
    public ObservableCollection<Data> DataValues { get; set; }
    public int RowSize { get; set; }
    public int ColumnSize { get; set; }
    
    public ViewModel()
    {
        DataValues = new ObservableCollection<Data>();
        GenerateParaboloid();
    }
    
    private void GenerateParaboloid()
    {
        // y = 2x² + 2z² - 4
        double inc = 8.0 / 35;
        for (double x = -4; x < 4; x += inc)
        {
            for (double z = -4; z < 4; z += inc)
            {
                double y = 2 * (x * x) + 2 * (z * z) - 4;
                DataValues.Add(new Data() { X = x, Y = y, Z = z });
            }
        }
        
        RowSize = 35;
        ColumnSize = 35;
    }
}
```

### Custom Data with Additional Properties

```csharp
public class DataPoint
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Z { get; set; }
    
    // Additional properties for internal use
    public string Category { get; set; }
    public DateTime Timestamp { get; set; }
}

public class ViewModel
{
    public ObservableCollection<DataPoint> DataValues { get; set; }
    
    public ViewModel()
    {
        DataValues = new ObservableCollection<DataPoint>();
        // Load data from file, database, or API
        LoadDataFromSource();
    }
    
    private void LoadDataFromSource()
    {
        // Your data loading logic
        // Ensure data is organized in grid format
    }
}
```

## Common Data Patterns

### Pattern 1: Mathematical Function Visualization

```csharp
// Sine wave surface: z = sin(x) * cos(y)
private void GenerateSineWave()
{
    for (double x = -Math.PI; x <= Math.PI; x += 0.1)
    {
        for (double y = -Math.PI; y <= Math.PI; y += 0.1)
        {
            double z = Math.Sin(x) * Math.Cos(y);
            chart.Data.AddPoints(x, z, y);
        }
    }
    
    chart.RowSize = 63;
    chart.ColumnSize = 63;
}
```

### Pattern 2: Dynamic Data Updates

```csharp
public class DynamicViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Data> _dataValues;
    
    public ObservableCollection<Data> DataValues
    {
        get => _dataValues;
        set
        {
            _dataValues = value;
            OnPropertyChanged(nameof(DataValues));
        }
    }
    
    public void UpdateData()
    {
        // Clear and regenerate data
        DataValues.Clear();
        
        for (double x = -10; x < 10; x++)
        {
            for (double z = -10; z < 10; z++)
            {
                double y = CalculateNewValue(x, z);
                DataValues.Add(new Data() { X = x, Y = y, Z = z });
            }
        }
    }
    
    private double CalculateNewValue(double x, double z)
    {
        // Your calculation logic
        return x * Math.Sin(z) + z * Math.Sin(x);
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Best Practices

### 1. Choose the Right Method

**Use ItemsSource when:**
- Implementing MVVM pattern
- Data changes dynamically
- Need automatic UI updates
- Multiple views bind to same data
- Data comes from external sources (database, API)

**Use Data.AddPoints when:**
- Data is static or rarely changes
- Generating mathematical functions
- Simple, straightforward scenarios
- Performance is critical (one-time loading)
- No need for MVVM architecture

### 2. Ensure Data Consistency

```csharp
// ✓ CORRECT: RowSize and ColumnSize match data
for (int i = 0; i < 10; i++)        // 10 rows
{
    for (int j = 0; j < 15; j++)    // 15 columns
    {
        chart.Data.AddPoints(i, CalculateY(i, j), j);
    }
}
chart.RowSize = 10;
chart.ColumnSize = 15;

// ✗ INCORRECT: Mismatch causes rendering issues
chart.RowSize = 8;   // Wrong!
chart.ColumnSize = 12;  // Wrong!
```

### 3. Maintain Data Order

Data must be added in consistent order (row-major):

```csharp
// ✓ CORRECT: Row-major order
for (double x = 0; x < 10; x++)     // Outer loop = rows
{
    for (double z = 0; z < 10; z++) // Inner loop = columns
    {
        chart.Data.AddPoints(x, y, z);
    }
}
```

### 4. Performance Optimization

```csharp
// For large datasets, consider:
// 1. Reducing data resolution
double step = 0.5;  // Instead of 0.1
for (double x = -10; x < 10; x += step)
{
    for (double z = -10; z < 10; z += step)
    {
        // Add points
    }
}

// 2. Using background thread for data generation
await Task.Run(() => GenerateDataPoints());
```

### 5. Validation

```csharp
private bool ValidateDataDimensions()
{
    int expectedCount = RowSize * ColumnSize;
    int actualCount = DataValues.Count;
    
    if (expectedCount != actualCount)
    {
        throw new InvalidOperationException(
            $"Data count mismatch. Expected: {expectedCount}, Actual: {actualCount}");
    }
    
    return true;
}
```

---