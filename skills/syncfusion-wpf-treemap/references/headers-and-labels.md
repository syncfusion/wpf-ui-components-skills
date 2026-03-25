# Headers and Labels

This guide covers customizing headers for group levels and labels for leaf nodes in TreeMap.

## Header Overview

Headers appear at the top of each group level, displaying the group name and providing visual structure to the hierarchy.

**Key Properties:**
- `HeaderHeight` - Height of header in pixels
- `ShowHeader` - Toggle header visibility
- `HeaderTemplate` - Custom header UI

## HeaderHeight

Sets the height of the header for a specific level.

**Usage:**
```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                             HeaderHeight="30"/>
```

**Recommendations:**
- Top level: 30-40 pixels
- Mid level: 20-30 pixels
- Deep level: 15-20 pixels
- Set to 0 to effectively hide header (alternative to ShowHeader="False")

## ShowHeader

Controls whether headers are displayed for a level.

**Usage:**
```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                             ShowHeader="True"
                             HeaderHeight="30"/>
```

- **True** - Header is visible
- **False** - Header is hidden (even if HeaderHeight > 0)

**When to hide headers:**
- Single-level TreeMaps where grouping is obvious
- Deep levels where headers clutter the view
- When using drill-down navigation

## HeaderTemplate

Customize the header appearance using a DataTemplate.

### Basic HeaderTemplate

```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Continent" HeaderHeight="30">
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
```

**Binding Context:**
- `{Binding Header}` - The group name (value from GroupPath property)

### Styled HeaderTemplate

```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Region" HeaderHeight="35">
    <syncfusion:TreeMapFlatLevel.HeaderTemplate>
        <DataTemplate>
            <Border Background="#2C3E50" 
                    BorderBrush="#34495E" 
                    BorderThickness="0,0,0,2"
                    Padding="10,0">
                <TextBlock Text="{Binding Header}" 
                          Foreground="White" 
                          FontSize="16" 
                          FontWeight="Bold"
                          VerticalAlignment="Center"/>
            </Border>
        </DataTemplate>
    </syncfusion:TreeMapFlatLevel.HeaderTemplate>
</syncfusion:TreeMapFlatLevel>
```

### HeaderTemplate with Icon

```xaml
<syncfusion:TreeMapFlatLevel.HeaderTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal" 
                   Background="#34495E" 
                   Height="30">
            <!-- Icon -->
            <Path Data="M12,2L2,7V17L12,22L22,17V7L12,2Z" 
                  Fill="White" 
                  Width="16" 
                  Height="16"
                  Stretch="Uniform"
                  Margin="10,0,5,0"
                  VerticalAlignment="Center"/>
            
            <!-- Header Text -->
            <TextBlock Text="{Binding Header}" 
                      Foreground="White" 
                      FontSize="14" 
                      FontWeight="SemiBold"
                      VerticalAlignment="Center"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:TreeMapFlatLevel.HeaderTemplate>
```

### HeaderTemplate with Count/Statistics

```xaml
<syncfusion:TreeMapFlatLevel.HeaderTemplate>
    <DataTemplate>
        <Grid Background="#333333" Height="30">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*"/>
                <ColumnDefinition Width="Auto"/>
            </Grid.ColumnDefinitions>
            
            <!-- Group Name -->
            <TextBlock Grid.Column="0" 
                      Text="{Binding Header}" 
                      Foreground="White" 
                      FontSize="14" 
                      FontWeight="Bold"
                      Margin="10,0"
                      VerticalAlignment="Center"/>
            
            <!-- Item Count -->
            <TextBlock Grid.Column="1" 
                      Text="{Binding Count, StringFormat='({0} items)'}" 
                      Foreground="#AAAAAA" 
                      FontSize="12"
                      Margin="10,0"
                      VerticalAlignment="Center"/>
        </Grid>
    </DataTemplate>
</syncfusion:TreeMapFlatLevel.HeaderTemplate>
```

