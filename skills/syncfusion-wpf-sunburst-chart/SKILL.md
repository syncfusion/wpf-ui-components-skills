---
name: syncfusion-wpf-sunburst-chart
description: Implement Syncfusion WPF Sunburst Chart (SfSunburstChart) for hierarchical data visualization. Use this when working with sunburst charts, hierarchical data visualization, multi-level circular charts, or drill-down visualizations. This skill covers displaying organizational hierarchies, file system structures, sales data by region/category, and other multi-level categorical data in concentric circles for WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

## Assembly Requirements

**Required Assembly:**
- Syncfusion.SfSunburstChart.WPF

**NuGet Package:**
```powershell
Install-Package Syncfusion.SfSunburstChart.WPF
```

**Minimum .NET Framework:** 4.5 or higher

# Implementing Sunburst Charts

Guide for implementing the Syncfusion WPF Sunburst Chart (SfSunburstChart) control for visualizing hierarchical data in concentric circular rings. The sunburst chart displays multi-level categorical data where each ring represents a level in the hierarchy, with the center representing the root and outer rings representing deeper levels.

## When to Use This Skill

Use this skill when you need to:
- **Visualize hierarchical data** with multiple levels in a circular layout
- **Display organizational structures** (departments, teams, roles)
- **Show file system hierarchies** (folders, subfolders, files)
- **Present sales data** organized by region, category, product, subcategory
- **Implement drill-down experiences** with interactive zooming
- **Create multi-level circular visualizations** where size represents value
- **Display tree-structured data** in a compact, visual format
- **Enable interactive exploration** of nested categorical data

Sunburst charts are ideal when you have 2-5 levels of hierarchical data and want to show both the structure and relative sizes of categories.

## Component Overview

The SfSunburstChart is a specialized WPF control that visualizes hierarchical data in concentric circles:

**Key Capabilities:**
- **Multi-level hierarchies:** Support for 2-5+ hierarchical levels
- **Data binding:** MVVM-compatible with IEnumerable data sources
- **Interactive features:** Tooltips, selection, zooming/drill-down
- **Visual customization:** Color palettes, data labels, legends
- **Animations:** FadeIn and Rotation animations for smooth rendering
- **Responsive design:** Customizable radius and angles

**Core Properties:**
- `ItemsSource` – Data collection (hierarchical data model)
- `ValueMemberPath` – Property name for segment sizing
- `Levels` – Collection defining each hierarchy level
- `GroupMemberPath` – Property for each level's grouping

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Use this reference when:
- Setting up a new sunburst chart project
- Adding assembly references (Syncfusion.SfSunburstChart.WPF)
- Creating data models and ViewModels
- Binding data to the chart
- Adding basic visual elements (header, legend, data labels)
- Understanding complete working examples

