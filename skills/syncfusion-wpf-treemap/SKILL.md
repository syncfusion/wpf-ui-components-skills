---
name: syncfusion-wpf-treemap
description: Implement Syncfusion WPF TreeMap (SfTreeMap) control for hierarchical data visualization using nested rectangles. Use this when visualizing large datasets with hierarchical structure, creating heat maps, or displaying proportional data. This skill covers TreeMap configuration, layout algorithms, color mapping, data binding, and interactive features for stock analysis, data categorization, and hierarchical visualization scenarios.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF TreeMap (SfTreeMap)

## When to Use This Skill

Use this skill when the user needs to:
- **Visualize hierarchical data** as nested, sized rectangles representing quantities
- **Display large datasets** where TreeMap's space-efficient layout is ideal
- **Implement data visualization** for stock market analysis, usage statistics, or categorized data
- **Create heat maps** where color represents values and size represents weight
- **Build WPF applications** that need the Syncfusion TreeMap control (SfTreeMap)
- **Customize TreeMap layouts**, colors, levels, or interactive features
- **Configure data binding** for hierarchical or flat data structures

This skill covers the complete implementation of Syncfusion WPF TreeMap, from basic setup to advanced features like drill-down, color mapping, and custom templates.

## Component Overview

**Syncfusion WPF TreeMap (SfTreeMap)** is a data visualization control that displays hierarchical information as a series of clustered rectangles. Each rectangle's size represents a quantitative value (weight), and its color can represent another dimension of the data.

**Key Capabilities:**
- Multiple layout algorithms (Squarified, SliceAndDice variants)
- Flexible color mapping strategies (solid, range, desaturation, palette, group-based)
- Hierarchical levels with customizable headers
- Interactive features (selection, tooltips, drill-down)
- Comprehensive data binding support
- Accessibility and keyboard navigation
- Theme support

**Common Use Cases:**
1. **Stock Market Analysis** - Rectangle size = stock weight, color = gain/loss
2. **Internet Usage Visualization** - Categorize traffic by type (retail, social, search)
3. **News Aggregation** - Color = section (business/politics), size = story count
4. **Weather Analysis** - Opacity/color = humidity, size = geographic area

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- Assembly and namespace references (Syncfusion.SfTreeMap.WPF)
- Creating TreeMap via Visual Studio, Expression Blend, or code
- Basic TreeMap configuration in XAML and C#
- Setting up ItemsSource with data models
- DataContext configuration
- First working example with population data
- Theme support (SfSkinManager, ThemeStudio)

### Layout Modes
📄 **Read:** [references/layout-modes.md](references/layout-modes.md)
- Available layout algorithms
- Squarified layout (default, optimized aspect ratios)
- SliceAndDiceAuto (alternates horizontal/vertical by level)
- SliceAndDiceHorizontal (horizontal slicing only)
- SliceAndDiceVertical (vertical slicing only)
- Choosing the right layout for your data
- Performance considerations

### Data Binding and Configuration
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- WeightValuePath property (determines rectangle sizes)
- ColorValuePath property (drives color mapping)
- ItemsSource binding to collections
- Data model requirements and structure
- Hierarchical vs flat data binding
- Supported collection types (ObservableCollection, List, etc.)

### Color Mapping
📄 **Read:** [references/color-mapping.md](references/color-mapping.md)
- Color mapping overview and types
- UniColorMapping (solid color for all leaves)
- RangeBrushColorMapping (colors based on value ranges)
- DesaturationColorMapping (opacity ranges for visual emphasis)
- PaletteColorMapping (cycling through color collections)
- GroupColorMapping (different colors per group)
- LeafColorMapping vs TreeMapLevel ColorMapping
- Applying colors to leaf nodes vs headers

### TreeMap Levels and Hierarchy
📄 **Read:** [references/levels-and-hierarchy.md](references/levels-and-hierarchy.md)
- TreeMapFlatLevel for grouping flat data
- GroupPath property for hierarchical organization
- Configuring multiple levels
- GroupGap for spacing between groups
- Header configuration (HeaderHeight, ShowHeader)
- Header templates and styling
- ShowLabels property for leaf labels
- Creating nested hierarchies

### Leaf Node Customization
📄 **Read:** [references/leaf-customization.md](references/leaf-customization.md)
- LeafItemSettings overview
- Border customization (LeafBorderBrush, LeafBorderThickness)
- LeafLabelPath for displaying text on leaves
- LeafTemplate for custom leaf UI
- Label positioning and formatting
- Customizing individual leaf appearances
- Template examples for complex leaf content

### Headers and Labels
📄 **Read:** [references/headers-and-labels.md](references/headers-and-labels.md)
- Header templates (HeaderTemplate property)
- Header styling and customization
- Label visibility and content control
- Custom header content with data binding
- Positioning headers within levels

### Interactive Features
📄 **Read:** [references/interactive-features.md](references/interactive-features.md)
- Selection support (single and multiple)
- SelectionModes (Default, Multiple)
- SelectedItems property and selection events
- Tooltip configuration and display
- TooltipTemplate for custom tooltips
- Drill-down navigation support
- DrillDownHeaderTemplate customization
- EnableDrillDown property

