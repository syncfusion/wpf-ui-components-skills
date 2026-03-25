# Styles and Templates in WPF DataPager

## Table of Contents
- [Overview](#overview)
- [WPF Styles and Templates Basics](#wpf-styles-and-templates-basics)
- [Editing Appearance in Expression Blend](#editing-appearance-in-expression-blend)
- [Editing Appearance in Visual Studio](#editing-appearance-in-visual-studio)
- [Custom Style Examples](#custom-style-examples)
- [Best Practices](#best-practices)

## Overview

WPF Styles and Templates provide a powerful suite of features that allow developers and designers to create visually compelling effects and ensure consistent appearance across applications. SfDataPager fully supports WPF's styling and templating system, enabling you to customize its visual appearance to match your application's design.

This guide explains how to edit the visual structure and appearance of SfDataPager using both Expression Blend and Visual Studio.

## WPF Styles and Templates Basics

### What Are Styles?

Styles are collections of property setters that can be applied to controls to change their appearance. Styles provide:
- Consistent appearance across multiple controls
- Centralized design management
- Reduced XAML redundancy
- Easy theme switching

### What Are Templates?

Templates define the visual structure of a control. Control templates allow you to:
- Completely redesign control appearance
- Add or remove visual elements
- Change control layout
- Create custom animations and transitions

### Style vs Template

| Feature | Style | Template |
|---------|-------|----------|
| **Purpose** | Change properties | Change structure |
| **Scope** | Property values | Visual tree |
| **Complexity** | Simple | Complex |
| **Use case** | Colors, fonts, sizes | Layout, animations |

## Editing Appearance in Expression Blend

Expression Blend provides a visual designer for editing styles and templates. Follow these steps to customize SfDataPager appearance in Blend.

### Step 1: Open Project in Expression Blend

1. Launch Expression Blend
2. Open your WPF application project
3. Navigate to the window or user control containing SfDataPager

### Step 2: Select SfDataPager Control

1. Click on the SfDataPager control in the designer
2. The control will be highlighted with selection handles
3. Verify the control is selected in the Objects and Timeline panel

### Step 3: Access Edit Style Menu

1. Go to the menu bar
2. Choose **Object** → **Edit Style**
3. Two options appear in the submenu:
   - **Edit a Copy** - Creates an editable copy of the default style
   - **Create Empty** - Creates a new empty style

### Option A: Edit a Copy

This option creates a copy of the default SfDataPager style that you can modify.

**Steps:**

1. Select **Object** → **Edit Style** → **Edit a Copy**
2. The **Create Style Resource** dialog opens

**Create Style Resource Dialog:**

![Create Style Resource Dialog](create-style-resource.png)

**Dialog Options:**
- **Key (Name)**: Enter a name for your style
  - Use descriptive names (e.g., "CustomDataPagerStyle")
  - Leave blank for default style applied to all SfDataPager controls
- **Define in**: Choose where to store the style
  - **Application**: Available throughout entire application
  - **This document**: Only in current window/user control
  - **Resource dictionary**: In a separate resource file (recommended for large projects)

3. Click **OK**

**Result:**

Expression Blend generates the complete style XAML in the Resources section. You can now:
- Edit property values in the Properties panel
- Modify the visual tree in the Objects and Timeline panel
- Add animations and transitions
- Preview changes in real-time

### Option B: Create Empty

This option creates a new empty style with no predefined properties.

**Steps:**

1. Select **Object** → **Edit Style** → **Create Empty**
2. The **Create Style Resource** dialog opens (same as Edit a Copy)
3. Enter style name and location
4. Click **OK**

**Result:**

Blend creates a minimal style:

```xml
<Style x:Key="CustomDataPagerStyle" TargetType="datapager:SfDataPager">
    <!-- Add your setters here -->
</Style>
```

You build the style from scratch by adding property setters.

### Editing the Style in Blend

Once in style editing mode:

1. **Properties Panel**: Modify visual properties (colors, fonts, sizes)
2. **Objects and Timeline**: View and edit visual tree structure
3. **Resources Panel**: Manage brushes, colors, and other resources
4. **Assets Panel**: Add additional controls to template
5. **XAML View**: Edit XAML directly for advanced customization

**Common Modifications:**
- Change AccentBackground and AccentForeground colors
- Modify button shapes and sizes
- Add custom borders or backgrounds
- Change font properties
- Add visual effects (shadows, gradients)

### Saving and Applying

1. **Save changes**: File → Save or Ctrl+S
2. **Exit editing mode**: Click the breadcrumb at top to return to main design view
3. **Apply style**: If you created a keyed style, apply it:

```xml
<datapager:SfDataPager Style="{StaticResource CustomDataPagerStyle}"/>
```

## Editing Appearance in Visual Studio

Visual Studio provides similar style editing capabilities through its XAML designer.

### Step 1: Open Project in Visual Studio

1. Launch Visual Studio
2. Open your WPF application project
3. Open the XAML file containing SfDataPager

### Step 2: Select SfDataPager in Design View

1. Switch to Design view (or Split view)
2. Click on the SfDataPager control in the designer
3. The control will be highlighted with selection handles

### Step 3: Access Edit Template Menu

1. Right-click on the SfDataPager control
2. From the context menu, select **Edit Template**
3. Two options appear in the submenu:
   - **Edit a Copy** - Creates an editable copy of the default style
   - **Create Empty** - Creates a new empty style

### Option A: Edit a Copy

**Steps:**

1. Right-click SfDataPager → **Edit Template** → **Edit a Copy**
2. The **Create Style Resource** dialog opens

**Create Style Resource Dialog:**

![Visual Studio Create Style Resource](vs-create-style-resource.png)

**Dialog Options:**
- **Key (Name)**: Enter a name for your style
  - Example: "BlueDataPagerStyle", "CompactDataPagerStyle"
- **Define in**: Choose resource location
  - **Application**: app.xaml (global)
  - **This document**: Current window (local)
  - **Resource dictionary**: Separate file

3. Click **OK**

**Result:**

Visual Studio generates the complete default style in the specified location. The XAML editor switches to show the style markup, and you can:
- Edit XAML directly
- Use Properties window to modify values
- Add or remove setters
- Modify control template structure

### Option B: Create Empty

**Steps:**

1. Right-click SfDataPager → **Edit Template** → **Create Empty**
2. The **Create Style Resource** dialog opens
3. Enter style name and select location
4. Click **OK**

**Result:**

Visual Studio creates an empty style template that you can populate:

```xml
<Style x:Key="EmptyDataPagerStyle" TargetType="{x:Type datapager:SfDataPager}">
    <!-- Define your setters here -->
</Style>
```

### Editing XAML in Visual Studio

Once the style is generated, you can edit it directly in XAML:

**Properties Window:**
- Select elements in the style
- Modify properties using the Properties panel
- Visual Studio updates XAML automatically

**XAML Editor:**
- Direct text editing for precise control
- IntelliSense for property discovery
- Syntax highlighting and validation
- Real-time error checking

**Design Preview:**
- See changes in Design view
- Test different states (hover, pressed, disabled)
- Verify layout and alignment

## Custom Style Examples

### Example 1: Custom Colors

```xml
<Window.Resources>
    <Style x:Key="CustomColorDataPagerStyle" TargetType="datapager:SfDataPager">
        <Setter Property="AccentBackground" Value="#FF2196F3"/>
        <Setter Property="AccentForeground" Value="White"/>
        <Setter Property="Background" Value="#FFF5F5F5"/>
        <Setter Property="BorderBrush" Value="#FFBDBDBD"/>
        <Setter Property="BorderThickness" Value="1"/>
        <Setter Property="Padding" Value="10"/>
    </Style>
</Window.Resources>

<datapager:SfDataPager Style="{StaticResource CustomColorDataPagerStyle}"
                       Source="{Binding DataCollection}"/>
```

### Example 2: Custom Button Style

```xml
<Window.Resources>
    <Style x:Key="RoundedButtonDataPagerStyle" TargetType="datapager:SfDataPager">
        <Setter Property="AccentBackground" Value="DodgerBlue"/>
        <Setter Property="AccentForeground" Value="White"/>
        <Style.Resources>
            <Style TargetType="datapager:NumericButton">
                <Setter Property="BorderBrush" Value="DodgerBlue"/>
                <Setter Property="BorderThickness" Value="2"/>
                <Setter Property="Margin" Value="2"/>
                <Setter Property="MinWidth" Value="30"/>
                <Setter Property="MinHeight" Value="30"/>
                <Setter Property="FontWeight" Value="Bold"/>
                <Setter Property="Background" Value="White"/>
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="datapager:NumericButton">
                            <Border Background="{TemplateBinding Background}"
                                    BorderBrush="{TemplateBinding BorderBrush}"
                                    BorderThickness="{TemplateBinding BorderThickness}"
                                    CornerRadius="15"
                                    Margin="{TemplateBinding Margin}">
                                <ContentPresenter HorizontalAlignment="Center"
                                                  VerticalAlignment="Center"/>
                            </Border>
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Style.Resources>
    </Style>
</Window.Resources>
```

### Example 3: Compact Style

```xml
<Window.Resources>
    <Style x:Key="CompactDataPagerStyle" TargetType="datapager:SfDataPager">
        <Setter Property="AccentBackground" Value="#FF607D8B"/>
        <Setter Property="AccentForeground" Value="White"/>
        <Setter Property="FontSize" Value="11"/>
        <Setter Property="Height" Value="28"/>
        <Style.Resources>
            <Style TargetType="datapager:NumericButton">
                <Setter Property="MinWidth" Value="24"/>
                <Setter Property="MinHeight" Value="24"/>
                <Setter Property="Margin" Value="1"/>
                <Setter Property="FontSize" Value="10"/>
            </Style>
        </Style.Resources>
    </Style>
</Window.Resources>

<datapager:SfDataPager Style="{StaticResource CompactDataPagerStyle}"
                       PageSize="20"
                       Source="{Binding DataCollection}"/>
```

### Example 4: Dark Theme Style

```xml
<Window.Resources>
    <Style x:Key="DarkDataPagerStyle" TargetType="datapager:SfDataPager">
        <Setter Property="Background" Value="#FF212121"/>
        <Setter Property="Foreground" Value="#FFFFFFFF"/>
        <Setter Property="AccentBackground" Value="#FF00BCD4"/>
        <Setter Property="AccentForeground" Value="#FF000000"/>
        <Setter Property="BorderBrush" Value="#FF424242"/>
        <Setter Property="BorderThickness" Value="1"/>
        <Style.Resources>
            <Style TargetType="datapager:NumericButton">
                <Setter Property="Foreground" Value="White"/>
                <Setter Property="Background" Value="#FF424242"/>
                <Setter Property="BorderBrush" Value="#FF616161"/>
                <Setter Property="BorderThickness" Value="1"/>
            </Style>
        </Style.Resources>
    </Style>
</Window.Resources>
```

### Example 5: Gradient Background

```xml
<Window.Resources>
    <LinearGradientBrush x:Key="DataPagerGradient" StartPoint="0,0" EndPoint="1,0">
        <GradientStop Color="#FF6A11CB" Offset="0"/>
        <GradientStop Color="#FF2575FC" Offset="1"/>
    </LinearGradientBrush>
    
    <Style x:Key="GradientDataPagerStyle" TargetType="datapager:SfDataPager">
        <Setter Property="Background" Value="{StaticResource DataPagerGradient}"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="AccentBackground" Value="White"/>
        <Setter Property="AccentForeground" Value="#FF6A11CB"/>
        <Setter Property="BorderThickness" Value="0"/>
        <Setter Property="Padding" Value="15,10"/>
    </Style>
</Window.Resources>
```

## Best Practices

### Resource Organization

**Use Resource Dictionaries:**
```xml
<!-- Themes/DataPagerStyles.xaml -->
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                    xmlns:datapager="clr-namespace:Syncfusion.UI.Xaml.Controls.DataPager;assembly=Syncfusion.SfGrid.WPF">
    
    <Style x:Key="DefaultDataPagerStyle" TargetType="datapager:SfDataPager">
        <!-- Style definition -->
    </Style>
    
    <Style x:Key="CompactDataPagerStyle" TargetType="datapager:SfDataPager">
        <!-- Style definition -->
    </Style>
</ResourceDictionary>
```

**Merge in Application:**
```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="Themes/DataPagerStyles.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Naming Conventions

- Use descriptive names: `BlueDataPagerStyle`, `CompactDataPagerStyle`
- Avoid generic names: `Style1`, `MyStyle`
- Include purpose: `ReportViewerDataPagerStyle`
- Be consistent across your application

### Style Reusability

**Base Styles:**
```xml
<Style x:Key="BaseDataPagerStyle" TargetType="datapager:SfDataPager">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontSize" Value="12"/>
</Style>

<Style x:Key="BlueDataPagerStyle" 
       TargetType="datapager:SfDataPager" 
       BasedOn="{StaticResource BaseDataPagerStyle}">
    <Setter Property="AccentBackground" Value="Blue"/>
</Style>
```

### Testing

- Test all button states (normal, hover, pressed, disabled)
- Verify appearance at different DPI settings
- Check both light and dark themes
- Test with different window sizes
- Validate accessibility (contrast ratios)

### Performance

- Avoid complex templates for frequently used controls
- Use static resources (not dynamic) when possible
- Minimize trigger usage
- Cache brushes and resources
- Profile application for style-related performance issues

### Maintenance

- Document custom styles
- Version control resource dictionaries
- Keep styles synchronized across application
- Update styles when upgrading Syncfusion versions
- Create style guide documentation

## Troubleshooting

### Style not applying
- Check TargetType matches control type exactly
- Verify resource key is spelled correctly
- Ensure resource is in scope (check MergedDictionaries)
- Use x:Key for explicit styles, omit for implicit styles

### Template editing errors
- Verify all required template parts are present
- Check TargetBinding syntax
- Ensure namespaces are declared
- Validate XAML syntax (no errors in Error List)

### Visual Studio/Blend differences
- Some features may differ between tools
- Save and reload project if changes don't appear
- Check XAML directly if designer doesn't update
- Clear designer cache if issues persist

### Upgrade issues
- Backup custom styles before upgrading Syncfusion
- Review migration guide for breaking changes
- Reapply customizations to new default template
- Test thoroughly after upgrades