Topics covered:
- Assembly references and installation
- Namespace imports (XAML and C#)
- Data model structure for hierarchical data
- ViewModel initialization with ObservableCollection
- Basic chart configuration (ItemsSource, ValueMemberPath)
- Adding header, legend, and data labels
- Theme integration

### Hierarchical Levels Configuration

📄 **Read:** [references/hierarchical-levels.md](references/hierarchical-levels.md)

Use this reference when:
- Defining the hierarchy structure
- Configuring multiple levels (2-5+ levels)
- Understanding GroupMemberPath mapping
- Organizing data by categories and subcategories

Topics covered:
- SunburstHierarchicalLevel overview
- Levels collection management
- GroupMemberPath property usage
- Multi-level hierarchy examples
- Level ordering and structure

### Data Labels

📄 **Read:** [references/data-labels.md](references/data-labels.md)

Use this reference when:
- Showing category names and values on segments
- Customizing label appearance and positioning
- Handling label overflow (Hide or Trim)
- Rotating labels for better readability
- Creating custom label templates

Topics covered:
- Enabling/disabling labels (ShowLabel)
- LabelOverflowMode (Hide, Trim)
- LabelRotationMode (Normal, Angle)
- Font customization (FontFamily, FontSize, FontStyle, FontWeight)
- Custom templates (LabelTemplate)
- Foreground color customization

### Legend Configuration

📄 **Read:** [references/legend.md](references/legend.md)

Use this reference when:
- Adding a legend to identify categories
- Customizing legend icons and layout
- Positioning the legend (Left, Right, Top, Bottom)
- Implementing legend interactivity (click to select/hide)
- Creating custom legend templates

Topics covered:
- Legend initialization
- LegendIcon shapes (Circle, Pentagon, Rectangle, etc.)
- Icon sizing (IconHeight, IconWidth)
- Custom legend templates (LegendIconTemplate)
- Positioning (DockPosition)
- Legend interactivity (ClickAction: ToggleSegmentSelection, ToggleSegmentVisibility)

### Animation

📄 **Read:** [references/animation.md](references/animation.md)

Use this reference when:
- Adding smooth entry animations
- Configuring animation duration
- Choosing between FadeIn and Rotation animations

Topics covered:
- EnableAnimation property
- AnimationDuration configuration
- AnimationType options (FadeIn, Rotation)
- Performance considerations

### Visual Customization

📄 **Read:** [references/visual-customization.md](references/visual-customization.md)

Use this reference when:
- Applying color palettes (predefined or custom)
- Creating custom color schemes
- Adjusting chart dimensions (radius, inner radius)
- Customizing start and end angles
- Using gradient brushes

Topics covered:
- Predefined palettes (Metro, AutumnBrights, FloraHues, etc.)
- Custom palettes (ColorModel, CustomBrushes)
- Gradient brush examples
- Region properties (StartAngle, EndAngle, Radius, InnerRadius)
- Visual structure customization

### Interactivity Features

📄 **Read:** [references/interactivity.md](references/interactivity.md)

Use this reference when:
- Adding tooltips on hover
- Implementing segment selection
- Enabling drill-down/zooming
- Configuring interactive behaviors
- Customizing user experience

Topics covered:
- **Tooltips:** ShowToolTip, alignment, offset, duration, custom templates
- **Selection:** SelectionDisplayMode, SelectionMode, SelectionType, SelectionBrush
- **Zooming:** SunburstZoomingBehavior, toolbar configuration, drill-down

### Event Handling

📄 **Read:** [references/events.md](references/events.md)

Use this reference when:
- Responding to segment creation
- Handling selection changes
- Customizing segments dynamically
- Implementing custom behaviors on user actions

Topics covered:
- SegmentCreated event
- SelectionChanged event
- Event argument properties
- Dynamic segment customization
- Event handler patterns

## Quick Start Example

Here's a minimal example to create a sunburst chart showing employee distribution:

### 1. Add Assembly Reference

```xml
<!-- In your XAML, add namespace -->
<Window xmlns:sunburst="clr-namespace:Syncfusion.UI.Xaml.SunburstChart;assembly=Syncfusion.SfSunburstChart.WPF">
```

### 2. Create Data Model

```csharp
public class EmployeeData
{
    public string Country { get; set; }
    public string Department { get; set; }
    public string Team { get; set; }
    public double EmployeeCount { get; set; }
}
```

### 3. Create ViewModel

```csharp
public class ViewModel
{
    public ObservableCollection<EmployeeData> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<EmployeeData>
        {
            new EmployeeData { Country = "USA", Department = "Sales", Team = "Field Sales", EmployeeCount = 45 },
            new EmployeeData { Country = "USA", Department = "Sales", Team = "Inside Sales", EmployeeCount = 30 },
            new EmployeeData { Country = "USA", Department = "Engineering", Team = "Development", EmployeeCount = 85 },
            new EmployeeData { Country = "USA", Department = "Engineering", Team = "QA", EmployeeCount = 35 },
            new EmployeeData { Country = "India", Department = "Engineering", Team = "Development", EmployeeCount = 120 },
            new EmployeeData { Country = "India", Department = "Engineering", Team = "QA", EmployeeCount = 40 },
            new EmployeeData { Country = "UK", Department = "Sales", Team = "Field Sales", EmployeeCount = 25 },
            new EmployeeData { Country = "UK", Department = "Marketing", Team = "Digital", EmployeeCount = 20 }
        };
    }
}
```

### 4. XAML Configuration

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" ValueMemberPath="EmployeeCount"
                          Header="Employee Distribution" FontSize="20">
    
    <!-- Define hierarchy levels -->
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
    </sunburst:SfSunburstChart.Levels>
    
    <!-- Add legend -->
    <sunburst:SfSunburstChart.Legend>
        <sunburst:SunburstLegend DockPosition="Left"/>
    </sunburst:SfSunburstChart.Legend>
    
    <!-- Add data labels -->
    <sunburst:SfSunburstChart.DataLabelInfo>
        <sunburst:SunburstDataLabelInfo ShowLabel="True"/>
    </sunburst:SfSunburstChart.DataLabelInfo>
    
</sunburst:SfSunburstChart>
```

### 5. Set DataContext

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new ViewModel();
}
```

## Common Patterns

### Pattern 1: Hierarchical Data with 3-4 Levels

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding HierarchicalData}" 
                          ValueMemberPath="Value"
                          Palette="Metro">
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level1"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level2"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level3"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Level4"/>
    </sunburst:SfSunburstChart.Levels>
