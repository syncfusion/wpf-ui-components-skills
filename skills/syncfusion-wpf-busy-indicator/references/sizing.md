# Sizing - ViewboxHeight and ViewboxWidth

The SfBusyIndicator control provides `ViewboxHeight` and `ViewboxWidth` properties to control the size of the animation viewbox. This guide covers sizing strategies, responsive design, and best practices for different scenarios.

## ViewboxHeight Property

The `ViewboxHeight` property sets the height of the viewbox containing the animation.

### Basic ViewboxHeight Usage

```xml
<!-- Set specific height -->
<syncfusion:SfBusyIndicator IsBusy="True"
                             ViewboxHeight="200"
                             Header="Loading..."
                             Background="CornflowerBlue"
                             Foreground="White"/>
```

### Code-Behind Configuration

```csharp
// C#
SfBusyIndicator busyIndicator = new SfBusyIndicator();
busyIndicator.IsBusy = true;
busyIndicator.Header = "Loading...";
busyIndicator.Foreground = Brushes.White;
busyIndicator.Background = Brushes.CornflowerBlue;
busyIndicator.ViewboxHeight = 200;
```

```vb
' VB.NET
Dim busyIndicator As New SfBusyIndicator()
busyIndicator.IsBusy = True
busyIndicator.Header = "Loading..."
busyIndicator.Foreground = Brushes.White
busyIndicator.Background = Brushes.CornflowerBlue
busyIndicator.ViewboxHeight = 200
```

## ViewboxWidth Property

The `ViewboxWidth` property sets the width of the viewbox containing the animation.

### Basic ViewboxWidth Usage

```xml
<!-- Set specific width -->
<syncfusion:SfBusyIndicator IsBusy="True"
                             ViewboxWidth="50"
                             Header="Loading..."
                             Background="CornflowerBlue"
                             Foreground="White"/>
```

### Code-Behind Configuration

```csharp
// C#
SfBusyIndicator busyIndicator = new SfBusyIndicator();
busyIndicator.IsBusy = true;
busyIndicator.Header = "Loading...";
busyIndicator.Foreground = Brushes.White;
busyIndicator.Background = Brushes.CornflowerBlue;
busyIndicator.ViewboxWidth = 50;
```

```vb
' VB.NET
Dim busyIndicator As New SfBusyIndicator()
busyIndicator.IsBusy = True
busyIndicator.Header = "Loading..."
busyIndicator.Foreground = Brushes.White
busyIndicator.Background = Brushes.CornflowerBlue
busyIndicator.ViewboxWidth = 50
```

## Combined Height and Width

Set both dimensions for complete size control:

```xml
<!-- Square 150x150 viewbox -->
<syncfusion:SfBusyIndicator IsBusy="True"
                             ViewboxHeight="150"
                             ViewboxWidth="150"
                             AnimationType="Globe"/>

<!-- Rectangular 200x100 viewbox -->
<syncfusion:SfBusyIndicator IsBusy="True"
                             ViewboxHeight="100"
                             ViewboxWidth="200"
                             AnimationType="Spin"/>

<!-- Wide viewbox for specific animations -->
<syncfusion:SfBusyIndicator IsBusy="True"
                             ViewboxHeight="80"
                             ViewboxWidth="300"
                             AnimationType="Flight"/>
```

## Sizing Strategies

### Strategy 1: Fixed Sizing

Use fixed dimensions when you need consistent appearance:

```xml
<!-- Small indicator for inline use -->
<syncfusion:SfBusyIndicator ViewboxHeight="32" 
                             ViewboxWidth="32"
                             IsBusy="True"/>

<!-- Medium indicator for dialogs -->
<syncfusion:SfBusyIndicator ViewboxHeight="100" 
                             ViewboxWidth="100"
                             IsBusy="True"/>

<!-- Large indicator for full-page loading -->
<syncfusion:SfBusyIndicator ViewboxHeight="200" 
                             ViewboxWidth="200"
                             IsBusy="True"/>
```

### Strategy 2: Proportional Sizing

Size relative to container using data binding:

```xml
<Grid x:Name="MainContainer">
    <syncfusion:SfBusyIndicator IsBusy="True"
                                 ViewboxHeight="{Binding ActualHeight, 
                                                ElementName=MainContainer, 
                                                Converter={StaticResource ProportionalSizeConverter},
                                                ConverterParameter=0.3}"
                                 ViewboxWidth="{Binding ActualWidth, 
                                               ElementName=MainContainer, 
                                               Converter={StaticResource ProportionalSizeConverter},
                                               ConverterParameter=0.3}"/>
</Grid>
```

