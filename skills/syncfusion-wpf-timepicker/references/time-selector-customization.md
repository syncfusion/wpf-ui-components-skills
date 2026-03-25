# Time Selector Customization in SfTimePicker

## Table of Contents
- [Overview](#overview)
- [Cell Templates](#cell-templates)
- [Cell Sizing](#cell-sizing)
- [Item Spacing](#item-spacing)
- [Selector Styling](#selector-styling)
- [Common Patterns](#common-patterns)

---

## Overview

The **SfTimeSelector** is the dropdown control that appears when the time picker is expanded. It contains three main selectable components:

1. **Hour cells** - Display available hours (1-12 or 0-23)
2. **Minute cells** - Display available minutes (0-59)
3. **Meridiem cells** - Display AM/PM (only in 12-hour mode)

Each component has its own cell template that you can customize independently.

## Cell Templates

### Hour Cell Template

Customize the hour selector appearance with **HourCellTemplate**:

```xaml
<syncfusion:SfTimePicker DropDownHeight="380" 
                         SelectorItemHeight="70" 
                         SelectorItemWidth="70" 
                         Width="200"
                         Name="sfTimePicker">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="HourCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Grid>
                            <!-- Clock icon -->
                            <TextBlock VerticalAlignment="Top" 
                                       HorizontalAlignment="Right"
                                       Margin="5"
                                       FontSize="22"
                                       FontFamily="Segoe UI Symbol"
                                       Text="&#xE170;"/>
                            <!-- Hour value -->
                            <TextBlock Text="{Binding HourNumber}" 
                                       VerticalAlignment="Bottom" 
                                       Margin="5"
                                       FontSize="22"/>
                        </Grid>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

**Template Context:** The DataContext is `DateTimeWrapper` with property `HourNumber`

### Minute Cell Template

Customize the minute selector appearance with **MinuteCellTemplate**:

```xaml
<syncfusion:SfTimePicker DropDownHeight="380" 
                         SelectorItemHeight="70" 
                         SelectorItemWidth="70"
                         Width="200"
                         Name="sfTimePicker">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="MinuteCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Grid>
                            <!-- Clock hand icon -->
                            <TextBlock VerticalAlignment="Top" 
                                       HorizontalAlignment="Right"
                                       Margin="5"
                                       FontSize="22"
                                       FontFamily="Segoe UI Symbol"
                                       Text="&#xE170;"/>
                            <!-- Minute value -->
                            <TextBlock Text="{Binding MinuteNumber}" 
                                       VerticalAlignment="Bottom" 
                                       Margin="5"
                                       FontSize="22"/>
                        </Grid>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

**Template Context:** The DataContext is `DateTimeWrapper` with property `MinuteNumber`

### Meridiem Cell Template

Customize the AM/PM selector appearance with **MeridiemCellTemplate**:

```xaml
<syncfusion:SfTimePicker DropDownHeight="380" 
                         SelectorItemHeight="70" 
                         SelectorItemWidth="70"
                         Width="200"
                         Name="sfTimePicker">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="MeridiemCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Grid>
                            <!-- Sun/Moon icon -->
                            <TextBlock VerticalAlignment="Top" 
                                       HorizontalAlignment="Right"
                                       Margin="5"
                                       FontSize="22"
                                       FontFamily="Segoe UI Symbol"
                                       Text="&#xE170;"/>
                            <!-- AM/PM text -->
                            <TextBlock Text="{Binding AmPmString}" 
                                       VerticalAlignment="Bottom" 
                                       Margin="5"
                                       FontSize="22"/>
                        </Grid>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

**Template Context:** The DataContext is `DateTimeWrapper` with property `AmPmString`

### DateTimeWrapper Properties

When using cell templates, the DataContext provides:
- `HourNumber` - Current hour value
- `MinuteNumber` - Current minute value
- `AmPmString` - AM or PM string
- `SecondNumber` - Current second value (if seconds enabled)

## Cell Sizing

Control the dimensions of selector cells using **SelectorItemWidth** and **SelectorItemHeight**:

### Default Size (30x30 pixels)

```xaml
<!-- Compact view -->
<syncfusion:SfTimePicker Name="sfTimePicker"/>
```

### Custom Size

```xaml
<!-- Large, touch-friendly cells -->
<syncfusion:SfTimePicker SelectorItemWidth="60" 
                         SelectorItemHeight="60" 
                         Name="sfTimePicker"/>
```

```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.SelectorItemWidth = 60;
timePicker.SelectorItemHeight = 60;
```

### Responsive Sizing Example

```xaml
<!-- Tablet-friendly size -->
<syncfusion:SfTimePicker SelectorItemWidth="80"
                         SelectorItemHeight="80"
                         DropDownHeight="500"
                         Name="timePicker"/>
```

## Item Spacing

Control the space between hour, minute, and meridiem selectors using **SelectorItemSpacing**:

### Default Spacing (4 pixels)

```xaml
<!-- Default spacing between cells -->
<syncfusion:SfTimePicker Name="sfTimePicker"/>
```

### Custom Spacing

```xaml
<!-- Wide spacing for clarity -->
<syncfusion:SfTimePicker SelectorItemSpacing="20" 
                         Name="sfTimePicker"/>
```

```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.SelectorItemSpacing = 20;
```

### Compact vs. Spacious Layout

```xaml
<!-- Compact layout -->
<syncfusion:SfTimePicker SelectorItemWidth="30"
                         SelectorItemHeight="30"
                         SelectorItemSpacing="2"
                         Name="compact"/>

<!-- Spacious layout -->
<syncfusion:SfTimePicker SelectorItemWidth="60"
                         SelectorItemHeight="60"
                         SelectorItemSpacing="30"
                         Name="spacious"/>
```

## Selector Styling

Apply custom styles to the entire time selector using **SelectorStyle**:

### Basic Selector Style

```xaml
<syncfusion:SfTimePicker Name="sfTimePicker">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <!-- Custom selector styling -->
            <Setter Property="Background" Value="LightGray"/>
            <Setter Property="Foreground" Value="DarkBlue"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

### Selector Colors

```xaml
<syncfusion:SfTimePicker Name="sfTimePicker">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="Foreground" Value="Blue"/>
            <Setter Property="SelectedForeground" Value="Yellow"/>
            <Setter Property="Background" Value="White"/>
            <Setter Property="AccentBrush" Value="Green"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

**Selector Color Properties:**
- `Foreground` - Color of unselected items
- `SelectedForeground` - Color of selected/highlighted items
- `Background` - Dropdown background color
- `AccentBrush` - Highlight color for selected item

## Common Patterns

### Pattern 1: Touch-Friendly Large Cells

```xaml
<syncfusion:SfTimePicker SelectorItemWidth="70"
                         SelectorItemHeight="70"
                         SelectorItemSpacing="15"
                         DropDownHeight="400"
                         Name="touchFriendly">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="Foreground" Value="DarkGray"/>
            <Setter Property="SelectedForeground" Value="White"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

### Pattern 2: Compact Desktop View

```xaml
<syncfusion:SfTimePicker SelectorItemWidth="35"
                         SelectorItemHeight="35"
                         SelectorItemSpacing="3"
                         DropDownHeight="250"
                         Name="compact">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="Background" Value="WhiteSmoke"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

### Pattern 3: Dark Theme Selector

```xaml
<syncfusion:SfTimePicker Name="darkTheme">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="Background" Value="#333333"/>
            <Setter Property="Foreground" Value="#CCCCCC"/>
            <Setter Property="SelectedForeground" Value="#FFFF00"/>
            <Setter Property="AccentBrush" Value="#0066FF"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

### Pattern 4: Icon-Enhanced Cells

```xaml
<syncfusion:SfTimePicker SelectorItemWidth="60"
                         SelectorItemHeight="60"
                         Name="iconEnhanced">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="HourCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <StackPanel VerticalAlignment="Center" 
                                    HorizontalAlignment="Center">
                            <TextBlock Text="🕐" FontSize="24" TextAlignment="Center"/>
                            <TextBlock Text="{Binding HourNumber}" 
                                       FontSize="16" 
                                       TextAlignment="Center"/>
                        </StackPanel>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
            <Setter Property="MinuteCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <StackPanel VerticalAlignment="Center"
                                    HorizontalAlignment="Center">
                            <TextBlock Text="⏱" FontSize="24" TextAlignment="Center"/>
                            <TextBlock Text="{Binding MinuteNumber}" 
                                       FontSize="16" 
                                       TextAlignment="Center"/>
                        </StackPanel>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

## Troubleshooting

### Issue: Cell Templates Not Appearing

**Solution:** Ensure the SelectorStyle is properly nested within SfTimePicker and target type is `syncfusion:SfTimeSelector`

### Issue: Spacing Too Large/Small

**Solution:** Adjust SelectorItemSpacing. Default is 4 pixels; increase for more space between selectors.

### Issue: Custom Templates Break Layout

**Solution:** Ensure your DataTemplate contains appropriate sizing (VerticalAlignment/HorizontalAlignment) to fill the cell dimensions.

## Next Steps

- Learn **[Appearance & Styling](appearance-and-styling.md)** for overall control styling
- Explore **[Time Formatting](time-formatting.md)** to control selector format strings
