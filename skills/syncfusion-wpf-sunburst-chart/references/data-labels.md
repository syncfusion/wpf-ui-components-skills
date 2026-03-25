# Data Labels

## Table of Contents
- [Overview](#overview)
- [Enabling Data Labels](#enabling-data-labels)
- [Label Overflow Management](#label-overflow-management)
- [Label Rotation](#label-rotation)
- [Font Customization](#font-customization)
- [Custom Label Templates](#custom-label-templates)
- [Best Practices](#best-practices)

## Overview

Data labels display information about segments directly on the sunburst chart, showing category names and values. They improve readability by providing immediate context without requiring tooltips or legends.

**Key Features:**
- Display category names and values on segments
- Automatic positioning based on segment size
- Overflow handling (Hide or Trim for small segments)
- Rotation options for better readability
- Full font and color customization
- Custom templates for advanced scenarios

## Enabling Data Labels

Data labels are controlled through the `DataLabelInfo` property with a `SunburstDataLabelInfo` object.

### Basic Configuration

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.DataLabelInfo>
        <sunburst:SunburstDataLabelInfo ShowLabel="True"/>
    </sunburst:SfSunburstChart.DataLabelInfo>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SunburstDataLabelInfo dataLabelInfo = new SunburstDataLabelInfo()
{
    ShowLabel = true
};
chart.DataLabelInfo = dataLabelInfo;
```

**Note:** By default, `ShowLabel` is `True`, so labels are visible unless explicitly disabled.

### Disabling Labels

To hide labels completely:

**XAML:**
```xml
<sunburst:SfSunburstChart.DataLabelInfo>
    <sunburst:SunburstDataLabelInfo ShowLabel="False"/>
</sunburst:SfSunburstChart.DataLabelInfo>
```

**C#:**
```csharp
dataLabelInfo.ShowLabel = false;
```

## Label Overflow Management

When segments are small, labels may overlap or extend beyond segment boundaries. The `LabelOverflowMode` property manages this.

### Available Modes

- **Trim** – Truncate labels that don't fit within segments
- **Hide** – Completely hide labels on segments that are too small

### Trim Mode

Truncates labels with ellipsis (...) when they exceed segment width:

**XAML:**
```xml
<sunburst:SfSunburstChart.DataLabelInfo>
    <sunburst:SunburstDataLabelInfo ShowLabel="True" 
                                    LabelOverflowMode="Trim"/>
</sunburst:SfSunburstChart.DataLabelInfo>
```

**C#:**
```csharp
SunburstDataLabelInfo dataLabelInfo = new SunburstDataLabelInfo()
{
    ShowLabel = true,
    LabelOverflowMode = LabelOverflowMode.Trim
};
chart.DataLabelInfo = dataLabelInfo;
```

**Result:** Labels appear as "Development", "Devel...", or "Dev..." depending on available space.

### Hide Mode

Completely hides labels when segments are too small:

**XAML:**
```xml
<sunburst:SfSunburstChart.DataLabelInfo>
    <sunburst:SunburstDataLabelInfo ShowLabel="True"
                                    LabelOverflowMode="Hide"/>
</sunburst:SfSunburstChart.DataLabelInfo>
```

**C#:**
```csharp
SunburstDataLabelInfo dataLabelInfo = new SunburstDataLabelInfo()
{
    ShowLabel = true,
    LabelOverflowMode = LabelOverflowMode.Hide
};
chart.DataLabelInfo = dataLabelInfo;
```

**Result:** Only segments with sufficient space display labels; tiny segments show no label.

**Recommendation:** Use `Hide` for cleaner visuals; use `Trim` to maximize information density.

## Label Rotation

The `LabelRotationMode` property controls label orientation.

### Rotation Modes

- **Angle** (default) – Labels follow the segment's arc angle
- **Normal** – Labels remain horizontal regardless of segment angle

### Angle Mode

Labels rotate to align with segment curves:

**XAML:**
```xml
<sunburst:SfSunburstChart.DataLabelInfo>
    <sunburst:SunburstDataLabelInfo ShowLabel="True" 
                                    LabelRotationMode="Angle"/>
</sunburst:SfSunburstChart.DataLabelInfo>
```

**C#:**
```csharp
dataLabelInfo.LabelRotationMode = LabelRotationMode.Angle;
```

**Best for:** Natural appearance following circular layout; default and recommended setting.

### Normal Mode

Labels remain horizontal (no rotation):

**XAML:**
```xml
<sunburst:SfSunburstChart.DataLabelInfo>
    <sunburst:SunburstDataLabelInfo ShowLabel="True" 
                                    LabelRotationMode="Normal"/>
</sunburst:SfSunburstChart.DataLabelInfo>
```

**C#:**
```csharp
SunburstDataLabelInfo dataLabelInfo = new SunburstDataLabelInfo()
{
    ShowLabel = true,
    LabelRotationMode = LabelRotationMode.Normal
};
chart.DataLabelInfo = dataLabelInfo;
```

**Best for:** Easier reading when segments are on top/bottom of the chart; may cause overlap.

**Note:** `Angle` mode (default) is generally recommended as it prevents overlapping and follows the chart's circular nature.

## Font Customization

Customize label appearance using standard WPF font properties.

### Font Properties

- **FontFamily** – Font typeface (e.g., Arial, Segoe UI)
- **FontSize** – Text size in points
- **FontStyle** – Normal, Italic, Oblique
- **FontWeight** – Normal, Bold, SemiBold, etc.
- **Foreground** – Text color

### Complete Font Customization Example

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="EmployeesCount">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobRole"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.DataLabelInfo>
        <sunburst:SunburstDataLabelInfo ShowLabel="True" 
                                        FontSize="12" 
                                        FontFamily="Comic Sans MS"
                                        FontStyle="Italic" 
                                        FontWeight="SemiBold"
                                        Foreground="White"/>
    </sunburst:SfSunburstChart.DataLabelInfo>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
SfSunburstChart sunburst = new SfSunburstChart();
sunburst.ValueMemberPath = "EmployeesCount";
sunburst.SetBinding(SfSunburstChart.ItemsSourceProperty, "Data");

// Add levels
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "Country" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobDescription" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobGroup" });
sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobRole" });

// Configure data labels
SunburstDataLabelInfo info = new SunburstDataLabelInfo();
info.ShowLabel = true;
info.FontSize = 12;
info.FontStyle = FontStyles.Italic;
info.FontWeight = FontWeights.SemiBold;
info.Foreground = new SolidColorBrush(Colors.White);
info.FontFamily = new FontFamily("Comic Sans MS");

sunburst.DataLabelInfo = info;
```

### Font Size Recommendations

- **Small charts (300-500px):** FontSize = 8-10
- **Medium charts (500-800px):** FontSize = 10-12 (default)
- **Large charts (800+px):** FontSize = 12-14

### Color Contrast

Ensure labels are readable against segment colors:

**Light segments:**
```xml
<sunburst:SunburstDataLabelInfo Foreground="Black"/>
```

**Dark segments:**
```xml
<sunburst:SunburstDataLabelInfo Foreground="White"/>
```

**Dynamic coloring:** Use custom templates (see below) to set color based on segment background.

## Custom Label Templates

For advanced scenarios, use the `LabelTemplate` property to create custom label designs.

### Basic Custom Template

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="Value">
    
    <sunburst:SfSunburstChart.DataLabelInfo>
        <sunburst:SunburstDataLabelInfo ShowLabel="True">
            
            <sunburst:SunburstDataLabelInfo.LabelTemplate>
                <DataTemplate>
                    <Border Background="LightGray" 
                            CornerRadius="4" 
                            Padding="4,2">
                        <TextBlock Text="{Binding Category}" 
                                   FontWeight="Bold"
                                   Foreground="DarkBlue"/>
                    </Border>
                </DataTemplate>
            </sunburst:SunburstDataLabelInfo.LabelTemplate>
            
        </sunburst:SunburstDataLabelInfo>
    </sunburst:SfSunburstChart.DataLabelInfo>
    
</sunburst:SfSunburstChart>
```

### Available Binding Properties

Within the template, you can bind to:
- **Category** – Segment category name
- **Value** – Segment numeric value
- **Percentage** – Segment percentage of parent
- **Interior** – Segment fill color

### Template with Value Display

Show both category and value:

```xml
<sunburst:SunburstDataLabelInfo.LabelTemplate>
    <DataTemplate>
        <StackPanel Orientation="Vertical">
            <TextBlock Text="{Binding Category}" 
                       FontWeight="Bold" 
                       FontSize="10"/>
            <TextBlock Text="{Binding Value, StringFormat='{}{0:N0}'}" 
                       FontSize="8" 
                       Opacity="0.8"/>
        </StackPanel>
    </DataTemplate>
</sunburst:SunburstDataLabelInfo.LabelTemplate>
```

### Template with Color-Coded Background

Use segment color as label background:

```xml
<sunburst:SunburstDataLabelInfo.LabelTemplate>
    <DataTemplate>
        <Border Background="{Binding Interior}"
                BorderBrush="{Binding Interior}"
                BorderThickness="2"
                CornerRadius="3"
                Padding="3,1">
            <TextBlock Text="{Binding Category}" 
                       Foreground="White"
                       FontWeight="SemiBold"/>
        </Border>
    </DataTemplate>
</sunburst:SunburstDataLabelInfo.LabelTemplate>
```

### Template with Icon

Add visual indicators:

```xml
<sunburst:SunburstDataLabelInfo.LabelTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <Ellipse Width="8" Height="8" 
                     Fill="{Binding Interior}" 
                     Margin="0,0,4,0"/>
            <TextBlock Text="{Binding Category}" 
                       FontSize="10"/>
        </StackPanel>
    </DataTemplate>
</sunburst:SunburstDataLabelInfo.LabelTemplate>
```

## Best Practices

### 1. Choose Appropriate Overflow Mode

- Use **Hide** for cleaner visuals with many small segments
- Use **Trim** when every segment needs a label (even partial)

### 2. Optimize Font Size

- Start with default (10-12px) and adjust based on chart size
- Smaller fonts for charts with many segments
- Larger fonts for simple hierarchies

### 3. Ensure Readability

- High contrast between label and segment color
- Use White foreground on dark palettes
- Use Black/DarkGray foreground on light palettes

### 4. Rotation Mode Selection

- **Angle** (default) for most cases – natural and prevents overlap
- **Normal** only for top/bottom segments where horizontal is easier to read

### 5. Custom Templates

- Keep templates simple for performance
- Avoid heavy visuals (gradients, shadows) on every label
- Test with real data to ensure visibility

### 6. Accessibility

- Ensure minimum font size of 9px for readability
- Provide tooltips as an alternative to small labels
- Use sufficient color contrast (WCAG AA: 4.5:1 minimum)

## Complete Example

Combining all features:

```xml
<sunburst:SfSunburstChart ItemsSource="{Binding EmployeeData}" 
                          ValueMemberPath="Count"
                          Palette="Metro">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Department"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Team"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Role"/>
    </sunburst:SfSunburstChart.Levels>
    
    <sunburst:SfSunburstChart.DataLabelInfo>
        <sunburst:SunburstDataLabelInfo ShowLabel="True"
                                        LabelOverflowMode="Hide"
                                        LabelRotationMode="Angle"
                                        FontFamily="Segoe UI"
                                        FontSize="11"
                                        FontWeight="SemiBold"
                                        Foreground="White">
            
            <sunburst:SunburstDataLabelInfo.LabelTemplate>
                <DataTemplate>
                    <Border Background="#80000000" 
                            CornerRadius="2" 
                            Padding="4,2">
                        <TextBlock Text="{Binding Category}" 
                                   Foreground="White"
                                   FontSize="10"/>
                    </Border>
                </DataTemplate>
            </sunburst:SunburstDataLabelInfo.LabelTemplate>
            
        </sunburst:SunburstDataLabelInfo>
    </sunburst:SfSunburstChart.DataLabelInfo>
    
</sunburst:SfSunburstChart>
```

## Troubleshooting

**Labels not appearing:**
- Verify `ShowLabel="True"`
- Check that DataLabelInfo is set
- Ensure segments are large enough (not hidden by LabelOverflowMode)

**Labels overlapping:**
- Switch to `LabelRotationMode="Angle"`
- Use `LabelOverflowMode="Hide"` to hide labels on small segments
- Reduce FontSize

**Labels truncated unexpectedly:**
- Change from `Trim` to `Hide` mode, or
- Increase chart size, or
- Reduce font size

**Custom template not displaying:**
- Verify DataTemplate syntax is correct
- Check binding paths (Category, Value, Interior)
- Ensure template elements have proper sizing

**Poor readability:**
- Increase contrast between Foreground and segment colors
- Add background to labels in custom template
- Increase FontWeight for better visibility
