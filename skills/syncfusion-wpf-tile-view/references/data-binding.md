# Data Binding

## Overview

TileViewControl supports data binding to populate tiles from data sources using `ItemsSource`, enabling dynamic tile creation based on collections and MVVM patterns.

## Basic Collection Binding

**Bind ItemsSource to a collection:**
```xaml
<syncfusion:TileViewControl ItemsSource="{Binding Tiles}">
    <syncfusion:TileViewControl.ItemTemplate>
        <DataTemplate>
            <Grid>
                <TextBlock Text="{Binding Content}"/>
            </Grid>
        </DataTemplate>
    </syncfusion:TileViewControl.ItemTemplate>
</syncfusion:TileViewControl>
```

**ViewModel with ObservableCollection:**
```csharp
public class TileViewModel
{
    public ObservableCollection<TileData> Tiles { get; set; }
    
    public TileViewModel()
    {
        Tiles = new ObservableCollection<TileData>
        {
            new TileData { Title = "Sales", Content = "Q4 Data" },
            new TileData { Title = "Marketing", Content = "Campaign Status" }
        };
    }
}

public class TileData
{
    public string Title { get; set; }
    public string Content { get; set; }
}
```

**Set DataContext in XAML:**
```xaml
<Window>
    <Window.DataContext>
        <local:TileViewModel/>
    </Window.DataContext>
    <!-- TileViewControl here -->
</Window>
```

## Templates

**ItemTemplate:** Defines how data objects render as tile content.

Simple text:
```xaml
<syncfusion:TileViewControl.ItemTemplate>
    <DataTemplate>
        <TextBlock Text="{Binding Content}" VerticalAlignment="Center" HorizontalAlignment="Center"/>
    </DataTemplate>
</syncfusion:TileViewControl.ItemTemplate>
```

Rich UI with controls:
```xaml
<syncfusion:TileViewControl.ItemTemplate>
    <DataTemplate>
        <Grid Background="White">
            <StackPanel Margin="10">
                <TextBlock Text="{Binding Title}" FontSize="14" FontWeight="Bold" Foreground="#333"/>
                <TextBlock Text="{Binding Content}" Margin="0,5,0,0" Foreground="#666" TextWrapping="Wrap"/>
                <Button Content="View Details" Margin="0,10,0,0"/>
            </StackPanel>
        </Grid>
    </DataTemplate>
</syncfusion:TileViewControl.ItemTemplate>
```

**HeaderTemplate:** Customize tile headers with icons and dynamic content.

```xaml
<syncfusion:TileViewControl.HeaderTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <Image Source="{Binding IconSource}" Width="16" Height="16"/>
            <TextBlock Text="{Binding Title}" Margin="5,0" VerticalAlignment="Center"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:TileViewControl.HeaderTemplate>
```

Extend data model to include icon:
```csharp
public class TileData
{
    public string Title { get; set; }
    public string Content { get; set; }
    public string IconSource { get; set; }
}
```

## Complete Example

**Dashboard ViewModel with INotifyPropertyChanged:**
```csharp
public class DashboardViewModel : INotifyPropertyChanged
{
    private ObservableCollection<DashboardTile> tiles;
    
    public ObservableCollection<DashboardTile> Tiles
    {
        get { return tiles; }
        set { tiles = value; OnPropertyChanged(nameof(Tiles)); }
    }
    
    public DashboardViewModel()
    {
        Tiles = new ObservableCollection<DashboardTile>
        {
            new DashboardTile { Title = "Sales", Metric = "$45,230", Trend = "+12%" },
            new DashboardTile { Title = "Users", Metric = "1,234", Trend = "+5%" }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}

public class DashboardTile { public string Title { get; set; } public string Metric { get; set; } public string Trend { get; set; } }
```

**Dashboard XAML:**
```xaml
<Window>
    <Window.DataContext>
        <local:DashboardViewModel/>
    </Window.DataContext>
    
    <syncfusion:TileViewControl ItemsSource="{Binding Tiles}" AllowDragDrop="True" Margin="10">
        <syncfusion:TileViewControl.HeaderTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding Title}" Foreground="White" FontWeight="Bold"/>
            </DataTemplate>
        </syncfusion:TileViewControl.HeaderTemplate>
        
        <syncfusion:TileViewControl.ItemTemplate>
            <DataTemplate>
                <Grid Background="White">
                    <StackPanel Margin="15">
                        <TextBlock Text="{Binding Metric}" FontSize="28" FontWeight="Bold" Foreground="#0078D4"/>
                        <TextBlock Text="{Binding Trend}" Margin="0,5,0,0" Foreground="#999"/>
                    </StackPanel>
                </Grid>
            </DataTemplate>
        </syncfusion:TileViewControl.ItemTemplate>
    </syncfusion:TileViewControl>
</Window>
```

## Dynamic Updates

```csharp
// Add: viewModel.Tiles.Add(new DashboardTile { Title = "New", Metric = "100" });
// Remove: viewModel.Tiles.Remove(viewModel.Tiles.FirstOrDefault(t => t.Title == "Old"));
// Update: viewModel.Tiles[0].Metric = "$50,000"; (requires INotifyPropertyChanged)
// Clear: viewModel.Tiles.Clear();
```

## Master-Detail Pattern

**Two-column layout with master-detail binding:**
```xaml
<Grid>
    <Grid.ColumnDefinitions><ColumnDefinition/><ColumnDefinition/></Grid.ColumnDefinitions>
    <syncfusion:TileViewControl Grid.Column="0" ItemsSource="{Binding Categories}" SelectedItem="{Binding SelectedCategory}">
        <syncfusion:TileViewControl.ItemTemplate><DataTemplate><TextBlock Text="{Binding Name}"/></DataTemplate></syncfusion:TileViewControl.ItemTemplate>
    </syncfusion:TileViewControl>
    <syncfusion:TileViewControl Grid.Column="1" ItemsSource="{Binding SelectedCategory.Items}">
        <syncfusion:TileViewControl.ItemTemplate><DataTemplate><TextBlock Text="{Binding ItemName}"/></DataTemplate></syncfusion:TileViewControl.ItemTemplate>
    </syncfusion:TileViewControl>
</Grid>
```

## Styling with Converters

Use value converters for dynamic styling based on data properties.

**Status-to-color converter:**
```csharp
public class StatusToColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value?.ToString() switch
        {
            "Active" => new SolidColorBrush(Colors.Green),
            "Inactive" => new SolidColorBrush(Colors.Red),
            "Pending" => new SolidColorBrush(Colors.Orange),
            _ => new SolidColorBrush(Colors.Gray)
        };
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture) 
        => throw new NotImplementedException();
}
```

**Use in template:**
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

## Best Practices

- **Use ObservableCollection** for automatic UI updates when items change
- **Implement INotifyPropertyChanged** for individual property-level updates
- **Test with large datasets** to ensure performance with many items
- **Use DataTemplateSelector** for different templates based on data type
- **Keep templates lightweight** to avoid scroll performance degradation