```csharp
// ProportionalSizeConverter implementation
public class ProportionalSizeConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is double size && parameter is string proportion)
        {
            if (double.TryParse(proportion, out double factor))
            {
                return size * factor;
            }
        }
        return value;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Strategy 3: Responsive Sizing

Adapt size based on window size or container:

```xml
<Grid>
    <syncfusion:SfBusyIndicator IsBusy="True">
        <syncfusion:SfBusyIndicator.Style>
            <Style TargetType="syncfusion:SfBusyIndicator">
                <!-- Default for small screens -->
                <Setter Property="ViewboxHeight" Value="80"/>
                <Setter Property="ViewboxWidth" Value="80"/>
                
                <Style.Triggers>
                    <!-- Medium screens -->
                    <DataTrigger Binding="{Binding ActualWidth, 
                                          RelativeSource={RelativeSource AncestorType=Window}}"
                                Value="800">
                        <Setter Property="ViewboxHeight" Value="120"/>
                        <Setter Property="ViewboxWidth" Value="120"/>
                    </DataTrigger>
                    
                    <!-- Large screens -->
                    <DataTrigger Binding="{Binding ActualWidth, 
                                          RelativeSource={RelativeSource AncestorType=Window}}"
                                Value="1200">
                        <Setter Property="ViewboxHeight" Value="180"/>
                        <Setter Property="ViewboxWidth" Value="180"/>
                    </DataTrigger>
                </Style.Triggers>
            </Style>
        </syncfusion:SfBusyIndicator.Style>
    </syncfusion:SfBusyIndicator>
</Grid>
```

## Common Sizing Scenarios

### Scenario 1: Full-Page Overlay

Large indicator covering entire page:

```xml
<Grid>
    <!-- Main content -->
    <ContentControl Content="{Binding MainContent}"/>
    
    <!-- Overlay with large busy indicator -->
    <Grid Background="#80000000" 
          Visibility="{Binding IsBusy, Converter={StaticResource BoolToVisibilityConverter}}">
        <syncfusion:SfBusyIndicator IsBusy="{Binding IsBusy}"
                                     ViewboxHeight="250"
                                     ViewboxWidth="250"
                                     AnimationType="DoubleCircle"
                                     Header="Loading Application..."
                                     Foreground="White"/>
    </Grid>
</Grid>
```

### Scenario 2: Dialog/Modal Indicator

Medium-sized indicator for dialogs:

```xml
<Window Title="Processing" Height="300" Width="400">
    <Grid Background="WhiteSmoke">
        <syncfusion:SfBusyIndicator IsBusy="True"
                                     ViewboxHeight="120"
                                     ViewboxWidth="120"
                                     AnimationType="Spin"
                                     Header="Processing your request..."/>
    </Grid>
</Window>
```

### Scenario 3: Inline Content Loader

Small indicator within content area:

```xml
<StackPanel>
    <TextBlock Text="User Details" FontSize="18" FontWeight="Bold"/>
    
    <Border BorderBrush="Gray" BorderThickness="1" Padding="10" Margin="0,10,0,0">
        <Grid Height="200">
            <ContentControl Content="{Binding UserDetails}" 
                           Visibility="{Binding IsLoading, 
                                      Converter={StaticResource InverseBoolToVisibilityConverter}}"/>
            
            <syncfusion:SfBusyIndicator IsBusy="{Binding IsLoading}"
                                         ViewboxHeight="60"
                                         ViewboxWidth="60"
                                         AnimationType="Ball"
                                         Header="Loading details..."/>
        </Grid>
    </Border>
</StackPanel>
```

### Scenario 4: Button-Integrated Indicator

Tiny indicator next to or in buttons:

```xml
<Button Width="120" Height="35">
    <StackPanel Orientation="Horizontal">
        <syncfusion:SfBusyIndicator IsBusy="{Binding IsSubmitting}"
                                     ViewboxHeight="16"
                                     ViewboxWidth="16"
                                     Margin="0,0,8,0"/>
        <TextBlock Text="Submit" VerticalAlignment="Center"/>
    </StackPanel>
</Button>
```

### Scenario 5: List Item Loader

Individual item loading indicators:

```xml
<ListBox ItemsSource="{Binding Items}">
    <ListBox.ItemTemplate>
        <DataTemplate>
            <Grid Height="50">
                <TextBlock Text="{Binding Name}" 
                          Visibility="{Binding IsLoaded, 
                                     Converter={StaticResource BoolToVisibilityConverter}}"/>
                
                <syncfusion:SfBusyIndicator IsBusy="{Binding IsLoading}"
                                             ViewboxHeight="30"
                                             ViewboxWidth="30"
                                             AnimationType="SingleCircle"/>
            </Grid>
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

## Aspect Ratio Considerations

Most animations work best with square dimensions:

