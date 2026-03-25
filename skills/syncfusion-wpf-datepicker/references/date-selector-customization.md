# Date Selector Customization in SfDatePicker

## Table of Contents
- [SfDateSelector Overview](#sfdateselector-overview)
- [Cell Templates](#cell-templates)
- [Customizing Day Cell Template](#customizing-day-cell-template)
- [Customizing Month Cell Template](#customizing-month-cell-template)
- [Customizing Year Cell Template](#customizing-year-cell-template)
- [Adjusting Cell Size](#adjusting-cell-size)
- [Adjusting Cell Spacing](#adjusting-cell-spacing)
- [Style-Based Customization](#style-based-customization)
- [Complete Customization Example](#complete-customization-example)

## SfDateSelector Overview

The **SfDateSelector** is the dropdown component that appears when you click the dropdown button on the SfDatePicker. It provides a visual interface for selecting day, month, and year values through interactive cells.

### Structure
The SfDateSelector contains three main selection areas:
- **Day Selector** – Cells for selecting day (1-31)
- **Month Selector** – Cells for selecting month (1-12)
- **Year Selector** – Cells for selecting year

### Visual Layout
```
┌─────────────────────────┐
│ Day  │  Month  │  Year  │  ← Three columns
├─────────────────────────┤
│ 1    │ 1 (Jan) │ 2023   │
│ 2    │ 2 (Feb) │ 2024   │
│ ... (scrollable list)   │
│ 31   │ 12(Dec) │ 2027   │
└─────────────────────────┘
```

### DataContext
The DataContext for each cell is a **DateTimeWrapper** object, which provides:
- `DayNumber` – The numeric day value (1-31)
- `MonthNumber` – The numeric month value (1-12)
- `YearNumber` – The numeric year value (e.g., 2026)

## Cell Templates

Cell templates allow you to customize the appearance and content of individual selector cells. You can add icons, images, text, or any custom visual.

### Available Template Properties
The SfDateSelector provides three template properties:
- **DayCellTemplate** – Customizes day selector cells
- **MonthCellTemplate** – Customizes month selector cells
- **YearCellTemplate** – Customizes year selector cells

All templates are applied through the **SelectorStyle** property of SfDatePicker.

## Customizing Day Cell Template

Add custom content (icons, images, styling) to the day cells in the dropdown selector.

### Basic Day Cell Template Example

```xaml
<syncfusion:SfDatePicker VerticalAlignment="Center"
                         HorizontalAlignment="Center"
                         Width="200"
                         x:Name="sfDatePicker">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="DayCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Grid>
                            <!-- Icon at top-right -->
                            <TextBlock VerticalAlignment="Top" 
                                       HorizontalAlignment="Right"
                                       Margin="5"
                                       FontSize="14"
                                       FontFamily="Segoe UI Symbol"
                                       Text="📅"/>

                            <!-- Day number at bottom -->
                            <TextBlock Text="{Binding DayNumber}" 
                                       VerticalAlignment="Bottom" 
                                       HorizontalAlignment="Left"
                                       Margin="5"
                                       FontSize="18"
                                       FontWeight="Bold"/>
                        </Grid>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

### Day Cell Template with Styling

```xaml
<syncfusion:SfDatePicker x:Name="sfDatePicker" Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="DayCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Border BorderThickness="1" 
                                BorderBrush="LightGray"
                                CornerRadius="4"
                                Background="AliceBlue"
                                Padding="8">
                            <TextBlock Text="{Binding DayNumber}"
                                       FontSize="16"
                                       Foreground="DarkBlue"
                                       HorizontalAlignment="Center"
                                       VerticalAlignment="Center"/>
                        </Border>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

## Customizing Month Cell Template

Add custom content to the month cells (e.g., month names, abbreviations, icons).

### Month Cell Template with Icons

```xaml
<syncfusion:SfDatePicker x:Name="sfDatePicker" Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="MonthCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Grid>
                            <!-- Icon at top -->
                            <TextBlock VerticalAlignment="Top" 
                                       HorizontalAlignment="Right"
                                       Margin="5"
                                       FontSize="16"
                                       FontFamily="Segoe UI Symbol"
                                       Text="📆"/>

                            <!-- Month number at bottom -->
                            <TextBlock Text="{Binding MonthNumber}" 
                                       VerticalAlignment="Bottom" 
                                       Margin="5"
                                       FontSize="16"
                                       FontWeight="Bold"/>
                        </Grid>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

### Month Cell Template with Colored Background

```xaml
<syncfusion:SfDatePicker x:Name="sfDatePicker" Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="MonthCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Border BorderThickness="1" 
                                BorderBrush="Gray"
                                Background="LightYellow"
                                Padding="8">
                            <TextBlock Text="{Binding MonthNumber}"
                                       FontSize="14"
                                       Foreground="DarkRed"
                                       HorizontalAlignment="Center"
                                       VerticalAlignment="Center"/>
                        </Border>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

## Customizing Year Cell Template

Customize year cells for special styling or indicator elements.

### Year Cell Template Example

```xaml
<syncfusion:SfDatePicker x:Name="sfDatePicker" Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="YearCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Grid>
                            <!-- Icon -->
                            <TextBlock VerticalAlignment="Top" 
                                       HorizontalAlignment="Right"
                                       Margin="5"
                                       FontSize="16"
                                       FontFamily="Segoe UI Symbol"
                                       Text="📅"/>

                            <!-- Year number -->
                            <TextBlock Text="{Binding YearNumber}" 
                                       VerticalAlignment="Bottom" 
                                       Margin="5"
                                       FontSize="16"
                                       FontWeight="Bold"/>
                        </Grid>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

## Adjusting Cell Size

Control the dimensions of selector cells using **SelectorItemWidth** and **SelectorItemHeight** properties.

### Default Values
- **SelectorItemWidth:** 80 pixels
- **SelectorItemHeight:** 70 pixels

### Via XAML – Larger Cells

```xaml
<syncfusion:SfDatePicker SelectorItemWidth="120" 
                         SelectorItemHeight="100" 
                         x:Name="sfDatePicker"/>
```

### Via C# – Smaller Cells

```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.SelectorItemWidth = 60;
sfDatePicker.SelectorItemHeight = 50;
```

### Practical Example – Mobile-Friendly Large Cells

```xaml
<syncfusion:SfDatePicker SelectorItemWidth="150" 
                         SelectorItemHeight="120" 
                         x:Name="sfDatePicker">
    <!-- Large touch-friendly cells for mobile/tablet -->
</syncfusion:SfDatePicker>
```

### Size Impact on User Experience
| SelectorItemWidth | SelectorItemHeight | Use Case |
|-------------------|------------------|----------|
| 60 | 50 | Desktop, compact UI |
| 80 | 70 | Default, balanced |
| 120 | 100 | Larger screen, more readable |
| 150+ | 120+ | Tablet/mobile, touch-friendly |

## Adjusting Cell Spacing

Control the space between day, month, and year selector columns using the **SelectorItemSpacing** property.

### Default Value
The default value is **4 pixels**.

### Via XAML – Increased Spacing

```xaml
<syncfusion:SfDatePicker SelectorItemSpacing="20" 
                         x:Name="sfDatePicker"/>
```

### Via C# – Minimal Spacing

```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.SelectorItemSpacing = 2;
```

### Spacing Scenarios
| SelectorItemSpacing | Visual Effect | Use Case |
|------------------|--------------|----------|
| 2 | Compact, minimal gaps | Tight layouts |
| 4 | Default, balanced | Standard UI |
| 10 | More breathing room | Enhanced readability |
| 20+ | Significant separation | Emphasize columns |

### Example – Compact Date Selector

```xaml
<syncfusion:SfDatePicker SelectorItemWidth="60"
                         SelectorItemHeight="50"
                         SelectorItemSpacing="2"
                         x:Name="sfDatePicker">
    <!-- Compact, space-efficient selector -->
</syncfusion:SfDatePicker>
```

### Example – Spacious Date Selector

```xaml
<syncfusion:SfDatePicker SelectorItemWidth="150"
                         SelectorItemHeight="120"
                         SelectorItemSpacing="25"
                         x:Name="sfDatePicker">
    <!-- Large, touch-friendly with clear separation -->
</syncfusion:SfDatePicker>
```

## Style-Based Customization

Customize the entire SfDateSelector appearance using the **SelectorStyle** property, which accepts a WPF Style targeting SfDateSelector.

### Common Style Customizations

**Property:** SelectorStyle (Type: Style)

```xaml
<syncfusion:SfDatePicker x:Name="sfDatePicker">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <!-- Customize DateSelector properties here -->
            <Setter Property="Property" Value="Value"/>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

### Customizable SfDateSelector Properties

Within SelectorStyle, you can set:
- `DayCellTemplate` – Day cell appearance
- `MonthCellTemplate` – Month cell appearance
- `YearCellTemplate` – Year cell appearance
- `Foreground` – Text color
- `SelectedForeground` – Selected item text color
- `Background` – Cell background

## Complete Customization Example

Here's a comprehensive example combining multiple customizations:

```xaml
<syncfusion:SfDatePicker VerticalAlignment="Center"
                         HorizontalAlignment="Center"
                         Width="300"
                         SelectorItemWidth="100"
                         SelectorItemHeight="80"
                         SelectorItemSpacing="10"
                         x:Name="sfDatePicker">
    
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <!-- Overall selector styling -->
            <Setter Property="Foreground" Value="DarkBlue"/>
            <Setter Property="SelectedForeground" Value="White"/>
            <Setter Property="Background" Value="White"/>
            
            <!-- Day cell customization -->
            <Setter Property="DayCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Border BorderThickness="1" 
                                BorderBrush="LightGray"
                                Background="WhiteSmoke"
                                CornerRadius="4"
                                Padding="8">
                            <StackPanel Orientation="Vertical"
                                       HorizontalAlignment="Center">
                                <TextBlock Text="Day" 
                                          FontSize="10" 
                                          Foreground="Gray"/>
                                <TextBlock Text="{Binding DayNumber}"
                                          FontSize="18"
                                          FontWeight="Bold"
                                          Foreground="DarkBlue"/>
                            </StackPanel>
                        </Border>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
            
            <!-- Month cell customization -->
            <Setter Property="MonthCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Border BorderThickness="1" 
                                BorderBrush="LightGray"
                                Background="LightYellow"
                                CornerRadius="4"
                                Padding="8">
                            <TextBlock Text="{Binding MonthNumber}"
                                      FontSize="18"
                                      FontWeight="Bold"
                                      HorizontalAlignment="Center"/>
                        </Border>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
            
            <!-- Year cell customization -->
            <Setter Property="YearCellTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Border BorderThickness="1" 
                                BorderBrush="LightGray"
                                Background="LightBlue"
                                CornerRadius="4"
                                Padding="8">
                            <TextBlock Text="{Binding YearNumber}"
                                      FontSize="18"
                                      FontWeight="Bold"
                                      HorizontalAlignment="Center"/>
                        </Border>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

## Best Practices

1. **Keep templates simple** – Complex templates can impact performance
2. **Test on touch devices** – Ensure cell sizes are touch-friendly (minimum 48px recommended)
3. **Maintain visual hierarchy** – Use contrast and sizing to guide user attention
4. **Use consistent styling** – Match cell templates across day, month, and year
5. **Consider accessibility** – Use sufficient color contrast and readable font sizes
6. **Document custom templates** – Comment complex binding logic for maintainability
