# Tooltips and Localization in OLAP Gauge

Comprehensive guide for implementing tooltips and localization features including multi-language support and RTL (Right-to-Left) layout in OLAP Gauge.

## Table of Contents

- [Tooltip Support](#tooltip-support)
- [Pointer Tooltips](#pointer-tooltips)
- [Marker Tooltips](#marker-tooltips)
- [Tooltip Customization](#tooltip-customization)
- [Localization Overview](#localization-overview)
- [Localization Process](#localization-process)
- [Translation](#translation)
- [Resource Files](#resource-files)
- [Culture Specification](#culture-specification)
- [RTL Support](#rtl-support)
- [Combining Localization and RTL](#combining-localization-and-rtl)
- [Best Practices](#best-practices)

## Tooltip Support

Tooltips provide additional information when users hover over gauge elements. The OLAP Gauge supports tooltips for both pointers (values) and markers (goals).

### Tooltip Types

1. **Pointer Tooltip**: Displays value information when hovering over the value pointer
2. **Marker Tooltip**: Displays goal information when hovering over the goal marker

### Tooltip Benefits

- **Detail on Demand**: Show exact values without cluttering the visual
- **User Exploration**: Allow users to explore data interactively
- **Accessibility**: Provide additional context for data points
- **Professional Polish**: Enhanced user experience

## Pointer Tooltips

Pointer tooltips display value information when the mouse hovers over the gauge pointer.

### ShowPointersTooltip Property

**Property:** `ShowPointersTooltip`  
**Type:** `bool`  
**Default:** `false`

### Enabling Pointer Tooltips

**XAML:**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" ShowPointersTooltip="True"/>
```

**C#:**

```csharp
this.OlapGauge1.ShowPointersTooltip = true;
```

**VB.NET:**

```vb
Me.OlapGauge1.ShowPointersTooltip = True
```

### Pointer Tooltip Content

The pointer tooltip typically displays:
- **KPI Name**: The name of the KPI being visualized
- **Value Label**: "Value" or localized equivalent
- **Actual Value**: The numeric KPI value
- **Member Information**: Dimension member details (if applicable)

### Example Display

```
Revenue
Value: $1,250,000
```

![Pointer tooltip display](Tooltip_images/Pointer-tooltip.png)

### Use Cases

**Detailed analysis:**

```csharp
// Enable for reports where exact values matter
this.OlapGauge1.ShowPointersTooltip = true;
```

**Executive dashboards:**

```csharp
// Disable for high-level overviews
this.OlapGauge1.ShowPointersTooltip = false;
```

## Marker Tooltips

Marker tooltips display goal information when the mouse hovers over the goal marker.

### ShowMarkersTooltip Property

**Property:** `ShowMarkersTooltip`  
**Type:** `bool`  
**Default:** `false`

### Enabling Marker Tooltips

**XAML:**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" ShowMarkersTooltip="True"/>
```

**C#:**

```csharp
this.OlapGauge1.ShowMarkersTooltip = true;
```

**VB.NET:**

```vb
Me.OlapGauge1.ShowMarkersTooltip = True
```

### Marker Tooltip Content

The marker tooltip typically displays:
- **KPI Name**: The name of the KPI
- **Goal Label**: "Goal" or localized equivalent
- **Target Value**: The numeric goal value
- **Member Information**: Dimension member details (if applicable)

### Example Display

```
Revenue
Goal: $1,000,000
```

![Marker tooltip display](Tooltip_images/Marker-tooltip.png)

### Use Cases

**Performance tracking:**

```csharp
// Show both value and goal tooltips for comprehensive information
this.OlapGauge1.ShowPointersTooltip = true;
this.OlapGauge1.ShowMarkersTooltip = true;
```

## Tooltip Customization

### Enabling Both Tooltip Types

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" 
                      ShowPointersTooltip="True"
                      ShowMarkersTooltip="True"/>
```

```csharp
this.OlapGauge1.ShowPointersTooltip = true;
this.OlapGauge1.ShowMarkersTooltip = true;
```

### Selective Tooltip Display

**Show only value tooltips:**

```csharp
this.OlapGauge1.ShowPointersTooltip = true;
this.OlapGauge1.ShowMarkersTooltip = false;
```

**Show only goal tooltips:**

```csharp
this.OlapGauge1.ShowPointersTooltip = false;
this.OlapGauge1.ShowMarkersTooltip = true;
```

**Disable all tooltips (default):**

```csharp
this.OlapGauge1.ShowPointersTooltip = false;
this.OlapGauge1.ShowMarkersTooltip = false;
```

### Complete Configuration Example

```xml
<Window x:Class="OlapGaugeApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:OlapGauge x:Name="OlapGauge1" 
                              ShowPointersTooltip="True"
                              ShowMarkersTooltip="True"/>
    </Grid>
</Window>
```

### Sample Location

Tooltip demonstration samples available at:

```
{system drive}:\Users\<User Name>\AppData\Local\Syncfusion\
EssentialStudio\<Version Number>\WPF\OlapGauge.WPF\Samples\
Product ShowCase\KPI\
```

## Localization Overview

Localization enables the OLAP Gauge to display UI elements in different languages, making it suitable for global applications.

### What Gets Localized

- **Labels**: "Value", "Goal", "Status", "Trend"
- **Headers**: Gauge headers and titles
- **Tooltips**: Tooltip labels and text
- **Messages**: Error messages and notifications

### Localization Benefits

- **Global Reach**: Support users in their native languages
- **User Comfort**: Increase usability for non-English speakers
- **Compliance**: Meet regulatory requirements for multi-language support
- **Professionalism**: Demonstrate commitment to international markets

### Supported Resource Format

Syncfusion OLAP Gauge uses **.resx** (resource) files for localization.

## Localization Process

Localizing the OLAP Gauge involves three main steps:

1. **Translation**: Translate strings to target language
2. **Resource File Creation**: Create .resx files with translations
3. **Culture Specification**: Set application culture to load appropriate resources

## Translation

### Step 1: Identify Strings

Determine which strings need translation:

- UI element labels (Value, Goal, Status, Trend)
- Tooltip text
- Header text
- Error messages

### Step 2: Translate Strings

Create translations for each target language.

**Example English to Spanish:**

| English Key | English Value | Spanish Value |
|-------------|---------------|---------------|
| Value | Value | Valor |
| Goal | Goal | Objetivo |
| Status | Status | Estado |
| Trend | Trend | Tendencia |

**Example English to French:**

| English Key | English Value | French Value |
|-------------|---------------|--------------|
| Value | Value | Valeur |
| Goal | Goal | Objectif |
| Status | Status | Statut |
| Trend | Trend | Tendance |

**Example English to Arabic:**

| English Key | English Value | Arabic Value |
|-------------|---------------|--------------|
| Value | Value | قيمة |
| Goal | Goal | هدف |
| Status | Status | حالة |
| Trend | Trend | اتجاه |

### Important Note

**Keep localization key fields unchanged**. Do not translate the key names, only the values.

## Resource Files

### Step 1: Create Resources Folder

1. Right-click project in Solution Explorer
2. Select **Add > New Folder**
3. Name the folder **"Resources"**

### Step 2: Create Resource File

1. Right-click **Resources** folder
2. Select **Add > New Item**
3. Select **Resources File** (.resx)
4. Name according to convention (see below)

![Adding new item](Localization_images/Localization-step1.png)

![Adding resource file](Localization_images/Localization-step2.png)

### File Naming Convention

**Format:**

```
Syncfusion.OlapGauge.wpf.<CultureCode>.resx
```

**Examples:**

```
Syncfusion.OlapGauge.wpf.es.resx     // Spanish
Syncfusion.OlapGauge.wpf.fr.resx     // French
Syncfusion.OlapGauge.wpf.de.resx     // German
Syncfusion.OlapGauge.wpf.ar.resx     // Arabic
Syncfusion.OlapGauge.wpf.zh-CN.resx  // Chinese (Simplified)
Syncfusion.OlapGauge.wpf.ja.resx     // Japanese
```

### Common Culture Codes

| Language | Culture Code |
|----------|--------------|
| Spanish | es |
| French | fr |
| German | de |
| Italian | it |
| Portuguese | pt |
| Arabic | ar |
| Chinese (Simplified) | zh-CN |
| Chinese (Traditional) | zh-TW |
| Japanese | ja |
| Korean | ko |
| Russian | ru |
| Dutch | nl |

### Step 3: Add Translations to Resource File

Open the .resx file and add key-value pairs:

**Syncfusion.OlapGauge.wpf.es.resx:**

```xml
<root>
  <data name="Value" xml:space="preserve">
    <value>Valor</value>
  </data>
  <data name="Goal" xml:space="preserve">
    <value>Objetivo</value>
  </data>
  <data name="Status" xml:space="preserve">
    <value>Estado</value>
  </data>
  <data name="Trend" xml:space="preserve">
    <value>Tendencia</value>
  </data>
</root>
```

**Syncfusion.OlapGauge.wpf.fr.resx:**

```xml
<root>
  <data name="Value" xml:space="preserve">
    <value>Valeur</value>
  </data>
  <data name="Goal" xml:space="preserve">
    <value>Objectif</value>
  </data>
  <data name="Status" xml:space="preserve">
    <value>Statut</value>
  </data>
  <data name="Trend" xml:space="preserve">
    <value>Tendance</value>
  </data>
</root>
```

## Culture Specification

### Setting CurrentUICulture

Specify the culture in your application to load the appropriate resource file.

### Option 1: In MainWindow Constructor

**C#:**

```csharp
using System.Globalization;
using System.Threading;

public MainWindow()
{
    // Set culture BEFORE InitializeComponent
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("es");  // Spanish
    
    InitializeComponent();
}
```

**VB.NET:**

```vb
Imports System.Globalization
Imports System.Threading

Public Sub New()
    ' Set culture BEFORE InitializeComponent
    Thread.CurrentThread.CurrentUICulture = New CultureInfo("es")  ' Spanish
    
    InitializeComponent()
End Sub
```

### Option 2: In App.xaml.cs Startup

**C#:**

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Set culture at application startup
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("fr");  // French
        
        base.OnStartup(e);
    }
}
```

**VB.NET:**

```vb
Partial Public Class App
    Inherits Application
    
    Protected Overrides Sub OnStartup(e As StartupEventArgs)
        ' Set culture at application startup
        Thread.CurrentThread.CurrentUICulture = New CultureInfo("fr")  ' French
        
        MyBase.OnStartup(e)
    End Sub
End Class
```

### Critical Timing

**Important:** Set `CurrentUICulture` **BEFORE** calling `InitializeComponent()`. Setting it after will not localize the control.

```csharp
// ✓ Correct
public MainWindow()
{
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("es");
    InitializeComponent();  // Resources loaded with Spanish culture
}

// ✗ Incorrect
public MainWindow()
{
    InitializeComponent();  // Resources loaded with default culture
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("es");  // Too late!
}
```

### Dynamic Culture Selection

Allow users to choose language at runtime:

```csharp
private void SetCulture(string cultureCode)
{
    Thread.CurrentThread.CurrentUICulture = new CultureInfo(cultureCode);
    
    // Recreate window to apply new culture
    var newWindow = new MainWindow();
    newWindow.Show();
    this.Close();
}

// Usage with ComboBox
private void LanguageComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string selectedCulture = (e.AddedItems[0] as ComboBoxItem).Tag.ToString();
    SetCulture(selectedCulture);
}
```

## RTL Support

RTL (Right-to-Left) support enables proper display for languages like Arabic and Hebrew that read from right to left.

### FlowDirection Property

**Property:** `FlowDirection`  
**Type:** `FlowDirection` (enum)  
**Values:** `LeftToRight` (default), `RightToLeft`

### Enabling RTL

**XAML:**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" FlowDirection="RightToLeft"/>
```

**C#:**

```csharp
OlapGauge1.FlowDirection = FlowDirection.RightToLeft;
```

**VB.NET:**

```vb
OlapGauge1.FlowDirection = FlowDirection.RightToLeft
```

### RTL Effect

When FlowDirection is set to RightToLeft:
- **Content flows right-to-left**: Elements arrange from right to left
- **Text alignment**: Text aligns to the right
- **Mirror layout**: UI elements mirror horizontally
- **Gauge positioning**: Gauges and indicators adjust accordingly

![RTL display](Localization_images/Localization-RTL.png)

### Window-Level RTL

Apply RTL to entire window:

```xml
<Window FlowDirection="RightToLeft">
    <Grid>
        <syncfusion:OlapGauge x:Name="OlapGauge1"/>
    </Grid>
</Window>
```

All controls within the window inherit RTL layout.

### Conditional RTL

Apply RTL based on culture:

```csharp
private void ApplyRTLIfNeeded(string cultureCode)
{
    // RTL cultures
    var rtlCultures = new[] { "ar", "he", "fa", "ur" };  // Arabic, Hebrew, Farsi, Urdu
    
    if (rtlCultures.Contains(cultureCode))
    {
        this.FlowDirection = FlowDirection.RightToLeft;
    }
    else
    {
        this.FlowDirection = FlowDirection.LeftToRight;
    }
}
```

## Combining Localization and RTL

For complete Arabic localization:

### Step 1: Create Arabic Resource File

**Syncfusion.OlapGauge.wpf.ar.resx:**

```xml
<root>
  <data name="Value" xml:space="preserve">
    <value>قيمة</value>
  </data>
  <data name="Goal" xml:space="preserve">
    <value>هدف</value>
  </data>
  <data name="Status" xml:space="preserve">
    <value>حالة</value>
  </data>
  <data name="Trend" xml:space="preserve">
    <value>اتجاه</value>
  </data>
</root>
```

### Step 2: Set Culture and RTL

**C#:**

```csharp
public MainWindow()
{
    // Set Arabic culture
    System.Threading.Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("ar");
    
    InitializeComponent();
    
    // Enable RTL
    this.FlowDirection = FlowDirection.RightToLeft;
}
```

**VB.NET:**

```vb
Public Sub New()
    ' Set Arabic culture
    System.Threading.Thread.CurrentThread.CurrentUICulture = New System.Globalization.CultureInfo("ar")
    
    InitializeComponent()
    
    ' Enable RTL
    Me.FlowDirection = FlowDirection.RightToLeft
End Sub
```

### Complete Example

```csharp
using System.Globalization;
using System.Threading;
using System.Windows;

namespace OlapGaugeApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            // Arabic localization
            Thread.CurrentThread.CurrentUICulture = new CultureInfo("ar");
            
            InitializeComponent();
            
            // RTL layout for Arabic
            this.FlowDirection = FlowDirection.RightToLeft;
            
            // Enable tooltips (will show in Arabic)
            this.OlapGauge1.ShowPointersTooltip = true;
            this.OlapGauge1.ShowMarkersTooltip = true;
        }
    }
}
```

### Sample Location

Localization demo samples available at:

```
{system drive}:\Users\<User Name>\AppData\Local\Syncfusion\
EssentialStudio\<Version Number>\WPF\OlapGauge.WPF\Samples\
Localization\Localization Demo\
```

## Best Practices

### 1. Resource File Organization

Organize resource files in a dedicated folder:

```
Project/
├── Resources/
│   ├── Syncfusion.OlapGauge.wpf.es.resx
│   ├── Syncfusion.OlapGauge.wpf.fr.resx
│   ├── Syncfusion.OlapGauge.wpf.de.resx
│   └── Syncfusion.OlapGauge.wpf.ar.resx
```

### 2. Consistent Naming

Follow the exact naming convention:

```
Syncfusion.OlapGauge.wpf.<culture>.resx
```

Incorrect naming prevents localization from working.

### 3. Complete Translations

Translate all strings, not just some:

```csharp
// ✓ Complete translation
Value → Valor
Goal → Objetivo
Status → Estado
Trend → Tendencia

// ✗ Incomplete translation (mixing languages)
Value → Valor
Goal → Goal  // Still in English!
```

### 4. Professional Translation

Use professional translators for:
- Technical accuracy
- Cultural appropriateness
- Idiomatic expressions

Avoid machine translation for production applications.

### 5. Tooltip Strategy

Enable tooltips for localized applications:

```csharp
// Tooltips provide context in user's language
this.OlapGauge1.ShowPointersTooltip = true;
this.OlapGauge1.ShowMarkersTooltip = true;
```

### 6. RTL Testing

Test RTL layouts thoroughly:
- Verify text alignment
- Check control positioning
- Ensure gauge readability
- Test on actual RTL systems

### 7. Culture Fallback

Implement fallback to default culture if translation missing:

```csharp
try
{
    Thread.CurrentThread.CurrentUICulture = new CultureInfo(userSelectedCulture);
}
catch
{
    // Fallback to English if culture not supported
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("en");
}
```

### 8. User Preference Persistence

Save user's language choice:

```csharp
// Save preference
Properties.Settings.Default.PreferredLanguage = "es";
Properties.Settings.Default.Save();

// Load on startup
string preferredLanguage = Properties.Settings.Default.PreferredLanguage;
if (!string.IsNullOrEmpty(preferredLanguage))
{
    Thread.CurrentThread.CurrentUICulture = new CultureInfo(preferredLanguage);
}
```

### 9. Testing Checklist

Test localization with:
- [ ] All supported languages load correctly
- [ ] Tooltips display in target language
- [ ] Headers and labels are translated
- [ ] RTL layouts display properly for RTL cultures
- [ ] No English strings appear in localized version
- [ ] Gauge remains readable with longer translations
- [ ] Culture switching works without restart (if supported)

### 10. Documentation

Document supported languages and cultures:

```csharp
/// <summary>
/// Supported cultures for OLAP Gauge localization
/// </summary>
public static class SupportedCultures
{
    public const string English = "en";
    public const string Spanish = "es";
    public const string French = "fr";
    public const string German = "de";
    public const string Arabic = "ar";
    public const string Chinese = "zh-CN";
}
```
