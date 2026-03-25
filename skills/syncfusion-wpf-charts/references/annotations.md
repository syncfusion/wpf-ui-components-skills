# Annotations

Add custom text, shapes, and images to highlight specific areas or values in your chart.

## Text Annotation

Add text labels at specific coordinates.

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Annotations>
        <syncfusion:TextAnnotation Text="Peak Sales" 
                                   X1="3" 
                                   Y1="75000"
                                   FontSize="14"
                                   FontWeight="Bold"
                                   Foreground="Red"/>
    </syncfusion:SfChart.Annotations>
</syncfusion:SfChart>
```

## Line Annotation

Draw lines to mark thresholds or trends.

### Horizontal Line

```xml
<syncfusion:HorizontalLineAnnotation Y1="60000"
                                     Stroke="Red"
                                     StrokeThickness="2"
                                     StrokeDashArray="5,3"
                                     Text="Target"
                                     ShowAxisLabel="True"/>
```

### Vertical Line

```xml
<syncfusion:VerticalLineAnnotation X1="4"
                                   Stroke="Blue"
                                   StrokeThickness="2"
                                   Text="Q2 Start"/>
```

### Straight Line

Draw a line between two points:

```xml
<syncfusion:LineAnnotation X1="0" Y1="20000"
                           X2="6" Y2="80000"
                           Stroke="Green"
                           StrokeThickness="2"/>
```

## Rectangle Annotation

Highlight rectangular regions.

```xml
<syncfusion:RectangleAnnotation X1="2" Y1="30000"
                                X2="4" Y2="70000"
                                Fill="LightGreen"
                                Opacity="0.3"
                                Stroke="Green"
                                StrokeThickness="1"
                                Text="High Performance Period"/>
```

## Ellipse Annotation

Draw circles or ellipses.

```xml
<syncfusion:EllipseAnnotation X1="3" Y1="65000"
                              X2="4" Y2="75000"
                              Fill="Yellow"
                              Opacity="0.4"
                              Stroke="Orange"
                              StrokeThickness="2"/>
```

## Image Annotation

Add images to the chart.

```xml
<syncfusion:ImageAnnotation X1="5" Y1="80000"
                            ImageSource="Images\Graduate.png"
                            Angle="0"/>
```

## Positioning Annotations

### Coordinate Mode

Position relative to axis values (default):

```xml
<syncfusion:TextAnnotation X1="3" Y1="50000"
                           CoordinateUnit="Axis"
                           Text="Point A"/>
```

### Pixel Mode

Position relative to pixels:

```xml
<syncfusion:TextAnnotation X1="100" Y1="150"
                           CoordinateUnit="Pixel"
                           Text="Fixed Position"/>
```

## Annotation Alignment

Control how annotations are aligned:

```xml
<syncfusion:TextAnnotation Text="Center Aligned"
                           X1="3" Y1="50000"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"/>
```

## Annotation with Axis

Associate annotation with specific axis:

```xml
<syncfusion:HorizontalLineAnnotation Y1="50000"
                                     YAxisName="SecondaryAxis"
                                     Stroke="Red"
                                     StrokeThickness="2"/>
```

## Draggable Annotations

Allow users to drag annotations:

```xml
<syncfusion:TextAnnotation Text="Draggable"
                           X1="2" Y1="40000"
                           CanDrag="True"
                           DraggingMode="All"/>
```

**DraggingMode options:**
- `Horizontal` - Horizontal only
- `Vertical` - Vertical only
- `All` - Both directions

## Annotation Events

Handle annotation interactions:

```xml
<syncfusion:TextAnnotation Text="Click Me"
                           X1="3" Y1="50000"
                           MouseDown="Annotation_MouseDown"/>
```

```csharp
private void Annotation_MouseDown(object sender, MouseButtonEventHandler e)
{
    var annotation = sender as TextAnnotation;
    // Handle click
}
```

## Multiple Annotations Example

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Annotations>
        <!-- Target line -->
        <syncfusion:HorizontalLineAnnotation Y1="60000"
                                             Stroke="Red"
                                             StrokeThickness="2"
                                             StrokeDashArray="5,3"
                                             Text="Target: $60K"/>
        
        <!-- Peak marker -->
        <syncfusion:TextAnnotation Text="Peak" 
                                   X1="5" 
                                   Y1="75000"
                                   FontWeight="Bold"
                                   Foreground="Green"/>
        
        <!-- Highlight region -->
        <syncfusion:RectangleAnnotation X1="3" Y1="40000"
                                        X2="5" Y2="70000"
                                        Fill="LightBlue"
                                        Opacity="0.2"/>
    </syncfusion:SfChart.Annotations>
</syncfusion:SfChart>
```

## Common Annotation Patterns

### Pattern 1: Threshold Line

```xml
<syncfusion:HorizontalLineAnnotation Y1="50000"
                                     Stroke="OrangeRed"
                                     StrokeThickness="2"
                                     StrokeDashArray="4,2"
                                     Text="Threshold"
                                     ShowAxisLabel="True"
                                     LabelFontWeight="Bold"/>
```

### Pattern 2: Highlighted Region

```xml
<syncfusion:RectangleAnnotation X1="2" Y1="0"
                                X2="4" Y2="100000"
                                Fill="Yellow"
                                Opacity="0.15"
                                Text="Promotion Period"
                                VerticalTextAlignment="Top"/>
```

### Pattern 3: Data Point Marker

```xml
<syncfusion:EllipseAnnotation X1="3.8" Y1="72000"
                              X2="4.2" Y2="78000"
                              Fill="Transparent"
                              Stroke="Red"
                              StrokeThickness="3"/>

<syncfusion:TextAnnotation Text="Record Sales"
                           X1="4" Y1="80000"
                           Foreground="Red"
                           FontWeight="Bold"/>
```

## Key Properties

### Common Properties
- `X1`, `Y1` - Starting position
- `X2`, `Y2` - Ending position (for shapes/lines)
- `CoordinateUnit` - Axis or Pixel positioning
- `CanDrag` - Enable dragging
- `HorizontalAlignment` / `VerticalAlignment` - Alignment

### Text Annotation
- `Text` - Text content
- `FontSize`, `FontWeight`, `Foreground` - Text styling

### Line Annotations
- `Stroke` - Line color
- `StrokeThickness` - Line width
- `StrokeDashArray` - Dash pattern
- `ShowAxisLabel` - Show value on axis

### Shape Annotations
- `Fill` - Fill color
- `Stroke` - Border color
- `Opacity` - Transparency
- `Text` - Label text
