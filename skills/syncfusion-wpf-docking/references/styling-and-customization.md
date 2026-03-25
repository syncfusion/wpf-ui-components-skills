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

### Close Button Template

Customize the close button appearance:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager">
    <syncfusion:DockingManager.CloseButtonTemplate>
        <DataTemplate>
            <Button Width="16" Height="16" 
                    Background="Transparent"
                    BorderThickness="0">
                <Path Data="M0,0 L8,8 M8,0 L0,8" 
                      Stroke="Red" 
                      StrokeThickness="2"/>
            </Button>
        </DataTemplate>
    </syncfusion:DockingManager.CloseButtonTemplate>
</syncfusion:DockingManager>
```

**Custom Close Button with Icon:**
```xml
<syncfusion:DockingManager.CloseButtonTemplate>
    <DataTemplate>
        <Button Width="20" Height="20" 
                Padding="2"
                ToolTip="Close">
            <Image Source="/Images/close_icon.png" />
        </Button>
    </DataTemplate>
</syncfusion:DockingManager.CloseButtonTemplate>
```

### Pin Button Template

Customize auto-hide pin button:

**XAML:**
```xml
<syncfusion:DockingManager.AwlButtonTemplate>
    <DataTemplate>
        <ToggleButton Width="18" Height="18"
                      IsChecked="{Binding IsAutoHidden, RelativeSource={RelativeSource AncestorType=syncfusion:DockingManager}}">
            <Path Data="M4,0 L8,4 L4,8 Z" Fill="Blue" />
        </ToggleButton>
    </DataTemplate>
</syncfusion:DockingManager.AwlButtonTemplate>
```

### Minimize Button Template

**XAML:**
```xml
<syncfusion:DockingManager.MinimizeButtonTemplate>
    <DataTemplate>
        <Button Width="18" Height="18" ToolTip="Minimize">
            <Rectangle Width="10" Height="2" Fill="{DynamicResource SyncfusionForeground}" />
        </Button>
    </DataTemplate>
</syncfusion:DockingManager.MinimizeButtonTemplate>
```

### Maximize/Restore Button Templates

**Maximize:**
```xml
<syncfusion:DockingManager.MaximizeButtonTemplate>
    <DataTemplate>
        <Button Width="18" Height="18" ToolTip="Maximize">
            <Rectangle Width="10" Height="10" 
                       Stroke="{DynamicResource SyncfusionForeground}"
                       StrokeThickness="2" />
        </Button>
    </DataTemplate>
</syncfusion:DockingManager.MaximizeButtonTemplate>
```

**Restore:**
```xml
<syncfusion:DockingManager.RestoreButtonTemplate>
    <DataTemplate>
        <Button Width="18" Height="18" ToolTip="Restore">
            <Grid>
                <Rectangle Width="8" Height="8" 
                           Margin="2,0,0,2"
                           Stroke="{DynamicResource SyncfusionForeground}"
                           StrokeThickness="1.5" />
                <Rectangle Width="8" Height="8" 
                           Margin="0,2,2,0"
                           Stroke="{DynamicResource SyncfusionForeground}"
                           StrokeThickness="1.5" />
            </Grid>
        </Button>
    </DataTemplate>
</syncfusion:DockingManager.RestoreButtonTemplate>
```

### Menu Button Template

Customize the context menu dropdown button:

**XAML:**
```xml
<syncfusion:DockingManager.MenuButtonTemplate>
    <DataTemplate>
        <Button Width="18" Height="18" ToolTip="Options">
            <Path Data="M2,5 L6,5 M2,8 L6,8 M2,11 L6,11" 
                  Stroke="{DynamicResource SyncfusionForeground}" 
                  StrokeThickness="1.5" />
        </Button>
    </DataTemplate>
</syncfusion:DockingManager.MenuButtonTemplate>
```

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

### Header with Icons

Add icons to headers:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left">
    <syncfusion:DockingManager.Header>
        <StackPanel Orientation="Horizontal">
            <Image Source="/Images/solution_explorer.png" 
                   Width="16" Height="16" 
                   Margin="0,0,5,0" />
            <TextBlock Text="Solution Explorer" 
                       VerticalAlignment="Center" />
        </StackPanel>
    </syncfusion:DockingManager.Header>
</ContentControl>
```

### Animated Headers

Add visual effects to headers:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Animated"
                syncfusion:DockingManager.State="Dock">
    <syncfusion:DockingManager.HeaderStyle>
        <Style TargetType="ContentPresenter">
            <Setter Property="Background" Value="#3F3F46" />
            <Style.Triggers>
                <Trigger Property="IsMouseOver" Value="True">
                    <Trigger.EnterActions>
                        <BeginStoryboard>
                            <Storyboard>
                                <ColorAnimation Storyboard.TargetProperty="(ContentPresenter.Background).(SolidColorBrush.Color)"
                                              To="#007ACC" 
                                              Duration="0:0:0.2" />
                            </Storyboard>
                        </BeginStoryboard>
                    </Trigger.EnterActions>
                    <Trigger.ExitActions>
                        <BeginStoryboard>
                            <Storyboard>
                                <ColorAnimation Storyboard.TargetProperty="(ContentPresenter.Background).(SolidColorBrush.Color)"
                                              To="#3F3F46" 
                                              Duration="0:0:0.2" />
                            </Storyboard>
                        </BeginStoryboard>
                    </Trigger.ExitActions>
                </Trigger>
            </Style.Triggers>
        </Style>
    </syncfusion:DockingManager.HeaderStyle>
</ContentControl>
```

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

### Float Window Style

Customize floating window appearance:

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

<syncfusion:DockingManager x:Name="dockManager"
                          FloatWindowStyle="{StaticResource CustomFloatWindowStyle}">
</syncfusion:DockingManager>
```

### Float Window Title Bar

Customize title bar of float windows:

**XAML:**
```xml
<Style x:Key="FloatWindowTitleStyle" TargetType="ContentPresenter">
    <Setter Property="Background" Value="#2D2D30" />
    <Setter Property="Foreground" Value="White" />
    <Setter Property="FontSize" Value="12" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="Padding" Value="8,4" />
</Style>

<syncfusion:DockingManager FloatWindowTitleStyle="{StaticResource FloatWindowTitleStyle}">
</syncfusion:DockingManager>
```

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
