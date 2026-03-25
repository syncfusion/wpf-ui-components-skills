# Interactive Features

## Table of Contents
- [Selection Support](#selection-support)
- [Tooltip Support](#tooltip-support)
- [Drill-Down Navigation](#drill-down-navigation)

## Selection Support

TreeMap supports selecting leaf nodes and entire groups, with visual highlighting and multiple selection modes.

### HighlightOnSelection

Enable selection highlighting for individual leaf nodes.

**Basic Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ColorValuePath="Growth"
                      HighlightOnSelection="True"
                      HighlightBorderBrush="Red"
                      HighlightBorderThickness="4">
</syncfusion:SfTreeMap>
```

**Properties:**
- **HighlightOnSelection** - Enable/disable selection highlighting
- **HighlightBorderBrush** - Color of highlight border
- **HighlightBorderThickness** - Thickness of highlight border (pixels)
- **HighlightHoverBrush** - Color shown on mouse hover

### SelectionModes

Control how items can be selected.

**Values:**
- **Default** - Single selection (click replaces previous selection)
- **Multiple** - Multiple selection (Ctrl+Click to add/remove)

**Usage:**
```xaml
<syncfusion:SfTreeMap SelectionModes="Multiple"
                      HighlightOnSelection="True"
                      HighlightBorderBrush="Blue"
                      HighlightBorderThickness="3">
</syncfusion:SfTreeMap>
```

**Interaction:**
- **Default mode:** Click item to select, click another to switch selection
- **Multiple mode:** 
  - Click to select first item
  - Ctrl+Click to add more items
  - Ctrl+Click again to deselect
  - Shift+Click for range selection (if supported)

### HighlightGroupOnSelection

Enable selection highlighting for entire groups instead of individual leaves.

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ColorValuePath="Growth"
                      HighlightGroupOnSelection="True"
                      HighlightBorderBrush="Red"
                      HighlightBorderThickness="4"
                      SelectionModes="Multiple">
</syncfusion:SfTreeMap>
```

**Behavior:**
- Clicking any leaf in a group selects the entire group
- All items in the group get highlighted
- Useful for category-level operations

### Complete Selection Example

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding SalesData}"
                      WeightValuePath="Revenue"
                      ColorValuePath="Profit"
                      HighlightOnSelection="True"
                      HighlightBorderBrush="#3498DB"
                      HighlightBorderThickness="3"
                      HighlightHoverBrush="#2ECC71"
                      SelectionModes="Multiple">
    
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:RangeBrushColorMapping>
            <syncfusion:RangeBrushColorMapping.Brushes>
                <syncfusion:RangeBrush From="0" To="5000" Color="#E74C3C"/>
                <syncfusion:RangeBrush From="5000" To="15000" Color="#F39C12"/>
                <syncfusion:RangeBrush From="15000" To="30000" Color="#27AE60"/>
            </syncfusion:RangeBrushColorMapping.Brushes>
        </syncfusion:RangeBrushColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
    
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Region" GroupGap="10"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## Tooltip Support

Display detailed information when hovering over TreeMap items.

### ShowToolTip

Enable tooltip display on hover.

**Basic Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population"
                      ColorValuePath="Growth"
                      ShowToolTip="True">
</syncfusion:SfTreeMap>
```

### ToolTipShowDuration

Control tooltip animation speed (in milliseconds).

**Usage:**
```xaml
<syncfusion:SfTreeMap ShowToolTip="True"
                      ToolTipShowDuration="500">
</syncfusion:SfTreeMap>
```

- Lower values = faster animation
- Higher values = slower, smoother animation
- Default: 1000ms (1 second)

### ToolTipTemplate

Customize tooltip appearance and content.

**Simple Template:**
```xaml
<syncfusion:SfTreeMap ShowToolTip="True">
    <syncfusion:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <Border Background="White" 
                    BorderBrush="Gray" 
                    BorderThickness="1" 
                    Padding="10"
                    CornerRadius="5">
                <StackPanel>
                    <TextBlock Text="{Binding Data.Country}" 
                              FontWeight="Bold" 
                              FontSize="14"/>
                    <TextBlock Text="{Binding Data.Population, StringFormat='Population: {0:N0}'}" 
                              FontSize="12" 
                              Margin="0,5,0,0"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeMap.ToolTipTemplate>
</syncfusion:SfTreeMap>
```

**Advanced Template with Color:**
```xaml
<syncfusion:SfTreeMap ShowToolTip="True" 
                      ToolTipShowDuration="300">
    <syncfusion:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <Grid Height="200" Width="420" Margin="0,-420,0,0">
                <!-- Tooltip balloon shape -->
                <Path Data="M0,0 L360,0 L360,200 L20,200 L-20,220 L0,160 L0,0 z" 
                      Fill="#666666" 
                      Stroke="Wheat" 
                      Opacity="0.9" 
                      Stretch="Fill"/>
                
                <!-- Content -->
                <StackPanel>
                    <!-- Header with mapped color -->
                    <Grid Background="{Binding MappedColor}" 
                          Margin="40,10,20,0" 
                          Height="40">
                        <TextBlock Text="{Binding Data.Continent}" 
                                  Foreground="White" 
                                  FontWeight="SemiBold" 
                                  FontSize="30" 
                                  TextAlignment="Center"/>
                    </Grid>
                    
                    <!-- Details -->
                    <StackPanel Margin="40,20">
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="Country" Width="140" FontSize="25" Foreground="White"/>
                            <TextBlock Text="-" Width="20" FontSize="25" Foreground="White"/>
                            <TextBlock Text="{Binding Data.Country}" FontSize="25" Foreground="White"/>
                        </StackPanel>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="Population" Width="140" FontSize="25" Foreground="White"/>
                            <TextBlock Text="-" Width="20" FontSize="25" Foreground="White"/>
                            <TextBlock Text="{Binding Data.Population}" FontSize="25" Foreground="White"/>
                        </StackPanel>
                    </StackPanel>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeMap.ToolTipTemplate>
</syncfusion:SfTreeMap>
```

**Tooltip Binding Context:**
- `{Binding Data.PropertyName}` - Access data item properties
- `{Binding MappedColor}` - Get the color assigned to the item
- `{Binding Weight}` - Get the weight value used for sizing

## Drill-Down Navigation

Drill-down allows users to navigate into hierarchical levels by clicking items, showing one level at a time.

### EnableDrillDown

Enable drill-down navigation.

**Basic Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding Data}"
                      EnableDrillDown="True"
                      WeightValuePath="Population"
                      ColorValuePath="Area">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" ShowLabels="True"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Country" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

**Behavior:**
- Click a group to drill down into it
- Group expands to fill the entire TreeMap
- Drill-down header appears at the top
- Click header to navigate back up

### DrillDown Properties

**DrillDownHeaderHeight:**
```xaml
<syncfusion:SfTreeMap EnableDrillDown="True"
                      DrillDownHeaderHeight="40">
</syncfusion:SfTreeMap>
```

**DrillDownSelectionStroke:**
Highlight color when clicking to drill down:
```xaml
<syncfusion:SfTreeMap EnableDrillDown="True"
                      DrillDownSelectionStroke="#1A9DAF">
</syncfusion:SfTreeMap>
```

### DrillDownHeaderTemplate

Customize the drill-down header appearance.

**Basic Template:**
```xaml
<syncfusion:SfTreeMap EnableDrillDown="True">
    <syncfusion:SfTreeMap.DrillDownHeaderTemplate>
        <DataTemplate>
            <Border Background="#34495E" 
                    BorderBrush="#2C3E50" 
                    BorderThickness="0,0,0,2">
                <StackPanel Orientation="Horizontal" 
                           VerticalAlignment="Center" 
                           Margin="10">
                    <!-- Back arrow -->
                    <Path Data="M10,0 L0,5 L10,10 Z" 
                          Fill="White" 
                          Width="10" 
                          Height="16" 
                          Margin="0,0,10,0"
                          VerticalAlignment="Center"/>
                    
                    <!-- Current level text -->
                    <TextBlock Text="{Binding}" 
                              FontSize="16" 
                              FontWeight="Bold" 
                              Foreground="White"
                              VerticalAlignment="Center"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeMap.DrillDownHeaderTemplate>
</syncfusion:SfTreeMap>
```

**Advanced Template:**
```xaml
<syncfusion:SfTreeMap.DrillDownHeaderTemplate>
    <DataTemplate>
        <Grid Background="#1A9DAF" Height="40">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            
            <!-- Back Button -->
            <Button Grid.Column="0" 
                    Background="Transparent" 
                    BorderThickness="0"
                    Padding="15,0">
                <StackPanel Orientation="Horizontal">
                    <Path Data="M197,153.5 L197,138 186.75,145.5 Z" 
                          Height="16" 
                          Width="8" 
                          Fill="White" 
                          Stretch="Fill"/>
                    <TextBlock Text="Back" 
                              Foreground="White" 
                              FontSize="14" 
                              Margin="10,0,0,0"
                              VerticalAlignment="Center"/>
                </StackPanel>
            </Button>
            
            <!-- Current Path -->
            <TextBlock Grid.Column="1" 
                      Text="{Binding}" 
                      FontSize="18" 
                      FontWeight="SemiBold" 
                      Foreground="White"
                      VerticalAlignment="Center"
                      Margin="10,0"/>
        </Grid>
    </DataTemplate>
</syncfusion:SfTreeMap.DrillDownHeaderTemplate>
```

### Complete Drill-Down Example

```xaml
<syncfusion:SfTreeMap x:Name="TreeMap" 
                      ItemsSource="{Binding HierarchicalData}"
                      EnableDrillDown="True"
                      DrillDownHeaderHeight="45"
                      DrillDownSelectionStroke="#3498DB"
                      WeightValuePath="Population" 
                      ColorValuePath="Area"
                      LeafLabelPath="Name" 
                      BorderThickness="1">
    
    <!-- Drill-down header -->
    <syncfusion:SfTreeMap.DrillDownHeaderTemplate>
        <DataTemplate>
            <Border Background="#2C3E50">
                <StackPanel Orientation="Horizontal" 
                           VerticalAlignment="Center" 
                           Margin="15,0">
                    <Path Data="M15,0 L0,7.5 L15,15 Z" 
                          Height="16" 
                          Width="10" 
                          Fill="White" 
                          Stretch="Fill"/>
                    <TextBlock Text="{Binding}" 
                              Margin="15,0" 
                              FontSize="18" 
                              FontWeight="SemiBold" 
                              Foreground="White"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeMap.DrillDownHeaderTemplate>
    
    <!-- Color mapping -->
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:UniColorMapping Color="#CCDFE3"/>
    </syncfusion:SfTreeMap.LeafColorMapping>
    
    <!-- Levels for drill-down -->
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" ShowLabels="True"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Country" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## Best Practices

**Selection:**
- Use contrasting highlight colors for visibility
- Consider Multiple selection mode for analysis scenarios
- Group selection works well for category-based actions

**Tooltips:**
- Keep tooltip content concise
- Show most relevant information first
- Use fast animation (300-500ms) for responsive feel
- Include visual hierarchy in complex tooltips

**Drill-Down:**
- Limit hierarchy to 3-4 levels for usability
- Make drill-down header visually distinct
- Include clear back navigation
- Consider breadcrumb navigation for deep hierarchies
- Use drill-down when showing all levels simultaneously is too cluttered

## Troubleshooting

**Selection not working:**
- Verify HighlightOnSelection="True"
- Check HighlightBorderThickness > 0
- Ensure HighlightBorderBrush is visible color

**Tooltips not showing:**
- Set ShowToolTip="True"
- Verify ToolTipTemplate renders correctly
- Check ToolTipShowDuration isn't too fast (>200ms recommended)

**Drill-down not responding:**
- Ensure EnableDrillDown="True"
- Verify at least 2 levels are defined
- Check DrillDownHeaderHeight > 0
- Ensure data has hierarchical structure

**Drill-down header not visible:**
- Set DrillDownHeaderHeight to reasonable value (30-50px)
- Verify DrillDownHeaderTemplate has visible content
- Check template background color isn't transparent
