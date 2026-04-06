# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Spin Animation](#spin-animation)
- [Accent Brush](#accent-brush)
- [Customizing Up/Down Button Style](#customizing-updown-button-style)
- [Complete Custom Style Example](#complete-custom-style-example)
- [Color Customization](#color-customization)
- [Font and Text Styling](#font-and-text-styling)
- [Border and Layout](#border-and-layout)
- [Best Practices](#best-practices)

## Overview

The `SfDomainUpDown` control offers extensive styling and appearance customization options. You can control animations, colors, button styles, and apply complete custom themes to match your application's design language.

**Customization Levels:**
1. **Simple:** `AccentBrush`, `EnableSpinAnimation`, standard properties
2. **Intermediate:** `UpDownStyle` for button customization
3. **Advanced:** Complete control templates and resource dictionaries

## Spin Animation

The `EnableSpinAnimation` property controls whether items transition smoothly when cycling through values.

### Property Details

**Type:** `bool`  
**Default:** `true`

### When Enabled (true)
- Items slide smoothly during transitions
- Visual feedback enhances user experience
- May have minor performance impact with complex templates

### When Disabled (false)
- Instant value changes
- Better performance with complex content
- Simpler visual behavior

### XAML Example

```xml
<syncfusion:SfDomainUpDown x:Name="domain"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"
                           Width="200"
                           Height="35"
                           EnableSpinAnimation="True"
                           ItemsSource="{Binding Items}"/>
```

### C# Example

```csharp
SfDomainUpDown domainUpDown = new SfDomainUpDown();
domainUpDown.EnableSpinAnimation = false; // Disable for instant changes
```

### Use Cases

**Enable animation when:**
- User experience is priority
- Content is simple text
- Smooth transitions add polish

**Disable animation when:**
- Performance is critical
- Content templates are complex
- Accessibility requires instant feedback

## Accent Brush

The `AccentBrush` property decorates the control's hot spots with a solid color, providing quick visual theming.

### Property Details

**Type:** `Brush`  
**Default:** System accent color

**Affects:**
- Spin button hover states
- Focus indicators
- Border highlights

### XAML Example

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"
                           Width="200"
                           Height="35"
                           AccentBrush="SteelBlue"
                           Value="James"
                           ItemsSource="{Binding Employees}"/>
```

### C# Example

```csharp
domainUpDown.AccentBrush = new SolidColorBrush(Colors.SteelBlue);
```

### Color Examples

```xml
<!-- Predefined colors -->
<syncfusion:SfDomainUpDown AccentBrush="Black"/>
<syncfusion:SfDomainUpDown AccentBrush="CornflowerBlue"/>
<syncfusion:SfDomainUpDown AccentBrush="#FF5733"/>

<!-- Gradient brush -->
<syncfusion:SfDomainUpDown>
    <syncfusion:SfDomainUpDown.AccentBrush>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Blue" Offset="0"/>
            <GradientStop Color="Purple" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:SfDomainUpDown.AccentBrush>
</syncfusion:SfDomainUpDown>
```

## Customizing Up/Down Button Style

The `UpDownStyle` property allows comprehensive customization of the spin buttons' appearance through control templates.

### Basic Button Style

```xml
<Window xmlns:editors="clr-namespace:Syncfusion.Windows.Controls;assembly=Syncfusion.SfInput.WPF"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    
    <Window.Resources>
        <Style x:Key="CustomUpDownButtonStyle" TargetType="editors:SfUpDown">
            <Setter Property="BorderBrush" Value="Black"/>
            <Setter Property="FontWeight" Value="Bold"/>
            <Setter Property="Background" Value="LightGray"/>
        </Style>
    </Window.Resources>
    
    <Grid>
        <syncfusion:SfDomainUpDown UpDownStyle="{StaticResource CustomUpDownButtonStyle}"/>
    </Grid>
</Window>
```

### Complete Button Template

For full control over button appearance and layout:

```xml
<Window xmlns:editors="clr-namespace:Syncfusion.Windows.Controls;assembly=Syncfusion.SfInput.WPF"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    
    <Window.Resources>
        <Style x:Key="CustomUpDownButtonStyle" TargetType="editors:SfUpDown">
            <Setter Property="BorderBrush" Value="Black"/>
            <Setter Property="FontWeight" Value="Bold"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="editors:SfUpDown">
                        <Border BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="1"
                                CornerRadius="2">
                            <Grid x:Name="PART_OuterGrid">
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="*" />
                                    <ColumnDefinition Width="Auto" />
                                    <ColumnDefinition Width="Auto" />
                                </Grid.ColumnDefinitions>
                                
                                <!-- Content Area -->
                                <ContentControl x:Name="PART_Content" 
                                              Grid.Column="0"
                                              IsTabStop="False" 
                                              Focusable="False" 
                                              Content="{TemplateBinding UpDownContent}"/>
                                
                                <!-- Down Button -->
                                <Button x:Name="PART_DownButton"
                                       Grid.Column="1"
                                       HorizontalAlignment="Stretch"
                                       VerticalAlignment="Stretch"
                                       Background="{TemplateBinding AccentBrush}"
                                       BorderBrush="{TemplateBinding BorderBrush}"
                                       Foreground="{TemplateBinding Foreground}"
                                       BorderThickness="1"
                                       Padding="5"
                                       FontSize="20"
                                       Command="{TemplateBinding DownCommand}"
                                       IsTabStop="False"
                                       Content="-"/>
                                
                                <!-- Up Button -->
                                <Button x:Name="PART_UpButton"
                                       Grid.Column="2"
                                       HorizontalAlignment="Stretch"
                                       VerticalAlignment="Stretch"
                                       Background="{TemplateBinding AccentBrush}"
                                       BorderBrush="{TemplateBinding BorderBrush}"
                                       Foreground="{TemplateBinding Foreground}"
                                       BorderThickness="1"
                                       Padding="5"
                                       FontSize="20"
                                       Command="{TemplateBinding UpCommand}"
                                       Content="+"
                                       IsTabStop="False"/>
                            </Grid>
                        </Border>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Window.Resources>
    
    <Grid>
        <syncfusion:SfDomainUpDown UpDownStyle="{StaticResource CustomUpDownButtonStyle}"/>
    </Grid>
</Window>
```

### Important Template Parts

When creating custom templates, preserve these named parts:

- `PART_OuterGrid` - Main layout container
- `PART_Content` - Content display area
- `PART_UpButton` - Increment button
- `PART_DownButton` - Decrement button

## Complete Custom Style Example

Combine multiple styling options to create a polished control:

```xml
<!-- Define custom button and domain-updown styles in Window.Resources -->
<Style x:Key="ModernUpDownButtonStyle" TargetType="editors:SfUpDown">
    <Setter Property="BorderBrush" Value="DarkSlateGray"/>
    <Setter Property="Template">
        <!-- Define template with PART_OuterGrid, PART_Content, PART_UpButton, PART_DownButton -->
    </Setter>
</Style>

<Style x:Key="ModernDomainUpDownStyle" TargetType="syncfusion:SfDomainUpDown">
    <Setter Property="AccentBrush" Value="#4A90E2"/>
    <Setter Property="UpDownStyle" Value="{StaticResource ModernUpDownButtonStyle}"/>
    <!-- Add other properties: FontSize, BorderThickness, Padding, etc. -->
</Style>

<!-- Use the custom style -->
<syncfusion:SfDomainUpDown Style="{StaticResource ModernDomainUpDownStyle}" ItemsSource="{Binding Items}"/>
```

For full template details, refer to the "Customizing Up/Down Button Style" section above.

## Color Customization

### Standard Properties

```xml
<syncfusion:SfDomainUpDown Foreground="White"
                           Background="DarkBlue"
                           BorderBrush="LightBlue"
                           BorderThickness="2"/>
```

### Theme Colors

```xml
<syncfusion:SfDomainUpDown Foreground="{DynamicResource {x:Static SystemColors.ControlTextBrushKey}}"
                           Background="{DynamicResource {x:Static SystemColors.WindowBrushKey}}"/>
```

## Font and Text Styling

```xml
<syncfusion:SfDomainUpDown FontFamily="Arial"
                           FontSize="16"
                           FontWeight="Bold"
                           FontStyle="Italic">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}" 
                      TextAlignment="Center"
                      TextWrapping="Wrap"/>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

## Border and Layout

```xml
<syncfusion:SfDomainUpDown BorderThickness="2"
                           BorderBrush="Black"
                           Padding="10"
                           Margin="20"
                           CornerRadius="5"
                           HorizontalContentAlignment="Left"
                           VerticalContentAlignment="Center"/>
```

## Best Practices

### 1. Maintain Visual Consistency
- Use consistent accent colors across your application
- Match button styles with other input controls
- Follow your application's design system

### 2. Ensure Accessibility
- Maintain sufficient color contrast (WCAG 2.1 guidelines)
- Don't rely solely on color to convey information
- Test with high contrast themes

### 3. Performance Considerations
- Disable animations if using complex content templates
- Avoid overly complex control templates
- Test performance with data binding

### 4. Responsive Design
- Define appropriate min/max widths
- Test appearance at different resolutions
- Consider scaling for high-DPI displays

### 5. Theme Integration
- Use `DynamicResource` for colors that should follow system themes
- Provide both light and dark theme variations
- Test with Windows theme changes

## Common Styling Patterns

### Pattern 1: Minimal Modern Style

```xml
<Style TargetType="syncfusion:SfDomainUpDown">
    <Setter Property="Background" Value="White"/>
    <Setter Property="Foreground" Value="#333"/>
    <Setter Property="BorderBrush" Value="#DDD"/>
    <Setter Property="BorderThickness" Value="1"/>
    <Setter Property="AccentBrush" Value="#007ACC"/>
    <Setter Property="FontSize" Value="13"/>
    <Setter Property="Padding" Value="8,4"/>
</Style>
```

### Pattern 2: Material Design Inspired

```xml
<Style TargetType="syncfusion:SfDomainUpDown">
    <Setter Property="Background" Value="Transparent"/>
    <Setter Property="BorderThickness" Value="0,0,0,2"/>
    <Setter Property="BorderBrush" Value="#9E9E9E"/>
    <Setter Property="AccentBrush" Value="#2196F3"/>
    <Setter Property="FontFamily" Value="Roboto"/>
    <Setter Property="FontSize" Value="14"/>
</Style>
```

### Pattern 3: High Contrast

```xml
<Style TargetType="syncfusion:SfDomainUpDown">
    <Setter Property="Background" Value="Black"/>
    <Setter Property="Foreground" Value="White"/>
    <Setter Property="BorderBrush" Value="Yellow"/>
    <Setter Property="BorderThickness" Value="2"/>
    <Setter Property="AccentBrush" Value="Cyan"/>
    <Setter Property="FontWeight" Value="Bold"/>
</Style>
```

## Troubleshooting

**Issue:** Custom style not applying  
**Solution:** Ensure the style is defined before the control and properly referenced with `Style="{StaticResource ...}"`

**Issue:** Buttons appear distorted  
**Solution:** Check template part names match exactly (PART_UpButton, PART_DownButton, etc.)

**Issue:** Colors don't update with theme changes  
**Solution:** Use `DynamicResource` instead of `StaticResource` for theme-aware colors

**Issue:** Animation stutters  
**Solution:** Simplify ContentTemplate or disable EnableSpinAnimation

**Issue:** Text not visible  
**Solution:** Check Foreground/Background contrast; verify FontSize is appropriate