```xml
<!-- Recommended: Square viewbox -->
<syncfusion:SfBusyIndicator ViewboxHeight="100" ViewboxWidth="100"/>

<!-- Caution: Rectangular may distort some animations -->
<syncfusion:SfBusyIndicator ViewboxHeight="100" ViewboxWidth="200"/>
```

Some animations (like Flight, Rain) work well with rectangular viewboxes, while others (Ball, Globe, DoubleCircle) are designed for square dimensions.

## Layout and Container Constraints

### Constrained Containers

When the container has limited space:

```xml
<!-- Container with max dimensions -->
<Border MaxHeight="150" MaxWidth="150">
    <syncfusion:SfBusyIndicator IsBusy="True"
                                 ViewboxHeight="140"
                                 ViewboxWidth="140"/>
</Border>
```

### Scrollable Containers

Center indicator in scrollable areas:

```xml
<ScrollViewer>
    <Grid MinHeight="800">
        <syncfusion:SfBusyIndicator IsBusy="True"
                                     ViewboxHeight="100"
                                     ViewboxWidth="100"
                                     VerticalAlignment="Center"
                                     HorizontalAlignment="Center"/>
    </Grid>
</ScrollViewer>
```

### Dynamic Containers

Adjust size based on available space:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <TextBlock Grid.Row="0" Text="Header" FontSize="24"/>
    
    <Grid Grid.Row="1" x:Name="ContentArea">
        <syncfusion:SfBusyIndicator IsBusy="True"
                                     ViewboxHeight="{Binding ActualHeight, 
                                                    ElementName=ContentArea,
                                                    Converter={StaticResource MaxSizeConverter},
                                                    ConverterParameter=0.5}"
                                     ViewboxWidth="{Binding ActualWidth, 
                                                   ElementName=ContentArea,
                                                   Converter={StaticResource MaxSizeConverter},
                                                   ConverterParameter=0.5}"/>
    </Grid>
</Grid>
```

## Best Practices

1. **Square dimensions for most animations** - Keeps animations proportional and undistorted
2. **Size relative to importance** - Larger for critical operations, smaller for background tasks
3. **Consider container size** - Don't make indicators larger than their containers
4. **Test different screen sizes** - Ensure indicators are visible but not overwhelming
5. **Leave room for headers** - Account for header height when sizing
6. **Match application scale** - Use consistent sizing across similar scenarios
7. **Consider accessibility** - Ensure indicators are large enough to see clearly
8. **Don't oversize** - Very large indicators can be distracting

## Recommended Sizes by Use Case

| Use Case | ViewboxHeight | ViewboxWidth | Notes |
|----------|---------------|--------------|-------|
| **Button inline** | 16-24 | 16-24 | Matches button text height |
| **Small widget** | 32-50 | 32-50 | For sidebars, cards |
| **Content area** | 80-120 | 80-120 | Default for most scenarios |
| **Dialog/Modal** | 100-150 | 100-150 | Prominent but not overwhelming |
| **Full-page overlay** | 200-300 | 200-300 | Highly visible, primary focus |
| **List item** | 24-40 | 24-40 | Fits within list row height |

## Dynamic Sizing with View Model

Control sizing through view model properties:

```csharp
public class SizableIndicatorViewModel : ViewModelBase
{
    private double _indicatorSize = 100;
    public double IndicatorSize
    {
        get => _indicatorSize;
        set => SetProperty(ref _indicatorSize, value);
    }
    
    public void AdjustSize(string sizeCategory)
    {
        IndicatorSize = sizeCategory switch
        {
            "Small" => 60,
            "Medium" => 100,
            "Large" => 160,
            "XLarge" => 250,
            _ => 100
        };
    }
}
```

```xml
<syncfusion:SfBusyIndicator IsBusy="True"
                             ViewboxHeight="{Binding IndicatorSize}"
                             ViewboxWidth="{Binding IndicatorSize}"/>
```

## Troubleshooting

**Issue:** Indicator appears too small or not visible
- **Solution:** Increase ViewboxHeight and ViewboxWidth values

**Issue:** Indicator is cut off by container
- **Solution:** Ensure container has sufficient size or adjust viewbox dimensions

**Issue:** Animation appears distorted
- **Solution:** Use square dimensions (equal height and width) for most animations

**Issue:** Indicator doesn't resize with window
- **Solution:** Implement responsive sizing with data binding or triggers

**Issue:** Header text overlaps animation
- **Solution:** Increase ViewboxHeight or reduce animation size to leave room for header

## GitHub Samples

Explore sizing examples:
https://github.com/SyncfusionExamples/wpf-BusyIndicator-examples/tree/master/Samples/Sizing

## Related Topics

- **Animation Types** - Different animations may look better at different sizes
- **Header Customization** - Consider header space when sizing
- **Getting Started** - Basic setup before implementing custom sizing
