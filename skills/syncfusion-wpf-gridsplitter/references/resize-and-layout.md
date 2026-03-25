# Resize and Layout Operations

## Table of Contents
- [Resizing Grid Rows](#resizing-grid-rows)
- [Resizing Grid Columns](#resizing-grid-columns)
- [ResizeBehavior Property](#resizebehavior-property)
- [Resize Increments](#resize-increments)
- [Programmatic Resizing](#programmatic-resizing)
- [Working with Merged Cells](#working-with-merged-cells)
- [Orientation Handling](#orientation-handling)

This guide covers all aspects of resizing grid rows and columns using SfGridSplitter, from basic resize operations to advanced layout configurations.

## Resizing Grid Rows

Horizontal splitters resize grid rows by redistributing vertical space.

### Basic Row Resizing

To resize grid rows, place the SfGridSplitter in its own row with proper configuration:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <TextBlock Grid.Row="0" 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   Text="Panel 1"/>
        
        <TextBlock Grid.Row="2"
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 2"/>

        <!-- Grid Splitter for rows -->
        <syncfusion:SfGridSplitter Name="gridSplitter"
                                   Grid.Row="1"
                                   HorizontalAlignment="Stretch" 
                                   ResizeBehavior="PreviousAndNext"
                                   Height="5"/>
    </Grid>
</Border>
```

**Requirements for Row Resizing:**
- Place splitter in a row with `Height="Auto"`
- Set `HorizontalAlignment="Stretch"`
- Specify explicit `Height` (e.g., 5 pixels)
- Set `ResizeBehavior` to control which rows resize

### Multi-Row Resizing

Resize multiple pairs of rows with multiple splitters:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <Border Grid.Row="0" Background="LightBlue"/>
    <Border Grid.Row="2" Background="LightGreen"/>
    <Border Grid.Row="4" Background="LightYellow"/>
    
    <!-- First splitter -->
    <syncfusion:SfGridSplitter Grid.Row="1"
                               HorizontalAlignment="Stretch"
                               Height="5"
                               ResizeBehavior="PreviousAndNext"/>
    
    <!-- Second splitter -->
    <syncfusion:SfGridSplitter Grid.Row="3"
                               HorizontalAlignment="Stretch"
                               Height="5"
                               ResizeBehavior="PreviousAndNext"/>
</Grid>
```

## Resizing Grid Columns

Vertical splitters resize grid columns by redistributing horizontal space.

### Basic Column Resizing

To resize grid columns, place the SfGridSplitter in its own column:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <TextBlock Grid.Column="0" 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   Text="Panel 1"/>
        
        <TextBlock Grid.Column="2"
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 2"/>

        <!-- Grid Splitter for columns -->
        <syncfusion:SfGridSplitter Name="gridSplitter"
                                   Grid.Column="1"
                                   VerticalAlignment="Stretch" 
                                   ResizeBehavior="PreviousAndNext"
                                   Width="5"/>
    </Grid>
</Border>
```

**Requirements for Column Resizing:**
- Place splitter in a column with `Width="Auto"`
- Set `VerticalAlignment="Stretch"`
- Specify explicit `Width` (e.g., 5 pixels)
- Set `ResizeBehavior` to control which columns resize

### Multi-Column Resizing

Create multiple adjustable columns:

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="2*" />
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="*" />
    </Grid.ColumnDefinitions>
    
    <Border Grid.Column="0" Background="LightCoral"/>
    <Border Grid.Column="2" Background="LightBlue"/>
    <Border Grid.Column="4" Background="LightGreen"/>
    
    <!-- First splitter -->
    <syncfusion:SfGridSplitter Grid.Column="1"
                               VerticalAlignment="Stretch"
                               Width="5"
                               ResizeBehavior="PreviousAndNext"/>
    
    <!-- Second splitter -->
    <syncfusion:SfGridSplitter Grid.Column="3"
                               VerticalAlignment="Stretch"
                               Width="5"
                               ResizeBehavior="PreviousAndNext"/>
</Grid>
```

## ResizeBehavior Property

The ResizeBehavior property controls which grid rows or columns are affected when the splitter is moved.

### ResizeBehavior Options

**1. PreviousAndNext (Most Common)**

Resizes the element before and after the splitter:

```xml
<syncfusion:SfGridSplitter ResizeBehavior="PreviousAndNext"/>
```

**Behavior:**
- Moving splitter up/left: Previous element shrinks, next element grows
- Moving splitter down/right: Previous element grows, next element shrinks

**When to use:** Standard split layouts where both sides should adjust proportionally.

**2. PreviousAndCurrent**

Resizes the previous element and the splitter's own row/column:

```xml
<syncfusion:SfGridSplitter ResizeBehavior="PreviousAndCurrent"/>
```

**Behavior:**
- Only affects the previous element and splitter's container
- Next element remains fixed

**When to use:** When the next element should maintain fixed size.

**3. CurrentAndNext**

Resizes the splitter's own row/column and the next element:

```xml
<syncfusion:SfGridSplitter ResizeBehavior="CurrentAndNext"/>
```

**Behavior:**
- Previous element remains fixed
- Only affects splitter's container and next element

**When to use:** When the previous element should maintain fixed size.

### ResizeBehavior Selection Guide

```xml
<!-- Example comparing behaviors -->
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100"/>  <!-- Fixed -->
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>    <!-- Flexible -->
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="200"/>  <!-- Fixed -->
    </Grid.RowDefinitions>
    
    <Border Grid.Row="0" Background="Gray"/>
    <Border Grid.Row="2" Background="LightBlue"/>
    <Border Grid.Row="4" Background="Gray"/>
    
    <!-- This splitter affects row 0 and row 2 -->
    <syncfusion:SfGridSplitter Grid.Row="1"
                               HorizontalAlignment="Stretch"
                               ResizeBehavior="PreviousAndNext"/>
    
    <!-- This splitter affects row 2 and row 4 -->
    <syncfusion:SfGridSplitter Grid.Row="3"
                               HorizontalAlignment="Stretch"
                               ResizeBehavior="PreviousAndNext"/>
</Grid>
```

**Important:** The ResizeBehavior must align with your row/column placement. Incorrect settings may prevent resizing.

## Resize Increments

Control the granularity of resizing operations with increment properties.

### DragIncrement Property

Sets the pixel interval for mouse-based resizing:

```xml
<syncfusion:SfGridSplitter DragIncrement="10"
                           HorizontalAlignment="Stretch"
                           ResizeBehavior="PreviousAndNext"/>
```

**Behavior:**
- Default value: `1` (smooth, pixel-by-pixel resizing)
- Higher values: Splitter "snaps" to increments
- Example with `DragIncrement="10"`: Splitter moves in 10-pixel jumps

**When to use:**
- Align panels to grid or content sizes
- Reduce jitter in heavy layouts
- Snap to specific size intervals

### KeyboardIncrement Property

Sets the pixel interval for keyboard-based resizing:

```xml
<syncfusion:SfGridSplitter KeyboardIncrement="20"
                           HorizontalAlignment="Stretch"
                           ResizeBehavior="PreviousAndNext"/>
```

**Behavior:**
- Default value: `20` pixels
- Used when moving splitter with arrow keys
- Provides keyboard accessibility

**When to use:**
- Provide precise keyboard control
- Accessibility requirements
- Larger increments for faster adjustment

### Combined Increment Configuration

Use different increments for mouse and keyboard:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <TextBlock Grid.Row="0" 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   Text="Panel 1"/>
        
        <TextBlock Grid.Row="2"
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 2"/>

        <!-- Grid Splitter with increments -->
        <syncfusion:SfGridSplitter DragIncrement="5"
                                   KeyboardIncrement="25"
                                   HorizontalAlignment="Stretch"
                                   Height="5"
                                   Grid.Row="1"
                                   ResizeBehavior="PreviousAndNext"/>
    </Grid>
</Border>
```

**Result:**
- Mouse dragging: 5-pixel increments (smoother)
- Keyboard arrows: 25-pixel increments (faster)

## Programmatic Resizing

Move the splitter and resize panels programmatically using the MoveSplitter method.

### MoveSplitter Method

Syntax:
```csharp
public void MoveSplitter(double offset)
```

**Parameters:**
- `offset`: Number of pixels to move the splitter
  - Positive values: Move down (horizontal) or right (vertical)
  - Negative values: Move up (horizontal) or left (vertical)

### Basic Programmatic Move

XAML:
```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <Border Grid.Row="0" Background="LightBlue"/>
    <Border Grid.Row="2" Background="LightGreen"/>
    
    <syncfusion:SfGridSplitter Name="gridSplitter"
                               Grid.Row="1"
                               HorizontalAlignment="Stretch"
                               Height="5"
                               ResizeBehavior="PreviousAndNext"/>
</Grid>
```

C#:
```csharp
// Move splitter down by 50 pixels
gridSplitter.MoveSplitter(50);

// Move splitter up by 30 pixels
gridSplitter.MoveSplitter(-30);

// Reset to center (example: move to specific position)
// Calculate current position first, then move accordingly
```

### Practical Use Cases

**1. Save and Restore Layout Positions**

```csharp
// Save position
private double savedSplitterPosition;

private void SaveLayout()
{
    // Store relative position (you need to calculate this based on panel sizes)
    savedSplitterPosition = GetCurrentSplitterPosition();
}

private void RestoreLayout()
{
    // Restore saved position
    double currentPosition = GetCurrentSplitterPosition();
    double offset = savedSplitterPosition - currentPosition;
    gridSplitter.MoveSplitter(offset);
}
```

**2. Preset Layout Configurations**

```csharp
private void SetCompactView()
{
    // Move splitter to give more space to bottom panel
    gridSplitter.MoveSplitter(-100);
}

private void SetExpandedView()
{
    // Move splitter to give more space to top panel
    gridSplitter.MoveSplitter(100);
}

private void SetBalancedView()
{
    // Reset to center (calculate required offset)
    double currentOffset = CalculateCenterOffset();
    gridSplitter.MoveSplitter(currentOffset);
}
```

**3. Animated Layout Transitions**

```csharp
private async void AnimateSplitterMove(double targetOffset)
{
    const int steps = 20;
    const int delayMs = 10;
    double increment = targetOffset / steps;
    
    for (int i = 0; i < steps; i++)
    {
        gridSplitter.MoveSplitter(increment);
        await Task.Delay(delayMs);
    }
}
```

## Working with Merged Cells

SfGridSplitter supports resizing merged (spanned) grid rows and columns.

### Horizontal Splitter with Row Spans

Resize rows while other content spans multiple rows:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <!-- Left column with horizontal split -->
        <TextBlock Grid.Row="0" Grid.Column="0" 
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center"
                   Text="Panel 1"/>
        
        <TextBlock Grid.Row="2" Grid.Column="0" 
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 2"/>
        
        <!-- Right column spans all rows -->
        <TextBlock Grid.RowSpan="3" Grid.Column="2" 
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 3 (Spans All Rows)"/>

        <!-- Horizontal Splitter (affects left column only) -->
        <syncfusion:SfGridSplitter Grid.Row="1" Grid.Column="0"
                                   HorizontalAlignment="Stretch"
                                   Height="5"
                                   ResizeBehavior="PreviousAndNext"/>

        <!-- Vertical Splitter (spans all rows) -->
        <syncfusion:SfGridSplitter Grid.RowSpan="3" Grid.Column="1"
                                   VerticalAlignment="Stretch"
                                   Width="5"
                                   ResizeBehavior="PreviousAndNext"/>
    </Grid>
</Border>
```

**Key Points:**
- Horizontal splitter in left column only (`Grid.Column="0"`)
- Vertical splitter spans all rows (`Grid.RowSpan="3"`)
- Each splitter operates independently

### Complex Multi-Splitter Layout

Create sophisticated layouts with multiple splitters and merged cells:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="200" />
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="200" />
    </Grid.ColumnDefinitions>
    
    <!-- Left sidebar (spans all rows) -->
    <Border Grid.RowSpan="3" Grid.Column="0" Background="LightGray"/>
    
    <!-- Main content area with horizontal split -->
    <Border Grid.Row="0" Grid.Column="2" Background="LightBlue"/>
    <Border Grid.Row="2" Grid.Column="2" Background="LightGreen"/>
    
    <!-- Right sidebar (spans all rows) -->
    <Border Grid.RowSpan="3" Grid.Column="4" Background="LightGray"/>
    
    <!-- Horizontal splitter in center -->
    <syncfusion:SfGridSplitter Grid.Row="1" Grid.Column="2"
                               HorizontalAlignment="Stretch"
                               Height="5"/>
    
    <!-- Vertical splitters -->
    <syncfusion:SfGridSplitter Grid.RowSpan="3" Grid.Column="1"
                               VerticalAlignment="Stretch"
                               Width="5"/>
    
    <syncfusion:SfGridSplitter Grid.RowSpan="3" Grid.Column="3"
                               VerticalAlignment="Stretch"
                               Width="5"/>
</Grid>
```

## Orientation Handling

### Automatic Orientation Detection

SfGridSplitter automatically determines orientation based on:
1. **Alignment properties:** HorizontalAlignment vs VerticalAlignment
2. **Size properties:** Height vs Width
3. **Grid placement:** Row vs Column position

### Explicit Horizontal Configuration

```xml
<syncfusion:SfGridSplitter Grid.Row="1"
                           HorizontalAlignment="Stretch"
                           Height="5"
                           ResizeBehavior="PreviousAndNext"/>
```

**Indicators of horizontal orientation:**
- `HorizontalAlignment="Stretch"`
- Explicit `Height` property
- Placed in grid row

### Explicit Vertical Configuration

```xml
<syncfusion:SfGridSplitter Grid.Column="1"
                           VerticalAlignment="Stretch"
                           Width="5"
                           ResizeBehavior="PreviousAndNext"/>
```

**Indicators of vertical orientation:**
- `VerticalAlignment="Stretch"`
- Explicit `Width` property
- Placed in grid column

## Edge Cases and Troubleshooting

### Splitter Not Moving

**Symptom:** Splitter visible but cannot be dragged.

**Solutions:**
- Verify alignment property is set to Stretch
- Check ResizeBehavior is configured
- Ensure adjacent rows/columns use star (*) sizing, not fixed

### Panels Collapsing to Zero

**Symptom:** Dragging splitter causes panels to disappear.

**Solution:** Set minimum sizes on row/column definitions:

```xml
<Grid.RowDefinitions>
    <RowDefinition Height="*" MinHeight="50"/>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*" MinHeight="50"/>
</Grid.RowDefinitions>
```

### Jerky Movement

**Symptom:** Splitter moves in large jumps.

**Solution:** Reduce DragIncrement or set to 1:

```xml
<syncfusion:SfGridSplitter DragIncrement="1"/>
```

### Performance Issues During Resize

**Symptom:** Application lags when dragging splitter.

**Solution:** Enable preview mode (covered in customization-styling.md):

```xml
<syncfusion:SfGridSplitter ShowsPreview="True"/>
```

This displays a preview line instead of real-time resizing, improving performance for complex layouts.
