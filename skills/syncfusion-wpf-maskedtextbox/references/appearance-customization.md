# Appearance and Customization

This guide covers visual customization options for SfMaskedEdit including colors, borders, watermarks, and theming.

## Table of Contents
- [Background Customization](#background-customization)
- [Foreground Customization](#foreground-customization)
- [Border Customization](#border-customization)
- [Error Indication](#error-indication)
- [Flow Direction (RTL Support)](#flow-direction-rtl-support)
- [Watermark Configuration](#watermark-configuration)
- [Custom Watermark Template](#custom-watermark-template)
- [Theme Integration](#theme-integration)

## Background Customization

### Setting Background Color

```xml
<syncfusion:SfMaskedEdit Background="LightYellow" x:Name="maskedEdit"/>
```

```csharp
SfMaskedEdit maskedEdit = new SfMaskedEdit();
maskedEdit.Background = Brushes.LightYellow;
```

**Default:** White

### Setting Selection Highlight Color

The `SelectionBrush` property controls the color of selected text:

```xml
<syncfusion:SfMaskedEdit 
    Background="White"
    SelectionBrush="LightBlue"
    x:Name="maskedEdit"/>
```

```csharp
maskedEdit.SelectionBrush = Brushes.LightBlue;
```

**Default:** Royal Blue

### Example: Custom Background Scheme

```xml
<StackPanel>
    <!-- Light theme -->
    <syncfusion:SfMaskedEdit 
        Background="White"
        SelectionBrush="DodgerBlue"
        Mask="\d{4}"
        MaskType="RegEx"
        Margin="5"/>
    
    <!-- Dark theme -->
    <syncfusion:SfMaskedEdit 
        Background="#2D2D30"
        Foreground="White"
        SelectionBrush="Orange"
        Mask="\d{4}"
        MaskType="RegEx"
        Margin="5"/>
    
    <!-- Accent theme -->
    <syncfusion:SfMaskedEdit 
        Background="#FFF8DC"
        SelectionBrush="#FFD700"
        Mask="\d{4}"
        MaskType="RegEx"
        Margin="5"/>
</StackPanel>
```

## Foreground Customization

### Setting Text Color

```xml
<syncfusion:SfMaskedEdit Foreground="DarkBlue" x:Name="maskedEdit"/>
```

```csharp
maskedEdit.Foreground = Brushes.DarkBlue;
```

**Default:** Black

### Setting Caret Color

The `CaretBrush` property controls the cursor color:

```xml
<syncfusion:SfMaskedEdit 
    Foreground="Black"
    CaretBrush="Red"
    x:Name="maskedEdit"/>
```

```csharp
maskedEdit.CaretBrush = Brushes.Red;
```

**Default:** null (uses system default)

### Example: Complete Foreground Styling

```xml
<syncfusion:SfMaskedEdit 
    Foreground="Navy"
    CaretBrush="OrangeRed"
    Background="AliceBlue"
    SelectionBrush="LightSteelBlue"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"/>
```

## Border Customization

### Setting Border Color

```xml
<syncfusion:SfMaskedEdit 
    BorderBrush="Blue"
    BorderThickness="2"
    x:Name="maskedEdit"/>
```

```csharp
maskedEdit.BorderBrush = Brushes.Blue;
maskedEdit.BorderThickness = new Thickness(2);
```

**Default:** Lavender (BorderBrush), 1 (BorderThickness)

### Custom Border Styles

```xml
<!-- Thick border -->
<syncfusion:SfMaskedEdit 
    BorderBrush="DarkGray"
    BorderThickness="3"
    Mask="\d+"
    MaskType="RegEx"/>

<!-- No border -->
<syncfusion:SfMaskedEdit 
    BorderThickness="0"
    Background="WhiteSmoke"
    Mask="\d+"
    MaskType="RegEx"/>

<!-- Asymmetric border (left accent) -->
<syncfusion:SfMaskedEdit 
    BorderBrush="DodgerBlue"
    BorderThickness="4,1,1,1"
    Mask="\d+"
    MaskType="RegEx"/>
```

### Focus Visual Style

Customize appearance when control has focus:

```xml
<syncfusion:SfMaskedEdit x:Name="maskedEdit">
    <syncfusion:SfMaskedEdit.Style>
        <Style TargetType="syncfusion:SfMaskedEdit">
            <Style.Triggers>
                <Trigger Property="IsFocused" Value="True">
                    <Setter Property="BorderBrush" Value="DodgerBlue"/>
                    <Setter Property="BorderThickness" Value="2"/>
                </Trigger>
            </Style.Triggers>
        </Style>
    </syncfusion:SfMaskedEdit.Style>
</syncfusion:SfMaskedEdit>
```

## Error Indication

### Error Border Color

When validation fails (`HasError` is true), an error border is displayed:

```xml
<syncfusion:SfMaskedEdit 
    ErrorBorderBrush="Red"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"
    x:Name="phoneInput"/>
```

```csharp
phoneInput.ErrorBorderBrush = Brushes.Red;
```

**Default:** Red

### Custom Error Styles

```xml
<!-- Orange error border -->
<syncfusion:SfMaskedEdit 
    ErrorBorderBrush="Orange"
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
    MaskType="RegEx"/>

<!-- Thick red error border -->
<syncfusion:SfMaskedEdit 
    ErrorBorderBrush="DarkRed"
    BorderThickness="3"
    Mask="\d{5}-\d{4}"
    MaskType="RegEx"/>
```

### Example: Error with Visual Feedback

```xml
<StackPanel>
    <syncfusion:SfMaskedEdit 
        x:Name="emailInput"
        Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
        MaskType="RegEx"
        ErrorBorderBrush="Red"
        ValidationMode="LostFocus"
        LostFocus="EmailInput_LostFocus"
        Width="250"
        HorizontalAlignment="Left"/>
    
    <TextBlock x:Name="errorMessage" 
               Foreground="Red" 
               Visibility="Collapsed"
               Text="Invalid email format"
               Margin="5,2,0,0"/>
</StackPanel>
```

```csharp
private void EmailInput_LostFocus(object sender, RoutedEventArgs e)
{
    if (emailInput.HasError)
    {
        errorMessage.Visibility = Visibility.Visible;
        emailInput.ErrorBorderBrush = Brushes.DarkRed;
    }
    else
    {
        errorMessage.Visibility = Visibility.Collapsed;
    }
}
```

## Flow Direction (RTL Support)

### Right-to-Left Layout

For languages like Arabic, Hebrew, Persian:

```xml
<syncfusion:SfMaskedEdit 
    FlowDirection="RightToLeft"
    Mask="\d+"
    MaskType="RegEx"
    x:Name="maskedEdit"/>
```

```csharp
maskedEdit.FlowDirection = FlowDirection.RightToLeft;
```

**Default:** LeftToRight

### Example: Bilingual Form

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    
    <!-- Left-to-right (English) -->
    <StackPanel Grid.Column="0" Margin="10">
        <TextBlock Text="Phone Number:" FontWeight="Bold"/>
        <syncfusion:SfMaskedEdit 
            FlowDirection="LeftToRight"
            Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
            MaskType="RegEx"/>
    </StackPanel>
    
    <!-- Right-to-left (Arabic) -->
    <StackPanel Grid.Column="1" Margin="10">
        <TextBlock Text="رقم الهاتف:" FontWeight="Bold" FlowDirection="RightToLeft"/>
        <syncfusion:SfMaskedEdit 
            FlowDirection="RightToLeft"
            Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
            MaskType="RegEx"/>
    </StackPanel>
</Grid>
```

## Watermark Configuration

### Basic Watermark Text

Display instructional text when control is empty:

```xml
<syncfusion:SfMaskedEdit 
    Watermark="Enter phone number"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"
    x:Name="phoneInput"/>
```

```csharp
phoneInput.Watermark = "Enter phone number";
```

**Default:** null (no watermark)

### When Watermark is Visible

Watermark is shown when:
- Control is not focused
- No valid characters have been entered
- Value is null or empty

Watermark is hidden when:
- Control has focus (unless ShowPromptOnFocus is false and no input yet)
- User has entered any valid character

### Example: Multiple Watermarks

```xml
<StackPanel Spacing="10">
    <syncfusion:SfMaskedEdit 
        Watermark="(___) ___-____"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"/>
    
    <syncfusion:SfMaskedEdit 
        Watermark="example@domain.com"
        Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
        MaskType="RegEx"/>
    
    <syncfusion:SfMaskedEdit 
        Watermark="MM/DD/YYYY"
        Mask="(0[1-9]|1[0-2])/(0[1-9]|[12][0-9]|3[01])/\d{4}"
        MaskType="RegEx"/>
    
    <syncfusion:SfMaskedEdit 
        Watermark="$ 0.00"
        Mask="$ \d+\.\d{2}"
        MaskType="RegEx"/>
</StackPanel>
```

## Custom Watermark Template

### Basic Custom Template

```xml
<syncfusion:SfMaskedEdit 
    Watermark="Enter email"
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
    MaskType="RegEx">
    <syncfusion:SfMaskedEdit.WatermarkTemplate>
        <DataTemplate>
            <TextBlock 
                Text="{Binding}" 
                Foreground="Gray"
                FontStyle="Italic"/>
        </DataTemplate>
    </syncfusion:SfMaskedEdit.WatermarkTemplate>
</syncfusion:SfMaskedEdit>
```

### Styled Watermark Template

```xml
<syncfusion:SfMaskedEdit 
    Watermark="Type here"
    Mask="\d+"
    MaskType="RegEx">
    <syncfusion:SfMaskedEdit.WatermarkTemplate>
        <DataTemplate>
            <Border Background="Yellow" Padding="2">
                <TextBlock 
                    Text="{Binding}" 
                    Foreground="Blue"
                    FontWeight="Bold"
                    TextAlignment="Center"/>
            </Border>
        </DataTemplate>
    </syncfusion:SfMaskedEdit.WatermarkTemplate>
</syncfusion:SfMaskedEdit>
```

### Watermark with Icon

```xml
<syncfusion:SfMaskedEdit 
    Watermark="Enter phone number"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"
    Width="250">
    <syncfusion:SfMaskedEdit.WatermarkTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Path Data="M6.62,10.79C8.06,13.62 10.38,15.94 13.21,17.38L15.41,15.18C15.69,14.9 16.08,14.82 16.43,14.93C17.55,15.3 18.75,15.5 20,15.5A1,1 0 0,1 21,16.5V20A1,1 0 0,1 20,21A17,17 0 0,1 3,4A1,1 0 0,1 4,3H7.5A1,1 0 0,1 8.5,4C8.5,5.25 8.7,6.45 9.07,7.57C9.18,7.92 9.1,8.31 8.82,8.59L6.62,10.79Z"
                      Fill="Gray" 
                      Width="14" 
                      Height="14"
                      Stretch="Uniform"
                      Margin="0,0,5,0"/>
                <TextBlock 
                    Text="{Binding}" 
                    Foreground="Gray"
                    FontStyle="Italic"
                    VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfMaskedEdit.WatermarkTemplate>
</syncfusion:SfMaskedEdit>
```

### Animated Watermark

```xml
<syncfusion:SfMaskedEdit 
    Watermark="Click to enter..."
    Mask="\d+"
    MaskType="RegEx">
    <syncfusion:SfMaskedEdit.WatermarkTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}" Foreground="Orange">
                <TextBlock.Triggers>
                    <EventTrigger RoutedEvent="TextBlock.Loaded">
                        <BeginStoryboard>
                            <Storyboard RepeatBehavior="Forever">
                                <DoubleAnimation 
                                    Storyboard.TargetProperty="Opacity"
                                    From="1.0" To="0.3" Duration="0:0:1" 
                                    AutoReverse="True"/>
                            </Storyboard>
                        </BeginStoryboard>
                    </EventTrigger>
                </TextBlock.Triggers>
            </TextBlock>
        </DataTemplate>
    </syncfusion:SfMaskedEdit.WatermarkTemplate>
</syncfusion:SfMaskedEdit>
```

## Theme Integration

### Using SfSkinManager

Apply Syncfusion themes to SfMaskedEdit:

```csharp
using Syncfusion.SfSkinManager;
using Syncfusion.Themes.MaterialLight.WPF;

// In Window or control code-behind
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=MaterialLight}">
    <StackPanel>
        <syncfusion:SfMaskedEdit 
            Mask="\d+"
            MaskType="RegEx"/>
    </StackPanel>
</Window>
```

### Available Themes

- **MaterialLight** / **MaterialDark**
- **FluentLight** / **FluentDark**
- **Office2019Colorful** / **Office2019Black** / **Office2019White**
- **Office2016Colorful** / **Office2016White** / **Office2016DarkGray**
- **VisualStudio2015**
- **VisualStudio2013**

### Theme Installation

Install theme NuGet packages:

```powershell
Install-Package Syncfusion.Themes.MaterialLight.WPF
```

### Example: Themed Application

```xml
<Window x:Class="ThemedApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=MaterialLight}"
        Title="Themed Masked Edit" Height="300" Width="400">
    <StackPanel Margin="20">
        <TextBlock Text="Material Light Theme" FontWeight="Bold" FontSize="16" Margin="0,0,0,10"/>
        
        <syncfusion:SfMaskedEdit 
            Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
            MaskType="RegEx"
            Watermark="Phone number"
            Margin="0,0,0,10"/>
        
        <syncfusion:SfMaskedEdit 
            Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
            MaskType="RegEx"
            Watermark="Email address"
            Margin="0,0,0,10"/>
        
        <syncfusion:SfMaskedEdit 
            Mask="$ \d+\.\d{2}"
            MaskType="RegEx"
            Watermark="Amount"/>
    </StackPanel>
</Window>
```

### Runtime Theme Switching

```xml
<Window x:Name="mainWindow">
    <StackPanel>
        <ComboBox x:Name="themeCombo" 
                  SelectionChanged="ThemeCombo_SelectionChanged"
                  Width="150"
                  HorizontalAlignment="Left"
                  Margin="20">
            <ComboBoxItem Content="MaterialLight" IsSelected="True"/>
            <ComboBoxItem Content="MaterialDark"/>
            <ComboBoxItem Content="FluentLight"/>
            <ComboBoxItem Content="FluentDark"/>
        </ComboBox>
        
        <syncfusion:SfMaskedEdit 
            Mask="\d+"
            MaskType="RegEx"
            Margin="20"/>
    </StackPanel>
</Window>
```

```csharp
private void ThemeCombo_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string themeName = (themeCombo.SelectedItem as ComboBoxItem).Content.ToString();
    SfSkinManager.SetTheme(mainWindow, new Theme(themeName));
}
```

## Complete Styling Example

```xml
<Window x:Class="CustomStyleDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Custom Styling Demo" Height="450" Width="500">
    <Window.Resources>
        <!-- Define reusable style -->
        <Style x:Key="ModernMaskedEditStyle" TargetType="syncfusion:SfMaskedEdit">
            <Setter Property="Background" Value="White"/>
            <Setter Property="Foreground" Value="#333333"/>
            <Setter Property="BorderBrush" Value="#CCCCCC"/>
            <Setter Property="BorderThickness" Value="1"/>
            <Setter Property="CaretBrush" Value="DodgerBlue"/>
            <Setter Property="SelectionBrush" Value="LightSkyBlue"/>
            <Setter Property="ErrorBorderBrush" Value="#E74C3C"/>
            <Setter Property="Height" Value="35"/>
            <Setter Property="Padding" Value="8,0"/>
            <Style.Triggers>
                <Trigger Property="IsFocused" Value="True">
                    <Setter Property="BorderBrush" Value="DodgerBlue"/>
                    <Setter Property="BorderThickness" Value="2"/>
                </Trigger>
                <Trigger Property="IsMouseOver" Value="True">
                    <Setter Property="BorderBrush" Value="#999999"/>
                </Trigger>
            </Style.Triggers>
        </Style>
    </Window.Resources>
    
    <StackPanel Margin="30">
        <TextBlock Text="Modern Form" FontSize="20" FontWeight="Bold" Margin="0,0,0,20"/>
        
        <TextBlock Text="Phone Number" Margin="0,0,0,5"/>
        <syncfusion:SfMaskedEdit 
            Style="{StaticResource ModernMaskedEditStyle}"
            Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
            MaskType="RegEx"
            Watermark="(___) ___-____"
            Margin="0,0,0,15"/>
        
        <TextBlock Text="Email Address" Margin="0,0,0,5"/>
        <syncfusion:SfMaskedEdit 
            Style="{StaticResource ModernMaskedEditStyle}"
            Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
            MaskType="RegEx"
            Watermark="example@domain.com"
            Margin="0,0,0,15"/>
        
        <TextBlock Text="Credit Card" Margin="0,0,0,5"/>
        <syncfusion:SfMaskedEdit 
            Style="{StaticResource ModernMaskedEditStyle}"
            Mask="\d{4} \d{4} \d{4} \d{4}"
            MaskType="RegEx"
            Watermark="____ ____ ____ ____"
            Margin="0,0,0,15"/>
        
        <Button Content="Submit" Width="100" Height="35" 
                Background="DodgerBlue" Foreground="White"
                BorderThickness="0"
                HorizontalAlignment="Left"/>
    </StackPanel>
</Window>
```

## Best Practices

1. **Consistent styling:** Use styles or themes for uniform appearance across your application
2. **Accessibility:** Ensure sufficient color contrast between text and background
3. **Error visibility:** Make error states clearly distinguishable (color + border)
4. **Watermark clarity:** Use watermarks that clearly indicate expected input format
5. **RTL support:** Test with FlowDirection="RightToLeft" for international applications
6. **Theme integration:** Leverage Syncfusion themes for professional, consistent UI
7. **Focus indicators:** Ensure focused controls are visually distinct
8. **Test in different themes:** Verify appearance in light and dark themes
