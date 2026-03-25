# Caption Customization

Captions provide descriptive labels for bullet graphs, helping users understand what metric or data the visualization represents.

## Overview

The caption is a content area that displays text or custom content describing the bullet graph. It's essential for identifying what's being measured, especially in multi-graph dashboards where multiple metrics are displayed simultaneously.

### Purpose

Captions help:
- **Identify the metric** being visualized (e.g., "Revenue YTD", "Customer Satisfaction")
- **Provide context** with units or additional information (e.g., "$ in Thousands", "Score out of 100")
- **Support multiple graphs** in dashboard layouts by labeling each visualization
- **Improve accessibility** by providing clear text descriptions

## Caption Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Caption` | object | Content to display as the caption | null |
| `CaptionPosition` | BulletGraphCaptionPosition | Position of caption relative to graph | Near |

### CaptionPosition Values

| Value | Horizontal Orientation | Vertical Orientation |
|-------|----------------------|---------------------|
| `Near` | Left side | Top side |
| `Far` | Right side | Bottom side |

## Adding a Simple Caption

### XAML with Text Caption

```xml
<syncfusion:SfBulletGraph 
    Caption="Revenue YTD"
    CaptionPosition="Far"
    Minimum="0" 
    Maximum="100"
    FeaturedMeasure="65">
</syncfusion:SfBulletGraph>
```

**Note:** For simple text captions, you can set the `Caption` property directly, though it's more common to use a TextBlock for better control over formatting.

## Adding Custom Caption Content

The `Caption` property accepts any WPF content, allowing you to create rich, multi-line captions with custom styling.

### Single-Line TextBlock Caption

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    CaptionPosition="Far"
    Minimum="0" 
    Maximum="100"
    FeaturedMeasure="75">
    
    <syncfusion:SfBulletGraph.Caption>
        <TextBlock 
            Text="Revenue YTD" 
            Foreground="Black"
            FontSize="14" 
            FontWeight="Bold"
            HorizontalAlignment="Center"/>
    </syncfusion:SfBulletGraph.Caption>
</syncfusion:SfBulletGraph>
```

### Multi-Line Caption with StackPanel

The most common pattern for captions uses a StackPanel to stack multiple TextBlocks vertically:

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    CaptionPosition="Far"
    Minimum="0" 
    Maximum="100"
    Interval="20"
    FeaturedMeasure="65"
    ComparativeMeasure="80">
    
    <syncfusion:SfBulletGraph.Caption>
        <StackPanel Margin="0,0,10,0">
            <TextBlock 
                Text="Revenue YTD" 
                Foreground="Black"
                FontSize="13" 
                HorizontalAlignment="Center"/>
            <TextBlock 
                Text="$ in Thousands" 
                Foreground="Gray"
                FontSize="11" 
                HorizontalAlignment="Center"/>
        </StackPanel>
    </syncfusion:SfBulletGraph.Caption>
</syncfusion:SfBulletGraph>
```

This creates a two-line caption with the metric name on top and units below.

## Caption Positioning

### Near Position (Left/Top)

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    CaptionPosition="Near"
    Minimum="0" 
    Maximum="10"
    FeaturedMeasure="6">
    
    <syncfusion:SfBulletGraph.Caption>
        <StackPanel Margin="0,0,10,0">
            <TextBlock Text="Sales Q1" FontSize="13" HorizontalAlignment="Right"/>
            <TextBlock Text="Units Sold" FontSize="11" Foreground="Gray" HorizontalAlignment="Right"/>
        </StackPanel>
    </syncfusion:SfBulletGraph.Caption>
</syncfusion:SfBulletGraph>
```

**Use when:** Caption should appear before the graph (traditional left-to-right reading order)

### Far Position (Right/Bottom)

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    CaptionPosition="Far"
    Minimum="0" 
    Maximum="10"
    FeaturedMeasure="6">
    
    <syncfusion:SfBulletGraph.Caption>
        <StackPanel Margin="10,0,0,0">
            <TextBlock Text="Sales Q1" FontSize="13" HorizontalAlignment="Left"/>
            <TextBlock Text="Units Sold" FontSize="11" Foreground="Gray" HorizontalAlignment="Left"/>
        </StackPanel>
    </syncfusion:SfBulletGraph.Caption>
</syncfusion:SfBulletGraph>
```

**Use when:** Graph should appear first, caption provides context after

## C# Implementation

### Setting Caption in Code-Behind

```csharp
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Syncfusion.UI.Xaml.BulletGraph;

SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.CaptionPosition = BulletGraphCaptionPosition.Far;
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 100;
bulletGraph.FeaturedMeasure = 65;
bulletGraph.ComparativeMeasure = 80;

// Create caption TextBlocks
TextBlock title = new TextBlock() 
{ 
    Text = "Revenue YTD",
    Foreground = new SolidColorBrush(Colors.Black),
    FontSize = 13,
    HorizontalAlignment = HorizontalAlignment.Center
};

TextBlock subtitle = new TextBlock() 
{ 
    Text = "$ in Thousands",
    Foreground = new SolidColorBrush(Colors.Gray),
    FontSize = 11,
    HorizontalAlignment = HorizontalAlignment.Center
};

// Create StackPanel to hold TextBlocks
StackPanel stackPanel = new StackPanel();
stackPanel.Margin = new Thickness(0, 0, 10, 0);
stackPanel.Children.Add(title);
stackPanel.Children.Add(subtitle);

// Set caption
bulletGraph.Caption = stackPanel;

grid.Children.Add(bulletGraph);
```

