# Getting Started with WPF TreeMap

This guide covers installation, setup, and creating your first TreeMap control in a WPF application.

## Assembly and Namespace

To use the Syncfusion WPF TreeMap control, reference the following:

**Assembly:** `Syncfusion.SfTreeMap.WPF`  
**Namespace:** `Syncfusion.UI.Xaml.TreeMap`

Add the namespace to your XAML:
```xaml
xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.TreeMap;assembly=Syncfusion.SfTreeMap.WPF"
```

## Creating TreeMap Control

### Method 1: Through Visual Studio

1. Open your WPF project in Visual Studio
2. Open the Toolbox
3. Drag `SfTreeMap` from the Toolbox to the designer
4. The control will be automatically added to your XAML with the required namespace

### Method 2: Through Expression Blend

1. Create a WPF project in Expression Blend
2. Add reference to `Syncfusion.SfTreeMap.WPF` assembly
3. Search for `SfTreeMap` in the Toolbox
4. Drag and drop it to the designer
5. The control will be generated with default configuration

### Method 3: Through XAML and C# (Recommended)

This method gives you full control over the TreeMap configuration.

**XAML:**
```xaml
<Window x:Class="TreeMapDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.TreeMap;assembly=Syncfusion.SfTreeMap.WPF"
        Title="TreeMap Demo" WindowState="Maximized">
    <Grid>
        <syncfusion:SfTreeMap>
        </syncfusion:SfTreeMap>
    </Grid>
</Window>
```

**C#:**
```csharp
SfTreeMap treemap = new SfTreeMap()
{
    Height = 300,
    Width = 300,
};
```

## Setting Up Data Source

TreeMap requires a data source (ItemsSource) to visualize. Here's how to create a ViewModel with sample data:

### Step 1: Create Data Model

```csharp
public class PopulationDetail
{
    public string Continent { get; set; }
    public string Country { get; set; }
    public double Growth { get; set; }
    public double Population { get; set; }
}
```

### Step 2: Create ViewModel

```csharp
public class PopulationViewModel
{
    public ObservableCollection<PopulationDetail> PopulationDetails { get; set; }
    
    public PopulationViewModel()
    {
        this.PopulationDetails = new ObservableCollection<PopulationDetail>();
        
        PopulationDetails.Add(new PopulationDetail() { Continent = "Asia", Country = "Indonesia", Growth = 3, Population = 237641326 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "Asia", Country = "Russia", Growth = 2, Population = 152518015 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "Asia", Country = "Malaysia", Growth = 1, Population = 29672000 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "North America", Country = "United States", Growth = 4, Population = 315645000 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "North America", Country = "Mexico", Growth = 2, Population = 112336538 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "North America", Country = "Canada", Growth = 1, Population = 35056064 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "South America", Country = "Colombia", Growth = 1, Population = 47000000 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "South America", Country = "Brazil", Growth = 3, Population = 193946886 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "Africa", Country = "Nigeria", Growth = 2, Population = 170901000 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "Africa", Country = "Egypt", Growth = 1, Population = 83661000 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "Europe", Country = "Germany", Growth = 1, Population = 81993000 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "Europe", Country = "France", Growth = 1, Population = 65605000 });
        PopulationDetails.Add(new PopulationDetail() { Continent = "Europe", Country = "UK", Growth = 1, Population = 63181775 });
    }
}
```

### Step 3: Configure TreeMap in XAML

```xaml
<Grid>
    <Grid.DataContext>
        <local:PopulationViewModel/>
    </Grid.DataContext>
    
    <syncfusion:SfTreeMap Name="TreeMap" 
                          ItemsSource="{Binding PopulationDetails}" 
                          ItemsLayoutMode="Squarified" 
                          WeightValuePath="Population"/>
        
        <syncfusion:SfTreeMap.Levels>
            <syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                                         GroupGap="5" 
                                         HeaderHeight="30">
                <syncfusion:TreeMapFlatLevel.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Header}" 
                                  Foreground="#D6D6D6" 
                                  FontSize="18" 
                                  FontWeight="Light" 
                                  HorizontalAlignment="Left" 
                                  VerticalAlignment="Center"/>
                    </DataTemplate>
                </syncfusion:TreeMapFlatLevel.HeaderTemplate>
            </syncfusion:TreeMapFlatLevel>
        </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

### Step 4: Set DataContext in Code-Behind (Alternative)

```csharp
this.TreeMap.DataContext = new PopulationViewModel();
```

## Complete Working Example

Here's a complete, minimal example that creates a functional TreeMap:

```xaml
<Window x:Class="TreeMapDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:TreeMapDemo"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.TreeMap;assembly=Syncfusion.SfTreeMap.WPF"
        Title="TreeMap Demo" Height="600" Width="800">
    <Grid>
        <Grid.DataContext>
            <local:PopulationViewModel/>
        </Grid.DataContext>
        
        <syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                              WeightValuePath="Population"
                              ItemsLayoutMode="Squarified">
            <syncfusion:SfTreeMap.Levels>
                <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
            </syncfusion:SfTreeMap.Levels>
        </syncfusion:SfTreeMap>
    </Grid>
</Window>
```

## Theme Support

TreeMap supports Syncfusion's built-in themes for consistent styling across your application.

### Applying Themes with SfSkinManager

1. Add reference to `Syncfusion.SfSkinManager.WPF`
2. Apply a theme to your application or specific control:

```csharp
// Apply theme to entire window
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));

// Apply theme to TreeMap only
SfSkinManager.SetTheme(treeMap, new Theme("MaterialDark"));
```

## Key Properties for Initial Setup

- **ItemsSource** - Bind to your data collection
- **WeightValuePath** - Property name that determines rectangle sizes (e.g., "Population")
- **ColorValuePath** - Property name used for color mapping (e.g., "Growth")
- **ItemsLayoutMode** - Layout algorithm (Squarified, SliceAndDiceAuto, etc.)

## Next Steps

Once you have a basic TreeMap working:
1. **Customize layouts** - See `layout-modes.md` for different layout algorithms
2. **Configure colors** - See `color-mapping.md` for color mapping strategies
3. **Add hierarchy** - See `levels-and-hierarchy.md` for multi-level grouping
4. **Enable interaction** - See `interactive-features.md` for selection, tooltips, and drill-down

## Troubleshooting

**TreeMap appears empty:**
- Verify ItemsSource is bound correctly
- Ensure WeightValuePath points to a numeric property
- Check that data collection contains items

**Rectangles not sized correctly:**
- Verify WeightValuePath property name matches your data model
- Ensure weight values are positive numbers
- Check for zero or null values in weight property

**Namespace errors:**
- Ensure Syncfusion.SfTreeMap.WPF assembly is referenced
- Verify namespace declaration in XAML matches the assembly
- Clean and rebuild the project if IntelliSense doesn't recognize the control
