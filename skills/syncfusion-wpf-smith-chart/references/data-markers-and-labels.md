# Data Markers and Labels

## Table of Contents
- [Overview](#overview)
- [Data Markers](#data-markers)
- [Marker Types](#marker-types)
- [Customizing Markers](#customizing-markers)
- [Custom Marker Templates](#custom-marker-templates)
- [Data Labels](#data-labels)
- [Label Styling](#label-styling)
- [Custom Label Templates](#custom-label-templates)
- [Common Patterns](#common-patterns)

## Overview

Data markers and labels enhance SmithChart visualization by:
- **Markers**: Visual indicators at each data point
- **Labels**: Text displaying data values

These features provide detailed information about individual data points, making it easier to analyze transmission line characteristics.

**Key Features:**
- Multiple built-in marker shapes (Circle, Rectangle, Diamond, etc.)
- Customizable marker size, color, and stroke
- Custom marker templates for unique visualizations
- Automatic data label positioning with collision detection
- Customizable label styling and formatting
- Custom label templates for specialized displays

## Data Markers

### Enabling Markers

Enable markers on a LineSeries:

```xml
<syncfusion:LineSeries ShowMarker="True" 
                       MarkerType="Circle">
</syncfusion:LineSeries>
```

```csharp
series.ShowMarker = true;
series.MarkerType = MarkerType.Circle;
```

### Basic Marker Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowMarker` | bool | false | Enable/disable markers |
| `MarkerType` | MarkerType | Circle | Marker shape |
| `MarkerHeight` | double | 8 | Marker height in pixels |
| `MarkerWidth` | double | 8 | Marker width in pixels |
| `MarkerInterior` | Brush | Auto | Marker fill color |
| `MarkerStroke` | Brush | Auto | Marker outline color |
| `MarkerTemplate` | DataTemplate | null | Custom marker template |

## Marker Types

### Available Built-in Markers

SmithChart provides several predefined marker shapes:

```csharp
public enum MarkerType
{
    Circle,       // Round marker
    Rectangle,    // Square/rectangular marker
    Diamond,      // Diamond-shaped marker
    Triangle,     // Triangle marker
    Pentagon,     // Five-sided marker
    Plus,         // Plus/cross marker
    Cross,        // X-shaped marker
    Custom        // Use MarkerTemplate
}
```

### Marker Type Examples

**Circle (Default):**
```xml
<syncfusion:LineSeries ShowMarker="True" MarkerType="Circle"/>
```

**Rectangle:**
```xml
<syncfusion:LineSeries ShowMarker="True" MarkerType="Rectangle"/>
```

**Diamond:**
```xml
<syncfusion:LineSeries ShowMarker="True" MarkerType="Diamond"/>
```

**Triangle:**
```xml
<syncfusion:LineSeries ShowMarker="True" MarkerType="Triangle"/>
```

**Pentagon:**
```xml
<syncfusion:LineSeries ShowMarker="True" MarkerType="Pentagon"/>
```

**Cross:**
```xml
<syncfusion:LineSeries ShowMarker="True" MarkerType="Cross"/>
```

### Choosing Marker Types

**Use Circle for:**
- General-purpose data points
- Clean, simple appearance
- Standard scientific charts

**Use Rectangle/Diamond for:**
- Distinguishing multiple series
- Creating visual variety
- Emphasizing data points

**Use Triangle/Pentagon/Hexagon for:**
- Multiple series comparison (different shapes per series)
- Specialized data point types
- Creating unique visualizations

**Use Plus/Cross for:**
- Intersection points
- Special markers that don't obscure data
- Lightweight visual indicators

## Customizing Markers

### Marker Size

Control marker dimensions:

```xml
<syncfusion:LineSeries ShowMarker="True" 
                       MarkerType="Circle"
                       MarkerHeight="12" 
                       MarkerWidth="12">
</syncfusion:LineSeries>
```

```csharp
series.ShowMarker = true;
series.MarkerType = MarkerType.Circle;
series.MarkerHeight = 12;
series.MarkerWidth = 12;
```

**Size Guidelines:**
- **4-6px**: Small, subtle markers
- **8-10px**: Standard size (default)
- **12-16px**: Large, emphasized markers
- **16+px**: Very prominent (use sparingly)

### Marker Colors

Customize marker fill and outline:

```xml
<syncfusion:LineSeries ShowMarker="True" 
                       MarkerType="Circle"
                       MarkerHeight="12" 
                       MarkerWidth="12"
                       MarkerInterior="Red" 
                       MarkerStroke="Yellow">
</syncfusion:LineSeries>
```

```csharp
series.ShowMarker = true;
series.MarkerType = MarkerType.Circle;
series.MarkerHeight = 12;
series.MarkerWidth = 12;
series.MarkerInterior = new SolidColorBrush(Colors.Red);
series.MarkerStroke = new SolidColorBrush(Colors.Yellow);
```

### Complete Marker Customization Example

```csharp
LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    ShowMarker = true,
    MarkerType = MarkerType.Diamond,
    MarkerHeight = 14,
    MarkerWidth = 14,
    MarkerInterior = new SolidColorBrush(Colors.DarkBlue),
    MarkerStroke = new SolidColorBrush(Colors.White),
    Label = "Transmission"
};
chart.Series.Add(series);
```

### Multiple Series with Different Markers

```xml
<syncfusion:SfSmithChart>
    <!-- Series 1: Circles -->
    <syncfusion:LineSeries ShowMarker="True" 
                           MarkerType="Circle"
                           MarkerInterior="Blue"
                           Label="50Ω Line"
                           ItemsSource="{Binding Data1}">
    </syncfusion:LineSeries>
    
    <!-- Series 2: Diamonds -->
    <syncfusion:LineSeries ShowMarker="True" 
                           MarkerType="Diamond"
                           MarkerInterior="Orange"
                           Label="75Ω Line"
                           ItemsSource="{Binding Data2}">
    </syncfusion:LineSeries>
    
    <!-- Series 3: Rectangles -->
    <syncfusion:LineSeries ShowMarker="True" 
                           MarkerType="Rectangle"
                           MarkerInterior="Green"
                           Label="100Ω Line"
                           ItemsSource="{Binding Data3}">
    </syncfusion:LineSeries>
</syncfusion:SfSmithChart>
```

## Custom Marker Templates

Create unique markers using DataTemplate.

### Defining Custom Marker Template

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <!-- Custom Ellipse Marker -->
        <DataTemplate x:Key="EllipseMarker">
            <Ellipse Stretch="Fill" 
                     Fill="{Binding Interior}" 
                     Stroke="{Binding Stroke}" 
                     StrokeThickness="2" 
                     Width="10" 
                     Height="17"/>
        </DataTemplate>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:LineSeries ShowMarker="True" 
                           MarkerType="Custom"
                           MarkerTemplate="{StaticResource EllipseMarker}">
    </syncfusion:LineSeries>
</syncfusion:SfSmithChart>
```

**Important:** Set `MarkerType="Custom"` when using MarkerTemplate.

### Custom Marker Template Examples

**Star Marker:**
```xml
<DataTemplate x:Key="StarMarker">
    <Path Fill="{Binding Interior}" 
          Stroke="{Binding Stroke}"
          Data="M 10,0 L 12,8 L 20,8 L 13,13 L 16,21 L 10,16 L 4,21 L 7,13 L 0,8 L 8,8 Z"/>
</DataTemplate>
```

**Image Marker:**
```xml
<DataTemplate x:Key="ImageMarker">
    <Image Source="/Images/marker.png" 
           Width="16" 
           Height="16"/>
</DataTemplate>
```

**Composite Marker:**
```xml
<DataTemplate x:Key="CompositeMarker">
    <Grid>
        <Ellipse Fill="{Binding Interior}" Width="12" Height="12"/>
        <Rectangle Fill="White" Width="4" Height="4"/>
    </Grid>
</DataTemplate>
```

### Template Binding Context

The DataTemplate binding context includes:
- **Interior**: Series marker fill color
- **Stroke**: Series marker stroke color
- **Other marker properties**

Use these bindings to maintain consistency:
```xml
<Ellipse Fill="{Binding Interior}" />
```

## Data Labels

Data labels display values at each data point with automatic positioning and collision detection.

### Enabling Data Labels

```xml
<syncfusion:LineSeries ItemsSource="{Binding Data}" 
                       ResistancePath="Resistance" 
                       ReactancePath="Reactance">
    <syncfusion:LineSeries.DataLabel>
        <syncfusion:DataLabel ShowLabel="True"/>
    </syncfusion:LineSeries.DataLabel>
</syncfusion:LineSeries>
```

```csharp
series.DataLabel.ShowLabel = true;
```

### Data Label Behavior

**Automatic Positioning:**
- Labels positioned above data points by default
- Automatically repositions to avoid collisions
- Connects repositioned labels with connector lines
- Hides labels when no space available

**Label Content:**
- By default shows resistance and reactance values
- Can be customized with LabelTemplate
- Formatted as text

### DataLabel Properties

| Property | Type | Description |
|----------|------|-------------|
| `ShowLabel` | bool | Enable/disable labels |
| `LabelStyle` | Style | Text styling (font, color, size) |
| `LabelTemplate` | DataTemplate | Custom label layout |

## Label Styling

### Basic Label Styling

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <Style TargetType="TextBlock" x:Key="labelStyle">
            <Setter Property="Foreground" Value="Yellow"/>
            <Setter Property="FontSize" Value="12"/>
            <Setter Property="FontFamily" Value="Calibri"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:LineSeries>
        <syncfusion:LineSeries.DataLabel>
            <syncfusion:DataLabel ShowLabel="True" 
                                  LabelStyle="{StaticResource labelStyle}"/>
        </syncfusion:LineSeries.DataLabel>
    </syncfusion:LineSeries>
</syncfusion:SfSmithChart>
```

### Label Style Properties

Common TextBlock properties for styling:
- **Foreground**: Text color
- **FontSize**: Text size
- **FontFamily**: Font typeface
- **FontWeight**: Bold, Normal, Light, etc.
- **FontStyle**: Normal, Italic
- **Background**: Label background color
- **Padding**: Space around text

## Custom Label Templates

Create sophisticated labels with custom layouts.

### Basic Custom Template

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <DataTemplate x:Key="labelTemplate">
            <Border CornerRadius="4" 
                    Background="{Binding Background}" 
                    BorderThickness="1" 
                    Padding="8,4,8,4" 
                    BorderBrush="{Binding BorderBrush}">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    
                    <TextBlock Grid.Row="0" 
                               Text="{Binding Resistance}" 
                               Style="{Binding LabelStyle}"/>
                    <TextBlock Grid.Row="1" 
                               Text="{Binding Reactance}"  
                               Style="{Binding LabelStyle}"/>
                </Grid>
            </Border>
        </DataTemplate>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:LineSeries ShowMarker="True" 
                           ItemsSource="{Binding Data}" 
                           ResistancePath="Resistance" 
                           ReactancePath="Reactance">
        <syncfusion:LineSeries.DataLabel>
            <syncfusion:DataLabel ShowLabel="True" 
                                  LabelTemplate="{StaticResource labelTemplate}"/>
        </syncfusion:LineSeries.DataLabel>
    </syncfusion:LineSeries>
</syncfusion:SfSmithChart>
```

### Template Binding Context

The LabelTemplate binding context provides:
- **Resistance**: Resistance value
- **Reactance**: Reactance value
- **Background**: Default label background
- **BorderBrush**: Default label border
- **LabelStyle**: Applied label style
- **Other data point properties**

### Advanced Label Template Examples

**Formatted Label with Units:**
```xml
<DataTemplate x:Key="formattedLabel">
    <Border Background="White" 
            BorderBrush="Gray" 
            BorderThickness="1" 
            CornerRadius="3" 
            Padding="6,3">
        <StackPanel>
            <TextBlock FontWeight="Bold" FontSize="10">
                <Run Text="R: "/>
                <Run Text="{Binding Resistance, StringFormat='{}{0:F2}'}"/>
                <Run Text=" Ω"/>
            </TextBlock>
            <TextBlock FontWeight="Bold" FontSize="10">
                <Run Text="X: "/>
                <Run Text="{Binding Reactance, StringFormat='{}{0:F2}'}"/>
                <Run Text=" Ω"/>
            </TextBlock>
        </StackPanel>
    </Border>
</DataTemplate>
```

**Colored Background Label:**
```xml
<DataTemplate x:Key="coloredLabel">
    <Border Background="DarkBlue" 
            BorderBrush="Yellow" 
            BorderThickness="2" 
            CornerRadius="8" 
            Padding="8,4">
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="{Binding Resistance}" 
                       Foreground="White" 
                       FontWeight="Bold" 
                       Margin="0,0,5,0"/>
            <TextBlock Text="/" 
                       Foreground="Yellow" 
                       Margin="0,0,5,0"/>
            <TextBlock Text="{Binding Reactance}" 
                       Foreground="White" 
                       FontWeight="Bold"/>
        </StackPanel>
    </Border>
</DataTemplate>
```

**Icon with Label:**
```xml
<DataTemplate x:Key="iconLabel">
    <StackPanel Orientation="Horizontal">
        <Ellipse Fill="Orange" Width="8" Height="8" Margin="0,0,4,0"/>
        <TextBlock Text="{Binding Resistance}" 
                   FontSize="10" 
                   Foreground="Black"/>
    </StackPanel>
</DataTemplate>
```

## Common Patterns

### Pattern 1: Series with Markers Only

```csharp
LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    ShowMarker = true,
    MarkerType = MarkerType.Circle,
    MarkerHeight = 8,
    MarkerWidth = 8,
    MarkerInterior = new SolidColorBrush(Colors.Blue)
};
chart.Series.Add(series);
```

### Pattern 2: Series with Markers and Labels

```csharp
LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    ShowMarker = true,
    MarkerType = MarkerType.Diamond,
    MarkerHeight = 10,
    MarkerWidth = 10
};

series.DataLabel.ShowLabel = true;

Style labelStyle = new Style(typeof(TextBlock));
labelStyle.Setters.Add(new Setter(TextBlock.FontSizeProperty, 10.0));
labelStyle.Setters.Add(new Setter(TextBlock.ForegroundProperty, new SolidColorBrush(Colors.DarkBlue)));
series.DataLabel.LabelStyle = labelStyle;

chart.Series.Add(series);
```

### Pattern 3: Custom Markers with Styled Labels

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <!-- Custom Star Marker -->
        <DataTemplate x:Key="starMarker">
            <Path Fill="{Binding Interior}" 
                  Stroke="{Binding Stroke}"
                  StrokeThickness="1"
                  Data="M 5,0 L 6,4 L 10,4 L 7,6 L 8,10 L 5,8 L 2,10 L 3,6 L 0,4 L 4,4 Z"/>
        </DataTemplate>
        
        <!-- Label Style -->
        <Style TargetType="TextBlock" x:Key="labelStyle">
            <Setter Property="Foreground" Value="DarkRed"/>
            <Setter Property="FontSize" Value="11"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:LineSeries ShowMarker="True" 
                           MarkerType="Custom"
                           MarkerTemplate="{StaticResource starMarker}"
                           ItemsSource="{Binding Data}"
                           ResistancePath="Resistance"
                           ReactancePath="Reactance">
        <syncfusion:LineSeries.DataLabel>
            <syncfusion:DataLabel ShowLabel="True" 
                                  LabelStyle="{StaticResource labelStyle}"/>
        </syncfusion:LineSeries.DataLabel>
    </syncfusion:LineSeries>
</syncfusion:SfSmithChart>
```

### Pattern 4: Multiple Series with Distinct Markers

```csharp
// Series 1: Blue circles
LineSeries series1 = new LineSeries
{
    ItemsSource = Data1,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Series 1",
    ShowMarker = true,
    MarkerType = MarkerType.Circle,
    MarkerInterior = new SolidColorBrush(Colors.Blue)
};

// Series 2: Orange diamonds
LineSeries series2 = new LineSeries
{
    ItemsSource = Data2,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Series 2",
    ShowMarker = true,
    MarkerType = MarkerType.Diamond,
    MarkerInterior = new SolidColorBrush(Colors.Orange)
};

// Series 3: Green rectangles
LineSeries series3 = new LineSeries
{
    ItemsSource = Data3,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Series 3",
    ShowMarker = true,
    MarkerType = MarkerType.Rectangle,
    MarkerInterior = new SolidColorBrush(Colors.Green)
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
```

## Troubleshooting

### Markers Not Showing

**Issue**: ShowMarker is true but markers not visible  
**Solutions**:
- Verify MarkerHeight and MarkerWidth > 0
- Check if MarkerInterior is transparent
- Ensure data points exist in series
- Try different MarkerType

### Custom Marker Template Not Working

**Issue**: MarkerTemplate defined but not displaying  
**Solutions**:
- Set MarkerType = Custom
- Verify template resource key matches
- Check template binding context
- Ensure template is in correct Resources section

### Labels Overlapping Data

**Issue**: Labels obscure chart data  
**Solutions**:
- Labels automatically reposition to avoid collisions
- Reduce label font size
- Use custom template with smaller layout
- Consider showing labels only on hover (use tooltips instead)

### Labels Not Showing

**Issue**: ShowLabel is true but labels not visible  
**Solutions**:
- Ensure data points have valid values
- Check if label color matches background
- Verify LabelStyle is correctly applied
- Check for sufficient space (labels hide if no room)

### Template Bindings Not Working

**Issue**: Custom template shows but bindings fail  
**Solutions**:
- Use {Binding PropertyName} syntax correctly
- Verify property names (Resistance, Reactance, Interior, Stroke)
- Check binding context in template
- Use Output window to check binding errors

## Summary

Data markers and labels enhance SmithChart visualization by highlighting data points and displaying values. Key points:

- Enable markers with ShowMarker property
- Choose from multiple built-in marker types (Circle, Diamond, Rectangle, etc.)
- Customize marker size, color, and stroke
- Create custom markers using MarkerTemplate
- Enable data labels with DataLabel.ShowLabel
- Style labels with LabelStyle for consistent appearance
- Create complex label layouts with LabelTemplate
- Automatic label positioning prevents collisions
- Use markers and labels together for maximum clarity
- Different marker types help distinguish multiple series