# Series Types in WPF SfChart3D

## Table of Contents
- [Overview](#overview)
- [Common Series Properties](#common-series-properties)
- [Column Charts](#column-charts)
- [Bar Charts](#bar-charts)
- [Line Charts](#line-charts)
- [Scatter Charts](#scatter-charts)
- [Area Charts](#area-charts)
- [Stacking Charts](#stacking-charts)
- [Circular Charts](#circular-charts)
- [Choosing the Right Series Type](#choosing-the-right-series-type)

## Overview

ChartSeries is the visual representation of data in SfChart3D. The control offers 11+ types of 3D series, each designed for specific data visualization scenarios. Choose the appropriate series type based on your data structure and visualization goals.

**Available Series Types:**
- Column3D
- Bar3D
- Line3D
- Scatter3D
- Area3D
- StackingColumn3D
- StackingColumn1003D
- StackingBar3D
- StackingBar1003D
- Pie3D
- Doughnut3D

## Common Series Properties

These properties are common across most series types:

**Data Binding:**
- `XBindingPath` - String property representing X values
- `YBindingPath` - String property representing Y values
- `ItemsSource` - Data collection to bind

**Appearance:**
- `Interior` - Brush to fill the series
- `Label` - Series name for legend
- `Palette` - Color palette selection

**Interactivity:**
- `ShowTooltip` - Enable/disable tooltips
- `AdornmentsInfo` - Data label configuration

## Column Charts

**ColumnSeries3D** displays discrete rectangles for given values, ideal for comparing values across categories.

**When to use:**
- Comparing values across different categories
- Showing discrete data points
- Highlighting individual values in time series

**Basic Implementation:**

**XAML:**
```xml
<chart:ColumnSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year" 
    YBindingPath="Metal">
</chart:ColumnSeries3D>
```

**C#:**
```csharp
ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Metal"
};

chart3D.Series.Add(series);
```

### Spacing Configuration

Control the spacing between segments and series:


**SegmentSpacing Property:**
Sets spacing between segments when multiple series are present (0-1 range):

**XAML:**
```xml
<chart:ColumnSeries3D 
    SegmentSpacing="0.6" 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year" 
    YBindingPath="Plastic">
</chart:ColumnSeries3D>

<chart:ColumnSeries3D 
    SegmentSpacing="0.6" 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year" 
    YBindingPath="Iron">
</chart:ColumnSeries3D>
```

**C#:**
```csharp
ColumnSeries3D series1 = new ColumnSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic",
    SegmentSpacing = 0.6
};

ColumnSeries3D series2 = new ColumnSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    SegmentSpacing = 0.6
};

chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
```

## Bar Charts

**BarSeries3D** are similar to column series but with horizontal orientation.

**When to use:**
- Long category names that don't fit well on X-axis
- Emphasizing ranking or comparison
- Space-constrained vertical layouts

**Basic Implementation:**

**XAML:**
```xml
<chart:BarSeries3D 
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year"
    YBindingPath="Plastic">
</chart:BarSeries3D>
```

**C#:**
```csharp
BarSeries3D series = new BarSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic"
};

chart3D.Series.Add(series);
```

**Note:** Bar series supports the same Spacing and SegmentSpacing properties as Column series.

## Line Charts

**LineSeries3D** connects data points with straight lines, ideal for showing trends over time or continuous data.

**When to use:**
- Showing trends and patterns over time
- Displaying continuous data
- Comparing multiple datasets' trajectories

**Basic Implementation:**

**XAML:**
```xml
<chart:LineSeries3D 
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year"
    YBindingPath="Metal">
</chart:LineSeries3D>
```

**C#:**
```csharp
LineSeries3D series = new LineSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Metal"
};

chart3D.Series.Add(series);
```

## Scatter Charts

**ScatterSeries3D** represents each data point as a rectangle, useful for showing distribution and correlation.

**When to use:**
- Showing data point distribution
- Identifying correlations between variables
- Plotting mathematical functions
- Displaying outliers

**Basic Implementation:**

**XAML:**
```xml
<chart:ScatterSeries3D  
    ItemsSource="{Binding DataPoints}"  
    XBindingPath="Year" 
    YBindingPath="Germany">
</chart:ScatterSeries3D>
```

**C#:**
```csharp
ScatterSeries3D series = new ScatterSeries3D()
{
    ItemsSource = new ViewModel().DataPoints,
    XBindingPath = "Year",
    YBindingPath = "Germany"
};

chart3D.Series.Add(series);
```

### Customizing Scatter Size

Control the size of scatter points:

**XAML:**
```xml
<chart:ScatterSeries3D  
    ScatterHeight="15"
    ScatterWidth="15"
    ItemsSource="{Binding DataPoints}"  
    XBindingPath="Year" 
    YBindingPath="Germany">
</chart:ScatterSeries3D>
```

**C#:**
```csharp
ScatterSeries3D series = new ScatterSeries3D()
{
    ItemsSource = new ViewModel().DataPoints,
    XBindingPath = "Year",
    YBindingPath = "Germany",
    ScatterHeight = 15,
    ScatterWidth = 15
};

chart3D.Series.Add(series);
```

## Area Charts

**AreaSeries3D** renders a collection of line segments forming a closed loop area, filled with the specified color.

**When to use:**
- Showing cumulative totals over time
- Visualizing volume or magnitude
- Comparing part-to-whole relationships over time

**Basic Implementation:**

**XAML:**
```xml
<chart:AreaSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Plastic">
</chart:AreaSeries3D>
```

**C#:**
```csharp
AreaSeries3D series = new AreaSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath="Plastic"
};

chart3D.Series.Add(series);
```

## Stacking Charts

Stacking charts display multiple series stacked on top of each other, showing both individual values and cumulative totals.

### Stacking Column Charts

**StackingColumnSeries3D** stacks multiple column series vertically.

**When to use:**
- Showing part-to-whole relationships across categories
- Comparing contributions of different components
- Visualizing cumulative totals

**XAML:**
```xml
<chart:StackingColumnSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Plastic">
</chart:StackingColumnSeries3D>

<chart:StackingColumnSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Iron">
</chart:StackingColumnSeries3D>

<chart:StackingColumnSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Metal">
</chart:StackingColumnSeries3D>
```

**C#:**
```csharp
StackingColumnSeries3D series1 = new StackingColumnSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic"
};

StackingColumnSeries3D series2 = new StackingColumnSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron"
};

StackingColumnSeries3D series3 = new StackingColumnSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Metal"
};

chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
chart3D.Series.Add(series3);
```

### Stacking Column 100 Charts

**StackingColumn100Series3D** ensures the cumulative portion always totals 100%.

**When to use:**
- Showing percentage composition over time
- Comparing proportions across categories
- Normalizing data for better comparison

**XAML:**
```xml
<chart:StackingColumn100Series3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Plastic">
</chart:StackingColumn100Series3D>

<chart:StackingColumn100Series3D 
    Interior="Brown" 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year" 
    YBindingPath="Iron">
</chart:StackingColumn100Series3D>
```

**C#:**
```csharp
StackingColumn100Series3D series1 = new StackingColumn100Series3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic"
};

StackingColumn100Series3D series2 = new StackingColumn100Series3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    Interior = new SolidColorBrush(Colors.Brown)
};

chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
```

### Stacking Bar Charts

**StackingBarSeries3D** stacks bar series horizontally.

**When to use:**
- Part-to-whole with long category names
- Horizontal space optimization
- Ranking with composition breakdown

**XAML:**
```xml
<chart:StackingBarSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Plastic">
</chart:StackingBarSeries3D>

<chart:StackingBarSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Iron">
</chart:StackingBarSeries3D>

<chart:StackingBarSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Metal">
</chart:StackingBarSeries3D>
```

**C#:**
```csharp
StackingBarSeries3D series1 = new StackingBarSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic"
};

StackingBarSeries3D series2 = new StackingBarSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron"
};

StackingBarSeries3D series3 = new StackingBarSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Metal"
};

chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
chart3D.Series.Add(series3);
```

### Stacking Bar 100 Charts

**StackingBar100Series3D** displays horizontal stacking with 100% cumulative portions.

**When to use:**
- Horizontal percentage compositions
- Proportional comparisons with long labels

**XAML:**
```xml
<chart:StackingBar100Series3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Plastic">
</chart:StackingBar100Series3D>

<chart:StackingBar100Series3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Iron">
</chart:StackingBar100Series3D>
```

**C#:**
```csharp
StackingBar100Series3D series1 = new StackingBar100Series3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic"
};

StackingBar100Series3D series2 = new StackingBar100Series3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron"
};

chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
```

## Circular Charts

Circular charts display data as sectors of a circle, showing proportional relationships.

### Pie Charts

**PieSeries3D** divides a circle into sectors illustrating numerical proportions.

**When to use:**
- Showing part-to-whole relationships
- Displaying market share or budget allocation
- Visualizing survey results with few categories

**Basic Implementation:**

**XAML:**
```xml
<chart:PieSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"
    YBindingPath="Iron">
</chart:PieSeries3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron"
};

chart3D.Series.Add(series);
```

### Doughnut Charts

**DoughnutSeries3D** is similar to pie charts but with a hollow center.

**When to use:**
- Emphasizing individual values over total
- Adding additional information in the center
- Modern aesthetic preference

**Basic Implementation:**

**XAML:**
```xml
<chart:DoughnutSeries3D 
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year"                        
    YBindingPath="Iron">
</chart:DoughnutSeries3D>
```

**C#:**
```csharp
DoughnutSeries3D series = new DoughnutSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron"
};

chart3D.Series.Add(series);
```

### Coefficient Configuration

Control the inner circle size for circular charts:

**DoughnutCoefficient:**
Sets the inner radius for doughnut charts (0-1 range):

**XAML:**
```xml
<chart:DoughnutSeries3D 
    DoughnutCoefficient="0.5"   
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year" 
    YBindingPath="Iron">
</chart:DoughnutSeries3D>
```

**C#:**
```csharp
DoughnutSeries3D series = new DoughnutSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    DoughnutCoefficient = 0.5
};

chart3D.Series.Add(series);
```

**CircleCoefficient:**
Adjusts the overall circle size for pie charts (0-1 range):

**XAML:**
```xml
<chart:PieSeries3D 
    CircleCoefficient="0.8"   
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year" 
    YBindingPath="Iron">
</chart:PieSeries3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    CircleCoefficient = 0.8
};

chart3D.Series.Add(series);
```

### Semi-Circular Charts

Create semi or quarter circular charts using StartAngle and EndAngle properties:

**Semi-Doughnut:**

**XAML:**
```xml
<chart:DoughnutSeries3D 
    StartAngle="180" 
    EndAngle="360" 
    DoughnutCoefficient="0.7"
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year" 
    YBindingPath="Iron">
</chart:DoughnutSeries3D>
```

**C#:**
```csharp
DoughnutSeries3D series = new DoughnutSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    DoughnutCoefficient = 0.7,
    StartAngle = 180,
    EndAngle = 360
};

chart3D.Series.Add(series);
```

**Semi-Pie:**

**XAML:**
```xml
<chart:PieSeries3D 
    StartAngle="180" 
    EndAngle="360" 
    CircleCoefficient="0.7"
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year" 
    YBindingPath="Iron">
</chart:PieSeries3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    CircleCoefficient = 0.7,
    StartAngle = 180,
    EndAngle = 360
};

chart3D.Series.Add(series);
```

### Dynamic Explode

Enable interactive segment explosion for circular series:

**ExplodeOnMouseClick:**
Explode segments on user click:

**XAML:**
```xml
<chart:SfChart3D EnableRotation="True" Tilt="-30" Rotation="45"
        Depth="30" PerspectiveAngle="90" Width="500" Height="500">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D Header="Year"/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D Header="Metal"/>
    </chart:SfChart3D.SecondaryAxis>           
    
    <chart:PieSeries3D  
        ExplodeOnMouseClick="True" 
        ItemsSource="{Binding CategoricalData}"  
        XBindingPath="Year" 
        YBindingPath="Iron">
    </chart:PieSeries3D>
    
</chart:SfChart3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    ExplodeOnMouseClick = true
};

chart3D.Series.Add(series);
```

**ExplodeIndex:**
Explode a specific segment by index:

```csharp
pieSeries.ExplodeIndex = 2; // Explodes the third segment
```

**ExplodeAll:**
Explode all segments:

```csharp
pieSeries.ExplodeAll = true;
pieSeries.ExplodeRadius = 3; // Optional: Set explode distance
```

## Choosing the Right Series Type

| Series Type | Best For | Avoid When |
|-------------|----------|-----------|
| **Column** | Discrete comparisons, category data | Too many categories (use Bar) |
| **Bar** | Long category names, rankings | Showing trends over time (use Line) |
| **Line** | Trends, time-series, continuous data | Discrete categories (use Column) |
| **Scatter** | Distributions, correlations, outliers | Part-to-whole (use Pie) |
| **Area** | Cumulative totals, volume over time | Precise value comparisons |
| **Stacking Column/Bar** | Part-to-whole across categories | More than 5-6 series (cluttered) |
| **Stacking 100** | Percentage compositions | When absolute values matter |
| **Pie** | Part-to-whole with few categories | More than 7 categories |
| **Doughnut** | Part-to-whole with central info | Comparing multiple datasets |

## Edge Cases and Tips

**Multiple Series:**
- Use consistent Spacing and SegmentSpacing across all series
- Apply different colors using palettes or Interior property
- Add Label property for legend identification

**Data Binding:**
- Always use ObservableCollection for dynamic updates
- Ensure binding path names match property names exactly
- Handle null or empty data collections gracefully

**Performance:**
- Limit data points for smooth 3D rotation (500-1000 points optimal)
- Use simpler series types for large datasets
- Consider data sampling for very large collections

**Visual Clarity:**
- Limit stacking series to 3-5 for readability
- Use contrasting colors for adjacent segments
- Add data labels for important values only
