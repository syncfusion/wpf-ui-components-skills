# Data Binding

## Overview

TileViewControl supports data binding to populate tiles from data sources. This enables dynamic tile creation based on collections, MVVM patterns, and complex data scenarios.

## Basic Collection Binding

### ItemsSource Binding

The most common approach is to bind the `ItemsSource` property to a collection:

```xaml
<syncfusion:TileViewControl ItemsSource="{Binding Tiles}">
    <!-- ItemTemplate defines how each item is displayed -->
</syncfusion:TileViewControl>
```

### ViewModel Setup

```csharp
public class TileViewModel
{
    private ObservableCollection<TileData> tiles;
    
    public ObservableCollection<TileData> Tiles
    {
        get { return tiles; }
        set { tiles = value; }
    }
    
    public TileViewModel()
    {
        Tiles = new ObservableCollection<TileData>
        {
            new TileData { Title = "Sales", Content = "Q4 Sales Data" },
            new TileData { Title = "Marketing", Content = "Campaign Status" },
            new TileData { Title = "Finance", Content = "Budget Report" }
        };
    }
}

public class TileData
{
    public string Title { get; set; }
    public string Content { get; set; }
}
```

### Window Setup

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:TileViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:TileViewControl ItemsSource="{Binding Tiles}">
            <syncfusion:TileViewControl.ItemTemplate>
                <DataTemplate>
                    <Grid>
                        <TextBlock Text="{Binding Content}"/>
                    </Grid>
                </DataTemplate>
            </syncfusion:TileViewControl.ItemTemplate>
        </syncfusion:TileViewControl>
    </Grid>
</Window>
```

## ItemTemplate

The ItemTemplate defines how data objects are rendered as tiles.

### Simple Template

```xaml
<syncfusion:TileViewControl ItemsSource="{Binding Tiles}">
    <syncfusion:TileViewControl.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Content}" 
                       VerticalAlignment="Center"
                       HorizontalAlignment="Center"/>
        </DataTemplate>
    </syncfusion:TileViewControl.ItemTemplate>
</syncfusion:TileViewControl>
```

### Rich Template

```xaml
<syncfusion:TileViewControl ItemsSource="{Binding Tiles}">
    <syncfusion:TileViewControl.ItemTemplate>
        <DataTemplate>
            <Grid Background="White">
                <StackPanel Margin="10">
                    <TextBlock Text="{Binding Title}" 
                               FontSize="14" 
                               FontWeight="Bold"
                               Foreground="#333"/>
                    <TextBlock Text="{Binding Content}" 
                               Margin="0,5,0,0"
                               Foreground="#666"
                               TextWrapping="Wrap"/>
                    <Button Content="View Details" 
                            Margin="0,10,0,0"/>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </syncfusion:TileViewControl.ItemTemplate>
</syncfusion:TileViewControl>
```

## HeaderTemplate

Customize the tile header using HeaderTemplate.

### Header with Icon

```xaml
<syncfusion:TileViewControl ItemsSource="{Binding Tiles}">
    <syncfusion:TileViewControl.HeaderTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="{Binding IconSource}" 
                       Width="16" 
                       Height="16"/>
                <TextBlock Text="{Binding Title}" 
                           Margin="5,0"
                           VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:TileViewControl.HeaderTemplate>
    
    <syncfusion:TileViewControl.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Content}"/>
        </DataTemplate>
    </syncfusion:TileViewControl.ItemTemplate>
</syncfusion:TileViewControl>
```

### Data Model with Icon

```csharp
public class TileData
{
    public string Title { get; set; }
    public string Content { get; set; }
    public string IconSource { get; set; }
}

// Usage
Tiles = new ObservableCollection<TileData>
{
    new TileData 
    { 
        Title = "Sales", 
        Content = "Revenue Data",
        IconSource = "pack://application:,,,/Assets/sales-icon.png"
    }
};
```

## Complete Data Binding Example

### ViewModel

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class DashboardViewModel : INotifyPropertyChanged
{
    private ObservableCollection<DashboardTile> tiles;
    
    public ObservableCollection<DashboardTile> Tiles
    {
        get { return tiles; }
        set 
        { 
            tiles = value;
            OnPropertyChanged(nameof(Tiles));
        }
    }
    
    public DashboardViewModel()
    {
        Tiles = new ObservableCollection<DashboardTile>
        {
            new DashboardTile 
            { 
                Title = "Sales Dashboard",
                Metric = "$45,230",
                Trend = "+12% from last month"
            },
            new DashboardTile 
            { 
                Title = "Active Users",
                Metric = "1,234",
                Trend = "+5% from last week"
            },
            new DashboardTile 
            { 
                Title = "System Status",
                Metric = "99.9% Uptime",
                Trend = "No incidents"
            }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}

public class DashboardTile
{
    public string Title { get; set; }
    public string Metric { get; set; }
    public string Trend { get; set; }
}
```