## Multi-Level Header Styling

Different header styles for each level:

```xaml
<syncfusion:SfTreeMap.Levels>
    <!-- Level 1: Large, bold headers -->
    <syncfusion:TreeMapFlatLevel GroupPath="Continent" HeaderHeight="40">
        <syncfusion:TreeMapFlatLevel.HeaderTemplate>
            <DataTemplate>
                <Border Background="#2C3E50" Padding="12,0">
                    <TextBlock Text="{Binding Header}" 
                              Foreground="White" 
                              FontSize="20" 
                              FontWeight="Bold"
                              VerticalAlignment="Center"/>
                </Border>
            </DataTemplate>
        </syncfusion:TreeMapFlatLevel.HeaderTemplate>
    </syncfusion:TreeMapFlatLevel>
    
    <!-- Level 2: Medium headers -->
    <syncfusion:TreeMapFlatLevel GroupPath="Country" HeaderHeight="28">
        <syncfusion:TreeMapFlatLevel.HeaderTemplate>
            <DataTemplate>
                <Border Background="#34495E" Padding="8,0">
                    <TextBlock Text="{Binding Header}" 
                              Foreground="White" 
                              FontSize="14" 
                              FontWeight="SemiBold"
                              VerticalAlignment="Center"/>
                </Border>
            </DataTemplate>
        </syncfusion:TreeMapFlatLevel.HeaderTemplate>
    </syncfusion:TreeMapFlatLevel>
    
    <!-- Level 3: Small headers, labels on leaves -->
    <syncfusion:TreeMapFlatLevel GroupPath="City" 
                                 HeaderHeight="20"
                                 ShowLabels="True">
        <syncfusion:TreeMapFlatLevel.HeaderTemplate>
            <DataTemplate>
                <Border Background="#455A64" Padding="5,0">
                    <TextBlock Text="{Binding Header}" 
                              Foreground="#CCCCCC" 
                              FontSize="11"
                              VerticalAlignment="Center"/>
                </Border>
            </DataTemplate>
        </syncfusion:TreeMapFlatLevel.HeaderTemplate>
    </syncfusion:TreeMapFlatLevel>
</syncfusion:SfTreeMap.Levels>
```

## ShowLabels Property

The `ShowLabels` property controls whether leaf nodes display labels.

**Usage:**
```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Country" 
                             ShowLabels="True"
                             HeaderHeight="25"/>
```

**When set on a level:**
- **True** - Leaf nodes within this level show labels (using LeafLabelPath)
- **False** - Leaf nodes have no labels

**Typical pattern:**
- Set `ShowLabels="True"` on the deepest level only
- Keeps intermediate levels cleaner
- Shows detail where it matters most

```xaml
<syncfusion:SfTreeMap.Levels>
    <syncfusion:TreeMapFlatLevel GroupPath="Region" 
                                 ShowLabels="False"
                                 HeaderHeight="30"/>
    <syncfusion:TreeMapFlatLevel GroupPath="Product" 
                                 ShowLabels="True"
                                 HeaderHeight="22"/>
</syncfusion:SfTreeMap.Levels>
```

## Complete Example

