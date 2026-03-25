# Legends

Legends help identify different series in a chart. This guide covers legend configuration and customization.

## Adding a Legend

```xml
<syncfusion:SfChart>
    <!-- Axes and Series -->
    
    <syncfusion:SfChart.Legend>
        <syncfusion:ChartLegend/>
    </syncfusion:SfChart.Legend>
</syncfusion:SfChart>
```

## Legend Positioning

Position the legend using `DockPosition`:

```xml
<syncfusion:ChartLegend DockPosition="Top"/>
```

**DockPosition options:**
- `Left` - Left side of chart
- `Right` - Right side (default)
- `Top` - Above chart
- `Bottom` - Below chart
- `Floating` - Custom position

### Floating Legend

Position legend at custom coordinates:

```xml
<syncfusion:ChartLegend DockPosition="Floating"
                        OffsetX="50"
                        OffsetY="20"/>
```

## Legend Visibility

Hide specific series from legend:

```xml
<syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                         XBindingPath="X"
                         YBindingPath="Y"
                         VisibilityOnLegend="Collapsed"/>
```

## Legend Orientation

Set legend item arrangement:

```xml
<syncfusion:ChartLegend Orientation="Horizontal"/>
```

**Options:** `Horizontal`, `Vertical`

## Series Label in Legend

Set series label that appears in legend:

```xml
<syncfusion:LineSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Sales"
                       Label="2024 Sales"/>
```

## Legend Icon Type

Customize the legend icon shape:

```xml
<syncfusion:LineSeries LegendIcon="Circle"/>
```

**LegendIcon options:**
- `SeriesType` - Matches series type (default)
- `Circle`
- `Cross`
- `Diamond`
- `Pentagon`
- `Rectangle`
- `Triangle`

## Legend Customization

### Background and Border

```xml
<syncfusion:ChartLegend Background="LightGray"
                        BorderBrush="Black"
                        BorderThickness="1"
                        CornerRadius="5"/>
```

### Icon Size

```xml
<syncfusion:ChartLegend IconWidth="20"
                        IconHeight="20"/>
```

## Legend Item Template

Create custom legend items:

```xml
<syncfusion:ChartLegend>
    <syncfusion:ChartLegend.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Ellipse Width="15" Height="15" 
                         Fill="{Binding Interior}"
                         Margin="5"/>
                <TextBlock Text="{Binding Label}" 
                           FontSize="12"
                           VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:ChartLegend.ItemTemplate>
</syncfusion:ChartLegend>
```

## Legend Toggle

Enable/disable series by clicking legend items:

```xml
<syncfusion:ChartLegend CheckBoxVisibility="Visible"/>
```

This allows users to show/hide series by checking/unchecking legend items.

## Multiple Legends

Add custom legends for specific series groups:

```xml
<syncfusion:SfChart.Legend>
    <syncfusion:ChartLegend/>
</syncfusion:SfChart.Legend>
```

## Legend Spacing

Control spacing between legend items:

```xml
<syncfusion:ChartLegend ItemMargin="10"/>
```

## Common Legend Configurations

### Top Horizontal Legend

```xml
<syncfusion:ChartLegend DockPosition="Top"
                        Orientation="Horizontal"
                        HorizontalAlignment="Center"/>
```

### Custom Styled Legend

```xml
<syncfusion:ChartLegend DockPosition="Right"
                        Background="WhiteSmoke"
                        BorderBrush="DarkGray"
                        BorderThickness="1"
                        CornerRadius="3"
                        IconWidth="25"
                        IconHeight="25"
                        ItemMargin="8">
    <syncfusion:ChartLegend.LabelStyle>
        <Style TargetType="TextBlock">
            <Setter Property="FontSize" Value="13"/>
            <Setter Property="Margin" Value="5,0,0,0"/>
        </Style>
    </syncfusion:ChartLegend.LabelStyle>
</syncfusion:ChartLegend>
```

### Legend with Checkboxes

```xml
<syncfusion:ChartLegend CheckBoxVisibility="Visible"
                        DockPosition="Bottom"
                        Orientation="Horizontal"/>
```

## Key Properties

| Property | Description |
|----------|-------------|
| `DockPosition` | Legend position (Left, Right, Top, Bottom, Floating) |
| `Orientation` | Horizontal or Vertical layout |
| `IconWidth` / `IconHeight` | Legend icon size |
| `CheckBoxVisibility` | Show checkboxes for toggling series |
| `ItemMargin` | Spacing between legend items |
| `OffsetX` / `OffsetY` | Custom position (Floating mode) |
| `Background` | Legend background color |
| `BorderBrush` / `BorderThickness` | Legend border |
| `CornerRadius` | Rounded corners |

## Series-Level Legend Properties

| Property | Description |
|----------|-------------|
| `Label` | Text shown in legend |
| `LegendIcon` | Icon shape for this series |
| `VisibilityOnLegend` | Show/hide series in legend |