</sunburst:SfSunburstChart>
```

**Use when:** Displaying organizational structures, file systems, or multi-level sales data.

### Pattern 2: Interactive Chart with Selection and Tooltips

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Behaviors>
        <!-- Enable selection -->
        <sunburst:SunburstSelectionBehavior EnableSelection="True"
                                           SelectionMode="MouseClick"
                                           SelectionType="Single"/>
        
        <!-- Enable tooltips -->
        <sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                         ShowDuration="3000"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**Use when:** Users need to interact with segments for detailed information.

### Pattern 3: Drill-Down with Zooming

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Behaviors>
        <!-- Enable zooming for drill-down -->
        <sunburst:SunburstZoomingBehavior EnableZooming="True"
                                         ToolBarHorizontalAlignment="Right"
                                         ToolBarVerticalAlignment="Top"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**Use when:** Users need to explore deep hierarchies by zooming into specific categories.

### Pattern 4: Custom Palette with Gradient Brushes

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" ValueMemberPath="Value" Palette="Custom">
    
    <sunburst:SfSunburstChart.ColorModel>
        <sunburst:SunburstColorModel>
            <sunburst:SunburstColorModel.CustomBrushes>
                <LinearGradientBrush>
                    <GradientStop Color="#FF6B88E6" Offset="0"/>
                    <GradientStop Color="#FF2C5AA0" Offset="1"/>
                </LinearGradientBrush>
                <LinearGradientBrush>
                    <GradientStop Color="#FFE67E6B" Offset="0"/>
                    <GradientStop Color="#FFA0422C" Offset="1"/>
                </LinearGradientBrush>
            </sunburst:SunburstColorModel.CustomBrushes>
        </sunburst:SunburstColorModel>
    </sunburst:SfSunburstChart.ColorModel>
    
</sunburst:SfSunburstChart>
```

**Use when:** Matching corporate branding or creating custom color themes.

### Pattern 5: Animated Entry with Rotation

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value"
                          EnableAnimation="True"
                          AnimationType="Rotation"
                          AnimationDuration="1500">
    <!-- Chart configuration -->
