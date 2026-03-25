# Customization and Blendability

## Table of Contents
- [Overview](#overview)
- [Expression Blend Integration](#expression-blend-integration)
- [Theme Support](#theme-support)
- [Style Customization](#style-customization)
- [Template Customization](#template-customization)
- [Color and Appearance](#color-and-appearance)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

ComboBoxAdv provides extensive customization options through Expression Blend integration, built-in themes, style editing, and template customization. This allows you to match the control's appearance to your application's design requirements.

**Customization Options:**
- Expression Blend design-time editing
- Built-in theme support via SfSkinManager
- Custom themes with ThemeStudio
- Style and template editing
- Property-based appearance customization

## Expression Blend Integration

Expression Blend is Microsoft's visual design tool for WPF applications, providing a designer-friendly interface for creating and customizing controls.

### Creating ComboBoxAdv in Expression Blend

**Step 1: Create New Project**

1. Open Expression Blend
2. File → New Project
3. Select "WPF Application"
4. Enter project name and click OK

**Step 2: Add ComboBoxAdv from Assets**

1. Window → Assets (or press Ctrl+Period)
2. In the Search box, type "ComboBoxAdv"
3. Drag ComboBoxAdv from Assets Library to the design surface

**Result:** ComboBoxAdv instance is created with automatic assembly references.

### Editing Style in Expression Blend

**Step 1: Access Template**

1. Right-click the ComboBoxAdv control in the design surface
2. Select "Edit Template" → "Edit a Copy..."

![Edit Template Menu](images/edit-template-menu.png)

**Step 2: Create Style Resource**

1. In the "Create Style Resource" dialog:
   - **Name (Key):** Enter a name (e.g., "CustomComboBoxStyle")
   - **Define in:** Choose where to save (Application, This document, Resource Dictionary)
2. Click OK

**Step 3: Customize Template**

The template editor opens, showing the control's visual tree. You can now:
- Modify colors, fonts, and sizes
- Rearrange visual elements
- Add animations and triggers
- Customize states (Normal, MouseOver, Pressed, etc.)

**Step 4: Apply Custom Style**

```xml
<syncfusion:ComboBoxAdv Style="{StaticResource CustomComboBoxStyle}"/>
```

### Blend Benefits

- **Visual editing**: See changes in real-time
- **Animation timeline**: Create smooth transitions
- **State management**: Design different visual states
- **Resource management**: Organize styles and templates

## Theme Support

ComboBoxAdv supports Syncfusion's theme framework for consistent application-wide styling.

### Using SfSkinManager

SfSkinManager applies themes to controls automatically.

**Step 1: Add Assembly Reference**

```
Syncfusion.SfSkinManager.WPF
```

**Step 2: Import Namespace**

```xml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
```

**Step 3: Apply Theme**

#### Window-Level Theme

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <syncfusionskin:SfSkinManager.Theme>
        <syncfusionskin:Theme ThemeName="MaterialLight"/>
    </syncfusionskin:SfSkinManager.Theme>
    
    <Grid>
        <syncfusion:ComboBoxAdv Height="30" Width="200"/>
    </Grid>
</Window>
```

#### Control-Level Theme

```xml
<syncfusion:ComboBoxAdv syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialDark}"/>
```

#### Code-Behind Application

```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

Or for specific control:

```csharp
SfSkinManager.SetTheme(comboBoxAdv, new Theme("FluentDark"));
```

### Available Built-In Themes

| Theme Name | Description |
|------------|-------------|
| **MaterialLight** | Google Material Design light theme |
| **MaterialDark** | Google Material Design dark theme |
| **MaterialLightBlue** | Material with blue accent |
| **MaterialDarkBlue** | Material dark with blue accent |
| **FluentLight** | Microsoft Fluent Design light |
| **FluentDark** | Microsoft Fluent Design dark |
| **Office2019Colorful** | Office 2019 colorful theme |
| **Office2019Black** | Office 2019 black theme |
| **Office2019White** | Office 2019 white theme |
| **Office2019DarkGray** | Office 2019 dark gray theme |
| **Office2019HighContrast** | Office 2019 high contrast |
| **Office2019HighContrastWhite** | Office 2019 high contrast white |
| **Office2016Colorful** | Office 2016 colorful theme |
| **Office2016White** | Office 2016 white theme |
| **Office2016DarkGray** | Office 2016 dark gray |
| **Office2016Black** | Office 2016 black theme |
| **VisualStudio2015** | Visual Studio 2015 theme |
| **VisualStudio2013** | Visual Studio 2013 theme |
| **Metro** | Windows Metro design |
| **Lime** | Lime green accent theme |
| **Saffron** | Saffron yellow accent theme |

### Theme Examples

#### Material Light

```xml
<syncfusionskin:SfSkinManager.Theme>
    <syncfusionskin:Theme ThemeName="MaterialLight"/>
</syncfusionskin:SfSkinManager.Theme>

<syncfusion:ComboBoxAdv ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        Height="30" Width="200"/>
```

#### Fluent Dark

```xml
<syncfusionskin:SfSkinManager.Theme>
    <syncfusionskin:Theme ThemeName="FluentDark"/>
</syncfusionskin:SfSkinManager.Theme>

<syncfusion:ComboBoxAdv ItemsSource="{Binding Products}"
                        DisplayMemberPath="ProductName"/>
```

#### Office 2019 Colorful

```xml
<syncfusionskin:SfSkinManager.Theme>
    <syncfusionskin:Theme ThemeName="Office2019Colorful"/>
</syncfusionskin:SfSkinManager.Theme>

<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        EnableToken="True"/>
```

## Creating Custom Themes with ThemeStudio

ThemeStudio is Syncfusion's online tool for creating custom color schemes.

### Using ThemeStudio

**Step 1: Access ThemeStudio**

Visit: [https://help.syncfusion.com/wpf/themes/theme-studio](https://help.syncfusion.com/wpf/themes/theme-studio)

**Step 2: Select Base Theme**

Choose a base theme to customize (e.g., MaterialLight, FluentLight)

**Step 3: Customize Colors**

- Primary color
- Secondary color
- Accent colors
- Background colors
- Text colors
- Border colors

**Step 4: Preview**

Preview your theme across different Syncfusion controls

**Step 5: Export**

Download the custom theme assembly or resource dictionary

**Step 6: Apply to Application**

```xml
<syncfusionskin:SfSkinManager.Theme>
    <syncfusionskin:Theme ThemeName="MyCustomTheme"/>
</syncfusionskin:SfSkinManager.Theme>
```

### Custom Theme Benefits

- **Brand consistency**: Match your company colors
- **Unique appearance**: Stand out from default themes
- **Accessibility**: Adjust contrast and colors for accessibility
- **Professional look**: Polish your application's appearance

## Style Customization

Customize individual properties without editing templates.

### Property-Based Customization

```xml
<syncfusion:ComboBoxAdv Height="35" 
                        Width="250"
                        FontFamily="Segoe UI"
                        FontSize="14"
                        FontWeight="SemiBold"
                        Foreground="#333333"
                        Background="#F5F5F5"
                        BorderBrush="#CCCCCC"
                        BorderThickness="1"
                        Padding="10,5"/>
```

### Style with Setters

```xml
<Window.Resources>
    <Style x:Key="CustomComboBoxStyle" TargetType="syncfusion:ComboBoxAdv">
        <Setter Property="Height" Value="35"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="#333333"/>
        <Setter Property="Background" Value="#F5F5F5"/>
        <Setter Property="BorderBrush" Value="#0078D7"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Padding" Value="8,4"/>
    </Style>
</Window.Resources>

<syncfusion:ComboBoxAdv Style="{StaticResource CustomComboBoxStyle}"/>
```

### Hover and Focus Styles

```xml
<Style x:Key="InteractiveComboBox" TargetType="syncfusion:ComboBoxAdv">
    <Setter Property="BorderBrush" Value="#CCCCCC"/>
    <Setter Property="BorderThickness" Value="1"/>
    <Style.Triggers>
        <Trigger Property="IsMouseOver" Value="True">
            <Setter Property="BorderBrush" Value="#0078D7"/>
            <Setter Property="BorderThickness" Value="2"/>
        </Trigger>
        <Trigger Property="IsFocused" Value="True">
            <Setter Property="BorderBrush" Value="#0078D7"/>
            <Setter Property="BorderThickness" Value="2"/>
        </Trigger>
    </Style.Triggers>
</Style>
```

## Template Customization

For advanced customization, edit the control template.

### Extracting Template

**In Visual Studio:**

1. Right-click ComboBoxAdv in XAML designer
2. Edit Template → Edit a Copy...
3. Choose where to save the template

**Template Structure:**

```xml
<Style x:Key="CustomTemplate" TargetType="syncfusion:ComboBoxAdv">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="syncfusion:ComboBoxAdv">
                <!-- Border -->
                <Border Background="{TemplateBinding Background}"
                        BorderBrush="{TemplateBinding BorderBrush}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <!-- Content -->
                    <Grid>
                        <!-- Text display area -->
                        <ContentPresenter/>
                        <!-- Dropdown button -->
                        <ToggleButton/>
                        <!-- Popup -->
                        <Popup>
                            <!-- Dropdown items -->
                        </Popup>
                    </Grid>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### Custom Dropdown Button

```xml
<ControlTemplate TargetType="syncfusion:ComboBoxAdv">
    <Border>
        <Grid>
            <ContentPresenter/>
            <!-- Custom dropdown button -->
            <Button Width="30" 
                    HorizontalAlignment="Right"
                    Background="#0078D7"
                    BorderThickness="0">
                <Path Data="M 0 0 L 4 4 L 8 0 Z" 
                      Fill="White"
                      Stretch="Uniform"
                      Width="10" Height="6"/>
            </Button>
        </Grid>
    </Border>
</ControlTemplate>
```

## Color and Appearance

### Customizing Colors

```xml
<syncfusion:ComboBoxAdv Foreground="#1E88E5"           <!-- Text color -->
                        Background="#FFFFFF"           <!-- Background -->
                        BorderBrush="#42A5F5"         <!-- Border -->
                        SelectionBrush="#BBDEFB">     <!-- Selection highlight -->
</syncfusion:ComboBoxAdv>
```

### Font Customization

```xml
<syncfusion:ComboBoxAdv FontFamily="Segoe UI"
                        FontSize="14"
                        FontWeight="SemiBold"
                        FontStyle="Normal"/>
```

### Sizing and Spacing

```xml
<syncfusion:ComboBoxAdv Height="40"
                        Width="300"
                        Padding="12,6"
                        Margin="10"/>
```

### Border Customization

```xml
<syncfusion:ComboBoxAdv BorderBrush="#0078D7"
                        BorderThickness="2"
                        CornerRadius="4"/>  <!-- If supported -->
```

## Best Practices

### 1. Use Themes for Consistency

Apply themes at application level for consistent look:

```xml
<Application>
    <syncfusionskin:SfSkinManager.Theme>
        <syncfusionskin:Theme ThemeName="MaterialLight"/>
    </syncfusionskin:SfSkinManager.Theme>
</Application>
```

### 2. Create Reusable Styles

```xml
<Window.Resources>
    <Style x:Key="StandardComboBox" TargetType="syncfusion:ComboBoxAdv">
        <Setter Property="Height" Value="30"/>
        <Setter Property="FontSize" Value="14"/>
    </Style>
</Window.Resources>

<!-- Apply to multiple controls -->
<syncfusion:ComboBoxAdv Style="{StaticResource StandardComboBox}"/>
<syncfusion:ComboBoxAdv Style="{StaticResource StandardComboBox}"/>
```

### 3. Test Accessibility

Ensure sufficient color contrast:

```xml
<!-- ✅ Good contrast -->
<syncfusion:ComboBoxAdv Foreground="#000000" Background="#FFFFFF"/>

<!-- ❌ Poor contrast -->
<syncfusion:ComboBoxAdv Foreground="#CCCCCC" Background="#DDDDDD"/>
```

### 4. Maintain Visual States

When customizing templates, preserve all visual states:
- Normal
- MouseOver
- Pressed
- Disabled
- Focused

### 5. Use ThemeStudio for Complex Customization

For brand-specific requirements, ThemeStudio provides comprehensive customization without manual template editing.

## Common Customization Examples

### Example 1: Modern Flat Design

```xml
<syncfusion:ComboBoxAdv Height="35" Width="250"
                        FontSize="13"
                        Foreground="#212121"
                        Background="#FAFAFA"
                        BorderBrush="#E0E0E0"
                        BorderThickness="0,0,0,2"
                        Padding="8,6"/>
```

### Example 2: Bold Accent

```xml
<syncfusion:ComboBoxAdv Height="32" Width="220"
                        FontWeight="SemiBold"
                        Foreground="#FFFFFF"
                        Background="#0078D7"
                        BorderThickness="0"/>
```

### Example 3: Subtle Rounded

```xml
<Border CornerRadius="4" BorderBrush="#CCCCCC" BorderThickness="1">
    <syncfusion:ComboBoxAdv Height="30" Width="200"
                            BorderThickness="0"
                            Background="Transparent"/>
</Border>
```

### Example 4: Dark Theme Compatible

```xml
<syncfusion:ComboBoxAdv Foreground="#E0E0E0"
                        Background="#2D2D30"
                        BorderBrush="#3F3F46"
                        BorderThickness="1"/>
```

## Troubleshooting

### Issue: Theme Not Applying

**Cause:** SfSkinManager assembly not referenced or namespace not imported

**Solution:**
1. Add `Syncfusion.SfSkinManager.WPF` reference
2. Import namespace
3. Rebuild project

### Issue: Custom Style Not Working

**Cause:** Style resource not found or TargetType mismatch

**Solution:**
```xml
<!-- Ensure TargetType matches -->
<Style TargetType="syncfusion:ComboBoxAdv" x:Key="MyStyle">
...
</Style>

<!-- Apply with correct key -->
<syncfusion:ComboBoxAdv Style="{StaticResource MyStyle}"/>
```

### Issue: Template Override Breaks Functionality

**Cause:** Required template parts missing or incorrectly named

**Solution:**
- Start with extracted template from default style
- Modify gradually, testing after each change
- Ensure all required parts (PART_*) are present

### Issue: Colors Not Visible in Dark Theme

**Cause:** Hardcoded colors not compatible with theme

**Solution:**
Use theme-aware colors or dynamic resources:

```xml
<syncfusion:ComboBoxAdv Foreground="{DynamicResource PrimaryForeground}"
                        Background="{DynamicResource PrimaryBackground}"/>
```

## See Also

- [Getting Started](getting-started.md) - Basic setup
- [Syncfusion Themes Documentation](https://help.syncfusion.com/wpf/themes/getting-started)
- [ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio)
- [Expression Blend for WPF](https://learn.microsoft.com/en-us/previous-versions/windows/apps/ff754511(v=expression.40))
