# Chart Series Types

SfChart supports 38+ series types for different data visualization needs. This guide covers the most commonly used series types and when to use them.

## Table of Contents
- [Line Series](#line-series)
- [Column and Bar Series](#column-and-bar-series)
- [Area Series](#area-series)
- [Pie and Doughnut Series](#pie-and-doughnut-series)
- [Financial Series](#financial-series)
- [Scatter and Bubble Series](#scatter-and-bubble-series)
- [Spline Series](#spline-series)
- [Stacking Series](#stacking-series)
- [Other Series Types](#other-series-types)

## Line Series

**Best for:** Trends over time, continuous data

```xml
<syncfusion:LineSeries ItemsSource="{Binding Data}"
                       XBindingPath="Date"
                       YBindingPath="Value"
                       Label="Sales Trend"/>
```

**Variants:**
- `LineSeries` - Standard line chart
- `StepLineSeries` - Step-wise line chart
- `FastLineSeries` - Optimized for large datasets

## Column and Bar Series

**Best for:** Comparing values across categories

### Column Series (Vertical)

```xml
<syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                         XBindingPath="Category"
                         YBindingPath="Value"/>
```

### Bar Series (Horizontal)

```xml
<syncfusion:BarSeries ItemsSource="{Binding Data}"
                      XBindingPath="Category"
                      YBindingPath="Value"/>
```

**Variants:**
- `ColumnSeries` / `BarSeries` - Standard columns/bars
- `RangeColumnSeries` / `RangeBarSeries` - Show value ranges
- `FastColumnBitmapSeries` / `FastBarBitmapSeries` - Performance optimized

## Area Series

**Best for:** Showing cumulative totals and magnitude of change

```xml
<syncfusion:AreaSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Revenue"/>
```

**Variants:**
- `AreaSeries` - Filled area under line
- `SplineAreaSeries` - Smooth curved area
- `StepAreaSeries` - Step-wise area

## Pie and Doughnut Series

**Best for:** Showing proportions and percentages

### Pie Series

```xml
<syncfusion:PieSeries ItemsSource="{Binding Data}"
                      XBindingPath="Category"
                      YBindingPath="Percentage"/>
```

### Doughnut Series

```xml
<syncfusion:DoughnutSeries ItemsSource="{Binding Data}"
                           XBindingPath="Category"
                           YBindingPath="Percentage"
                           DoughnutCoefficient="0.6"/>
```

**Key Properties:**
- `StartAngle` - Starting angle for the first slice
- `EndAngle` - Ending angle for the last slice
- `DoughnutCoefficient` - Inner radius ratio (doughnut only)

## Financial Series

**Best for:** Stock market data, financial analysis

### Candlestick Series

```xml
<syncfusion:CandleSeries ItemsSource="{Binding StockData}"
                         XBindingPath="Date"
                         Open="Open"
                         High="High"
                         Low="Low"
                         Close="Close"/>
```

### OHLC Series

```xml
<syncfusion:HiLoOpenCloseSeries ItemsSource="{Binding StockData}"
                                XBindingPath="Date"
                                Open="Open"
                                High="High"
                                Low="Low"
                                Close="Close"/>
```

**Financial Series Types:**
- `CandleSeries` - Japanese candlestick chart
- `HiLoOpenCloseSeries` - OHLC bars
- `HiLoSeries` - High-Low range only

## Scatter and Bubble Series

**Best for:** Correlation analysis, multi-dimensional data

### Scatter Series

```xml
<syncfusion:ScatterSeries ItemsSource="{Binding Data}"
                          XBindingPath="X"
                          YBindingPath="Y"
                          ScatterHeight="10"
                          ScatterWidth="10"/>
```

### Bubble Series

```xml
<syncfusion:BubbleSeries ItemsSource="{Binding Data}"
                         XBindingPath="X"
                         YBindingPath="Y"
                         Size="Size"
                         MinimumRadius="5"
                         MaximumRadius="20"/>
```

## Spline Series

**Best for:** Smooth curves, continuous data trends

```xml
<syncfusion:SplineSeries ItemsSource="{Binding Data}"
                         XBindingPath="Month"
                         YBindingPath="Temperature"
                         SplineType="Cardinal"/>
```

**Spline Types:**
- `Natural` - Natural spline curve
- `Monotonic` - Monotonic spline curve
- `Cardinal` - Cardinal spline curve
- `Clamped` - Clamped spline curve

## Stacking Series

**Best for:** Part-to-whole relationships over time

### Stacking Column

```xml
<syncfusion:StackingColumnSeries ItemsSource="{Binding Product1Data}"
                                 XBindingPath="Month"
                                 YBindingPath="Sales"
                                 Label="Product 1"/>

<syncfusion:StackingColumnSeries ItemsSource="{Binding Product2Data}"
                                 XBindingPath="Month"
                                 YBindingPath="Sales"
                                 Label="Product 2"/>
```

**Stacking Series Types:**
- `StackingColumnSeries` - Stacked columns
- `StackingBarSeries` - Stacked bars
- `StackingAreaSeries` - Stacked areas
- `StackingLineSeries` - Stacked lines

### 100% Stacking

Use `StackingColumn100Series` to show percentage contribution:

```xml
<syncfusion:StackingColumn100Series ItemsSource="{Binding Data}"
                                    XBindingPath="Month"
                                    YBindingPath="Value"/>
```

## Other Series Types

### Funnel and Pyramid

**Best for:** Process stages, hierarchical data

```xml
<syncfusion:FunnelSeries ItemsSource="{Binding Data}"
                         XBindingPath="Stage"
                         YBindingPath="Count"/>

<syncfusion:PyramidSeries ItemsSource="{Binding Data}"
                          XBindingPath="Level"
                          YBindingPath="Population"/>
```

### Radar and Polar

**Best for:** Cyclical data, multivariate analysis

```xml
<syncfusion:RadarSeries ItemsSource="{Binding Data}"
                        XBindingPath="Direction"
                        YBindingPath="Speed"/>

<syncfusion:PolarSeries ItemsSource="{Binding Data}"
                        XBindingPath="Angle"
                        YBindingPath="Magnitude"/>
```

### Range Series

**Best for:** Temperature ranges, price ranges

```xml
<syncfusion:RangeAreaSeries ItemsSource="{Binding Data}"
                            XBindingPath="Date"
                            High="HighTemp"
                            Low="LowTemp"/>

<syncfusion:RangeColumnSeries ItemsSource="{Binding Data}"
                              XBindingPath="Month"
                              High="MaxPrice"
                              Low="MinPrice"/>
```

## Combining Multiple Series

You can combine different series types on the same chart:

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:CategoryAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <!-- Column Series -->
    <syncfusion:ColumnSeries ItemsSource="{Binding Sales}"
                             XBindingPath="Month"
                             YBindingPath="Amount"
                             Label="Sales"/>
    
    <!-- Line Series -->
    <syncfusion:LineSeries ItemsSource="{Binding Target}"
                           XBindingPath="Month"
                           YBindingPath="Amount"
                           Label="Target"/>
</syncfusion:SfChart>
```

## Series Selection Guide

| Data Type | Recommended Series |
|-----------|-------------------|
| Trends over time | LineSeries, SplineSeries |
| Comparing categories | ColumnSeries, BarSeries |
| Proportions/percentages | PieSeries, DoughnutSeries |
| Stock/financial data | CandleSeries, HiLoOpenCloseSeries |
| Cumulative totals | AreaSeries, StackingAreaSeries |
| Correlation | ScatterSeries, BubbleSeries |
| Value ranges | RangeAreaSeries, RangeColumnSeries |
| Process stages | FunnelSeries, PyramidSeries |
| Large datasets | FastLineSeries, FastColumnSeries |

## Performance Considerations

**Fast Series:** Use `FastLineSeries`, `FastColumnSeries`, `FastBarSeries` for datasets with 1000+ points.

**Regular Series:** Use standard series for rich interactivity and visual features with smaller datasets (<1000 points).
