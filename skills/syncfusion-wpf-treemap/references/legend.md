# Legend

This guide covers adding and customizing legends in TreeMap to explain color mappings and provide visual reference.

## ShowLegend

Enable or disable legend display.

**Basic Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population"
                      ColorValuePath="Growth"
                      ShowLegend="True">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:RangeBrushColorMapping>
            <syncfusion:RangeBrushColorMapping.Brushes>
                <syncfusion:RangeBrush From="0" To="1" Color="#A4C400" LegendLabel="Low (0-1)"/>
                <syncfusion:RangeBrush From="1" To="2" Color="#AA00FF" LegendLabel="Medium (1-2)"/>
                <syncfusion:RangeBrush From="2" To="3" Color="#F0A30A" LegendLabel="High (2-3)"/>
                <syncfusion:RangeBrush From="3" To="4" Color="#1BA1E2" LegendLabel="Very High (3-4)"/>
            </syncfusion:RangeBrushColorMapping.Brushes>
        </syncfusion:RangeBrushColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
</syncfusion:SfTreeMap>
```

## Legend Types

### LegendType

Control how the legend is displayed.

**Supported values:**
- **Gradient** - Shows a continuous color gradient
- **DockPosition** - Shows discrete legend items

**Usage:**
```xaml
<syncfusion:SfTreeMap ShowLegend="True" 
                      LegendType="DockPosition">
</syncfusion:SfTreeMap>
```

## Legend Sizing

### LegendHeight and LegendWidth

Specify legend dimensions.

**Usage:**
```xaml
<syncfusion:SfTreeMap ShowLegend="True"
                      LegendHeight="100"
                      LegendWidth="200">
</syncfusion:SfTreeMap>
```

**Recommendations:**
- **Height:** 80-120 pixels for horizontal legends
- **Width:** 150-250 pixels for vertical legends
- Auto-size if not specified

## Legend Positioning

The legend position depends on the LegendType and available space. Typically appears:
- Bottom of TreeMap (horizontal orientation)
- Right side of TreeMap (vertical orientation)

## LegendTemplate

Customize legend appearance with a DataTemplate.

### Basic LegendTemplate

```xaml
<syncfusion:SfTreeMap ShowLegend="True">
    <syncfusion:SfTreeMap.LegendTemplate>
        <DataTemplate>
            <Border Background="White" 
                    BorderBrush="Gray" 
                    BorderThickness="1" 
                    Padding="10"
                    Margin="5">
                <StackPanel>
                    <TextBlock Text="Legend" 
                              FontWeight="Bold" 
                              FontSize="14" 
                              Margin="0,0,0,10"/>
                    <ItemsControl ItemsSource="{Binding}">
                        <ItemsControl.ItemTemplate>
                            <DataTemplate>
                                <StackPanel Orientation="Horizontal" 
                                           Margin="0,3">
                                    <Rectangle Width="20" 
                                              Height="20" 
                                              Fill="{Binding Color}" 
                                              Margin="0,0,10,0"/>
                                    <TextBlock Text="{Binding Label}" 
                                              VerticalAlignment="Center"/>
                                </StackPanel>
                            </DataTemplate>
                        </ItemsControl.ItemTemplate>
                    </ItemsControl>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeMap.LegendTemplate>
</syncfusion:SfTreeMap>
```

### Styled LegendTemplate

```xaml
<syncfusion:SfTreeMap.LegendTemplate>
    <DataTemplate>
        <Border Background="#2C3E50" 
                BorderBrush="#34495E" 
                BorderThickness="2" 
                CornerRadius="5"
                Padding="15"
                Margin="10">
            <!-- Legend Title -->
            <StackPanel>
                <TextBlock Text="Growth Rate" 
                          FontWeight="Bold" 
                          FontSize="16" 
                          Foreground="White"
                          Margin="0,0,0,10"/>
                
                <!-- Legend Items -->
                <ItemsControl ItemsSource="{Binding}">
                    <ItemsControl.ItemTemplate>
                        <DataTemplate>
                            <Grid Margin="0,5">
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="Auto"/>
                                    <ColumnDefinition Width="*"/>
                                </Grid.ColumnDefinitions>
                                
                                <!-- Color Box -->
                                <Border Grid.Column="0" 
                                        Width="30" 
                                        Height="20" 
                                        Background="{Binding Color}"
                                        BorderBrush="White"
                                        BorderThickness="1"
                                        Margin="0,0,10,0"/>
                                
                                <!-- Label -->
                                <TextBlock Grid.Column="1" 
                                          Text="{Binding Label}" 
                                          Foreground="White"
                                          VerticalAlignment="Center"
                                          FontSize="13"/>
                            </Grid>
                        </DataTemplate>
                    </ItemsControl.ItemTemplate>
                </ItemsControl>
            </StackPanel>
        </Border>
    </DataTemplate>
