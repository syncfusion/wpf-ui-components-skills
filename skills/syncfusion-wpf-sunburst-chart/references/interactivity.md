# Interactivity Features

## Table of Contents
- [Overview](#overview)
- [Tooltip Configuration](#tooltip-configuration)
- [Selection Behavior](#selection-behavior)
- [Zooming Behavior](#zooming-behavior)
- [Combined Interactivity](#combined-interactivity)

## Overview

Sunburst Chart supports three primary interactive behaviors to enhance user experience:

1. **Tooltips** – Display information on mouse hover
2. **Selection** – Highlight segments on click or hover
3. **Zooming** – Drill-down into hierarchies for detailed exploration

All behaviors are added to the `Behaviors` collection and can be combined for rich interactivity.

## Tooltip Configuration

Tooltips display segment information when users hover over or touch segments.

### Basic Tooltip Setup

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Behaviors>
        <sunburst:SunburstToolTipBehavior ShowToolTip="True"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstToolTipBehavior tooltip = new SunburstToolTipBehavior();
tooltip.ShowToolTip = true;
chart.Behaviors.Add(tooltip);
```

**Default display:** Category name and value

### Tooltip Alignment

Control tooltip position relative to the cursor.

#### Horizontal Alignment

**XAML:**
```xml
<sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                 HorizontalAlignment="Right"/>
```

**C#:**
```csharp
tooltip.HorizontalAlignment = HorizontalAlignment.Right;
```

**Options:** Left, Center, Right

#### Vertical Alignment

**XAML:**
```xml
<sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                 VerticalAlignment="Bottom"/>
```

**C#:**
```csharp
tooltip.VerticalAlignment = VerticalAlignment.Bottom;
```

**Options:** Top, Center, Bottom

### Tooltip Offset

Fine-tune position with pixel offsets.

**XAML:**
```xml
<sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                 HorizontalOffset="50"
                                 VerticalOffset="50"/>
```

**C#:**
```csharp
tooltip.HorizontalOffset = 50;
tooltip.VerticalOffset = 50;
```

**Values:** Positive integers (pixels from cursor)

### Tooltip Duration

Control how long tooltips remain visible.

**XAML:**
```xml
<sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                 ShowDuration="6000"/>
```

**C#:**
```csharp
tooltip.ShowDuration = 6000; // 6 seconds
```

**Default:** 3000ms (3 seconds)

### Initial Show Delay

Set delay before tooltip appears.

**XAML:**
```xml
<sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                 InitialShowDelay="500"/>
```

**C#:**
```csharp
tooltip.InitialShowDelay = 500; // 0.5 second delay
```

**Default:** 0ms (immediate)  
**Recommended:** 300-500ms to prevent accidental displays

### Tooltip Animation

Enable smooth tooltip transitions.

**XAML:**
```xml
<sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                 EnableAnimation="True"
                                 AnimationDuration="300"/>
```

**C#:**
```csharp
tooltip.EnableAnimation = true;
tooltip.AnimationDuration = 300; // milliseconds
```

### Custom Tooltip Template

Create custom tooltip designs using `ToolTipTemplate`.

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Behaviors>
        <sunburst:SunburstToolTipBehavior ShowToolTip="True">
            
            <sunburst:SunburstToolTipBehavior.ToolTipTemplate>
                <DataTemplate>
                    <Border Background="{Binding Interior}"
                            BorderBrush="{Binding Interior}"
                            BorderThickness="4"
                            CornerRadius="5"
                            Padding="8">
                        <StackPanel>
                            <StackPanel Orientation="Horizontal">
                                <TextBlock Text="Category: " 
                                          Foreground="White"
                                          FontWeight="Bold"/>
                                <TextBlock Text="{Binding Category}" 
                                          Foreground="White"/>
                            </StackPanel>
                            <StackPanel Orientation="Horizontal">
                                <TextBlock Text="Value: " 
                                          Foreground="White"
                                          FontWeight="Bold"/>
                                <TextBlock Text="{Binding Value}" 
                                          Foreground="White"/>
                            </StackPanel>
                        </StackPanel>
                    </Border>
                </DataTemplate>
            </sunburst:SunburstToolTipBehavior.ToolTipTemplate>
            
        </sunburst:SunburstToolTipBehavior>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**Available bindings:**
- **Category** – Segment category name
- **Value** – Segment numeric value
- **Interior** – Segment fill color

## Selection Behavior

Enable users to select segments by clicking or hovering.

### Basic Selection Setup

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Behaviors>
        <sunburst:SunburstSelectionBehavior EnableSelection="True"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstSelectionBehavior selection = new SunburstSelectionBehavior();
selection.EnableSelection = true;
chart.Behaviors.Add(selection);
```

### Selection Display Mode

Control how selection is visually indicated.

#### HighlightByColor

Use a custom brush to highlight selected segments.

**XAML:**
```xml
<sunburst:SunburstSelectionBehavior EnableSelection="True"
                                   SelectionBrush="Black"
                                   SelectionDisplayMode="HighlightByColor"/>
```

**C#:**
```csharp
selection.EnableSelection = true;
selection.SelectionBrush = new SolidColorBrush(Colors.Black);
selection.SelectionDisplayMode = SelectionDisplayMode.HighlightByColor;
```

#### HighlightByOpacity (Default)

Reduce opacity of unselected segments.

**XAML:**
```xml
<sunburst:SunburstSelectionBehavior EnableSelection="True"
                                   SelectionDisplayMode="HighlightByOpacity"/>
```

**C#:**
```csharp
selection.SelectionDisplayMode = SelectionDisplayMode.HighlightByOpacity;
```

### Selection Mode

Determine how users trigger selection.

**Options:**
- **MouseClick** – Click to select (default)
- **MouseMove** – Hover to select
- **Both** – Click or hover to select

**XAML:**
```xml
<sunburst:SunburstSelectionBehavior EnableSelection="True"
                                   SelectionMode="MouseClick"/>
```

**C#:**
```csharp
selection.SelectionMode = Syncfusion.UI.Xaml.SunburstChart.SelectionMode.MouseClick;
```

**Recommendations:**
- **MouseClick:** Standard, prevents accidental selections
- **MouseMove:** Quick exploration, may feel too sensitive
- **Both:** Maximum flexibility, may confuse users

### Selection Type

Control which related segments get selected.

#### Single

Select only the clicked segment.

**XAML:**
```xml
<sunburst:SunburstSelectionBehavior EnableSelection="True"
                                   SelectionType="Single"/>
```

**C#:**
```csharp
selection.SelectionType = SelectionType.Single;
```

**Use when:** Users need to focus on one specific segment.

#### Child

Select the clicked segment and all its children.

**XAML:**
```xml
<sunburst:SunburstSelectionBehavior EnableSelection="True"
                                   SelectionType="Child"/>
```

**C#:**
```csharp
selection.SelectionType = SelectionType.Child;
```

**Use when:** Users want to see all subcategories of a parent.

#### Parent

Select the clicked segment and all its parents (up the hierarchy).

**XAML:**
```xml
<sunburst:SunburstSelectionBehavior EnableSelection="True"
                                   SelectionType="Parent"/>
```

**C#:**
```csharp
selection.SelectionType = SelectionType.Parent;
```

**Use when:** Users want to see the path from root to current segment.

#### Group

Select all segments in the same group/level.

**XAML:**
```xml
<sunburst:SunburstSelectionBehavior EnableSelection="True"
                                   SelectionType="Group"/>
```

**C#:**
```csharp
selection.SelectionType = SelectionType.Group;
```

**Use when:** Users want to highlight entire categories at once.

### Selection Cursor

Customize the mouse cursor on hover.

**XAML:**
```xml
<sunburst:SunburstSelectionBehavior EnableSelection="True"
                                   SelectionCursor="Hand"/>
```

**C#:**
```csharp
selection.SelectionCursor = Cursors.Hand;
```

**Options:** Hand, Arrow, etc.

## Zooming Behavior

Enable drill-down functionality to explore deep hierarchies.

### Basic Zooming Setup

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Behaviors>
        <sunburst:SunburstZoomingBehavior EnableZooming="True"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstZoomingBehavior zoom = new SunburstZoomingBehavior();
zoom.EnableZooming = true;
chart.Behaviors.Add(zoom);
```

**Note:** EnableZooming defaults to True when behavior is added.

**Behavior:** Click a segment to zoom into it, making it the new center. A toolbar appears with back and reset buttons.

### Zooming Toolbar Configuration

The toolbar provides navigation controls during zooming.

#### Toolbar Position

**XAML:**
```xml
<sunburst:SunburstZoomingBehavior EnableZooming="True"
                                 ToolBarHorizontalAlignment="Center"
                                 ToolBarVerticalAlignment="Top"/>
```

**C#:**
```csharp
zoom.ToolBarHorizontalAlignment = HorizontalAlignment.Center;
zoom.ToolBarVerticalAlignment = VerticalAlignment.Top;
```

**Horizontal options:** Left, Center, Right  
**Vertical options:** Top, Center, Bottom

#### Toolbar Item Sizing

**XAML:**
```xml
<sunburst:SunburstZoomingBehavior EnableZooming="True"
                                 ToolBarItemHeight="30"
                                 ToolBarItemWidth="50"
                                 ToolBarItemMargin="5"/>
```

**C#:**
```csharp
zoom.ToolBarItemHeight = 30;
zoom.ToolBarItemWidth = 50;
zoom.ToolBarItemMargin = new Thickness(5);
```

#### Toolbar Offset

Fine-tune toolbar position with offset values (0.0 to 1.0).

**XAML:**
```xml
<sunburst:SunburstZoomingBehavior EnableZooming="True"
                                 ToolbarOffsetX="0.1" 
                                 ToolbarOffsetY="0.5"/>
```

**C#:**
```csharp
zoom.ToolbarOffsetX = 0.1; // 10% from left
zoom.ToolbarOffsetY = 0.5; // 50% from top (center)
```

**Values:**
- 0.0 = Left/Top edge
- 0.5 = Center
- 1.0 = Right/Bottom edge

## Combined Interactivity

Combine multiple behaviors for rich user experience.

### Example 1: Tooltip + Selection

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Behaviors>
        
        <!-- Tooltip on hover -->
        <sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                         InitialShowDelay="300"/>
        
        <!-- Selection on click -->
        <sunburst:SunburstSelectionBehavior EnableSelection="True"
                                           SelectionMode="MouseClick"
                                           SelectionType="Single"
                                           SelectionCursor="Hand"/>
        
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**Behavior:** Hover shows tooltip, click selects segment.

### Example 2: All Features Combined

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding EmployeeData}" 
                          ValueMemberPath="Count"
                          Palette="Metro">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Role"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.Behaviors>
        
        <!-- Custom tooltip -->
        <sunburst:SunburstToolTipBehavior ShowToolTip="True"
                                         HorizontalAlignment="Right"
                                         VerticalAlignment="Bottom"
                                         InitialShowDelay="400"
                                         ShowDuration="5000"
                                         EnableAnimation="True"/>
        
        <!-- Selection with color highlight -->
        <sunburst:SunburstSelectionBehavior EnableSelection="True"
                                           SelectionDisplayMode="HighlightByColor"
                                           SelectionBrush="#FF4CAF50"
                                           SelectionMode="MouseClick"
                                           SelectionType="Child"
                                           SelectionCursor="Hand"/>
        
        <!-- Zooming for deep hierarchies -->
        <sunburst:SunburstZoomingBehavior EnableZooming="True"
                                         ToolBarHorizontalAlignment="Right"
                                         ToolBarVerticalAlignment="Top"
                                         ToolBarItemHeight="28"
                                         ToolBarItemWidth="45"/>
        
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**User Experience:**
1. Hover over segment → Tooltip appears (with delay)
2. Click segment → Segment + children highlight in green
3. Click again → Zoom into that segment
4. Use toolbar → Navigate back or reset to root

### Example 3: Minimal Interactive Chart

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding SimpleData}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Category"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Subcategory"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.Behaviors>
        <sunburst:SunburstToolTipBehavior ShowToolTip="True"/>
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**Use when:** Simple data exploration with just tooltip information.

## Best Practices

### Tooltip Recommendations

- Use InitialShowDelay (300-500ms) to prevent accidental displays
- Keep tooltips concise; show key information only
- Use custom templates for formatted data (currency, percentages)
- Set reasonable ShowDuration (3000-6000ms)
- Enable animation for smooth experience

### Selection Recommendations

- **MouseClick mode** for precise control
- **Single SelectionType** for detailed focus
- **Child SelectionType** for exploring subcategories
- **HighlightByColor** with contrasting brush for clarity
- Use Hand cursor to indicate clickable segments

### Zooming Recommendations

- Enable for hierarchies with 4+ levels
- Position toolbar where it doesn't obscure important segments
- Use appropriate toolbar sizing (25-35px height/width)
- Combine with tooltip for better navigation context

### Combined Features

- Tooltip + Selection: Standard interactive pattern
- Tooltip + Zooming: Deep hierarchy exploration
- All three: Maximum interactivity for complex data
- Test on touch devices if supporting tablets

## Troubleshooting

**Tooltip not appearing:**
- Verify ShowToolTip="True"
- Check that behavior is added to Behaviors collection
- Ensure segments are large enough to hover over
- Confirm InitialShowDelay isn't too long

**Selection not working:**
- Verify EnableSelection="True"
- Check SelectionMode is set correctly
- Ensure mouse events aren't blocked
- Confirm SelectionBrush is visible (contrasting color)

**Zooming not triggering:**
- Verify EnableZooming="True" (default when behavior added)
- Check that segments are clickable
- Ensure multiple levels exist for drill-down
- Confirm toolbar isn't hidden outside viewport

**Behaviors conflicting:**
- Tooltip and Selection work well together
- Zooming may interfere with Selection (by design)
- Test interactions thoroughly when combining all three
- Consider disabling Selection when Zooming is active

**Performance issues:**
- Simplify custom tooltip templates
- Reduce animation duration
- Disable animations on lower-end devices
- Limit number of simultaneous behaviors
