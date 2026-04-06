# Styling and Customization

## Table of Contents
- [Overview](#overview)
- [Applying Themes](#applying-themes)
- [Button Templates](#button-templates)
- [Header Customization](#header-customization)
- [Context Menu Styling](#context-menu-styling)
- [Float Window Styling](#float-window-styling)
- [Drag Provider Customization](#drag-provider-customization)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

DockingManager provides extensive customization options through templates, styles, and themes. You can customize button appearance, headers, context menus, drag providers, and apply consistent themes using SfSkinManager.

## Applying Themes

### Using SfSkinManager

Apply Syncfusion themes to DockingManager:

**XAML:**
```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <DockPanel syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialDark}">
        <syncfusion:DockingManager x:Name="dockManager">
            <!-- Children -->
        </syncfusion:DockingManager>
    </DockPanel>
</Window>
```

**C#:**
```csharp
// Apply theme programmatically
SfSkinManager.SetTheme(this, new Theme("MaterialDark"));

// Available themes:
// - MaterialLight
// - MaterialDark
// - Office2019Colorful
// - Office2019Black
// - Office2019White
// - FluentLight
// - FluentDark
```

### Theme Resources

Reference theme resources directly:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          Background="{DynamicResource SyncfusionDockingBackground}"
                          Foreground="{DynamicResource SyncfusionDockingForeground}">
</syncfusion:DockingManager>
```

## Button Templates

Customize button templates for window chrome (close, minimize, maximize, pin, menu). All follow the same pattern:

**Available Templates:** `CloseButtonTemplate`, `MinimizeButtonTemplate`, `MaximizeButtonTemplate`, `RestoreButtonTemplate`, `AwlButtonTemplate` (pin), `MenuButtonTemplate`

**Example - Close Button with Custom Icon:**
```xml
<syncfusion:DockingManager x:Name="dockManager">
    <syncfusion:DockingManager.CloseButtonTemplate>
        <DataTemplate>
            <Button Width="16" Height="16" ToolTip="Close">
                <Path Data="M0,0 L8,8 M8,0 L0,8" Stroke="Red" StrokeThickness="2"/>
            </Button>
        </DataTemplate>
    </syncfusion:DockingManager.CloseButtonTemplate>
</syncfusion:DockingManager>
```

Apply the same pattern to other templates by changing the property name (`MinimizeButtonTemplate`, etc.) and adjusting the SVG Path data or content accordingly.

## Header Customization

### Custom Header Style

Style window headers globally:

**XAML:**
```xml
<Window.Resources>
    <Style x:Key="CustomDockHeaderStyle" TargetType="ContentPresenter">
        <Setter Property="Background" Value="#2D2D30" />
        <Setter Property="Foreground" Value="White" />
        <Setter Property="Padding" Value="10,5" />
        <Setter Property="FontWeight" Value="SemiBold" />
        <Setter Property="FontSize" Value="13" />
    </Style>
</Window.Resources>

<syncfusion:DockingManager x:Name="dockManager"
                          DockHeaderStyle="{StaticResource CustomDockHeaderStyle}">
</syncfusion:DockingManager>
```

### Per-Window Header Style

Apply unique style to specific window:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Important"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left">
    <syncfusion:DockingManager.HeaderStyle>
        <Style TargetType="ContentPresenter">
            <Setter Property="Background" Value="OrangeRed" />
            <Setter Property="Foreground" Value="White" />
            <Setter Property="FontWeight" Value="Bold" />
        </Style>
    </syncfusion:DockingManager.HeaderStyle>
</ContentControl>
```

### Advanced Header Content

Headers can contain complex content:

**XAML (with icons and animations):**
```xml
<ContentControl syncfusion:DockingManager.State="Dock">
    <syncfusion:DockingManager.Header>
        <StackPanel Orientation="Horizontal">
            <Image Source="/Images/icon.png" Width="16" Height="16" Margin="0,0,5,0" />
            <TextBlock Text="Panel Title" VerticalAlignment="Center" />
        </StackPanel>
    </syncfusion:DockingManager.Header>
</ContentControl>
```

Use triggers and storyboards in `HeaderStyle` for hover effects or animations.

## Context Menu Styling

### Style Context Menu

Customize appearance of context menus:

**XAML:**
```xml
<Window.Resources>
    <Style x:Key="CustomContextMenuStyle" TargetType="ContextMenu">
        <Setter Property="Background" Value="#2D2D30" />
        <Setter Property="Foreground" Value="White" />
        <Setter Property="BorderBrush" Value="#007ACC" />
        <Setter Property="BorderThickness" Value="1" />
        <Setter Property="Padding" Value="2" />
    </Style>
    
    <Style x:Key="CustomMenuItemStyle" TargetType="MenuItem">
        <Setter Property="Foreground" Value="White" />
        <Setter Property="Height" Value="28" />
        <Style.Triggers>
            <Trigger Property="IsHighlighted" Value="True">
                <Setter Property="Background" Value="#007ACC" />
            </Trigger>
        </Style.Triggers>
    </Style>
</Window.Resources>

<ContentControl syncfusion:DockingManager.Header="Styled Menu"
                syncfusion:DockingManager.State="Dock">
    <syncfusion:DockingManager.DockContextMenu>
        <ContextMenu Style="{StaticResource CustomContextMenuStyle}">
            <MenuItem Header="Float" Style="{StaticResource CustomMenuItemStyle}" />
            <MenuItem Header="Dock" Style="{StaticResource CustomMenuItemStyle}" />
            <Separator Background="#3F3F46" />
            <MenuItem Header="Close" Style="{StaticResource CustomMenuItemStyle}" />
        </ContextMenu>
    </syncfusion:DockingManager.DockContextMenu>
</ContentControl>
```

## Float Window Styling

Customize floating window appearance and title bars:

**XAML:**
```xml
<Window.Resources>
    <Style x:Key="CustomFloatWindowStyle" TargetType="Window">
        <Setter Property="Background" Value="#1E1E1E" />
        <Setter Property="BorderBrush" Value="#007ACC" />
        <Setter Property="BorderThickness" Value="1" />
        <Setter Property="ResizeMode" Value="CanResizeWithGrip" />
    </Style>
</Window.Resources>

<syncfusion:DockingManager FloatWindowStyle="{StaticResource CustomFloatWindowStyle}">
</syncfusion:DockingManager>
```

Similarly customize `FloatWindowTitleStyle` for title bar appearance.

## Drag Provider Customization

### Drag Hint Style

Customize drag-and-drop hints:

**XAML:**
```xml
<Window.Resources>
    <Style x:Key="CustomDragHintStyle" TargetType="Path">
        <Setter Property="Fill" Value="#AA007ACC" />
        <Setter Property="Stroke" Value="#007ACC" />
        <Setter Property="StrokeThickness" Value="2" />
        <Setter Property="Effect">
            <Setter.Value>
                <DropShadowEffect Color="#007ACC" BlurRadius="10" ShadowDepth="0" />
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<syncfusion:DockingManager DragHintStyle="{StaticResource CustomDragHintStyle}">
</syncfusion:DockingManager>
```

### Drag Preview

Customize the preview shown while dragging:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          DragPreviewOpacity="0.6"
                          ShowDragPreview="True">
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.ShowDragPreview = true;
dockManager.DragPreviewOpacity = 0.6;
```

## Common Patterns

### Dark Theme Customization

Complete dark theme implementation:

**XAML:**
```xml
<Window.Resources>
    <!-- Color resources -->
    <SolidColorBrush x:Key="DarkBackground" Color="#1E1E1E" />
    <SolidColorBrush x:Key="DarkForeground" Color="#FFFFFF" />
    <SolidColorBrush x:Key="DarkAccent" Color="#007ACC" />
    <SolidColorBrush x:Key="DarkBorder" Color="#3F3F46" />
    
    <!-- Header style -->
    <Style x:Key="DarkHeaderStyle" TargetType="ContentPresenter">
        <Setter Property="Background" Value="{StaticResource DarkBackground}" />
        <Setter Property="Foreground" Value="{StaticResource DarkForeground}" />
        <Setter Property="BorderBrush" Value="{StaticResource DarkBorder}" />
        <Setter Property="BorderThickness" Value="0,0,0,1" />
        <Setter Property="Padding" Value="8,4" />
    </Style>
    
    <!-- Button template -->
    <ControlTemplate x:Key="DarkButtonTemplate" TargetType="Button">
        <Border Background="Transparent" 
                BorderThickness="0"
                Padding="4">
            <ContentPresenter />
        </Border>
        <ControlTemplate.Triggers>
            <Trigger Property="IsMouseOver" Value="True">
                <Setter Property="Background" Value="{StaticResource DarkAccent}" />
            </Trigger>
        </ControlTemplate.Triggers>
    </ControlTemplate>
</Window.Resources>

<syncfusion:DockingManager x:Name="dockManager"
                          Background="{StaticResource DarkBackground}"
                          Foreground="{StaticResource DarkForeground}"
                          DockHeaderStyle="{StaticResource DarkHeaderStyle}">
    
    <!-- Apply button templates -->
    <syncfusion:DockingManager.CloseButtonTemplate>
        <DataTemplate>
            <Button Template="{StaticResource DarkButtonTemplate}" 
                    Width="16" Height="16">
                <Path Data="M0,0 L8,8 M8,0 L0,8" 
                      Stroke="{StaticResource DarkForeground}" 
                      StrokeThickness="1.5"/>
            </Button>
        </DataTemplate>
    </syncfusion:DockingManager.CloseButtonTemplate>
    
</syncfusion:DockingManager>
```

### Visual Studio Theme

Replicate Visual Studio styling:

**XAML:**
```xml
<Window.Resources>
    <SolidColorBrush x:Key="VSBlue" Color="#007ACC" />
    <SolidColorBrush x:Key="VSDarkGray" Color="#2D2D30" />
    <SolidColorBrush x:Key="VSLightGray" Color="#3F3F46" />
    
    <Style x:Key="VSHeaderStyle" TargetType="ContentPresenter">
        <Setter Property="Background" Value="{StaticResource VSDarkGray}" />
        <Setter Property="Foreground" Value="White" />
        <Setter Property="FontFamily" Value="Segoe UI" />
        <Setter Property="FontSize" Value="11" />
        <Setter Property="Padding" Value="10,4" />
    </Style>
</Window.Resources>

<syncfusion:DockingManager x:Name="dockManager"
                          Background="{StaticResource VSDarkGray}"
                          Foreground="White"
                          DockHeaderStyle="{StaticResource VSHeaderStyle}"
                          IsVS2010DraggingEnabled="True"
                          UseDocumentContainer="True">
</syncfusion:DockingManager>
```

### Conditional Styling

Apply different styles based on state:

**XAML:**
```xml
<Window.Resources>
    <Style x:Key="ConditionalHeaderStyle" TargetType="ContentPresenter">
        <Setter Property="Background" Value="Gray" />
        <Style.Triggers>
            <DataTrigger Binding="{Binding IsActive, RelativeSource={RelativeSource AncestorType=Window}}" 
                         Value="True">
                <Setter Property="Background" Value="#007ACC" />
                <Setter Property="Foreground" Value="White" />
            </DataTrigger>
        </Style.Triggers>
    </Style>
</Window.Resources>
```

### Icon-Based Button Templates

Use icon fonts for buttons:

**XAML:**
```xml
<Window.Resources>
    <!-- Assuming Segoe MDL2 Assets font -->
    <Style x:Key="IconButtonStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Segoe MDL2 Assets" />
        <Setter Property="FontSize" Value="14" />
        <Setter Property="Width" Value="20" />
        <Setter Property="Height" Value="20" />
        <Setter Property="Background" Value="Transparent" />
        <Setter Property="Foreground" Value="White" />
        <Setter Property="BorderThickness" Value="0" />
        <Style.Triggers>
            <Trigger Property="IsMouseOver" Value="True">
                <Setter Property="Background" Value="#40FFFFFF" />
            </Trigger>
        </Style.Triggers>
    </Style>
</Window.Resources>

<syncfusion:DockingManager.CloseButtonTemplate>
    <DataTemplate>
        <Button Content="&#xE8BB;" Style="{StaticResource IconButtonStyle}" />
    </DataTemplate>
</syncfusion:DockingManager.CloseButtonTemplate>

<syncfusion:DockingManager.AwlButtonTemplate>
    <DataTemplate>
        <Button Content="&#xE718;" Style="{StaticResource IconButtonStyle}" />
    </DataTemplate>
</syncfusion:DockingManager.AwlButtonTemplate>
```

## Troubleshooting

### Theme Not Applying

❌ **Problem:** SfSkinManager theme doesn't affect DockingManager.

✅ **Solutions:**
```csharp
// Ensure SfSkinManager is installed
// Install-Package Syncfusion.SfSkinManager.WPF

// Apply to parent container, not directly to DockingManager
SfSkinManager.SetTheme(this, new Theme("MaterialDark"));

// OR apply to DockPanel/Grid containing DockingManager
```

**XAML:**
```xml
<!-- ✅ CORRECT: Apply to parent -->
<DockPanel syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialDark}">
    <syncfusion:DockingManager x:Name="dockManager" />
</DockPanel>

<!-- ❌ INCORRECT: Direct application -->
<syncfusion:DockingManager syncfusionskin:SfSkinManager.Theme="..." />
```

### Custom Template Not Showing

❌ **Problem:** Button templates don't appear.

✅ **Check:**
```xml
<!-- Ensure DataTemplate contains a Button or ToggleButton -->
<syncfusion:DockingManager.CloseButtonTemplate>
    <DataTemplate>
        <Button>  <!-- Must be Button/ToggleButton -->
            <Path ... />
        </Button>
    </DataTemplate>
</syncfusion:DockingManager.CloseButtonTemplate>
```

### Header Style Not Working

❌ **Problem:** DockHeaderStyle has no effect.

✅ **Solution:**
```xml
<!-- TargetType must be ContentPresenter -->
<Style x:Key="MyHeaderStyle" TargetType="ContentPresenter">
    <Setter Property="Background" Value="Blue" />
</Style>

<syncfusion:DockingManager DockHeaderStyle="{StaticResource MyHeaderStyle}">
</syncfusion:DockingManager>
```

### Context Menu Style Inconsistent

❌ **Problem:** Context menu doesn't match custom style.

✅ **Solution:**
```xml
<!-- Apply style to both ContextMenu and MenuItem -->
<ContextMenu Style="{StaticResource CustomContextMenuStyle}">
    <MenuItem Header="Item" Style="{StaticResource CustomMenuItemStyle}" />
</ContextMenu>

<!-- Or use implicit styles in Resources -->
<ContextMenu.Resources>
    <Style TargetType="MenuItem">
        <Setter Property="Foreground" Value="White" />
    </Style>
</ContextMenu.Resources>
```

### Float Window Style Not Applied

❌ **Problem:** FloatWindowStyle ignored.

✅ **Check:**
```csharp
// Ensure using native float windows
dockManager.UseNativeFloatWindow = true;

// Then apply style
dockManager.FloatWindowStyle = Resources["CustomFloatWindowStyle"] as Style;
```

### Drag Hints Wrong Color

❌ **Problem:** Drag hints don't use custom colors.

✅ **Solution:**
```xml
<!-- Ensure DragHintStyle targets Path -->
<Style x:Key="CustomDragHintStyle" TargetType="Path">
    <Setter Property="Fill" Value="Blue" />
    <Setter Property="Opacity" Value="0.5" />
</Style>

<syncfusion:DockingManager DragHintStyle="{StaticResource CustomDragHintStyle}">
</syncfusion:DockingManager>
```
