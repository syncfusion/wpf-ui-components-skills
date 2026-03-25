# Appearance and Styling in SfDatePicker

## Table of Contents
- [Foreground Color](#foreground-color)
- [Background Color](#background-color)
- [Accent Brush](#accent-brush)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Theme Support](#theme-support)
- [Complete Styling Example](#complete-styling-example)

## Foreground Color

The **Foreground** property controls the text color of the SfDatePicker control itself and the items in the date selector.

### Setting Foreground on Control

Change the text color displayed in the main DatePicker textbox:

#### Via XAML
```xaml
<syncfusion:SfDatePicker Name="sfDatePicker"
                         Foreground="Red"
                         Width="200"/>
```

#### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.Foreground = new SolidColorBrush(Colors.Red);
```

### Setting Foreground on Selector Items

Change the text color of items displayed in the dropdown date selector. This is done through the SelectorStyle:

#### Custom Foreground for Selector Items
```xaml
<syncfusion:SfDatePicker Name="sfDatePicker" Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="Foreground" Value="Blue"/>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

### Setting SelectedForeground

Change the text color of the **selected/highlighted item** in the date selector:

```xaml
<syncfusion:SfDatePicker Name="sfDatePicker" Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="Foreground" Value="Blue"/>
            <Setter Property="SelectedForeground" Value="Yellow"/>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

### Practical Example: High-Contrast Text

```xaml
<syncfusion:SfDatePicker Name="sfDatePicker"
                         Foreground="DarkBlue"
                         Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="Foreground" Value="DarkBlue"/>
            <Setter Property="SelectedForeground" Value="White"/>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

## Background Color

The **Background** property controls the background color of the control and selector items.

### Setting Background on Control

Change the background color of the main SfDatePicker textbox area:

#### Via XAML
```xaml
<syncfusion:SfDatePicker Name="sfDatePicker"
                         Background="LightGray"
                         Width="200"/>
```

#### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.Background = new SolidColorBrush(Colors.LightGray);
```

### Setting Background on Selector

Change the background of the date selector dropdown popup:

```xaml
<syncfusion:SfDatePicker Name="sfDatePicker"
                         Background="LightGray"
                         Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="Background" Value="Blue"/>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

### Practical Example: Dark Theme
```xaml
<syncfusion:SfDatePicker Name="sfDatePicker"
                         Background="DarkGray"
                         Foreground="White"
                         Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="Background" Value="DarkSlateGray"/>
            <Setter Property="Foreground" Value="White"/>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

## Accent Brush

The **AccentBrush** property controls the color of the **selected item** in the date selector dropdown (the highlighted/active cell).

### Purpose
- Highlights which cell is currently selected
- Provides visual feedback to the user
- Defaults to blue accent color

### Via XAML
```xaml
<syncfusion:SfDatePicker Name="sfDatePicker"
                         AccentBrush="Green"
                         Width="200"/>
```

### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.AccentBrush = new SolidColorBrush(Colors.Green);
```

### Comprehensive Color Customization

Combine all three color properties for complete control:

```xaml
<syncfusion:SfDatePicker Name="sfDatePicker"
                         Background="White"
                         Foreground="Blue"
                         AccentBrush="Green"
                         Width="200">
    <syncfusion:SfDatePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfDateSelector">
            <Setter Property="Background" Value="AliceBlue"/>
            <Setter Property="Foreground" Value="Blue"/>
            <Setter Property="SelectedForeground" Value="White"/>
        </Style>
    </syncfusion:SfDatePicker.SelectorStyle>
</syncfusion:SfDatePicker>
```

### Color Combination Examples

**Example 1: Professional Blue Theme**
```xaml
<syncfusion:SfDatePicker Background="White"
                         Foreground="DarkBlue"
                         AccentBrush="Teal"/>
```

**Example 2: Warm Orange Theme**
```xaml
<syncfusion:SfDatePicker Background="Lightyellow"
                         Foreground="Orange"
                         AccentBrush="OrangeRed"/>
```

**Example 3: Minimal Dark Theme**
```xaml
<syncfusion:SfDatePicker Background="DarkGray"
                         Foreground="White"
                         AccentBrush="LimeGreen"/>
```

## Right-to-Left (RTL) Support

The **FlowDirection** property controls the direction of text and layout flow, supporting right-to-left languages like Arabic, Hebrew, and Persian.

### Default Value
The default is **LeftToRight**.

### Via XAML – RTL Layout
```xaml
<syncfusion:SfDatePicker FlowDirection="RightToLeft" 
                         x:Name="sfDatePicker"/>
```

### Via C# – RTL Layout
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.FlowDirection = FlowDirection.RightToLeft;
```

### RTL Behavior Changes
When FlowDirection is set to RightToLeft:
- Text alignment reverses (text flows right-to-left)
- Selector columns appear in reverse order
- Controls align to the right side of their container
- Appropriate for Arabic, Hebrew, Urdu, and other RTL languages

### Practical Example: RTL Application
```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        FlowDirection="RightToLeft">
    <Grid>
        <!-- RTL inherited from Window -->
        <syncfusion:SfDatePicker x:Name="sfDatePicker" Width="200"/>
    </Grid>
</Window>
```

**Behavior:**
- The entire window and all child controls follow RTL layout
- SfDatePicker automatically inherits RTL direction
- No need to set on each control individually

### Overriding RTL in Child Controls
```xaml
<Window FlowDirection="RightToLeft">
    <StackPanel>
        <!-- This follows RTL (inherited from Window) -->
        <syncfusion:SfDatePicker x:Name="sfDatePicker1"/>
        
        <!-- This overrides and uses LTR -->
        <syncfusion:SfDatePicker FlowDirection="LeftToRight" 
                                 x:Name="sfDatePicker2"/>
    </StackPanel>
</Window>
```

## Theme Support

SfDatePicker supports built-in themes and custom theming through Syncfusion's theme system.

### Built-in Themes
Syncfusion provides several pre-built themes:
- Material Light
- Material Dark
- Office2019 Light / Dark
- Windows11 Light / Dark
- Fluent Light / Dark
- Bootstrap Light / Dark

### Applying Themes with SfSkinManager

The **SfSkinManager** is used to apply themes globally or to specific controls.

#### Apply Theme to All Controls in Application
```xaml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
             xmlns:skinmanager="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <Application.Resources>
    </Application.Resources>
</Application>
```

#### Apply Theme to Specific Control
```xaml
<syncfusion:SfDatePicker x:Name="sfDatePicker"
                         skinmanager:SfSkinManager.VisualStyle="MaterialLight"
                         Width="200"/>
```

### Creating Custom Themes with ThemeStudio

Syncfusion provides **ThemeStudio**, a visual tool for creating custom themes:

1. Open Syncfusion Theme Studio
2. Select **SfDatePicker** component
3. Customize colors, fonts, and other properties
4. Export the theme as a .xaml file
5. Include the theme file in your project
6. Apply the theme using SfSkinManager

### Theme Application Example
```csharp
// In code-behind
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    
    // Apply Material Light theme
    SfSkinManager.SetVisualStyle(sfDatePicker, VisualStyles.MaterialLight);
}
```

## Complete Styling Example

Here's a comprehensive example combining all styling options:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Styled DatePicker Sample">
    <Grid Background="White" Padding="20">
        <StackPanel Spacing="20">
            
            <!-- Light Theme -->
            <StackPanel>
                <TextBlock Text="Light Theme" FontSize="14" FontWeight="Bold" Margin="0,0,0,10"/>
                <syncfusion:SfDatePicker Background="White"
                                         Foreground="DarkBlue"
                                         AccentBrush="Teal"
                                         Width="200">
                    <syncfusion:SfDatePicker.SelectorStyle>
                        <Style TargetType="syncfusion:SfDateSelector">
                            <Setter Property="Background" Value="AliceBlue"/>
                            <Setter Property="Foreground" Value="DarkBlue"/>
                            <Setter Property="SelectedForeground" Value="White"/>
                        </Style>
                    </syncfusion:SfDatePicker.SelectorStyle>
                </syncfusion:SfDatePicker>
            </StackPanel>
            
            <!-- Dark Theme -->
            <StackPanel>
                <TextBlock Text="Dark Theme" FontSize="14" FontWeight="Bold" Margin="0,0,0,10"/>
                <syncfusion:SfDatePicker Background="DarkGray"
                                         Foreground="White"
                                         AccentBrush="LimeGreen"
                                         Width="200">
                    <syncfusion:SfDatePicker.SelectorStyle>
                        <Style TargetType="syncfusion:SfDateSelector">
                            <Setter Property="Background" Value="DarkSlateGray"/>
                            <Setter Property="Foreground" Value="White"/>
                            <Setter Property="SelectedForeground" Value="LimeGreen"/>
                        </Style>
                    </syncfusion:SfDatePicker.SelectorStyle>
                </syncfusion:SfDatePicker>
            </StackPanel>
            
            <!-- RTL Example -->
            <StackPanel FlowDirection="RightToLeft">
                <TextBlock Text="RTL Layout (Right-to-Left)" FontSize="14" FontWeight="Bold" Margin="0,0,0,10"/>
                <syncfusion:SfDatePicker Background="LightYellow"
                                         Foreground="DarkRed"
                                         Width="200"/>
            </StackPanel>
            
        </StackPanel>
    </Grid>
</Window>
```

## Best Practices for Styling

1. **Use built-in themes first** – They're professionally designed and well-tested
2. **Maintain sufficient contrast** – Ensure text is readable against backgrounds (WCAG AA minimum)
3. **Be consistent** – Use the same color scheme across your application
4. **Test with different displays** – Colors may appear different on various screens
5. **Consider accessibility** – Avoid relying on color alone to convey information
6. **Document custom themes** – Maintain a color palette reference for your team
7. **Use RTL properly** – Set FlowDirection at the window level, not individual controls
