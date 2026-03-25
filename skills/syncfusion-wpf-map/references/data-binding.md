# Data Binding in WPF Maps (SfMap)

## Table of Contents
- [Overview](#overview)
- [Binding Fundamentals](#binding-fundamentals)
- [ItemsSource Binding](#itemssource-binding)
- [Shape Matching Properties](#shape-matching-properties)
- [Value Path Properties](#value-path-properties)
- [MVVM Pattern Implementation](#mvvm-pattern-implementation)
- [Troubleshooting Data Binding](#troubleshooting-data-binding)

## Overview

Data binding connects your business data to map shapes, enabling data-driven visualizations. The Maps control supports standard WPF data binding patterns with specialized properties for geographical data mapping.

## Binding Fundamentals

Maps data binding requires three key steps:
1. **Provide data source** - Set ItemsSource to your collection
2. **Match shapes to data** - Use ShapeIDPath and ShapeIDTableField
3. **Map data values** - Use ShapeValuePath and ShapeColorValuePath

## ItemsSource Binding

The `ItemsSource` property accepts any IEnumerable collection of data objects.

### Basic Binding

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF">
    <Grid>
        <Grid.DataContext>
            <local:MapViewModel />
        </Grid.DataContext>

        <syncfusion:SfMap>
            <syncfusion:SfMap.Layers>
                <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                                           ItemsSource="{Binding States}"
                                           ShapeIDPath="Name"
                                           ShapeIDTableField="STATE_NAME" />
            </syncfusion:SfMap.Layers>
        </syncfusion:SfMap>
    </Grid>
</Window>
```

```csharp
public class StateData
{
    public string Name { get; set; }
    public int Population { get; set; }
    public double GrowthRate { get; set; }
}

public class MapViewModel
{
    public ObservableCollection<StateData> States { get; set; }

    public MapViewModel()
    {
        States = new ObservableCollection<StateData>
        {
            new StateData { Name = "California", Population = 39500000, GrowthRate = 0.1 },
            new StateData { Name = "Texas", Population = 29000000, GrowthRate = 1.4 },
            new StateData { Name = "Florida", Population = 21500000, GrowthRate = 1.5 }
        };
    }
}
```

### Code-Behind Binding

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";
layer.ItemsSource = viewModel.States;
layer.ShapeIDPath = "Name";
layer.ShapeIDTableField = "STATE_NAME";
```

## Shape Matching Properties

These properties connect your data objects to specific shapes in the shape file.

### ShapeIDPath

Specifies the property name in your data source that contains the identifier.

**Type:** string  
**Example:** `"Name"`, `"CountryCode"`, `"ID"`

```xml
<syncfusion:ShapeFileLayer ShapeIDPath="CountryCode" />
```

```csharp
layer.ShapeIDPath = "CountryCode";
```

### ShapeIDTableField

Specifies the field name in the shape file's .dbf file that contains the identifier.

**Type:** string  
**Example:** `"NAME"`, `"CNTRY_NAME"`, `"STATE_NAME"`

To find available fields:
1. Open the .dbf file in a database viewer or Excel
2. Look at column headers
3. Use the exact field name (case-sensitive)

```xml
<syncfusion:ShapeFileLayer ShapeIDTableField="CNTRY_NAME" />
```

```csharp
layer.ShapeIDTableField = "CNTRY_NAME";
```

### Matching Example

**Data source (C# class):**
```csharp
public class Country
{
    public string Name { get; set; }  // This will match shape file field
    public int Population { get; set; }
}
```

**Shape file .dbf contains:**
- NAME (field with country names)
- AREA
- POP_EST

**Binding configuration:**
```xml
<syncfusion:ShapeFileLayer ItemsSource="{Binding Countries}"
                           ShapeIDPath="Name"
                           ShapeIDTableField="NAME" />
```

The control matches each `Country` object where `Country.Name` equals the `NAME` field value in the shape file.

## Value Path Properties

These properties specify which data should be visualized on the map.

### ShapeValuePath

Specifies the property containing numerical data for visualization (used for sizing, color mapping).

**Type:** string  
**Example:** `"Population"`, `"Revenue"`, `"Temperature"`

```xml
<syncfusion:ShapeFileLayer.ShapeSettings>
    <syncfusion:ShapeSetting ShapeValuePath="Population" />
</syncfusion:ShapeFileLayer.ShapeSettings>
```

```csharp
ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeValuePath = "Population";
layer.ShapeSettings = shapeSetting;
```

### ShapeColorValuePath

Specifies the property for categorical color mapping.

**Type:** string  
**Example:** `"Category"`, `"Winner"`, `"Status"`

```xml
<syncfusion:ShapeFileLayer.ShapeSettings>
    <syncfusion:ShapeSetting ShapeColorValuePath="Winner" />
</syncfusion:ShapeFileLayer.ShapeSettings>
```

```csharp
shapeSetting.ShapeColorValuePath = "Winner";
```

### Complete Binding Example

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                                   ItemsSource="{Binding ElectionResults}"
                                   ShapeIDPath="State"
                                   ShapeIDTableField="STATE_NAME">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeValuePath="Electors"
                                         ShapeColorValuePath="Candidate" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

```csharp
public class ElectionResult
{
    public string State { get; set; }        // Matches STATE_NAME in .dbf
    public string Candidate { get; set; }    // Used for color (e.g., "Democrat", "Republican")
    public int Electors { get; set; }        // Used for numerical visualization
}
```

## MVVM Pattern Implementation

### ViewModel with INotifyPropertyChanged

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class MapViewModel : INotifyPropertyChanged
{
    private ObservableCollection<CountryData> countries;

    public ObservableCollection<CountryData> Countries
    {
        get { return countries; }
        set
        {
            countries = value;
            OnPropertyChanged(nameof(Countries));
        }
    }

    public MapViewModel()
    {
        LoadCountries();
    }

    private void LoadCountries()
    {
        Countries = new ObservableCollection<CountryData>
        {
            new CountryData { Name = "United States", Population = 331000000, GDP = 21.43 },
            new CountryData { Name = "China", Population = 1400000000, GDP = 14.72 },
            new CountryData { Name = "India", Population = 1380000000, GDP = 2.87 }
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class CountryData : INotifyPropertyChanged
{
    private string name;
    private int population;
    private double gdp;

    public string Name
    {
        get { return name; }
        set { name = value; OnPropertyChanged(nameof(Name)); }
    }

    public int Population
    {
        get { return population; }
        set { population = value; OnPropertyChanged(nameof(Population)); }
    }

    public double GDP
    {
        get { return gdp; }
        set { gdp = value; OnPropertyChanged(nameof(GDP)); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### XAML with DataContext

```xml
<Window x:Class="MapApp.MainWindow"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF"
        xmlns:local="clr-namespace:MapApp">
    
    <Window.DataContext>
        <local:MapViewModel />
    </Window.DataContext>

    <Grid>
        <syncfusion:SfMap>
            <syncfusion:SfMap.Layers>
                <syncfusion:ShapeFileLayer Uri="MapApp.ShapeFiles.world1.shp"
                                           ItemsSource="{Binding Countries}"
                                           ShapeIDPath="Name"
                                           ShapeIDTableField="NAME">
                    <syncfusion:ShapeFileLayer.ShapeSettings>
                        <syncfusion:ShapeSetting ShapeValuePath="Population" />
                    </syncfusion:ShapeFileLayer.ShapeSettings>
                </syncfusion:ShapeFileLayer>
            </syncfusion:SfMap.Layers>
        </syncfusion:SfMap>
    </Grid>
</Window>
```

### Dynamic Data Updates

ObservableCollection automatically notifies the map of collection changes:

```csharp
// Add item
Countries.Add(new CountryData { Name = "Brazil", Population = 212000000, GDP = 1.84 });

// Remove item
Countries.Remove(Countries.First(c => c.Name == "Brazil"));

// Clear all
Countries.Clear();
```

Individual property changes require INotifyPropertyChanged:

```csharp
// Update population - map updates automatically
Countries[0].Population = 335000000;
```

## Simple List Binding

```csharp
List<StateData> states = new List<StateData>
{
    new StateData { Name = "California", Population = 39500000 }
};

layer.ItemsSource = states;
```

## Troubleshooting Data Binding

### Issue: Shapes not colored despite binding

**Causes:**
- ShapeIDPath or ShapeIDTableField mismatch
- Case sensitivity in field names
- Missing data in collection

**Solution:**
```csharp
// Debug: Check if shapes are matched
foreach (var item in layer.ItemsSource)
{
    var id = item.GetType().GetProperty(layer.ShapeIDPath).GetValue(item);
    Debug.WriteLine($"Data ID: {id}");
}

// Verify .dbf field names match exactly
layer.ShapeIDTableField = "STATE_NAME"; // Must match .dbf field exactly
```

### Issue: Data changes not reflecting on map

**Cause:** Not using ObservableCollection or INotifyPropertyChanged

**Solution:**
```csharp
// Use ObservableCollection for collection changes
public ObservableCollection<StateData> States { get; set; }

// Implement INotifyPropertyChanged for property changes
public class StateData : INotifyPropertyChanged
{
    // ... implementation
}
```

### Issue: Null reference exception

**Cause:** Accessing property that doesn't exist

**Solution:**
```csharp
// Ensure property names are correct
layer.ShapeIDPath = "Name"; // Must match property in data class
shapeSetting.ShapeValuePath = "Population"; // Must exist in data class
```

### Issue: Some shapes match, others don't

**Cause:** Data values don't exactly match shape file values

**Solution:**
```csharp
// Normalize data before binding
Countries = new ObservableCollection<CountryData>(
    rawData.Select(c => new CountryData
    {
        Name = c.Name.Trim().ToUpper(), // Match shape file casing
        Population = c.Population
    })
);
```

## Best Practices

1. **Use ObservableCollection** for dynamic data
2. **Implement INotifyPropertyChanged** for real-time updates
3. **Verify field names** in .dbf files before binding
4. **Normalize data** to match shape file identifiers exactly
5. **Handle null values** in data properties
6. **Use async loading** for large datasets
7. **Dispose resources** properly when loading from databases/files

## Performance Considerations

- **Large datasets:** Consider filtering data before binding
- **Frequent updates:** Batch updates to minimize re-rendering
- **Complex objects:** Bind only necessary properties
- **Memory:** Clear ItemsSource when navigating away from map view

```csharp
// Efficient update pattern
layer.ItemsSource = null; // Temporarily unbind
Countries.Clear();
// ... load new data
Countries = newData;
layer.ItemsSource = Countries; // Rebind once
```
