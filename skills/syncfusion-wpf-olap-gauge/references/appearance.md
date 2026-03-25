# Appearance in OLAP Gauge

Comprehensive guide for customizing the visual appearance of OLAP Gauge including radius, frame types, and theming.

## Table of Contents

- [Overview](#overview)
- [Gauge Radius](#gauge-radius)
- [Built-in Frame Types](#built-in-frame-types)
- [Theming with Skins](#theming-with-skins)
- [Applying Themes](#applying-themes)
- [Theme Combinations](#theme-combinations)
- [Custom Appearance](#custom-appearance)
- [Best Practices](#best-practices)

## Overview

The OLAP Gauge provides extensive appearance customization through:
- **Radius adjustment**: Control gauge size
- **Frame types**: Select from built-in frame styles
- **Skins/Themes**: Apply professional color schemes
- **Visual styles**: Match application design language

## Gauge Radius

The radius property controls the overall size of each gauge.

### Radius Property

**Property:** `Radius`  
**Type:** `double`  
**Default:** Auto-calculated based on available space  
**Unit:** Device-independent pixels (DIP)

### Setting Radius in XAML

```xml
<syncfusion:OlapGauge Name="olapGauge1" Radius="120"/>
```

### Setting Radius in Code

**C#:**

```csharp
this.OlapGauge1.Radius = 100;
```

**VB.NET:**

```vb
Me.OlapGauge1.Radius = 100
```

### Radius Values

**Small gauges (80-100):**

```csharp
this.OlapGauge1.Radius = 80;
```

Use for: Dense dashboards with many KPIs

**Medium gauges (100-150):**

```csharp
this.OlapGauge1.Radius = 120;
```

Use for: Standard dashboards, balanced view

**Large gauges (150-200):**

```csharp
this.OlapGauge1.Radius = 180;
```

Use for: Feature displays, presentation mode

**Extra large gauges (200+):**

```csharp
this.OlapGauge1.Radius = 250;
```

Use for: Single KPI focus, kiosk displays

### Radius Result

![Gauge with custom radius](Appearance_images/Appearance_img1.png)

### Responsive Radius

Calculate radius based on available space:

```csharp
private void SetResponsiveRadius()
{
    // Calculate based on window size and gauge count
    double availableWidth = this.ActualWidth;
    double availableHeight = this.ActualHeight;
    
    int columns = this.OlapGauge1.ColumnsCount;
    int rows = this.OlapGauge1.RowsCount;
    
    double maxWidth = (availableWidth / columns) - 20;  // 20 = padding
    double maxHeight = (availableHeight / rows) - 40;   // 40 = header space
    
    double radius = Math.Min(maxWidth, maxHeight) / 2;
    
    this.OlapGauge1.Radius = Math.Max(radius, 60);  // Minimum 60
}
```

### Auto-Sizing

Omit `Radius` property to let the control auto-calculate:

```xml
<!-- Auto-sized gauge -->
<syncfusion:OlapGauge x:Name="olapGauge1"/>
```

Control calculates optimal radius based on:
- Available container space
- Number of gauges (RowsCount × ColumnsCount)
- Gauge headers and labels

## Built-in Frame Types

Frame types provide different visual rim styles for the gauge.

### FrameType Property

**Property:** `FrameType`  
**Type:** `GaugeFrameType` (enum)  
**Default:** System default frame

### Available Frame Types

#### 1. CircularCenterGradient

Circular frame with gradient from center.

```csharp
this.OlapGauge1.FrameType = GaugeFrameType.CircularCenterGradient;
```

**Characteristics:**
- Soft gradient radiating from center
- Modern, subtle appearance
- Good for light themes

![CircularCenterGradient frame](Appearance_images/Appearance_img2.png)

#### 2. CircularWithDarkOuterFrames

Circular frame with dark outer rim.

```csharp
this.OlapGauge1.FrameType = GaugeFrameType.CircularWithDarkOuterFrames;
```

**Characteristics:**
- Bold outer border
- High contrast
- Professional appearance
- Good for emphasis

![CircularWithDarkOuterFrames frame](Appearance_images/Appearance_img3.png)

#### 3. FullCircle

Complete circular frame (360 degrees).

```csharp
this.OlapGauge1.FrameType = GaugeFrameType.FullCircle;
```

**Characteristics:**
- Traditional gauge appearance
- Full scale visibility
- Symmetrical design

![FullCircle frame](Appearance_images/Appearance_img4.png)

#### 4. HalfCircle

Semicircular frame (180 degrees).

```csharp
this.OlapGauge1.FrameType = GaugeFrameType.HalfCircle;
```

**Characteristics:**
- Compact vertical space usage
- Speedometer-like appearance
- Good for horizontal layouts

![HalfCircle frame](Appearance_images/Appearance_img5.png)

### Setting Frame Type

**XAML:**

```xml
<syncfusion:OlapGauge x:Name="olapGauge1" 
                      FrameType="CircularWithInnerLeftGradient"/>
```

**C#:**

```csharp
this.OlapGauge1.FrameType = GaugeFrameType.CircularWithInnerLeftGradient;
```

**VB.NET:**

```vb
Me.OlapGauge1.FrameType = GaugeFrameType.CircularWithInnerLeftGradient
```

### Frame Type Selection Guide

| Frame Type | Best For | Visual Impact |
|------------|----------|---------------|
| CircularCenterGradient | Modern dashboards | Subtle |
| CircularWithDarkOuterFrames | Executive dashboards | Strong |
| FullCircle | Traditional displays | Balanced |
| HalfCircle | Space-constrained layouts | Compact |

### Sample Location

Demo samples available at:

```
{system drive}:\Users\<User Name>\AppData\Local\Syncfusion\
EssentialStudio\<Version Number>\WPF\OlapGauge.WPF\Samples\
Appearance\Customization\
```

## Theming with Skins

Syncfusion provides built-in themes (skins) to match various design languages and office styles.

### Theme Architecture

Themes affect:
- Gauge colors
- Frame appearance
- Header styling
- Label formatting
- Overall color scheme

### Available Themes

#### Metro Theme

Modern, flat design inspired by Windows 8 Metro style.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Metro);
```

![Metro theme](Appearance_images/Appearance_img6.png)

**Characteristics:**
- Flat colors
- Minimal gradients
- Modern appearance
- Good for contemporary applications

#### Blend Theme

Microsoft Blend-inspired design.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Blend);
```

![Blend theme](Appearance_images/Appearance_img7.png)

**Characteristics:**
- Smooth gradients
- Professional look
- Designer-friendly
- Versatile for various applications

#### Office 2010 Black

Dark Office 2010 theme.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2010Black);
```

![Office2010Black theme](Appearance_images/Appearance_img8.png)

**Characteristics:**
- Dark background
- High contrast
- Professional
- Good for dark-themed applications

#### Office 2010 Blue

Classic blue Office 2010 theme.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2010Blue);
```

![Office2010Blue theme](Appearance_images/Appearance_img9.png)

**Characteristics:**
- Traditional blue color scheme
- Familiar to Office users
- Professional
- Default Office appearance

#### Office 2010 Silver

Silver/gray Office 2010 theme.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2010Silver);
```

![Office2010Silver theme](Appearance_images/Appearance_img10.png)

**Characteristics:**
- Neutral color palette
- Professional
- Easy on eyes
- Good for extended viewing

#### Office 2013 LightGray

Light gray Office 2013 theme.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2013LightGray);
```

![Office2013LightGray theme](Appearance_images/Appearance_img11.png)

**Characteristics:**
- Flat, modern design
- Light background
- Clean appearance
- Office 2013 style

#### Office 2013 DarkGray

Dark gray Office 2013 theme.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2013DarkGray);
```

![Office2013DarkGray theme](Appearance_images/Appearance_img12.png)

**Characteristics:**
- Dark color scheme
- Modern flat design
- Reduced eye strain
- Office 2013 dark mode

#### Office 2013 White

White Office 2013 theme.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2013White);
```

![Office2013White theme](Appearance_images/Appearance_img13.png)

**Characteristics:**
- Bright, clean appearance
- Minimal colors
- Maximum contrast
- Modern minimalist

#### VisualStudio2013 Theme

Visual Studio 2013-inspired theme.

```csharp
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.VisualStudio2013);
```

![VisualStudio2013 theme](Appearance_images/Appearance_img14.png)

**Characteristics:**
- Developer-focused design
- Subtle colors
- Professional
- IDE-inspired appearance

## Applying Themes

### Method 1: XAML with SkinStorage (Legacy)

```xml
<Window xmlns:sfshared="http://schemas.syncfusion.com/wpf">
    <syncfusion:OlapGauge Name="olapGauge1" 
                          sfshared:SkinStorage.VisualStyle="Metro"/>
</Window>
```

### Method 2: Code with SfSkinManager (Recommended)

**C#:**

```csharp
using Syncfusion.SfSkinManager;

// In constructor or loaded event
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Metro);
```

**VB.NET:**

```vb
Imports Syncfusion.SfSkinManager

' In constructor or loaded event
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Metro)
```

### Method 3: Application-Wide Theme

Apply theme to entire application:

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        base.OnStartup(e);
        
        // Set application-wide theme
        SfSkinManager.ApplyStylesOnApplication = true;
        SfSkinManager.SetVisualStyle(this, VisualStyles.Office2013White);
    }
}
```

### Method 4: Window-Level Theme

Apply theme to window and all children:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Theme applies to entire window
        SfSkinManager.SetVisualStyle(this, VisualStyles.Metro);
    }
}
```

### Required Namespace

```csharp
using Syncfusion.SfSkinManager;
```

XAML:

```xml
xmlns:sfshared="http://schemas.syncfusion.com/wpf"
```

## Theme Combinations

### Theme with Frame Type

Combine theme and frame for custom appearance:

```xml
<syncfusion:OlapGauge Name="olapGauge1" 
                      FrameType="HalfCircle"
                      sfshared:SkinStorage.VisualStyle="Metro"/>
```

```csharp
this.OlapGauge1.FrameType = GaugeFrameType.HalfCircle;
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Metro);
```

### Theme with Radius

```csharp
this.OlapGauge1.Radius = 150;
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2013DarkGray);
```

### Complete Appearance Configuration

```xml
<syncfusion:OlapGauge Name="olapGauge1"
                      Radius="120"
                      FrameType="CircularWithDarkOuterFrames"
                      ShowGaugeHeaders="True"
                      ShowGaugeLabels="True"
                      sfshared:SkinStorage.VisualStyle="Office2013LightGray"/>
```

```csharp
this.OlapGauge1.Radius = 120;
this.OlapGauge1.FrameType = GaugeFrameType.CircularWithDarkOuterFrames;
this.OlapGauge1.ShowGaugeHeaders = true;
this.OlapGauge1.ShowGaugeLabels = true;
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2013LightGray);
```

## Custom Appearance

### Runtime Theme Switching

Allow users to change themes dynamically:

```csharp
private void ApplyTheme(string themeName)
{
    VisualStyles theme = themeName switch
    {
        "Metro" => VisualStyles.Metro,
        "Office2013White" => VisualStyles.Office2013White,
        "Office2013DarkGray" => VisualStyles.Office2013DarkGray,
        "Office2010Blue" => VisualStyles.Office2010Blue,
        _ => VisualStyles.Metro
    };
    
    SfSkinManager.SetVisualStyle(this.olapGauge1, theme);
}

// Usage with ComboBox
private void ThemeComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string selectedTheme = (e.AddedItems[0] as ComboBoxItem).Content.ToString();
    ApplyTheme(selectedTheme);
}
```

### Theme Persistence

Save and restore user's theme preference:

```csharp
// Save theme preference
Properties.Settings.Default.SelectedTheme = "Metro";
Properties.Settings.Default.Save();

// Restore theme on startup
private void RestoreTheme()
{
    string savedTheme = Properties.Settings.Default.SelectedTheme;
    if (!string.IsNullOrEmpty(savedTheme))
    {
        ApplyTheme(savedTheme);
    }
}
```

### Conditional Theming

Apply themes based on conditions:

```csharp
private void ApplyContextualTheme()
{
    // Dark theme for night mode
    if (DateTime.Now.Hour >= 20 || DateTime.Now.Hour < 6)
    {
        SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2013DarkGray);
    }
    else
    {
        SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2013White);
    }
}
```

## Best Practices

### 1. Theme Consistency

Apply the same theme to all controls in your application:

```csharp
// Application-wide theming
SfSkinManager.ApplyStylesOnApplication = true;
SfSkinManager.SetVisualStyle(Application.Current, VisualStyles.Metro);
```

### 2. Frame Type Selection

Choose frame types that complement your theme:

**Modern themes (Metro, Office 2013):**
- CircularCenterGradient
- HalfCircle

**Traditional themes (Office 2010, Blend):**
- FullCircle
- CircularWithDarkOuterFrames

### 3. Radius Considerations

Match radius to dashboard density:

**Dense dashboards (6+ gauges):**

```csharp
this.OlapGauge1.Radius = 80;
```

**Balanced dashboards (3-5 gauges):**

```csharp
this.OlapGauge1.Radius = 120;
```

**Focused displays (1-2 gauges):**

```csharp
this.OlapGauge1.Radius = 180;
```

### 4. High DPI Awareness

Gauges scale automatically with DPI. Test appearance on different monitors:

```csharp
// Optional: Manual DPI handling
double dpiScale = VisualTreeHelper.GetDpi(this).DpiScaleX;
this.OlapGauge1.Radius = 120 * dpiScale;
```

### 5. Dark Theme Readability

For dark themes, ensure sufficient contrast:

```csharp
// Dark themes
SfSkinManager.SetVisualStyle(olapGauge1, VisualStyles.Office2010Black);
// Consider CircularWithDarkOuterFrames for emphasis
this.OlapGauge1.FrameType = GaugeFrameType.CircularWithDarkOuterFrames;
```

### 6. Performance Considerations

- Theming is lightweight and doesn't impact performance
- Frame type changes re-render gauge (negligible impact)
- Radius changes trigger layout recalculation (still fast)

### 7. Theme Samples

Explore theme samples at:

```
{system drive}:\Users\<User Name>\AppData\Local\Syncfusion\
EssentialStudio\<Version Number>\WPF\OlapGauge.WPF\Samples\
Appearance\Visual Styles\
```

### 8. Accessibility

Consider accessibility when choosing themes:

**High contrast:**
- Office2010Black
- Office2013DarkGray with CircularWithDarkOuterFrames

**Neutral for color blindness:**
- Office2013LightGray
- Office2010Silver
- Blend

### 9. Branding Alignment

Match theme to company branding:

**Corporate/formal:** Office 2010 Blue, Office 2010 Silver  
**Modern/tech:** Metro, Office 2013 White, VisualStudio2013  
**Creative:** Blend with custom radius and frame types

### 10. User Preferences

Allow theme customization when appropriate:

```xml
<ComboBox x:Name="ThemeSelector" 
          SelectionChanged="ThemeSelector_SelectionChanged">
    <ComboBoxItem Content="Metro" IsSelected="True"/>
    <ComboBoxItem Content="Office2013White"/>
    <ComboBoxItem Content="Office2013DarkGray"/>
    <ComboBoxItem Content="Office2010Blue"/>
    <ComboBoxItem Content="VisualStudio2013"/>
</ComboBox>
```

This empowers users to choose themes that suit their preferences and working environment.
