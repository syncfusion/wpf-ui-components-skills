# Map Shape Labels in WPF Maps (SfMap)

Shape labels display text on map shapes to identify regions, countries, or areas. They can show shape names, bound data values, or custom text with full template customization support.

## Basic Label Configuration

### LabelPath Property

Specifies the property in your data source that contains the label text.

**Type:** string  
**Default:** null (no labels)

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           ItemsSource="{Binding States}"
                           ShapeIDPath="Name"
                           ShapeIDTableField="STATE_NAME"
                           LabelPath="Abbreviation" />
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";
layer.ItemsSource = viewModel.States;
layer.ShapeIDPath = "Name";
layer.ShapeIDTableField = "STATE_NAME";
layer.LabelPath = "Abbreviation";
```

### Data Model

```csharp
public class StateData
{
    public string Name { get; set; }
    public string Abbreviation { get; set; }  // Used for labels
    public int Population { get; set; }
}
```

## Custom Label Templates

Use `ShapeLabelTemplate` for complete control over label appearance.

### Basic Template

```xml
<syncfusion:ShapeFileLayer LabelPath="Name">
    <syncfusion:ShapeFileLayer.ShapeLabelTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}" 
                       FontSize="12"
                       FontWeight="Bold"
                       Foreground="Black" />
        </DataTemplate>
    </syncfusion:ShapeFileLayer.ShapeLabelTemplate>
</syncfusion:ShapeFileLayer>
```

### Styled Label Template

```xml
<syncfusion:ShapeFileLayer.ShapeLabelTemplate>
    <DataTemplate>
        <Border Background="White"
                BorderBrush="Gray"
                BorderThickness="1"
                CornerRadius="3"
                Padding="4,2">
            <TextBlock Text="{Binding}" 
                       FontSize="11"
                       FontFamily="Segoe UI"
                       Foreground="DarkBlue" />
        </Border>
    </DataTemplate>
</syncfusion:ShapeFileLayer.ShapeLabelTemplate>
```

### Multi-Line Label Template

```xml
<syncfusion:ShapeFileLayer.ShapeLabelTemplate>
    <DataTemplate>
        <StackPanel>
            <TextBlock Text="{Binding Name}" 
                       FontWeight="Bold" 
                       FontSize="13"
                       Foreground="Black"
                       HorizontalAlignment="Center" />
            <TextBlock Text="{Binding Population, StringFormat='Pop: {0:N0}'}" 
                       FontSize="10"
                       Foreground="Gray"
                       HorizontalAlignment="Center" />
        </StackPanel>
    </DataTemplate>
</syncfusion:ShapeFileLayer.ShapeLabelTemplate>
```

## Label Positioning

Labels are automatically centered on shapes. For custom positioning, use margins in the template.

```xml
<DataTemplate>
    <TextBlock Text="{Binding}" 
               Margin="0,10,0,0"  <!-- Offset from center -->
               FontSize="12" />
</DataTemplate>
```

## Label Visibility Control

### Hide Labels for Specific Shapes

Use a converter to control visibility:

```csharp
public class PopulationVisibilityConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is int population)
        {
            return population > 1000000 ? Visibility.Visible : Visibility.Collapsed;
        }
        return Visibility.Collapsed;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

```xml
<Window.Resources>
    <local:PopulationVisibilityConverter x:Key="PopulationVisibilityConverter" />
</Window.Resources>

<syncfusion:ShapeFileLayer.ItemsTemplate>
    <DataTemplate>
        <TextBlock Text="{Binding Name}" 
                   Visibility="{Binding Population, Converter={StaticResource PopulationVisibilityConverter}}"
                   FontSize="11" />
    </DataTemplate>
</syncfusion:ShapeFileLayer.ItemsTemplate>
```

### Zoom-Based Visibility

Labels can be shown/hidden based on zoom level using triggers or converters.

## Label Styling Examples

### Example 1: Simple State Labels

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           ItemsSource="{Binding States}"
                           ShapeIDPath="Name"
                           ShapeIDTableField="STATE_NAME"
                           LabelPath="Abbreviation">
    <syncfusion:ShapeFileLayer.ItemsTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}" 
                       FontSize="14"
                       FontWeight="SemiBold"
                       FontFamily="Arial"
                       Foreground="White">
                <TextBlock.Effect>
                    <DropShadowEffect Color="Black" 
                                      ShadowDepth="2" 
                                      BlurRadius="3" 
                                      Opacity="0.7" />
                </TextBlock.Effect>
            </TextBlock>
        </DataTemplate>
    </syncfusion:ShapeFileLayer.ItemsTemplate>
</syncfusion:ShapeFileLayer>
```

### Example 2: Country Labels with Population

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                           ItemsSource="{Binding Countries}"
                           ShapeIDPath="Name"
                           ShapeIDTableField="NAME"
                           LabelPath="Name">
    <syncfusion:ShapeFileLayer.ItemsTemplate>
        <DataTemplate>
            <StackPanel Background="#80FFFFFF" Padding="5,2">
                <TextBlock Text="{Binding Name}" 
                           FontWeight="Bold" 
                           FontSize="12"
                           Foreground="#212121" />
                <TextBlock Text="{Binding Population, StringFormat='{}{0:N0}'}" 
                           FontSize="10"
                           Foreground="#757575" />
            </StackPanel>
        </DataTemplate>
    </syncfusion:ShapeFileLayer.ItemsTemplate>
</syncfusion:ShapeFileLayer>
```