```xaml
<Window.Resources>
    <local:SalesViewModel x:Key="salesVM"/>
</Window.Resources>

<Grid Background="#1E1E1E">
    <syncfusion:SfTreeMap DataContext="{StaticResource salesVM}"
                          ItemsSource="{Binding SalesData}"
                          WeightValuePath="Revenue"
                          ColorValuePath="Profit">
        
        <!-- Leaf settings for labels -->
        <syncfusion:SfTreeMap.LeafItemSettings>
            <syncfusion:LeafItemSettings LeafLabelPath="ProductName"
                                         LeafBorderBrush="White"
                                         LeafBorderThickness="1"/>
        </syncfusion:SfTreeMap.LeafItemSettings>
        
        <!-- Color mapping -->
        <syncfusion:SfTreeMap.LeafColorMapping>
            <syncfusion:RangeBrushColorMapping>
                <syncfusion:RangeBrushColorMapping.Brushes>
                    <syncfusion:RangeBrush From="0" To="5000" Color="#E74C3C"/>
                    <syncfusion:RangeBrush From="5000" To="15000" Color="#F39C12"/>
                    <syncfusion:RangeBrush From="15000" To="30000" Color="#27AE60"/>
                </syncfusion:RangeBrushColorMapping.Brushes>
            </syncfusion:RangeBrushColorMapping>
        </syncfusion:SfTreeMap.LeafColorMapping>
        
        <!-- Levels with custom headers -->
        <syncfusion:SfTreeMap.Levels>
            <!-- Region Level -->
            <syncfusion:TreeMapFlatLevel GroupPath="Region" 
                                         GroupGap="10" 
                                         HeaderHeight="38">
                <syncfusion:TreeMapFlatLevel.HeaderTemplate>
                    <DataTemplate>
                        <Border Background="#2C3E50" 
                                BorderBrush="#3498DB" 
                                BorderThickness="0,0,0,3"
                                Padding="12,0">
                            <StackPanel Orientation="Horizontal">
                                <Ellipse Fill="#3498DB" 
                                        Width="8" 
                                        Height="8" 
                                        VerticalAlignment="Center"
                                        Margin="0,0,8,0"/>
                                <TextBlock Text="{Binding Header}" 
                                          Foreground="White" 
                                          FontSize="18" 
                                          FontWeight="Bold"
                                          VerticalAlignment="Center"/>
                            </StackPanel>
                        </Border>
                    </DataTemplate>
                </syncfusion:TreeMapFlatLevel.HeaderTemplate>
            </syncfusion:TreeMapFlatLevel>
            
            <!-- Product Level -->
            <syncfusion:TreeMapFlatLevel GroupPath="Category" 
                                         GroupGap="5" 
                                         HeaderHeight="26"
                                         ShowLabels="True">
                <syncfusion:TreeMapFlatLevel.HeaderTemplate>
                    <DataTemplate>
                        <Border Background="#34495E" Padding="8,0">
                            <TextBlock Text="{Binding Header}" 
                                      Foreground="White" 
                                      FontSize="13" 
                                      FontWeight="SemiBold"
                                      VerticalAlignment="Center"/>
                        </Border>
                    </DataTemplate>
                </syncfusion:TreeMapFlatLevel.HeaderTemplate>
            </syncfusion:TreeMapFlatLevel>
        </syncfusion:SfTreeMap.Levels>
    </syncfusion:SfTreeMap>
</Grid>
```

## Best Practices

1. **Header Hierarchy** - Use progressively smaller headers for deeper levels
2. **Consistent Styling** - Maintain visual consistency across levels
3. **Contrast** - Ensure header text contrasts with background
4. **Spacing** - Use padding in templates for breathing room
5. **Icons Sparingly** - Icons add visual interest but can clutter
6. **Label Strategy** - Show labels only where they add value

## Troubleshooting

**Headers not showing:**
- Verify ShowHeader="True" (or property not set, defaults to True)
- Check HeaderHeight > 0
- Ensure HeaderTemplate renders visible content
- Verify GroupPath groups data correctly

**Header text cut off:**
- Increase HeaderHeight
- Reduce FontSize
- Add TextTrimming="CharacterEllipsis" to TextBlock
- Use horizontal scrolling (rare)

**Labels not appearing on leaves:**
- Set ShowLabels="True" on the level
- Verify LeafLabelPath is set in LeafItemSettings
- Check property name in LeafLabelPath is correct
- Ensure rectangles are large enough for text

**Header template not updating:**
- Check binding path "{Binding Header}" is correct
- Verify DataContext is properly inherited
- Test with simple TextBlock first, then add complexity
