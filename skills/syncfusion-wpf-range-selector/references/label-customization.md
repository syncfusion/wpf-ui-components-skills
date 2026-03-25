# Label Customization in Range Selectors

## Table of Contents
- [Overview](#overview)
- [Interval Types](#interval-types)
- [Interval Configuration](#interval-configuration)
- [Label Formatting](#label-formatting)
- [Label Style Customization](#label-style-customization)
- [Label Bar Visibility](#label-bar-visibility)
- [Range Padding](#range-padding)

## Overview

The SfDateTimeRangeNavigator displays time information in two hierarchical label bars (higher and lower level). The control intelligently calculates appropriate intervals and label formats based on your data range and zoom level. You can also manually configure intervals, customize label appearance, and control label bar visibility.

**Key Customization Capabilities:**
- **Interval Types** - Year, Quarter, Month, Week, Day, Hour
- **Smart Labels** - Automatic format adjustment when zooming
- **Label Formatting** - Custom date-time format strings
- **Style Customization** - Fonts, colors, alignment for labels
- **Selected vs Unselected** - Different styles for selected regions
- **Label Bar Positioning** - Inside or outside bar placement
- **Range Padding** - None or Round padding modes

## Interval Types

The navigator supports six interval types that can be assigned to the higher and lower label bars:

### Available Interval Types

| Interval Type | Typical Use | Example Format |
|--------------|-------------|----------------|
| **Year** | Multi-year datasets | 2024, 2025, 2026 |
| **Quarter** | Quarterly business data | Q1 2024, Q2 2024 |
| **Month** | Annual or multi-year data | Jan, Feb, Mar |
| **Week** | Monthly data | Week 1, Week 2 |
| **Day** | Weekly or monthly data | Mon 15, Tue 16 |
| **Hour** | Intraday data | 12 AM, 1 AM, 2 AM |

### Smart Interval Selection

By default, the navigator automatically selects appropriate intervals based on your data range:
- **Years of data** → Higher: Year, Lower: Quarter or Month
- **Months of data** → Higher: Month, Lower: Week or Day
- **Days of data** → Higher: Day, Lower: Hour
- **Hours of data** → Higher: Hour, Lower: Minutes (calculated)

The smart selection ensures labels remain readable regardless of zoom level.

## Interval Configuration

### Setting Custom Intervals

Override automatic intervals by explicitly configuring the `Intervals` collection:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.Intervals>
        <!-- Higher level bar shows years -->
        <syncfusion:Interval IntervalType="Year"/>
        
        <!-- Lower level bar shows months -->
        <syncfusion:Interval IntervalType="Month"/>
    </syncfusion:SfDateTimeRangeNavigator.Intervals>
    
</syncfusion:SfDateTimeRangeNavigator>
```

**Order matters:** The first interval is assigned to the higher level bar, the second to the lower level bar.

### Code-Behind Interval Configuration

```csharp
SfDateTimeRangeNavigator rangeNavigator = new SfDateTimeRangeNavigator
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Date"
};

// Add intervals
rangeNavigator.Intervals.Add(new Interval { IntervalType = NavigatorIntervalType.Quarter });
rangeNavigator.Intervals.Add(new Interval { IntervalType = NavigatorIntervalType.Month });
```

### Common Interval Combinations

#### Annual Financial Data
```xml
<syncfusion:SfDateTimeRangeNavigator.Intervals>
    <syncfusion:Interval IntervalType="Year"/>
    <syncfusion:Interval IntervalType="Quarter"/>
</syncfusion:SfDateTimeRangeNavigator.Intervals>
```

#### Monthly Sales Data
```xml
<syncfusion:SfDateTimeRangeNavigator.Intervals>
    <syncfusion:Interval IntervalType="Quarter"/>
    <syncfusion:Interval IntervalType="Month"/>
</syncfusion:SfDateTimeRangeNavigator.Intervals>
```

#### Weekly Activity Tracking
```xml
<syncfusion:SfDateTimeRangeNavigator.Intervals>
    <syncfusion:Interval IntervalType="Month"/>
    <syncfusion:Interval IntervalType="Week"/>
</syncfusion:SfDateTimeRangeNavigator.Intervals>
```

#### Intraday Stock Trading
```xml
<syncfusion:SfDateTimeRangeNavigator.Intervals>
    <syncfusion:Interval IntervalType="Day"/>
    <syncfusion:Interval IntervalType="Hour"/>
</syncfusion:SfDateTimeRangeNavigator.Intervals>
```

## Label Formatting

### Automatic Format Strings

When you zoom in or out, the navigator automatically adjusts label formats for optimal readability. Here are the automatic formats for each interval type:

#### Hour Interval
- `7/21/2011 12:00:00 AM` → **12 AM**
- `7/21/2011 12:00:00 AM` → **12A**

#### Day Interval
- `7/21/2011 12:00:00 AM` → **Thursday, July 21, 2011**
- `7/21/2011 12:00:00 AM` → **Thu, Jul 21, 2011**
- `7/21/2011 12:00:00 AM` → **Thursday, 21**
- `7/21/2011 12:00:00 AM` → **Thu, 21**

#### Week Interval
- `7/21/2011 12:00:00 AM` → **Week 29, July, 2011**
- `7/21/2011 12:00:00 AM` → **Week 29, Jul, 2011**
- `7/21/2011 12:00:00 AM` → **Week 29**
- `7/21/2011 12:00:00 AM` → **W29**

#### Month Interval
- `7/21/2011 12:00:00 AM` → **July, 2011**
- `7/21/2011 12:00:00 AM` → **July**
- `7/21/2011 12:00:00 AM` → **Jul**
- `7/21/2011 12:00:00 AM` → **J**

#### Quarter Interval
- `7/21/2011 12:00:00 AM` → **Quarter 3, 2011**
- `7/21/2011 12:00:00 AM` → **Quarter 3**
- `7/21/2011 12:00:00 AM` → **Q3, 2011**
- `7/21/2011 12:00:00 AM` → **Q3**

#### Year Interval
- `7/21/2011 12:00:00 AM` → **2011**

### Custom Label Formatters

Override automatic formats with custom format strings using the `LabelFormatters` property:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.Intervals>
        <syncfusion:Interval IntervalType="Year">
            <syncfusion:Interval.LabelFormatters>
                <x:String>yyyy</x:String>
            </syncfusion:Interval.LabelFormatters>
        </syncfusion:Interval>
        
        <syncfusion:Interval IntervalType="Month">
            <syncfusion:Interval.LabelFormatters>
                <x:String>MMM</x:String>
                <x:String>MMMM</x:String>
            </syncfusion:Interval.LabelFormatters>
        </syncfusion:Interval>
    </syncfusion:SfDateTimeRangeNavigator.Intervals>
    
</syncfusion:SfDateTimeRangeNavigator>
```

### Standard DateTime Format Strings

Common format patterns:

| Pattern | Result |
|---------|--------|
| `yyyy` | 2024 |
| `yy` | 24 |
| `MMMM` | January |
| `MMM` | Jan |
| `MM` | 01 |
| `dd` | 15 |
| `ddd` | Mon |
| `dddd` | Monday |
| `HH` | 14 (24-hour) |
| `hh` | 02 (12-hour) |
| `tt` | AM/PM |
| `MMM/dd` | Jan/15 |
| `yyyy-MM-dd` | 2024-01-15 |

### Code-Behind Label Formatting

```csharp
Interval yearInterval = new Interval 
{ 
    IntervalType = NavigatorIntervalType.Year 
};
yearInterval.LabelFormatters.Add("yyyy");

Interval monthInterval = new Interval 
{ 
    IntervalType = NavigatorIntervalType.Month 
};
monthInterval.LabelFormatters.Add("MMM");
monthInterval.LabelFormatters.Add("MMMM");

rangeNavigator.Intervals.Add(yearInterval);
rangeNavigator.Intervals.Add(monthInterval);
```

## Label Style Customization

### Label Bar Style Properties

Customize label appearance using `LabelBarStyle` applied to higher or lower level bars:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
        <syncfusion:LabelBarStyle 
            Background="#E0E0E0"
            LabelHorizontalAlignment="Center"
            Position="Outside"/>
    </syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
    
    <syncfusion:SfDateTimeRangeNavigator.LowerLevelBarStyle>
        <syncfusion:LabelBarStyle 
            Background="#F5F5F5"
            LabelHorizontalAlignment="Left"
            Position="Inside"/>
    </syncfusion:SfDateTimeRangeNavigator.LowerLevelBarStyle>
    
</syncfusion:SfDateTimeRangeNavigator>
```

### SelectedLabelStyle

Apply different styling to labels within the selected range:

```xml
<syncfusion:SfDateTimeRangeNavigator.Resources>
    <Style TargetType="TextBlock" x:Key="SelectedStyle">
        <Setter Property="FontSize" Value="12"/>
        <Setter Property="FontWeight" Value="Bold"/>
        <Setter Property="Foreground" Value="DodgerBlue"/>
    </Style>
    
    <Style TargetType="TextBlock" x:Key="UnselectedStyle">
        <Setter Property="FontSize" Value="10"/>
        <Setter Property="Foreground" Value="Gray"/>
    </Style>
</syncfusion:SfDateTimeRangeNavigator.Resources>

<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
        <syncfusion:LabelBarStyle 
            Background="White"
            SelectedLabelStyle="{StaticResource SelectedStyle}"/>
    </syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
    
</syncfusion:SfDateTimeRangeNavigator>
```

### Label Positioning

Control whether labels appear inside or outside the label bars:

```xml
<syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
    <syncfusion:LabelBarStyle Position="Outside"/>
</syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>

<syncfusion:SfDateTimeRangeNavigator.LowerLevelBarStyle>
    <syncfusion:LabelBarStyle Position="Inside"/>
</syncfusion:SfDateTimeRangeNavigator.LowerLevelBarStyle>
```

**Position Options:**
- **Outside** - Labels appear outside the bar (default)
- **Inside** - Labels appear inside the bar

### Complete Label Styling Example

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.Resources>
        <!-- Selected label style -->
        <Style TargetType="TextBlock" x:Key="HigherSelectedStyle">
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="FontWeight" Value="Bold"/>
            <Setter Property="Foreground" Value="#1976D2"/>
        </Style>
        
        <Style TargetType="TextBlock" x:Key="LowerSelectedStyle">
            <Setter Property="FontSize" Value="12"/>
            <Setter Property="FontWeight" Value="SemiBold"/>
            <Setter Property="Foreground" Value="#42A5F5"/>
        </Style>
    </syncfusion:SfDateTimeRangeNavigator.Resources>
    
    <!-- Higher level bar -->
    <syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
        <syncfusion:LabelBarStyle 
            Background="#ECEFF1"
            LabelHorizontalAlignment="Center"
            Position="Outside"
            SelectedLabelStyle="{StaticResource HigherSelectedStyle}"/>
    </syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
    
    <!-- Lower level bar -->
    <syncfusion:SfDateTimeRangeNavigator.LowerLevelBarStyle>
        <syncfusion:LabelBarStyle 
            Background="#FFFFFF"
            LabelHorizontalAlignment="Center"
            Position="Outside"
            SelectedLabelStyle="{StaticResource LowerSelectedStyle}"/>
    </syncfusion:SfDateTimeRangeNavigator.LowerLevelBarStyle>
    
    <!-- Intervals with custom formats -->
    <syncfusion:SfDateTimeRangeNavigator.Intervals>
        <syncfusion:Interval IntervalType="Year">
            <syncfusion:Interval.LabelFormatters>
                <x:String>yyyy</x:String>
            </syncfusion:Interval.LabelFormatters>
        </syncfusion:Interval>
        
        <syncfusion:Interval IntervalType="Quarter">
            <syncfusion:Interval.LabelFormatters>
                <x:String>'Q'Q</x:String>
            </syncfusion:Interval.LabelFormatters>
        </syncfusion:Interval>
    </syncfusion:SfDateTimeRangeNavigator.Intervals>
    
</syncfusion:SfDateTimeRangeNavigator>
```

## Label Bar Visibility

Control the visibility of individual label bars:

### LowerLabelBarVisibility

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    LowerLabelBarVisibility="Collapsed"/>
```

### UpperLabelBarVisibility

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    UpperLabelBarVisibility="Collapsed"/>
```

**Visibility Options:**
- **Visible** - Label bar is displayed (default)
- **Collapsed** - Label bar is hidden, space is removed
- **Hidden** - Label bar is hidden, space is preserved

### Use Cases for Hidden Label Bars

**Single-level labels:**
```xml
<!-- Show only lower bar for simple time navigation -->
<syncfusion:SfDateTimeRangeNavigator 
    UpperLabelBarVisibility="Collapsed">
    <syncfusion:SfDateTimeRangeNavigator.Intervals>
        <syncfusion:Interval IntervalType="Month"/>
    </syncfusion:SfDateTimeRangeNavigator.Intervals>
</syncfusion:SfDateTimeRangeNavigator>
```

**Minimalist interface:**
```xml
<!-- No label bars, only content and scrollbar -->
<syncfusion:SfDateTimeRangeNavigator 
    UpperLabelBarVisibility="Collapsed"
    LowerLabelBarVisibility="Collapsed"
    ShowToolTip="True"/>
```

## Range Padding

Control how the navigator handles range boundaries with the `RangePadding` property.

### None (Default)

No padding is applied to the data range:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    RangePadding="None"/>
```

**Behavior:** The navigator displays exactly from the first to the last data point with no extra space.

```csharp
rangeNavigator.RangePadding = NavigatorRangePadding.None;
```

### Round

Range is rounded to the nearest interval boundary:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    RangePadding="Round"/>
```

**Behavior:** If your data spans from Jan 15 to Nov 20, with Year/Month intervals, the range extends to Jan 1 through Dec 31 for clean label boundaries.

```csharp
rangeNavigator.RangePadding = NavigatorRangePadding.Round;
```

### Visual Comparison

**None (Exact data range):**
```
Data: [Jan 15 ........................ Nov 20]
Range: [Jan 15 ...................... Nov 20]
Labels: Jan 15 | Feb | Mar | ... | Nov 20
```

**Round (Interval-aligned):**
```
Data: [Jan 15 ........................ Nov 20]
Range: [Jan 1 ........................ Dec 31]
Labels: Jan | Feb | Mar | ... | Nov | Dec
```

## GridLine and TickLine Styling

Customize gridlines and tick marks on label bars:

### GridLine Styles

```xml
<Grid.Resources>
    <Style TargetType="Line" x:Key="HigherGridStyle">
        <Setter Property="Stroke" Value="DarkGray"/>
        <Setter Property="StrokeThickness" Value="1"/>
        <Setter Property="Opacity" Value="0.6"/>
    </Style>
    
    <Style TargetType="Line" x:Key="LowerGridStyle">
        <Setter Property="Stroke" Value="LightGray"/>
        <Setter Property="StrokeThickness" Value="1"/>
        <Setter Property="StrokeDashArray" Value="2,2"/>
    </Style>
</Grid.Resources>

<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    HigherBarGridLineStyle="{StaticResource HigherGridStyle}"
    LowerBarGridLineStyle="{StaticResource LowerGridStyle}"/>
```

### TickLine Styles

```xml
<Grid.Resources>
    <Style TargetType="Line" x:Key="HigherTickStyle">
        <Setter Property="Stroke" Value="Black"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
    
    <Style TargetType="Line" x:Key="LowerTickStyle">
        <Setter Property="Stroke" Value="Gray"/>
        <Setter Property="StrokeThickness" Value="1"/>
    </Style>
</Grid.Resources>

<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    HigherBarTickLineStyle="{StaticResource HigherTickStyle}"
    LowerBarTickLineStyle="{StaticResource LowerTickStyle}"/>
```

## Complete Customization Example

```xml
<Window x:Class="RangeNavigatorDemo.CustomLabelsWindow"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    
    <Grid Margin="20">
        <syncfusion:SfDateTimeRangeNavigator 
            ItemsSource="{Binding Data}"
            XBindingPath="Date"
            Height="150"
            RangePadding="Round"
            VerticalAlignment="Bottom">
            
            <syncfusion:SfDateTimeRangeNavigator.Resources>
                <!-- Label styles -->
                <Style TargetType="TextBlock" x:Key="HigherSelected">
                    <Setter Property="FontSize" Value="14"/>
                    <Setter Property="FontWeight" Value="Bold"/>
                    <Setter Property="Foreground" Value="#FF6F00"/>
                </Style>
                
                <Style TargetType="TextBlock" x:Key="LowerSelected">
                    <Setter Property="FontSize" Value="12"/>
                    <Setter Property="FontWeight" Value="SemiBold"/>
                    <Setter Property="Foreground" Value="#FF8F00"/>
                </Style>
                
                <!-- GridLine styles -->
                <Style TargetType="Line" x:Key="GridStyle">
                    <Setter Property="Stroke" Value="#E0E0E0"/>
                    <Setter Property="StrokeThickness" Value="1"/>
                </Style>
                
                <!-- TickLine styles -->
                <Style TargetType="Line" x:Key="TickStyle">
                    <Setter Property="Stroke" Value="#9E9E9E"/>
                    <Setter Property="StrokeThickness" Value="2"/>
                </Style>
            </syncfusion:SfDateTimeRangeNavigator.Resources>
            
            <!-- Higher level bar customization -->
            <syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
                <syncfusion:LabelBarStyle 
                    Background="#FFF3E0"
                    LabelHorizontalAlignment="Center"
                    Position="Outside"
                    SelectedLabelStyle="{StaticResource HigherSelected}"/>
            </syncfusion:SfDateTimeRangeNavigator.HigherLevelBarStyle>
            
            <!-- Lower level bar customization -->
            <syncfusion:SfDateTimeRangeNavigator.LowerLevelBarStyle>
                <syncfusion:LabelBarStyle 
                    Background="#FFFFFF"
                    LabelHorizontalAlignment="Center"
                    Position="Outside"
                    SelectedLabelStyle="{StaticResource LowerSelected}"/>
            </syncfusion:SfDateTimeRangeNavigator.LowerLevelBarStyle>
            
            <!-- Intervals with custom formatters -->
            <syncfusion:SfDateTimeRangeNavigator.Intervals>
                <syncfusion:Interval IntervalType="Quarter">
                    <syncfusion:Interval.LabelFormatters>
                        <x:String>'Q'Q yyyy</x:String>
                        <x:String>'Q'Q</x:String>
                    </syncfusion:Interval.LabelFormatters>
                </syncfusion:Interval>
                
                <syncfusion:Interval IntervalType="Month">
                    <syncfusion:Interval.LabelFormatters>
                        <x:String>MMMM</x:String>
                        <x:String>MMM</x:String>
                    </syncfusion:Interval.LabelFormatters>
                </syncfusion:Interval>
            </syncfusion:SfDateTimeRangeNavigator.Intervals>
            
            <!-- Apply line styles -->
            <syncfusion:SfDateTimeRangeNavigator.HigherBarGridLineStyle>
                <Binding Source="{StaticResource GridStyle}"/>
            </syncfusion:SfDateTimeRangeNavigator.HigherBarGridLineStyle>
            
            <syncfusion:SfDateTimeRangeNavigator.LowerBarGridLineStyle>
                <Binding Source="{StaticResource GridStyle}"/>
            </syncfusion:SfDateTimeRangeNavigator.LowerBarGridLineStyle>
            
            <syncfusion:SfDateTimeRangeNavigator.HigherBarTickLineStyle>
                <Binding Source="{StaticResource TickStyle}"/>
            </syncfusion:SfDateTimeRangeNavigator.HigherBarTickLineStyle>
            
            <syncfusion:SfDateTimeRangeNavigator.LowerBarTickLineStyle>
                <Binding Source="{StaticResource TickStyle}"/>
            </syncfusion:SfDateTimeRangeNavigator.LowerBarTickLineStyle>
            
            <!-- Content -->
            <syncfusion:SfDateTimeRangeNavigator.Content>
                <syncfusion:SfLineSparkline 
                    ItemsSource="{Binding Data}"
                    YBindingPath="Value"/>
            </syncfusion:SfDateTimeRangeNavigator.Content>
            
        </syncfusion:SfDateTimeRangeNavigator>
    </Grid>
</Window>
```

## Summary

You've learned how to:
- ✓ Configure interval types (Year, Quarter, Month, Week, Day, Hour)
- ✓ Create custom interval combinations for different data types
- ✓ Use automatic label formatting with smart zoom behavior
- ✓ Apply custom label formatters with DateTime format strings
- ✓ Style selected and unselected labels differently
- ✓ Position labels inside or outside bars
- ✓ Control label bar visibility
- ✓ Configure range padding (None vs Round)
- ✓ Customize gridlines and tick marks
- ✓ Build completely customized label bars

Next: Explore [tooltip-support.md](tooltip-support.md) for tooltip configuration and templates.