</sunburst:SfSunburstChart>
```

**Use when:** Adding visual polish with smooth entry animations.

## Key Properties

### Core Data Binding
- **ItemsSource** – `IEnumerable` data source (hierarchical data collection)
- **ValueMemberPath** – Property name for segment size calculation
- **Levels** – Collection of `SunburstHierarchicalLevel` defining hierarchy
- **GroupMemberPath** (on levels) – Property for category grouping at each level

### Visual Elements
- **Header** – Chart title
- **Legend** – `SunburstLegend` for category identification
- **DataLabelInfo** – `SunburstDataLabelInfo` for segment labels
- **Palette** – Color scheme (Metro, AutumnBrights, Custom, etc.)
- **ColorModel** – Custom brush definitions

### Region Properties
- **Radius** – Chart outer radius (0.0 to 1.0, default: 0.9)
- **InnerRadius** – Center circle radius (0.0 to 1.0, default: 0.2)
- **StartAngle** – Starting angle in degrees (default: 0)
- **EndAngle** – Ending angle in degrees (default: 360)

### Animation
- **EnableAnimation** – Enable/disable entry animation
- **AnimationType** – FadeIn or Rotation
- **AnimationDuration** – Animation time in milliseconds

### Behaviors
- **SunburstToolTipBehavior** – Tooltip configuration
- **SunburstSelectionBehavior** – Selection interactivity
- **SunburstZoomingBehavior** – Drill-down/zooming

## Common Use Cases

### Use Case 1: Organizational Hierarchy Visualization
**Scenario:** Display company structure by region, department, team, and role.

**Solution:**
- 4 hierarchy levels (Region → Department → Team → Role)
- ValueMemberPath = "EmployeeCount"
- Enable selection to highlight departments
- Add tooltips showing employee counts

### Use Case 2: File System Explorer
**Scenario:** Visualize disk usage by folder hierarchy.

**Solution:**
- 3-5 hierarchy levels (Drive → Folder → Subfolder → File)
- ValueMemberPath = "SizeInBytes"
- Enable zooming for drill-down navigation
- Custom palette showing size with color intensity

### Use Case 3: Sales Dashboard
**Scenario:** Display sales by region, category, product, and SKU.

**Solution:**
- 4 hierarchy levels (Region → Category → Product → SKU)
- ValueMemberPath = "SalesAmount"
- Interactive legend with ToggleSegmentVisibility
- Custom data labels showing sales figures

### Use Case 4: Budget Allocation Viewer
**Scenario:** Show budget distribution across departments and projects.

**Solution:**
- 3 hierarchy levels (Department → Project → Phase)
- ValueMemberPath = "BudgetAmount"
- Custom color palette matching budget categories
- Selection highlighting with custom brushes

### Use Case 5: Website Analytics Hierarchy
**Scenario:** Display website traffic by country, page category, and page.

**Solution:**
- 3 hierarchy levels (Country → Category → Page)
- ValueMemberPath = "PageViews"
- Animated entry (FadeIn)
- Tooltips with additional metrics (bounce rate, time on page)

## Best Practices

1. **Data Structure:** Ensure your data model has distinct properties for each hierarchy level and a numeric property for ValueMemberPath.

2. **Level Count:** Limit to 3-5 levels for optimal readability. Too many levels create overcrowded outer rings.

3. **Value Consistency:** Ensure ValueMemberPath points to positive numeric values representing size/count/amount.

4. **Label Readability:** Use LabelOverflowMode="Trim" or "Hide" to prevent label clutter on smaller segments.

5. **Interactive Features:** Combine tooltips with selection for the best user experience. Add zooming for deep hierarchies.

6. **Performance:** For large datasets (1000+ items), consider data aggregation or limiting visible levels.

7. **Color Consistency:** Use predefined palettes for consistency or create custom palettes matching your application's theme.

8. **MVVM Pattern:** Implement INotifyPropertyChanged in your ViewModel for dynamic data updates.

## Troubleshooting

**Chart not displaying:**
- Verify ItemsSource binding is correct
- Check that ValueMemberPath matches a numeric property
- Ensure at least one SunburstHierarchicalLevel is defined
- Verify data collection is not empty

**Segments not sized correctly:**
- Confirm ValueMemberPath points to a numeric property
- Check for null or negative values in the data
- Ensure property name matches exactly (case-sensitive)

**Labels overlapping:**
- Set LabelOverflowMode="Hide" or "Trim"
- Reduce FontSize on DataLabelInfo
- Consider hiding labels on smaller segments

**Legend not showing:**
- Verify Legend property is set (not null)
- Check that DockPosition is valid
- Ensure first-level categories exist in data