# Appearance and Customization — Themes & Styling

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
