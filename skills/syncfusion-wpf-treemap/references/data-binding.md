# Data Binding in TreeMap

TreeMap visualization depends on properly configured data binding. This guide covers the essential properties and techniques for binding data to the TreeMap control.

## Core Data Binding Properties

### WeightValuePath

The `WeightValuePath` property specifies which property of your data model determines the size of each rectangle in the TreeMap.

**Purpose:** Controls rectangle dimensions - larger weight values create larger rectangles.

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population">
</syncfusion:SfTreeMap>
```

**Requirements:**
- Must point to a numeric property (int, double, decimal, etc.)
- Property name is case-sensitive
- Values should be positive numbers
- Zero or negative values are ignored

**Example Data Model:**
```csharp
public class PopulationDetail
{
    public string Country { get; set; }
    public double Population { get; set; }  // ← WeightValuePath points here
}
```

### ColorValuePath

The `ColorValuePath` property specifies which property drives the color mapping logic for rectangles.

**Purpose:** Determines the value used for color mapping strategies (range-based, desaturation, etc.)

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:RangeBrushColorMapping>
            <syncfusion:RangeBrushColorMapping.Brushes>
                <syncfusion:RangeBrush From="0" To="2" Color="Yellow"/>
                <syncfusion:RangeBrush From="2" To="4" Color="Green"/>
            </syncfusion:RangeBrushColorMapping.Brushes>
        </syncfusion:RangeBrushColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
</syncfusion:SfTreeMap>
```

**Example Data Model:**
```csharp
public class PopulationDetail
{
    public string Country { get; set; }
    public double Population { get; set; }  // For WeightValuePath
    public double Growth { get; set; }       // ← ColorValuePath points here
}
```

### ItemsSource

The `ItemsSource` property binds to your data collection. It accepts any IEnumerable collection.

**Supported collection types:**
- `ObservableCollection<T>` (recommended for dynamic data)
- `List<T>`
- `IEnumerable<T>`
- Arrays
- DataTable (via conversion)

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}">
</syncfusion:SfTreeMap>
```

## Complete Data Binding Example

### Step 1: Define Data Model

```csharp
public class SalesData
{
    public string Region { get; set; }
    public string Product { get; set; }
    public double Revenue { get; set; }
    public double Profit { get; set; }
    public int Units { get; set; }
}
```

### Step 2: Create ViewModel

```csharp
public class SalesViewModel : INotifyPropertyChanged
{
    private ObservableCollection<SalesData> _salesData;
    
    public ObservableCollection<SalesData> SalesData
    {
        get { return _salesData; }
        set
        {
            _salesData = value;
            OnPropertyChanged(nameof(SalesData));
        }
    }
    
    public SalesViewModel()
    {
        SalesData = new ObservableCollection<SalesData>
        {
            new SalesData { Region = "North", Product = "Laptop", Revenue = 50000, Profit = 5000, Units = 100 },
            new SalesData { Region = "North", Product = "Phone", Revenue = 30000, Profit = 3000, Units = 200 },
            new SalesData { Region = "South", Product = "Laptop", Revenue = 45000, Profit = 4500, Units = 90 },
            new SalesData { Region = "South", Product = "Tablet", Revenue = 25000, Profit = 2500, Units = 150 }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Step 3: Bind in XAML

```xaml
<Window.DataContext>
    <local:SalesViewModel/>
</Window.DataContext>

<syncfusion:SfTreeMap ItemsSource="{Binding SalesData}" 
                      WeightValuePath="Revenue"
                      ColorValuePath="Profit">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Region" GroupGap="10"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## Data Model Requirements

### Minimum Requirements

Your data model must have:
1. **At least one numeric property** for WeightValuePath
2. **Properties for grouping** if using TreeMapFlatLevel (e.g., Category, Region)
3. **Optional numeric property** for ColorValuePath if using color mapping

### Recommended Data Model Structure

```csharp
public class TreeMapDataItem
{
    // Required: For rectangle sizing
    public double Weight { get; set; }
    
    // Optional: For color mapping
    public double Value { get; set; }
    
    // Optional: For hierarchy grouping
    public string Category { get; set; }
    public string SubCategory { get; set; }
    
    // Optional: For labels and tooltips
    public string Name { get; set; }
    public string Description { get; set; }
}
```

## Hierarchical Data Binding

TreeMap supports flat data with hierarchical grouping using the `GroupPath` property in levels.

### Example: Two-Level Hierarchy

**Data Model:**
```csharp
public class HierarchicalData
{
    public string Continent { get; set; }    // Level 1
    public string Country { get; set; }      // Level 2
    public double Population { get; set; }
    public double Area { get; set; }
}
```

**XAML:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding Data}" 
                      WeightValuePath="Population">
    <syncfusion:SfTreeMap.Levels>
        <!-- First level: Group by Continent -->
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
        
        <!-- Second level: Group by Country -->
        <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## Dynamic Data Updates

When using `ObservableCollection`, TreeMap automatically updates when the collection changes.

**Example:**
```csharp
public class DynamicViewModel : INotifyPropertyChanged
{
    public ObservableCollection<DataItem> Items { get; set; }
    
    public void AddItem(DataItem item)
    {
        Items.Add(item);  // TreeMap updates automatically
    }
    
    public void RemoveItem(DataItem item)
    {
        Items.Remove(item);  // TreeMap updates automatically
    }
    
    public void UpdateItem(DataItem item, double newWeight)
    {
        item.Weight = newWeight;  // If DataItem implements INotifyPropertyChanged
    }
}
```

**Data Item with Property Change Notification:**
```csharp
public class DataItem : INotifyPropertyChanged
{
    private double _weight;
    
    public double Weight
    {
        get { return _weight; }
        set
        {
            _weight = value;
            OnPropertyChanged(nameof(Weight));  // TreeMap updates automatically
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Code-Behind Data Binding

You can set ItemsSource programmatically:

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Create data
    var data = new List<DataItem>
    {
        new DataItem { Name = "Item1", Weight = 100, Value = 50 },
        new DataItem { Name = "Item2", Weight = 200, Value = 75 }
    };
    
    // Set ItemsSource
    treeMap.ItemsSource = data;
    treeMap.WeightValuePath = "Weight";
    treeMap.ColorValuePath = "Value";
}
```

## Common Data Binding Patterns

### Pattern 1: Single Numeric Value
When you only need size, no colors:
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding Items}" 
                      WeightValuePath="Value">
</syncfusion:SfTreeMap>
```

### Pattern 2: Size and Color from Different Properties
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding Items}" 
                      WeightValuePath="Quantity"
                      ColorValuePath="Performance">
</syncfusion:SfTreeMap>
```

### Pattern 3: Size and Color from Same Property
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding Items}" 
                      WeightValuePath="Amount"
                      ColorValuePath="Amount">
</syncfusion:SfTreeMap>
```

## Troubleshooting

**TreeMap shows nothing:**
- Check ItemsSource binding path
- Verify data collection has items
- Ensure WeightValuePath property name matches model exactly (case-sensitive)

**Rectangles all same size:**
- Verify WeightValuePath points to correct property
- Check that weight property has varying values
- Ensure property values are positive numbers

**Colors not applying:**
- Verify ColorValuePath property name is correct
- Check that color mapping is configured
- Ensure ColorValuePath property has numeric values

**TreeMap doesn't update when data changes:**
- Use ObservableCollection instead of List
- Implement INotifyPropertyChanged on data items
- Verify property change notifications are firing

**Binding errors in Output window:**
- Check property names are spelled correctly
- Verify DataContext is set correctly
- Ensure ViewModel properties are public