### Example 3: Colored Background Labels

```xml
<syncfusion:ShapeFileLayer.ItemsTemplate>
    <DataTemplate>
        <Border Background="#E3F2FD"
                BorderBrush="#1976D2"
                Padding="6,3">
            <TextBlock Text="{Binding}" 
                       FontSize="11"
                       FontWeight="Medium"
                       Foreground="#1976D2" />
        </Border>
    </DataTemplate>
</syncfusion:ShapeFileLayer.ItemsTemplate>
```

### Example 4: Icon + Text Labels

```xml
<syncfusion:ShapeFileLayer.ItemsTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal" Background="White" Padding="4,2">
            <Path Data="M12,2 L15,8 L22,9 L17,14 L18,21 L12,18 L6,21 L7,14 L2,9 L9,8 Z"
                  Fill="Gold"
                  Width="12"
                  Height="12"
                  Stretch="Uniform"
                  Margin="0,0,4,0" />
            <TextBlock Text="{Binding Name}" 
                       FontSize="11"
                       VerticalAlignment="Center" />
        </StackPanel>
    </DataTemplate>
</syncfusion:ShapeFileLayer.ItemsTemplate>
```

## Label Font Customization

```xml
<DataTemplate>
    <TextBlock Text="{Binding}" 
               FontFamily="Segoe UI"
               FontSize="13"
               FontWeight="Bold"
               FontStyle="Italic"
               Foreground="#1976D2" />
</DataTemplate>
```

## Complete Working Example

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF"
        xmlns:local="clr-namespace:MyApp">
    <Grid>
        <Grid.DataContext>
            <local:StateViewModel />
        </Grid.DataContext>

        <syncfusion:SfMap>
            <syncfusion:SfMap.Layers>
                <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                                           ItemsSource="{Binding States}"
                                           ShapeIDPath="Name"
                                           ShapeIDTableField="STATE_NAME"
                                           LabelPath="Abbreviation">
                    
                    <syncfusion:ShapeFileLayer.ShapeSettings>
                        <syncfusion:ShapeSetting ShapeFill="LightBlue"
                                                 ShapeStroke="White"
                                                 ShapeStrokeThickness="1" />
                    </syncfusion:ShapeFileLayer.ShapeSettings>
                    
                    <syncfusion:ShapeFileLayer.ItemsTemplate>
                        <DataTemplate>
                            <TextBlock Text="{Binding}" 
                                       FontSize="12"
                                       FontWeight="Bold"
                                       Foreground="DarkBlue">
                                <TextBlock.Effect>
                                    <DropShadowEffect Color="White" 
                                                      ShadowDepth="0" 
                                                      BlurRadius="4" />
                                </TextBlock.Effect>
                            </TextBlock>
                        </DataTemplate>
                    </syncfusion:ShapeFileLayer.ItemsTemplate>
                </syncfusion:ShapeFileLayer>
            </syncfusion:SfMap.Layers>
        </syncfusion:SfMap>
    </Grid>
</Window>
```

```csharp
public class StateData
{
    public string Name { get; set; }
    public string Abbreviation { get; set; }
    public int Population { get; set; }
}

public class StateViewModel
{
    public ObservableCollection<StateData> States { get; set; }

    public StateViewModel()
    {
        States = new ObservableCollection<StateData>
        {
            new StateData { Name = "California", Abbreviation = "CA", Population = 39500000 },
            new StateData { Name = "Texas", Abbreviation = "TX", Population = 29000000 },
            new StateData { Name = "Florida", Abbreviation = "FL", Population = 21500000 }
        };
    }
}
```

## Best Practices

1. **Readability** - Use sufficient font size (11-14px) and high contrast colors
2. **Shadow effects** - Add drop shadows for labels over varied backgrounds
3. **Short text** - Use abbreviations or short names to avoid clutter
4. **Selective display** - Show labels only for larger/important shapes
5. **Background** - Add semi-transparent backgrounds for better readability
6. **Font choice** - Use clear, sans-serif fonts like Segoe UI or Arial
7. **Performance** - Minimize complex templates for maps with many shapes

## Troubleshooting

### Labels not appearing

**Check:**
- LabelPath is set and matches property name in data source
- Property value is not null or empty
- TextBlock has sufficient size (not collapsed)
- Foreground color contrasts with background

### Labels obscured by shapes

**Solution:**
- Add DropShadowEffect for contrast
- Use Border with Background in template
- Increase FontWeight to Bold or SemiBold

### Text cut off or clipped

**Adjust:**
- Increase Padding in Border/StackPanel
- Verify template Width/Height allow enough space
- Check for restrictive parent container constraints

### Labels overlapping

**Consider:**
- Using shorter text (abbreviations)
- Hiding labels for smaller shapes
- Adjusting font size
- Implementing zoom-based visibility
