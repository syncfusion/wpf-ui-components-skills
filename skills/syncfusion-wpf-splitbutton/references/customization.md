# Customization and Theming

This guide covers styles, templates, and theming options for customizing the appearance of the Split Button control.

## Table of Contents
- [Overview](#overview)
- [Editing Appearance in Expression Blend](#editing-appearance-in-expression-blend)
- [Editing Appearance in Visual Studio](#editing-appearance-in-visual-studio)
- [Applying Built-in Themes](#applying-built-in-themes)
- [Creating Custom Themes](#creating-custom-themes)
- [Best Practices](#best-practices)

## Overview

WPF styles and templates is a suite of features that allows developers and designers to create visually compelling effects and achieve a consistent appearance across products. The Split Button control supports full customization through WPF's styling and templating system.

**Customization Options:**
- **Styles** - Change colors, fonts, margins, and other visual properties
- **Templates** - Completely redesign the control's visual structure
- **Themes** - Apply consistent styling across your entire application
- **Custom Themes** - Create branded themes using ThemeStudio

**Tools:**
- **Expression Blend** - Visual designer for creating and editing templates
- **Visual Studio** - XAML editor with design view support
- **ThemeStudio** - Online tool for creating custom themes

## Editing Appearance in Expression Blend

Expression Blend provides a visual way to edit control templates and styles.

### Steps to Edit Template in Blend

**Step 1: Open Application in Expression Blend**
- Launch Expression Blend
- Open your WPF project
- Navigate to the view containing the Split Button

**Step 2: Select the Control**
- Click on the Split Button control in the designer
- The control becomes selected with selection handles

**Step 3: Access Edit Template Menu**
- Right-click on the Split Button control
- Choose the **Edit Template** menu option
- You'll see two sub-options:
  - **Edit a Copy...** - Edit a copy of the default style
  - **Create Empty...** - Create an empty template from scratch

### Edit a Copy Option

This option creates a copy of the default template that you can modify.

**Step 4: Create ControlTemplate Resource**

When selecting "Edit a Copy...", the **Create ControlTemplate Resource** dialog opens:

**Dialog Options:**
- **Name (Key):** Enter a name for your template resource
  - Example: `CustomSplitButtonTemplate`
  - Leave blank to apply directly to the control
- **Define in:** Choose where to store the template
  - **Application** - Available throughout the app
  - **This document** - Available only in current window
  - **Resource dictionary** - Store in separate XAML file

**Step 5: Edit the Template**

After clicking OK:
- The template is generated in the Resources section
- Blend enters template editing mode
- You can visually modify all parts of the control
- Changes appear in real-time in the designer

**Generated XAML Location:**
- Appears in `<Window.Resources>` or `App.xaml` depending on your choice
- Can be viewed and edited in XAML view
- Includes all visual elements of the Split Button

### Create Empty Option

This option creates a minimal template that you build from scratch.

**When to Use:**
- Creating completely custom designs
- No resemblance to default appearance needed
- Full control over visual structure

**Process:**
- Opens same "Create ControlTemplate Resource" dialog
- Generates minimal template structure
- You add all visual elements manually

### Example: Customized Split Button in Blend

**Edited Template (Simplified):**

```xaml
<Window.Resources>
    <Style x:Key="CustomSplitButtonStyle" TargetType="syncfusion:SplitButtonAdv">
        <Setter Property="Background" Value="#FF0078D7"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="FontWeight" Value="Bold"/>
        <Setter Property="BorderBrush" Value="#FF005A9E"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Padding" Value="10,5"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="syncfusion:SplitButtonAdv">
                    <!-- Custom visual tree here -->
                    <Border Background="{TemplateBinding Background}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}"
                            CornerRadius="5">
                        <ContentPresenter/>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<syncfusion:SplitButtonAdv Label="Custom Button" 
                            Style="{StaticResource CustomSplitButtonStyle}"/>
```

## Editing Appearance in Visual Studio

Visual Studio provides XAML editing with design view support for template customization.

### Steps to Edit Template in Visual Studio

**Step 1: Open Application in Visual Studio**
- Launch Visual Studio
- Open your WPF project
- Open the XAML file containing the Split Button

**Step 2: Select the Control**
- Switch to Design view or Split view
- Click on the Split Button control in the designer
- The control becomes selected

**Step 3: Access Edit Template Menu**
- Right-click on the Split Button control
- Choose the **Edit Template** menu option
- You'll see two sub-options:
  - **Edit a Copy...** - Edit a copy of the default style
  - **Create Empty...** - Create an empty template from scratch

### Edit a Copy Option

**Step 4: Create ControlTemplate Resource Dialog**

The **Create ControlTemplate Resource** dialog appears with options:

**Dialog Fields:**
- **Name (Key):** Name for the template resource
  - Example: `SplitButtonTemplate1`
  - Optional: Leave blank for anonymous style
- **Define in:** Storage location
  - **Application** - `App.xaml`
  - **This document** - Current XAML file
  - **Resource dictionary** - External file

**Step 5: Edit the Generated Template**

After clicking OK:
- Template is generated in the specified location
- XAML view shows the complete template structure
- You can edit properties, colors, and structure in XAML
- Design view updates to reflect changes

**Template Access:**
- Located in Resources section of chosen location
- Can be reused across multiple controls
- Fully editable in XAML editor

### Create Empty Option

Creates a minimal template for complete customization from scratch.

**Generated Empty Template:**

```xaml
<Style x:Key="EmptySplitButtonStyle" TargetType="syncfusion:SplitButtonAdv">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="syncfusion:SplitButtonAdv">
                <!-- Add your custom visual tree here -->
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### Example: Custom Split Button in Visual Studio

**Complete Custom Style:**

```xaml
<Window.Resources>
    <Style x:Key="ModernSplitButtonStyle" TargetType="syncfusion:SplitButtonAdv">
        <Setter Property="Background" Value="#FF2D2D30"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="BorderBrush" Value="#FF3E3E42"/>
        <Setter Property="BorderThickness" Value="1"/>
        <Setter Property="Padding" Value="12,6"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="syncfusion:SplitButtonAdv">
                    <Grid>
                        <Border x:Name="Border"
                                Background="{TemplateBinding Background}"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="3">
                            <ContentPresenter Margin="{TemplateBinding Padding}"
                                            HorizontalAlignment="Center"
                                            VerticalAlignment="Center"/>
                        </Border>
                    </Grid>
                    <ControlTemplate.Triggers>
                        <Trigger Property="IsMouseOver" Value="True">
                            <Setter TargetName="Border" Property="Background" Value="#FF3E3E42"/>
                        </Trigger>
                        <Trigger Property="IsPressed" Value="True">
                            <Setter TargetName="Border" Property="Background" Value="#FF007ACC"/>
                        </Trigger>
                    </ControlTemplate.Triggers>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<syncfusion:SplitButtonAdv Label="Modern Button" 
                            Style="{StaticResource ModernSplitButtonStyle}"/>
```

## Applying Built-in Themes

Syncfusion provides several built-in themes that can be applied using SfSkinManager.

### Available Themes

- **MaterialLight** - Google Material Design light theme
- **MaterialDark** - Google Material Design dark theme
- **Office2019Colorful** - Microsoft Office 2019 colorful theme
- **Office2019Black** - Microsoft Office 2019 dark theme
- **Office2019White** - Microsoft Office 2019 light theme
- **Office2019HighContrast** - High contrast theme
- **FluentLight** - Microsoft Fluent Design light theme
- **FluentDark** - Microsoft Fluent Design dark theme
- And many more...

### Using SfSkinManager

**Step 1: Add Assembly Reference**

Add reference to:
- `Syncfusion.SfSkinManager.WPF`

**Step 2: Import Namespace**

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
```

**Step 3: Apply Theme to Window**

```xaml
<Window x:Class="ThemeExample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}"
        Title="Themed Application" Height="450" Width="800">
    <Grid>
        <syncfusion:SplitButtonAdv Label="Themed Button" 
                                    SizeMode="Normal">
            <syncfusion:DropDownMenuGroup>
                <syncfusion:DropDownMenuItem Header="Option 1"/>
                <syncfusion:DropDownMenuItem Header="Option 2"/>
                <syncfusion:DropDownMenuItem Header="Option 3"/>
            </syncfusion:DropDownMenuGroup>
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

**Apply Theme to Specific Control:**

```xaml
<syncfusion:SplitButtonAdv Label="Themed Button"
                            syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentDark}"/>
```

**Apply Theme in Code-Behind:**

```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    SfSkinManager.SetTheme(this, new Theme("MaterialDark"));
}
```

### Theme Examples

**MaterialLight Theme:**

```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    <syncfusion:SplitButtonAdv Label="Material Light Button"/>
</Window>
```

**Office2019Colorful Theme:**

```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=Office2019Colorful}">
    <syncfusion:SplitButtonAdv Label="Office 2019 Button"/>
</Window>
```

**FluentDark Theme:**

```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentDark}">
    <syncfusion:SplitButtonAdv Label="Fluent Dark Button"/>
</Window>
```

## Creating Custom Themes

ThemeStudio is an online tool for creating custom themes for Syncfusion controls.

### Using ThemeStudio

**Step 1: Access ThemeStudio**

Visit: [https://help.syncfusion.com/wpf/themes/theme-studio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)

**Step 2: Choose Base Theme**

- Select a base theme to start from
- Options: Material, Fluent, Office, Bootstrap, etc.

**Step 3: Customize Colors**

- Primary color
- Accent color
- Background colors
- Text colors
- Border colors

**Step 4: Preview**

- See real-time preview of controls
- Test different controls with your theme
- Adjust colors as needed

**Step 5: Export Theme**

- Download the generated theme files
- Include theme assemblies in your project
- Apply the custom theme using SfSkinManager

### Custom Theme Example

**Generated Custom Theme:**

```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=CustomTheme}">
    <Grid>
        <syncfusion:SplitButtonAdv Label="Custom Themed Button"/>
    </Grid>
</Window>
```

**Applying Custom Theme in Code:**

```csharp
using Syncfusion.SfSkinManager;
using Syncfusion.Themes.CustomTheme.WPF;

public MainWindow()
{
    InitializeComponent();
    SfSkinManager.RegisterThemeSettings("CustomTheme", new CustomThemeSettings());
    SfSkinManager.SetTheme(this, new Theme("CustomTheme"));
}
```

## Best Practices

### 1. Use Resource Dictionaries

```xaml
<!-- Styles.xaml -->
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                    xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Style x:Key="AppSplitButtonStyle" TargetType="syncfusion:SplitButtonAdv">
        <!-- Style setters -->
    </Style>
</ResourceDictionary>

<!-- App.xaml -->
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="Styles.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### 2. Use BasedOn for Style Inheritance

```xaml
<Style x:Key="BaseSplitButtonStyle" TargetType="syncfusion:SplitButtonAdv">
    <Setter Property="Padding" Value="10,5"/>
    <Setter Property="FontSize" Value="14"/>
</Style>

<Style x:Key="PrimarySplitButtonStyle" 
       TargetType="syncfusion:SplitButtonAdv" 
       BasedOn="{StaticResource BaseSplitButtonStyle}">
    <Setter Property="Background" Value="Blue"/>
    <Setter Property="Foreground" Value="White"/>
</Style>
```

### 3. Use TemplateBinding

```xaml
<ControlTemplate TargetType="syncfusion:SplitButtonAdv">
    <Border Background="{TemplateBinding Background}"
            BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}">
        <!-- Content -->
    </Border>
</ControlTemplate>
```

### 4. Consistent Theming

```csharp
// Apply theme globally in App.xaml.cs
public App()
{
    SfSkinManager.ApplyStylesOnApplication = true;
    SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
}
```

### 5. Test Themes Across Controls

Ensure your custom theme works consistently across:
- Split buttons
- Regular buttons
- Dropdown menus
- Other Syncfusion controls

## Troubleshooting

**Problem:** Theme not applying  
**Solution:** Ensure SfSkinManager assembly is referenced and namespace is imported

**Problem:** Custom style not working  
**Solution:** Verify TargetType matches control type exactly

**Problem:** Template changes not visible  
**Solution:** Check if another style is overriding your template

**Problem:** ThemeStudio theme not loading  
**Solution:** Ensure theme assembly is included and registered correctly

## Summary

This guide covered:
- Editing control appearance in Expression Blend
- Editing control appearance in Visual Studio
- Applying built-in themes with SfSkinManager
- Creating custom themes with ThemeStudio
- Best practices for styling and theming
- Resource organization and style inheritance

## Related Topics

- [Getting Started](getting-started.md) - Basic setup and configuration
- [Syncfusion WPF Themes Documentation](https://help.syncfusion.com/wpf/themes/skin-manager)
- [ThemeStudio Documentation](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)