## Complete Examples

### Revenue Dashboard Card

```xml
<syncfusion:SfBulletGraph 
    Orientation="Horizontal"
    CaptionPosition="Far"
    Minimum="0" 
    Maximum="500" 
    Interval="100"
    FeaturedMeasure="375"
    ComparativeMeasure="400"
    FeaturedMeasureBarStroke="#27AE60"
    QualitativeRangesSize="30">
    
    <syncfusion:SfBulletGraph.Caption>
        <StackPanel Margin="10,0,0,0">
            <TextBlock 
                Text="Revenue YTD" 
                FontSize="14" 
                FontWeight="SemiBold"
                Foreground="#2C3E50"
                HorizontalAlignment="Left"/>
            <TextBlock 
                Text="$ in Thousands" 
                FontSize="11" 
                Foreground="#7F8C8D"
                HorizontalAlignment="Left"/>
            <TextBlock 
                Text="Target: $400K" 
                FontSize="10" 
                Foreground="#95A5A6"
                Margin="0,4,0,0"
                HorizontalAlignment="Left"/>
        </StackPanel>
    </syncfusion:SfBulletGraph.Caption>
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="200" RangeStroke="#FFEBEE"/>
        <syncfusion:QualitativeRange RangeEnd="350" RangeStroke="#FFF9C4"/>
        <syncfusion:QualitativeRange RangeEnd="500" RangeStroke="#E8F5E9"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

### Expense Tracker with Icon

```xml
<syncfusion:SfBulletGraph 
    CaptionPosition="Near"
    Minimum="0" 
    Maximum="10000"
    FeaturedMeasure="7500">
    
    <syncfusion:SfBulletGraph.Caption>
        <StackPanel Orientation="Horizontal" Margin="0,0,15,0">
            <TextBlock 
                Text="💰" 
                FontSize="20" 
                VerticalAlignment="Center"
                Margin="0,0,8,0"/>
            <StackPanel>
                <TextBlock Text="Operating Expenses" FontSize="13" FontWeight="Bold"/>
                <TextBlock Text="Monthly Budget" FontSize="11" Foreground="Gray"/>
            </StackPanel>
        </StackPanel>
    </syncfusion:SfBulletGraph.Caption>
</syncfusion:SfBulletGraph>
```

### Vertical Bullet Graph with Top Caption

```xml
<syncfusion:SfBulletGraph 
    Orientation="Vertical"
    CaptionPosition="Near"
    Minimum="0" 
    Maximum="100"
    FeaturedMeasure="72"
    Height="300">
    
    <syncfusion:SfBulletGraph.Caption>
        <StackPanel Margin="0,0,0,10">
            <TextBlock 
                Text="Customer Satisfaction" 
                FontSize="12" 
                FontWeight="SemiBold"
                HorizontalAlignment="Center"/>
            <TextBlock 
                Text="Score %" 
                FontSize="10" 
                Foreground="Gray"
                HorizontalAlignment="Center"/>
        </StackPanel>
    </syncfusion:SfBulletGraph.Caption>
</syncfusion:SfBulletGraph>
```

### Styled Caption with Background

```xml
<syncfusion:SfBulletGraph 
    CaptionPosition="Far"
    Minimum="0" 
    Maximum="100"
    FeaturedMeasure="85">
    
    <syncfusion:SfBulletGraph.Caption>
        <Border 
            Background="#ECF0F1" 
            Padding="10,8"
            CornerRadius="4"
            Margin="10,0,0,0">
            <StackPanel>
                <TextBlock 
                    Text="Project Completion" 
                    FontSize="13" 
                    FontWeight="Bold"
                    Foreground="#2C3E50"/>
                <TextBlock 
                    Text="85% Complete" 
                    FontSize="11" 
                    Foreground="#27AE60"
                    Margin="0,2,0,0"/>
            </StackPanel>
        </Border>
    </syncfusion:SfBulletGraph.Caption>
</syncfusion:SfBulletGraph>
```

## Caption Styling Patterns

### Minimal Caption

```xml
<syncfusion:SfBulletGraph.Caption>
    <TextBlock 
        Text="Revenue" 
        FontSize="12" 
        Foreground="#333333"/>
</syncfusion:SfBulletGraph.Caption>
```

### Informative Caption

```xml
<syncfusion:SfBulletGraph.Caption>
    <StackPanel>
        <TextBlock Text="Q4 Revenue" FontSize="14" FontWeight="SemiBold"/>
        <TextBlock Text="Millions USD" FontSize="11" Foreground="Gray"/>
        <TextBlock Text="vs. Target $10M" FontSize="10" Foreground="DimGray" Margin="0,2,0,0"/>
    </StackPanel>
