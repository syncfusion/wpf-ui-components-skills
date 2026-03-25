# Appearance and Customization

## Table of Contents
- [Overview](#overview)
- [Color Customization](#color-customization)
- [CheckBox Placement](#checkbox-placement)
- [Flow Direction and RTL Support](#flow-direction-and-rtl-support)
- [Themes and Visual Styles](#themes-and-visual-styles)
- [Custom Templates](#custom-templates)
- [Font and Text Styling](#font-and-text-styling)

## Overview

The CheckListBox provides extensive customization options to match your application's visual design. From simple color changes to complete template overrides, you can tailor every aspect of the control's appearance.

**Customization levels:**
1. **Properties** - Quick color/font changes (Foreground, Background, etc.)
2. **Styles** - ItemContainerStyle for item-level customization
3. **Templates** - Complete visual restructuring (ControlTemplate, ItemTemplate)
4. **Themes** - Built-in Syncfusion visual styles

**Common scenarios:**
- Brand-specific colors and fonts
- Checkbox on right side for RTL languages
- Hover and selection highlighting
- Custom item templates with icons
- Applying Syncfusion themes

## Color Customization

### Foreground Color

The `Foreground` property controls the text color of all items.

```xml
<syncfusion:CheckListBox Foreground="Red">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
    <syncfusion:CheckListBoxItem Content="Bermuda" />
    <syncfusion:CheckListBoxItem Content="Belize" />
    <syncfusion:CheckListBoxItem Content="Panama" />
</syncfusion:CheckListBox>
```

```csharp
CheckListBox checkListBox = new CheckListBox();
checkListBox.Items.Add(new CheckListBoxItem { Content = "Mexico" });
checkListBox.Items.Add(new CheckListBoxItem { Content = "Canada" });
checkListBox.Items.Add(new CheckListBoxItem { Content = "Bermuda" });
checkListBox.Items.Add(new CheckListBoxItem { Content = "Belize" });
checkListBox.Items.Add(new CheckListBoxItem { Content = "Panama" });

// Set foreground color
checkListBox.Foreground = Brushes.Red;
```

**Using hex colors:**

```xml
<syncfusion:CheckListBox Foreground="#2E7D32">
```

**Using system colors:**

```csharp
checkListBox.Foreground = SystemColors.WindowTextBrush;
```

### Background Colors

Control the background for normal, hovered, and selected states.

**All background properties:**

```xml
<syncfusion:CheckListBox Background="SkyBlue"
                         MouseOverBackground="DeepPink"
                         SelectedItemBackground="Yellow">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
    <syncfusion:CheckListBoxItem Content="Bermuda" />
</syncfusion:CheckListBox>
```

```csharp
CheckListBox checkListBox = new CheckListBox();

// Normal state background
checkListBox.Background = Brushes.SkyBlue;

// Mouse hover background
checkListBox.MouseOverBackground = Brushes.DeepPink;

// Selected (focused) item background
checkListBox.SelectedItemBackground = Brushes.Yellow;
```

**Background property details:**

| Property | State | Default |
|----------|-------|---------|
| `Background` | Normal item background | Transparent |
| `MouseOverBackground` | Mouse hovering over item | System default (light blue) |
| `SelectedItemBackground` | Currently focused item | System default (highlight) |

**⚠️ Note:** These backgrounds are for the entire item area, not just text.

### Gradient Backgrounds

Use LinearGradientBrush for modern gradient effects:

```xml
<syncfusion:CheckListBox>
    <syncfusion:CheckListBox.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#E3F2FD" Offset="0" />
            <GradientStop Color="#BBDEFB" Offset="1" />
        </LinearGradientBrush>
    </syncfusion:CheckListBox.Background>
    
    <syncfusion:CheckListBoxItem Content="Item 1" />
    <syncfusion:CheckListBoxItem Content="Item 2" />
</syncfusion:CheckListBox>
```

### Individual Item Colors

Use ItemContainerStyle to customize individual items:

```xml
<syncfusion:CheckListBox>
    <syncfusion:CheckListBox.ItemContainerStyle>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="Foreground" Value="#1976D2" />
            <Setter Property="FontWeight" Value="SemiBold" />
        </Style>
    </syncfusion:CheckListBox.ItemContainerStyle>
    
    <syncfusion:CheckListBoxItem Content="Item 1" />
    <syncfusion:CheckListBoxItem Content="Item 2" />
</syncfusion:CheckListBox>
```

**Conditional colors with data binding:**

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Items}">
    <syncfusion:CheckListBox.ItemContainerStyle>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Style.Triggers>
                <!-- Highlight high-priority items -->
                <DataTrigger Binding="{Binding Priority}" Value="High">
                    <Setter Property="Foreground" Value="Red" />
                    <Setter Property="FontWeight" Value="Bold" />
                </DataTrigger>
                <!-- Style completed items -->
                <DataTrigger Binding="{Binding IsCompleted}" Value="True">
                    <Setter Property="Foreground" Value="Gray" />
                    <Setter Property="FontStyle" Value="Italic" />
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

## CheckBox Placement

By default, checkboxes appear on the left side. For right-to-left languages or design preferences, place them on the right.

### CheckBoxPlacement Property

```xml
<!-- Left side (default) -->
<syncfusion:CheckListBox CheckBoxPlacement="Left">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
</syncfusion:CheckListBox>

<!-- Right side -->
<syncfusion:CheckListBox CheckBoxPlacement="Right">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
</syncfusion:CheckListBox>
```

```csharp
// Set checkbox to right side
checkListBox.CheckBoxPlacement = CheckBoxPlacement.Right;
```

**When to use right placement:**
- RTL language applications (Arabic, Hebrew)
- Design consistency with other right-aligned controls
- User preference settings

### Alternative: CheckBoxAlignment

Some versions use `CheckBoxAlignment` property instead:

```xml
<syncfusion:CheckListBox CheckBoxAlignment="Right">
```

```csharp
checkListBox.CheckBoxAlignment = CheckBoxAlignment.Right;
```

## Flow Direction and RTL Support

Support right-to-left languages by changing the flow direction.

### FlowDirection Property

```xml
<!-- Left to right (default) -->
<syncfusion:CheckListBox FlowDirection="LeftToRight">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
</syncfusion:CheckListBox>

<!-- Right to left -->
<syncfusion:CheckListBox FlowDirection="RightToLeft">
    <syncfusion:CheckListBoxItem Content="المكسيك" />
    <syncfusion:CheckListBoxItem Content="كندا" />
</syncfusion:CheckListBox>
```

```csharp
// Set RTL flow direction
checkListBox.FlowDirection = FlowDirection.RightToLeft;
```

**What changes with RTL:**
- Text alignment reverses (right-aligned)
- Checkbox moves to right side (if CheckBoxPlacement not explicitly set)
- Scrollbar appears on left
- Overall layout mirrors

**Complete RTL example:**

```xml
<Window FlowDirection="RightToLeft">
    <Grid>
        <syncfusion:CheckListBox FlowDirection="RightToLeft"
                                 CheckBoxPlacement="Right"
                                 ItemsSource="{Binding Items}"
                                 DisplayMemberPath="NameArabic">
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

**⚠️ Note:** Set FlowDirection on parent Window for consistent RTL across the entire application.

## Themes and Visual Styles

Syncfusion provides professional themes to quickly style your application.

### Available Themes

- **Blend** - Microsoft Blend-inspired
- **Lime** - Fresh green color scheme
- **Metro** - Windows 8/10 flat design
- **Office2010** - Microsoft Office 2010
- **Office2013** - Microsoft Office 2013
- **Office2016** - Microsoft Office 2016
- **Office2019** - Microsoft Office 2019
- **Saffron** - Warm orange theme
- **VS2010** - Visual Studio 2010
- **MaterialLight** - Material Design light
- **MaterialDark** - Material Design dark

### Applying Themes

**Method 1: SfSkinManager (Recommended)**

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    
    <Grid>
        <syncfusion:CheckListBox>
            <!-- Theme applies automatically -->
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

**Method 2: Resource Dictionary**

```xml
<Window.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Syncfusion.Themes.MaterialLight.WPF;component/CheckListBox/CheckListBox.xaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Window.Resources>
```

**Method 3: Code-behind with SfSkinManager**

```csharp
// Apply theme to entire window
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));

// Apply theme to specific control
SfSkinManager.SetTheme(checkListBox, new Theme("Office2019"));
```

### Theme Installation

Install theme NuGet packages:

```powershell
Install-Package Syncfusion.Themes.MaterialLight.WPF
Install-Package Syncfusion.Themes.Office2019.WPF
Install-Package Syncfusion.SfSkinManager.WPF
```

**Required assemblies:**
- `Syncfusion.SfSkinManager.WPF`
- Theme-specific assembly (e.g., `Syncfusion.Themes.MaterialLight.WPF`)

## Custom Templates

For complete control over appearance, override the control template or item template.

### ItemTemplate - Custom Item Display

Display items with custom layout (icons, multiple text lines, etc.).

**Example: Items with icons**

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Countries}">
    <syncfusion:CheckListBox.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="{Binding FlagImage}" 
                       Width="24" 
                       Height="16" 
                       Margin="0,0,8,0" />
                <StackPanel>
                    <TextBlock Text="{Binding Name}" 
                               FontWeight="Bold" />
                    <TextBlock Text="{Binding Region}" 
                               FontSize="10" 
                               Foreground="Gray" />
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </syncfusion:CheckListBox.ItemTemplate>
    
    <!-- Don't forget ItemContainerStyle for IsSelected binding -->
    <syncfusion:CheckListBox.ItemContainerStyle>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="IsSelected" Value="{Binding IsChecked, Mode=TwoWay}" />
        </Style>
    </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

**Example: Multi-line items with badges**

```xml
<syncfusion:CheckListBox.ItemTemplate>
    <DataTemplate>
        <Border BorderBrush="#E0E0E0" 
                BorderThickness="0,0,0,1" 
                Padding="8,4">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                
                <StackPanel Grid.Column="0">
                    <TextBlock Text="{Binding Title}" 
                               FontWeight="SemiBold" 
                               FontSize="14" />
                    <TextBlock Text="{Binding Description}" 
                               FontSize="12" 
                               Foreground="#666" 
                               TextWrapping="Wrap" />
                </StackPanel>
                
                <Border Grid.Column="1" 
                        Background="#FF5722" 
                        CornerRadius="10" 
                        Padding="8,2" 
                        VerticalAlignment="Center"
                        Visibility="{Binding HasBadge, Converter={StaticResource BoolToVisibilityConverter}}">
                    <TextBlock Text="{Binding BadgeText}" 
                               Foreground="White" 
                               FontSize="11" 
                               FontWeight="Bold" />
                </Border>
            </Grid>
        </Border>
    </DataTemplate>
</syncfusion:CheckListBox.ItemTemplate>
```

### ItemContainerStyle - Customize Item Wrapper

Style the CheckListBoxItem container that wraps each item.

**Example: Rounded items with hover effect**

```xml
<syncfusion:CheckListBox>
    <syncfusion:CheckListBox.ItemContainerStyle>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="Background" Value="Transparent" />
            <Setter Property="Margin" Value="4" />
            <Setter Property="Padding" Value="8" />
            <Setter Property="BorderBrush" Value="#E0E0E0" />
            <Setter Property="BorderThickness" Value="1" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="syncfusion:CheckListBoxItem">
                        <Border Background="{TemplateBinding Background}"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="8"
                                Padding="{TemplateBinding Padding}">
                            <Grid>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="Auto" />
                                    <ColumnDefinition Width="*" />
                                </Grid.ColumnDefinitions>
                                
                                <CheckBox Grid.Column="0" 
                                          IsChecked="{Binding IsSelected, RelativeSource={RelativeSource TemplatedParent}, Mode=TwoWay}"
                                          Margin="0,0,8,0"
                                          VerticalAlignment="Center" />
                                
                                <ContentPresenter Grid.Column="1" 
                                                  VerticalAlignment="Center" />
                            </Grid>
                        </Border>
                        
                        <ControlTemplate.Triggers>
                            <Trigger Property="IsMouseOver" Value="True">
                                <Setter Property="Background" Value="#F5F5F5" />
                                <Setter Property="BorderBrush" Value="#2196F3" />
                            </Trigger>
                            <Trigger Property="IsSelected" Value="True">
                                <Setter Property="Background" Value="#E3F2FD" />
                            </Trigger>
                        </ControlTemplate.Triggers>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

### ControlTemplate - Customize Entire Control

Override the entire CheckListBox control template for maximum customization.

```xml
<syncfusion:CheckListBox>
    <syncfusion:CheckListBox.Template>
        <ControlTemplate TargetType="syncfusion:CheckListBox">
            <Border Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    CornerRadius="4">
                <ScrollViewer Padding="{TemplateBinding Padding}">
                    <ItemsPresenter />
                </ScrollViewer>
            </Border>
        </ControlTemplate>
    </syncfusion:CheckListBox.Template>
</syncfusion:CheckListBox>
```

## Font and Text Styling

### Basic Font Properties

```xml
<syncfusion:CheckListBox FontFamily="Segoe UI"
                         FontSize="14"
                         FontWeight="Normal"
                         FontStyle="Normal">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
</syncfusion:CheckListBox>
```

```csharp
checkListBox.FontFamily = new FontFamily("Segoe UI");
checkListBox.FontSize = 14;
checkListBox.FontWeight = FontWeights.SemiBold;
checkListBox.FontStyle = FontStyles.Normal;
```

### Custom Fonts

**Using system fonts:**

```csharp
checkListBox.FontFamily = new FontFamily("Arial");
checkListBox.FontFamily = new FontFamily("Calibri");
checkListBox.FontFamily = new FontFamily("Times New Roman");
```

**Using custom fonts:**

1. Add font file to project (e.g., `Fonts/CustomFont.ttf`)
2. Set Build Action to "Resource"
3. Reference in code:

```xml
<syncfusion:CheckListBox FontFamily="pack://application:,,,/Fonts/#Custom Font Name">
```

```csharp
checkListBox.FontFamily = new FontFamily(new Uri("pack://application:,,,/"), "./Fonts/#Custom Font Name");
```

### Text Styling with ItemContainerStyle

```xml
<syncfusion:CheckListBox>
    <syncfusion:CheckListBox.ItemContainerStyle>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="FontSize" Value="16" />
            <Setter Property="FontWeight" Value="Medium" />
            <Setter Property="Foreground" Value="#212121" />
            <Setter Property="Padding" Value="12,8" />
            
            <Style.Triggers>
                <!-- Checked items bold -->
                <Trigger Property="IsSelected" Value="True">
                    <Setter Property="FontWeight" Value="Bold" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

### Text Wrapping

For long text that should wrap:

```xml
<syncfusion:CheckListBox>
    <syncfusion:CheckListBox.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}" 
                       TextWrapping="Wrap" 
                       MaxWidth="300" />
        </DataTemplate>
    </syncfusion:CheckListBox.ItemTemplate>
</syncfusion:CheckListBox>
```

## Complete Customization Example

Here's a fully customized CheckListBox combining all concepts:

```xml
<Window x:Class="CustomizedCheckListBox.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:CustomizedCheckListBox"
        Title="Customized CheckListBox" Height="500" Width="400">
    
    <Window.DataContext>
        <local:ViewModel />
    </Window.DataContext>
    
    <Grid Margin="20" Background="#F5F5F5">
        <syncfusion:CheckListBox ItemsSource="{Binding Tasks}"
                                 Background="White"
                                 BorderBrush="#E0E0E0"
                                 BorderThickness="1"
                                 Padding="8">
            
            <!-- Custom item template -->
            <syncfusion:CheckListBox.ItemTemplate>
                <DataTemplate>
                    <Border BorderBrush="#E0E0E0" 
                            BorderThickness="0,0,0,1" 
                            Padding="0,8">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="*" />
                                <ColumnDefinition Width="Auto" />
                            </Grid.ColumnDefinitions>
                            
                            <StackPanel Grid.Column="0">
                                <TextBlock Text="{Binding Title}" 
                                           FontSize="14" 
                                           FontWeight="SemiBold"
                                           Foreground="#212121" />
                                <TextBlock Text="{Binding DueDate, StringFormat='Due: {0:MMM dd}'}" 
                                           FontSize="11" 
                                           Foreground="#757575"
                                           Margin="0,2,0,0" />
                            </StackPanel>
                            
                            <TextBlock Grid.Column="1" 
                                       Text="{Binding Priority}" 
                                       FontSize="10"
                                       FontWeight="Bold"
                                       Foreground="White"
                                       Background="#FF5722"
                                       Padding="6,2"
                                       VerticalAlignment="Top"
                                       Visibility="{Binding HasPriority, Converter={StaticResource BoolToVisibilityConverter}}" />
                        </Grid>
                    </Border>
                </DataTemplate>
            </syncfusion:CheckListBox.ItemTemplate>
            
            <!-- Custom item container style -->
            <syncfusion:CheckListBox.ItemContainerStyle>
                <Style TargetType="syncfusion:CheckListBoxItem">
                    <Setter Property="IsSelected" Value="{Binding IsCompleted, Mode=TwoWay}" />
                    <Setter Property="Margin" Value="0,4" />
                    
                    <Style.Triggers>
                        <Trigger Property="IsMouseOver" Value="True">
                            <Setter Property="Background" Value="#F0F0F0" />
                        </Trigger>
                        <DataTrigger Binding="{Binding IsCompleted}" Value="True">
                            <Setter Property="Opacity" Value="0.6" />
                        </DataTrigger>
                    </Style.Triggers>
                </Style>
            </syncfusion:CheckListBox.ItemContainerStyle>
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

## Troubleshooting

**Issue: Colors not applying**
- Check property spelling (Foreground vs ForeGround)
- Verify Brushes are correctly instantiated
- Theme styles may override - use Implicit styles or set directly

**Issue: ItemTemplate not showing**
- Ensure ItemsSource is bound correctly
- Check DataTemplate binding paths
- Remove DisplayMemberPath when using ItemTemplate

**Issue: Checkbox placement not changing**
- Verify property name (CheckBoxPlacement vs CheckBoxAlignment)
- Check Syncfusion version for correct property
- Template overrides may ignore this property

**Issue: Theme not applying**
- Install correct theme NuGet package
- Check assembly reference paths in ResourceDictionary
- Verify SfSkinManager is referenced

## Next Steps

- **Layout Configuration:** [layout-features.md](layout-features.md)
- **Performance Optimization:** [virtualization.md](virtualization.md)
- **Data Binding:** [data-binding.md](data-binding.md)
