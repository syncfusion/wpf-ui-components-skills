# Appearance and Styling

Customize the visual appearance of charts including colors, themes, backgrounds, and styles.

## Table of Contents
- [Chart Area Styling](#chart-area-styling)
- [Series Colors](#series-colors)
- [Chart Header](#chart-header)
- [Background and Border](#background-and-border)
- [Color Palettes](#color-palettes)

## Chart Area Styling

### Chart Background

```xml
<syncfusion:SfChart Background="WhiteSmoke"/>
```

### Chart Border

```xml
<syncfusion:SfChart BorderBrush="DarkGray"
                    BorderThickness="2"/>
```

### Area Border

Style the plot area border:

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.AreaBorderBrush>
        <SolidColorBrush Color="LightGray"/>
    </syncfusion:SfChart.AreaBorderBrush>
</syncfusion:SfChart>
```

## Series Colors

### Single Series Color

```xml
<syncfusion:ColumnSeries Interior="RoyalBlue"/>
```

### Gradient Fill

```xml
<syncfusion:ColumnSeries>
    <syncfusion:ColumnSeries.Interior>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="LightBlue" Offset="0"/>
            <GradientStop Color="DarkBlue" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:ColumnSeries.Interior>
</syncfusion:ColumnSeries>
```

### Segment Colors (Individual)

Assign different colors to each segment:

```xml
<syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                         XBindingPath="Category"
                         YBindingPath="Value"
                         Palette="Custom">
    <syncfusion:ColumnSeries.ColorModel>
        <syncfusion:ChartColorModel>
            <syncfusion:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="Red"/>
                <SolidColorBrush Color="Green"/>
                <SolidColorBrush Color="Blue"/>
                <SolidColorBrush Color="Orange"/>
            </syncfusion:ChartColorModel.CustomBrushes>
        </syncfusion:ChartColorModel>
    </syncfusion:ColumnSeries.ColorModel>
</syncfusion:ColumnSeries>
```

## Chart Header

### Simple Header

```xml
<syncfusion:SfChart Header="Monthly Sales Report"/>
```

### Custom Header Template

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Header>
        <StackPanel Orientation="Vertical" Margin="10">
            <TextBlock Text="Sales Dashboard" 
                       FontSize="20" 
                       FontWeight="Bold"
                       HorizontalAlignment="Center"/>
            <TextBlock Text="Q1 2024" 
                       FontSize="14" 
                       Foreground="Gray"
                       HorizontalAlignment="Center"/>
        </StackPanel>
    </syncfusion:SfChart.Header>
</syncfusion:SfChart>
```

## Background and Border

### Solid Background

```xml
<syncfusion:SfChart Background="White"/>
```

### Gradient Background

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="White" Offset="0"/>
            <GradientStop Color="LightGray" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:SfChart.Background>
</syncfusion:SfChart>
```

### Area Background

```xml
<syncfusion:SfChart AreaBackground="WhiteSmoke"/>
```

## Color Palettes

Apply predefined color schemes to series.

### Built-in Palettes

```xml
<syncfusion:ColumnSeries Palette="Metro"/>
```

**Available Palettes:**
- `Metro` - Modern flat colors
- `AutumnBrights` - Warm autumn colors
- `FloraHues` - Natural green/pink tones
- `Pineapple` - Yellow/orange tones
- `TomotoSpectrum` - Red spectrum
- `RedChrome` - Chrome red theme
- `BlueChrome` - Chrome blue theme
- `PurpleChrome` - Chrome purple theme
- `GreenChrome` - Chrome green theme
- `Elite` - Professional colors
- `LightCandy` - Pastel colors
- `SandyBeach` - Beach-inspired colors

### Custom Palette

```xml
<syncfusion:ColumnSeries Palette="Custom">
    <syncfusion:ColumnSeries.ColorModel>
        <syncfusion:ChartColorModel>
            <syncfusion:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="#FF6B9E"/>
                <SolidColorBrush Color="#4ECDC4"/>
                <SolidColorBrush Color="#FFA07A"/>
                <SolidColorBrush Color="#9370DB"/>
            </syncfusion:ChartColorModel.CustomBrushes>
        </syncfusion:ChartColorModel>
    </syncfusion:ColumnSeries.ColorModel>
</syncfusion:ColumnSeries>
```

## Series Styling

### Stroke and Fill

```xml
<syncfusion:LineSeries Interior="RoyalBlue"
                       Stroke="DarkBlue"
                       StrokeThickness="3"/>
```

### Line Series Dash Array

```xml
<syncfusion:LineSeries StrokeDashArray="5,3"/>
```

### Area Series Opacity

```xml
<syncfusion:AreaSeries Opacity="0.6"/>
```

## Grid Lines Styling

### Customize Grid Lines

```xml
<syncfusion:NumericalAxis ShowGridLines="True">
    <syncfusion:NumericalAxis.MajorGridLineStyle>
        <Style TargetType="Line">
            <Setter Property="Stroke" Value="LightGray"/>
            <Setter Property="StrokeThickness" Value="1"/>
            <Setter Property="StrokeDashArray" Value="3,3"/>
        </Style>
    </syncfusion:NumericalAxis.MajorGridLineStyle>
</syncfusion:NumericalAxis>
```

## Watermark

Add watermark text to the chart:

```xml
<syncfusion:SfChart Watermark="Confidential">
    <syncfusion:SfChart.WatermarkStyle>
        <Style TargetType="TextBlock">
            <Setter Property="FontSize" Value="60"/>
            <Setter Property="Foreground" Value="LightGray"/>
            <Setter Property="Opacity" Value="0.3"/>
        </Style>
    </syncfusion:SfChart.WatermarkStyle>
</syncfusion:SfChart>
```

## Themes

Apply visual themes to match your application.

### Metro Theme Example

```xml
<syncfusion:SfChart>
    <!-- Use Metro palette -->
    <syncfusion:ColumnSeries Palette="Metro"/>
    
    <!-- Modern flat styling -->
    <syncfusion:SfChart.Resources>
        <Style TargetType="syncfusion:ChartAxis">
            <Setter Property="ShowGridLines" Value="True"/>
        </Style>
    </syncfusion:SfChart.Resources>
</syncfusion:SfChart>
```

## Complete Styling Example

```xml
<syncfusion:SfChart Header="Sales Report Q1 2024"
                    Background="White"
                    BorderBrush="LightGray"
                    BorderThickness="1"
                    AreaBackground="WhiteSmoke">
    
    <!-- Styled Primary Axis -->
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:CategoryAxis Header="Month">
            <syncfusion:CategoryAxis.HeaderStyle>
                <Style TargetType="ContentControl">
                    <Setter Property="FontSize" Value="14"/>
                    <Setter Property="FontWeight" Value="Bold"/>
                </Style>
            </syncfusion:CategoryAxis.HeaderStyle>
            <syncfusion:CategoryAxis.LabelStyle>
                <syncfusion:LabelStyle FontSize="12"/>
            </syncfusion:CategoryAxis.LabelStyle>
        </syncfusion:CategoryAxis>
    </syncfusion:SfChart.PrimaryAxis>
    
    <!-- Styled Secondary Axis -->
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis Header="Sales ($)"
                                  LabelFormat="$0,K">
            <syncfusion:NumericalAxis.MajorGridLineStyle>
                <Style TargetType="Line">
                    <Setter Property="Stroke" Value="LightGray"/>
                    <Setter Property="StrokeDashArray" Value="2,2"/>
                </Style>
            </syncfusion:NumericalAxis.MajorGridLineStyle>
        </syncfusion:NumericalAxis>
    </syncfusion:SfChart.SecondaryAxis>
    
    <!-- Styled Series -->
    <syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                             XBindingPath="Month"
                             YBindingPath="Amount"
                             Palette="Metro">
        <syncfusion:ColumnSeries.AdornmentsInfo>
            <syncfusion:ChartAdornmentInfo ShowLabel="True"
                                           LabelPosition="Top"
                                           SegmentLabelFormat="$0,K">
                <syncfusion:ChartAdornmentInfo.LabelTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding}" 
                                   FontWeight="Bold"
                                   FontSize="11"/>
                    </DataTemplate>
                </syncfusion:ChartAdornmentInfo.LabelTemplate>
            </syncfusion:ChartAdornmentInfo>
        </syncfusion:ColumnSeries.AdornmentsInfo>
    </syncfusion:ColumnSeries>
    
    <!-- Legend -->
    <syncfusion:SfChart.Legend>
        <syncfusion:ChartLegend Background="WhiteSmoke"
                                BorderBrush="Gray"
                                BorderThickness="1"/>
    </syncfusion:SfChart.Legend>
</syncfusion:SfChart>
```

## Key Styling Properties

### Chart Level
- `Background` - Chart background color
- `BorderBrush` / `BorderThickness` - Chart border
- `AreaBackground` - Plot area background
- `AreaBorderBrush` - Plot area border
- `Header` - Chart title

### Series Level
- `Interior` - Fill color
- `Stroke` - Border/line color
- `StrokeThickness` - Border/line width
- `StrokeDashArray` - Dash pattern
- `Opacity` - Transparency
- `Palette` - Color scheme

### Axis Level
- `HeaderStyle` - Axis title style
- `LabelStyle` - Axis label style
- `MajorGridLineStyle` - Grid line style
- `AxisLineStyle` - Axis line style