</syncfusion:SfTreeMap.LegendTemplate>
```

## LegendLabel in Color Mappings

Add descriptive labels to color ranges:

### RangeBrushColorMapping with Labels

```xaml
<syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:RangeBrushColorMapping>
        <syncfusion:RangeBrushColorMapping.Brushes>
            <syncfusion:RangeBrush From="0" To="25" Color="#27AE60" LegendLabel="Excellent (0-25)"/>
            <syncfusion:RangeBrush From="25" To="50" Color="#F39C12" LegendLabel="Good (25-50)"/>
            <syncfusion:RangeBrush From="50" To="75" Color="#E67E22" LegendLabel="Fair (50-75)"/>
            <syncfusion:RangeBrush From="75" To="100" Color="#E74C3C" LegendLabel="Poor (75-100)"/>
        </syncfusion:RangeBrushColorMapping.Brushes>
    </syncfusion:RangeBrushColorMapping>
</syncfusion:SfTreeMap.LeafColorMapping>
```

## Complete Legend Example

```xaml
<Window.Resources>
    <local:SalesViewModel x:Key="salesViewModel"/>
</Window.Resources>

<Grid Background="#ECF0F1">
    <syncfusion:SfTreeMap DataContext="{StaticResource salesViewModel}"
                          ItemsSource="{Binding SalesData}"
                          WeightValuePath="Revenue"
                          ColorValuePath="Profit"
                          ShowLegend="True"
                          LegendType="DockPosition"
                          LegendHeight="120"
                          LegendWidth="220">
        
        <!-- Color mapping with legend labels -->
        <syncfusion:SfTreeMap.LeafColorMapping>
            <syncfusion:RangeBrushColorMapping>
                <syncfusion:RangeBrushColorMapping.Brushes>
                    <syncfusion:RangeBrush From="0" To="5000" 
                                          Color="#E74C3C" 
                                          LegendLabel="Low Profit ($0-$5K)"/>
                    <syncfusion:RangeBrush From="5000" To="15000" 
                                          Color="#F39C12" 
                                          LegendLabel="Medium Profit ($5K-$15K)"/>
                    <syncfusion:RangeBrush From="15000" To="30000" 
                                          Color="#27AE60" 
                                          LegendLabel="High Profit ($15K-$30K)"/>
                    <syncfusion:RangeBrush From="30000" To="50000" 
                                          Color="#16A085" 
                                          LegendLabel="Excellent Profit ($30K+)"/>
                </syncfusion:RangeBrushColorMapping.Brushes>
            </syncfusion:RangeBrushColorMapping>
        </syncfusion:SfTreeMap.LeafColorMapping>
        
        <!-- Custom legend template -->
        <syncfusion:SfTreeMap.LegendTemplate>
            <DataTemplate>
                <Border Background="White" 
                        BorderBrush="#BDC3C7" 
                        BorderThickness="2" 
                        CornerRadius="8"
                        Padding="15"
                        Margin="10">
                    <StackPanel>
                        <!-- Title -->
                        <TextBlock Text="Profit Levels" 
                                  FontWeight="Bold" 
                                  FontSize="15" 
                                  Foreground="#2C3E50"
                                  Margin="0,0,0,10"/>
                        
                        <!-- Divider -->
                        <Rectangle Height="2" 
                                  Fill="#ECF0F1" 
                                  Margin="0,0,0,10"/>
                        
                        <!-- Legend items -->
                        <ItemsControl ItemsSource="{Binding}">
                            <ItemsControl.ItemTemplate>
                                <DataTemplate>
                                    <StackPanel Orientation="Horizontal" 
                                               Margin="0,6">
                                        <Border Width="25" 
                                               Height="18" 
                                               Background="{Binding Color}"
                                               BorderBrush="#95A5A6"
                                               BorderThickness="1"
                                               CornerRadius="3"
                                               Margin="0,0,12,0"/>
                                        <TextBlock Text="{Binding Label}" 
                                                  VerticalAlignment="Center"
                                                  FontSize="12"
                                                  Foreground="#34495E"/>
                                    </StackPanel>
                                </DataTemplate>
                            </ItemsControl.ItemTemplate>
                        </ItemsControl>
                    </StackPanel>
                </Border>
            </DataTemplate>
        </syncfusion:SfTreeMap.LegendTemplate>
        
        <!-- Levels -->
        <syncfusion:SfTreeMap.Levels>
            <syncfusion:TreeMapFlatLevel GroupPath="Region" GroupGap="10"/>
        </syncfusion:SfTreeMap.Levels>
    </syncfusion:SfTreeMap>
</Grid>
```

## Best Practices

1. **Always use LegendLabel** - Provide clear, descriptive labels for each range
2. **Consistent Format** - Use consistent format across all labels (e.g., "Low (0-25)")
3. **Positioning** - Place legend where it doesn't obscure important data
4. **Styling** - Match legend style to overall application theme
5. **Sizing** - Make legend large enough to read comfortably

## Troubleshooting

**Legend not showing:**
- Verify ShowLegend="True"
- Ensure color mapping is configured
- Check that LegendLabel is set on color ranges

**Legend too small:**
- Set explicit LegendHeight and LegendWidth
- Increase font sizes in LegendTemplate
- Add padding to template elements

**Legend labels missing:**
- Add LegendLabel property to each RangeBrush
- Verify legend template binds to Label property correctly

**Legend positioned incorrectly:**
- Adjust LegendHeight/LegendWidth
- Consider custom positioning with Grid layout
- Check container layout doesn't constrain legend space
