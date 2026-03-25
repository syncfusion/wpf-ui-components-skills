# TreeMap Layout Modes

TreeMap provides multiple layout algorithms to organize rectangles differently based on your visualization needs. The layout algorithm is controlled by the `ItemsLayoutMode` property.

## Available Layout Modes

TreeMap supports four layout algorithms:

1. **Squarified** (Default)
2. **SliceAndDiceAuto**
3. **SliceAndDiceHorizontal**
4. **SliceAndDiceVertical**

## Squarified Layout

**Best for:** General-purpose visualization where readability is important.

The Squarified layout algorithm creates rectangles with aspect ratios as close to 1:1 (square) as possible. This makes labels more readable and improves visual clarity.

**Characteristics:**
- Creates near-square rectangles
- Better aspect ratios for readability
- Optimal for displaying labels
- Default and most commonly used layout

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ItemsLayoutMode="Squarified">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

**When to use:**
- Default choice for most scenarios
- When label readability is critical
- When you want balanced, aesthetically pleasing layouts

## SliceAndDiceAuto Layout

**Best for:** Hierarchical data with multiple levels where direction alternation helps distinguish levels.

This layout alternates between horizontal and vertical slicing at each level of the hierarchy. The first level slices horizontally, the second level vertically, and so on.

**Characteristics:**
- Alternates slicing direction per level
- Level 1: Horizontal slicing
- Level 2: Vertical slicing
- Level 3: Horizontal again, and so on
- Good for showing hierarchical structure

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ItemsLayoutMode="SliceAndDiceAuto">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

**When to use:**
- Multi-level hierarchies where level distinction is important
- When you want visual separation between hierarchy levels
- File system or organizational hierarchy visualizations

## SliceAndDiceHorizontal Layout

**Best for:** Comparing items horizontally, like timeline or progress tracking.

This layout divides the space horizontally at every level, creating vertical strips for each item.

**Characteristics:**
- All rectangles are arranged as vertical strips
- Consistent horizontal slicing at all levels
- Easy horizontal comparison
- Good for wide displays

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ItemsLayoutMode="SliceAndDiceHorizontal">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

**When to use:**
- Horizontal comparison is important
- Timeline visualizations
- Wide display areas
- When you want consistent vertical strips

## SliceAndDiceVertical Layout

**Best for:** Comparing items vertically, like rankings or stacked categories.

This layout divides the space vertically at every level, creating horizontal strips for each item.

**Characteristics:**
- All rectangles are arranged as horizontal strips
- Consistent vertical slicing at all levels
- Easy vertical comparison
- Good for tall displays

**Usage:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ItemsLayoutMode="SliceAndDiceVertical">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

**When to use:**
- Vertical comparison is important
- Ranking visualizations (top to bottom)
- Tall display areas
- Stacked category displays

## Layout Comparison Examples

### Example: Population by Continent and Country

**Squarified Layout:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ItemsLayoutMode="Squarified">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```
Result: Rectangles are roughly square-shaped, easy to read labels.

**SliceAndDiceAuto Layout:**
```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}" 
                      WeightValuePath="Population"
                      ItemsLayoutMode="SliceAndDiceAuto">
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
        <syncfusion:TreeMapFlatLevel GroupPath="Country" GroupGap="5" ShowLabels="True"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```
Result: Continents sliced horizontally, countries within each continent sliced vertically.

## Choosing the Right Layout

| Scenario | Recommended Layout | Reason |
|----------|-------------------|---------|
| General visualization | Squarified | Best readability and aesthetics |
| Multi-level hierarchy | SliceAndDiceAuto | Clearly distinguishes levels |
| Horizontal comparison | SliceAndDiceHorizontal | Consistent vertical strips |
| Vertical comparison | SliceAndDiceVertical | Consistent horizontal strips |
| Label-heavy display | Squarified | Better aspect ratios for text |
| Wide dashboard | SliceAndDiceHorizontal | Uses width effectively |
| Tall sidebar widget | SliceAndDiceVertical | Uses height effectively |
| File explorer | SliceAndDiceAuto | Shows folder hierarchy clearly |

## Performance Considerations

**Squarified Layout:**
- Slightly more computation intensive (calculates optimal aspect ratios)
- Best user experience
- Negligible performance difference for typical datasets (<10,000 items)

**SliceAndDice Layouts:**
- Faster computation (simple division)
- Better performance with very large datasets
- Consider for real-time updating scenarios

## Code-Behind Configuration

You can set the layout mode programmatically:

```csharp
// Set layout in code
treeMap.ItemsLayoutMode = TreeMapLayoutMode.Squarified;

// Change layout dynamically
private void OnLayoutModeChanged(string mode)
{
    switch (mode)
    {
        case "Squarified":
            treeMap.ItemsLayoutMode = TreeMapLayoutMode.Squarified;
            break;
        case "Auto":
            treeMap.ItemsLayoutMode = TreeMapLayoutMode.SliceAndDiceAuto;
            break;
        case "Horizontal":
            treeMap.ItemsLayoutMode = TreeMapLayoutMode.SliceAndDiceHorizontal;
            break;
        case "Vertical":
            treeMap.ItemsLayoutMode = TreeMapLayoutMode.SliceAndDiceVertical;
            break;
    }
}
```

## Best Practices

1. **Start with Squarified** - It's the default for good reason
2. **Test with real data** - Different datasets may benefit from different layouts
3. **Consider container size** - Wide containers favor horizontal, tall containers favor vertical
4. **Multi-level hierarchies** - Use SliceAndDiceAuto to make levels visually distinct
5. **User preference** - Consider allowing users to switch layouts via UI controls

## Troubleshooting

**Layout doesn't look right:**
- Ensure ItemsLayoutMode is spelled correctly (case-sensitive)
- Verify data has appropriate weight values
- Check GroupGap isn't too large for your data density

**Rectangles overlap:**
- This shouldn't happen with any layout mode
- Verify TreeMap container has explicit Width and Height
- Check for binding errors in ItemsSource

**Labels unreadable:**
- Switch to Squarified layout for better aspect ratios
- Increase font size in LeafLabelPath template
- Consider showing labels only on larger rectangles