### Legend
📄 **Read:** [references/legend.md](references/legend.md)
- Legend overview and purpose
- ShowLegend property
- LegendType (Gradient, DockPosition)
- LegendHeight and LegendWidth configuration
- LegendTemplate for custom legends
- Legend positioning options

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- Keyboard navigation support
- Tab and Shift+Tab navigation between items
- SelectionModes impact on keyboard behavior
- WCAG compliance features
- Screen reader support

## Quick Start Example

Here's a minimal example to create a functional TreeMap displaying population data:

```xaml
<Window x:Class="TreeMapDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.TreeMap;assembly=Syncfusion.SfTreeMap.WPF"
        Title="TreeMap Demo" WindowState="Maximized">
    <Grid>
        <Grid.DataContext>
            <local:PopulationViewModel/>
        </Grid.DataContext>
        
        <syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                              WeightValuePath="Population"
                              ColorValuePath="Growth"
                              ItemsLayoutMode="Squarified">
            
            <!-- Color mapping for growth values -->
            <syncfusion:SfTreeMap.LeafColorMapping>
                <syncfusion:RangeBrushColorMapping>
                    <syncfusion:RangeBrushColorMapping.Brushes>
                        <syncfusion:RangeBrush From="0" To="1" Color="#A4C400"/>
                        <syncfusion:RangeBrush From="1" To="2" Color="#AA00FF"/>
                        <syncfusion:RangeBrush From="2" To="3" Color="#F0A30A"/>
                        <syncfusion:RangeBrush From="3" To="4" Color="#1BA1E2"/>
                    </syncfusion:RangeBrushColorMapping.Brushes>
                </syncfusion:RangeBrushColorMapping>
            </syncfusion:SfTreeMap.LeafColorMapping>
            
            <!-- Hierarchical levels -->
            <syncfusion:SfTreeMap.Levels>
                <syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                                             GroupGap="10"
                                             HeaderHeight="30">
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
        </syncfusion:SfTreeMap>
    </Grid>
</Window>
```

```csharp
// ViewModel with sample data
public class PopulationViewModel
{
    public ObservableCollection<PopulationDetail> PopulationDetails { get; set; }
    
    public PopulationViewModel()
    {
        PopulationDetails = new ObservableCollection<PopulationDetail>
        {
            new PopulationDetail { Continent = "Asia", Country = "Indonesia", Growth = 3, Population = 237641326 },
            new PopulationDetail { Continent = "Asia", Country = "Russia", Growth = 2, Population = 152518015 },
            new PopulationDetail { Continent = "North America", Country = "United States", Growth = 4, Population = 315645000 },
            new PopulationDetail { Continent = "Europe", Country = "Germany", Growth = 1, Population = 81993000 }
        };
    }
}

public class PopulationDetail
{
    public string Continent { get; set; }
    public string Country { get; set; }
    public double Growth { get; set; }
    public double Population { get; set; }
}
```

## Common Patterns

### Pattern 1: Basic TreeMap with Single Level
When displaying flat data without hierarchical grouping:

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding Items}" 
                      WeightValuePath="Value"
                      ItemsLayoutMode="Squarified">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:UniColorMapping Color="Crimson"/>
    </syncfusion:SfTreeMap.LeafColorMapping>
</syncfusion:SfTreeMap>
```

### Pattern 2: Multi-Level Hierarchy
When displaying nested hierarchical data (e.g., Continent → Country):

```xaml
<syncfusion:SfTreeMap.Levels>
    <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10" HeaderHeight="30"/>
    <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" HeaderHeight="20" ShowLabels="True"/>
</syncfusion:SfTreeMap.Levels>
```

### Pattern 3: Range-Based Color Mapping
When coloring rectangles based on value ranges:

```xaml
<syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:RangeBrushColorMapping>
        <syncfusion:RangeBrushColorMapping.Brushes>
            <syncfusion:RangeBrush From="0" To="25" Color="LightGreen"/>
            <syncfusion:RangeBrush From="25" To="50" Color="Yellow"/>
            <syncfusion:RangeBrush From="50" To="75" Color="Orange"/>
            <syncfusion:RangeBrush From="75" To="100" Color="Red"/>
        </syncfusion:RangeBrushColorMapping.Brushes>
    </syncfusion:RangeBrushColorMapping>
</syncfusion:SfTreeMap.LeafColorMapping>
```

### Pattern 4: Selection and Interaction
When enabling user interaction:

```xaml
<syncfusion:SfTreeMap SelectionModes="Multiple">
    <!-- Multiple selection with Ctrl/Shift -->
</syncfusion:SfTreeMap>
```

### Pattern 5: Custom Tooltips
When showing detailed information on hover:

```xaml
<syncfusion:SfTreeMap>
    <syncfusion:SfTreeMap.TooltipTemplate>
        <DataTemplate>
            <StackPanel Background="White" Margin="5">
                <TextBlock Text="{Binding Country}" FontWeight="Bold"/>
                <TextBlock Text="{Binding Population, StringFormat='Population: {0:N0}'}"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfTreeMap.TooltipTemplate>
