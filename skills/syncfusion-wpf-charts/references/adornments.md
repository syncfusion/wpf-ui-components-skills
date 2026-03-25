# Adornments (Data Labels and Markers)

Adornments display additional information on data points through labels and markers (symbols).

## Data Labels

Display values on data points.

### Enable Data Labels

```xml
<syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                         XBindingPath="Month"
                         YBindingPath="Sales">
    <syncfusion:ColumnSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="True"/>
    </syncfusion:ColumnSeries.AdornmentsInfo>
</syncfusion:ColumnSeries>
```

### Label Content

Customize what the label displays:

```xml
<syncfusion:ChartAdornmentInfo ShowLabel="True"
                               LabelPosition="Outer"
                               SegmentLabelContent="LabelContentPath"/>
```

**SegmentLabelContent options:**
- `YValue` - Y-axis value (default)
- `XValue` - X-axis value
- `Percentage` - Percentage value (for Pie/Doughnut)
- `LabelContentPath` - Custom property from data model
- `DateTime` - DateTime value

### Label Format

Format label text:

```xml
<syncfusion:ChartAdornmentInfo ShowLabel="True"
                               SegmentLabelFormat="$0.00"/>
```

Common formats:
- `$0` - Currency
- `0.00` - Decimal places
- `0%` - Percentage
- `MMM dd` - Date format

### Label Positioning

```xml
<syncfusion:ChartAdornmentInfo ShowLabel="True"
                               LabelPosition="Top"/>
```

**LabelPosition options:**
- `Auto` - Automatic positioning
- `Top` - Above data point
- `Bottom` - Below data point
- `Center` - Center of data point
- `Inner` - Inside segment
- `Outer` - Outside segment

### Label Rotation

```xml
<syncfusion:ChartAdornmentInfo ShowLabel="True"
                               LabelRotationAngle="45"/>
```

### Label Style

Customize label appearance:

```xml
<syncfusion:ChartAdornmentInfo ShowLabel="True">
    <syncfusion:ChartAdornmentInfo.LabelTemplate>
        <DataTemplate>
            <Border Background="DarkBlue" 
                    CornerRadius="3" 
                    Padding="5">
                <TextBlock Text="{Binding Item.Sales, StringFormat='${0:N0}'}"
                           Foreground="White"
                           FontSize="12"
                           FontWeight="Bold"/>
            </Border>
        </DataTemplate>
    </syncfusion:ChartAdornmentInfo.LabelTemplate>
</syncfusion:ChartAdornmentInfo>
```

## Data Markers

Display symbols at data points.

### Enable Markers

```xml
<syncfusion:LineSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Sales">
    <syncfusion:LineSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowMarker="True"
                                       Symbol="Ellipse"/>
    </syncfusion:LineSeries.AdornmentsInfo>
</syncfusion:LineSeries>
```

### Marker Symbols

```xml
<syncfusion:ChartAdornmentInfo ShowMarker="True"
                               Symbol="Diamond"/>
```

**Symbol options:**
- `Ellipse`
- `Cross`
- `Diamond`
- `Hexagon`
- `InvertedTriangle`
- `Pentagon`
- `Plus`
- `Rectangle`
- `Triangle`

### Marker Size

```xml
<syncfusion:ChartAdornmentInfo ShowMarker="True"
                               Symbol="Ellipse"
                               SymbolHeight="12"
                               SymbolWidth="12"/>
```

### Marker Style

Customize marker appearance:

```xml
<syncfusion:ChartAdornmentInfo ShowMarker="True"
                               Symbol="Ellipse"
                               SymbolInterior="Red"
                               SymbolStroke="DarkRed"/>
```

### Custom Marker Template

```xml
<syncfusion:ChartAdornmentInfo ShowMarker="True">
    <syncfusion:ChartAdornmentInfo.SymbolTemplate>
        <DataTemplate>
            <Ellipse Width="10" Height="10" 
                     Fill="Orange" 
                     Stroke="DarkOrange"
                     StrokeThickness="2"/>
        </DataTemplate>
    </syncfusion:ChartAdornmentInfo.SymbolTemplate>
</syncfusion:ChartAdornmentInfo>
```