</syncfusion:SfBulletGraph.Caption>
```

### Branded Caption

```xml
<syncfusion:SfBulletGraph.Caption>
    <StackPanel>
        <TextBlock 
            Text="MONTHLY SALES" 
            FontSize="11" 
            FontWeight="Bold"
            Foreground="#3498DB"
            LetterSpacing="1"/>
        <TextBlock 
            Text="Performance Indicator" 
            FontSize="9" 
            Foreground="Gray"/>
    </StackPanel>
</syncfusion:SfBulletGraph.Caption>
```

## Best Practices

### Content Guidelines

1. **Keep captions concise:** Use short, descriptive labels (2-4 words for title)
2. **Provide units:** Always indicate measurement units (dollars, percentage, count)
3. **Multi-line structure:** Use title + subtitle pattern for clarity
4. **Consistent formatting:** Apply the same caption style across related bullet graphs

### Positioning Guidelines

1. **Use Far position for dashboards:** Most common in left-to-right reading cultures
2. **Use Near position for reports:** Traditional label-then-data format
3. **Consider overall layout:** Align caption positions consistently across multiple graphs

### Styling Guidelines

1. **Font hierarchy:**
   - Title: 13-14px, SemiBold or Bold
   - Subtitle: 11-12px, Normal weight
   - Additional info: 9-10px, Light color

2. **Color contrast:** Ensure text is readable against the background

3. **Alignment:**
   - Far position: Left-align text
   - Near position: Right-align text
   - Vertical graphs: Center-align text

4. **Spacing:** Add margin between caption and graph (8-12px)

### Accessibility

1. **Use text captions:** Screen readers can access TextBlock content
2. **Avoid icon-only captions:** Always include text labels
3. **Sufficient font size:** Minimum 11px for readability
4. **High contrast:** Dark text on light background (or vice versa)

## Common Use Cases

### Dashboard with Multiple Graphs

```xml
<StackPanel>
    <syncfusion:SfBulletGraph CaptionPosition="Far" FeaturedMeasure="375" Maximum="500" Margin="0,0,0,10">
        <syncfusion:SfBulletGraph.Caption>
            <StackPanel Margin="10,0,0,0">
                <TextBlock Text="Revenue" FontSize="13" FontWeight="SemiBold"/>
                <TextBlock Text="$ in Thousands" FontSize="11" Foreground="Gray"/>
            </StackPanel>
        </syncfusion:SfBulletGraph.Caption>
    </syncfusion:SfBulletGraph>
    
    <syncfusion:SfBulletGraph CaptionPosition="Far" FeaturedMeasure="7500" Maximum="10000" Margin="0,0,0,10">
        <syncfusion:SfBulletGraph.Caption>
            <StackPanel Margin="10,0,0,0">
                <TextBlock Text="Expenses" FontSize="13" FontWeight="SemiBold"/>
                <TextBlock Text="$ Monthly" FontSize="11" Foreground="Gray"/>
            </StackPanel>
        </syncfusion:SfBulletGraph.Caption>
    </syncfusion:SfBulletGraph>
    
    <syncfusion:SfBulletGraph CaptionPosition="Far" FeaturedMeasure="85" Maximum="100">
        <syncfusion:SfBulletGraph.Caption>
            <StackPanel Margin="10,0,0,0">
                <TextBlock Text="Customer Satisfaction" FontSize="13" FontWeight="SemiBold"/>
                <TextBlock Text="Score %" FontSize="11" Foreground="Gray"/>
            </StackPanel>
        </syncfusion:SfBulletGraph.Caption>
    </syncfusion:SfBulletGraph>
</StackPanel>
```

### Report Format

```xml
<syncfusion:SfBulletGraph CaptionPosition="Near" FeaturedMeasure="65" Maximum="100">
    <syncfusion:SfBulletGraph.Caption>
        <StackPanel Margin="0,0,15,0">
            <TextBlock Text="Q1 Performance" FontSize="12" HorizontalAlignment="Right"/>
            <TextBlock Text="Target: 80%" FontSize="10" Foreground="Gray" HorizontalAlignment="Right"/>
        </StackPanel>
    </syncfusion:SfBulletGraph.Caption>
</syncfusion:SfBulletGraph>
```

## Troubleshooting

**Issue:** Caption text is cut off  
**Solution:** Increase the margin or ensure the parent container provides sufficient space. Check `StackPanel.Margin` values.

**Issue:** Caption positioning doesn't match expected location  
**Solution:** Verify the `CaptionPosition` property value. Remember: Near = Left/Top, Far = Right/Bottom.

**Issue:** Caption doesn't align with bullet graph  
**Solution:** Set `VerticalAlignment` or `HorizontalAlignment` on caption TextBlocks. Use consistent margins.

**Issue:** Caption styling inconsistent across graphs  
**Solution:** Create a `Style` resource and apply it to all caption TextBlocks for consistency.
