# Appearance Customization in WPF SfChart3D

## Table of Contents
- [Overview](#overview)
- [Predefined Palettes](#predefined-palettes)
- [Custom Palettes](#custom-palettes)
- [SegmentColorPath](#segmentcolorpath)
- [Series Interior](#series-interior)
- [Theme Integration](#theme-integration)
- [Best Practices](#best-practices)

## Overview

SfChart3D provides comprehensive appearance customization options to create visually appealing and brand-consistent 3D visualizations. You can apply predefined color palettes, create custom color schemes, bind colors from data, and integrate with Syncfusion themes.

**Customization Levels:**
- **Chart Level** - Apply palettes to all series
- **Series Level** - Individual series styling
- **Segment Level** - Per-data-point coloring

## Predefined Palettes

Syncfusion provides 12 built-in color palettes for quick, professional styling.

###Available Palettes

- **Metro** (default)
- **AutumnBrights**
- **FloraHues**
- **Pineapple**
- **TomotoSpectrum**
- **RedChrome**
- **PurpleChrome**
- **BlueChrome**
- **GreenChrome**
- **Elite**
- **LightCandy**
- **SandyBeach**

### Applying Palette to Series

Apply a palette to color multiple series with coordinated colors:

**XAML:**
```xml
<chart:SfChart3D Height="250" Width="350" Palette="Metro">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <chart:ColumnSeries3D ItemsSource="{Binding Data1}" 
                          XBindingPath="Category" YBindingPath="Value"/>
    <chart:ColumnSeries3D ItemsSource="{Binding Data2}" 
                          XBindingPath="Category" YBindingPath="Value"/>
    <chart:ColumnSeries3D ItemsSource="{Binding Data3}" 
                          XBindingPath="Category" YBindingPath="Value"/>
    
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    Height = 250,
    Width = 350,
    Palette = ChartColorPalette.Metro
};

// Add multiple series - each gets a different color from palette
chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
chart3D.Series.Add(series3);
```

**Example with BlueChrome:**

**XAML:**
```xml
<chart:SfChart3D Height="250" Width="350" Palette="BlueChrome">
    <!-- Series configuration -->
</chart:SfChart3D>
```

**C#:**
```csharp
chart3D.Palette = ChartColorPalette.BlueChrome;
```

### Applying Palette to Segments

Apply palettes to individual segments within a single series:

**XAML:**
```xml
<chart:ColumnSeries3D 
    ItemsSource="{Binding Data}"
    XBindingPath="XValue"
    YBindingPath="YValue"
    Palette="Metro"/>
```

**C#:**
```csharp
ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = viewModel.Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    Palette = ChartColorPalette.Metro
};

chart3D.Series.Add(series);
```

**Example with AutumnBrights:**

**XAML:**
```xml
<chart:ColumnSeries3D 
    ItemsSource="{Binding Data}"
    XBindingPath="XValue"
    YBindingPath="YValue"
    Palette="AutumnBrights"/>
```

**C#:**
```csharp
ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = viewModel.Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    Palette = ChartColorPalette.AutumnBrights
};

chart3D.Series.Add(series);
```

**Note:** Metro is the default palette for both series and segments if none is specified.

## Custom Palettes

Create custom color schemes using the ColorModel property to match your brand colors or design requirements.

### Creating Custom Palette

**XAML:**
```xml
<chart:SfChart3D Height="250" Width="350" Palette="Custom">
    
    <!-- Define Custom Color Model -->
    <chart:SfChart3D.ColorModel>
        <chart:ChartColorModel>
            <chart:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="BlueViolet"/>
                <SolidColorBrush Color="PeachPuff"/>
                <SolidColorBrush Color="Purple"/>
                <SolidColorBrush Color="Teal"/>
                <SolidColorBrush Color="Coral"/>
            </chart:ChartColorModel.CustomBrushes>
        </chart:ChartColorModel>
    </chart:SfChart3D.ColorModel>
    
    <!-- Series will use custom colors -->
    <chart:ColumnSeries3D ItemsSource="{Binding Data1}" 
                          XBindingPath="X" YBindingPath="Y"/>
    <chart:ColumnSeries3D ItemsSource="{Binding Data2}" 
                          XBindingPath="X" YBindingPath="Y"/>
    
</chart:SfChart3D>
```

**C#:**
```csharp
// Create chart
SfChart3D chart3D = new SfChart3D()
{
    Palette = ChartColorPalette.Custom
};

// Define custom color model
ChartColorModel colorModel = new ChartColorModel();
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.BlueViolet));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.PeachPuff));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Purple));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Teal));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Coral));

chart3D.ColorModel = colorModel;

// Add series
chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
```

### Custom Palette for Segments

Apply custom colors to segments within a series:

**XAML:**
```xml
<chart:DoughnutSeries3D  
    YBindingPath="Percentage" 
    XBindingPath="Category" 
    ItemsSource="{Binding Tax}"
    Palette="Custom">
    
    <!-- Custom segment colors -->
    <chart:DoughnutSeries3D.ColorModel>
        <chart:ChartColorModel>
            <chart:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="Cyan"/>
                <SolidColorBrush Color="DarkCyan"/>
                <SolidColorBrush Color="LightCyan"/>
                <SolidColorBrush Color="Aqua"/>
            </chart:ChartColorModel.CustomBrushes>
        </chart:ChartColorModel>
    </chart:DoughnutSeries3D.ColorModel>
    
</chart:DoughnutSeries3D>
```

**C#:**
```csharp
// Create color model
ChartColorModel colorModel = new ChartColorModel();
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Cyan));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.DarkCyan));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.LightCyan));
colorModel.CustomBrushes.Add(new SolidColorBrush(Colors.Aqua));

// Create series with custom colors
DoughnutSeries3D series = new DoughnutSeries3D()
{
    ItemsSource = new ViewModel().Tax,
    XBindingPath = "Category",
    YBindingPath = "Percentage",
    Palette = ChartColorPalette.Custom,
    ColorModel = colorModel
};

chart3D.Series.Add(series);
```

### Modern Color Schemes

**Example: Material Design Colors**

**XAML:**
```xml
<chart:ChartColorModel.CustomBrushes>
    <SolidColorBrush Color="#6366F1"/>  <!-- Indigo -->
    <SolidColorBrush Color="#8B5CF6"/>  <!-- Purple -->
    <SolidColorBrush Color="#EC4899"/>  <!-- Pink -->
    <SolidColorBrush Color="#F59E0B"/>  <!-- Amber -->
    <SolidColorBrush Color="#10B981"/>  <!-- Emerald -->
</chart:ChartColorModel.CustomBrushes>
```

**Example: Gradient Colors**

**XAML:**
```xml
<chart:ChartColorModel.CustomBrushes>
    <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
        <GradientStop Color="#4F46E5" Offset="0"/>
        <GradientStop Color="#7C3AED" Offset="1"/>
    </LinearGradientBrush>
    <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
        <GradientStop Color="#EC4899" Offset="0"/>
        <GradientStop Color="#F43F5E" Offset="1"/>
    </LinearGradientBrush>
</chart:ChartColorModel.CustomBrushes>
```

## SegmentColorPath

Bind segment colors directly from your data source using the SegmentColorPath property.

### Data-Driven Coloring

**When to use:**
- Colors represent data categories
- Dynamic color assignment based on data
- Conditional coloring (e.g., positive/negative values)
- Custom color logic per data point

**Data Model:**
```csharp
public class Model
{
    public string XValue { get; set; }
    public double YValue { get; set; }
    public Brush ColorPath { get; set; }  // Color property
}

public class ViewModel
{
    public ObservableCollection<Model> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Model>();
        Data.Add(new Model 
        { 
            XValue = "Jan", 
            YValue = 10, 
            ColorPath = new SolidColorBrush(Colors.Cyan) 
        });
        Data.Add(new Model 
        { 
            XValue = "Feb", 
            YValue = 24, 
            ColorPath = new SolidColorBrush(Colors.Pink) 
        });
        Data.Add(new Model 
        { 
            XValue = "Mar", 
            YValue = 18, 
            ColorPath = new SolidColorBrush(Colors.Red) 
        });
        Data.Add(new Model 
        { 
            XValue = "Apr", 
            YValue = 16, 
            ColorPath = new SolidColorBrush(Colors.Orange) 
        });
        Data.Add(new Model 
        { 
            XValue = "May", 
            YValue = 28, 
            ColorPath = new SolidColorBrush(Colors.LightGreen) 
        });
    }
}
```

**XAML:**
```xml
<chart:ColumnSeries3D 
    ItemsSource="{Binding Data}" 
    XBindingPath="XValue" 
    YBindingPath="YValue"
    SegmentColorPath="ColorPath">
</chart:ColumnSeries3D>
```

**C#:**
```csharp
ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = viewModel.Data,
    XBindingPath = "XValue",
    YBindingPath = "YValue",
    SegmentColorPath = "ColorPath"  // Binds to ColorPath property
};

chart3D.Series.Add(series);
```

**Note:** SegmentColorPath is not applicable to Area and CircularSeries (Pie, Doughnut).

### Conditional Coloring Example

**Color based on value:**
```csharp
public class SalesData
{
    public string Month { get; set; }
    public double Sales { get; set; }
    
    public Brush ColorPath
    {
        get
        {
            // Green for high sales, red for low sales
            if (Sales >= 50)
                return new SolidColorBrush(Colors.Green);
            else if (Sales >= 30)
                return new SolidColorBrush(Colors.Orange);
            else
                return new SolidColorBrush(Colors.Red);
        }
    }
}
```

## Series Interior

Set a single color for an entire series using the Interior property.

**XAML:**
```xml
<chart:ColumnSeries3D 
    ItemsSource="{Binding Data}" 
    XBindingPath="X" 
    YBindingPath="Y"
    Interior="Teal"/>
    
<chart:ColumnSeries3D 
    ItemsSource="{Binding Data2}" 
    XBindingPath="X" 
    YBindingPath="Y"
    Interior="Coral"/>
```

**C#:**
```csharp
ColumnSeries3D series1 = new ColumnSeries3D()
{
    ItemsSource = viewModel.Data,
    XBindingPath = "X",
    YBindingPath = "Y",
    Interior = new SolidColorBrush(Colors.Teal)
};

ColumnSeries3D series2 = new ColumnSeries3D()
{
    ItemsSource = viewModel.Data2,
    XBindingPath = "X",
    YBindingPath = "Y",
    Interior = new SolidColorBrush(Colors.Coral)
};

chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
```

**Use Interior when:**
- Single-color series styling
- Specific brand colors required
- Overriding palette colors

## Theme Integration

SfChart3D integrates with Syncfusion's theme framework for consistent application-wide styling.

### Using SfSkinManager

Apply themes using SfSkinManager:

**C#:**
```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    
    // Apply theme to window
    SfSkinManager.SetTheme(this, new Theme("MaterialDark"));
}
```

**Available Themes:**
- **MaterialLight**
- **MaterialDark**
- **Office2019Colorful**
- **Office2019Black**
- **Office2019White**
- **FluentLight**
- **FluentDark**

## Best Practices

### Palette Selection

**Choose palettes based on:**
- **Data Type:** Use contrasting colors for categorical data
- **Brand Identity:** Match company colors with custom palettes
- **Accessibility:** Ensure sufficient color contrast
- **Context:** Professional (Chrome palettes) vs. Creative (Flora, Autumn)

### Color Guidelines

**Effective Color Usage:**
- Limit to 5-7 distinct colors per chart
- Use color to highlight important data
- Maintain consistency across related charts
- Test in grayscale for accessibility

**Color Combinations:**
- **Sequential:** Similar hues for ordered data
- **Diverging:** Contrasting hues for positive/negative values
- **Qualitative:** Distinct hues for categories

### Performance Tips

**Optimization:**
- Use predefined palettes when possible (better performance)
- Avoid complex gradients for large datasets
- Cache color objects in data models
- Use SegmentColorPath judiciously

### Common Patterns

**Pattern 1: Multi-Series with Palette**
```xml
<chart:SfChart3D Palette="BlueChrome">
    <chart:ColumnSeries3D ItemsSource="{Binding Sales}"/>
    <chart:ColumnSeries3D ItemsSource="{Binding Costs}"/>
    <chart:ColumnSeries3D ItemsSource="{Binding Profit}"/>
</chart:SfChart3D>
```

**Pattern 2: Single Series with Segment Colors**
```xml
<chart:ColumnSeries3D 
    Palette="AutumnBrights"
    ItemsSource="{Binding Data}"/>
```

**Pattern 3: Custom Brand Colors**
```xml
<chart:SfChart3D Palette="Custom">
    <chart:SfChart3D.ColorModel>
        <chart:ChartColorModel>
            <chart:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="#FF6B35"/>  <!-- Brand Orange -->
                <SolidColorBrush Color="#004E89"/>  <!-- Brand Blue -->
                <SolidColorBrush Color="#1A936F"/>  <!-- Brand Green -->
            </chart:ChartColorModel.CustomBrushes>
        </chart:ChartColorModel>
    </chart:SfChart3D.ColorModel>
</chart:SfChart3D>
```

**Pattern 4: Data-Driven Colors**
```xml
<chart:ColumnSeries3D 
    SegmentColorPath="StatusColor"
    ItemsSource="{Binding StatusData}"/>
```

### Accessibility Considerations

**WCAG Compliance:**
- Minimum contrast ratio of 4.5:1 for text
- Don't rely solely on color to convey information
- Provide alternative indicators (patterns, labels)
- Test with color blindness simulators

**Color-Blind Friendly Palettes:**
- Avoid red-green combinations only
- Use blue-orange combinations
- Add patterns or textures
- Include data labels

### Edge Cases

**Issue: Colors Not Applying**
- Ensure Palette="Custom" when using ColorModel
- Verify brush objects are properly created
- Check SegmentColorPath property name matches data model

**Issue: Gradient Not Rendering in 3D**
- 3D rendering may not support all gradient types
- Use solid colors for consistent results
- Test gradients with different rotation angles

**Issue: Theme Colors Overriding Custom Colors**
- Custom colors take precedence over themes
- Apply Interior or custom palette after theme
- Check SfSkinManager application order

**Issue: Too Many Colors in Palette**
- Palette cycles after running out of colors
- Provide enough colors for your data points
- Consider data aggregation if too many categories
