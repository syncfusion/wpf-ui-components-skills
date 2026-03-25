# Hierarchical Levels

Comprehensive guide for configuring hierarchical levels in the Sunburst Chart. The Levels collection defines how your data is organized into concentric circles, with each level representing a category in the hierarchy.

## Overview

Sunburst charts visualize hierarchical data where:
- **Center circle** represents the root level (first level in hierarchy)
- **Each outer ring** represents a deeper level
- **Segment size** is determined by the ValueMemberPath property
- **Each level** is defined by a `SunburstHierarchicalLevel` with a `GroupMemberPath`

The chart automatically arranges data based on the GroupMemberPath values, creating parent-child relationships between levels.

## The Levels Collection

The `Levels` property is a collection of `SunburstHierarchicalLevel` objects that define the hierarchy structure.

### Basic Syntax

**XAML:**
```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level1Property"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level2Property"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level3Property"/>
</sunburst:SfSunburstChart.Levels>
```

**C#:**
```csharp
SunburstHierarchicalLevel level1 = new SunburstHierarchicalLevel();
level1.GroupMemberPath = "Level1Property";

SunburstHierarchicalLevel level2 = new SunburstHierarchicalLevel();
level2.GroupMemberPath = "Level2Property";

SunburstHierarchicalLevel level3 = new SunburstHierarchicalLevel();
level3.GroupMemberPath = "Level3Property";

sunburstChart.Levels.Add(level1);
sunburstChart.Levels.Add(level2);
sunburstChart.Levels.Add(level3);
```

## GroupMemberPath Property

The `GroupMemberPath` is a string property that maps to a property name in your data model. It determines how data is grouped at each level.

### Example Data Model

```csharp
public class SalesData
{
    public string Region { get; set; }      // Level 1
    public string Category { get; set; }    // Level 2
    public string Product { get; set; }     // Level 3
    public string SKU { get; set; }         // Level 4
    public double Sales { get; set; }       // Value for sizing
}
```

### Mapping Levels to Properties

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding SalesData}" 
                          ValueMemberPath="Sales">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="SKU"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**Result:**
- **Inner circle:** Regions (North, South, East, West)
- **Second ring:** Categories within each region (Electronics, Clothing, etc.)
- **Third ring:** Products within each category (Laptop, Phone, etc.)
- **Outer ring:** SKUs within each product (specific models)

## Level Count Recommendations

### 2-Level Hierarchy (Simple)

Best for basic categorization:

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
</sunburst:SfSunburstChart.Levels>
```

**Use when:** Showing simple parent-child relationships (Department → Team, Category → Subcategory)

### 3-Level Hierarchy (Standard)

Most common configuration, balances depth with readability:

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="State"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="City"/>
</sunburst:SfSunburstChart.Levels>
```

**Use when:** Displaying geographical hierarchies, organizational structures, or moderate data depth

### 4-Level Hierarchy (Complex)

More detailed but still manageable:

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Role"/>
</sunburst:SfSunburstChart.Levels>
```

**Use when:** Deep organizational hierarchies, detailed product catalogs, or complex categorization

### 5+ Level Hierarchy (Advanced)

Use sparingly, can become crowded:

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="State"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="City"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="District"/>
</sunburst:SfSunburstChart.Levels>
```

**Use when:** Very deep hierarchies with zooming enabled for drill-down

**Recommendation:** Enable zooming behavior for 5+ levels to allow users to drill down.

## Working with Variable-Depth Data

Not all data items need to have values for all levels. The chart handles this gracefully.

### Example: Variable Depth