</syncfusion:SfTreeMap>
```

## Key Properties

### Essential Properties
- **ItemsSource** - Collection of data objects to visualize
- **WeightValuePath** - Property name determining rectangle sizes
- **ColorValuePath** - Property name determining color mapping values
- **ItemsLayoutMode** - Layout algorithm (Squarified, SliceAndDiceAuto, etc.)

### Color Mapping Properties
- **LeafColorMapping** - Color mapping strategy for leaf nodes
- **RangeBrushColorMapping.Brushes** - Color ranges for value-based coloring
- **DesaturationColorMapping** - Opacity-based color mapping
- **PaletteColorMapping.Colors** - Collection of colors to cycle through

### Level Configuration
- **Levels** - Collection of TreeMapFlatLevel objects
- **GroupPath** - Property name for grouping data at each level
- **GroupGap** - Spacing between groups (in pixels)
- **HeaderHeight** - Height of group headers
- **HeaderTemplate** - Custom template for headers
- **ShowHeader** - Show/hide headers at a level
- **ShowLabels** - Show/hide labels on leaf nodes

### Leaf Customization
- **LeafItemSettings** - Settings for leaf node appearance
- **LeafBorderBrush** - Border color for leaf nodes
- **LeafBorderThickness** - Border thickness for leaf nodes
- **LeafLabelPath** - Property name for leaf labels
- **LeafTemplate** - Custom template for leaf content

### Interactive Features
- **SelectionModes** - Selection behavior (Default, Multiple)
- **SelectedItems** - Collection of currently selected items
- **TooltipTemplate** - Custom template for tooltips
- **EnableDrillDown** - Enable drill-down navigation
- **DrillDownHeaderTemplate** - Template for drill-down headers

### Legend Properties
- **ShowLegend** - Show/hide legend
- **LegendType** - Legend display type
- **LegendHeight/LegendWidth** - Legend dimensions
- **LegendTemplate** - Custom template for legend

## Common Use Cases

### Use Case 1: Stock Portfolio Visualization
Display stock holdings where rectangle size = investment amount, color = performance:

**Guidance:** Use RangeBrushColorMapping with negative (red) to positive (green) ranges. Set WeightValuePath to investment amount, ColorValuePath to percentage gain/loss.

**References:** [color-mapping.md](references/color-mapping.md), [data-binding.md](references/data-binding.md)

### Use Case 2: Disk Space Usage Analysis
Show folder sizes in a filesystem hierarchy:

**Guidance:** Use multi-level TreeMapFlatLevel with GroupPath for folder hierarchy. Set WeightValuePath to file size. Consider SliceAndDiceAuto layout for better readability.

**References:** [levels-and-hierarchy.md](references/levels-and-hierarchy.md), [layout-modes.md](references/layout-modes.md)

### Use Case 3: Sales Performance Dashboard
Display sales by region and salesperson:

**Guidance:** Create two levels (Region → Salesperson), use DesaturationColorMapping to highlight top performers with stronger colors. Enable tooltips to show detailed metrics.

**References:** [interactive-features.md](references/interactive-features.md), [levels-and-hierarchy.md](references/levels-and-hierarchy.md)

### Use Case 4: Website Traffic Analysis
Visualize page views by category and page:

**Guidance:** Use PaletteColorMapping for distinct category colors. Add custom LeafTemplate to display page names and metrics. Enable drill-down for navigation.

**References:** [color-mapping.md](references/color-mapping.md), [leaf-customization.md](references/leaf-customization.md), [interactive-features.md](references/interactive-features.md)

### Use Case 5: Weather Data Visualization
Display temperature or precipitation data by geographic region:

**Guidance:** Use DesaturationColorMapping where opacity represents humidity/precipitation. Set up GroupColorMapping for different regions with base colors.

**References:** [color-mapping.md](references/color-mapping.md)

## Assembly and Namespace

**Assembly:** Syncfusion.SfTreeMap.WPF  
**Namespace:** Syncfusion.UI.Xaml.TreeMap

Add this namespace to your XAML:
```xaml
xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.TreeMap;assembly=Syncfusion.SfTreeMap.WPF"
```

## Additional Resources

**Theme Support:**
- Apply themes using SfSkinManager
- Create custom themes with ThemeStudio
- See: [references/getting-started.md](references/getting-started.md)

**Accessibility:**
- Keyboard navigation (Tab/Shift+Tab)
- WCAG compliance
- See: [references/accessibility.md](references/accessibility.md)

---

**Next Steps:**
1. Review [getting-started.md](references/getting-started.md) for installation and setup
2. Choose appropriate layout from [layout-modes.md](references/layout-modes.md)
3. Configure data binding using [data-binding.md](references/data-binding.md)
4. Apply color mapping strategies from [color-mapping.md](references/color-mapping.md)
5. Add levels and hierarchy as needed from [levels-and-hierarchy.md](references/levels-and-hierarchy.md)