## Combining Labels and Markers

Display both labels and markers:

```xml
<syncfusion:LineSeries>
    <syncfusion:LineSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="True"
                                       ShowMarker="True"
                                       Symbol="Ellipse"
                                       LabelPosition="Top"
                                       SegmentLabelFormat="0.0"/>
    </syncfusion:LineSeries.AdornmentsInfo>
</syncfusion:LineSeries>
```

## Smart Label Alignment

Automatically adjust labels to prevent overlap:

```xml
<syncfusion:ChartAdornmentInfo ShowLabel="True"
                               UseSeriesPalette="True"
                               ConnectorLineStyle="{StaticResource connectorStyle}"/>
```

## Connector Lines

Connect labels to data points:

```xml
<syncfusion:ChartAdornmentInfo ShowLabel="True"
                               ShowConnectorLine="True"
                               ConnectorHeight="20">
    <syncfusion:ChartAdornmentInfo.ConnectorLineStyle>
        <Style TargetType="Path">
            <Setter Property="Stroke" Value="Gray"/>
            <Setter Property="StrokeDashArray" Value="1,1"/>
        </Style>
    </syncfusion:ChartAdornmentInfo.ConnectorLineStyle>
</syncfusion:ChartAdornmentInfo>
```

## Pie/Doughnut Adornments

Special adornment features for circular charts:

```xml
<syncfusion:PieSeries>
    <syncfusion:PieSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="True"
                                       SegmentLabelContent="Percentage"
                                       SegmentLabelFormat="0'%'"
                                       LabelPosition="OutsideExtended"
                                       ShowConnectorLine="True"/>
    </syncfusion:PieSeries.AdornmentsInfo>
</syncfusion:PieSeries>
```

## Common Adornment Patterns

### Pattern 1: Simple Value Labels

```xml
<syncfusion:ColumnSeries>
    <syncfusion:ColumnSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="True"
                                       LabelPosition="Top"
                                       SegmentLabelFormat="0"/>
    </syncfusion:ColumnSeries.AdornmentsInfo>
</syncfusion:ColumnSeries>
```

### Pattern 2: Styled Labels with Markers

```xml
<syncfusion:LineSeries>
    <syncfusion:LineSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="True"
                                       ShowMarker="True"
                                       Symbol="Ellipse"
                                       SymbolHeight="10"
                                       SymbolWidth="10"
                                       SymbolInterior="White"
                                       SymbolStroke="{Binding Interior, RelativeSource={RelativeSource Mode=Self}}"
                                       LabelPosition="Top"
                                       Foreground="Black"/>
    </syncfusion:LineSeries.AdornmentsInfo>
</syncfusion:LineSeries>
```

### Pattern 3: Percentage Labels for Pie Chart

```xml
<syncfusion:PieSeries>
    <syncfusion:PieSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="True"
                                       SegmentLabelContent="Percentage"
                                       SegmentLabelFormat="0.0'%'"
                                       LabelPosition="Outside"
                                       ShowConnectorLine="True"
                                       ConnectorHeight="30"/>
    </syncfusion:PieSeries.AdornmentsInfo>
</syncfusion:PieSeries>
```

## Key Properties

### Label Properties
- `ShowLabel` - Enable/disable labels
- `LabelPosition` - Label placement
- `SegmentLabelFormat` - Format string
- `LabelRotationAngle` - Rotation in degrees
- `SegmentLabelContent` - What to display
- `LabelTemplate` - Custom label template

### Marker Properties
- `ShowMarker` - Enable/disable markers
- `Symbol` - Marker shape
- `SymbolHeight` / `SymbolWidth` - Marker size
- `SymbolInterior` - Fill color
- `SymbolStroke` - Border color
- `SymbolTemplate` - Custom marker template

### Connector Properties
- `ShowConnectorLine` - Enable connector lines
- `ConnectorHeight` - Connector line length
- `ConnectorLineStyle` - Connector line styling
