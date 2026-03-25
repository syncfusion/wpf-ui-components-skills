# Data Adornments in WPF SfChart3D

## Table of Contents
- [Overview](#overview)
- [Enabling Adornments](#enabling-adornments)
- [Markers](#markers)
- [Labels](#labels)
- [Connector Lines](#connector-lines)
- [Positioning](#positioning)
- [Smart Labels](#smart-labels)
- [Best Practices](#best-practices)

## Overview

Adornments (data markers) provide visual information about data points, enhancing chart readability. Each adornment can include:

- **Marker** - Symbol displayed at the (X, Y) data point
- **Label** - Text showing the data value or custom content
- **ConnectorLine** - Line connecting the data point to the label

Adornments help users quickly identify values, trends, and outliers in 3D visualizations.

## Enabling Adornments

Add adornments by configuring the `AdornmentsInfo` property on any series.

### Basic Configuration

**XAML:**
```xml
<chart:ColumnSeries3D 
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year" 
    YBindingPath="Plastic">
    
    <!-- Enable Adornments -->
    <chart:ColumnSeries3D.AdornmentsInfo>
        <chart:ChartAdornmentInfo3D ShowLabel="True"/>
    </chart:ColumnSeries3D.AdornmentsInfo>
    
</chart:ColumnSeries3D>
```

**C#:**
```csharp
ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic"
};

ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D() 
{ 
    ShowLabel = true 
};

series.AdornmentsInfo = adornmentInfo;
chart3D.Series.Add(series);
```

## Markers

Markers are symbols displayed at each data point to highlight their location.

### Enabling Markers

Set `ShowMarker="True"` and specify the symbol type:

**XAML:**
```xml
<chart:ColumnSeries3D 
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year"
    YBindingPath="Plastic">
    
    <chart:ColumnSeries3D.AdornmentsInfo>
        <chart:ChartAdornmentInfo3D 
            ShowMarker="True" 
            Symbol="Diamond" 
            SymbolInterior="Brown"/>
    </chart:ColumnSeries3D.AdornmentsInfo>
    
</chart:ColumnSeries3D>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowMarker = true,
    SymbolInterior = new SolidColorBrush(Colors.Brown),
    Symbol = ChartSymbol.Diamond
};

series.AdornmentsInfo = adornmentInfo;
```

### Available Marker Symbols

- **ChartSymbol.Diamond**
- **ChartSymbol.Ellipse**
- **ChartSymbol.Square**
- **ChartSymbol.Triangle**
- **ChartSymbol.InvertedTriangle**
- **ChartSymbol.Pentagon**
- **ChartSymbol.Hexagon**
- **ChartSymbol.Cross**
- **ChartSymbol.Plus**

### Customizing Marker Appearance

Control marker size, color, and stroke:

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowMarker="True" 
    Symbol="Triangle"
    SymbolHeight="15" 
    SymbolWidth="15"
    SymbolInterior="Brown" 
    SymbolStroke="DarkGray"
    AdornmentsPosition="Top"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowMarker = true,
    Symbol = ChartSymbol.Triangle,
    SymbolHeight = 15,
    SymbolWidth = 15,
    SymbolInterior = new SolidColorBrush(Colors.Brown),
    SymbolStroke = new SolidColorBrush(Colors.DarkGray),
    AdornmentsPosition = AdornmentsPosition.Top
};
```

**Properties:**
- **SymbolHeight** - Marker height in pixels
- **SymbolWidth** - Marker width in pixels
- **SymbolInterior** - Fill color
- **SymbolStroke** - Border color

### Custom Marker Template

Define custom marker shapes using `SymbolTemplate`:

**XAML:**
```xml
<Window.Resources>
    <DataTemplate x:Key="symbolTemplate">
        <Grid>
            <Grid Name="backgroundGrid" Width="24" Height="24">
                <Ellipse Fill="#FFE2DBDB" Name="Fill"/>
            </Grid>
            
            <Path Stretch="Uniform" Fill="Brown" Stroke="Green" 
                  Width="24" Height="24" RenderTransformOrigin="0.5,0.5">
                <Path.Data>
                    <PathGeometry FillRule="Nonzero" 
                                  Figures="M23.93,10.62L20.76,11.22 18.09,13.03..."/>
                </Path.Data>
                <Path.RenderTransform>
                    <TransformGroup>
                        <RotateTransform Angle="0"/>
                        <ScaleTransform ScaleX="1" ScaleY="1"/>
                    </TransformGroup>
                </Path.RenderTransform>
            </Path>
        </Grid>
    </DataTemplate>
</Window.Resources>

<chart:ColumnSeries3D ItemsSource="{Binding CategoricalData}" 
                      XBindingPath="Year" YBindingPath="Plastic">
    <chart:ColumnSeries3D.AdornmentsInfo>
        <chart:ChartAdornmentInfo3D 
            ShowMarker="True"
            AdornmentsPosition="Top"
            SymbolTemplate="{StaticResource symbolTemplate}"/>
    </chart:ColumnSeries3D.AdornmentsInfo>
</chart:ColumnSeries3D>
```

## Labels

Labels display data values or custom text at each data point.

### Label Content

Use `SegmentLabelContent` to define what to display:

**Available Options:**
- **YValue** - Y-axis value (default)
- **XValue** - X-axis value
- **LabelContentPath** - Custom property from data model
- **DateTime** - Date/time value
- **Percentage** - Percentage value (circular series)

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowLabel="True" 
    SegmentLabelContent="LabelContentPath"
    AdornmentsPosition="Top"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    SegmentLabelContent = LabelContent.LabelContentPath,
    AdornmentsPosition = AdornmentsPosition.Top
};
```

### Label Customization

**Border and Background:**

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowLabel="True"
    Foreground="Black" 
    Background="SkyBlue"
    BorderBrush="Aqua"  
    BorderThickness="1"
    FontFamily="Calibri"
    FontSize="11" 
    FontStyle="Italic"
    Margin="1"
    LabelPosition="Outer"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    LabelPosition = AdornmentsLabelPosition.Outer,
    Foreground = new SolidColorBrush(Colors.Black),
    BorderBrush = new SolidColorBrush(Colors.Aqua),
    Background = new SolidColorBrush(Colors.SkyBlue),
    BorderThickness = new Thickness(1),
    Margin = new Thickness(1),
    FontStyle = FontStyles.Italic,
    FontFamily = new FontFamily("Calibri"),
    FontSize = 11
};
```

**Customization Properties:**
- **Foreground** - Text color
- **Background** - Label background color
- **BorderBrush** - Border color
- **BorderThickness** - Border width
- **FontFamily** - Font type
- **FontSize** - Text size
- **FontStyle** - Normal, Italic, Oblique
- **Margin** - Spacing around label

### Label Rotation

Rotate labels to prevent overlapping:

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowLabel="True" 
    LabelRotationAngle="45"
    UseSeriesPalette="True"
    BorderBrush="White" 
    BorderThickness="1"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    LabelRotationAngle = 45,
    UseSeriesPalette = true,
    BorderBrush = new SolidColorBrush(Colors.White),
    BorderThickness = new Thickness(1)
};
```

### Label Formatting

Format numeric labels with `SegmentLabelFormat`:

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowLabel="True" 
    SegmentLabelFormat="#.###"
    LabelPosition="Outer"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    SegmentLabelFormat = "#.###",
    LabelPosition = AdornmentsLabelPosition.Outer
};
```

**Format Examples:**
- `"#.##"` - Two decimal places
- `"#,###"` - Thousands separator
- `"0.00"` - Always show two decimals
- `"C"` - Currency format
- `"P"` - Percentage format

### Apply Series Colors

Use series colors for label backgrounds:

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowLabel="True" 
    UseSeriesPalette="True"
    LabelPosition="Outer"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    LabelPosition = AdornmentsLabelPosition.Outer,
    UseSeriesPalette = true
};
```

For circular series (Pie, Doughnut), segment colors are applied to label backgrounds.

## Connector Lines

Connector lines link data points to their labels, useful when labels are positioned away from data points.

### Basic Connector Line

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowLabel="True"
    ShowConnectorLine="True" 
    ConnectorHeight="10"
    LabelPosition="Outer"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    ShowConnectorLine = true,
    ConnectorHeight = 10,
    LabelPosition = AdornmentsLabelPosition.Outer
};
```

**Properties:**
- **ShowConnectorLine** - Enable/disable connector lines
- **ConnectorHeight** - Line length in pixels
- **ConnectorRotationAngle** - Line rotation angle

### Connector Type (Circular Series Only)

Circular series support Line and Bezier connector types:

**XAML:**
```xml
<chart:PieSeries3D 
    ItemsSource="{Binding CategoricalData}" 
    ConnectorType="Line"
    LabelPosition="OutsideExtended"
    CircleCoefficient="0.7" 
    XBindingPath="Year" 
    YBindingPath="Plastic">
    
    <chart:PieSeries3D.AdornmentsInfo>
        <chart:ChartAdornmentInfo3D 
            ShowLabel="True" 
            ShowConnectorLine="True"
            ConnectorHeight="80"
            SegmentLabelContent="LabelContentPath"/>
    </chart:PieSeries3D.AdornmentsInfo>
    
</chart:PieSeries3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic",
    LabelPosition = CircularSeriesLabelPosition.OutsideExtended,
    ConnectorType = ConnectorMode.Line,
    CircleCoefficient = 0.7
};

ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    SegmentLabelContent = LabelContent.LabelContentPath,
    ShowConnectorLine = true,
    ConnectorHeight = 80
};

series.AdornmentsInfo = adornmentInfo;
```

**Note:** ConnectorType.StraightLine behavior is not applicable for 3D series.

### Custom Connector Style

Customize connector line appearance:

**XAML:**
```xml
<Window.Resources>
    <Style TargetType="Path" x:Key="lineStyle">
        <Setter Property="StrokeDashArray" Value="10,7,5"/>
        <Setter Property="Stroke" Value="Black"/>
    </Style>
</Window.Resources>

<chart:PieSeries3D 
    ItemsSource="{Binding CategoricalData}" 
    ConnectorType="Line" 
    LabelPosition="OutsideExtended"
    XBindingPath="Year" YBindingPath="Plastic">
    
    <chart:PieSeries3D.AdornmentsInfo>
        <chart:ChartAdornmentInfo3D 
            ShowLabel="True"
            ConnectorLineStyle="{StaticResource lineStyle}"
            ConnectorHeight="60" 
            ShowConnectorLine="True"/>
    </chart:PieSeries3D.AdornmentsInfo>
    
</chart:PieSeries3D>
```

## Positioning

Control where adornments appear relative to data points.

### AdornmentsPosition

Positions markers and labels within series:

**Options:**
- **Top** - Top edge of segment
- **Bottom** - Bottom edge of segment
- **TopAndBottom** - Both edges (specific series)

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowMarker="True" 
    Symbol="Diamond"
    SymbolHeight="10" 
    SymbolWidth="10"
    SymbolInterior="Brown"
    AdornmentsPosition="Top"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowMarker = true,
    Symbol = ChartSymbol.Diamond,
    SymbolHeight = 10,
    SymbolWidth = 10,
    SymbolInterior = new SolidColorBrush(Colors.Brown),
    AdornmentsPosition = AdornmentsPosition.Top
};
```

**Note:** Behavior varies by series type (Column, Bar, Line, etc.).

### LabelPosition

Advanced positioning for labels:

**Options:**
- **Default** - Series-specific default
- **Auto** - Automatic positioning to avoid overlap
- **Inner** - Inside data point
- **Outer** - Outside data point
- **Center** - Centered on data point

**XAML:**
```xml
<chart:ChartAdornmentInfo3D 
    ShowLabel="True"
    LabelPosition="Outer"
    SegmentLabelFormat="#.00"/>
```

**C#:**
```csharp
ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    LabelPosition = AdornmentsLabelPosition.Outer,
    SegmentLabelFormat = "#.00"
};
```

### Circular Series Label Position

For Pie and Doughnut series, use CircularSeriesLabelPosition:

**Options:**
- **Inside** - Labels inside segments
- **Outside** - Labels outside with short connectors
- **OutsideExtended** - Labels far outside with long connectors

**XAML:**
```xml
<chart:PieSeries3D 
    LabelPosition="OutsideExtended"
    ItemsSource="{Binding CategoricalData}" 
    XBindingPath="Year" 
    YBindingPath="Plastic">
    
    <chart:PieSeries3D.AdornmentsInfo>
        <chart:ChartAdornmentInfo3D 
            ShowLabel="True" 
            ShowConnectorLine="True"/>
    </chart:PieSeries3D.AdornmentsInfo>
    
</chart:PieSeries3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    LabelPosition = CircularSeriesLabelPosition.OutsideExtended,
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic"
};

ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    ShowConnectorLine = true
};

series.AdornmentsInfo = adornmentInfo;
```

## Smart Labels

Smart Labels automatically adjust label positions to prevent overlapping in circular series.

### Enabling Smart Labels

**XAML:**
```xml
<chart:PieSeries3D  
    ItemsSource="{Binding CategoricalData}" 
    ConnectorType="Bezier"
    XBindingPath="Year"
    YBindingPath="Plastic" 
    EnableSmartLabels="True" 
    LabelPosition="OutsideExtended"
    ExplodeAll="True" 
    ExplodeRadius="3">
    
    <chart:PieSeries3D.AdornmentsInfo>
        <chart:ChartAdornmentInfo3D 
            ShowLabel="True"
            HorizontalAlignment="Center" 
            VerticalAlignment="Center"
            ShowConnectorLine="True"/>
    </chart:PieSeries3D.AdornmentsInfo>
    
</chart:PieSeries3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Plastic",
    LabelPosition = CircularSeriesLabelPosition.Outer,
    ConnectorType = ConnectorMode.Bezier,
    EnableSmartLabels = true,
    ExplodeAll = true,
    ExplodeRadius = 3
};

ChartAdornmentInfo3D adornmentInfo = new ChartAdornmentInfo3D()
{
    ShowLabel = true,
    ShowConnectorLine = true,
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};

series.AdornmentsInfo = adornmentInfo;
```

**When to use:**
- Many data points in circular series
- Labels overlapping
- OutsideExtended label position
- Improved readability needed

## Best Practices

### Performance Considerations

**For Large Datasets:**
- Show labels for key points only
- Use simpler marker symbols
- Avoid connector lines on every point
- Consider data aggregation

### Visual Clarity

**Marker Selection:**
- Use contrasting colors for markers
- Choose appropriate symbol sizes (10-20px typical)
- Avoid too many different symbols in one chart

**Label Readability:**
- Use adequate font sizes (10-14px)
- Apply contrasting backgrounds
- Rotate labels if overlapping
- Format numbers appropriately

**Connector Lines:**
- Use for circular series with outside labels
- Keep ConnectorHeight moderate (40-80px)
- Apply custom styles sparingly

### Common Patterns

**Pattern 1: Markers Only**
```xml
<chart:ChartAdornmentInfo3D ShowMarker="True" Symbol="Ellipse"/>
```

**Pattern 2: Labels Only**
```xml
<chart:ChartAdornmentInfo3D ShowLabel="True" UseSeriesPalette="True"/>
```

**Pattern 3: Markers + Labels**
```xml
<chart:ChartAdornmentInfo3D 
    ShowMarker="True" 
    ShowLabel="True"
    Symbol="Diamond"
    LabelPosition="Outer"/>
```

**Pattern 4: Full Adornment (Circular)**
```xml
<chart:ChartAdornmentInfo3D 
    ShowLabel="True"
    ShowConnectorLine="True"
    UseSeriesPalette="True"
    ConnectorHeight="60"/>
```

### Edge Cases

**Issue: Labels Overlapping**
- Enable Smart Labels for circular series
- Rotate labels with LabelRotationAngle
- Use Auto LabelPosition
- Reduce number of data points

**Issue: Markers Too Small/Large**
- Adjust SymbolHeight and SymbolWidth
- Test at different chart sizes
- Maintain aspect ratio

**Issue: Connector Lines Not Showing**
- Ensure LabelPosition is Outer or OutsideExtended
- Set ConnectorHeight > 0
- Verify ShowConnectorLine="True"

**Issue: Custom Colors Not Applying**
- Check UseSeriesPalette=False when using custom colors
- Verify brush values are valid
- Apply to correct property (SymbolInterior vs Background)
