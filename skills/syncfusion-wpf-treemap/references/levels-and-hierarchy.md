# TreeMap Levels and Hierarchy

## Table of Contents
- [Overview](#overview)
- [TreeMapFlatLevel](#treemapflatlevel)
  - [GroupPath](#grouppath)
  - [GroupGap](#groupgap)
  - [Group Styling](#group-styling)
- [Headers](#headers)
- [Multi-Level Hierarchies](#multi-level-hierarchies)
- [Complete Examples](#complete-examples)

## Overview

TreeMap supports hierarchical data visualization through levels. The `Levels` collection defines how data is grouped and displayed hierarchically.

**Two level types:**
1. **TreeMapFlatLevel** - For flat data collections with grouping
2. **TreeMapHierarchicalLevel** - For nested/tree-structured data (less common)

This guide focuses on TreeMapFlatLevel, which is the most commonly used approach.

## TreeMapFlatLevel

TreeMapFlatLevel allows you to create hierarchical groupings from flat data by specifying which property to group by at each level.

###Basic Setup

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population"
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## GroupPath

The `GroupPath` property specifies which data property to use for grouping items at each level.

**Requirements:**
- Must be specified for each TreeMapFlatLevel
- Case-sensitive property name
- Property should contain categorical data (strings, enums)

**Example Data Model:**
```csharp
public class PopulationDetail
{
    public string Continent { get; set; }    // ← Level 1 GroupPath
    public string Country { get; set; }      // ← Level 2 GroupPath
    public double Population { get; set; }
    public double Growth { get; set; }
}
```

**Single-Level Grouping:**
```xaml
<syncfusion:SfTreeMap.Levels>
    <syncfusion:TreeMapFlatLevel GroupPath="Continent" HeaderHeight="30">
        <syncfusion:TreeMapFlatLevel.HeaderTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding Header}" 
                          Foreground="#D6D6D6" 
                          FontSize="18" 
                          FontWeight="Light"/>
            </DataTemplate>
        </syncfusion:TreeMapFlatLevel.HeaderTemplate>
    </syncfusion:TreeMapFlatLevel>
</syncfusion:SfTreeMap.Levels>
```

**Result:** Data is grouped by Continent, with each continent showing as a container with header.

## GroupGap

The `GroupGap` property creates spacing between groups at a level, helping visually distinguish different groups.

**Usage:**
```xaml
<syncfusion:SfTreeMap.Levels>
    <syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                                 GroupGap="10" 
                                 HeaderHeight="30"/>
</syncfusion:SfTreeMap.Levels>
```

**Value:** Pixels of spacing between groups

**Visual Effect:**
- `GroupGap="0"` - Groups touch each other
- `GroupGap="5"` - Small gaps for subtle separation
- `GroupGap="10"` - Medium gaps for clear separation
- `GroupGap="20"` - Large gaps for dramatic separation

**Best Practices:**
- Level 1 (top): Use larger gaps (10-15 pixels)
- Level 2 (nested): Use smaller gaps (5-10 pixels)
- Level 3+ (deep): Use minimal gaps (2-5 pixels)

## Group Styling

### GroupBorderBrush and GroupBorderThickness

Add visible borders around groups:

```xaml
<syncfusion:SfTreeMap.Levels>
    <syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                                 GroupBorderBrush="Red" 
                                 GroupBorderThickness="3"
                                 HeaderHeight="30"/>
</syncfusion:SfTreeMap.Levels>
```

### GroupBackground

Set background color for group containers:

```xaml
<syncfusion:SfTreeMap.Levels>
    <syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                                 GroupBackground="LightGray"
                                 GroupPadding="5"
                                 HeaderHeight="30"/>
</syncfusion:SfTreeMap.Levels>
```

**Note:** GroupBackground is most visible when GroupPadding is also set.

### GroupPadding

Add internal padding inside groups:

```xaml
<syncfusion:SfTreeMap.Levels>
    <syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                                 GroupPadding="5"
                                 GroupBackground="DarkGray"
                                 HeaderHeight="30"/>
</syncfusion:SfTreeMap.Levels>
```

## Headers

### ShowHeader

Control header visibility:

```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                             ShowHeader="True"
                             HeaderHeight="30"/>
```

- **True** - Headers are displayed
- **False** - Headers are hidden

### HeaderHeight

Specify header height in pixels:

```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                             HeaderHeight="40"/>
```

**Recommendations:**
- Level 1: 30-40 pixels
- Level 2: 20-30 pixels
- Level 3: 15-20 pixels

### HeaderTemplate

Customize header appearance:

```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Continent" HeaderHeight="30">
    <syncfusion:TreeMapFlatLevel.HeaderTemplate>
        <DataTemplate>
            <Grid Background="#333333">
                <TextBlock Text="{Binding Header}" 
                          Foreground="White" 
                          FontSize="16" 
                          FontWeight="Bold"
                          Margin="5"
                          VerticalAlignment="Center"/>
            </Grid>
        </DataTemplate>
    </syncfusion:TreeMapFlatLevel.HeaderTemplate>
</syncfusion:TreeMapFlatLevel>
```

**Header Binding Context:**
- `{Binding Header}` - The group name (value from GroupPath property)

### ShowLabels

Display labels on leaf nodes:

```xaml
<syncfusion:TreeMapFlatLevel GroupPath="Country" 
                             ShowLabels="True"
                             HeaderHeight="20"/>
```

- **True** - Leaf rectangles show labels
- **False** - Leaf rectangles have no labels

## Multi-Level Hierarchies

Create nested hierarchies by adding multiple TreeMapFlatLevel objects:

### Two-Level Example

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population"
                      ColorValuePath="Growth">
    <syncfusion:SfTreeMap.Levels>
        <!-- Level 1: Continents -->
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                                     GroupGap="10" 
                                     HeaderHeight="30">
            <syncfusion:TreeMapFlatLevel.HeaderTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding Header}" 
                              Foreground="White" 
                              FontSize="18" 
                              FontWeight="Bold"/>
                </DataTemplate>
            </syncfusion:TreeMapFlatLevel.HeaderTemplate>
        </syncfusion:TreeMapFlatLevel>
        
        <!-- Level 2: Countries -->
        <syncfusion:TreeMapFlatLevel GroupPath="Country" 
                                     GroupGap="5" 
                                     HeaderHeight="20"
                                     ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

### Three-Level Example

```csharp
// Data model with three levels
public class SalesData
{
    public string Region { get; set; }      // Level 1
    public string State { get; set; }       // Level 2
    public string City { get; set; }        // Level 3
    public double Sales { get; set; }
}
```

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding SalesData}"
                      WeightValuePath="Sales">
    <syncfusion:SfTreeMap.Levels>
        <!-- Level 1: Regions -->
        <syncfusion:TreeMapFlatLevel GroupPath="Region" 
                                     GroupGap="15" 
                                     HeaderHeight="35"/>
        
        <!-- Level 2: States -->
        <syncfusion:TreeMapFlatLevel GroupPath="State" 
                                     GroupGap="10" 
                                     HeaderHeight="25"/>
        
        <!-- Level 3: Cities -->
        <syncfusion:TreeMapFlatLevel GroupPath="City" 
                                     GroupGap="5" 
                                     HeaderHeight="20"
                                     ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## Complete Examples

### Example 1: Regional Sales Dashboard

```xaml
<Window.Resources>
    <local:SalesViewModel x:Key="salesViewModel"/>
</Window.Resources>

<syncfusion:SfTreeMap DataContext="{StaticResource salesViewModel}"
                      ItemsSource="{Binding SalesData}"
                      WeightValuePath="Revenue"
                      ColorValuePath="Profit"
                      ItemsLayoutMode="Squarified">
    
    <!-- Color mapping -->
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:RangeBrushColorMapping>
            <syncfusion:RangeBrushColorMapping.Brushes>
                <syncfusion:RangeBrush From="0" To="10000" Color="#FF4444"/>
                <syncfusion:RangeBrush From="10000" To="50000" Color="#FFAA44"/>
                <syncfusion:RangeBrush From="50000" To="100000" Color="#44FF44"/>
            </syncfusion:RangeBrushColorMapping.Brushes>
        </syncfusion:RangeBrushColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
    
    <!-- Levels -->
    <syncfusion:SfTreeMap.Levels>
        <!-- Regions -->
        <syncfusion:TreeMapFlatLevel GroupPath="Region" 
                                     GroupGap="12" 
                                     HeaderHeight="35"
                                     GroupBorderBrush="#666666"
                                     GroupBorderThickness="2">
            <syncfusion:TreeMapFlatLevel.HeaderTemplate>
                <DataTemplate>
                    <Border Background="#333333" Padding="8,0">
                        <TextBlock Text="{Binding Header}" 
                                  Foreground="White" 
                                  FontSize="18" 
                                  FontWeight="Bold"
                                  VerticalAlignment="Center"/>
                    </Border>
                </DataTemplate>
            </syncfusion:TreeMapFlatLevel.HeaderTemplate>
        </syncfusion:TreeMapFlatLevel>
        
        <!-- Products -->
        <syncfusion:TreeMapFlatLevel GroupPath="Product" 
                                     GroupGap="6" 
                                     HeaderHeight="22"
                                     ShowLabels="True">
            <syncfusion:TreeMapFlatLevel.HeaderTemplate>
                <DataTemplate>
                    <Border Background="#555555" Padding="5,0">
                        <TextBlock Text="{Binding Header}" 
                                  Foreground="White" 
                                  FontSize="14"
                                  VerticalAlignment="Center"/>
                    </Border>
                </DataTemplate>
            </syncfusion:TreeMapFlatLevel.HeaderTemplate>
        </syncfusion:TreeMapFlatLevel>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

### Example 2: File System Explorer

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding FileSystemData}"
                      WeightValuePath="Size"
                      ItemsLayoutMode="SliceAndDiceAuto">
    
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:PaletteColorMapping>
            <syncfusion:PaletteColorMapping.Colors>
                <SolidColorBrush Color="#4ECDC4"/>
                <SolidColorBrush Color="#FF6B6B"/>
                <SolidColorBrush Color="#95E1D3"/>
                <SolidColorBrush Color="#F38181"/>
            </syncfusion:PaletteColorMapping.Colors>
        </syncfusion:PaletteColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
    
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Drive" 
                                     GroupGap="8" 
                                     HeaderHeight="30"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Folder" 
                                     GroupGap="4" 
                                     HeaderHeight="24"/>
        <syncfusion:TreeMapFlatLevel GroupPath="SubFolder" 
                                     GroupGap="2" 
                                     HeaderHeight="18"
                                     ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## Best Practices

1. **Group Gap Hierarchy** - Use progressively smaller gaps for deeper levels
2. **Header Heights** - Decrease header height as level depth increases
3. **Show Labels Strategically** - Only show labels on the deepest level or large rectangles
4. **Limit Depth** - 2-3 levels is optimal; 4+ becomes hard to read
5. **Color Coordination** - Use group colors that complement leaf colors
6. **Border Usage** - Borders help at top level but can clutter deep levels

## Troubleshooting

**Groups not appearing:**
- Ensure GroupPath property name matches data model exactly
- Verify data actually has multiple distinct values for GroupPath property
- Check that GroupGap isn't 0 (makes groups touch without visual separation)

**Headers not showing:**
- Set ShowHeader="True"
- Ensure HeaderHeight is > 0
- Check that HeaderTemplate renders visible content

**Labels unreadable:**
- Increase HeaderHeight for more space
- Use smaller FontSize in HeaderTemplate
- Consider custom HeaderTemplate with wrapping or truncation

**Too many levels cluttered:**
- Reduce number of levels (keep to 2-3 max)
- Increase GroupGap for better separation
- Use drill-down instead of showing all levels at once