### XAML

```xaml
<Window x:Class="DataBindingApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:DataBindingApp"
        Title="Dashboard" Height="500" Width="700">
    
    <Window.DataContext>
        <local:DashboardViewModel/>
    </Window.DataContext>
    
    <Grid Background="#F5F5F5">
        <syncfusion:TileViewControl ItemsSource="{Binding Tiles}"
                                    AllowDragDrop="True"
                                    Margin="10">
            
            <syncfusion:TileViewControl.HeaderTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding Title}" 
                               Foreground="White"
                               FontWeight="Bold"/>
                </DataTemplate>
            </syncfusion:TileViewControl.HeaderTemplate>
            
            <syncfusion:TileViewControl.ItemTemplate>
                <DataTemplate>
                    <Grid Background="White">
                        <StackPanel Margin="15">
                            <TextBlock Text="{Binding Metric}" 
                                       FontSize="28"
                                       FontWeight="Bold"
                                       Foreground="#0078D4"/>
                            <TextBlock Text="{Binding Trend}" 
                                       Margin="0,5,0,0"
                                       Foreground="#999"
                                       FontSize="12"/>
                        </StackPanel>
                    </Grid>
                </DataTemplate>
            </syncfusion:TileViewControl.ItemTemplate>
        </syncfusion:TileViewControl>
    </Grid>
</Window>
```

## Dynamic Updates

### Adding Items

```csharp
var newTile = new DashboardTile 
{ 
    Title = "New Metric",
    Metric = "100",
    Trend = "Stable"
};
viewModel.Tiles.Add(newTile);
```

### Removing Items

```csharp
// Remove first tile
viewModel.Tiles.RemoveAt(0);

// Or remove specific item
var tileToRemove = viewModel.Tiles.FirstOrDefault(t => t.Title == "Old Metric");
if (tileToRemove != null)
{
    viewModel.Tiles.Remove(tileToRemove);
}
```

### Updating Items

```csharp
// Update existing tile
var tile = viewModel.Tiles[0];
tile.Metric = "$50,000"; // Property change automatically updates UI if using INotifyPropertyChanged
```

### Clearing All Items

```csharp
viewModel.Tiles.Clear();
```

## Master-Detail Binding

### Two-TileView Master-Detail Pattern

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    
    <!-- Master TileView -->
    <syncfusion:TileViewControl Grid.Column="0"
                                ItemsSource="{Binding Categories}"
                                SelectedItem="{Binding SelectedCategory}">
        <syncfusion:TileViewControl.ItemTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding Name}"/>
            </DataTemplate>
        </syncfusion:TileViewControl.ItemTemplate>
    </syncfusion:TileViewControl>
    
    <!-- Detail TileView -->
    <syncfusion:TileViewControl Grid.Column="1"
                                ItemsSource="{Binding SelectedCategory.Items}">
        <syncfusion:TileViewControl.ItemTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="{Binding ItemName}"/>
                </Grid>
            </DataTemplate>
        </syncfusion:TileViewControl.ItemTemplate>
    </syncfusion:TileViewControl>
</Grid>
```

## Styling Data-Bound Items

### Converter for Dynamic Styling

```csharp
public class StatusToColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        string status = value?.ToString();
        return status switch
        {
            "Active" => new SolidColorBrush(Colors.Green),
            "Inactive" => new SolidColorBrush(Colors.Red),
            "Pending" => new SolidColorBrush(Colors.Orange),
            _ => new SolidColorBrush(Colors.Gray)
        };
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Use in Template

```xaml
<Window.Resources>
    <local:StatusToColorConverter x:Key="StatusToColorConverter"/>
</Window.Resources>

<syncfusion:TileViewControl.ItemTemplate>
    <DataTemplate>
        <Grid Background="{Binding Status, Converter={StaticResource StatusToColorConverter}}">
            <TextBlock Text="{Binding Content}"/>
        </Grid>
    </DataTemplate>
</syncfusion:TileViewControl.ItemTemplate>
```

## Tips and Best Practices

1. **Use ObservableCollection** - Enables automatic UI updates
2. **Implement INotifyPropertyChanged** - For property-level updates
3. **Test with large datasets** - Ensure performance with many items
4. **Use DataTemplateSelector** - For different templates based on data type
5. **Keep templates efficient** - Complex templates may impact scroll performance