```csharp
public class BudgetData
{
    public string Department { get; set; }
    public string Project { get; set; }
    public string Phase { get; set; }
    public string Task { get; set; }
    public double Budget { get; set; }
}

// Sample data with varying depths
var data = new ObservableCollection<BudgetData>
{
    // Only department level
    new BudgetData { Department = "HR", Budget = 50000 },
    
    // Department and project
    new BudgetData { Department = "Engineering", Project = "Product A", Budget = 100000 },
    
    // Department, project, and phase
    new BudgetData { Department = "Engineering", Project = "Product B", Phase = "Development", Budget = 75000 },
    
    // All levels
    new BudgetData { Department = "Engineering", Project = "Product B", Phase = "Development", Task = "Coding", Budget = 30000 }
};
```

The chart automatically handles null or missing level values, creating segments only where data exists.

## Level Ordering

The order in which you add levels matters. Levels are rendered from inside to outside.

### Correct Order (General to Specific)

```xml
<!-- Renders as: Region (center) → Category → Product (outer) -->
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
</sunburst:SfSunburstChart.Levels>
```

### Incorrect Order (Specific to General)

```xml
<!-- Don't do this: Product (center) → Category → Region (outer) -->
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Product"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
</sunburst:SfSunburstChart.Levels>
```

**Best Practice:** Always order from most general (broad categories) to most specific (detailed items).

## Common Patterns

### Pattern 1: Organizational Hierarchy

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Company"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Division"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
</sunburst:SfSunburstChart.Levels>
```

### Pattern 2: Geographical Breakdown

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Continent"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Region"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="City"/>
</sunburst:SfSunburstChart.Levels>
```

### Pattern 3: Product Catalog

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subcategory"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Brand"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Model"/>
</sunburst:SfSunburstChart.Levels>
```

### Pattern 4: Time-Based Hierarchy

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Year"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Quarter"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Month"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Week"/>
</sunburst:SfSunburstChart.Levels>
```

### Pattern 5: File System

```xml
<sunburst:SfSunburstChart.Levels>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Drive"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Folder"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subfolder"/>
    <sunburst:SunburstHierarchicalLevel GroupMemberPath="FileName"/>
</sunburst:SfSunburstChart.Levels>
```

## Dynamic Level Configuration

You can add or remove levels programmatically based on user input or data structure:

```csharp
public void ConfigureLevels(int levelCount)
{
    sunburstChart.Levels.Clear();
    
    string[] levelProperties = { "Level1", "Level2", "Level3", "Level4", "Level5" };
    
    for (int i = 0; i < Math.Min(levelCount, levelProperties.Length); i++)
    {
        var level = new SunburstHierarchicalLevel();
        level.GroupMemberPath = levelProperties[i];
        sunburstChart.Levels.Add(level);
    }
}
```

## Troubleshooting

**Segments not grouping correctly:**
- Verify GroupMemberPath property name matches your data model exactly (case-sensitive)
- Check for typos in property names
- Ensure the property is a string type
- Confirm data has values for the specified property

**Empty rings appearing:**
- This indicates null or empty values in that level's property
- Filter your data to remove items with null values, or
- Design your data model to have consistent depth

**Hierarchy appears reversed:**
- Check the order of levels in the Levels collection
- Remember: first level = center, last level = outermost ring

**Too many levels causing crowding:**
- Reduce the number of levels (aim for 3-4)
- Enable zooming behavior for drill-down
- Increase chart size (Radius property)
- Consider data aggregation at higher levels

## Best Practices

1. **Limit depth:** Use 3-4 levels for optimal visualization; 5+ requires zooming
2. **Consistent naming:** Use clear, descriptive property names (Region, not R or Reg)
3. **Data validation:** Ensure all level properties contain meaningful values
4. **Order logically:** Start with broad categories, end with specific items
5. **Test with real data:** Use representative datasets during development
6. **Consider aggregation:** For large datasets, aggregate at higher levels
7. **Handle nulls:** Filter or handle null values in your data source

## Performance Considerations

- **Large datasets:** With 1000+ items, consider limiting visible levels
- **Level count:** Each additional level multiplies rendering complexity
- **Data binding:** Use ObservableCollection for dynamic updates
- **Filtering:** Pre-filter data in ViewModel rather than in chart configuration
